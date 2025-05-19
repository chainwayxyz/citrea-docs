## secp256r1 and Schnorr Precompiles

The secp256r1 and Schnorr precompiles are enabled with the [Tangerine](https://www.blog.citrea.xyz/tangerine-upgrade-bitvm-activation-on-clementine/) upgrade of Citrea. In this document we will go over the details of these precompiles and how to use them.

#### secp256r1 Curve Support ([RIP 7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md))

RIP 7212 is an improvement proposal that introduces a precompile support for the secp256r1 curve. In Citrea, like other EVM rollups, this precompile is available at address `0x0000000000000000000000000000000000000100`.

This precompile enables a whole new set of applications and use cases, such as:
- **Hardware \& Biometric Authentication**: Native support of secp256r1 allows smart contracts to directly validate the signatures from secure enclaves and passkey devices, such as Apple's [Secure Enclave](https://support.apple.com/guide/security/secure-enclave-sec59b0b31ff/web), Android's [Keystore](https://developer.android.com/privacy-and-security/keystore), Yubikeys, and WebAuthn authenticators.
- **Account Abstraction \& Smart Wallets**: Combining features above with account abstraction \& smart wallets, self-custodial platforms can be built much more easily and efficiently, such as [Tanari](https://www.tanari.io/). It also improves the security and the UX perspective of applications massively. 
- **Gas-efficient P256 signature verification**: With this precompile, verification on P256 comes down to `3450` gas, which is significantly cheaper than any other existing smart contract verification method.

##### Technical Details

The precompile accepts a 160-byte input, which is a concatenation of:
1. `Message Hash (32 bytes)`: The 32-byte hash of the message that was signed.
2. `Signature r value (32 bytes)`: The r component of the ECDSA signature.
3. `Signature s value (32 bytes)`: The s component of the ECDSA signature.
4. `Public Key X-coordinate (32 bytes)`: The X-coordinate of the uncompressed secp256r1 public key.
5. `Public Key Y-coordinate (32 bytes)`: The Y-coordinate of the uncompressed secp256r1 public key.

Upon successful verification, the precompile returns a 32-byte value `0x00...01`. On failure (e.g., invalid signature, malformed input), it returns empty bytes: `0x`.

#### Example Smart Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;

/// @title P256 Precompile Example
/// @notice Calls the P256 precompile to verify secp256r1 ECDSA signatures
contract P256VerifyCaller {
    address constant P256_VERIFY_PRECOMPILE = 0x0000000000000000000000000000000000000100;

    /**
     * @notice Verifies a secp256r1 (RIP-7212) ECDSA signature.
     * @dev All inputs must be 32-byte big-endian values.
     * @param messageHash 32-byte message hash
     * @param r 32-byte signature r value
     * @param s 32-byte signature s value
     * @param pubKeyX 32-byte public key X coordinate
     * @param pubKeyY 32-byte public key Y coordinate 
     * @return isValid True if signature is valid, false otherwise.
     */
    function callP256Verify(
        bytes32 messageHash,
        bytes32 r,
        bytes32 s,
        bytes32 pubKeyX,
        bytes32 pubKeyY
    ) external view returns (bool isValid) {
        // Concatenate inputs in correct order: hash | r | s | pubKeyX | pubKeyY
        bytes memory input = abi.encodePacked(
            messageHash,
            r,
            s,
            pubKeyX,
            pubKeyY
        );
        (bool ok, bytes memory output) = P256_VERIFY_PRECOMPILE.staticcall(input);
        // 32-byte return, last byte == 0x01 means success
        isValid = ok && output.length == 32 && output[31] == 0x01;
    }
}
```

----- 

#### Schnorr Signature Verification 

Schnorr signature is a Bitcoin-native, linear, and aggregation-friendly signature scheme. It is a key component of Bitcoin's Taproot upgrade ([BIP 340](https://github.com/bitcoin/bips/blob/master/bip-0340.mediawiki)) and also offers advantages for multi-signature schemes like [MuSig2](https://bitcoinops.org/en/topics/musig/). In Citrea, this precompile is available at the address `0x0000000000000000000000000000000000000200`.

Schnorr precompile enables an interesting set of developments, such as:
- **Scriptless cross-chain atomic swaps**: With Schnorr adaptor signatures it is possible to build BTC <> cBTC atomic swaps without HTLCs or any other third-party custody.
- **Bitcoin-aware oracles \& bridges**: A smart contract on Citrea can now prove that a Taproot key signed some data without any external verifiers.

##### Technical Details

The precompile accepts a 128-byte input, concatenated as follows:

1. `Public Key X-coordinate (32 bytes)`: The 32-byte x-coordinate of the Schnorr public key. The y-coordinate is implicitly assumed to be even as per BIP340.
2. `Message Hash (32 bytes)`: The 32-byte hash of the message that was signed.
3. `Signature (64 bytes)`: The 64-byte Schnorr signature (typically r and s components, each 32 bytes).

If the signature is valid for the given message hash and public key, the precompile returns a 32-byte value `0x00...01`. If verification fails, it returns empty bytes: `0x`.

Base gas cost of Schnorr precompile is set to `4600`.

#### Example Smart Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;

/// @title Schnorr Precompile Example
/// @notice Calls the Schnorr precompile to verify BIP340 secp256k1 signatures
contract SchnorrVerifyCaller {
    address constant SCHNORR_VERIFY_PRECOMPILE = 0x0000000000000000000000000000000000000200;

    /**
     * @notice Verifies a BIP340 Schnorr signature.
     * @dev All inputs must be big-endian byte sequences.
     * @param pubKeyX 32-byte public key X coordinate (big-endian, Y is implicitly even per BIP340)
     * @param messageHash 32-byte hash of the signed message
     * @param signature 64-byte Schnorr signature (r || s), both 32-byte values concatenated
     * @return isValid True if signature is valid, false otherwise.
     */
    function schnorrVerify(
        bytes32 pubKeyX,
        bytes32 messageHash,
        bytes calldata signature // must be 64 bytes
    ) external view returns (bool isValid) {
        require(signature.length == 64, "Invalid signature length");
        // Concatenate inputs in correct order: pubKeyX | messageHash | signature
        bytes memory input = abi.encodePacked(
            pubKeyX,
            messageHash,
            signature
        );
        (bool ok, bytes memory output) = SCHNORR_VERIFY_PRECOMPILE.staticcall(input);
        // 32-byte return, last byte == 0x01 means success
        isValid = ok && output.length == 32 && output[31] == 0x01;
    }
}
```
