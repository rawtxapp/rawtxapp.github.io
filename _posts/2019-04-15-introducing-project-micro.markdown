---
layout: post
title: "Make Micropayments Great Again, introducing project micro!"
date: 2019-04-15 09:00:00 -0700
categories: rawtx update
---

**tl;dr** lightning network on top of Bitcoin makes micropayments financially
viable, project micro attempts to reduce the cognitive load of micropayments.

{% include micro-sep.html %}

### What is project micro ?

It's an attempt at reducing cognitive load of micropayments by bringing
lightning apps (lapps) directly inside the rawtx bitcoin wallet and allowing payments
without prompting the user.

### Lightning app presentation

{% include img-cap.html url="/assets/micro-post/homescreen.png"
 description="Home screen: currently 3 apps are available in testnet, more coming soon" %}

### Opening an app

When the user decides to open an app, they click on the logo and the wallet gets
out of the way. These lightning apps are simply web apps wrapped in WebView.

{% include video.html
 urlmp4="/assets/micro-post/rawtx-chat-open.mp4"
 urlwebm="/assets/micro-post/rawtx-chat-open.webm"
 description="Opening rawtx chat app" %}

### Setting micro allowance

Each app can be given a maximum of 10k satoshis as "allowance", equivalent to
roughly 50 cents in US dollars at today's exchange rates. The user can cancel
the allowance at any time.

An allowance let's a lightning app charge up to 10k satoshis without prompting the user
(unless it's trying to charge for more than 100 satoshis at a time, which will
ask for user confirmation, attack vectors are discussed below).

{% include video.html
 urlmp4="/assets/micro-post/rawtx-chat-allowance.mp4"
 urlwebm="/assets/micro-post/rawtx-chat-allowance.webm"
 description="Setting and cancelling lapp's allowance" %}

### Using a lightning app

