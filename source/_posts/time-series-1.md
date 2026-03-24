---
title: Time series(1)
cover: /cover_image/38032_week1_cover.jpg
tags:
  - math
categories:
  - time series
abbrlink: ecfd70f5
date: 2025-01-25 09:28:05
---
# Time Series

{% progress 1 gray 开始观看 %}

### 什么是时间序列？

1. **时间序列的定义**  
   时间序列是一个随机过程，可以记作 $\{x_t, t \in T\}$，其中 $t$ 表示时间, $x_t$ 表示观测数据。
2. **离散参数时间序列**  
   当 $T = \mathbb{Z}$（整数集）时，时间序列称为离散参数时间序列：
    - 时间点是等间隔的，比如每小时、每天、每周、每年等。
    - 通常假设时间序列可以追溯到无限过去。
3. **连续参数时间序列**  
   如果 $T = \mathbb{R}$（实数集），时间序列就是连续参数的。例如，时间可以是任意实数值。
4. **连续转离散**  
   有些情况下，连续时间序列可以通过采样变成离散序列，比如每天中午的温度（如果我们要记录每时每刻温度的变化，那么此时就是连续时间序列，但如果我们每天只记录中午12点的气温。这种记录方式就相当于对连续序列进行了采样，时间点变成离散的）现实中这种采样例子有很多，比如说从股票的每秒变动记录中提取每天的收盘价。

---

### Finite realization and learning from historical data

The set of observations $\{x_t, t = 1, . . . , n\}$ is called a realization(<span style="color:#990000;">实现</span>或一条<span style="color:#990000;">轨迹</span>) of the time series $\{x_t, t ∈ Z\}$

1. **有限观测**  
   在实际中，我们无法观察到时间序列的无限过去，只能观测到有限时间点的数据 $\{x_1, x_2, \dots, x_n\}$，其中 $n > 0$ 是有限的。
2. **理解依赖关系**  
   我们的目标是从数据中理解 $x_t$ 如何依赖于过去的值，比如 $x_{t-1}, x_{t-2}$ 等。
3. **平稳性假设**  
   理解 $x_t$ 依赖关系的难度可以降低，如果假设：
    - $x_t$ 的均值相同。
    - 对过去值的依赖关系一致。

> [!Tip]
>
> 时间序列分析的主要任务就是根据观测数据的特点为数据建立尽可能合理的统计模型，然后利用模型的统计特性去解释数据的统计规律，以其达到控制或者预报的目的，
>
> :exclamation:<span style="color:#990000;">时间序列是按时间次序排列的随机变量序列，任何时间序列经过合理的函数变换后都可以被认为是由三个部分叠加而成. 这三个部分是趋势项部分、周期项部分和随机噪声项部分</span>

## Stationarity(平稳序列)

### Strict Stationarity

1. **严格平稳性的定义**  
   时间序列 $\{x_t\}$ 被称为**严格平稳（strictly stationary）**，如果任意时间点的联合分布不随时间平移而变化。具体来说，对于任意 $n \geq 1$ 和任意时间点 $t_1, t_2, \dots, t_n \in \mathbb{Z}$ 以及 $s \in \mathbb{Z}$：
   $$ (x_{t_1}, x_{t_2}, \dots, x_{t_n}) \sim (x_{t_1+s}, x_{t_2+s}, \dots, x_{t_n+s}). $$  
   这意味着无论时间点如何移动，联合分布保持相同。

2. **特殊情况**
    - 当 $n = 1$：单个随机变量的分布不随时间平移而变化，即 $x_t \sim x_{t+s}$。
    - 当 $n = 2$：两个随机变量的联合分布不随时间平移而变化，即 $(x_{t_1}, x_{t_2}) \sim (x_{t_1+s}, x_{t_2+s})$。
    - 类似地，对于任意 $n$，所有变量的联合分布不随时间移动而变化。

3. **结论**  
   严格平稳性表示时间序列的分布特性不依赖于时间，而只依赖于时间点之间的相对位置。这是时间序列分析中最强的平稳性条件。

