---
layout: post
title: "単回帰モデル"
description: Simple Regression, Univariate Regression
tags: 機械学習 回帰モデル
---
<span style="font-family:Sawarabi Gothic;">


　基本中の基本である単回帰モデルについて復習したいと思います。単回帰モデルは2変数の関係を特定するためのモデルです。例えば、身長を体重の関係を説明したい時などに適応できます。

## 単回帰モデルの解
　あるデータセット$\color{#1F618D}{\mathcal{D}=\{(x_1, y_1), (x_2, y_2), ..., (x_N, y_N)\}}$が与えられ、$\color{#1F618D}{y=ax+b}$のモデルで表現する場合、このモデルを<span style="color:#148F77; font-family:Sawarabi Mincho;">単回帰モデル(simple regression)</span>と言います。数式からわかるように、二つのパラメータ$\color{#1F618D}{a}$と$\color{#1F618D}{b}$で$\color{#1F618D}{y}$を表現します。このパラメータを<span style="color:#148F77; font-family:Sawarabi Mincho;">回帰係数(regression coefficient)</span>と呼びます。パラメータを用いるモデルのことを<span style="color:#148F77; font-family:Sawarabi Mincho;">パラメトリックモデル(parametric model)</span>といいます。ところで、与えられたデータを最もよく表すモデルとはなんでしょうか？ここでは作成されたモデルとデータセットの誤差(残差: residual)を最小にするモデルを良いモデルだと考えます。そのために二乗誤差を最小にするパラメータを求めます(<span style="color:#148F77; font-family:Sawarabi Mincho;">最小二乗法(least-squares method)</span>)。数式で表すと

$$
\color{#1F618D}{
\begin{eqnarray*}
    \hat{y_1} = a x_1 + b \\
    \varepsilon_1 = y_1 - \hat y_1
\end{eqnarray*}
}
$$

上記のままでは、符合に作用されてしまうので、微分しやすいように二乗誤差が用いられます。

$$
\color{#1F618D}{
\begin{eqnarray*}
    \varepsilon_i = (y_1 - y)^2
\end{eqnarray*}
}
$$

さらに得られたデータで和や平均を取ると、誤差は以下のようになります。

$$
\color{#1F618D}{
\begin{eqnarray*}
    \varepsilon_{total} = \sum_{i=1}^N (y_i - \hat y_i)^2 \\
    \varepsilon_{mean} = \frac{1}{N}\sum_{i=1}^N (y_i - \hat y_i)^2
\end{eqnarray*}
}
$$

上記の$\color{#1F618D}{\varepsilon_{mean}}$を$\color{#1F618D}{a}$と$\color{#1F618D}{b}$で微分すると以下の式を得ます。

$$
\color{#1F618D}{
\begin{eqnarray*}
    \frac{\partial \varepsilon_{mean}}{\partial a} = -\frac{2}{N}\sum_{i=1}^N x_i (y_i - a x_i - b) \\
    \frac{\partial \varepsilon_{mean}}{\partial b} = -\frac{2}{N}\sum_{i=1}^N (y_i - a x_i - b) \\
\end{eqnarray*}
}
$$

誤差を最小化するために、それらの微分値を0とすると、以下の方程式を解くことになる。

$$
\color{#1F618D}{
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
\color{#1F618D}{
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

ここで、それぞれのシグマ記号をただの記号だと考えると、$\color{#1F618D}{a}$と$\color{#1F618D}{b}$の連立方程式を解くことになります。手元でちょこちょこ計算すると

$$
\color{#1F618D}{
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
\color{#1F618D}{
\begin{eqnarray} \label{eqn:n}
    \sum_{i=1}^N 1 = N
\end{eqnarray}
}
$$

上記を展開すると、$\ref{eqn:abcd}$、$\ref{eqn:n}$を$\ref{eqn:leq}$に代入すると、

$$
\color{#1F618D}{
\begin{eqnarray*}
    A - a B - b C = 0 \\
    D - a C - b N = 0
\end{eqnarray*}
}
$$

あとは普通によくみる連立方程式なので解くだけです。

$$
\color{#1F618D}{
\begin{eqnarray*}
a & = &\frac{CD-NA}{C^2-NB} = \frac{ \sum_{i=1}^N x_i \sum_{i=1}^N y_i - N  \sum_{i=1}^N x_i y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2} \\
b & = & \frac{AC-BD}{C^2 - NB} = \frac{\sum_{i=1}^N x_i \sum_{i=1}^N x_i y_i - \sum_{i=1}^N x_i^2  \sum_{i=1}^N y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2}
\end{eqnarray*}
}
$$

## 単回帰モデルの中心化と正規化

<span style="color:#148F77; font-family:Sawarabi Mincho;">中心化(zero-center)</span>と<span style="color:#148F77; font-family:Sawarabi Mincho;">正規化(normalize)</span>をすると単回帰モデルの解はどうなるでしょうか？式を整理するために、まず分散と共分散を以下のように定義します。

$$
\color{#1F618D}{
\begin{eqnarray*}
\sigma_x & = & \frac{1}{N} \sum_{i=1}^N x_i^2 - \left(\frac{1}{N}\sum_{i=1}^N x_i \right)^2  \\
\sigma_{xy} & = & \frac{1}{N}\sum_{i=1}^N x_i y_i - \frac{1}{N}\sum_{i=1}^N x_i \frac{1}{N}\sum_{i=1}^N y_i
\end{eqnarray*}
}
$$

さて上記を解の方程式に代入すると

$$
\color{#1F618D}{
\begin{eqnarray*}
a & = & \frac{\sum_{i=1}^N x_i \sum_{i=1}^N y_i - N  \sum_{i=1}^N x_i y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2} = \frac{\sigma_{xy}}{\sigma_x}\\
b & = & \frac{\sum_{i=1}^N x_i \sum_{i=1}^N x_i y_i - \sum_{i=1}^N x_i^2  \sum_{i=1}^N y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2} = \frac{\sum_{i=1}^N x_i\sigma_{xy}}{\sigma_x}\\
\end{eqnarray*}
}
$$

ここで、$x'=\frac{x}{\sigma_x}$とすると、

$$
\color{#1F618D}{
\begin{eqnarray*}
a & = & \sigma_{x'y}\\
b & = & \sum_{i=1}^N x_i\sigma_{x'y}\\
\end{eqnarray*}
}
$$

ここで最小二乗法を思い出すと、$\color{#1F618D}{a}$と$\color{#1F618D}{b}$を最小化することは$\color{#1F618D}{\sigma_{x'}}$を最小化していることにつながります。



## 単回帰モデルの統計検定

Rustのコードで表すと
```rust
let x = vec![1, 2, 3, 4, 5];
let y = vec![10, 11, 13, 15, 18];
```


---
## 参考文献

* [機械学習](https://www.amazon.co.jp/dp/4254122187/)
* [ガウス過程と機械学習](https://www.amazon.co.jp/dp/B07QMMJJV8/)
* [An Introduction to Statistical Learning](https://www.amazon.co.jp/dp/1461471370/)
* [データはここから拝借](https://www.e-stat.go.jp/)

----
[^1]: hoge
