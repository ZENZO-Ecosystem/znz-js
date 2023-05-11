# ZNZ.js - An ZNZ implementation in pure Node.js

ZNZ.js can be used as an internal factory system to produce Transactions, On-Chain Scripts, Contracts and Signature creation/verification, all without any full node, RPC or cryptography necessary!

For example, the [ZENZO Forge](https://github.com/ZENZO-Ecosystem/zenzo-forge-v2) (an Electron-based Lightwallet) uses ZNZ.js as it's internal wallet system.

# API Documentation

The following docs assume that you've minimally imported the ZNZ.js library into your Node.js script, e.g:
```js
const ZNZ = require('znz-js');
// The ZNZ module exposes two sub-modules, the Wallet and Signer modules
// ZNZ.wallet
// ZNZ.signer
```
---

## Wallet (Key Store)

### Generate Wallet (Promise)
Creates a cryptographically-random ZNZ wallet key pair (Private Key and Public Key)
```js
const ZNZ = require('znz-js');

// Async
async function newWallet() {
    return await ZNZ.wallet.generateWallet(); // { 'pubkey': '...', 'privkey': '...' }
}

// Callback
ZNZ.generateWallet().then(cWallet => {
    // { 'pubkey': '...', 'privkey': '...' }
});
```

### Derive Public Key from Private Key
Derives the public key from an existing WIF-encoded private key.

- Decodes Base58 private keys by default, but can accept raw key bytes with an optional flag.
- Returns a network-encoded, Base58 address by default, but can return a raw Secp256k1 public key with an optional flag.
```js
const ZNZ = require('znz-js');

// Derive a native base58 ZNZ address (String)
//                                           Private Key String
const base58Address = ZNZ.wallet.pubFromPriv('base58_private_key');

// Derive a raw Secp256k1 public key (Uint8Array)
//                                    Private Key String,   fRaw,  fPubBytesOnly
const pubKey = ZNZ.wallet.pubFromPriv('base58_private_key', false, true);

// Derive a native base58 ZNZ address (String) from raw private key bytes (Uint8Array)
//                                           Private Key String, fRaw
const base58Address = ZNZ.wallet.pubFromPriv(uint8PrivKeyBytes,  true);
```

---

## Wallet (Transactions)

`// TODO!`

---

## Signer

### Create Signature (Promise)
Creates a cryptographic signature of a given message, for a given private key.
```js
const ZNZ = require('znz-js');

// Async
async function signMessage(msg, privkey)) {
    return await ZNZ.signer.sign(msg, privkey); // Buffer([signature bytes]);
}

// Callback
ZNZ.signer.sign('hello world', 'WIF_private_key').then(cSig => {
    // Buffer([signature bytes]);
});
```

### Verify Signature (Promise)
Verify the integrity and authorship of a message by it's public key and signature.
```js
const ZNZ = require('znz-js');

// Async
async function verifyMessage(msg, pubkey, cSig)) {
    return await ZNZ.signer.verify(msg, pubkey, cSig); // Boolean(true / false)
}

// Callback
ZNZ.signer.verify(msg, pubkey, cSig).then(fValid => {
    // Boolean(true / false)
});
```
