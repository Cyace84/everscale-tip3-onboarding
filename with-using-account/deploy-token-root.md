# Deploy Token Root

Great! Now we write scripts to deploy Token Root:

```typescript
import { Address } from "locklift/.";

import { isValidEverAddress, isNumeric, zeroAddress, Migration } from "./utils";


async function main() {
  const signer = (await locklift.keystore.getSigner("0"))!;

  const initialSupplyTo = zeroAddress;
  const rootOwner = "";
  const name = "Onboarding Token;
  const symbol = "ONT42";
  const decimals = 6;
  const disableMint = "false";
  const disableBurnByRoot = "false";
  const pauseBurn = "false";

  let initialSupply = "0";
  
  /* 
    Returns compilation artifacts based on the .sol file name
      or name from value config.extarnalContracts[pathToLib].
  */
  const TokenWallet = locklift.factory.getContractArtifacts("TokenWallet");
  
  /* 
    Deploy the TIP-3 Token Root contract.
    @params deployWalletValue: Along with the deployment of the root token,
      the wallet will be automatically deployed to the owner. 
      This is the amount of EVERs that will be sent to the wallet.
  */
  const { contract: tokenRoot } = await locklift.factory.deployContract({
    contract: "TokenRoot",
    publicKey: signer.publicKey,
    initParams: {
      deployer_: new Address(zeroAddress),
      randomNonce_: (Math.random() * 6400) | 0,
      rootOwner_: rootOwner,
      name_: name,
      symbol_: symbol,
      decimals_: decimals,
      walletCode_: TokenWallet.code,
    },
    constructorParams: {
      initialSupplyTo: initialSupplyTo,
      initialSupply: new BigNumber(initialSupply).shiftedBy(decimals).toFixed(),
      deployWalletValue: locklift.utils.toNano(1),
      mintDisabled: disableMint,
      burnByRootDisabled: disableBurnByRoot,
      burnPaused: pauseBurn,
      remainingGasTo: new Address(myAccountAddress),
    },
    value: locklift.utils.toNano(5),
  });

  console.log(`${name}: ${tokenRoot.address}`);
}

main()
  .then(() => process.exit(0))
  .catch(e => {
    console.log(e);
    process.exit(1);
  });

```



Let's run our script using locklift

```powershell
npx locklift run -s ./scripts/01-deploy-token.ts -n local
```

![](<../.gitbook/assets/image (15).png>)



Congratulations, you have deployed your first TIP3 Token Root!
