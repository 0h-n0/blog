---
layout: post
title: "Ubuntuでwifi6アダプターのセットアップ"
description: Simple Regression, Univariate Regression
tags: Wifi Setup
---

　Wifiアダプターーを買ってきて公式ドライバーが

```shell
$ sudo apt install linux-oem-osp1 linux-firmware
```

そもそもハードウェアを認識しているかの確認.

```shell
$ sudo lshw -class network -showrt
```
## wifi6に関して。
