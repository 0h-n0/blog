---
layout: post
title: "確率で登場するいくつかの不等式"
description: Linear Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

　前回のポストで単回帰モデルを復習したので、次に重回帰モデルしたいと思います。重回帰モデルは単回帰モデルのパラメータを増やしたモデルです。単回帰モデルが2変数の関係を表現したのに対して、重回帰モデルは3変数以上の関係を表現するモデルです。例えば、体重と身長だけでなく、年齢なども追加することができます。

## $チェビシェフの不等式(Chebyshev's\ \ inequality)$
確率変数Xに対して、平均$\mu = E(X)$、分散$\sigma^2 = V(X)$が存在するとき、定数$c>0$に対して

$$
P(|X - \mu| \geq c) \leq \frac{\sigma^2}{c^2}
$$

が成り立つ。

## $マルコフの不等式(Markov's\ \ inequality)$
確率変数Xと定数$c>0$に対して

$$
P(|X|\geq c) \leq \frac{E(|X|)}{c}
$$

が成り立つ。

## $イェンセンの不等式(Jensen's\ \ inequality)$
凸関数$g(x)$と確率変数Xに対して

$$
E(|X|) < \infty,\ \ E(Y^2) < \infty
$$

ならば


$$
g\left( E(X) \right) \leq E[g(x)]

$$

が成り立つ。


## $コーシー・シュヴルツの不等式(Cauchy-Schwarz's\ \ inequality)$
確率変数$X,Y$に対して

$$
E(X^2) < \infty,\ \ E(Y^2) < \infty
$$

ならば

$$
E(|XY|) \leq \left\{E(X^2)E(Y^2)\right\}^{1/2}
$$

が成り立つ。

## $ヘルダーの不等式(H\ddot{o}lder's\ \ inequality)$

確率変数$X, Y$と$p>0,q>0,\frac{1}{p}+\frac{1}{q}=1$となる定数に対して、$E(\|X\|^P)<\infty,E(\|Y\|^P)<\infty$ならば

$$
E(|XY|) \leq \{E(|X|^p\}^{1/p}\{E(|Y|^q\}^{1/q}
$$

が成り立つ。

## $ミンコフスキーの不等式(Minkowski's\ \ inequality)$
確率変数$X, Y$と$r\geq1$なる定数に対して、$E(\|X\|^r)< \infty, E(\|Y\|^r)< \infty$ならば

$$
{E(|X+Y|^r)}^{1/r} \leq E(|X|^r)^{1/r} + E(|Y|^r)^{1/r}
$$

が成り立つ

## $リヤプノフの不等式(Lyapunov's\ \ inequality)$
確率変数$X$と定数$0<s<r$に対して$E(|X|^r) < \infty$ならば

$$
\{E(|X|^s\}^{1/s} \leq \{E(|X|^r\}^{1/r}
$$

が成り立つ。


---
## 参考文献

* [機械学習](https://www.amazon.co.jp/dp/4254122187/)
* [ガウス過程と機械学習](https://www.amazon.co.jp/dp/B07QMMJJV8/)

----
[^simple-regression]: データが$\color{#1F618D}{\mathcal{D}=\{(x_1, y_1), (x_1, y_2), ..., (x_1, y_N)\}}$の場合、つまり$\color{#1F618D}{x}$が全て同じ値を取るときは、うまくフィッティングできません。
