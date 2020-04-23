---
layout: post
title: "単回帰モデル"
description: Simple Regression, Univariate Regression
tags: 機械学習 回帰モデル
---

基本中の基本である単回帰モデルについて復習したいと思います。

## 単回帰モデル[^1]

　あるデータセット$\color{#2a8eba}{\mathcal{D}=\{(x_1, y_1), (x_2, y_2), ..., (x_N, y_N)\}}$が与えられ、$\color{#2a8eba}{y=ax+b}$のモデルで表現する場合、このモデルを**単回帰モデル**(simple regression)と言います。与えられたデータを最もよく表すモデルとはなんでしょうか？ここでは作成されたモデルとデータセットの誤差が小さい場合、そのモデルを良いモデルだと考えます。数式で表すと

$$
\color{#2a8eba}{
\begin{eqnarray*}
    \hat{y_1} = a x_1 + b \\
    ERR = y_1 - \hat y_1
\end{eqnarray*}
}
$$

上記のような誤差(err)の場合、符合に作用されてしまうので、微分しやすいように二乗誤差が用いられます。

$$
\color{#2a8eba}{
\begin{eqnarray*}
    ERR_i = (y_1 - y)^2
\end{eqnarray*}
}
$$

さらに得られたデータで和や平均を取ると、誤差は以下のようになります。

$$
\color{#2a8eba}{
\begin{eqnarray*}
    ERR_{total} = \sum_{i=1}^N (y_i - \hat y_i)^2 \\
    ERR_{mean} = \frac{1}{N}\sum_{i=1}^N (y_i - \hat y_i)^2
\end{eqnarray*}
}
$$

上記の$\color{#2a8eba}{ERR_{mean}}$を$\color{#2a8eba}{a}$と$\color{#2a8eba}{b}$で微分すると以下の式を得ます。

$$
\color{#2a8eba}{
\begin{eqnarray*}
    \frac{\partial ERR_{mean}}{\partial a} = -\frac{2}{N}\sum_{i=1}^N x_i (y_i - a x_i - b) \\
    \frac{\partial ERR_{mean}}{\partial b} = -\frac{2}{N}\sum_{i=1}^N (y_i - a x_i - b) \\
\end{eqnarray*}
}
$$

誤差を最小化するために、それらの微分値を0とすると、以下の方程式を解くことになる。

$$
\color{#2a8eba}{
\begin{eqnarray*}
\left
\{\begin{array}{l}
    -\frac{2}{N}\sum_{i=1}^N x_i (y_i - a x_i - b) = 0 \\
    -\frac{2}{N}\sum_{i=1}^N (y_i - a x_i - b) = 0 \\
\end{array}
\right.
\end{eqnarray*}
}
$$

これを解くと

$$
\color{#2a8eba}{
\begin{eqnarray}
\left
\{\begin{array}{l}
\label{eqn:leq}
    \sum_{i=1}^N x_i y_i - a \sum_{i=1}^N x_i x_i - b\sum_{i=1}^N x_i = 0 \\
    \sum_{i=1}^N y_i - a \sum_{i=1}^N x_i - b\sum_{i=1}^N 1 = 0
\end{array}
\right.
\end{eqnarray}
}
$$

ここで、それぞれのシグマ記号をただの記号だと考えると、$\color{#2a8eba}{a}$と$\color{#2a8eba}{b}$の連立方程式を解くことになります。手元でちょこちょこ計算すると

$$
\color{#2a8eba}{
\begin{eqnarray}
\left
\{\begin{array}{l}
\label{eqn:abcd}
    A = \sum_{i=1}^N x_i y_i \\
    B = \sum_{i=1}^N x_i x_i \\
    C = \sum_{i=1}^N x_i \\
    D = \sum_{i=1}^N y_i \\
\end{array}
\right.
\end{eqnarray}
}
$$

さらに当たり前ですが、

$$
\color{#2a8eba}{
\begin{eqnarray} \label{eqn:n}
    \sum_{i=1}^N 1 = N
\end{eqnarray}
}
$$

上記を展開すると、$\ref{eqn:abcd}$、$\ref{eqn:n}$を$\ref{eqn:leq}$に代入すると、

$$
\color{#2a8eba}{
\begin{eqnarray*}
    A - a B - b C = 0 \\
    D - a C - b N = 0
\end{eqnarray*}
}
$$

あとは普通のよくみる連立方程式なので

$$
\color{#2a8eba}{
\begin{eqnarray*}
a & = &\frac{CD-NA}{C^2-NB} = \frac{ \sum_{i=1}^N x_i \sum_{i=1}^N y_i - N  \sum_{i=1}^N x_i y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2} \\
b & = & \frac{AC-BD}{C^2 - NB} = \frac{\sum_{i=1}^N x_i \sum_{i=1}^N x_i y_i - \sum_{i=1}^N x_i^2  \sum_{i=1}^N y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2}
\end{eqnarray*}
}
$$

Rustのコードで表すと
```rust
    let x = vec![1, 2, 3, 4, 5];
    let y = vec![10, 11, 13, 15, 18];
```

----
[^1]: 基本的には線形モデルの説明から入る本が多いようです。

{% bibliography %}
