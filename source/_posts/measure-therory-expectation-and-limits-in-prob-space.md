---
title: measure therory-expectation and limits in prob space
cover: /cover_image/37021_week8_cover.jpg
tags:
  - math
  - measure theory
categories:
  - measure theory
abbrlink: '5038e228'
date: 2024-11-15 02:05:12
---
# &#x1F3C2;{% span blue, Modern Prob %}

{% progress 1 gray 开始观看 %}

## &#x1F955; {% emp Expectation %}

### &#x26FA;{% wavy 什么是期望？ %}

首先，我们假设有一个随机变量 $X$，它是在概率空间 $(\Omega, \mathcal{F}, \mathbb{P})$ 上定义的。简单来说，这意味着 $X$ 在某种“世界”中随机地取值，而我们希望了解它的“平均值”或“期望”，也就是 $\mathbb{E}(X)$。在这个世界中，$\Omega$ 是所有可能发生的情况的集合（称为样本空间），$\mathcal{F}$ 是这些事件的集合，$\mathbb{P}$ 是每个事件发生的概率。

我们可以将期望 $\mathbb{E}(X)$ 想象成这样一个问题：假设你每次从一个袋子中随机取一个数 $X$，那么你取到的数的平均值会是多少呢？这就是我们要用 Lebesgue-Stieltjes 积分来计算的。

公式：
$$
\mathbb{E}(X) := \int_{\Omega} X \, d\mathbb{P}
$$

这个积分表示了我们在所有可能的情况（样本空间 $\Omega$）中，对 $X$ 进行加权平均，而加权的方式由概率 $\mathbb{P}$ 来决定。

---
{% span red, Step 1: 简单随机变量的期望 %}

假设 $X$ 是一个简单的随机变量，这意味着它可以被分解成一些常数和指示函数的加和形式：
$$
X = \sum_{k=1}^{\infty} a_k \mathbf{1}_{A_k}
$$
这里，$a_k$ 是一些常数，$\mathbf{1}_{A_k}$ 是事件 $A_k$ 是否发生的“指示函数”（它在 $A_k$ 上为 1，其他地方为 0）。举个例子，如果 $X$ 表示抛一枚硬币的结果，那么 $X$ 可能取值为 0（表示反面）或 1（表示正面）。我们可以将 $X$ 表示成 $X = 1 \cdot \mathbf{1}_{\{正面\}} + 0 \cdot \mathbf{1}_{\{反面\}}$。

对于这样的简单变量，期望可以定义为：
$$
\mathbb{E}(X) = \sum_{k=1}^{\infty} a_k \mathbb{P}(A_k)
$$
这相当于我们在每种可能的结果上取它的值 $a_k$ 并乘以其发生的概率 $\mathbb{P}(A_k)$。

---
{% span red, Step 2: 一般非负随机变量的期望 %}

如果 $X \geq 0$ 是一个一般的随机变量，我们可以将它“分层”表示成一系列简单随机变量 $X_n$ 的极限，其中：
$$
X_n := \sum_{k=1}^{\infty} \frac{k-1}{2^n} \mathbf{1}\left(\frac{k-1}{2^n} < X \leq \frac{k}{2^n}\right)
$$
通过这种方式，我们逐渐“逼近” $X$，并定义 $\mathbb{E}(X)$ 为 $X_n$ 的期望的极限：
$$
\mathbb{E}(X) = \lim_{n \to \infty} \mathbb{E}(X_n)
$$
可以想象我们在无限细分 $X$ 的可能取值区间，使得 $X_n$ 越来越接近真实的 $X$。比如，把一个随机数值的范围不断切分得更小，以便获得更精确的期望值。

---
{% span red, Step 3: 一般随机变量的期望 %}

对于任意随机变量 $X$，我们将其拆分为正的部分和负的部分：
$$
X = X^+ - X^-
$$

其中 $X^+ := X \vee 0$ 表示 $X$ 的非负部分，而 $X^- := (-X) \vee 0$ 表示 $X$ 的非正部分。

