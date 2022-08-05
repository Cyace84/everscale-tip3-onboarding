# tip3-deploy-onboarding

### Built With
- NodeJS (tested with NodeJS v16+)
- Yarn

## Getting Started

### Prerequisites

Clone the TIP-3 token repository
```shell
git clone https://github.com/broxus/tip3
cd tip3
```

Install node modules:

```shell
yarn install
```

Rname the locklift.config.template.js file to llocklift.config.js
and edit the following options:

- compiler.path
- linker.path
- networks.local.keys.phrase

## Deploy Token Root
### Deploy to an external owner

Use this command and follow the interactive menu

```shell
yarn locklift run -s ./scripts/10-deploy-token-root.js -n local --config locklift.config.js
```
<img width="1085" alt="image" src="https://user-images.githubusercontent.com/44075582/181854237-08ff42a0-960f-4f05-90aa-c2d8a4a7074e.png">

## Deploy Token Wallet

You can deploy a TIP3 wallet using an account or implement it in a smart contract:

### With Account
```javascript
 const accountsFactory = locklift.factory.getAccountsFactory("Account");
 const account = accountsFactory.getAccount(myAccountAddress, myPublicKey);
 const tokenRoot = locklift.factory.getDeployedContract("TokenRoot", tokenRootAddress);
 await account.runTarget(
    {
      contract: tokenRoot,
      value: locklift.utils.toNano(6),
    },
    tr =>
      tr.methods.deployWallet({
        walletOwner: account.address,
        deployWalletValue: locklift.utils.toNano(5),
      }),
  );
  
  const walletAddress = (
    await tokenRoot.methods
      .walletOf({ answerId: 0, walletOwner: account.address })
      .call({ responsible: true })
  ).value0;
```

### Вirectly in the smart contract

We have two functions. One accesses the token root and deploys a new wallet, and the second accepts a callback with a newly created address

```solidity
pragma ton-solidity >= 0.57.3;
pragma AbiHeader expire;
pragma AbiHeader pubkey;

import '@broxus/contracts/contracts/utils/CheckPubKey.sol';
import "@broxus/tip3/contracts/interfaces/ITokenRoot.sol";

contract DeployerTIP3Wallet is CheckPubKey {
    address tokenRoot;
    address tokenWallet;

    function deployTIP3Wallet(
        uint128 _deployWalletBalance,
        address _owner
    ) public checkPubKey view {
        tvm.accept();
        ITokenRoot(tokenRoot).deployWallet{
            value: _deployWalletBalance + 0.5 ever,
            flag: 2,
            callback: DeployerTIP3Wallet.receiveTokenWalletAddress
        }(
            _owner,
            _deployWalletBalance
        );
    }

    function receiveTokenWalletAddress(address wallet) external {
        require(msg.sender == tokenRoot, 200, "Sender is not Token Root");
        tokenWallet = wallet;
    }
}

```



```typescript
 const accountsFactory = locklift.factory.getAccountsFactory("Account");
 const account = accountsFactory.getAccount(myAccountAddress, myPublicKey);
 const deployer = locklift.factory.getDeployedContract("DeployerTIP3Wallet", deployerADdress);
 await account.runTarget(
    {
      contract: deployer,
      value: locklift.utils.toNano(6),
    },
    tr =>
      tr.methods.deployTIP3Wallet({
        _deployWalletBalance: locklift.utils.toNano(5),
        _owner: account.address,
      }),
  );
  const myTokenWallet : Address = await deployer.methods.tokenWallet({}).call({});
```

## Transfer tokens

### With Account

```javascript
 const accountsFactory = locklift.factory.getAccountsFactory("Account");
 const account = accountsFactory.getAccount(myAccountAddress, myPublicKey);
 const tokenWallet = locklift.factory.getDeployedContract("TokenWallet", tokenWalletAddress);
 await account.runTarget(
    {
      contract: tokenWallet,
      value: locklift.utils.toNano(2),
    },
    tw =>
       tw.methods.transfer({
        amount: 100,
        recipient: aliceAddress,
        deployWalletValue: 0,
        remainingGasTo: account.address,
        
        (** 
             Flag for token wallet to send transfer callback. 
             We are passing Falls because the base Account does not have a callback implementation.
         **)
        notify: false,  
        payload: "te6ccgEBAQEAAgAAAA==",
      }),
  );
  
  const walletAddress = (
    await tokenRoot.methods
      .walletOf({ answerId: 0, walletOwner: account.address })
      .call({ responsible: true })
  ).value0;
```
