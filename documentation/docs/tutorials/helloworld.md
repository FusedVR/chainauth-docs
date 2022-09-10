# Hello World - Unity

The FusedVR ChainAuth Unity SDK are designed to be as simple as possible to integrate into any game or application. Below is a short video documenting the process of integrating the SDK into a brand new Unity project, at which point you can check to see what NFTs the player owns. 

```csharp
Web3Manager manager = await Web3Manager.Register("appId", "email");

if (await manager.AwaitLogin()) {
	//your game logic here
    CallAPIs(manager, Web3Manager.CHAIN.eth);
}
```