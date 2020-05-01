---
layout: post
title: "単回帰モデル(WIP)"
description: Simple Regression, Univariate Regression
tags: 機械学習 回帰モデル
---
<span style="font-family:Sawarabi Gothic;">


　基本中の基本である単回帰モデルについて復習したいと思います。単回帰モデルは2変数の関係を特定するためのモデルです。例えば、身長を体重の関係を説明したい時などに適応できます。

## 単回帰モデルの解
　あるデータセット$\color{#1F618D}{\mathcal{D}=\{(x_1, y_1), (x_2, y_2), ..., (x_N, y_N)\}}$が与えられ、$\color{#1F618D}{y=ax+b}$のモデルで表現する場合、このモデルを<span style="color:#148F77; font-family:Sawarabi Mincho;">単回帰モデル(simple regression)</span>[^simple-regression]と言います。数式からわかるように、二つのパラメータ$\color{#1F618D}{a}$と$\color{#1F618D}{b}$で$\color{#1F618D}{y}$を表現します。このパラメータを<span style="color:#148F77; font-family:Sawarabi Mincho;">回帰係数(regression coefficient)</span>と呼びます。パラメータを用いるモデルのことを<span style="color:#148F77; font-family:Sawarabi Mincho;">パラメトリックモデル(parametric model)</span>といいます。ところで、与えられたデータを最もよく表すモデルとはなんでしょうか？ここでは作成されたモデルとデータセットの誤差(残差: residual)を最小にするモデルを良いモデルだと考えます。そのために二乗誤差を最小にするパラメータを求めます(<span style="color:#148F77; font-family:Sawarabi Mincho;">最小二乗法(least-squares method)</span>)[^least-squares-method] [^convex] [^gauss-markov]。数式で表すと

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
    \varepsilon_i = (y_i - \hat{y}_i)^2
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
\sigma_{xx} & = & \frac{1}{N} \sum_{i=1}^N x_i^2 - \left(\frac{1}{N}\sum_{i=1}^N x_i \right)^2  \\
\sigma_{xy} & = & \frac{1}{N}\sum_{i=1}^N x_i y_i - \frac{1}{N}\sum_{i=1}^N x_i \frac{1}{N}\sum_{i=1}^N y_i
\end{eqnarray*}
}
$$

さて上記を解の方程式に代入すると

$$
\color{#1F618D}{
\begin{eqnarray*}
a & = & \frac{\sum_{i=1}^N x_i \sum_{i=1}^N y_i - N  \sum_{i=1}^N x_i y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2} = \frac{\sigma_{xy}}{\sigma_{xx}}\\
b & = & \frac{\sum_{i=1}^N x_i \sum_{i=1}^N x_i y_i - \sum_{i=1}^N x_i^2  \sum_{i=1}^N y_i}{(\sum_{i=1}^N x_i)^2 - N  \sum_{i=1}^N x_i^2} = \frac{\sum_{i=1}^N x_i \sum_{i=1}^N x_i y_i - \sum_{i=1}^N x_i^2  \sum_{i=1}^N y_i}{\sigma_{xx}}\\
\end{eqnarray*}
}
$$

