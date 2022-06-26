# ChainAuth for Game Developers

## What is ChainAuth?

ChainAuth provides a solution to one of the many difficulties when developing a Web 3 standalone game : **authenticating a player's non-custodial wallet to validate the tokens, NFTs, or other digital assets a player owns**. The most secure way to prove that players own a given wallet is to ask them to sign a random message (nonce) with the wallet's private key and then cryptographically validate that the signature could only have been generated with the player's private key. However, without a backend solution, most wallets do not natively have a means to connect to with other standalone applications. While some solutions exist if the game or app exists in a browser or for just mobile wallets using Wallet Connect, ChainAuth provides the APIs and SDKs neccesary to validate your players' wallets whether they prefer using :
- Mobile Wallet Apps i.e. Metamask [Android](https://play.google.com/store/apps/details?id=io.metamask&hl=en_US&gl=US) or [iOS](https://apps.apple.com/us/app/metamask-blockchain-wallet/id1438144202)
- Web Browser Wallets i.e. [Brave](https://brave.com/wallet/) or [Metamask](https://metamask.io/)
- or Hardware Wallets i.e. [Ledger](https://www.ledger.com/metamask) or [Trezor](https://wiki.trezor.io/Apps:MetaMask)

ChainAuth enables developers to send a magic email to their players, which if opened from a mobile wallet will re-direct to the player's wallet for a signature. And if opened from the browser, it will request the signature from the web browser wallet / hardware wallet. Once a player signs the random nonce, a message is sent back to your game with the validated wallet address. Once validated, you can now read the state of the blockchain to see what NFTs, tokens, and assets a player has on-chain. 

[![Connect to Any Wallet](https://img.youtube.com/vi/8bQfi_mL6T4/maxresdefault.jpg)](https://www.youtube.com/watch?v=8bQfi_mL6T4)

## Why FusedVR ChainAuth

The ChainAuth solution by FusedVR makes it incredibly simple for developers to solve the authentication problem either using the [Unity SDK](https://github.com/FusedVR/web3-unity-sdk) or via our API integration. For indie developers working in Unity, the SDK integration is really as simple as :

1. Add the Web3 SDK to your project via the Package Manager
2. Get the e-mail address of your player
3. Call the login function in the SDK to send an email to the player and await their signature

```csharp
bool isLoggedIn = await Web3Manager.Login("email", "appId");
```

## Use Cases

The ChainAuth solution provides an efficient solution for passwordless authentication of players, which enables use cases that require just reading the state of the blockchain once authenticated. Such use cases could include:

- Determining if a player owns digital assets associated with the game i.e. avatar skins, in game items, or tokens.
- Determining if a player owns an NFT the represents ownership of the game
- Whether or not a player has interacted with a custom smart contract and the events emitted as a result
- Cross-Chain ownership of EVM assets, such as Binance Smart Chain, Polygon, or Ethereum

The ChainAuth APIs and SDKs are **NOT** currently recommended for writing transactions to the blockchain from your game. For such use cases, it is recommended to create your own dApp outside of the standalone game that players can interact with such as a NFT marketplace. More work & research is required to make the UX of the onboarding process as seamless as possible for new users, especially those who have never used the blockchain before. 