---

### An i.i.d Sequence

1. **独立同分布序列**  
   假设时间序列 $\{x_t\}$ 是一个独立同分布（i.i.d.）的随机序列，其均值和方差分别为 $\mu$ 和 $\sigma^2$：  
   $$ \{x_t\} \sim IID(\mu, \sigma^2). $$

2. **联合分布证明严格平稳性**

    - 任意 $t_1, t_2, \dots, t_n$，联合分布函数是：  
      $$ P(x_{t_1} \leq x_1, \dots, x_{t_n} \leq x_n) = P(x_{t_1} \leq x_1) \cdot P(x_{t_2} \leq x_2) \cdot \dots \cdot P(x_{t_n} \leq x_n). $$  
      因为 $\{x_t\}$ 是独立的。
    - 经过时间平移后的联合分布是：  
      $$ P(x_{t_1+s} \leq x_1, \dots, x_{t_n+s} \leq x_n) = P(x_{t_1+s} \leq x_1) \cdot P(x_{t_2+s} \leq x_2) \cdot \dots \cdot P(x_{t_n+s} \leq x_n). $$  
      因为每个随机变量的分布相同（同分布），这等于原分布。

   > **结论** : 因此，i.i.d. 序列是严格平稳的，因为无论如何平移时间点，联合分布都保持不变。

### Weak Stationarity

1. **一阶和二阶矩的回顾**

    - **方差**：对于单个随机变量 $X$，其方差定义为 $Var\{X\} = \mathbb{E}[(X - \mathbb{E}[X])^2]$，表示其波动的程度。
    - **协方差**：对于两个随机变量 $X$ 和 $Y$，协方差定义为 $Cov\{X, Y\} = \mathbb{E}[(X - \mathbb{E}[X])(Y - \mathbb{E}[Y])]$。如果 $X = Y$，协方差就是方差。

2. **弱平稳性的定义**  
   如果满足以下条件，则时间序列 $\{x_t\}$ 是**弱平稳**（weakly stationary）的：

    - $\mathbb{E}[x_t] = \mathbb{E}[x_{t+s}]$，即 $x_t$ 的均值不随时间 $t$ 变化。
    - 对于任意的 $t_1, t_2$ 和 $s$，协方差 $Cov\{x_{t_1}, x_{t_2}\}$ 仅与时间差 $s = t_2 - t_1$ 有关，不依赖于具体的时间点。

   > [!CAUTION]
   >
   > **弱平稳的两个重要性质**
   >
   > - 均值 $\mu_t = \mathbb{E}[x_t]$ 是一个常数（不依赖于 $t$）。
   > - 协方差 $R_t(s) = Cov\{x_t, x_{t+s}\}$ 是仅依赖于时间差 $s$ 的函数。

4. **自协方差函数**  
   函数 $R(s) = Cov\{x_t, x_{t+s}\}$ 称为自协方差函数（autocovariance function）。其中 $s$ 是**滞后（lag）**，即两个时间点之间的间隔。  
   特别地，当 $s = 0$ 时：  
   $$R(0) = Cov\{x_t, x_t\} = Var\{x_t\}$$  
   因此，时间序列的方差是一个常量。

---

### White Noise

1. **白噪声的定义**  
   白噪声是均值相同、方差相同且不相关的一组随机变量 $\{x_t\}$。它用以下形式表示：  
   $$\{x_t\} \sim WN(\mu, \sigma^2)$$  
   通常，我们假设 $\mu = 0$。

2. **白噪声的弱平稳性**
    - 均值 $\mathbb{E}[x_t] = \mu$ 是常数。
    - 任意时间点之间的协方差满足：  
      $$Cov\{x_t, x_{t+s}\} = \begin{cases}
      \sigma^2, & \text{if } s = 0, \\
      0, & \text{if } s \neq 0.
      \end{cases}$$  
      这说明，白噪声的协方差只与滞后 $s$ 有关。