于是，我们可以定义 $X$ 的期望为：
$$
\mathbb{E}(X) := \int_{\Omega} X \, d\mathbb{P}:= \int_{\Omega} X^+-\int_{\Omega} X^- \ \, d\mathbb{P}:=\mathbb{E}(X^+) - \mathbb{E}(X^-)\tag{1}
$$
这种方法的本质是，我们将一个有正负值的随机变量分解成纯粹的非负值的部分，从而使得每一部分的期望都可计算，然后通过减法得到 $X$ 的期望。

可以把这种分解的过程类比成我们计算净资产：假设 $X$ 是你的总资产值（有正有负），$X^+$ 是你所有的资产，而 $X^-$ 是你的所有负债，那么 $X$ 的期望就是你资产的期望减去负债的期望，得到净资产的期望值。

---

接下来讲一下可积随机变量，这个概念告诉我们，在什么情况下我们能够计算一个随机变量的期望，并且这个期望是有限的。

## &#x1F955; {% emp Integrable random variables %}

### &#x26FA;{% wavy 可积随机变量的定义 %}

假设我们有一个随机变量 $X$，如果 $X$ 的期望 $\mathbb{E}(X)$ 存在且是有限的，那么我们称 $X$ 为一个**可积随机变量**。这也就是为什么在(14)中，我们有了一个条件来判断期望是否存在并且是有限的。

### &#x26FA;{% wavy 公式分析 %}

1. **绝对值的分解**：我们将 $|X|$ 表示为 $X^+$ 和 $X^-$ 的和：
   $$
   |X| = X^+ + X^- \tag{2}
   $$

   这意味着 $X$ 的绝对值等于它的正部分和负部分之和。可以理解为，将 $X$ 的正值部分和负值部分拆开，然后把这两部分加起来。

2. **期望的绝对值**：通过对 $|X|$ 求期望，我们得到：
   $$
   \mathbb{E}|X| = \mathbb{E}X^+ + \mathbb{E}X^-\tag{3}
   $$

   这条公式表明，如果 $X$ 的正部分和负部分的期望都是有限的，那么 $X$ 的期望就是有限的。换句话说，如果 $\mathbb{E}X^+$ 和 $\mathbb{E}X^-$ 都存在且是有限值，那么 $\mathbb{E}|X|$ 也是有限的。

3. **判断期望存在的条件**：从(1)和(3)，可以得到以下等价条件：
   $$
   \mathbb{E}(X) \text{ 存在且有限} \iff \mathbb{E}|X| < \infty \iff \mathbb{E}X^+ < \infty \, \text{且} \, \mathbb{E}X^- < \infty \tag{4}
   $$

   这个条件的意思是，只要 $X$ 的正部分和负部分的期望都存在且有限，那么整个 $X$ 的期望就是存在且有限的。

>:star2:可以将这个情况想象成一个“资产负债表”的情景：$X$ 是你的净资产，$X^+$ 是你所有的资产，$X^-$ 是你的负债。如果你的资产和负债都不是无限大的（也就是 $\mathbb{E}X^+ < \infty$ 且 $\mathbb{E}X^- < \infty$），那么你的净资产 $X$ 的期望值就是有限的。这样，我们就可以对你的资产状况做出有意义的统计分析了。

这段定理的核心是理解在什么情况下我们可以计算一个随机变量的期望值，并且保证这个值是有限的。只要正部分和负部分的期望都是有限的，那么这个随机变量就称为可积随机变量(<u>integrable random variable</u>)或者that its <u>expectation/mean is ﬁnite</u>，也就是我们可以合理地去计算它的期望。

## &#x1F955; {% emp Lebesgue-Stieltjes (LS) 积分 %}

### &#x26FA;{% wavy LS积分的构造 %}

