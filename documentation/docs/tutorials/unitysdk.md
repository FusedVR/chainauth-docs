# Unity SDK Guide

The FusedVR ChainAuth Unity SDK is designed to be as simple as possible to integrate into any game or application. The goal of the SDK is to enable you to authenticate your player's Web 3 wallets and enable them to prove ownership of their digital assets like tokens & NFTs. This guide will walk you through how to use the SDK to get access to the SDK, authenticate a player, and then verify the assets the player owns, which you can use however you like in the game. 

## Setup SDK

The ChainAuth Unity SDK is avaliable on [Github as an open source project](https://github.com/FusedVR/web3-unity-sdk). To use the SDK, you will require an Application ID, which you can request on the [FusedVR Discord Server](https://discord.com/invite/rV8fEAmG5B).

Once you have an Application ID, you can import the Github repo into your repository as a Unity Package. In your Unity Project, open the Package Manager, which is under **Window** *>* **Package Manager**. Click the **+** icon and select **Add package from git URL...**

This will prompt you to input a URL, in which you should paste the Git URL : [https://github.com/FusedVR/web3-unity-sdk.git](https://github.com/FusedVR/web3-unity-sdk.git). Click **Add**, which begin the process of importing the SDK into your project. 

## Authenticating Players with their Wallet

Once you have installed the SDK, you will need to integrate the Client Registration and Login Sequence into your game in order to authenticate players. The following code snippet shows an example of how you can authenticate players with just a few lines of code. In this section, we will walk through what each of these functions does in the authenticate process for players. 

```csharp
Web3Manager manager = await Web3Manager.Register("unique id", "appId"); //register the client with the APIs
Debug.Log(manager.RegisterCode); //auth code for player to input into the link page received from Registration

if (await manager.AwaitLogin()) { //await player login
	//your game logic here
    CallAPIs(manager, Web3Manager.CHAIN.eth);
}
```

### Register and Get Registration Code

The first step in using the SDK is Registering the Client. To do so, call the static function ***Register***, which is responsible for calling the ***Register API*** for you. THe parameters that are required are a unique ID for this client and the Application ID from above. **NOTE: if the unique ID is an email address, an email will automatically be sent to the player, which will allow them to click on the link and authenticate without the need for a code.**

```csharp
Web3Manager manager = await Web3Manager.Register("unique id", "appId"); //register the client with the APIs
```
The SDK will return a Web3Manager Object, which will enable you to make all the required calls for other supported functionality below. This object will store the Registeration Code that is retured from calling the Register API. 

You will need to provide this code to the player on a GUI so that they can input the code into [https://link.fusedvr.com](https://link.fusedvr.com)

```csharp
manager.RegisterCode; //auth code for player to input into the link page received from Registration
```

These codes are only valid for 10 minutes. If the code has become invalid, you may call the non-static ***Register*** function to re-register and get a valid code. 

```csharp
string code = await manager.Register(); //re-register to get new auth code
```

### Magic Link

Alternatively, if you would like to provide a clickable experience without a code, you call call the GetMagicLink function after Register.

This will return a URL, which players can click on to start the authentication process to your application without having to enter a code.

```csharp
Web3Manager manager = await Web3Manager.Register("unique id", "appId"); //register the client with the APIs
string magicLink = await manager.GetMagicLink(); //gets the magic link for the player
```

### Await Login

Once you have registered the player and have provided them a means to start the authentication process (via email, code, or magic link), you then need to wait for them to authenticate their wallet. To await for the player to login, you can call the ***AwaitLogin*** function. 

```csharp
bool success = await manager.AwaitLogin(); //awaits player login
```
This will return once the login process is complete whether or not it was succesful, at which point you can start reading the state of their wallet. 

## Verifying Player's Digital Assets on the Blockchain

### Authentication Token

Once a player has logged in and authenticated your app, you will receive back a JWT token, which when decoded contains important information, such as the player's wallet address & the signature proof they used to authenticate. A valid token will enable you to call FusedVR APIs and read the state of the blockchain more easily.

This token is saved in your Player Prefs, so that you can quickly login players between sessions without needing to re-authenticate each time. This requires the Unique ID you used to remain the same between sessions.

You can get the Bearer Token at any time from a Logged In user through the Web3Manager and Decode it to see the contents of the token, which are stored as a key value pair in a dictionary. 

```csharp
string token = manager.GetBearerToken(); //get token
Dictionary<string, string> tokenContent = Web3Manager.DecodeJWT(token); //decode contents of the token as a key-value pair
```

### Player Address

As mentioned above, the player's authenticated wallet address is stored within the authentication token. To make it easier to get the player's address, a wrapper function is provided. 

```csharp
string address = mngr.GetAddress(); //get the wallet address used for authentication
```

### Select Chain

The FusedVR Crypto APIs support a subset of EVM compatiable blockchains & testnets, which stored in the CHAIN sub-class. When you call any of the following functions, you will be required to pass an instance of the CHAIN object, which will indicate which blockchain to query. Here are the currently supported chains:

```csharp
Web3Manager.CHAIN.eth
Web3Manager.CHAIN.ropsten
Web3Manager.CHAIN.rinkeby
Web3Manager.CHAIN.goerli
Web3Manager.CHAIN.polygon
Web3Manager.CHAIN.mumbai
Web3Manager.CHAIN.bsc
Web3Manager.CHAIN.bsctestnet
```

### Get Native Balance

Once a player is authenticated, you can pick any of the supported chains to retrive the native balance in the native currency of the player's wallet address. For example, on the ethereum blockchain, you would receive the balance of the player in [Wei](https://www.investopedia.com/terms/w/wei.asp). 

```csharp
string balance = await mngr.GetNativeBalance(chain);
```

### Get ERC-20 Tokens

Once a player is authenticated, you can get the full list of ERC-20 Tokens (or equivalent on other chains) that a player owns. The result is an array of all owned tokens and the attributes of each token. 

```csharp
List<Dictionary<string, string>> erc20s = await mngr.GetERC20Tokens(chain);
```
In JSON format, here is an example of the returned attributes of each ERC-20 token

```json
  {
    "token_address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
    "name": "Circle USDC",
    "symbol": "USDC",
    "logo": "ipfs://logo-url.png",
    "thumbnail": "ipfs://thumbnail-logo-url.png",
    "decimals": 6,
    "balance": "101"
  }
```

### Get NFT Tokens

Once a player is authenticated, you can get the full list of NFTs (ERC-721 or ERC-1155) that a player owns. The result is an array of all owned tokens and the attributes of each token. 

```csharp
List<Dictionary<string, string>> nfts = await mngr.GetNFTTokens(chain);
```

```json
  {
    "token_address": "0x4d66898952d879c27bdadb5876f38e38a083eff8",
    "token_id": "token1",
    "owner_of": "0x74bcd46288e661ecea04992d5e8fbe6c29cf6f64",
    "block_number": "27529244",
    "block_number_minted": "22171688",
    "amount": "1",
    "contract_type": "ERC1155",
    "name": "Axie",
    "symbol": "AXS",
    "token_uri": "https://raw.githubusercontent.com/FusedVR/nft.games/master/1/meta.json",
    "metadata": {
      "\\n  \\\"name\\\"": "\\\"FusedVR Token\\\"",
      "\\n  \\\"description\\\"": "\\\"This token is an example ERC-1155 asset that can be used to trigger streaming with FusedVR Render Streaming. In other words",
      "think of this as token representation of purchasing a game.\\\"": null,
      "\\n  \\\"image\\\"": "\\\"https://raw.githubusercontent.com/FusedVR/nft.games/master/1/FusedVR.png\\\"",
      "\\n  \\\"external_url\\\"": "\\\"https://fusedvr.com/rendering\\\"\\n"
    }
  }
```