3. **自协方差函数**  
   白噪声的自协方差函数可以用 Kronecker delta 函数 $\delta_s$ 表示：  
   $$R(s) = \sigma^2 \delta_s, \quad s \in \mathbb{Z}.$$  
   其中 $\delta_s = 1$ 当 $s = 0$，否则 $\delta_s = 0$。  
   这反映了白噪声的独立性：只有 $s = 0$ 时有协方差，其余滞后时为零。

### Connection between Strict and Weak Stationarity

1. **严格平稳与弱平稳的关系**
    - **严格平稳性**意味着联合分布在时间平移后保持不变。
        - 如果均值和方差存在，严格平稳性也会带来相同的均值和方差。这排除了像柯西分布（Cauchy distribution）这样均值不存在的分布。
          $$ f(x) = \pi^{-1}(1 + x^2)^{-1}, \quad x \in (-\infty, \infty) $$  
          对于柯西分布，$x f(x)$ 不可积，因此均值不存在。
        - 如果二元联合分布相同，则协方差也是相同的（如果协方差存在）。

    - **严格平稳 ⇒ 弱平稳**：  
      当一阶矩（均值）和二阶矩（方差和协方差）都存在时，严格平稳性必然意味着弱平稳性。

2. **弱平稳性的定义回顾**  
   弱平稳性只要求均值、方差和协方差在时间平移后保持不变, 不需要完整的联合分布不变。

---

3. **定义 1.3: 高斯性（Gaussianity）**
    - 一个时间序列 $\{x_t\}$ 被称为**高斯时间序列**，如果任意时间点 $t_1, \dots, t_n$ 的联合分布是**多元正态分布**。
    - **多元正态分布的性质**：  
      多元正态分布完全由其**均值向量**和**协方差矩阵**决定。
      联合概率密度函数只依赖于这两个统计量。

4. **高斯性与平稳性结合**
    - 如果一个时间序列是<span style="background:#FFCCCC;">弱平稳且高斯分布</span>，那么它也是严格平稳的。  
      原因是对于高斯分布，联合分布完全由一阶矩和二阶矩决定。

> [!Tip]
>
> 1. 严格平稳性是指联合分布不会随着时间点的平移而发生相同的变化。
>
> 2. 弱平稳性是指均值、方差和协方差具有平移不变性。
> 3. 严格平稳性和有限的一阶和二阶矩的存在 ⇒ 弱平稳性。
> 4. 弱平稳性和高斯性 ⇒ 严格平稳性。
> 5. 具有有限均值和有限方差的 iid 序列是白噪声过程。
> 6. 高斯白噪声过程是 iid 序列。

### Examples of time series

#### Example: Simulated Data

