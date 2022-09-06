# ChainAuth for Game Developers

## What is ChainAuth?

ChainAuth provides a simplied solution to Game Developers for one of the many difficulties when developing a Web 3 standalone game : **authenticating a player's non-custodial wallet to prove ownership of the tokens, NFTs, or other digital assets a player owns**. *Why is this a challenge?* Players can own digital assets across a wide range of wallets (Metamask, Coinbase, Trust Wallet, etc.) on various different blockchains (ethereum, binance, polygon, etc.) and games should be flexible to support as many players as possible. One common workaround to this problem is to have the game (or typically a 3rd party service) manage the wallet on behalf of players, known as a custodial wallet. But this fundamentally looks no different from Web 2 and defeats the purpose of enabling players to have true ownership of their assets in the metaverse. 

The only secure way to prove that players own a given wallet is to ask them to sign a random message (nonce) with the wallet's private key and then cryptographically validate that the signature could only have been generated with the player's private key. However, without a backend solution, most wallets do not natively have a means to connect with other standalone applications. While some solutions exist if the game or app exists in a browser or for just mobile wallets using Wallet Connect, ChainAuth provides the APIs and SDKs neccesary to validate your players' wallets whether they prefer using :
- Mobile Wallet Apps i.e. Metamask [Android](https://play.google.com/store/apps/details?id=io.metamask&hl=en_US&gl=US) or [iOS](https://apps.apple.com/us/app/metamask-blockchain-wallet/id1438144202)
- Web Browser Wallets i.e. [Brave](https://brave.com/wallet/) or [Metamask](https://metamask.io/)
- or Hardware Wallets i.e. [Ledger](https://www.ledger.com/metamask) or [Trezor](https://wiki.trezor.io/Apps:MetaMask)

ChainAuth enables developers to **either** provide players a one time code for authentication or send them a magic link email to their players to start the authentication process. By going to [https://link.fusedvr.com](https://link.fusedvr.com), players can input that code in the browser, which will request a signature from their web browser wallet, hardware wallet, or mobile wallet through Wallet Connect. Players will sign a randomly generated message (nonce), which is sent back to your game along with the validated wallet address. Once validated, you can now read the state of the blockchain to see what NFTs, tokens, and assets a player has on any chain associated with that address using the APIs provided.  

[![Connect to Any Wallet](https://img.youtube.com/vi/8bQfi_mL6T4/maxresdefault.jpg)](https://www.youtube.com/watch?v=8bQfi_mL6T4)

## Why FusedVR ChainAuth

The ChainAuth solution by FusedVR is designed by a developers to make it incredibly simple for developers to solve the authentication problem either using the [Unity SDK](https://github.com/FusedVR/web3-unity-sdk) or via our API integration. For indie developers working in Unity, the SDK integration is as simple as :

1. Add the Web3 SDK to your project via the Package Manager
2. Call the Register Function (optionally with an e-mail address) with your application id to get a manger instance with the code
3. Call the Login function in the SDK with that code and await the player's authorization signature

```csharp
Web3Manager instance = await Web3Manager.Register("appId", "optional email");
if (instance.Login()){
	//call Web 3 apis for tokens and nfts
}
```

## Use Cases

The ChainAuth solution provides an efficient solution for passwordless authentication of players, which enables use cases that require just reading the state of the blockchain once authenticated. Such use cases could include:

- Determining if a player owns digital assets associated with the game i.e. avatar skins, in game items, or tokens.
- Determining if a player owns an NFT the represents ownership of the game
- Whether or not a player has interacted with a custom smart contract and the events emitted as a result
- Cross-Chain ownership of EVM assets, such as Binance Smart Chain, Polygon, or Ethereum

The ChainAuth APIs and SDKs are **NOT** currently recommended for writing transactions to the blockchain from your game. For such use cases, it is recommended to create your own dApp outside of the standalone game that players can interact with such as a NFT marketplace. More work & research is required to make the UX of the onboarding process as seamless as possible for new users, especially those who have never used the blockchain before. 