首先，我们有一个测度空间 $(S, \mathcal{B}, \mu)$，其中：
- $S$ 是我们讨论的空间（可以是实数集 $\mathbb{R}$ 或更广泛的集合）。
- $\mathcal{B}$ 是 $S$ 上的一个 $\sigma$-代数，也就是说它是 $S$ 的所有可能“事件”或“集合”的一个集合。
- $\mu$ 是定义在 $\mathcal{B}$ 上的一个测度（它可以不是概率测度）。测度的作用是给集合赋予一个“大小”或“体积”。

假设我们有一个函数 $f: S \rightarrow \mathbb{R}$，并且它在测度 $\mu$ 下是可测的。我们定义 $f$ 关于 $\mu$ 的 LS 积分为：
$$
\int_S f \, d\mu = \int_S f(s) \, d\mu(s)
$$
这个积分的含义是我们对 $S$ 空间中的每一个“点” $s$，按照测度 $\mu$ 的权重来“加权”函数 $f$ 的值。这种加权积分方式比传统的 Riemann 积分更灵活，因为它允许处理的函数和集合更加复杂。

### &#x26FA;{% wavy 特殊情况：Lebesgue 积分 %}

当 $S = \mathbb{R}$，$\mathcal{B} = \mathcal{B}(\mathbb{R})$ 表示实数上的 Borel 集合，$\mu = \lambda$ 为 Lebesgue 测度时，我们得到 **Lebesgue 积分**。这时，LS 积分就简化为普通的 Lebesgue 积分，记作：
$$
\int_{\mathbb{R}} f \, d\lambda = \int_{\mathbb{R}} f(x) \, dx
$$

Lebesgue 积分的出现是为了扩展 Riemann 积分，使得我们可以对更加复杂的函数进行积分，比如一些在传统 Riemann 积分下无法处理的函数。

### &#x26FA;{% wavy 特殊情况：Lebesgue-Stieltjes 积分 %}

当测度 $\mu$ 是 $\lambda_F$，其中 $\lambda_F$ 是一个由分布函数 $F$ 生成的测度时，我们得到的是 Lebesgue-Stieltjes 积分：
$$
\int_{\mathbb{R}} f \, d\lambda_{F} = \int_{\mathbb{R}} f(x) \, dF(x)
$$

分布函数 $F$ 描述了随机变量的累积分布，而积分 $\int f \, dF$ 表示对 $f$ 进行加权平均，权重由 $F$ 确定。

## &#x1F955; {% emp The standard machine %}

这个方法是用来证明一些“线性”陈述的，特别是关于期望的等式。

### &#x26FA;{% wavy 标准机器的应用 %}

标准机器的流程如下：假设我们想要证明某个关于随机变量 $X$ 的函数 $f(X)$ 的期望 $\mathbb{E}[f(X)]$ 可以通过积分的形式表示为
$$
\mathbb{E}[f(X)] = \int_{\Omega} f(X) \, d\mathbb{P} = \int_{\mathbb{R}} f(x) \, d\mathbb{P}_X(x) = \int_{\mathbb{R}} f(x) \, dF_X(x)\tag{5}
$$
其中：
- $\mathbb{P}_X$ 是随机变量 $X$ 的分布，即 $X$ 的“规律”或“法则”。
- $F_X$ 是 $X$ 的累积分布函数，描述了 $X$ 在不同区间上取值的概率累积。

这个等式意味着：**对随机变量 $X$ 的期望可以通过 $X$ 的概率分布来计算**。它告诉我们，计算期望时可以直接使用分布函数 $F_X$ 来加权求和，而不必回到原始的概率空间 $\Omega$。

{% span green, 使用标准机器的步骤 %}

为了证明上面的等式（公式 (5)），我们会按照以下步骤来逐步处理不同的函数类型：
{% note primary modern %}
1. **从简单函数开始**：先考虑 $f$ 是某个 Borel 集合的指示函数。这样可以通过简单的事件计算来理解。
2. **逐步扩展到更复杂的函数**：然后我们将 $f$ 扩展到简单函数（可以表示为有限个常数和指示函数的和），之后是非负的函数。
3. **最终处理一般实值函数**：最后，处理任意的实值函数，这一步是通过限制条件和取极限完成的。
{% endnote %}