![Image](https://github.com/pennyzhao1507288/img_store/raw/main/38032%20time_seriess/week1/1.svg)

> [!Question]
>
> **问题：协方差是什么？**
>
> - **白噪声的协方差**：对于白噪声 $\{\epsilon_t\}$，协方差为 0，因为序列的各项独立。
> - **构造序列的协方差**：$\{x_t\}$ 中的协方差会受到 $\epsilon_{t-1}$ 的滞后效应影响。可以通过计算 $Cov\{x_t, x_{t+s}\}$ 确定不同滞后下的协方差。

---

#### Lag Plots(滞后图)

lag=1

![Image](https://github.com/pennyzhao1507288/img_store/raw/main/38032%20time_seriess/week1/2.svg)

1. **线性趋势**  
   在这些图中，可以通过散点画出一条具有正斜率的直线。这说明 $\{x_t\}$ 和 $\{x_{t-1}\}$ 之间存在稳定的线性相关性。

2. **意义**  
   滞后图是用来检查时间序列中是否存在自相关的常用工具：
    - 如果图中点分布随机，说明没有自相关。
    - 如果存在明显的线性关系（如正斜率的直线），说明存在自相关性。

>[!SUCCESS]
>
>- 白噪声序列独立且无自相关，而 $\{x_t\}$ 通过引入滞后项 $\epsilon_{t-1}$，显示了线性相关性。
>- 滞后图提供了直观的方式来判断时间序列的相关性。

---

#### Real Data - Air Passenger Dataset

这是一个经典的时间序列数据集，表示1949年1月到1960年12月期间（共144个月）美国航空客运量（单位：千人）

![Image](https://github.com/pennyzhao1507288/img_store/raw/main/38032%20time_seriess/week1/3.svg)

> [!Tip]
>
> 原始序列和对数变换后的序列均显示增长趋势和周期性季节模式。
>
> 对数变换是处理非平稳性（如变化幅度随时间增大）的一种常用方法。

---

#### Differencing

计算相邻观测值之间的差值，这种方法被称为**差分**

差分是将时间序列转化为平稳序列的另一种常用方法。通过消除趋势或季节性成分，可以使序列满足平稳性假设。

![Image](https://github.com/pennyzhao1507288/img_store/raw/main/38032%20time_seriess/week1/4.svg)


## Properties of the Autocovariance Function

### 定理 1

自协方差函数 $R(s)$ 的性质

对于一个（弱）平稳时间序列 $\{x_t\}$ 的自协方差函数 $R(s)$，满足以下性质：

1. **有界性**：  
   $$|R(s)| \leq R(0), \quad s \in \mathbb{Z},$$  
   其中 $R(0) = Var\{x_t\}$ 表示时间序列的方差。  
   这意味着滞后 $s$ 处的协方差不会超过方差本身。

2. **偶函数**：  
   $$R(s) = R(-s), \quad s \in \mathbb{Z}.$$  
   自协方差函数是关于 $s$ 的偶函数，即对于任意滞后，协方差是对称的。

3. **正半定性**：  
   $$\sum_{i=1}^n \sum_{j=1}^n a_i R(t_i - t_j) a_j \geq 0, \quad \forall t_1, t_2, \dots, t_n \in \mathbb{Z}, \, \forall a_1, a_2, \dots, a_n \in \mathbb{R}.$$  
   这个性质表明，对于任意线性组合的随机变量，其方差非负。

---

#### 协方差作为内积
协方差在处理随机变量的线性组合时，可以看作是一种“内积”：
1. 它在每个参数上是线性的。
2. 例如：  $Cov\{aX + bY, Z\} = aCov\{X, Z\} + bCov\{Y, Z\}.$
   这种线性性质可以扩展到更复杂的随机变量组合。

---

> [!Quote]
>
> [定理1的证明](#定理 1)
>
> <span style="color:#000099;">(a) 有界性</span>
>
> 由定义：  
> $$R(s) = Cov\{x_t, x_{t+s}\}, \quad R(0) = Var\{x_t\}.$$  
> 根据柯西-施瓦兹不等式（Cauchy-Schwarz Inequality）：  
> $$|R(s)| = |Cov\{x_t, x_{t+s}\}| \leq \sqrt{Var\{x_t\}Var\{x_{t+s}\}} = R(0).$$  
> 因此 $R(s)$ 的绝对值被方差限制。
>
> ---
>
> <span style="color:#000099;">(b) 偶函数</span>
>
> 由协方差的对称性：  
> $$R(s) = Cov\{x_t, x_{t+s}\} = Cov\{x_{t+s}, x_t\} = R(-s).$$  
> 这证明了 $R(s)$ 是偶函数。
>
> ---
>
> <span style="color:#000099;">(c) 正半定性</span>
>
> 对任意 $t_1, t_2, \dots, t_n \in \mathbb{Z}$ 和 $a_1, a_2, \dots, a_n \in \mathbb{R}$，考虑如下和：  
> $$\sum_{i=1}^n \sum_{j=1}^n a_i R(t_i - t_j) a_j.$$  
> 将 $R(t_i - t_j)$ 替换为协方差：  
> $$\sum_{i=1}^n \sum_{j=1}^n a_i Cov\{x_{t_i}, x_{t_j}\} a_j.$$  
> 这可以看作随机变量 $\sum_{i=1}^n a_i x_{t_i}$ 的方差：  
> $$Var\left(\sum_{i=1}^n a_i x_{t_i}\right) \geq 0.$$  
> 因此，$R(s)$ 是正半定的。

---

## Autocovariance Matrix and Autocorrelation Function

1. **自协方差矩阵 $R_n$**  
   自协方差矩阵描述了时间序列 $\{x_t\}$ 在不同时间点之间的协方差关系：  
   $$
   R_n = \begin{bmatrix}
   R(0) & R(1) & \cdots & R(n-1) \\
   R(1) & R(0) & \cdots & R(n-2) \\
   \vdots & \vdots & \ddots & \vdots \\
   R(n-1) & R(n-2) & \cdots & R(0)
   \end{bmatrix}
   $$

    - 矩阵的元素是 $R(|i-j|)$，表示滞后为 $|i-j|$ 时的协方差。
    - 它是一个 **Toeplitz 矩阵**（对角线元素相同）。

2. **正半定性与性质 (c)**

    - 矩阵 $R_n$ 是正半定的，这来源于自协方差函数的正半定性质。
    - 任何线性组合的方差总是非负的，保证了正半定性。

3. **自相关函数 $r(s)$**
    - 自相关函数是协方差的标准化形式：  
      $$
      r(s) = Corr\{x_t, x_{t+s}\} = \frac{Cov\{x_t, x_{t+s}\}}{\sqrt{Var\{x_t\}Var\{x_{t+s}\}}} = \frac{R(s)}{R(0)}
      $$
    - 它具有与自协方差函数相同的性质 (a)-(c)。
    - 特别地：$r(0) = 1$，表示与自身完全相关。

---

### Linear Filtering of Time Series

在数字信号分析中,时间序列$\{X\}$被称为信号过程。信号过程的频率是单位时间内该信号过程的振动次数振动次数越大,频率就越高,在一些通讯工程问题里,常常需要设计<u>线性滤波器</u>来滤掉信号过程中的高频噪声

1. **线性滤波的定义**  
   假设 $\{x_t\}$ 是一个平稳时间序列，定义输出序列 $\{y_t\}$ 为：  
   $$
   y_t = \sum_{i=-\infty}^\infty a_i x_{t-i}, \quad t \in \mathbb{Z}
   $$
   其中 $\sum_{i=-\infty}^\infty |a_i| < \infty$ 保证收敛。

    - $\{y_t\}$ 被称为 $\{x_t\}$ 的**滑动平均（moving average）**。
    - 从 $\{x_t\}$ 到 $\{y_t\}$ 的变换被称为**线性滤波（linear filter）**。

2. **线性滤波的例子**
    - **差分**：$y_t = x_t - x_{t-1}$，用于去除趋势。
    - **插值**：$y_t = (x_{t-1} + x_{t+1}) / 2$，用于平滑序列。
    - **加权平均**：$y_t = \frac{x_{t-1}}{2} + \frac{x_{t-2}}{4} + \frac{x_{t-3}}{8} + \cdots$，权重随时间滞后减小。

3. **滤波过程**
    - 滤波器将输入序列 $\{x_t\}$ 与系数 $\{a_i\}$ 进行加权和，生成输出序列 $\{y_t\}$。
    - 系数 $\{a_i\}$ 可以是对称的（如加权平均）或非对称的（如差分）。

---

### Properties of $\{y_t\}$ from $\{x_t\}$

#### 定理 2：线性滤波序列的弱平稳性
假设时间序列 $\{x_t\}$ 是弱平稳的，其自协方差函数为 $R_x(s)$。经过线性滤波生成的序列 $\{y_t\}$ 定义为：  
$$
y_t = \sum_{i=-\infty}^\infty a_i x_{t-i}.
$$
那么 $\{y_t\}$ 也是弱平稳的，其自协方差函数为：  
$$
R_y(s) = \sum_{i=-\infty}^\infty \sum_{j=-\infty}^\infty a_i a_j R_x(s + i - j), \quad s \in \mathbb{Z}.
$$

---

<span style="background:#FFFFCC; color:#000099;">证明：</span>

1. **均值的平稳性**  
   $$
   \mathbb{E}[y_t] = \sum_{i=-\infty}^\infty a_i \mathbb{E}[x_{t-i}] = \mu_x \sum_{i=-\infty}^\infty a_i.
   $$
   均值不依赖于时间 $t$，因此满足均值平稳。

2. **协方差的平稳性**  
   $$
   Cov\{y_t, y_{t+s}\} = Cov\left\{\sum_{i=-\infty}^\infty a_i x_{t-i}, \sum_{j=-\infty}^\infty a_j x_{t+s-j}\right\}.
   $$
   利用线性组合的协方差公式：  
   $$
   Cov\{y_t, y_{t+s}\} = \sum_{i=-\infty}^\infty \sum_{j=-\infty}^\infty a_i a_j Cov\{x_{t-i}, x_{t+s-j}\}.
   $$
   由于 $\{x_t\}$ 是弱平稳的：  
   $$
   Cov\{x_{t-i}, x_{t+s-j}\} = R_x(s + i - j).
   $$
   代入后得到：  
   $$
   R_y(s) = \sum_{i=-\infty}^\infty \sum_{j=-\infty}^\infty a_i a_j R_x(s + i - j).
   $$
   这是 $R_y(s)$ 的定义，说明协方差仅依赖于滞后 $s$，从而 $\{y_t\}$ 是弱平稳的。

---

> [!Example]
>
> <span style="font-size:1.7em; color:#990000;">A Special Case</span>
>
> 特殊情况：$\{x_t\}$ 是白噪声
>
> 假设 $\{x_t\}$ 是白噪声 $\{\epsilon_t\}$，满足 $R_x(s) = \sigma^2$ 当 $s = 0$，否则 $R_x(s) = 0$。此时，$\{y_t\}$ 的自协方差函数为：
> $$R_y(s) = \sigma_x^2 \sum_{i=-\infty}^\infty a_i a_{i+s}.$$
>
> <span style="background:#FFFFCC;">例子 1.2</span>
>
> 定义 $\{x_t\}$ 如下：  
> $$x_t = \epsilon_t + a\epsilon_{t-1}, \quad \{\epsilon_t\} \sim WN(0, \sigma^2).$$  
> 这是一个移动平均模型（Moving Average Model, MA(1)）：
>
> - 系数 $a_0 = 1$，$a_1 = a$，其余系数为 0。
>
> 根据定理 2，自协方差函数为：  
> $$R_x(s) = \sigma^2(a_0a_s + a_1a_{s+1}).$$  
> 通过具体计算：
>
> - 当 $s = 0$：$R_x(0) = \sigma^2(1 + a^2)$，包含 $\epsilon_t$ 和 $\epsilon_{t-1}$ 的影响。
> - 当 $s = 1$ 或 $s = -1$：$R_x(1) = R_x(-1) = \sigma^2 a$，仅与 $\epsilon_t$ 和 $\epsilon_{t-1}$ 之间的相关性有关。
> - 当 $|s| > 1$：$R_x(s) = 0$，因为 $\epsilon_t$ 和 $\epsilon_{t-s}$ 独立。

以上文档的pdf可以用以下链接下载：
<a href="https://github.com/pennyzhao1507288/img_store/raw/main/38032%20time_seriess/week1/time_series_1.pdf" class="download-button" download>
<i class="fas fa-download"></i> 下载文档
</a>

这周tutorial sheet可以用以下链接下载：
<a href="https://github.com/pennyzhao1507288/img_store/raw/main/38032%20time_seriess/week1/tutorial 1.pdf" class="download-button" download>
<i class="fas fa-download"></i> 下载tutorial sheet
</a>
{% progress 100 red 观看结束，感谢观看 %}





