# Locklift config setting

### Possible problems

You most likely have a locklift config marked as incorrect. We'll fix it now!

{% hint style="success" %}
Ignore the incorrect factory import for now, as soon as we compile our first contract, this error will disappear
{% endhint %}

![](<../.gitbook/assets/image (8).png>)

{% hint style="info" %}
If you get an error like in the screenshot below this hint, then hardcode the locklift and everscale-standalone-client version in package.json as shown in the screenshot:

![](<../.gitbook/assets/image (23).png>)
{% endhint %}

![](<../.gitbook/assets/image (5).png>)

### Local network entrypoint

We will use our sandbox, so let's delete the graphql in the path

```javascript
const LOCAL_NETWORK_ENDPOINT = "http://localhost/graphql"; 
// to
const LOCAL_NETWORK_ENDPOINT = "http://localhost/;
```

### Compilator and tmv linker

![](<../.gitbook/assets/image (11).png>)

### Local network config

We will only work in the sandbox, the only thing we need to change is the mnemonic phrase. You can generate your own, or use the one commented out in the code

![](<../.gitbook/assets/image (10).png>)

{% hint style="success" %}
Let's make sure that we have everything set up correctly, and try to compile the sample contract
{% endhint %}

```powershell
 npx locklift build 
```

As a result, a message about successful building should appear in the terminal, and your config will no longer be marked as erroneous

![](<../.gitbook/assets/image (20).png>)
