---
title: Transition Rates Forward Equations and Applications
cover: /cover_image/37011_week9_cover.jpg
tags:
   - math
   - Markov Process
categories:
   - Markov Process
abbrlink: e96ac1d8
date: 2024-11-24 05:24:07
---
# &#x1F3C2;{% span blue, Markov Process %}

:zap: {% span green, 问题背景 %}
{% progress 1 gray 开始观看 %}
在离散时间的 Markov 过程（Discrete-Time Markov Chains, DTMC）中，我们可以通过幂运算计算转移矩阵 $P^n$ 来得到状态之间的转移概率。然而，在连续时间 Markov 过程（Continuous-Time Markov Chains, CTMC）中，这种方法不再适用。


因为在连续时间下，我们关注状态之间在非常短的时间 $t$ 内的转移概率，通常记为 $p_{jk}(t)$。这里的 $p_{jk}(t)$ 表示从状态 $j$ 转移到状态 $k$ 的概率。

如果状态空间 $S$ 是无限的，分析会变得非常复杂。因此，我们常假设状态空间 $|S|$ 是有限的，即 $|S| < \infty$。

---

### &#x26FA;{% wavy 定理1：转移率（Transition Rate） %}

定义：对于 $j \neq k \in S$，有  
$$
q_{jk} = q_j r_{jk}\tag{1}
$$

其中：

- $q_{jk}$ 表示从状态 $j$ 到状态 $k$ 的**转移率**（transition rate）。
- $q_j$ 是从状态 $j$ 出发的总体转移率（总转移到其他状态的速率）。
- $r_{jk}$ 是条件概率，即假设转移发生的情况下，从 $j$ 转移到 $k$ 的概率。

---

:pencil:{% span green, 举例解释 %}

假设你在超市里购物，每分钟会随机决定是否从一个货架走到另一个货架（这就是转移率 $q_j$）。如果你选择转移，你可能会按某种概率走向特定的货架（对应 $r_{jk}$）。  
比如：

- 你每分钟离开水果区的概率是 $q_j = 0.5$。
- 如果你离开水果区，你有 80% 的概率去零食区，20% 的概率去饮料区。  
  因此：
- 从水果区到零食区的转移率是 $q_{jk} = 0.5 \times 0.8 = 0.4$。
- 从水果区到饮料区的转移率是 $q_{jk} = 0.5 \times 0.2 = 0.1$。

---

*<u>接下来在讲解新定理之前我们来了解以下几个知识点：</u>*

1. Notation $o(h)$ 的定义

如果满足以下条件：  
$$
\frac{g(h)}{h} \to 0 \quad \text{当 } h \to 0。
$$


则函数 $g(h)$ 是 $o(h)$。这意味着，当 $h$ 变得非常小时，$g(h)$ 的增长速度比 $h$ 快速趋于 0。

:chestnut:例子：
- $h^2 + 3h^3$ 是 $o(h)$，因为 $\frac{h^2 + 3h^3}{h} = h + 3h^2$，当 $h \to 0$ 时，趋于 0。
- $2h + h^2$ 不是 $o(h)$，因为 $\frac{2h + h^2}{h} = 2 + h$，当 $h \to 0$ 时，不趋于 0。

---

2. $e^{-ah}$ 的近似

通过泰勒展开，我们有：
$$
e^{-ah} = 1 - ah + o(h)
$$

这表示在 $h$ 很小时，指数函数可以用 $1 - ah$ 来近似。

:chestnut:例子：
想象你有一个非常小的时间间隔 $h$，假设你在 $h$ 时间内完成某事件的概率可以通过指数分布建模。通过这个近似，可以快速估计转移概率。

---

3. 概率 $P(T \leq h)$

如果 $T \sim \text{exp}(\alpha)$，即 $T$ 遵循参数为 $\alpha$ 的指数分布，则有：  
$$
P(T \leq h) = 1 - e^{-\alpha h} = \alpha h + o(h)
$$

这表明，在非常短的时间间隔 $h$ 内，事件发生的概率接近于 $\alpha h$。


{% note success flat %}
:beers:生活中的例子：
假设你观察公交车到站的时间间隔（符合指数分布），如果平均每分钟有 1 辆车到达（$\alpha = 1$），那么在非常短的时间 $h = 0.1$ 分钟内车到达的概率大约是 $\alpha h = 0.1$（即 10%）。
{% endnote %}

---

*好了，了解之后咱们来看一下这个定理*

### &#x26FA;{% wavy 定理2 %}

假设状态空间 $|S| = N < \infty$，以下成立：
1.
$$
p_{jj}(h) = 1 - q_j h + o(h)
$$
2.
$$
p_{jk}(h) = q_{jk} h + o(h), \quad \forall j \neq k
$$

其中：
- $p_{jj}(h)$ 是状态 $j$ 在时间 $h$ 内保持不变的概率。
- $p_{jk}(h)$ 是在时间 $h$ 内从状态 $j$ 转移到状态 $k$ 的概率。

我们称 $q_{jk}h$ 和 $1 - q_jh$ 为**无限小转移概率（infinitesimal transition probabilities）**。