这样分步操作的优点是，先处理简单的情况，然后逐渐增加复杂性，直到覆盖一般的情况。这种方法可以确保证明过程清晰、严谨，并且可以用来处理非常一般的随机变量和函数。

>“标准机器”方法是一种有条理的证明工具，特别适用于在测度论中处理关于期望的线性陈述。通过分步处理，我们可以从最简单的指示函数出发，最终证明出一般函数的期望公式。这种技巧在测度论和概率论中非常实用，可以用来证明各种关于期望和积分的等式。

## &#x1F955; {% emp Monotone Convergence Theorem %}

**单调收敛定理**（Monotone Convergence Theorem），是测度论中一个非常重要的工具，用于分析一列随机变量的期望收敛行为。

### &#x26FA;{% wavy 定理内容 %}

假设 $(X_n)$ 是一列随机变量，并且 $X$ 是一个随机变量，定义在同一个概率空间 $(\Omega, \mathcal{F}, \mathbb{P})$ 上。若满足以下两个条件：
1. 当$n \to \infty$，则$X_n \geq 0$ 且 $X_n \uparrow X$ [6]，即 $(X_n)$ 是一个非减序列并且收敛到 $X$；
2. 则我们有 $\mathbb{E}(X_n) \uparrow \mathbb{E}(X)$，即 $(X_n)$ 的期望随着 $n$ 增大单调收敛到 $X$ 的期望。

形式化地表示为：
$$
\lim_{n \to \infty} \mathbb{E}(X_n) = \mathbb{E}(X)
$$
<font title="red">证明过程</font>

这个证明的核心思想是通过构造一列简单的函数 $(Z_n)$ 来逼近 $X$，并验证在这种构造下，$Z_n$ 和 $X_n$ 的期望逐步收敛到 $\mathbb{E}(X)$。证明分为几个关键步骤。

*第一步：定义序列 $Z_n$*

定义 $Z_n$ 为：
$$
Z_n := \sum_{k=1}^{\infty} \frac{k-1}{2^n} \mathbf{1}\left(\frac{k-1}{2^n} < X \leq \frac{k}{2^n}\right)
$$
其中 $\mathbf{1}$ 表示指示函数，即对于满足 $\frac{k-1}{2^n} < X \leq \frac{k}{2^n}$ 的情形，$\mathbf{1}$ 取值为 1，其他情形取值为 0。这样构造的 $Z_n$ 是一个简单函数，它逼近 $X$。

*第二步：验证 $Z_n \uparrow X$ 且 $\mathbb{E}(Z_n) \uparrow \mathbb{E}(X)$*

由于 $Z_n \uparrow X$（因为 $Z_n$ 随着 $n$ 增大不断逼近 $X$），根据单调性，我们有 $\mathbb{E}(Z_n) \uparrow \mathbb{E}(X)$。所以可以找到一个足够大的 $n$ 使得
$$
L < \mathbb{E}(Z_n)
$$

其中 $L \leq \mathbb{E}(X)$ 是一个假设值，目的是为了构建一个矛盾。

*第三步：重写 $Z_n$*

通过以下过程将 $Z_n$ 进一步展开为简单的项：
$$
Z_n = \sum_{k=1}^{\infty} \frac{k-1}{2^n} \mathbf{1}\left(\frac{k-1}{2^n} < X\right) - \sum_{k=1}^{\infty} \frac{k-1}{2^n} \mathbf{1}\left(\frac{k}{2^n} < X\right)
$$


接下来对 $Z_n$ 进行一系列分拆、移项，得到
$$
Z_n = \sum_{k=1}^{\infty} \frac{1}{2^n} \mathbf{1}\left(X > \frac{k}{2^n}\right)
$$


*第四步：利用公式[6]*

