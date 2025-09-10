---
title: 'Challenge Initiation: A Cha...'
---

* **Challenge Initiation:** A Challenger detects the fraudulent `KickOff` transaction by the operator and posts a `Challenge` transaction on Bitcoin, which signals the beginning of a dispute.
* **Watchtower Challenge Transaction:** After waiting enough time to make sure that an Operator is not trying to maliciously make a private Bitcoin fork with exceeding proof-of-work, a Watchtower posts the canonical Bitcoin header-chain proof with work for the Challenger to use.&#x20;
* **Challenge-Response Game:** The Operator is now forced to defend their claim by providing a zkSNARK Light Client Proof of Bitcoin which shows their committed chain has more proof-of-work (than the Watchtower's claim) and correctly includes the user's payout transaction.&#x20;
  * If the operator is not malicious, by waiting enough time and posting a more worked claim than Watchtower's claim, it can get win the challenge against the Challenger and get reimbursed by waiting some time and posting a valid Light Client Proof with more work compared to Challenger's claim.
  * If the operator is malicious, it cannot post a valid Light Client Proof with more work included due to hash rate. Whether it posts an invalid proof or posts nothing, it will lose the challenge.
    * This phase uses BitVM - the complex computation is executed off-chain, but any step in the exeuction can be disputed and verified on-chain.