---

{% span red, 证明部分 %}

1. **定义事件**

   - $A(h)$：在时间间隔 $(0, h]$ 内没有发生转移。
   - $B(h)$：在时间间隔 $(0, h]$ 内发生至少两次转移。

2. **计算 $p_{jj}(h)$**  
   $$
   p_{jj}(h) = P(X(h) = j, A(h) \mid X(0) = j) + P(X(h) = j, B(h) \mid X(0) = j)
   $$

   对于第一项，根据指数分布的性质，有：
   $$
   P(A(h) \mid X(0) = j) = e^{-q_j h} = 1 - q_j h + o(h)
   $$

   第二项是 $P(B(h) \mid X(0) = j)$，可以证明其为 $o(h)$，因为事件 $B(h)$ 表示发生至少两次转移，其概率随着 $h$ 减小时更快趋于 0。

   所以，结合两项：
   $$
   p_{jj}(h) = 1 - q_j h + o(h)
   $$

3. **计算 $p_{jk}(h)$ （$j \neq k$）**  从状态 $j$ 跳转到状态 $k$ 的概率满足：
   $$
   p_{jk}(h) = P(\text{从 $j$ 直接跳到 $k$ 在时间 $h$ 内}) + P(B(h) \mid X(0) = j)
   $$

   第一项是 $q_{jk} h + o(h)$，第二项同样为 $o(h)$，因此：
   $$
   p_{jk}(h) = q_{jk} h + o(h)
   $$

---

生成矩阵 $Q$

定义生成矩阵 $Q$：
$$
Q = (q_{jk}), \quad q_{jj} = -q_j, \quad q_{jk} = q_j r_{jk}, \quad \forall j \neq k
$$

生成矩阵的性质：
- 每一行的元素和为 0：
  $$
  \sum_{k \in S} q_{jk} = 0
  $$
  证明如下：
  $$
  \sum_{k \in S} q_{jk} = q_{jj} + \sum_{k \neq j} q_{jk} = -q_j + q_j \sum_{k \neq j} r_{jk} = -q_j + q_j = 0
  $$

- $q_{jj} < 0$，$q_{jk} \geq 0 \, (j \neq k)$。



### &#x26FA;{% wavy 定理3：Forward Equations %}

对于有限状态空间 $|S| < \infty$，转移概率矩阵 $P(t)$ 满足：
$$
P'(t) = P(t)Q \tag{3}
$$
其中：
- $P'(t)$ 表示转移矩阵 $P(t)$ 中每个元素对时间 $t$ 的导数。
- $Q$ 是生成矩阵。

这个公式被称为正向方程（Forward Equations）。

{% span red, 证明部分 %}

1. 目标  
   我们需要证明每个元素 $p_{jk}(t)$ 满足：
   $$
   p'_{jk}(t) = \sum_{r \in S} p_{jr}(t)q_{rk}.
   $$

2. 右导数的计算  
   利用 2.5 中的结果，对任意 $j, k$：
   $$
   \lim_{h \downarrow 0} \frac{p_{jk}(t+h) - p_{jk}(t)}{h}
   $$
   由转移概率的定义：
   $$
   p_{jk}(t+h) = \sum_{r \in S} p_{jr}(t) p_{rk}(h)
   $$
   因此：
   $$
   \lim_{h \downarrow 0} \frac{p_{jk}(t+h) - p_{jk}(t)}{h} = \lim_{h \downarrow 0} \frac{\sum_{r \in S} p_{jr}(t)p_{rk}(h) - p_{jk}(t)}{h}
   $$

