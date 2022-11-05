---
layout: post
title: "Ubuntu20.04でgcc-9からgcc-8に変更する方法"
description: Ubuntu Settings
tags: Ubuntu
---

ubuntuを20.04にアップデートするとデフォルトのGCCがバージョン9系になっています。そのため、cudaが使えず、GPU系のプログラムをdコンパイルすることができません。このポストではgcc-9.3からgccをダウングレードする方法を紹介します。

## 現在のgccとg++のバージョンを確認

```shell
$ gcc --version
$ g++ --version
```

## gccの各バージョンをインストール

```shell
$ sudo apt install build-essential
$ sudo apt -y install gcc-7 g++-7 gcc-8 g++-8 gcc-9 g++-9
```

## gccの各バージョンをgccの代替候補として登録

```shell
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 7
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 7
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 9
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 9
```

## gccコマンドとして代替のバージョンを登録

```shell
$ sudo update-alternatives --config gcc
```

先程登録したgccの代替バージョンのどのバージョンを指定するか、問われるんで好みのバージョンを指定。同様に`g++`も登録する。

```shell
$ sudo update-alternatives --config g++
```

## バージョンが変更されているかを再確認

```shell
$ gcc --version
$ g++ --version
```


## 参照

* [How to switch between multiple GCC and G++ compiler versions on Ubuntu 20.04 LTS Focal Fossa](https://linuxconfig.org/how-to-switch-between-multiple-gcc-and-g-compiler-versions-on-ubuntu-20-04-lts-focal-fossa)
