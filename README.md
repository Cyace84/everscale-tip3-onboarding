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

## Deploy ment
### Deploy to an external owner

Use this command and follow the interactive menu

```shell
yarn locklift run -s ./scripts/10-deploy-token-root.js -n local --config locklift.config.js
```
<img width="1085" alt="image" src="https://user-images.githubusercontent.com/44075582/181854237-08ff42a0-960f-4f05-90aa-c2d8a4a7074e.png">

### Deploy to an inernal owner

You can deploy a Token Root by sending a message through your Account:


```typescript
yarn locklift run -s ./scripts/10-deploy-token-root.js -n local --config locklift.config.js
```