通过以上展开，我们可以得到
$$
\mathbb{E}(Z_n) = \sum_{k=1}^{\infty} \frac{1}{2^n} \mathbb{P}\left(X > \frac{k}{2^n}\right)
$$


所以，只要 $X_n \uparrow X$，我们就可以证明 $\mathbb{E}(X_n) \uparrow \mathbb{E}(X)$，这完成了证明。

### &#x26FA;{% wavy Fatou's Lemma %}

{% span green, 引理内容 %}

设 $(X_n)$ 是定义在同一概率空间 $(\Omega, \mathcal{F}, \mathbb{P})$ 上的随机变量序列。如果满足以下条件，则有：
1. 如果 $X_n \geq 0$ 对于所有 $n \geq 1$，则
   $$
   \mathbb{E} \left( \liminf_{n \to \infty} X_n \right) \leq \liminf_{n \to \infty} \mathbb{E}(X_n)\tag{7}
   $$


2. 如果 $X_n \leq 0$ 对于所有 $n \geq 1$，则
   $$
   \mathbb{E} \left( \limsup_{n \to \infty} X_n \right) \geq \limsup_{n \to \infty} \mathbb{E}(X_n)\tag{8}
   $$



这些不等式的意义在于，它们描述了当我们对一个随机变量序列取下极限或上极限后，再计算期望时的行为。

{% span blue, 证明过程 %}
对于(7)

假设 $X_n \geq 0$ 对于所有 $n \geq 1$，我们希望证明
$$
\mathbb{E} \left( \liminf_{n \to \infty} X_n \right) \leq \liminf_{n \to \infty} \mathbb{E}(X_n)
$$

1. 通过单调收敛定理（Monotone Convergence Theorem，MCT），我们知道 $\inf_{k \geq n} X_k \geq 0$ 随着 $n$ 增大而单调递增，并且 $\inf_{k \geq n} X_k \leq X_k$ 对所有 $k \geq n$ 成立。

2. 因此，我们可以得到：
   $$
   \mathbb{E} \left( \liminf_{n \to \infty} X_n \right) = \mathbb{E} \left( \lim_{n \to \infty} \inf_{k \geq n} X_k \right) = \lim_{n \to \infty} \mathbb{E} \left( \inf_{k \geq n} X_k \right)
   $$

   这里使用了单调收敛定理，因为 $\inf_{k \geq n} X_k$ 是一个非减序列。

3. 根据期望的性质，可以得到不等式：
   $$
   \mathbb{E} \left( \inf_{k \geq n} X_k \right) \leq \inf_{k \geq n} \mathbb{E}(X_k)
   $$



4. 于是，有：
   $$
   \lim_{n \to \infty} \mathbb{E} \left( \inf_{k \geq n} X_k \right) \leq \lim_{n \to \infty} \inf_{k \geq n} \mathbb{E}(X_k) = \liminf_{n \to \infty} \mathbb{E}(X_n)
   $$

这证明了第一个不等式成立。

对于(8)

如果 $X_n \leq 0$ 对所有 $n \geq 1$，我们希望证明
$$
\mathbb{E} \left( \limsup_{n \to \infty} X_n \right) \geq \limsup_{n \to \infty} \mathbb{E}(X_n)
$$

1. 我们可以通过对(7)两边乘以 $-1$ 来得到这个结论。设 $Y_n = -X_n$，于是 $Y_n \geq 0$，根据(19)得到：
   $$
   \mathbb{E} \left( \liminf_{n \to \infty} Y_n \right) \leq \liminf_{n \to \infty} \mathbb{E}(Y_n)
   $$



2. 展开上述不等式，我们有：
   $$
   \mathbb{E} \left( -\limsup_{n \to \infty} X_n \right) \leq -\limsup_{n \to \infty} \mathbb{E}(X_n)
   $$

3. 从而得到：
   $$
   \mathbb{E} \left( \limsup_{n \to \infty} X_n \right) \geq \limsup_{n \to \infty} \mathbb{E}(X_n)
   $$



