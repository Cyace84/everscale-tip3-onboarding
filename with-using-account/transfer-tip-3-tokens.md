# Transfer TIP-3 Tokens

We have already learned how to send messages to a contract through an account.&#x20;

Therefore, making a transfer is as easy as shelling pears!





{% hint style="info" %}
TIP-3 Token Wallet has 2 transfer methods.



* Transfer - Transfer tokens and optionally deploy TokenWallet for recipient account address.
* Transfer tokens using another TokenWallet address, that wallet must be deployed previously.


{% endhint %}



```typescript
import { Address, Contract } from "locklift/.";
import { AccountAbi } from "../build/factorySource";

const EMPTY_TVM_CELL = "te6ccgEBAQEAAgAAAA==";

async function main() {
  const tokenWalletAddress = "";
  const myAccountAddress = "";
  const aliceAccount = "";

  const signer = (await locklift.keystore.getSigner("0"))!;
  
  const accountFactory = locklift.factory.getAccountsFactory("Account");
  const account = accountFactory.getAccount(myAccountAddress, signer.publicKey);
  
  const tw = locklift.factory.getDeployedContract(
    "TokenWallet",
    new Address(tokenWalletAddress),
  );
  
  /* 
    Transfer with the deployment of a wallet for the recipient account.
    
    Don't pay attention to notify and payload yet, we'll get back to them.
  */
  await account.runTarget(
    {
      contract: tw,
      value: locklift.utils.toNano(6),
    },
    tw =>
      tw.methods.transfer({
        amount: 100,
        recipient: aliceAccount,
        deployWalletValue: locklift.utils.toNano(5),
        remainingGasTo: account.address,
        notify: true,
        payload: EMPTY_TVM_CELL,
      }),
  );
  
  const tokenRootAddress = await tw.methods
    .root({
      answerId: 0,
    })
    .call({ responsible: true });
  
  const tokenRoot = locklift.factory.getDeployedContract(
    "TokenRoot",
    tokenRootAddress.value0,
  );
  
  /* 
    We recognize the newly created token wallet for Alice.
    To send her more tokens.
  */
  const aliceTokenWallet = (await tokenRoot.methods
    .walletOf({
      answerId: 0,
      walletOwner: aliceAccount,
    })
    .call({ responsible: true }))
    .value0;

   /* 
     Сhecking Alice's balance
   */
   const aliceTW: Contract<TokenWalletAbi> =
    locklift.factory.getDeployedContract(
      "TokenWallet",
      aliceTokenWallet,
    );
    
    const  aliceBalance = (await tw.methods
      .balance({
        answerId: 0,
      })
      .call({ responsible: true })).value0;
      
    /* 
       Transfer to deployed token wallet
    */
    await account.runTarget(
      {
        contract: tw,
        value: locklift.utils.toNano(1),
      },
      tw =>
        tw.methods.transferToWallet({
          amount: 100,
          recipientTokenWallet: aliceTokenWallet,
          remainingGasTo: account.address,
          notify: true,
          payload: EMPTY_TVM_CELL,
        }),
    );
    
}

main()
  .then(() => process.exit(0))
  .catch(e => {
    console.log(e);
    process.exit(1);
  });

```

![](<../.gitbook/assets/image (3).png>)
