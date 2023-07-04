# ENS Executable Functions Wallet Guide

These are interactions the ENS Manager dApp suggests might suggest the user execute.
The aim of this document is to aggregate functions in order to provide transparency in user frontends.

## Resolvers

Any smart-contract could be a resolver. A Resolver contract is responsible for retrieving the data associated with a name.
The ENS Registry contract stores the address of the resolver contract responsible for a name.

| Contract | Address                                                                                                               |
| -------- | --------------------------------------------------------------------------------------------------------------------- |
| Public   | [0x4976fb03C32e5B8cfe2b6cCB31c09Ba78EBaBa41](https://etherscan.io/address/0x4976fb03C32e5B8cfe2b6cCB31c09Ba78EBaBa41) |

### Public Resolver

There is however one special resolver, the "public resolver". This resolver is a generalized key-value storage resolver that anyone can use.
It is the default resolver for all names, unless changed by the user through [setting resolver](#setting-resolver).

#### Public Resolver 2

resolver.eth

## ETH Registrar

ETHRegistrarController / ETHRegistrar / Old ETHRegistrar Controller

| Contract                     | Address                                                                                                               |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| ETH Registrar Controller     | [0x253553366da8546fc250f225fe3d25d0c782303b](https://etherscan.io/address/0x253553366da8546fc250f225fe3d25d0c782303b) |
| Old ETH Registrar Controller | [0x283af0b28c62c092c9727f1ee09c02ca627eb7f5](https://etherscan.io/address/0x283af0b28c62c092c9727f1ee09c02ca627eb7f5) |

### `commit(commitment bytes32)`

Signature: 0xf14fcbc8

This function is used to commit to a name before registering it. The bytes32 is an opaque hash of the name so the user can't be frontrun. This transaction is generally followed by [registerWithConfig](#registerwithconfigname-string-owner-address-duration-uint256-secret-bytes32-resolver-address-addr-address).

[Example Transaction](https://etherscan.io/tx/0xea772f0f05543cc90e25a19997c0430d82e85331d45e2264603bc3cd2bbff434) registering `meowy.eth`,
followed by [registerWithConfig](#registerwithconfig) ([here](https://etherscan.io/tx/0x07528c73fe837a339e2f48188278e3f2fadabb8e56c7766ddadd69f3a009b0f5)) to complete the registration.

#### To reproduce

Open ENS Manager dApp, type in a name you would like to register, select "Ethereum", and press "Next", "Begin", "Start Timer", "Open Wallet".

### `register(name string, owner address, duration uint256, secret bytes32)`

Signature: 0x85f6d155

This function is used to register a name. In contrary to [commit](#commitcommitment-bytes32) this function takes the name as a string and the secret as a bytes32. This function under the hood runs [registerWithConfig](#registerwithconfigname-string-owner-address-duration-uint256-secret-bytes32-resolver-address-addr-address)`(name, owner, duration, secret, address(0), address(0))`.

This transaction has a **payable amount**.

### `registerWithConfig(name string, owner address, duration uint256, secret bytes32, resolver address, addr address)`

Signature: 0xf7a16963

This function is used to register a name. In contrary to [commit](#commitcommitment-bytes32) this function takes the name as a string and the secret as a bytes32.
This function requires that the user [commit](#commitcommitment-bytes32)ed to this name beforehand.

This transaction has a **payable amount**.

[Example Transaction](https://etherscan.io/tx/0x07528c73fe837a339e2f48188278e3f2fadabb8e56c7766ddadd69f3a009b0f5) registering `meowy.eth`.
The [commit](#commitcommitment-bytes32) transaction that preceded it can be found [here](https://etherscan.io/tx/0xea772f0f05543cc90e25a19997c0430d82e85331d45e2264603bc3cd2bbff434).

#### To reproduce

Open ENS Manager dApp, type in a name you would like to register, select "Ethereum", and press "Next", "Begin", "Start Timer", "Open Wallet", execute the [commit](#commitcommitment-bytes32) transaction, and wait 60 seconds. Now press "Open Wallet" again and execute the [registerWithConfig](#registerwithconfigname-string-owner-address-duration-uint256-secret-bytes32-resolver-address-addr-address) transaction.

### `renew(name string, duration uint256)`

Signature: 0xacf1a841

This function is used to renew a name.

This transaction has a **payable amount**.

[Example Transaction](https://etherscan.io/tx/0xd17a36d5c2ffd629ef63f443144f389b3600ec8560f5e016b2a56adf87d6eeac) renewing `helgesson.eth` for `31536000` **seconds** (1 year).

#### To reproduce

Open ENS Manager dApp, navigate to any name, select "Extend", press "Next", "Open Wallet".

## ReverseRegistrar

| Contract          | Address                                                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Reverse Registrar | reverse.ens.eth [0xa58E81fe9b61B5c3fE2AFD33CF304c454AbFc7Cb](https://etherscan.io/address/0xa58E81fe9b61B5c3fE2AFD33CF304c454AbFc7Cb) |
| Old               | [0x9062C0A6Dbd6108336BcBe4593a3D1cE05512069](https://etherscan.io/address/0x9062C0A6Dbd6108336BcBe4593a3D1cE05512069)                 |
| Old 2             | [0x084b1c3C81545d370f3634392De611CaaBFf8148](https://etherscan.io/address/0x084b1c3C81545d370f3634392De611CaaBFf8148)                 |

### `setName(name string)`

[Example Transaction](https://etherscan.io/tx/0x905e106763556d0cb16d1fc11ab13d75cf5b1227e0480098b909bba50c4271b8)

#### To reproduce

Open ENS Manager dApp, navigate to [/my/settings](https://ens.app/my/settings), select "Change", the name you would like to change it to, "Next", "Start", "Open Wallet".

## Registry

## NameWrapper

The NameWrapper is a contract users can use to "wrap" their name in order to provide it with additional functionality.
This functionality includes creating trustless namewrapper subnames (a seperate ERC721 token).

## StaticBulkRenewal

## DNSRegistrar

### `proveAndClaim(name bytes, input DNSSEC.RRSetWithSignature[])`

[Contract Implementation](https://github.com/ensdomains/ens-contracts/blob/787c5d8f1a99ad14435a65784a7c5ceca1e2575e/contracts/dnsregistrar/DNSRegistrar.sol#L90)

### `proveAndClaimWithResolver(name bytes, input DNSSEC.RRSetWithSignature[], resolver address, addr address)`

Signature 

[Contract Implementation](https://github.com/ensdomains/ens-contracts/blob/787c5d8f1a99ad14435a65784a7c5ceca1e2575e/contracts/dnsregistrar/DNSRegistrar.sol#L101)

Basically what [registerWithConfig](#registerwithconfigname-string-owner-address-duration-uint256-secret-bytes32-resolver-address-addr-address) is to [register](#registername-string-owner-address-duration-uint256-secret-bytes32), [proveAndClaimWithResolver](#proveandclaimwithresolvername-bytes-input-dnssecrrsetwithsignature-resolver-address-addr-address) is to [proveAndClaim](#proveandclaimname-bytes-input-dnssecrrsetwithsignature).

## Old BulkRenewal

## Other

ENS Registry With Fallback
[0x00000000000C2E074eC69A0dFb2997BA6C7d2e1e](https://etherscan.io/address/0x00000000000c2e074ec69a0dfb2997ba6c7d2e1e)
