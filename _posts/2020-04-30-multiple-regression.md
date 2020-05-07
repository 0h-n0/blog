---
layout: post
title: "重回帰モデル"
description: Multiple Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

　前回のポストで単回帰モデルを復習したので、次に重回帰モデルしたいと思います。重回帰モデルは単回帰モデルのパラメータを増やしたモデルです。単回帰モデルが2変数の関係を表現したのに対して、重回帰モデルは3変数以上の関係を表現するモデルです。例えば、体重と身長だけでなく、年齢なども追加することができます。

## 重回帰モデルの解

　重回帰モデルは$\color{#1F618D}{y=\sum_{i=0}^M w_i x_i}$のように表現することができます。このとき入力は$M$次元だとしています。つまり$M$個のパラメータのパラメトリックモデルとなります。また、$\color{#1F618D}{x_i = 0}$としています。さらに表記を簡単にするため、$\color{#1F618D}{y=\mathbf{w}^T\mathbf{x}}$のようにベクトルで表現します。このとき、$\color{#1F618D}{\mathbf{x}}$は列ベクトルで$\color{#1F618D}{\mathbf{x} = \left(x_0, x_1, x_2, ..., x_M\right)^T}$と定義します。さらにデータ数の情報を付け加えると、データセットは$$\color{#1F618D}{\mathcal{D} = \{(\mathbf{x}_1, y_1), \\ (\mathbf{x}_2, y_2), \\ ..., (\mathbf{x}_N, y_N) \}}$$のようになります。モデルは

$$
\color{#1F618D}{
\begin{eqnarray*}
    y_i = \mathbf{x}_i^T\mathbf{w}
\end{eqnarray*}
}
$$

となります。さらに$$\color{#1F618D}{\mathbf{X} =\{\mathbf{x_1}, \mathbf{x_2}, ..., \mathbf{x_N}\}}$$とすることでさらに表記が簡単になります。単回帰モデルと同様に最小二乗法を使いパラメータを推定します。二乗誤差は

$$
\color{#1F618D}{
\begin{eqnarray*}
    \varepsilon_{mean} =  \frac{1}{N}(\mathbf{y} - X\mathbf{w})^T(\mathbf{y} - X\mathbf{w})
\end{eqnarray*}
}
$$

上記の二乗誤差を$\color{#1F618D}{\mathbf{w}}$で微分し[^vector-partial]、その値が0であるから

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
    && X^T (\mathbf{y} - X\mathbf{w})=0 \\
    &\rightarrow& X^T\mathbf{y} - X^TX\mathbf{w} = 0 \\
    &\rightarrow& X^TX\mathbf{w} = X^T\mathbf{y} \\
   &\rightarrow& \mathbf{w} = (X^TX)^{-1}X^T\mathbf{y} \\
\end{eqnarray*}
}
$$

よって、解は

$$
\color{#1F618D}{
\begin{eqnarray*}
   \mathbf{w} = (X^TX)^{-1}X^T\mathbf{y}
\end{eqnarray*}
}
$$

となります。ここで、$\color{#1F618D}{(X^TX)^{-1}}$で逆行列を計算しています。もちろん$\color{#1F618D}{(X^TX)^{-1}}$の逆行列が存在しない場合は解が存在しません。行列$\color{#1F618D}{X}$の次元は$\color{#1F618D}{(N,M)}$の行列となります。すると$\color{#1F618D}{X^TX}$の次元は$\color{#1F618D}{(M,M)}$となるため、$\color{#1F618D}{M}$が大きいほど(パラメータの数が大きいほど)逆行列が存在しにくくなります。それを回避するための正則化となります。([リッジ回帰モデル]({{ '2020/05/02/ridge-regression.html' | relative_url }}))



---
## 参考文献

* [機械学習](https://www.amazon.co.jp/dp/4254122187/)
* [ガウス過程と機械学習](https://www.amazon.co.jp/dp/B07QMMJJV8/)

----

[^vector-partial]: このベクトルの微分は$$\color{#1F618D}{\frac{\partial \mathbf{w}^T\mathbf{w}}{\partial \mathbf{w}}=2\mathbf{w}}$$
