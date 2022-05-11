---
description: The quick and easy way to start creating tables on Tableland.
---

# Quick Start

## Get Access

For now, Tableland API requests are authenticated by ETH address. Any request that doesn't include a self-signed access token that is tied to a per-registered ETH address will return an error.

You can gain access to the Tableland MVP by joining our Discord. While you're there, don't forget to say hello in the `#👋-intros` channel! Oh, and welcome to the community!

{% embed url="https://discord.gg/A7JpNjpk3K" %}

{% hint style="info" %}
Tableland is currently in private beta, so to join you’ll need to do a few more steps! Having trouble getting access? One of the core Tableland developer team members will be happy to help. Just ask in #general or #support!
{% endhint %}

Next, jump into the `#🎟-dev-whitelist` channel and post your ETH public address. It should look something like this:

```bash
0xbAb12215Ed94713A290e0c618fa8177fAb5eFd2D
```

{% hint style="danger" %}
Only ever share you _public_ address. We will never DM you and we will never ask you for your secrets, private keys, passwords, or phrases. Be diligent, and report scammers and spammers.
{% endhint %}

That's it! Your ETH address will be added to the access control list shortly. You’ll be able to use `#🔩-dev-chat` to share your project, find collaborators, learn from others, ask technical questions, or let us know about any issues you run into. We'll also be posting periodic updates in all of our other discord channels too, so stay tuned!

## Play in the Browser

The best way to interact and play with our API is to use our official playground tools. You can test out a simple interactive terminal for querying tables on Tableland below.

{% hint style="info" %}
Be sure to connect with Metamask before running queries in the demo below. Feel free to explore the demo on codepen for the full-screen effect!
{% endhint %}

{% embed url="https://codepen.io/carsonfarmer/pen/BamZJEQ" %}

Or jump on over to [our website](https://tableland.xyz) where you can play with our integrated testnet Terminal!

## Build with our SDK

The best way to _build on_ Tableland is to use our official Typescript/Javascript client. You can install this via `npm` or `yarn`. You can also pull the ESM build via `cdn.skypack.dev` or other means. For details and code snippets to get you going quickly, jump on over to our Javascript SDK guide.

{% content-ref url="javascript-sdk.md" %}
[javascript-sdk.md](javascript-sdk.md)
{% endcontent-ref %}

## Explore with our API

If raw REST and RPC APIs are more your thing, then you might be more comfortable digging into our raw APIs. You can find API specs, command line examples, and more in our Remote API guide.

{% content-ref url="remote-api.md" %}
[remote-api.md](remote-api.md)
{% endcontent-ref %}

## Tinker from the CLI

Sometimes you just wanna type stuff into a terminal and observe the results. That's what the experimental command line tools for Tableland are for. Read on to get started playing with Tableland from the comfort of your command line.

{% content-ref url="command-line.md" %}
[command-line.md](command-line.md)
{% endcontent-ref %}
