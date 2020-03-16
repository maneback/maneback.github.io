---
title: latex 速查表
date: 2020-02-18 17:15:00
tags: 
  - latex
categories: knowledge
description: latex 常用公式速查表
---

# 一、 集合操作

|                       |                   |
| --------------------- | ----------------- |
| `\mid`                | $A \mid B$        |
| 属于`\in`             | $a_i\in A$        |
| 不属于`\not\in`       | $a_i\not\in A$    |
| 包含于`\subset`       | $A \subset B$     |
| 子集 `\subseteq` | $A \subseteq B$ |
| 真包含于`\subsetneqq` | $A \subsetneqq B$ |
| 包含`\supset`         | $A \supset B$     |
| 真包含 `\supsetneqq`  | $A \supsetneqq B$ |
| 交集`\cap`            | $A \cap B$        |
| 并集 `\cup`           | $A \cup B$        |
|实数集合 `\mathbb{R}`|$\mathbb{R}$|
|空集`\emptyset`|$\emptyset$|
|||




# 二、数学运算

|                                                             |                                                        |
| ----------------------------------------------------------- | ------------------------------------------------------ |
| 分数 `\frac{}{}`                                            | $\frac{a}{b}$                                          |
| 花体字母`\mathbb{}`                                         | $\mathbb{ABC}$                                         |
| `\nabla`                                                    | $\nabla$                                               |
| `\partial`                                                  | $\partial x$                                           |
| 不等号`\neq`                                                | $x \neq 1$                                             |
| 角度``\sin\!\frac{\pi}{3}=\sin60^\{operatorname{\omicron}}` | $\sin\!\frac{\pi}{3}=\sin60^{\operatorname{\omicron}}$ |



# 三、逻辑

|                |             |
| -------------- | ----------- |
| 逻辑与 `land`  | $A \land B$ |
| 逻辑或 `lor`   | $A \lor B$  |
| 逻辑非 `\lnot` | $\lnot B$   |
| `\to`          | $p\to q$    |
|                |             |

# 三、上标、下标及积分等

|                                         |                               |
| --------------------------------------- | ----------------------------- |
| 前置上下标`{}_1^2X_3^4`                 | ${}_1^2X_3^4$                 |
| 向量 `\vec{x}`                          | $\vec{x}$                     |
| 无穷 `\infty`                           | $\infty$                      |
| 求和 `\sum_{k=1}^n x_k`                 | $\sum_{k=1}^n x_k$            |
| 求积`\prod_{k=1}^n x_k`                 | $\prod_{k=1}^n x_k$           |
| 极限 `\lim_{n \to \infty} x_n`          | $\lim_{n \to \infty} x_n$     |
| 积分 `\int_{-N}^{N} e^x`                | $\int_{-N}^{N} e^x$           |
| 双重积分 `\iint_{D}^{W}\,dx\,dy`        | $\iint_{D}^{W}xy\,dxdy$       |
| 三重积分`\iiint_{E}^{V}\,xyz\,dxdydz`   | $\iiint_{E}^{V}\,xyz\,dxdydz$ |
| 曲线曲面积分`\oint_{C}x^3\,dx+4y^2\,dy` | $\oint_{C}x^3\,dx+4y^2\,dy$   |

# 四、分数、矩阵和多行列式

|                                              |                                          |
| -------------------------------------------- | ---------------------------------------- |
| 分数`\frac{1}{2}=0.5`                        | $\frac{1}{2}=0.5$                        |
| 小分数`\tfrac{1}{2}=0.5`                     | $\tfrac{1}{2}=0.5$                       |
| 二项式系数`\dbinom{n}{r}=C_n^r`              | $\dbinom{n}{r}=C_n^r$                    |
| 矩阵`\begin{matrix}x&y \\ z&v\end{matrix}`   | $$\begin{matrix}x&y \\ z&v\end{matrix}$$ |
| 矩阵`\begin{vmatrix}x&y \\ z&v\end{vmatrix}` | $\begin{vmatrix}x&y \\ z&v\end{vmatrix}$ |
| 矩阵`\begin{Vmatrix}x&y \\ z&v\end{Vmatrix}` | $\begin{Vmatrix}x&y \\ z&v\end{Vmatrix}$ |
|                                              |                                          |
|                                              |                                          |
|                                              |                                          |

## 矩阵

```matlab
\begin{matrix}
x & y \\
z & v
\end{matrix}
```

$$
\begin{matrix}
x & y \\
z & v
\end{matrix}
$$



```matlab
\begin{bmatrix}
0 & \cdots & 0 \\
\vdots & \ddots & \vdots \\
0 & \cdots & 0
\end{bmatrix}
```

$$
\begin{bmatrix}
0 & \cdots & 0 \\
\vdots & \ddots & \vdots \\
0 & \cdots & 0
\end{bmatrix}
$$

```matlab
\begin{Bmatrix}
1 & 2 \\
3 & 4
\end{Bmatrix}
```

$$
\begin{Bmatrix}
1 & 2 \\
3 & 4
\end{Bmatrix}
$$

```matlab
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
```

$$
\begin{pmatrix}
a & b \\
c & d
\end{pmatrix}
$$

- 条件定义

```matlab
f(n)=
\begin{cases}
n/2, & \mbox{if }n \mbox{ is even} \\
3n+1, & \mobx{if }n \mobx{ is odd}
\end{cases}
```

$$
f(n)=
\begin{cases}
n/2, & if\; n\;is\; even \\
3n+1, & if\; n\; is\; odd
\end{cases}
$$

```matlab
\begin{align}
f(x)&=(m+n)^2 \\
&=m^2 + 2mn + n^2\\
\end{align}
```

$$
\begin{align}
f(x)&=(m+n)^2 \\
&=m^2 + 2mn + n^2\\
\end{align}
$$

```matlab
\begin{alignat}{2}
f(x) & = (m-n) ^ 2 \\
f(x) & = (-m+n) ^ 2 \\
& = m^2-2mn+n^2 \\
\end{alignat}
```

$$
\begin{alignat}{2}
f(x) & = (m-n) ^ 2 \\
f(x) & = (-m+n) ^ 2 \\
& = m^2-2mn+n^2 \\
\end{alignat}
$$

```matlab
\begin{array}{lcl}
z & = & a\\
f(x,y,z) & = & x+y+z
\end{array}
```

$$
\begin{array}{lcl}
z & = & a\\
f(x,y,z) & = & x+y+z
\end{array}
$$

```matlab
\begin{cases}
3x + 5y +  z \\
7x - 2y + 4z \\
-6x + 3y + 2z
\end{cases}
```

$$
\begin{cases}
3x + 5y +  z \\
7x - 2y + 4z \\
-6x + 3y + 2z
\end{cases}
$$

```matlab
	
