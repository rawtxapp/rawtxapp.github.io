---
layout: post
title:  "Removed iOS version of the wallet"
date:   2019-01-21 09:01:01 -0700
categories: rawtx update
---
We removed the iOS version of the wallet from the app store due to
low benefit/cost ratio.

From a developer's perspective, iOS is more limited compared to Android.
For example, it allows only limited background processing, it doesn't allow
creating a separate process, gives only limited amount of memory, etc. While
these limitations help with battery and performance, it makes it more difficult
for developers.

These become problematic when running cross-compiled native code, chasing down 
and fixing every bug that comes up is proving to be very time consuming.

For these reasons, we've removed the iOS version to focus on the Android app.
You can still compile it for iOS from the source code.
