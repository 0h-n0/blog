---
layout: post
title: "線形回帰モデル"
description: Linear Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

　前回のポストで単回帰モデルを復習したので、次に重回帰モデルしたいと思います。重回帰モデルは単回帰モデルのパラメータを増やしたモデルです。単回帰モデルが2変数の関係を表現したのに対して、重回帰モデルは3変数以上の関係を表現するモデルです。例えば、体重と身長だけでなく、年齢なども追加することができます。

## 線形回帰モデルの解


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

となります。よって対角成分に$\color{#1F618D}{\lambda}$が足されているので逆行列が存在することが担保されます。



---
## 参考文献

* [機械学習](https://www.amazon.co.jp/dp/4254122187/)
* [ガウス過程と機械学習](https://www.amazon.co.jp/dp/B07QMMJJV8/)

----
[^simple-regression]: データが$\color{#1F618D}{\mathcal{D}=\{(x_1, y_1), (x_1, y_2), ..., (x_1, y_N)\}}$の場合、つまり$\color{#1F618D}{x}$が全て同じ値を取るときは、うまくフィッティングできません。
