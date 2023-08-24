# Deploy token wallet

With the deployment of the wallet

```typescript

import { Address, Contract } from "locklift/.";
import { AccountAbi } from "../build/factorySource";

async function main() {
  const tokenRootAddress = "";
  const myAccountAddress = "";
   
  const signer = (await locklift.keystore.getSigner("0"))!;
  const accountFactory = locklift.factory.getAccountsFactory("Account");
   
  /* 
    Get Account contract instance: Account<AccountAbi>
  */
  const account = accountFactory.getAccount(myAccountAddress, signer.publicKey);
  
  /* 
    Get instance of already deployed contract
  */
  const tokenRoot = locklift.factory.getDeployedContract(
    "TokenRoot",
    new Address(tokenRootAddress),
  );
  
  /* 
    Under the hood of runTarget, the Account sendTransaction method is called 
  */
  await account.runTarget(
    {
      contract: tokenRoot,
      value: locklift.utils.toNano(1),
    },
    tr =>
      tr.methods.deployWallet({
        answerId: 0,
        walletOwner: account.address,
        deployWalletValue: 0,
      }),
  );
  
  /* 
    We call the get method on the contract.
    @params answerId: If the function marked as responsible then
      field answerId appears in the list of input parameters
        of the function in *abi.json file. answerId is function id
        that will be called.
      In external calls, we did not notice that this affected anything.
        But answerid is critical when you call these functions on-chain
  */
  const wallet = await tokenRoot.methods
    .walletOf({
      answerId: 0,
      walletOwner: account.address,
    })
    .call({ responsible: true });
  console.log(`Account deployed at: ${Account.address}`);
  console.log(`TIP3 Wallet deployed at: ${wallet.value0.toString()}`);
}

main()
  .then(() => process.exit(0))
  .catch(e => {
    console.log(e);
    process.exit(1);
  });

```

{% hint style="info" %}
TON Solidity compiler allows specifying different parameters of the outbound internal message that is sent via external function call. Note, all external function calls are asynchronous, so callee function will be called after termination of the current transaction. `value`, `currencies`, `bounce` or `flag` options can be set. See [\<address>.transfer()](https://github.com/tonlabs/TON-Solidity-Compiler/blob/master/API.md#addresstransfer) where these options are described.&#x20;

**Note:** if `value` isn't set, then the default value is equal to 0.01 ever, or 10^7 nanoever. It's equal to 10\_000 units of gas in workchain. If the callee function returns some value and marked as `responsible` then `callback` option must be set. This callback function will be called by another contract. Remote function will pass its return values as function arguments for the callback function. That's why types of return values of the callee function must be equal to function arguments of the callback function. If the function marked as `responsible` then field `answerId` appears in the list of input parameters of the function in `*abi.json` file. `answerId` is function id that will be called.
{% endhint %}

Use this command and deploy token wallet

```shell-session
npx locklift run -s ./scripts/02-deploy-wallet.js -n local
```

![](<../.gitbook/assets/image (17).png>)