3. **分解计算**  
   将 $r = k$ 和 $r \neq k$ 的部分拆分：
   $$
   = \lim_{h \downarrow 0} \frac{\sum_{r \neq k} p_{jr}(t)p_{rk}(h) + p_{jk}(t)p_{kk}(h) - p_{jk}(t)}{h}.
   $$
   使用 [定理2](#定理2) 的结果：
   - 对于 $r \neq k$，$p_{rk}(h) = q_{rk}h + o(h)$。
   - 对于 $r = k$，$p_{kk}(h) = 1 - q_k h + o(h)$。
     替换后得到：
     $$
     = \lim_{h \downarrow 0} \frac{\sum_{r \neq k} p_{jr}(t)(q_{rk}h + o(h)) + p_{jk}(t)(1 - q_k h + o(h)) - p_{jk}(t)}{h}.
     $$

4. **化简**  
   将 $h$ 提取出来：
   $$
   = \lim_{h \downarrow 0} \sum_{r \neq k} p_{jr}(t)q_{rk} + p_{jk}(t)(-q_k) + o(h).
   $$
   当 $h \to 0$ 时，$o(h)$ 消失，最终得到：
   $$
   p'_{jk}(t) = \sum_{r \in S} p_{jr}(t)q_{rk}.
   $$

5. **左导数的计算**  
   同理，对于左导数：
   $$
   \lim_{h \downarrow 0} \frac{p_{jk}(t) - p_{jk}(t-h)}{h}
   $$
   可使用类似的方法计算，最终也得到：
   $$
   p'_{jk}(t) = \sum_{r \in S} p_{jr}(t)q_{rk}.
   $$

6. **导数的存在性**  
   由于左导数和右导数相等，因此 $p_{jk}(t)$ 的导数存在，并且满足：
   $$
   p'_{jk}(t) = \sum_{r \in S} p_{jr}(t)q_{rk}.
   $$

---


{% note blue 'fas fa-bullhorn' simple%}
:bulb:总结
通过以上证明，我们验证了转移概率矩阵的导数满足：
$$
P'(t) = P(t)Q,
$$
其中，生成矩阵 $Q$ 表示每个状态之间的转移速率。
{% endnote %}


#### &#x1FA90;{% u Example: Flip-Flop Circuit %}

我们分析一个简单的两状态 Markov 过程，它的生成矩阵 $Q$ 和转移概率矩阵 $P(t)$ 满足 $P'(t) = P(t)Q$。
{% span green,以下是他的推导过程： %}


:icecream: 定义生成矩阵 $Q$

根据题意：
- $q_{00} = -q_0 = -\lambda$
- $q_{01} = 1 \cdot \lambda = \lambda$
- $q_{10} = 1 \cdot \mu = \mu$
- $q_{11} = -q_1 = -\mu$

因此生成矩阵为：
$$
Q =
\begin{pmatrix}
-\lambda & \lambda \\
\mu & -\mu
\end{pmatrix}
$$

---

:icecream:求解 $P'(t) = P(t)Q$

假设转移概率矩阵 $P(t)$ 的形式为：
$$
P(t) =
\begin{pmatrix}
p_{00}(t) & p_{01}(t) \\
p_{10}(t) & p_{11}(t)
\end{pmatrix}.
$$

由 $P'(t) = P(t)Q$ 可得：
$$
\begin{pmatrix}
p'_{00}(t) & p'_{01}(t) \\
p'_{10}(t) & p'_{11}(t)
\end{pmatrix}
=
\begin{pmatrix}
p_{00}(t) & p_{01}(t) \\
p_{10}(t) & p_{11}(t)
\end{pmatrix}
\begin{pmatrix}
-\lambda & \lambda \\
\mu & -\mu
\end{pmatrix}.
$$

通过矩阵乘法，对每个元素的导数进行计算，例如，取矩阵第一行第一列（row 1 × col 1）：
$$
p'_{00}(t) = -\lambda p_{00}(t) + \mu p_{01}(t).
$$

同理，其它导数公式可以分别计算得出。

---
:icecream:替换 $p_{01}(t) = 1 - p_{00}(t)$


注意到 $p_{00}(t) + p_{01}(t) = 1$，所以 $p_{01}(t) = 1 - p_{00}(t)$。代入上述导数表达式：
$$
p'_{00}(t) = -\lambda p_{00}(t) + \mu (1 - p_{00}(t)).
$$
展开化简：
$$
p'_{00}(t) = -(\lambda + \mu)p_{00}(t) + \mu.
$$

---

:icecream:求解微分方程

该方程为一阶线性微分方程，可以通过变量分离或积分因子法求解。设初始条件：
$$
p_{00}(0) = P(X(0) = 0 \mid X(0) = 0) = 1.
$$

解得：
$$
p_{00}(t) = \frac{1}{\lambda + \mu}\{\mu + \lambda e^{-(\lambda + \mu)t}\}.
$$

---

:icecream:求 $p_{01}(t)$

由 $p_{01}(t) = 1 - p_{00}(t)$，代入 $p_{00}(t)$：
$$
p_{01}(t) = 1 - \frac{1}{\lambda + \mu}\{\mu + \lambda e^{-(\lambda + \mu)t}\}.
$$
化简后：
$$
p_{01}(t) = \frac{1}{\lambda + \mu}\{\lambda - \lambda e^{-(\lambda + \mu)t}\}.
$$

---

:icecream:对称性与其他元素

类似地，可以求得：
$$
p_{10}(t) = \frac{1}{\lambda + \mu}\{\mu - \mu e^{-(\lambda + \mu)t}\},
$$
$$
p_{11}(t) = \frac{1}{\lambda + \mu}\{\lambda + \mu e^{-(\lambda + \mu)t}\}.
$$

---

:icecream:长时间的极限行为

当 $t \to \infty$，指数项 $e^{-(\lambda + \mu)t} \to 0$，此时：
$$
p_{00}(\infty) = \frac{\mu}{\lambda + \mu}, \quad p_{01}(\infty) = \frac{\lambda}{\lambda + \mu}.
$$

这说明，系统达到平稳状态时，转移概率只依赖于生成矩阵的参数 $\lambda$ 和 $\mu$，与初始状态无关。


以上文档的pdf可以用以下链接下载：
<a href="https://github.com/pennyzhao1507288/img_store/raw/main/37011%20markov%20process/37011_week9.pdf" class="download-button" download>
<i class="fas fa-download"></i> 下载文档
</a>


{% progress 100 red 观看结束，感谢观看 %}
