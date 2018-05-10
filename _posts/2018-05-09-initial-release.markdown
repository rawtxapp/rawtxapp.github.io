---
layout: post
title:  "Introducing rawtx, a mobile lightning wallet!"
date:   2018-05-09 09:01:01 -0700
categories: jekyll update
---
Today, we are releasing rawtx, a mobile lightning network wallet for Bitcoin.
For now, it's available on Android, with iOS version coming soon.

You can download it from [Android Play Store](https://play.google.com/store/apps/details?id=com.rtxwallet).

We have produced 11 short videos (<1 min) showing all the different features on the app, you can
see the playlist on youtube: [https://www.youtube.com/playlist?list=PL6M7JUIlnUqzfZQ4hxdFu2k912cKxWMqK](https://www.youtube.com/playlist?list=PL6M7JUIlnUqzfZQ4hxdFu2k912cKxWMqK).

### Features (aka tl;dr)
* You can **send and receive** testnet Bitcoins on blockchain and lightning network.
* Support **multiple wallets**.
    * Each wallet is composed of 2 accounts: **checking account and savings account**.
        * Checking account = **funds on lightning network**, savings account = **funds on blockchain**.
        * This seemed the most intuitive way, because most people understand those concepts
        and understand that transferring funds between savings<->checking account takes time and some money.
        We are open to other ideas.
    * Only 1 wallet can run at any given time, but we are going to add
    possibility of running multiple wallets at the same time.
* Pay lightning invoices by **QR code or lightning invoice**.
* You can **generate** lightning invoices.
* You can directly get testnet coins from the **faucet**.
* Show **outgoing and incomings payments** on the lightning network.
* Transfer money from *checking account*->*savings account* by **closing channels**.
* Transfer money from *savings account*->*checking account* by **creating channels to an existing peer or by QR code**.
* Displays lightning network information (**number of nodes, edges, etc.**).
* Show a **list of nodes** and filter nodes by pubkey or alias.
* **lnd-as-a-service**: runs [lnd](https://github.com/lightningnetwork/lnd) in the background (can run 24/7 if the wallet is open) and you can
access the instance even outside the app by using the https certs and macaroons which are displayed in **About** screen.
    * The battery usage seemed reasonable in our testings, but this is just a start and it will keep improving.
* Show **lnd logs** for debugging purposes.

&nbsp;

### Support
Our goal is to build the best lightning (and crypto) wallet ever made, so please let us
know if you run into issues, have any feedback or think something could be done better!

Right now, we are mostly concerned with bugs related to crashes and lost funds (even though it's only testnet).
Performance/bandwidth and storage usage is our other priority.

&nbsp;

### How is my data used ?
We understand that users are concerned about how their data is used.

We did our best to **preserve your privacy**. We use **neutrino**, a privacy-preserving Bitcoin
light client, to interact with the Bitcoin blockchain. We give you possibility to select the neutrino
btcd server that you want to point to (needs to be [roasbeef's fork](https://github.com/Roasbeef/btcd)).
We run a neutrino-compatible btcd instance which allows to sync very quickly, but if you want
there's also **faucet.lightning.community** node that you can point to. We didn't include it by default,
because we don't own/run those servers.

Some of the lightning wallets set your **alias** in lnd.conf because it serves as marketing tool
when looking through ln explorers, we decided against that for 2 reasons: it makes you a **target**
if a vulnerability was discovered in our app or lnd and we think that if we do a kick-ass job, we
won't need the marketing that comes from ln explorers.

We **don't** make network calls to any of our servers from the app (other than neutrino which you can change easily). We
host our static site on github, we have plans to eventually make a call to **rawtx.com/announcements.json**
URL to fetch important announcements to display, but if we do that we will give you ability to
disable making that call.

In addition to all of that, we've decided against including libraries like crashlytics,
libraries that helps us measure how the app is used or libraries that helps us control different
flags of the app, because we want to keep our dependencies to a minimum and including those 3rd party libraries
potentially exposes your data.

Those things make it difficult for us to **debug crashes**, etc, so we've given you a way to look through
the logs by yourself and you vet and share it with us if you want us to take a look at it.

We ask for **camera permissions** when you want to scan a QR code, if you don't want to use
that functionality, we will never ask for camera permissions.

&nbsp;

### Will it be open source ?
The major portion of this app is already open-source [lnd](https://github.com/lightningnetwork/lnd).
Lightning labs developers have done an amazing work on the lightning daemon, we've simply adapted
it for Android and put a UI built in react-native on top of it.

As for the app, not sure yet if it's going to be open-source, while we do want people to play around
with it and build stuff on top, we also want to avoid [situations](https://github.com/spesmilo/electrum-docs/blob/master/decompiling_guide.md)
like [this](https://www.reddit.com/r/Bitcoin/comments/8i5wck/decompiling_the_electrum_pro_stealware/).

Though no matter what we decide to do with the app, we will release the exact patches to lnd and a way
to prove that it's that specific library that's included in the mobile apps we ship.

If you're curious about the technology behind the app, here they are:
* interact with lightning network thanks to **lnd**,
* UI built in **react-native**,

We tried keeping 3rd party libraries to a minimum and these are our exact dependencies
in our current build.
```
    "babel-plugin-transform-remove-console": "^6.9.0",
    "flow-bin": "^0.68.0",
    "react": "^16.3.1",
    "react-mixin": "^4.0.0",
    "react-native": "^0.55.2",
    "react-native-button": "^2.3.0",
    "react-native-qrcode": "^0.2.6",
    "react-native-simple-radio-button": "^2.7.1",
    "react-navigation": "^1.5.9",
    "react-timer-mixin": "^0.13.3"
```

&nbsp;

### What's the meaning of rawtx ?
rawtx stands for rawtransaction which refers to a blockchain transaction, lightning network is essentially
a network of nodes messaging rawtransactions to each other that will eventually be committed to the blockchain.

&nbsp;

### What's next ?
There's so much to do, our priorities for now are:
* Get iOS app (very soon!)
* Create wallet diagnostics
    * While we currently show lnd logs, that requires quite a bit of technical knowledge to understand, we
    want to make a system which can help users quickly diagnose what's wrong with a wallet on device.
* Litecoin integration.
* The never ending performance improvements, battery/bandwidth/storage reductions and bugfixes.