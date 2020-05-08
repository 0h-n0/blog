---
layout: post
title: "リッジ回帰モデル"
description: Linear Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

　重回帰モデルでは、パラメータ数が増えると逆行列の正方行列の次元がそれに比例して増えるので逆行列の存在しにくくなってしまいます。そのために正則化項を加えることでモデルを安定化させます。それがリッジ回帰モデルです。

## リッジ回帰モデルの解

リッジ回帰の二乗誤差は、重回帰モデルの二乗誤差に$\color{#1F618D}{\lambda\mathbf{w}^T\mathbf{w}}$を付け加えてモデルになります。この項を導入することでそれぞれのパラメータが大きくなり過ぎることを抑制します。

$$
\color{#1F618D}{
\begin{eqnarray*}
    \varepsilon_{mean} =  \frac{1}{N}(\mathbf{y} - X\mathbf{w})^T(\mathbf{y} - X\mathbf{w}) + \lambda\mathbf{w}^T\mathbf{w}
\end{eqnarray*}
}
$$

$$
\color{#1F618D}{
\begin{eqnarray*}
    \frac{\partial\varepsilon_{mean}}{\partial \mathbf{w}} = -\frac{2}{N} X^T (\mathbf{y} - X\mathbf{w})=0
\end{eqnarray*}
}
$$

となります。上記の方程式は

$$
\color{#1F618D}{
\begin{eqnarray*}
    && X^T (\mathbf{y} - X\mathbf{w}) + \lambda \mathbf{w}=0 \\
    &\rightarrow& X^T\mathbf{y} - (X^TX + \lambda \mathbf{E})\mathbf{w} = 0 \\
    &\rightarrow& (X^TX + \lambda \mathbf{E})\mathbf{w} = X^T\mathbf{y} \\
   &\rightarrow& \mathbf{w} = (X^TX + \lambda \mathbf{E})^{-1}X^T\mathbf{y} \\
\end{eqnarray*}
}
$$

となり、解は

$$
\color{#1F618D}{
\begin{eqnarray*}
   \mathbf{w} = (X^TX + \lambda \mathbf{E})^{-1}X^T\mathbf{y}
\end{eqnarray*}
}
$$

となります。よって対角成分に$\color{#1F618D}{\lambda}$が足されているので逆行列が存在することが担保されます。ただし理論の上では対角成分に微少量を加算すれば逆行列が存在しますが、**$\color{#1F618D}{\lambda}$が小さいときなどは気をつけないと桁落ちしてしまいます**。そのため、せっかく正則化項を加えたのに、解が求まらなかったりします。そのような場合は、最急降下法wを用いて$\color{#1F618D}{\mathbf{w}}$を逐次的にアップデートしていく方法などがあります。この場合、学習係数などを調整する必要などがあり、最小二乗法と違い最良不偏推定量を得られないことに注意しなければなりません。また$\color{#1F618D}{(X^TX)^{-1}}$の逆行列の計算を、ムーア・ペンローズの逆行列として計算することが出来ます。ムーア・ペンローズの逆行列を用いたときの`リスク`などはまだ勉強できていないので明らかにしていきたいです。



---
## 参考文献

* [多変量解析入門――線形から非線形へ](https://amzn.to/3cb0m0D)
* [ガウス過程と機械学習 (機械学習プロフェッショナルシリーズ)](https://amzn.to/2YHtlp3)
* [【解説】 一般逆行列](https://www.slideshare.net/wosugi/ss-79624897)

----
[^simple-regression]: データが$\color{#1F618D}{\mathcal{D}=\{(x_1, y_1), (x_1, y_2), ..., (x_1, y_N)\}}$の場合、つまり$\color{#1F618D}{x}$が全て同じ値を取るときは、うまくフィッティングできません。
