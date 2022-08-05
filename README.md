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

To deploy a wallet, you need a smart contract

<img width="693" alt="image" src="https://user-images.githubusercontent.com/44075582/183107567-d3378fd8-ad4f-4c15-bbd2-f46aeade169a.png">

```typescript
 const accountsFactory = locklift.factory.getAccountsFactory("Account");
 const account = accountsFactory.getAccountmyAccountAddress, myPublicKey);
 const deployer = locklift.factory.getDeployedContract("DeployerTIP3Wallet", deployerADdress);
 await account.runTarget(
    {
      contract: deployer,
      value: locklift.utils.toNano(1),
    },
    tr =>
      tr.methods.deployTIP3Wallet({
        _deployWalletBalance: locklift.utils.toNano(5),
        _owner: account.address,
      }),
  );
  const myTokenWallet : Address = await deployer.methods.tokenWallet({}).call({});
```