Once an allowance is set, the user can just enjoy the app and monitor payments
on the upper right corner. Here's an example of sending a single message using
[rawtx chat demo app](https://chat.rawtx.com) where each message costs 10 satoshis (roughy \$0.00051).

{% include video.html
 urlmp4="/assets/micro-post/rawtx-chat-using.mp4"
 urlwebm="/assets/micro-post/rawtx-chat-using.webm"
 description="Sending a single message worth 10 satoshis, pay attention to upper right corner" %}

### Supporting normal lapps and theming

Some lapps might not support micro yet or ever. Some might require payments
exceeding 100 satoshis regularly, micro supports those use cases as well.
It also allows theme customization in order to give as much immersive experience
to the lapp as possible. [yalls](https://testnet.yalls.org) is a popular
lightning app and here's how it's integrated.

{% include video.html
 urlmp4="/assets/micro-post/yalls-publish.mp4"
 urlwebm="/assets/micro-post/yalls-publish.webm"
 description="Publishing on yalls without using micro, as you can see the status bar is themed with yalls colors" %}

### Where can I get it?

You can try it on Android by downloading from the
[Play Store](https://play.google.com/store/apps/details?id=com.rtxwallet&utm_source=micropost&utm_campaign=homepage).
If you run into any problems please create an issue on [rawtx github repo](https://github.com/rawtxapp/rawtxapp).

### What kind of apps are available ?

Right now, it only includes testnet lightning apps. The
[demo rawtx chat app](https://chat.rawtx.com/) supports the micro project. In
addition to that, yalls and starblocks are also included.

Ideally, we would see clever uses of micropayments. While the lightning network
has a few cons such as having to have liquidity in Bitcoin, it also has very
powerful properties such as being able to pay anyone anywhere in the world
instantly, practically free and without disclosing any of your personal
information. Hopefully, those properties will help us create the next generation
of apps.

### I'm a lightning app developer

You can include your lapp in the wallet by adding it to [lapps repo](https://github.com/rawtxapp/lapps),
so feel free to send a pull request. Keep in mind, it's testnet only for now.

If you'd like to integrate micro, it's very simple, you can take a look at how
[it's integrated](https://github.com/rawtxapp/chat/blob/master/src/Micro.tsx) in the demo chat app.
It's essentially communicating with the wallet through postMessage as if it was
an iframe. There are a few messages, but at the very basic of it, you need to
acknowledge init micro message and you can then post your lightning invoices
through the same method.

If you're completely new to lightning app development, you can take a look
at the [frontend](https://github.com/rawtxapp/chat/tree/master) and
[the backend](https://github.com/rawtxapp/chat-backend) of the chat app to get
an idea. If you'd like to see a more detailed tutorial on how to build lightning
apps, feel free to create an issue on github.

### I'm a lightning wallet developer

It would be great to see micro and lightning apps integrated into all the
lightning wallets in a standardized way. For example, [BlueWallet](https://bluewallet.io/)
already has a lightning store, though it's not integrated as tightly. There's
also [webln](https://github.com/joule-labs/webln) as a potential
standard, though it's "to facilitate communication between apps and users' lightning nodes".
In the case of a mobile wallet, the lapp isn't talking directly to the node, it's
talking to the wallet. So, hopefully we can fix those differences.

If you'd like to see the code for micro inside the wallet, you can find it on
[github](https://github.com/rawtxapp/rawtxapp/tree/master/src/micro/Micro.js) alongside
it's tests and webview integrations.

### Attack vectors

Letting an external app talk to the wallet is dangerous due to many attack vectors
and highly motivated adversaries.

The wallet does a few things to protect itself from the app:

- the app never talks to the lightning node directly, instead, it goes through
  webview's communication method (postMessage),
- every message is sanitized to make sure it only includes letters, numbers and
  colons,
- apps are vetted before inclusion into the lapps repo and can be removed at any
  time,
- to avoid the case of an attacker taking over a lapp and redirecting the payments
  to their own node, each lapp specifies their pubkey in the lapps repo which is
  then checked before a payment,
- to avoid the case of a lapp charging all the allowance at once, micro payments
  are limited to those under 100 satoshis, anything above that will trigger a user
  confirmation dialog,
- there can only be 3 pending lightning payments at any given time, if a lapp
  tries to overwhelm the wallet, those requests will just be dropped,
- the lapp view displays current allowance and ongoing payments in the status
  bar, so the user is aware if the lapp tries to charge anything while in the
  background.

Those are the current protections, please file a bug on github if you notice
another possible attack.

### Why should I care about micropayments ?

That's a fair question. Micropayments have been tried in the past with mixed
results. While micropayment-only startups have mostly failed, micropayments
are widely present in games.

So far, 2 issues seem to have killed micropayment related projects:

- **Cost**, the margin on credit card payments is high and the lowest you could charge
  for an action is usually 0.01$. Lightning network solves this by introducing a
practically free and instant payment method which can send as low as 1 satoshi,
equivalent to roughly 0.00005$ today.
- **Cognitive load**, nickel-and-diming the user is annoying. For example, every
  time the user has to decide whether they should pay a cent to do an action, it
  decreases engagement.

  By setting aside an allowance and directly paying from it,
  the user doesn't have to think about payments. Micro lightning apps should be
  designed in such a way that the user can do many actions with the 10k satoshis
  allowance. Ideally, the 10k satoshis should last the whole month for an average
  user.

  Gamifying the experience would also help with user engagement, for example, if
  it costs 1 satoshi to post a comment, the user could get their satoshi back
  when someone upvotes them, etc. Keep in mind that micro refunds aren't implemented
  yet.

Micropayments have the potential of shifting internet monetization from ad-supported
into user-supported model and in the process creating higher quality content especially
for niche interests. It can also potentially create very different kinds of
applications.

### lnd 0.6

In addition to micro, the new version of wallet integrates the lnd 0.6 including
latest [neutrino changes](https://github.com/lightningnetwork/lnd/pull/2978).
Thanks to lightning labs and all lnd contributors, there are many improvements
and bugfixes which should make the wallet more stable. It also comes with a
static backup method which will be supported soon.

### What's next ?

One thing that would interesting is to allow native micro apps. On
Android, we can run a micro server in the background and allow another Android
lightning app to charge directly. Stay tuned!

### Questions/feedback

If you have any questions or feedback, feel free to create an issue on the
[github repo](https://github.com/rawtxapp/rawtxapp). If you want to discuss
privately, feel free to send a DM on [twitter](https://twitter.com/rawtxapp) or
email support@rawtx.com.
