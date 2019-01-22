---
layout: post
title:  "Faster sync with lnd 0.5.1"
date:   2018-12-04 09:01:01 -0700
categories: rawtx update
---
As some of you know, the latest version of the wallet had a problem with
syncing. In that, it would first sync block headers, then sync filter headers.
The problem is that these would be sequential and the progress bar would only
reflect the block headers. So, many users were stuck at 99% while filter headers
synced, leading to confusion.

lnd 0.5.1 eliminates this by syncing both in parallel. So you will see faster
sync times and more accurate progress percentage (%).

In addition to that, we've fixed a bug when the user tries connecting to a brand
new peer, it would somehow fail even if the connection was successful. This is
due to how we detect if we are connected, which included querying the graph
info, the issue is that new peers are parts of peers list, not necessarily the
graph. This has been fixed as well.

We've also included the blockchain transactions as part of the UI.

There are many more bugfixes and stability improvements underneath.

Please give it a try and feel free to file bugs/feature requests on
[github](https://github.com/rawtxapp/rawtxapp).