这样，我们证明了第二个不等式。

---

## &#x1F955; {% emp Dominated Convergence Theorem %}

**主导收敛定理**（Dominated Convergence Theorem, DCT）通常用于处理随机变量序列的期望的极限交换问题。该定理可在一定条件下将极限操作和积分操作交换顺序。

{% span green, 引理内容 %}

假设 $(X_n)$ 和 $X$ 是定义在概率空间 $(\Omega, \mathcal{F}, \mathbb{P})$ 上的随机变量序列。若满足以下条件：
1. $X_n \to X$ 几乎必然地收敛于 $X$，即
   $$
   X_n \to X \quad \text{当} \quad n \to \infty
   $$


2. 存在一个**可积随机变量** $Z$ 使得对所有 $n \geq 1$，
   $$
   |X_n| \leq Z
   $$

   
则我们可以得到以下结论：
$$
\lim_{n \to \infty} \mathbb{E}(X_n) = \mathbb{E}(X)
$$



{% span blue, 证明过程 %}

为了证明这个定理，我们主要应用法图引理（Fatou's Lemma）以及对期望的极限性质的分析。

1. **利用有界性条件**：由于我们知道 $|X_n| \leq Z$ 且 $Z$ 是一个可积的随机变量，因此我们可以构造一个新的随机变量 $\tilde{Z}$ 使得 $\tilde{Z} = 2Z$。这样，我们有
   $$
   |X_n - X| \leq |X_n| + |X| \leq 2Z = \tilde{Z}
   $$
   这说明 $(X_n - X)$ 也被 $\tilde{Z}$ 支配。

2. **应用Fatou引理**：根据法图引理，我们可以得到
   $$
   \mathbb{E} \left( \liminf_{n \to \infty} |X_n - X| \right) \leq \liminf_{n \to \infty} \mathbb{E}(|X_n - X|)
   $$

   由于 $X_n \to X$，因此 $\liminf_{n \to \infty} |X_n - X| = 0$。于是有
   $$
   \mathbb{E}(0) \leq \liminf_{n \to \infty} \mathbb{E}(|X_n - X|)
   $$

   因此得到
   $$
   \liminf_{n \to \infty} \mathbb{E}(|X_n - X|) = 0
   $$



3. **证明$\limsup$也为零**：另一方面，我们也可以得到
   $$
   0 \leq \limsup_{n \to \infty} \mathbb{E}(|X_n - X|) \leq \mathbb{E}(0) = 0
   $$

   这意味着
   $$
   \lim_{n \to \infty} \mathbb{E}(|X_n - X|) = 0
   $$

   
4. **推导期望的极限**：由此我们可以得到
   $$
   \lim_{n \to \infty} \mathbb{E}(X_n) = \mathbb{E}(X)
   $$

这完成了主导收敛定理的证明。
{% note blue 'fas fa-bullhorn' modern %}
主导收敛定理，使我们能够在满足一定条件（有界性和收敛性）的情况下，将求期望和取极限这两个操作交换顺序。
{% endnote %}
---

在主导收敛定理中，$|X_n| \leq Z$不可省略。如果我们想让 $X_n \to X$ 几乎必然地收敛推出结论 $\mathbb{E}(X_n) \to \mathbb{E}(X)$，则必须有 $|X_n| \leq Z$的存在，即存在一个可积随机变量 $Z$ 使得 $|X_n| \leq Z$ 对所有 $n \geq 1$ 成立。主导条件提供了一个“上界”，保证了收敛的随机变量序列 $(X_n)$ 在期望上也是受控的。

通过证明过程，我们实际上得到了一个更强的结论即：
$$
\lim_{n \to \infty} \mathbb{E}(|X_n - X|) = 0
$$

---

## &#x1F955; {% emp Scheffé引理 %}

