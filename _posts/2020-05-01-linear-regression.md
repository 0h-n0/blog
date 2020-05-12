---
layout: post
title: "線形回帰モデル"
description: Linear Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

　重回帰モデルをさらに一般化させたモデルが線形回帰モデルとなります。

## 線形回帰モデルの解

　線形回帰モデルは$\color{#1F618D}{y=\sum_i^M \phi_i(x_i)w_i}$とし、ある関数$\color{#1F618D}{\phi_i(x)}$を導入してモデルを作成します。ここで注意するべきことは関数$\phi_i(x)$は非線形関数でも良いとところです。[^non-linear]関数$\color{#1F618D}{\phi_i(x)}$を導入しても、$\color{#1F618D}{\phi_i=\phi_i(x_i)}$とし、$\color{#1F618D}{\pmb{\phi} = \left(\phi_0, \phi_1, \phi_2, ..., \phi_M\right)^T}$となり、$\color{#1F618D}{\Phi = \left(\pmb{\phi}_1, \pmb{\phi}_2, ..., \pmb{\phi}_M\right)^T}$とすることで、重回帰モデルの$\color{#1F618D}{X}$を$\color{#1F618D}{\Phi}$に置き換えることができます。


$$
\color{#1F618D}{
\begin{eqnarray*}
    \varepsilon = (\mathbf{y} - \Phi\mathbf{w})^T(\mathbf{y} - \Phi\mathbf{w})
\end{eqnarray*}
}
$$

$$
\color{#1F618D}{
\begin{eqnarray*}
    \frac{\partial\varepsilon}{\partial \mathbf{w}} = - 2 \Phi^T (\mathbf{y} - \Phi\mathbf{w})=0
\end{eqnarray*}
}
$$

となります。上記の方程式は

$$
\color{#1F618D}{
\begin{eqnarray*}
    && -\Phi^T (\mathbf{y} - \Phi\mathbf{w})=0 \\
    &\rightarrow& -\Phi^T\mathbf{y} + \Phi^T\Phi\mathbf{w} = 0 \\
    &\rightarrow& \Phi^T\Phi\mathbf{w} = \Phi^T\mathbf{y} \\
   &\rightarrow& \mathbf{w} = (\Phi^T\Phi)^{-1}\Phi^T\mathbf{y} \\
\end{eqnarray*}
}
$$

となり、解は

$$
\color{#1F618D}{
\begin{eqnarray*}
   \mathbf{w} = (\Phi^T\Phi)^{-1}\Phi^T\mathbf{y}
\end{eqnarray*}
}
$$

となります。



---
## 参考文献

* [機械学習](https://www.amazon.co.jp/dp/4254122187/)
* [ガウス過程と機械学習](https://www.amazon.co.jp/dp/B07QMMJJV8/)

----
[^non-linear]: あくまで線形性は$\color{#1F618D}{w}$を変数として見たときの関数$\color{#1F618D}{y(w)}$に対してです。つまり、$\color{#1F618D}{y(w_1 + w_2)=y(w_1) + y(w_2)}$と$\color{#1F618D}{y(aw)=ay(w)}$とが成り立つ必要があるためです
