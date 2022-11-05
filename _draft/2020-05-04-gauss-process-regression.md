---
layout: post
title: "ガウス過程回帰モデル"
description: Linear Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

　前回のポストで単回帰モデルを復習したので、次に重回帰モデルしたいと思います。重回帰モデルは単回帰モデルのパラメータを増やしたモデルです。単回帰モデルが2変数の関係を表現したのに対して、重回帰モデルは3変数以上の関係を表現するモデルです。例えば、体重と身長だけでなく、年齢なども追加することができます。

## リッジ回帰モデルの解

---
## 参考文献

* [機械学習](https://www.amazon.co.jp/dp/4254122187/)
* [ガウス過程と機械学習](https://www.amazon.co.jp/dp/B07QMMJJV8/)

----
[^simple-regression]: データが$\color{#1F618D}{\mathcal{D}=\{(x_1, y_1), (x_1, y_2), ..., (x_1, y_N)\}}$の場合、つまり$\color{#1F618D}{x}$が全て同じ値を取るときは、うまくフィッティングできません。