**Scheffé引理**在测度论中提供了一个重要的结果，说明了当一列随机变量的积分或期望收敛时，该随机变量的$L^1$收敛性（即期望的绝对差的收敛性）。以下是对引理内容及其证明的详细讲解。

{% span green, 引理内容 %}

设 $(X_n)$ 和 $X$ 是定义在同一概率空间 $(\Omega, \mathcal{F}, \mathbb{P})$ 上的随机变量序列。若满足以下条件：

1. $X_n \to X$ 几乎必然收敛，即
   $$
   X_n \to X \quad \text{当} \quad n \to \infty
   $$
2. 同时满足
   $$
   \mathbb{E}|X_n| \to \mathbb{E}|X| \quad \text{当} \quad n \to \infty
   $$

则可以得出结论
$$
\mathbb{E}|X_n - X| \to 0 \quad \text{当} \quad n \to \infty
$$
<font title="red">证明过程</font>

证明采用分解和极限分析，具体步骤如下：

1. **设定非负性条件**：首先假设 $X_n \geq 0$ 对于所有 $n \geq 1$，因此 $X \geq 0$ 也成立（因为 $X_n \to X$）。

   根据 $L^1$的三角不等式，可以得到
   $$
   \mathbb{E}|X_n - X| = \mathbb{E}((X_n - X)^+) + \mathbb{E}((X_n - X)^-)
   $$
   我们需要证明 $\mathbb{E}((X_n - X)^+) \to 0$ 和 $\mathbb{E}((X_n - X)^-) \to 0$。
2. **分解正负部分的期望**：利用条件$\mathbb{E}|X_n| \to \mathbb{E}|X|$，我们可以得出
   $$
   \mathbb{E}((X_n - X)^+) - \mathbb{E}((X_n - X)^-) \to 0
   $$
   由于 $X_n \geq 0$ 并且 $X \geq 0$，这意味着只需证明 $\mathbb{E}((X_n - X)^-) \to 0$ 即可。

3. **处理负部分的期望**：通过观察 $(X_n - X)^- = (X - X_n) \mathbf{1}_{\{X_n \leq X\}} \leq X$，可以利用主导收敛定理（DCT），因为 $X$ 可积且 $\mathbb{E}|X_n| \to \mathbb{E}|X|$，因此
   $$
   \mathbb{E}((X_n - X)^-) \to 0
   $$

4. **一般实值情况**：如果 $X_n$ 和 $X$ 是实值的，我们可以利用Fatou引理得出正负部分的极限。具体计算会表明 $\mathbb{E}((X_n)^+) \to \mathbb{E}(X^+)$ 和 $\mathbb{E}((X_n)^-) \to \mathbb{E}(X^-)$，因此 $\mathbb{E}|X_n - X| \to 0$。

最终，以上步骤完成了对Scheffé引理的证明。

## &#x1F955; {% emp Null sets and expectation %}

{% span green, 定理内容 %}

1. **部分零测集上的期望为零**  
   若 $X$ 是一个随机变量，且事件 $A \in \mathcal{F}$ 满足 $\mathbb{P}(A) = 0$，则
   $$
   \mathbb{E}(X \mathbf{1}_A) = 0
   $$

   这是因为在零测集 $A$ 上，随机变量 $X$ 的取值对于期望没有贡献。

2. **几乎处处相等的随机变量的期望相等**  
   如果两个随机变量 $X$ 和 $Y$ 满足几乎处处相等，即 $\mathbb{P}(X = Y) = 1$，则
   $$
   \mathbb{E}(X) = \mathbb{E}(Y)
   $$

   这是因为 $X$ 和 $Y$ 的差值仅在零测集上取非零值，所以它们的期望相等。

3. **非负随机变量的期望为零意味着几乎处处为零**  
   如果 $X \geq 0$ 且 $\mathbb{E}(X) = 0$，则
   $$
   X = 0 \quad \mathbb{P}\text{-a.s.}
   $$

   也就是说，$X$ 几乎处处等于零。因为对于一个非负随机变量来说，只有当它在几乎所有位置取零值时，其期望才会为零。

