---
layout: post
title: "単回帰モデル"
---

<link href="https://fonts.googleapis.com/css?family=Sawarabi+Mincho" rel="stylesheet">

基本中の基本である単回帰モデルについて復習したいと思います。

## 単回帰モデル

　あるデータセット$\color{#2a8eba}{\mathcal{D}=\{(x_1, y_1), (x_2, y_2), ..., (x_N, y_N)\}}$が与えられ、$y=ax+b$のモデルで表現する場合、このモデルを**単回帰モデル**(simple regression)と言います。与えられたデータを最もよく表すモデルとはなんでしょうか？ここでは作成されたモデルとデータセットの誤差が小さい場合、そのモデルを良いモデルだと考えます。数式で表すと

$$
\color{#59bl8c}{
\begin{eqnarray*}
    \hat{y_1} = a x_1 + b \\
    ERR = y_1 - \hat y_1
\end{eqnarray*}
}
$$

$$
\mycolor{
\begin{eqnarray*}
    \hat{y_1} = a x_1 + b \\
    ERR = y_1 - \hat y_1
\end{eqnarray*}
}
$$

$$
\color{#2a8eba}{
\begin{eqnarray*}
    \hat{y_1} = a x_1 + b \\
    ERR = y_1 - \hat y_1
\end{eqnarray*}
}
$$

$$
\color{#92BDCA}{
\begin{eqnarray*}
    \hat{y_1} = a x_1 + b \\
    ERR = y_1 - \hat y_1
\end{eqnarray*}
}

$$
