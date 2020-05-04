---
layout: post
title: "重回帰モデル"
description: Multiple Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

　前回のポストで単回帰モデルを復習したので、次に重回帰モデルしたいと思います。重回帰モデルは単回帰モデルのパラメータを増やしたモデルです。単回帰モデルが2変数の関係を表現したのに対して、重回帰モデルは3変数以上の関係を表現するモデルです。例えば、体重と身長だけでなく、年齢なども追加することができます。

## 重回帰モデルの解

　重回帰モデルは$\color{#1F618D}{y=\sum_{i=0}^M w_i x_i}$のように表現することができます。このとき入力は$M$次元だとしています。つまり$M$個のパラメータのパラメトリックモデルとなります。また、$\color{#1F618D}{x_i = 0}$としています。さらに表記を簡単にするため、$\color{#1F618D}{y=\mathbf{w}^T\mathbf{x}}$のようにベクトルで表現します。このとき、$\color{#1F618D}{\mathbf{x}}$は列ベクトルで$\color{#1F618D}{\mathbf{x} = \left(x_0, x_1, x_2, ..., x_M\right)^T}$と定義します。さらにデータ数の情報を付け加えると、データセットは$$\color{#1F618D}{\mathcal{D} = \{(x_{1,0}, x_{1,1}, x_{1,2}, .., x_{1,M}, y_1), \\ (x_{2,0}, x_{2,1}, x_{2,2}, ..., x_{2, M}, y_2), \\ ..., (x_{N,0}, x_{N,1}, x_{N,2}, ..., x_{N, M}, y_N) \}}$$のようになります。モデルは

$$
\color{#1F618D}{
\begin{eqnarray*}
    y_i = \mathbf{w}^T\mathbf{x}_i
\end{eqnarray*}
}
$$

となります。さらに$$\color{#1F618D}{\mathbf{X} =\{\mathbf{x_1}, \mathbf{x_2}, ..., \mathbf{x_N}\}}$$とすることでさらに表記が簡単になります。単回帰モデルと同様に最小二乗法を使いパラメータを推定します。二乗誤差は

$$
\color{#1F618D}{
\begin{eqnarray*}
    \varepsilon_{mean} =  \frac{1}{N}(\mathbf{y} - \mathbf{w}^T{X})^T(\mathbf{y} - \mathbf{w}^T{X})
\end{eqnarray*}
}
$$

上記の二条誤差を$\color{#1F618D}{\mathbf{w}}$で微分すると[^vector-partial]

$$
\color{#1F618D}{
\begin{eqnarray*}
    \frac{\partial\varepsilon_{mean}}{\partial \mathbf{w}} = \frac{1}{N}\sum_{i=1}^N (\mathbf{y}_i - \mathbf{wx}_i^T)
\end{eqnarray*}
}
$$








---
## 参考文献

* [機械学習](https://www.amazon.co.jp/dp/4254122187/)
* [ガウス過程と機械学習](https://www.amazon.co.jp/dp/B07QMMJJV8/)

----

[^vector-partial]: このベクトルの微分は$$\color{#1F618D}{\frac{\partial \mathbf{w}^T\mathbf{w}}{\partial \mathbf{w}}=2\}$$
