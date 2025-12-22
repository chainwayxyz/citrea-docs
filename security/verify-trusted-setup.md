# Trusted Setup Verification

[Clementine](https://docs.citrea.xyz/essentials/clementine-trust-minimized-bitcoin-bridge), Citrea’s BitVM‑based bridge, proves execution with RISCZero (STARK) and wraps those receipts in a Groth16 SNARK so that it can be verified on Bitcoin efficiently.&#x20;

The **Citrea Risc0‑to‑BitVM trusted setup ceremony** establishes the public parameters for a Groth16 verifier that checks RISCZero STARK receipts via BitVM, enabling a single compact proof to be verified on Bitcoin without consensus changes and moving BitVM‑based bridges into their implementation phase.&#x20;

***

Verification below matters to confirm the setup wasn’t manipulated and that Clementine points to the exact parameters produced. By rebuilding the circuit, fetching the precise Powers of Tau and final zkey, and checking the transcript, you can reproduce the commitments and compare them to the published parameter and verifying‑key hashes. &#x20;

{% hint style="success" %}
This setup uses a similar methodology to RISCZero's Groth16 trusted setup. You can read more about the security details [here](https://dev.risczero.com/api/trusted-setup-ceremony).
{% endhint %}

A copy of the verification logs are available [on Github](https://gist.github.com/ekrembal/527f44c91736a511f6bfd61e01bc4f22).&#x20;

You can also find each individual attestation by visiting gist pages of the contributors (unless it's removed/hidden by the contributor). An example can be found [here](https://gist.github.com/otaliptus/f35c53a3c9b68332321c61dcc0716a85).

***

The commands below show the exact steps for verification and should conclude with `snarkJS: ZKey Ok!`.

{% hint style="warning" %}
The verification requires more than 32 GB RAM (i.e. 64 GB RAM suffices).&#x20;

Guide below is tested for Ubuntu 24.04.
{% endhint %}

### 1) Setup Circom & SnarkJS

```bash
# deps
sudo apt-get update
sudo apt-get install -y build-essential git npm

# Rust (for building circom)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
. "$HOME/.cargo/env"

# circom (v2.2.x)
git clone https://github.com/iden3/circom.git
cd circom
cargo install --path circom          # installs the 'circom' binary into ~/.cargo/bin
circom --version                     # sanity check (expect v2.2.x)
cd ..

# snarkjs
npm install -g snarkjs
snarkjs --version
```

### 2) Clone Citrea RiscZero to BitVM repository

```bash
git clone https://github.com/chainwayxyz/risc0-to-bitvm2
cd risc0-to-bitvm2
```

### 3) Install git-lfs and pull larger files in the repository

```bash
sudo apt-get install -y git-lfs
git lfs pull
```

### 4) Generate r1cs file

```bash
# circomlib (used by the circuit)
git clone https://github.com/iden3/circomlib.git

# compile the circuit -> verify_for_guest.r1cs
sed -i '$d' ./groth16_proof/circuits/stark_verify.circom
circom groth16_proof/circuits/verify_for_guest.circom --r1cs --O2

# (optional) shasum check of the r1cs you built
sha256sum verify_for_guest.r1cs
# c90be1fb4b83f8f10f4be51e47ebdde2fd97dacd60d658d63d6828f3f642a116
```

### 5) Install Powers of Tau and the final zkey

```bash
# download the exact artifacts used for the ceremony (10+ GB download)
wget https://pse-trusted-setup-ppot.s3.eu-central-1.amazonaws.com/pot28_0080/ppot_0080_23.ptau
wget https://static.mainnet.citrea.xyz/verify_for_guest_final.zkey

# (optional) check them, too
sha256sum ppot_0080_23.ptau verify_for_guest_final.zkey
# 05085994c8aa34e4925585202e178e09fb8acc04e9565311751e8a04dec81cb1  ppot_0080_23.ptau
# 86fc7478fce8b91b502538cc64e8295a104f68ecf025042a21b10b54d7d504c0  verify_for_guest_final.zkey
```

### 6) Run snarkJS for verification

```bash
# increase Node memory for large circuits
export NODE_OPTIONS="--max-old-space-size=32768"

# verify that the zkey transcript matches your r1cs and the ptau
# can take between 30 mins to hours depending on your system
snarkjs zkey verify verify_for_guest.r1cs ppot_0080_23.ptau verify_for_guest_final.zkey
```

You can also compare the result with [our published logs](https://gist.github.com/ekrembal/527f44c91736a511f6bfd61e01bc4f22).
