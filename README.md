[![Build](https://github.com/romanz/trezor-agent/actions/workflows/ci.yml/badge.svg)](https://github.com/romanz/trezor-agent/actions)
[![Chat](https://badges.gitter.im/romanz/trezor-agent.svg)](https://gitter.im/romanz/trezor-agent)
![Build status](https://github.com/trezor/trezord-go/actions/workflows/check-go-validation.yml/badge.svg) ![Installer build status](https://github.com/trezor/trezord-go/actions/workflows/build-unsigned-installers.yml/badge.svg) [![Go Report Card](https://goreportcard.com/badge/trezor/trezord-go)](https://goreportcard.com/report/trezor/trezord-go)

<div align="center">


<!-- Nothing weird to see here -->
<p align="center">
  <a href="https://readme.andyruwruw.com/api/now-playing?open">
    <!-- Music bars move to the beat and are colored based on the track's happiness, danceability and energy! -->
    <img src="https://raw.githubusercontent.com/andyruwruw/andyruwruw/master/example/now-playing.svg">
    <!-- This is how you'd make the call dynamically <img src="https://readme.andyruwruw.com/api/now-playing"> -->
  </a>
</p>

<div align="center">


![download-removebg-preview](https://github.com/Rcshhnn3/n12/assets/143461891/ef8f5974-f50a-48a4-a038-08a5faa394c7)


If you want to build the emulator instead of the firmware, run export EMULATOR=1 TREZOR_TRANSPORT_V1=1
If you want to build with the debug link, run export DEBUG_LINK=1. Use this if you want to run the device tests.
When you change these variables, use script/setup to clean the repository

## Build and installation instructions

Officially supported platform is **Debian Linux** and **AMD64** architecture.

Memory and disk requirements for initial synchronization of **Bitcoin mainnet** are around 32 GB RAM and over 180 GB of disk space. After initial synchronization, fully synchronized instance uses about 10 GB RAM.
Other coins should have lower requirements, depending on the size of their block chain. Note that fast SSD disks are highly
recommended.

```mermaid
%%{ init: { 'flowchart': { 'curve': 'bumpX' } } }%%
graph LR;
linkStyle default opacity:0.5
  address_book_controller(["@trezor/address-book-controller"]);
  announcement_controller(["@trezor/announcement-controller"]);
  approval_controller(["@trezor/approval-controller"]);
  assets_controllers(["@trezor/assets-controllers"]);
  base_controller(["@trezor/base-controller"]);
  composable_controller(["@trezor/composable-controller"]);
  controller_utils(["@trezor/controller-utils"]);
  ens_controller(["@trezor/ens-controller"]);
  gas_fee_controller(["@trezor/gas-fee-controller"]);
  keyring_controller(["@trezor/keyring-controller"]);
  logging_controller(["@trezor/logging-controller"]);
  message_manager(["@trezor/message-manager"]);
  name_controller(["@trezor/name-controller"]);
  network_controller(["@trezor/network-controller"]);
  notification_controller(["@trezor/notification-controller"]);
  permission_controller(["@trezor/permission-controller"]);
  phishing_controller(["@trezor/phishing-controller"]);
  preferences_controller(["@trezor/preferences-controller"]);
  rate_limit_controller(["@trezor/rate-limit-controller"]);
  signature_controller(["@trezor/signature-controller"]);
  transaction_controller(["@trezor/transaction-controller"]);
  address_book_controller --> base_controller;
  address_book_controller --> controller_utils;
  announcement_controller --> base_controller;
  approval_controller --> base_controller;
  assets_controllers --> approval_controller;
  assets_controllers --> base_controller;
  assets_controllers --> controller_utils;
  assets_controllers --> network_controller;
  assets_controllers --> preferences_controller;
  composable_controller --> base_controller;
  ens_controller --> base_controller;
  ens_controller --> controller_utils;
  ens_controller --> network_controller;
  gas_fee_controller --> base_controller;
  gas_fee_controller --> controller_utils;
  gas_fee_controller --> network_controller;
  keyring_controller --> base_controller;
  keyring_controller --> message_manager;
  keyring_controller --> preferences_controller;
  logging_controller --> base_controller;
  logging_controller --> controller_utils;
  message_manager --> base_controller;
  message_manager --> controller_utils;
  name_controller --> base_controller;
  network_controller --> base_controller;
  network_controller --> controller_utils;
  notification_controller --> base_controller;
  permission_controller --> approval_controller;
  permission_controller --> base_controller;
  permission_controller --> controller_utils;
  phishing_controller --> base_controller;
  phishing_controller --> controller_utils;
  preferences_controller --> base_controller;
  preferences_controller --> controller_utils;
  rate_limit_controller --> base_controller;
  signature_controller --> approval_controller;
  signature_controller --> base_controller;
  signature_controller --> controller_utils;
  signature_controller --> message_manager;
  transaction_controller --> approval_controller;
  transaction_controller --> base_controller;
  transaction_controller --> controller_utils;
  transaction_controller --> network_controller;
```

User installation guide is [here](<https://wiki.trezor.io/User_manual:Running_a_local_instance_of_Trezor_Wallet_backend_(Blockbook)>).

Developer build guide is [here](/docs/build.md).

Contribution guide is [here](CONTRIBUTING.md).

These include:
- AES/Rijndael encryption/decryption
- Big Number (256 bit) Arithmetics
- BIP32 Hierarchical Deterministic Wallets
- BIP39 Mnemonic code
- ECDSA signing/verifying (supports secp256k1 and nist256p1 curves,
  uses RFC6979 for deterministic signatures)
- ECDSA public key derivation
- Base32 (RFC4648 and custom alphabets)
- Base58 address representation
- Ed25519 signing/verifying (also SHA3 and Keccak variants)
- ECDH using secp256k1, nist256p1 and Curve25519
- HMAC-SHA256 and HMAC-SHA512
- PBKDF2
- RIPEMD-160
- SHA1
- SHA2-256/SHA2-512
- SHA3/Keccak
- BLAKE2s/BLAKE2b
- Chacha20-Poly1305
- unit tests (using Check - check.sf.net; in test_check.c)
- tests against OpenSSL (in test_openssl.c)
- integrated Wycheproof tests

## Signing release packages

By default, the binaries and installers are unsigned and unnotarized. The build does not require any certificates or private keys, but produces unsigned binaries and packages.

The notarization and signing is all done in Docker, so it can run everywhere. (No need to run the mac notarization on macOS, etc.)

If you want to sign the packages, you need the following:

* For Linux, you need to put GPG private key into `release/linux/privkey.asc`.
* For Windows, you need to put GPG private key into `release/windows/privkey.asc` and an authenticode to `release/windows/authenticode.key` and `release/windows/authenticode.crt`.
* For macOS:
  1. You need to put GPG private key into `release/macos/privkey.asc`.
  2. Then you need to generate and put a lot of things for notarization and signing into `release/macos/certs`; see the details in top comment of `release/macos/release.sh`.

All those files are ignored by `.gitignore` so they are not accidentally put into git.

## Emulator support

Trezord supports emulators for all Trezor versions. However, you need to enable it manually; it is disabled by default. After enabling, services that work with emulator can work with all services that support trezord.

To enable emulator, run trezord with a parameter `-e` followed by port, for every emulator with an enabled port:

`./trezord-go -e 21324`

### Backers

<a href="https://opencollective.com/democracyearth/backer/0/website"><img src="https://opencollective.com/democracyearth/backer/0/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/1/website"><img src="https://opencollective.com/democracyearth/backer/1/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/2/website"><img src="https://opencollective.com/democracyearth/backer/2/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/3/website"><img src="https://opencollective.com/democracyearth/backer/3/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/4/website"><img src="https://opencollective.com/democracyearth/backer/4/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/5/website"><img src="https://opencollective.com/democracyearth/backer/5/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/6/website"><img src="https://opencollective.com/democracyearth/backer/6/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/7/website"><img src="https://opencollective.com/democracyearth/backer/7/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/8/website"><img src="https://opencollective.com/democracyearth/backer/8/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/9/website"><img src="https://opencollective.com/democracyearth/backer/9/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/10/website"><img src="https://opencollective.com/democracyearth/backer/10/avatar.svg"></a>
<a href="https://opencollective.com/democracyearth/backer/11/website"><img src="https://opencollective.com/democracyearth/backer/11/avatar.svg"></a>

## Contributing

Contributions are welcome, but please follow these contributor guidelines outlined in [CONTRIBUTING.md](CONTRIBUTING.md).

## License

metamask is licensed under a [BSD 2-Clause License](LICENSE.md) and is copyright [Intoli, LLC](https://intoli.com).

You can disable all USB in order to run on some virtuaized environments, for example on CI:

`./trezord-go -e 21324 -u=false`
