# Embedding the TIP-3 token repository

For our further work, we need TIP-3 token contracts. It would be inconvenient to copy them to our project. Therefore, we import them into locklift.



Add repository TIP-3 and Broxus Contracts to package.json dependencies.

```json
{
...
"devDependencies": {
    "@broxus/tip3": "git+https://github.com/broxus/tip3.git#5.0",
    "@broxus/contracts": "^1.1.0",
    ...
  },
 ...
}
```



Finally, we need to tell locklift that he has external contracts.&#x20;

Add to locklift.config.ts/compiler

```typescript
externalContracts: {
      "node_modules/@broxus/tip3/build": ["TokenRoot", "TokenWallet"],
 },
```

Let's set up our contracts, and and compile to make sure

```powershell
npm i
npx locklift build
```



{% hint style="success" %}
If you did everything correctly, then in the build folder you should have TIP-3 contracts: TokenRoot and TokenWallet
{% endhint %}

![](<../.gitbook/assets/image (9).png>)