\begin{array}{|c|c||c|} a & b & S \\
\hline
0&0&1\\
0&1&1\\
1&0&1\\
1&1&0\\
\end{array}
```

$$
\begin{array}{|c|c|c|} a & b & S \\
\hline
0ooo&0&1\\
0&1&1\\
1&0&1\\
1&1&0\\
\end{array}
$$

# 五、字体

## 希腊字母

- 大写

| \Alpha   |  \Beta  | \Gamma   | \Delta   | \Epsilon   |
| -------- | :-----: | -------- | -------- | ---------- |
| $\Alpha$ | $\Beta$ | $\Gamma$ | $\Delta$ | $\Epsilon$ |

| \Zeta   | \Eta   | \Theta   | \Iota   | \Kappa   |
| ------- | ------ | -------- | ------- | -------- |
| $\Zeta$ | $\Eta$ | $\Theta$ | $\Iota$ | $\Kappa$ |

| \Lambda   | \Mu   | \\Nu  | \Xi   | \Omicron   |
| --------- | ----- | ----- | ----- | ---------- |
| $\Lambda$ | $\Mu$ | $\Nu$ | $\Xi$ | $\Omicron$ |

| \Pi   | \Rho   | \Sigma   | \Tau   | \Upsilon   |
| ----- | ------ | -------- | ------ | ---------- |
| $\Pi$ | $\Rho$ | $\Sigma$ | $\Tau$ | $\Upsilon$ |

| \Phi   | \Chi   | \Psi   | \Omega   | \\\\\\\\\\\\\ |
| ------ | ------ | ------ | -------- | ------------- |
| $\Phi$ | $\Chi$ | $\Psi$ | $\Omega$ |               |

- 小写

|  \alpha  | \beta   | \gamma   | \delta   | \epsilon   |
| :------: | ------- | -------- | -------- | ---------- |
| $\alpha$ | $\beta$ | $\gamma$ | $\delta$ | $\epsilon$ |

| \zeta   | \eta   | \theta   | \iota   | \kappa   |
| ------- | ------ | -------- | ------- | -------- |
| $\zeta$ | $\eta$ | $\theta$ | $\iota$ | $\kappa$ |

| \varkappa   | \lambda   | \mu   | \nu   | \xi   |
| ----------- | --------- | ----- | ----- | ----- |
| $\varkappa$ | $\lambda$ | $\mu$ | $\nu$ | $\xi$ |

| \omicron   | \pi   | \rho   | \sigma   | \tau   |
| ---------- | ----- | ------ | -------- | ------ |
| $\omicron$ | $\pi$ | $\rho$ | $\sigma$ | $\tau$ |

| \upsilon   | \phi    | \chi   | \psi   | \omega   |
| ---------- | ------- | ------ | ------ | -------- |
| $\upsilon$ | $\phi $ | $\chi$ | $\psi$ | $\omega$ |

| \varepsilon   | \vartheta   | \varkappa     | \varpi   |
| ------------- | ----------- | ------------- | -------- |
| $\varepsilon$ | $\vartheta$ | $ \varkappa $ | $\varpi$ |

| \varrho   | \varsigma   | \varphi   |
| --------- | ----------- | --------- |
| $\varrho$ | $\varsigma$ | $\varphi$ |

## 字体

#### 粗体

```matlab
\mathbb{ABCDEFGHIJKLMNOPQRSTUVWXYZ}
```

$$
\mathbb{ABCDEFGHIJKLMNOPQRSTUVWXYZ}
$$

只有大写拉丁字母才能正常显示，使用小写字母或数字时会得到其他符号。

---

```matlab
\boldsymbol{\Alpha \alpha \Beta \beta}
```

$$
\boldsymbol{\Alpha \alpha \Beta \beta}
$$

粗体的希腊字母，大小写均可

---

```matlab
\mathbf{012,abc,ABC}
```

$$
\mathbf{012,abc,ABC}
$$

拉丁字母和数字的粗体，不能用于希腊字母。

#### 哥特体

```matlab
\mathfrak{abcdefghijklmnopqrstuvwxyz}
\mathfrak{ABCDEFGHIJKLMNOPQRSTUVWXYZ}
\marhfrak{1234567890}
```

$$
\mathfrak{abcdefghijklmnopqrstuvwxyz}\\
\mathfrak{ABCDEFGHIJKLMNOPQRSTUVWXYZ}\\
\mathfrak{1234567890}
$$

#### 手写体

```matlab
\mathcal{ABCDEFGHIJKLMNOPQRSTUVWXYZ}
```

$$
\mathcal{ABCDEFGHIJKLMNOPQRSTUVWXYZ}
$$

注意：手写字体仅对大写拉丁字母有效。

# 六、括号



# 其他

|                                |                 |
| ------------------------------ | --------------- |
| 加帽子^  `\hat` 或者`\widehat` | $\hat A$        |
| 上划线 `\overline`             | $ \overline A$  |
| 下划线`\undweline`             | $\underline{A}$ |
| 加波浪线 `\widetilde`          | $\widetilde A$  |
| 加点 `\dot{}`                  | $\dot A$        |
| 加两个点 `\ddot`               | $\ddot A$       |
| 加箭头 `\vec`                  | $\vec A$        |