ここで、$\color{#1F618D}{x'=\frac{x}{\sigma_{xx}}}$とすると、

$$
\color{#1F618D}{
\begin{eqnarray*}
a & = & \sigma_{x'y}\\
b & = & \sum_{i=1}^N x_i' \sum_{i=1}^N x_i' y_i - \sum_{i=1}^N x_i'^2  \sum_{i=1}^N y_i
\end{eqnarray*}
}
$$

になります。最小二乗法を思い出すと、$\color{#1F618D}{a}$と$\color{#1F618D}{b}$を最小化することは$\color{#1F618D}{\sigma_{x'y}}$を最小化していることにつながります。

## 推定量の性質

　推定された$\color{#1F618D}{a}$と$\color{#1F618D}{b}$は<span style="color:#148F77; font-family:Sawarabi Mincho;">不偏推定量(unbiased estimator)</span>となります。実際に不偏推定量になっているかは、以下のように計算で確かめることができます。[^unbiased-estimator]

$$
\color{#1F618D}{
\begin{eqnarray*}
a & = & \frac{1}{N}\frac{\sum_{i=1}^N x_i y_i  - \frac{1}{N}\sum_{i=1}^N x_i \sum_{i=1}^N y_i}{\sigma_{xx}} \\
  & = & \frac{1}{N}\sum_{i=1}^N\left(\frac{ x_i - \mathbf{E}_{sample}[x]}{\sigma_{xx}} \right) y_i = \frac{1}{N}\sum_{i=1}^N c_i y_i\\
\end{eqnarray*}
}
$$

上記の$\color{#1F618D}{a}$にたいして母平均をとります。

$$
\color{#1F618D}{
\begin{eqnarray}
\label{eqn:mean}
\mathbf{E}[a] & = & \mathbf{E}[\frac{1}{N}\sum_{i=1}^N c_i y_i] = \frac{1}{N}\sum_{i=1}^N c_i \mathbf{E}[y_i] \\
 & = & \frac{1}{N}\sum_{i=1}^N c_i (\bar{a} x_i + \bar{b}) = \frac{\bar{a}}{N} \sum_{i=1}^N c_i  x_i + \frac{\bar{b}}{N} \sum_{i=1}^N c_i\\
\end{eqnarray}
}
$$

上記の$\color{#1F618D}{\bar{a}}$と$\color{#1F618D}{\bar{b}}$は真の$\color{#1F618D}{a}$と$\color{#1F618D}{b}$となります。[^ture-parameter] $\color{#1F618D}{c_i}$に関わる部分は以下のようになります。

$$
\color{#1F618D}{
\begin{eqnarray*}
\frac{1}{N}\sum_{i=1}^N c_i x_i & = & \frac{1}{N}\sum_{i=1}^N\left(\frac{ x_i - \mathbf{E}_{sample}[x]}{\sigma_{xx}} \right) x_i = \frac{1}{\sigma_{xx}} \sum_{i=1}^N \left( \frac{x_i x_i}{N} - x_i \frac{\mathbf{E}_{sample}[x]}{N} \right)\\
& = & \frac{1}{\sigma_{xx}} \{ \sum_{i=1}^N \left(\frac{x_i x_i}{N}\right) -  \mathbf{E}_{sample}[x]\sum_{i=1}^N \frac{x_i}{N} \} \\
& = & \frac{1}{\sigma_{xx}} \{ \mathbf{E}_{sample}[x^2] -  (\mathbf{E}_{sample}[x])^2 \} = 1\\

\frac{1}{N}\sum_{i=1}^N c_i & = & \frac{1}{N}\sum_{i=1}^N\left(\frac{ x_i - \mathbf{E}_{sample}[x]}{\sigma_{xx}} \right) = \frac{1}{\sigma_{xx}} \sum_{i=1}^N \left( \frac{x_i}{N} - \mathbf{E}_{sample}[x] \right) \\
 & = & \frac{1}{N\sigma_{xx}}\left( \mathbf{E}_{sample}[x] - \mathbf{E}_{sample}[x]\right) = 0
\end{eqnarray*}
}
$$

となります。これを$\ref{eqn:mean}$に代入すると、

$$
\color{#1F618D}{
\begin{eqnarray*}
\mathbf{E}[a] & = & \frac{\bar{a}}{N} \sum_{i=1}^N c_i  x_i + \frac{\bar{b}}{N} \sum_{i=1}^N c_i = \bar{a} \\
\end{eqnarray*}
}
$$

よって、$\color{#1F618D}{a}$は母平均とサンプル平均が一致するので$\color{#1F618D}{a}$は不偏推定量となります。$\color{#1F618D}{b}$についても同様に計算することが出来ます。

$$
\color{#1F618D}{
\begin{eqnarray*}
b & = & \frac{1}{N}\frac{\sum_{i=1}^N x_i y_i  - \frac{1}{N}\sum_{i=1}^N x_i \sum_{i=1}^N y_i}{\sigma_{xx}} \\
  & = & \frac{1}{N}\sum_{i=1}^N\left(\frac{ x_i - \mathbf{E}_{sample}[x]}{\sigma_{xx}} \right) y_i = \frac{1}{N}\sum_{i=1}^N c_i y_i\\
\end{eqnarray*}
}
$$

\color{#1F618D}{b}$についても同様に母平均をとり不偏推定量となることを確認することが出来ます。

$$
\color{#1F618D}{
\begin{eqnarray*}
\mathbf{E}[b] & = & \mathbf{E}[\sum_{i=1}^N c_i y_i] = \sum_{i=1}^N c_i \mathbf{E}[y_i]
\end{eqnarray*}
}
$$




## 外れ値(outlier)に対して

## 単回帰モデルの統計検定

　こんな単純なモデルでも推定された$\color{#1F618D}{a}$と$\color{#1F618D}{b}$がどれほど妥当か？つまり有意かを検定したいと思うのは人の常です。


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
* [多変量解析入門――線形から非線形へ](https://www.amazon.co.jp/dp/4000056530)
* [データはここから拝借](https://www.e-stat.go.jp/)

----
[^simple-regression]: データが$\color{#1F618D}{\mathcal{D}=\{(x_1, y_1), (x_1, y_2), ..., (x_1, y_N)\}}$の場合、つまり$\color{#1F618D}{x}$が全て同じ値を取るときは、うまくフィッティングできません。
[^least-squares-method]: 他にも<span style="color:#148F77; font-family:Sawarabi Mincho;">最小絶対値法(Least-absolute-value(LAV))</span>などがあります。これは$\color{#1F618D}{\sum \|\varepsilon_i\|}$を最小化します。最小絶対値法は、数学的に扱うのは厄介（計算機を使って解く）ですが外れ値に強い性質(Robust Regression)を持ちます。
[^convex]: 最小二乗法はコスト関数が凸関数なので、必ず最小値を持ちます。
[^gauss-markov]: 推定された$\color{#1F618D}{a}$と$\color{#1F618D}{b}$は<span style="color:#148F77; font-family:Sawarabi Mincho;">最良線形不偏推定量(Best Linear Uniased Estimator: BLUE)</span>です。そのままですが、最小二乗法は不偏推定量のうち最良のもの推定することができます。これはガウス・マルコフの定理として知られいます。証明は『統計学入門』(東京大学出版会)のp275などを参考にしてください。
[^unbiased-estimator]: 標本平均と母平均が一致することを示します。
[^ture-parameter]: ノイズが正規分布に従うとすると、母平均をとるとノイズが消されます。パラメータは真のパラメータと一致します。
