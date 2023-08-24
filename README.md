# Smart-contracts Onboarding

In this onboarding, we will get acquainted with Everscale smart contracts. We  take the [TIP-3 ](https://github.com/broxus/tip3)token as an example and look at two ways to interact with the TIP-3 Token.

We will use default Account and  typescript. And in the second case, we will write our smart contract using the TIP-3 Token



{% hint style="info" %}
TIP-3 Token is a Everscale token standard that describes the basic principles of building smart token contracts.

The TIP-3 token standard contains:

* Smart contracts of custom wallets have the right to deploy only the root smart contract from their address.
* User wallets have the same code as the root smart contract, but contain different user data.
* The TIP-3 standard also describes the basic ways of interaction of user smart contracts with each other and a mechanism for checking the type of a smart contract by its address on the blockchain.

This standard describes only the basic principles, so Everscale has created a consortium for extensions to the TIP-3 standard. The consortium registers the characteristics of the token's smart contract, after which it is stored inside the blockchain.
{% endhint %}
