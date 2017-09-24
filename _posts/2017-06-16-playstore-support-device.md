---
layout: post
title: The solution when the number of devices supported by the Play Store is 0
description: "The solution when the number of devices supported by the Play Store is 0"
tags: [android, playstore]
---

There are sometimes 0 devices that are supported when registering the Play Store, but this is usually caused by the build settings or the manifest file.

One of these was attributed to camera authority.

To fix this, add the `android: required = "false"` attribute as shown below.
