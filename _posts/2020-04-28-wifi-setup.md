---
layout: post
title: "Ubuntuでwifi6アダプターのセットアップ"
description: Simple Regression, Univariate Regression
tags: Wifi Setup
---

　UbuntuOSに外付けのWifiカードを接続したいときがあります。そのまま刺しただけではハードウェアを認識せず、上手く行きません。以下のように必要なドライバー類をアップデートすれば正常に使用することができます。

```shell
$ sudo apt install linux-oem-osp1 linux-firmware
```

そもそもハードウェアを認識しているかの確認.

```shell
$ sudo lshw -class network -showrt
```
## wifi6に関して。
