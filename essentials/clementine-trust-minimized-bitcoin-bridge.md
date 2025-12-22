# Clementine: trust-minimized Bitcoin Bridge

Clementine is Citrea’s native two‑way peg that moves BTC ↔ cBTC in a trust-minimized way.&#x20;

{% hint style="success" %}

### Why Clementine?

Existing Bitcoin bridges usually depend on trusted multisigs or on separate chains/consensus, pushing users to accept an **honest‑majority** assumption and moving economic activity away from Bitcoin.&#x20;

Clementine is a trust-minimized solution based on [BitVM2](https://bitvm.org/bitvm_bridge.pdf) - a new breakthrough that enables optimistic verification of ZK Proofs directly on Bitcoin without any changes / soft-forks.&#x20;

Using BitVM2, one can:

* verify the correctness of a ZK Proof **on Bitcoin**
* and, in case a provided proof is not correct, can determine and take action against it **on Bitcoin.**

With the help of BitVM2 Clementine keeps the safety of funds *on Bitcoin,* reduces the peg-out trust to **“1‑of‑N honesty”,** and achieves trust-minimization:

* One honest **Signer** ensures funds can only follow pre‑approved spend paths.
* One honest **Watchtower** can block a claim anchored to the wrong Bitcoin chain.
* One rational **Challenger** can prove an invalid computation and seize a malicious Operator’s collateral.&#x20;

As long as 1-of-N honest assumption holds, funds are **protected** against **both** liveness failures and theft.
{% endhint %}

Clementine is formally described and explained in its [whitepaper](https://citrea.xyz/clementine_whitepaper.pdf). Clementine is also open-source, you can read the codebase [here](https://github.com/chainwayxyz/clementine).

In this page, we provide a detailed high-level overview of the protocol. For a formal definition with more explanations, please refer to the whitepaper.

Let's continue with the roles in the Clementine bridge.

***

## Roles and Responsibilities

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2F2ZZ8CwoiXZXg3E2XhGnm%2Fimage.png?alt=media&#x26;token=7840727f-e354-4168-ba37-d750438f6272" alt=""><figcaption></figcaption></figure>

There are **five** major roles on a functional Clementine bridge. In this section we provide the major definition of these roles, and in the following sections you may understand their exact functions under different conditions.

* **Users** - start peg‑ins (deposits BTC to mint **cBTC**, Citrea's native asset) and peg‑outs (burn **cBTC** to a system contract to withdraw BTC to Bitcoin).
* **Signers** - a committee of n signers that **pre‑signs the allowed spend rules**. These signers emulate [covenant](http://covenants.info)‑like restrictions: funds can only move along Clementine’s approved paths, or they are returned to the user after a timeout.\ <sub>*A third option is the optimistic payouts, which we discuss in the following sections*</sub><sub>.</sub>
* **Operators** - entities who **facilitate fast withdrawals to Bitcoin by fronting BTC** payments to the user and **later claim reimbursement from the bridge vault** if nobody can prove malicious or fraudulent intent. In the case of malicious Operator intent, Clementine initiates a challenge response game described in the sections below.\ <sub>*to make sure economics are safe, Operators put a slashable bond to the protocol (\~ 2 BTC). This is discussed under section 5.*</sub>
* **Watchtowers** - entities who monitor both Citrea and Bitcoin. In the case of an initiated challenge transaction where malicious intent by an operator is detected, watchtowers are responsible for publishing the compact **Bitcoin header-chain proof** with total work.
* **Challengers** - entities who **force** an operator to prove their intent by initiating a Challenge transaction and using the header-chain proof published by the watchtower.&#x20;

{% hint style="info" %}
These entities are not mutually exclusive. For example, in mainnet, a signer in Clementine will also function as a watchtower and a challenger. Thus, roughly, the following relation will hold:

<mark style="background-color:$success;">**Operators ⊆ Signers ⊆ Watchtowers ⊆ Challenger ⊆ Users (permissionless)**</mark>
{% endhint %}

***

## How funds move around

In this section we go over the flow of the funds based on the protocol's rules, for both peg-in(s) and peg-out(s).

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2F9KQF6JzcRMMOiy9h6DNr%2Fimage.png?alt=media&#x26;token=0eca420d-47d5-434e-9713-d16f10a4457e" alt=""><figcaption></figcaption></figure>

### A) Peg-in (BTC → cBTC)

***

Peg-in flow to Citrea is fairly simple:

1. **User deposit:** user sends **BTC** to a [Taproot](https://bitcoinops.org/en/topics/taproot/) address that encodes two paths:
   * *Bridge path:* spendable only along Clementine’s rules (pre‑signed by the Signer set).
   * *Refund path:* the user can take funds back after a timelock (`OP_CSV`) if the bridge doesn’t act as described below in 200 Bitcoin blocks. This timeout prevents funds from getting stuck if Step 2 does not happen.
2. **Move BTC to vault:** Operators move the deposit into a "vault" UTXO that is only spendable by the bridge’s allowed exits.&#x20;
   * This `MovetoVault` transaction *emulates* covenants with pre‑signed transactions and MuSig‑style aggregation. Keys are erased by the signers afterwards, so funds **must** follow the pre‑signed paths.
3. **Mint on Citrea:** Once the `MovetoVault` transaction is successfully finalized (6+ blocks), the `Bridge` system smart contract on Citrea checks its validity using the `BitcoinLightClient` smart contract. The contract then mints equivalent amount of **cBTC** to the user’s Citrea address (the address is bound in the deposit).

### B) Peg‑out (cBTC → BTC)&#x20;

***

Bridging from Citrea to Bitcoin (converting cBTC on Citrea to BTC on Bitcoin) is handled in a couple steps.

First, a user should ***burn*** their cBTC on Citrea using the [Bridge](https://docs.citrea.xyz/developer-documentation/system-contracts/bridge) system contract using the `safeWithdraw` function. Then, they need to submit their request to exit to the Operators of the bridge using a *Payout* transaction template that includes their BTC address and the amount. As a special template, this is a presigned transaction that requires an additional UTXO input from an operator.

After user's burn, there are two scenarios

* **Optimistic Case**: This is the case where there's no one malicious.&#x20;
* **Dispute & Challenge Case**: This is the case where there's an operator with malicious intent.

Let's cover these cases in separate sections, in detail.

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FnBSIKG8kmnKDaioD47eT%2Fimage.png?alt=media&#x26;token=23a1e587-6399-486c-9d26-a1c6ab967e23" alt=""><figcaption></figcaption></figure>

#### Peg-out Scenario 1: Happy Case

If there is no malicious intent from the operators, the payout can be completed in two ways:

1. **Optimistic Payout with n Signers:** If all signers are online, they can give signatures and directly transfer the BTC to the user address without the need of Operator. We call this an **optimistic payout** - just like a regular transfer from an n-of-n multisig.&#x20;
2. **Operator Payout:** If a signer is not online for *12 hours*, the optimistic payout above cannot be completed. Thus, the user still needs to be paid. In this case, an operator from the operator set steps in and fetches the Payout template from the user. The operator then attaches their own inputs to the template and broadcasts the *Payout* transaction. This template uses [**`SIGHASH_SINGLE|ANYONECANPAY`**](https://wiki.bitcoinsv.io/index.php/SIGHASH_flags) , so the user’s output is locked while any Operator can fund it.&#x20;

<mark style="background-color:$success;">From the user’s perspective, once either of these transactions is finalized, the withdrawal is complete.</mark>

If the payout is completed through an Operator Payout (step 2), then the operator needs to be **reimbursed** from the bridge vault on Bitcoin. It is completed as follows:

1. The operator asks for reimbursement by posting a claim transaction (called `KickOff`) on Bitcoin.&#x20;
2. This starts a challenge window. If nobody disputes in a predetermined time period (i.e. 1.5 days where everything is fine), the Operator is able to post another transaction called `NoChallenge`, that gets unlocked only upon timelock expiry. Using this `NoChallenge` transaction, Operator can reimburse itself.

For the cases above so far, we assumed that operators are honest. However... what if an Operator claims a payment that did not happen? What if they are malicious?

This leads us to the **`Dispute`** scenario. Let's go over it.

#### Peg-out Scenario 2: Dispute & Challenge Case

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FVYWyAP8AZJxNpgqWez7x%2Ffinal_diagram.png?alt=media&#x26;token=ec6330a0-0e1a-4740-b872-449998d0bb77" alt="Green Boxes indicate Operator reimbursement, whereas grey boxes indicate cases that Operator is slashed &#x26; kicked out from the vault."><figcaption><p>Green Boxes indicate Operator reimbursement, whereas grey boxes indicate cases that Operator is slashed &#x26; kicked out from the vault.</p></figcaption></figure>

If an Operator makes a fraudulent reimbursement claim (e.g., for a non-existent withdrawal), the protocol enters a dispute resolution phase enforced by Watchtowers and Challengers with the following steps:

1. **Challenge Initiation:** A Challenger detects the fraudulent `KickOff` transaction by the operator and posts a `Challenge` transaction on Bitcoin, which signals the beginning of a dispute.
2. **Watchtower Challenge Transaction:** After waiting enough time to make sure that an Operator is not trying to maliciously make a private Bitcoin fork with exceeding proof-of-work, a Watchtower posts the canonical Bitcoin header-chain proof with Work.&#x20;
3. **Challenge-Response Game:** The Operator is now forced to defend their reimbursement claim by providing a zkSNARK Light Client Proof of Bitcoin which shows their committed chain has more proof-of-work (than the Watchtower's claim) and correctly includes the user's payout transaction.&#x20;
   1. **If the operator is not malicious**, by waiting some time and posting the correct Light Client Proof, it can win the challenge against the Challenger and get reimbursed. The only requirement for the Operator's Light Client Proof is to carry more Work compared to Watchtower's proof.
   2. **If the operator is malicious**, it cannot post a valid Light Client Proof with more work included due to hash rate. Whether it posts an invalid proof (or cannot prove that a valid payout exists), it will lose the challenge.
      * This phase uses BitVM - the complex computation is executed off-chain, but any step in the execution can be disputed and verified on-chain.

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FwzdO5HYQkIyiKD2PLZGW%2Fimage.png?alt=media&#x26;token=ea03944f-537a-490c-b013-2bd00dbfda30" alt=""><figcaption></figcaption></figure>

To summarize, after all the steps above, if the on-chain step confirms the Operator is dishonest, or if the Operator fails to provide a valid proof, their entire collateral bond is **slashed**.&#x20;

Once an operator is slashed, they are also effectively **kicked out** of the Signer & Operator set to prevent any other malicious activity.

{% hint style="success" %}
The disputes and challenges can occur for an honest operator as well. However, operator will win the challenge if it is honest and did not take any malicious actions, and the challenger will pay for the whole transaction fees.
{% endhint %}

***

## Economics, Security, and Liveness

### a) Operator's Bond

In Clementine, each operator locks **their own BTC** as a **bond**. That single bond backs a round of many user withdrawals and continues to persist as long as the operator is honest for consecutive rounds. If **any** paid withdrawal claim by an operator in the round is proven fraudulent (or the operator misses required steps during dispute), the bond is **slashed** and **none** of the reimbursements in that round are paid. This helps keep the bridge safe, as the operator is discouraged from making any malicious attempts, while the stake can be relatively low because one bond works for all claims.

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FYZYHnQj67HK185VT7GuF%2Fimage.png?alt=media&#x26;token=1a4e9754-3a36-4a8f-acc8-038da42208ea" alt=""><figcaption></figcaption></figure>

### b) Payoff Rounds & Capital Efficiency

Unlike BitVM2, Clementine requires only one successful challenge to slash an Operator's entire bond for a malicious case, preventing them from claiming *any* of their reimbursements for that payoff round. Also, upon the finalization of a round, the same collateral is reused for the next round as well.  This drastically reduces the on-chain load and provides a strong economic deterrent against misbehavior.

### **c) Security & Liveness Guarantees**

Let's go over a summary of Clementine's trust assumptions, as stated in the whitepaper too:

* Clementine is trust-minimized as **1-of-N** - the bridge’s safety does not depend on a majority of honest actors, as explained in the sections above. It only requires **one honest participant** for each key role, out of N.
* Clementine is **secure** as long as an attacker does not control more than 45% of Bitcoin's hash rate for 2 weeks.
* Assuming the two conditions above hold, **incorrect peg-outs cannot succeed**, as they will be challenged and stopped. In that case, the user can be paid later by another operator.
* Also in a case where a watchtower and challenger duo is malicious, the operator can provide their valid proofs and win the challenge, which prevents any potential DOS attacks.
* During a peg-out, the same *Payout* template cannot be used twice. Therefore, **a withdrawal request cannot be paid twice**.