{% span green, 证明 %}

(i) 如果 $X$ 是随机变量，且事件 $A$ 满足 $\mathbb{P}(A) = 0$，则 $\mathbb{E}(X \mathbf{1}_A) = 0$。

{% span blue, 证明过程 %}

1. 首先，我们注意到 $\mathbb{E}(X \mathbf{1}_A)$ 的绝对值可以被 $\mathbb{E}(|X| \mathbf{1}_A)$ 控制：
   $$
   |\mathbb{E}(X \mathbf{1}_A)| \leq \mathbb{E}(|X| \mathbf{1}_A)
   $$

   因此，我们可以假设 $X \geq 0$，因为若对 $|X|$ 成立，则对 $X$ 也成立。

2. 接下来，利用**单调收敛定理**（MCT，Theorem 4），考虑一个截断的随机变量序列 $(X \land n) \mathbf{1}_A$。这个序列随着 $n \to \infty$ 单调递增趋向于 $X \mathbf{1}_A$。

3. 根据 MCT，我们有：
   $$
   \mathbb{E}(X \mathbf{1}_A) = \lim_{n \to \infty} \mathbb{E}((X \land n) \mathbf{1}_A)
   $$



4. 因为 $A$ 是零测集，所以对于任何固定的 $n$，$\mathbb{E}((X \land n) \mathbf{1}_A) \leq n \cdot \mathbb{P}(A) = 0$。所以，
   $$
   \mathbb{E}(X \mathbf{1}_A) = 0
   $$

   证明完成。

(ii) 如果 $X = Y$ 几乎处处成立，则 $\mathbb{E}(X) = \mathbb{E}(Y)$。

{% span blue, 证明过程 %}

1. 定义 $A = \{X \neq Y\}$。因为 $X = Y$ 几乎处处成立，所以 $\mathbb{P}(A) = 0$。

2. 利用(i)的结论，我们有：
   $$
   \mathbb{E}(X) = \mathbb{E}(X \mathbf{1}_A + X \mathbf{1}_{A^c}) = \mathbb{E}(X \mathbf{1}_{A^c}) = \mathbb{E}(Y \mathbf{1}_{A^c}) = \mathbb{E}(Y \mathbf{1}_A + Y \mathbf{1}_{A^c}) = \mathbb{E}(Y)
   $$


这证明了若 $X = Y$ 几乎处处成立，则 $\mathbb{E}(X) = \mathbb{E}(Y)$。

(iii) 如果 $X \geq 0$ 且 $\mathbb{E}(X) = 0$，则 $X = 0$ 几乎处处成立。

{% span blue, 证明过程 %}

1. 假设 $X$ 是非负的，且 $\mathbb{E}(X) = 0$。

2. 对于每一个 $n \geq 1$，我们有：
   $$
   0 = \mathbb{E}(X) = \mathbb{E}(X \mathbf{1}_{\{X > 0\}}) \geq \mathbb{E}\left(X \mathbf{1}\left\{X \geq \frac{1}{n}\right\}\right) \geq \frac{1}{n} \mathbb{P}\left(X \geq \frac{1}{n}\right)
   $$

3. 这说明 $\mathbb{P}\left(X \geq \frac{1}{n}\right) = 0$ 对于所有 $n \geq 1$ 成立。

4. 通过概率的下连续性（continuity from below），我们得到
   $$
   \mathbb{P}(X > 0) = \lim_{n \to \infty} \mathbb{P}\left(X \geq \frac{1}{n}\right) = 0
   $$

5. 因此，$X = 0$ 几乎处处成立。证明完成。

以上文档的pdf可以用以下链接下载：

<a href="https://github.com/pennyzhao1507288/img_store/raw/main/37021%20modern%20prob/37021_week7.pdf" class="download-button" download>
<i class="fas fa-download"></i> 下载文档
</a>

{% progress 100 red 观看结束，感谢观看 %}