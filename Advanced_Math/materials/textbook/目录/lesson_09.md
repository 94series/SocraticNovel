🔧 **工具调用:** project_knowledge_search

> 💭 思考中...

🔧 **工具调用:** project_knowledge_search

> 💭 思考中...

🔧 **工具调用:** bash_tool

```bash
grep -n "average value\|Average Value\|mean value theorem for integral\|area between\|improper integral\|Improper Integral\|convergent\|divergent" /mnt/project/calculus.md | head -60
```

> 💭 思考中...

🔧 **工具调用:** bash_tool

```bash
grep -n "logarithmic mean\|log mean\|LMTD\|heat exchanger" /mnt/project/calculus.md | head -20
```

> 💭 思考中...

🔧 **工具调用:** view

Read the average value of a function section in detail

> 💭 思考中...

🔧 **工具调用:** view

Read the improper integrals section

> 💭 思考中...

Now I have sufficient context from the Stewart textbook. Let me compose this lesson.

```markdown
# 第九课 · 积分的应用——平均值、累积量、对数平均与无穷的驯服

---

> **"积分不是面积计算器。它是自然用来做簿记的语言。"**

---

## 前置知识

进入这一课之前，请确认：

- **定积分的定义与计算**（第七课）：你知道 $\int_a^b f(x)\,dx$ 是什么意思——Riemann 和的极限——并且能用微积分基本定理算出来。
- **基本的积分技巧**（第八课）：换元法、分部积分。本课不需要太花哨的技巧，但你得能算 $\int e^{-x}\,dx$、$\int \frac{1}{x}\,dx$、$\int x\,e^{-x}\,dx$ 这类基本积分。
- **导数作为变化率的理解**（第六课）：特别是"导数是局部行为，积分是全局累积"这一对偶关系。
- **对数函数的基本性质**：$\ln(a/b) = \ln a - \ln b$。第三部分推导 LMTD 时会反复用到。

---

## 引言：积分是一台求和机器

第七课里，我们把积分理解为"面积"。这个理解没有错，但它只是冰山一角。积分的本质是**对连续变化量的累加**——面积只不过是其中最直观的一个特例。

这一课我们将看到积分还能做什么：

- 对空间变化的浓度求**平均值**（你的导师问"膜内平均浓度是多少"，答案就是一个积分）。
- 对随时间变化的通量求**累积产水量**（通量乘以时间的积分就是总产水体积）。
- 推导化工和膜科学中最常见的公式之一——**对数平均浓度差（LMTD）**，揭示为什么它不是简单的算术平均。
- 处理**无穷区间上的积分**，发现一个令人惊奇的事实：无穷长的区域可以有有限的面积。

让我们开始。

---

## 第一部分：函数的平均值——当"平均"遇上连续

### 1.1 离散平均的回忆

你知道怎么算有限个数的平均值：

$$
\bar{y} = \frac{y_1 + y_2 + \cdots + y_n}{n}
$$

比如测了 $n = 5$ 个点的浓度 $c_1, c_2, c_3, c_4, c_5$，平均浓度就是它们的和除以 $5$。没有任何困难。

但是，如果浓度在空间上是**连续变化的**呢？在膜内部，从进料侧到渗透侧，溶质浓度 $c(x)$ 是位置 $x$ 的连续函数。你不是只有 $5$ 个测量点——原则上，每个位置都有一个浓度值，有无穷多个。怎么"平均"无穷多个数？

### 1.2 从离散到连续：积分的登场

让我们沿着一条自然的思路走。

假设膜的厚度为 $L$，浓度分布为 $c(x)$，$x \in [0, L]$。把膜厚 $[0, L]$ 等分成 $n$ 份，每份宽度 $\Delta x = L/n$。在第 $i$ 个小区间里取一个采样点 $x_i^*$，那里的浓度是 $c(x_i^*)$。

这 $n$ 个采样值的算术平均：

$$
\bar{c}_n = \frac{c(x_1^*) + c(x_2^*) + \cdots + c(x_n^*)}{n}
$$

因为 $n = L / \Delta x$，所以：

$$
\bar{c}_n = \frac{1}{L} \sum_{i=1}^{n} c(x_i^*) \, \Delta x
$$

仔细看这个表达式——右边的求和正是 $c(x)$ 在 $[0, L]$ 上的一个 **Riemann 和**！当 $n \to \infty$（分得越来越细），Riemann 和趋向定积分：

$$
\bar{c} = \lim_{n \to \infty} \bar{c}_n = \frac{1}{L} \int_0^L c(x) \, dx
$$

这就是连续函数平均值的定义。没有任何"规定"或"约定"——它是离散平均概念在连续情形下的唯一自然推广。

> **函数平均值的定义**：函数 $f$ 在区间 $[a, b]$ 上的平均值为
>
> $$
> f_{\text{avg}} = \frac{1}{b - a} \int_a^b f(x) \, dx
> $$

结构是透明的：**积分求总量，除以区间长度得到平均值。** 就像离散情况下"总和除以个数"一样。

### 1.3 几何直觉

对于正值函数 $f$，$\int_a^b f(x)\,dx$ 是曲线下方的面积。平均值 $f_{\text{avg}}$ 是这样一个高度：以它为高、$[a, b]$ 为底的矩形面积恰好等于曲线下方的面积。

```

f(x)
|        ___
|       /   \        曲线下面积 = 矩形面积
|      /     ___
|     /          
|----/--+--+--+-------- f_avg（平均值水平线）
|   /   |  |  |    
|  /    |  |  |     
| /     |  |  |      
+----------------------------
a                      b

````

想象一下你在做蛋糕：曲线下方的面积是蛋糕上面不均匀的奶油。平均值就是把奶油摊平后的均匀高度。"积分再除以区间长度"就是在做"摊平"这个动作。

### 1.4 积分中值定理

这里有一个优美的定理，值得你知道：

> **积分中值定理（Mean Value Theorem for Integrals）**：若 $f$ 在 $[a, b]$ 上连续，则存在某个 $c \in [a, b]$，使得
>
> $$
> f(c) = f_{\text{avg}} = \frac{1}{b-a}\int_a^b f(x)\,dx
> $$

换句话说，连续函数**一定**在某个点达到它的平均值。这不奇怪——如果你在某些地方高于平均，某些地方低于平均，连续性保证你必须在某处**恰好等于**平均。这本质上就是介值定理的推论。

在膜科学中这意味着：膜内浓度分布 $c(x)$ 无论多复杂，总存在某个位置 $x = c$，那里的局部浓度恰好等于全膜平均浓度。

### 1.5 例题：膜内线性浓度分布的平均

**问题**：在最简单的膜传质模型中，假设溶质浓度从进料侧 $c_f$ 线性下降到渗透侧 $c_p$：

$$
c(x) = c_f + \frac{c_p - c_f}{L} \, x, \quad x \in [0, L]
$$

求膜内平均浓度。

**解**：

$$
\bar{c} = \frac{1}{L} \int_0^L \left[ c_f + \frac{c_p - c_f}{L} \, x \right] dx
$$

$$
= \frac{1}{L} \left[ c_f \, x + \frac{c_p - c_f}{L} \cdot \frac{x^2}{2} \right]_0^L
$$

$$
= \frac{1}{L} \left[ c_f \, L + \frac{c_p - c_f}{L} \cdot \frac{L^2}{2} \right]
$$

$$
= c_f + \frac{c_p - c_f}{2} = \frac{c_f + c_p}{2}
$$

线性分布的平均值就是两端的**算术平均**。

这看上去理所当然。但请记住这个结论——当我们在第三部分遇到**指数型**浓度分布时，平均值将不再是算术平均，而是某种更深刻的东西。线性情况的简单性反而是例外，而非规则。

---

## 第二部分：面积与累积量

### 2.1 两条曲线之间的面积

如果 $f(x) \geq g(x)$ 在 $[a, b]$ 上成立，那么 $y = f(x)$ 和 $y = g(x)$ 之间区域的面积是：

$$
A = \int_a^b [f(x) - g(x)] \, dx
$$

道理很直接：$f(x) - g(x)$ 是在位置 $x$ 处两条曲线之间的"竖直距离"，沿 $x$ 方向积分就累加出了总面积。

如果两条曲线交叉——有时候 $f$ 在上面，有时候 $g$ 在上面——你需要分段处理，或者直接取绝对值：

$$
A = \int_a^b |f(x) - g(x)| \, dx
$$

### 2.2 物理量的时间累积

积分最自然的物理含义不是"面积"，而是"累积"。

假设膜的水通量 $J(t)$ 随时间变化（比如因为膜污染，通量逐渐下降）。那么从时刻 $t_1$ 到 $t_2$，**单位膜面积的总产水体积**是：

$$
V = \int_{t_1}^{t_2} J(t) \, dt
$$

这就是"速率 $\times$ 时间 = 总量"在连续变化情形下的推广。如果通量恒定 $J_0$，积分给出 $V = J_0(t_2 - t_1)$——回到了简单的乘法。但通量通常不是恒定的，这时你必须积分。

**例题**：膜污染导致通量按指数衰减：

$$
J(t) = J_0 \, e^{-\beta t}
$$

其中 $J_0$ 是初始通量，$\beta > 0$ 是污染速率常数。在 $[0, T]$ 时间内总产水量为：

$$
V = \int_0^T J_0 \, e^{-\beta t} \, dt = J_0 \left[-\frac{1}{\beta} e^{-\beta t}\right]_0^T = \frac{J_0}{\beta}\left(1 - e^{-\beta T}\right)
$$

**几个值得注意的事情**：

- 当 $T$ 很小时（$\beta T \ll 1$），$e^{-\beta T} \approx 1 - \beta T$（线性近似！第六课的内容），所以 $V \approx J_0 T$——短时间内通量还没怎么衰减，总产量近似"初始通量乘以时间"。
- 当 $T \to \infty$ 时，$V \to J_0 / \beta$。即使运行无穷长时间，总产水量也是有限的！这就是我们在第四部分要讨论的反常积分。物理上很好理解：通量衰减得足够快，以至于后期几乎不再产水。
- $1/\beta$ 是特征时间尺度——它同时决定了通量衰减的快慢和最终总产量的大小。

### 2.3 平均通量与累积量的关系

在 $[0, T]$ 时间段内的平均通量：

$$
J_{\text{avg}} = \frac{1}{T} \int_0^T J(t)\,dt = \frac{V}{T}
$$

所以 $V = J_{\text{avg}} \cdot T$。**总产量 = 平均通量 $\times$ 时间**。这等式两边都用到了积分——左边直接积分算总产量，右边先积分求平均值再乘以时间——但结果必然一致。这种一致性不是巧合，它是平均值定义的直接结果。

把上面指数衰减的例子代进来：

$$
J_{\text{avg}} = \frac{J_0}{\beta T}\left(1 - e^{-\beta T}\right)
$$

当 $T$ 很大时，$e^{-\beta T} \to 0$，$J_{\text{avg}} \approx J_0 / (\beta T) \to 0$。平均通量趋于零——后期的大量"零产出"拖低了平均值。这就是为什么实际操作中膜要定期清洗或更换，而不是"一直跑下去"。

---

## 第三部分：对数平均浓度差（LMTD）的完整推导

### 3.1 问题的提出

这是本课的核心战役。

在化工、膜科学和换热器设计中，有一个无处不在的量叫做**对数平均浓度差**（Logarithmic Mean Temperature/Concentration Difference，LMTD 或 LMCD）。每一本化工原理教科书都给出这个公式，但绝大多数教科书只是展示结论而跳过推导，或者用一两行含糊的文字带过。

今天我们要从头推导它。每一步都写清楚。当你看完这个推导，你不仅知道公式长什么样，更知道它**为什么**必须是那个样子——任何其他形式都是错的。

### 3.2 物理场景

考虑一个简单的膜分离器（或换热器），料液沿长度方向 $z \in [0, L]$ 流动，跨膜传质的驱动力是浓度差 $\Delta c(z) = c(z) - c_p$，其中 $c(z)$ 是料液侧浓度（沿 $z$ 变化），$c_p$ 是渗透侧浓度（为简化分析，假设为常数——比如渗透侧有大量稀释液快速带走溶质）。

局部的溶质传质通量正比于局部浓度差：

$$
j(z) = K \, \Delta c(z) = K \, [c(z) - c_p]
$$

其中 $K$ 是总传质系数。

**核心问题**：整个膜器的总传质速率 $\dot{N}$ 是多少？

一个天真的想法是：用两端浓度差的算术平均来代替"有效平均浓度差"，即

$$
\Delta c_{\text{am}} = \frac{\Delta c_1 + \Delta c_2}{2}
$$

其中 $\Delta c_1 = \Delta c(0)$ 是入口端浓度差，$\Delta c_2 = \Delta c(L)$ 是出口端浓度差。

但这**是错的**。为什么？因为浓度差沿长度的变化不是线性的，而是**指数型**的。让我来证明这一点。

### 3.3 浓度沿长度的分布规律

在膜器的一个微元 $dz$ 内，料液失去的溶质量等于跨膜传质量。做一个质量衡算：

$$
Q \, dc = -K \, [c(z) - c_p] \cdot W \, dz
$$

这里 $Q$ 是料液体积流量（假设近似不变），$W$ 是膜的宽度（或周长），$dc$ 是浓度在 $dz$ 内的变化。$W\,dz$ 是微元膜面积 $dA$。

令 $\Delta c(z) = c(z) - c_p$。因为 $c_p$ 是常数，$d(\Delta c) = dc$。所以：

$$
Q \, d(\Delta c) = -K \, \Delta c \cdot W \, dz
$$

分离变量：

$$
\frac{d(\Delta c)}{\Delta c} = -\frac{KW}{Q} \, dz
$$

两边积分，从 $z = 0$ 到 $z$：

$$
\ln \frac{\Delta c(z)}{\Delta c_1} = -\frac{KW}{Q} \, z
$$

即：

$$
\Delta c(z) = \Delta c_1 \, \exp\!\left(-\frac{KW}{Q} \, z\right)
$$

**浓度差沿长度指数衰减。** 这就是为什么算术平均不对——你不能用线性平均去代表一个指数变化的量。

令 $\lambda = KW/Q$ 以简化记号。在出口端 $z = L$：

$$
\Delta c_2 = \Delta c_1 \, e^{-\lambda L}
$$

所以：

$$
\lambda L = \ln \frac{\Delta c_1}{\Delta c_2}
$$

这个关系后面会用到。

### 3.4 总传质速率的积分计算

总传质速率是局部通量沿整个膜面积的积分：

$$
\dot{N} = \int_0^L K \, \Delta c(z) \, W \, dz = KW \int_0^L \Delta c_1 \, e^{-\lambda z} \, dz
$$

计算这个积分：

$$
\dot{N} = KW \, \Delta c_1 \int_0^L e^{-\lambda z} \, dz = KW \, \Delta c_1 \left[-\frac{1}{\lambda} e^{-\lambda z}\right]_0^L
$$

$$
= \frac{KW \, \Delta c_1}{\lambda} \left(1 - e^{-\lambda L}\right)
$$

现在用 $\Delta c_2 = \Delta c_1 \, e^{-\lambda L}$ 和 $\lambda = KW/Q$ 替换：

$$
1 - e^{-\lambda L} = 1 - \frac{\Delta c_2}{\Delta c_1} = \frac{\Delta c_1 - \Delta c_2}{\Delta c_1}
$$

所以：

$$
\dot{N} = \frac{KW \, \Delta c_1}{\lambda} \cdot \frac{\Delta c_1 - \Delta c_2}{\Delta c_1} = \frac{KW}{\lambda} (\Delta c_1 - \Delta c_2)
$$

又因为 $\lambda L = \ln(\Delta c_1 / \Delta c_2)$，即 $\frac{1}{\lambda} = \frac{L}{\ln(\Delta c_1 / \Delta c_2)}$，代入：

$$
\dot{N} = KW \cdot \frac{L}{\ln(\Delta c_1 / \Delta c_2)} \cdot (\Delta c_1 - \Delta c_2)
$$

注意到 $A = WL$ 是总膜面积，所以：

$$
\boxed{\dot{N} = K \, A \, \frac{\Delta c_1 - \Delta c_2}{\ln(\Delta c_1 / \Delta c_2)}}
$$

定义**对数平均浓度差**：

$$
\boxed{\Delta c_{\text{lm}} = \frac{\Delta c_1 - \Delta c_2}{\ln(\Delta c_1 / \Delta c_2)}}
$$

则总传质速率的优雅形式是：

$$
\dot{N} = K \, A \, \Delta c_{\text{lm}}
$$

这就是 LMTD（或 LMCD）公式。

### 3.5 为什么不是算术平均？——深层解读

让我们停下来想想刚才到底发生了什么。

我们想把一个积分 $\int_0^L K \, \Delta c(z) \, W \, dz$ 写成 $K \cdot A \cdot \overline{\Delta c}$ 的形式，其中 $\overline{\Delta c}$ 是某个"等效平均浓度差"。而这个平均值，按照第一部分的定义，正是：

$$
\overline{\Delta c} = \frac{1}{L} \int_0^L \Delta c(z) \, dz
$$

我们刚才已经算出了这个积分。把上面的 $\dot{N}$ 表达式除以 $KA = KWL$：

$$
\overline{\Delta c} = \frac{\dot{N}}{KA} = \frac{\Delta c_1 - \Delta c_2}{\ln(\Delta c_1 / \Delta c_2)} = \Delta c_{\text{lm}}
$$

所以 LMTD 就是**指数型浓度差分布的真正平均值**。

现在回忆第一部分：线性分布的平均值是算术平均。指数分布的平均值是对数平均。分布的形状决定了平均值的形式。用算术平均去代替对数平均，就像用线性函数去近似指数函数——在 $\Delta c_1 \approx \Delta c_2$ 的时候还行（指数函数在短区间上近似线性），但两端差别大的时候误差很大。

我们可以验证这一点。令 $r = \Delta c_1 / \Delta c_2$，则：

$$
\Delta c_{\text{lm}} = \Delta c_2 \cdot \frac{r - 1}{\ln r}, \quad \Delta c_{\text{am}} = \Delta c_2 \cdot \frac{r + 1}{2}
$$

当 $r = 1$（两端浓度差相等），两者都退化为 $\Delta c_1 = \Delta c_2$（用 L'Hôpital 法则可以验证 $\lim_{r \to 1} \frac{r-1}{\ln r} = 1$——第六课的工具在这里又派上用场了）。

当 $r = 10$（一端浓度差是另一端的 10 倍）：

$$
\frac{\Delta c_{\text{am}}}{\Delta c_{\text{lm}}} = \frac{(r+1)/2}{(r-1)/\ln r} = \frac{(r+1)\ln r}{2(r-1)} = \frac{11 \times \ln 10}{2 \times 9} = \frac{11 \times 2.303}{18} \approx 1.41
$$

算术平均比对数平均高出 $41\%$！如果你用算术平均去设计膜器，你会严重高估传质速率——这意味着你的膜面积不够，产品不达标。

### 3.6 一个优雅的不等式

事实上，对于任何 $a, b > 0$ 且 $a \neq b$，有：

$$
\text{调和平均} \leq \text{几何平均} \leq \text{对数平均} \leq \text{算术平均}
$$

即：

$$
\frac{2ab}{a+b} \leq \sqrt{ab} \leq \frac{a - b}{\ln a - \ln b} \leq \frac{a + b}{2}
$$

对数平均总是夹在几何平均和算术平均之间。这个不等式链在数学上是优美的，在工程上是实用的——它告诉你用算术平均**总是高估**，用几何平均**总是低估**，而对数平均才是真相。

> **动手验证**：用 Python 画出 $r = \Delta c_1 / \Delta c_2$ 从 $1$ 到 $20$ 时，算术平均、对数平均和几何平均（都归一化到 $\Delta c_2$）的比较图。你会看到三条曲线从同一点出发，然后逐渐分开。
>
> ```python
> import numpy as np
> import matplotlib.pyplot as plt
>
> r = np.linspace(1.01, 20, 500)
> am = (r + 1) / 2           # 算术平均 / Δc₂
> lm = (r - 1) / np.log(r)   # 对数平均 / Δc₂
> gm = np.sqrt(r)             # 几何平均 / Δc₂
>
> plt.figure(figsize=(8, 5))
> plt.plot(r, am, 'r-', label='算术平均', linewidth=2)
> plt.plot(r, lm, 'b-', label='对数平均 (LMTD)', linewidth=2)
> plt.plot(r, gm, 'g--', label='几何平均', linewidth=2)
> plt.xlabel(r'$r = \Delta c_1 / \Delta c_2$')
> plt.ylabel(r'平均浓度差 / $\Delta c_2$')
> plt.title('三种平均的比较')
> plt.legend()
> plt.grid(True, alpha=0.3)
> plt.show()
> ```

---

## 第四部分：反常积分——驯服无穷

### 4.1 问题的起源

在定积分 $\int_a^b f(x)\,dx$ 的定义中，我们假设了两件事：（1）积分区间 $[a, b]$ 是**有限的**；（2）被积函数 $f$ 在 $[a, b]$ 上是**有界的**。

当这两个条件之一被违反时，我们进入了**反常积分**（improper integral）的领地。

这听起来像是数学家的边缘趣味，但事实上反常积分在科学中无处不在。你在第二部分已经遇到了一个——当膜运行时间 $T \to \infty$ 时，$\int_0^\infty J_0 e^{-\beta t}\,dt$ 就是一个反常积分，而它的值是有限的。概率论中的正态分布 $\int_{-\infty}^{\infty} e^{-x^2/2}\,dx = \sqrt{2\pi}$ 也是反常积分。Laplace 变换、Fourier 变换——这些在膜科学建模中常用的工具——全都建立在反常积分之上。

### 4.2 第一型：无穷积分区间

**定义**：

$$
\int_a^{\infty} f(x)\,dx = \lim_{t \to \infty} \int_a^t f(x)\,dx
$$

如果这个极限存在（为有限数），我们说反常积分**收敛**；如果极限不存在或为 $\pm\infty$，则**发散**。

类似地定义 $\int_{-\infty}^b$ 和 $\int_{-\infty}^{\infty}$。

**关键例题：$p$-积分判别**

$$
\int_1^{\infty} \frac{1}{x^p} \, dx
$$

这是一切反常积分理论的试金石。

当 $p \neq 1$ 时：

$$
\int_1^t \frac{1}{x^p}\,dx = \left[\frac{x^{1-p}}{1-p}\right]_1^t = \frac{t^{1-p} - 1}{1-p}
$$

- 若 $p > 1$，则 $1 - p < 0$，$t^{1-p} \to 0$，极限 $= \frac{1}{p-1}$。**收敛。**
- 若 $p < 1$，则 $1 - p > 0$，$t^{1-p} \to \infty$。**发散。**

当 $p = 1$ 时：

$$
\int_1^t \frac{1}{x}\,dx = \ln t \to \infty
$$

**发散。**

总结：

$$
\int_1^{\infty} \frac{dx}{x^p} \begin{cases} = \dfrac{1}{p-1}, & p > 1 \\[6pt] \text{发散}, & p \leq 1 \end{cases}
$$

$p = 1$ 是分水岭。$1/x$ 衰减得"刚好不够快"，面积刚好是无穷大。$1/x^2$ 衰减得更快一点点，面积就变成了有限的。这个临界行为值得深思——自然界中很多"刚好在边界上"的现象（比如某些分形维度恰好等于临界值的情况）都有类似的数学结构。

### 4.3 指数衰减：物理学家最好的朋友

让我们严格计算那个膜通量的例子：

$$
\int_0^{\infty} J_0 \, e^{-\beta t} \, dt = J_0 \lim_{T \to \infty} \left[-\frac{1}{\beta} e^{-\beta t}\right]_0^T = J_0 \lim_{T \to \infty} \frac{1}{\beta}(1 - e^{-\beta T}) = \frac{J_0}{\beta}
$$

指数衰减函数在 $[0, \infty)$ 上的积分**总是收敛的**。更一般地，对于任何 $\beta > 0$：

$$
\int_0^{\infty} e^{-\beta t} \, dt = \frac{1}{\beta}
$$

这个结果你应该记住——它在 Laplace 变换、概率分布、传递函数分析中反复出现。

为什么指数衰减的积分是有限的？直觉是这样的：指数函数衰减得**极其迅速**——比任何幂函数都快。虽然"尾巴"无限长，但尾巴的高度以指数速度趋近于零，使得总面积保持有限。你可以把它想象成一条逐渐变薄的纸带：虽然纸带无限长，但如果它的宽度以指数速度递减，总面积（=总用纸量）是有限的。

### 4.4 第二型：被积函数无界

还有一种反常积分：积分区间有限，但被积函数在某点趋向无穷。

**定义**：若 $f$ 在 $[a, b)$ 上连续但 $\lim_{x \to b^-} f(x) = \pm\infty$，则：

$$
\int_a^b f(x)\,dx = \lim_{t \to b^-} \int_a^t f(x)\,dx
$$

**例**：

$$
\int_0^1 \frac{1}{\sqrt{x}} \, dx = \lim_{t \to 0^+} \int_t^1 x^{-1/2}\,dx = \lim_{t \to 0^+} \left[2\sqrt{x}\right]_t^1 = \lim_{t \to 0^+} (2 - 2\sqrt{t}) = 2
$$

虽然 $1/\sqrt{x}$ 在 $x = 0$ 处趋向无穷，但积分只有 $2$——"奔向无穷"的速度不够快，被 $\sqrt{x} \to 0$ 的区间压缩所补偿了。

相比之下：

$$
\int_0^1 \frac{1}{x}\,dx = \lim_{t \to 0^+} [-\ln t] = +\infty
$$

发散了。$1/x$ 不仅在 $x = 0$ 处爆炸，而且爆炸得太猛，积分无法补偿。

### 4.5 反常积分的物理直觉总结

| 衰减方式 | $\int_1^\infty$ 收敛吗？ | 典型例子 |
|----------|--------------------------|----------|
| 指数衰减 $e^{-\beta x}$ | 收敛 | 膜通量衰减、放射性衰变 |
| 快幂衰减 $1/x^p$, $p>1$ | 收敛 | 引力场、静电场 |
| 临界衰减 $1/x$ | 发散 | 调和级数的连续版本 |
| 慢幂衰减 $1/x^p$, $p<1$ | 发散 | —— |

一条经验法则：**如果一个正值函数衰减得比 $1/x$ 快，它在 $[1, \infty)$ 上的积分就有可能收敛。** 指数衰减是最"安全"的情况——它比任何幂函数都衰减得快。

这个层级结构在膜科学中很有用。当你建立一个膜污染模型、一个溶质在多孔介质中的扩散模型、或者一个浓差极化层的浓度分布模型时，模型的长程行为（远离膜表面、或者长时间运行后的行为）取决于解的衰减方式。如果衰减是指数型的，各种积分量（总沉积量、总通量损失等）都会收敛，模型行为"温顺"。如果衰减是幂律型的，你就需要仔细检查指数 $p$ 是否大于临界值。

### 4.6 一个深刻的例子：Gabriel 的号角

（纯数学，但太美了不能不讲。）

取曲线 $y = 1/x$（$x \geq 1$），绕 $x$ 轴旋转，得到一个喇叭形的立体——"Gabriel 的号角"（也叫 Torricelli 的小号）。

它的**体积**：

$$
V = \pi \int_1^{\infty} \frac{1}{x^2}\,dx = \pi \cdot 1 = \pi
$$

有限！（因为 $1/x^2$ 的积分收敛。）

它的**表面积**：

$$
S = 2\pi \int_1^{\infty} \frac{1}{x}\sqrt{1 + \frac{1}{x^4}}\,dx > 2\pi \int_1^{\infty} \frac{1}{x}\,dx = \infty
$$

无限！（因为 $1/x$ 的积分发散。）

所以这个号角**可以装满颜料**（体积有限），但**不能把内壁涂满**（表面积无限）。一个体积有限、表面积无限的物体。

这不是诡辩。它告诉你"有限"和"无限"的边界比你想象的更微妙。在膜科学的多孔介质模型中，有时候分形结构的孔道有类似的特性——有限体积内可以有惊人大的表面积。这正是多孔膜追求的目标：在紧凑的空间里塞进尽可能大的传质面积。

---

## 精讲例题

### 例题 1：平均浓度与截留率

反渗透膜的截留率定义为 $R = 1 - c_p/c_f$，其中 $c_f$ 是进料浓度，$c_p$ 是渗透液浓度。在一个连续运行的膜组件中，进料侧浓度从入口到出口升高。假设：

$$
c_f(z) = c_0 \, e^{\alpha z}, \quad z \in [0, L]
$$

其中 $\alpha > 0$ 反映了沿程浓缩效应，$c_0$ 是入口浓度。

(a) 求进料侧的平均浓度 $\bar{c}_f$。

(b) 如果有人用入口浓度 $c_0$ 和出口浓度 $c_0 e^{\alpha L}$ 的算术平均来估算 $\bar{c}_f$，误差有多大？

**解 (a)**：

$$
\bar{c}_f = \frac{1}{L}\int_0^L c_0 \, e^{\alpha z}\,dz = \frac{c_0}{L} \left[\frac{1}{\alpha} e^{\alpha z}\right]_0^L = \frac{c_0}{\alpha L}(e^{\alpha L} - 1)
$$

令 $c_L = c_0 e^{\alpha L}$ 为出口浓度，$\alpha L = \ln(c_L/c_0)$，则：

$$
\bar{c}_f = \frac{c_L - c_0}{\ln(c_L / c_0)}
$$

看出来了吗？这就是 $c_0$ 和 $c_L$ 的**对数平均**！LMTD 公式再次浮现——因为指数型分布的平均值**必然**是对数平均。这不是巧合，而是数学的必然。

**解 (b)**：

算术平均 $\bar{c}_{\text{am}} = (c_0 + c_L)/2$。

用具体数字：设 $c_0 = 1\;\text{g/L}$，$\alpha L = 1$（即 $c_L = e \approx 2.718$）。

$$
\bar{c}_f = \frac{e - 1}{\ln e} = e - 1 \approx 1.718
$$

$$
\bar{c}_{\text{am}} = \frac{1 + e}{2} \approx 1.859
$$

相对误差 $\frac{1.859 - 1.718}{1.718} \approx 8.2\%$。当浓缩比更大时（比如 $\alpha L = 2$，出口浓度是入口的 $7.4$ 倍），误差会更大。

### 例题 2：反常积分的收敛判断

判断下列积分的收敛性：

(a) $\displaystyle \int_1^{\infty} \frac{e^{-x}}{x}\,dx$

(b) $\displaystyle \int_1^{\infty} \frac{1}{x \ln x}\,dx$

**解 (a)**：

在 $[1, \infty)$ 上，$0 < \frac{e^{-x}}{x} \leq e^{-x}$。而 $\int_1^\infty e^{-x}\,dx = e^{-1}$ 收敛。由**比较判别法**，原积分也收敛。

直觉：$e^{-x}$ 本身就衰减得够快了，再除以一个增长的 $x$，只会衰减得更快。

**解 (b)**：

用换元 $u = \ln x$，$du = dx/x$。当 $x = 1$ 时 $u = 0$，当 $x \to \infty$ 时 $u \to \infty$：

$$
\int_1^{\infty} \frac{dx}{x \ln x} = \int_0^{\infty} \frac{du}{u}
$$

$\int_0^\infty du/u$ 在 $u = 0$ 处有第二型反常积分（$1/u$ 在 $0$ 处爆炸），在 $u = \infty$ 处有第一型反常积分——两端都发散。

实际上我们只需从 $u = \ln 1 = 0$ 到 $\infty$，但由于 $\int_1^\infty du/u = \infty$（从 $u = 0$ 到某正数的积分也有问题），这个积分发散。

更严格地：对 $x \geq e$（即 $\ln x \geq 1$）的部分：

$$
\int_e^{t} \frac{dx}{x\ln x} = [\ln(\ln x)]_e^t = \ln(\ln t) - \ln 1 = \ln(\ln t) \to \infty
$$

发散。$\frac{1}{x\ln x}$ 衰减得比 $\frac{1}{x}$ 快，但还是不够快——只是"差一点点"就能收敛。（类比：$\frac{1}{x(\ln x)^2}$ 就收敛了。你可以验证一下。）

### 例题 3：曲线间面积的物理含义

两种膜的通量-压力曲线分别为：

$$
J_A(\Delta P) = 5(1 - e^{-0.1\Delta P}), \quad J_B(\Delta P) = 3(1 - e^{-0.05\Delta P})
$$

其中 $\Delta P \in [0, 40]$（bar），$J$ 的单位是 $\text{L/(m}^2\text{·h)}$。

(a) 在整个压力范围内，$J_A > J_B$ 是否成立？

(b) 求两条曲线之间的面积并解释其物理含义。

**解 (a)**：

在 $\Delta P = 0$ 时，$J_A = J_B = 0$。对于 $\Delta P > 0$：

$J_A(40) = 5(1 - e^{-4}) \approx 5 \times 0.982 = 4.91$

$J_B(40) = 3(1 - e^{-2}) \approx 3 \times 0.865 = 2.59$

$J_A$ 的极限通量（$5$）高于 $J_B$（$3$），且 $J_A$ 响应更快（衰减常数 $0.1 > 0.05$）。可以验证 $J_A(\Delta P) > J_B(\Delta P)$ 对所有 $\Delta P > 0$ 成立（求导比较初始斜率：$J_A'(0) = 0.5$，$J_B'(0) = 0.15$）。

**解 (b)**：

$$
A = \int_0^{40} [J_A(\Delta P) - J_B(\Delta P)] \, d(\Delta P)
$$

$$
= \int_0^{40} [5(1 - e^{-0.1\Delta P}) - 3(1 - e^{-0.05\Delta P})] \, d(\Delta P)
$$

$$
= \int_0^{40} [2 - 5e^{-0.1\Delta P} + 3e^{-0.05\Delta P}] \, d(\Delta P)
$$

$$
= \left[2\Delta P + 50 e^{-0.1\Delta P} - 60 e^{-0.05\Delta P}\right]_0^{40}
$$

$$
= (80 + 50 e^{-4} - 60 e^{-2}) - (0 + 50 - 60)
$$

$$
= 80 + 50(0.0183) - 60(0.1353) - (50 - 60)
$$

$$
= 80 + 0.916 - 8.12 + 10 = 82.8
$$

**物理含义**：这个面积的单位是 $\text{L/(m}^2\text{·h)} \times \text{bar}$，它代表了在整个操作压力范围内，膜 A 相对于膜 B 的"累积通量优势"。如果你在所有压力下等概率操作，这个数值正比于你选择膜 A 而非膜 B 能多获得的产水量。

---

## 本课总结

| 概念 | 核心公式 | 物理含义 |
|------|----------|----------|
| 函数平均值 | $f_{\text{avg}} = \frac{1}{b-a}\int_a^b f(x)\,dx$ | 总量除以区间 = 摊平 |
| 曲线间面积 | $\int_a^b |f(x) - g(x)|\,dx$ | 两种速率的累积差异 |
| LMTD | $\Delta c_{\text{lm}} = \frac{\Delta c_1 - \Delta c_2}{\ln(\Delta c_1/\Delta c_2)}$ | 指数分布的真正平均值 |
| 反常积分 | $\int_a^\infty f\,dx = \lim_{t\to\infty}\int_a^t f\,dx$ | 无穷尾巴可以有有限面积 |

这四个主题并非拼盘——它们有一条暗线：**"平均"和"累积"是积分的两面。** 平均值是累积量除以区间长度；LMTD 是指数型累积的平均；反常积分告诉你累积可以在无穷区间上仍然有限。每一个看似独立的公式，都是积分这台求和机器在不同场景下的产出。

---

## 练习

**练习 1**（函数平均值）

膜孔内的速度分布（Hagen-Poiseuille 流）为：

$$
v(r) = v_{\max}\left(1 - \frac{r^2}{R^2}\right), \quad r \in [0, R]
$$

(a) 求 $v(r)$ 在 $[0, R]$ 上关于 $r$ 的"简单平均"$\frac{1}{R}\int_0^R v(r)\,dr$。

(b) 实际上物理意义更重要的是**截面平均速度**（流量除以截面积）。在圆管中，它应该写成

$$
\bar{v} = \frac{\int_0^R v(r) \cdot 2\pi r\,dr}{\pi R^2}
$$

计算 $\bar{v}$ 并与 (a) 的结果比较。哪一个才是工程上有用的"平均速度"？

**答案提示**：(a) $\frac{1}{R}\int_0^R v_{\max}(1 - r^2/R^2)\,dr = v_{\max} \cdot \frac{2}{3}$。 (b) $\bar{v} = \frac{2v_{\max}}{R^2}\int_0^R r(1 - r^2/R^2)\,dr = \frac{v_{\max}}{2}$。工程上用的是 (b)——因为靠近管壁的流体占据更大的环形面积（$2\pi r\,dr$ 权重），但那里速度低，所以截面平均比简单平均要低。这是"加权平均"的威力——权重不同，平均值不同。

---

**练习 2**（LMTD 的极端情况）

(a) 利用 L'Hôpital 法则证明：当 $\Delta c_1 \to \Delta c_2$ 时，$\Delta c_{\text{lm}} \to \Delta c_1$。

(b) 当 $\Delta c_2 \to 0$（即出口端几乎完全分离）时，$\Delta c_{\text{lm}}$ 趋向什么？与 $\Delta c_1 / 2$（算术平均）相比如何？

**答案提示**：(a) 令 $\Delta c_2 = \Delta c_1 - \epsilon$，$\epsilon \to 0$。$\Delta c_{\text{lm}} = \frac{\epsilon}{\ln(\Delta c_1/(\Delta c_1 - \epsilon))}$。$\ln(\Delta c_1/(\Delta c_1 - \epsilon)) = -\ln(1 - \epsilon/\Delta c_1) \approx \epsilon/\Delta c_1$（线性近似）。所以 $\Delta c_{\text{lm}} \approx \frac{\epsilon}{\epsilon/\Delta c_1} = \Delta c_1$。 (b) $\Delta c_{\text{lm}} = \frac{\Delta c_1}{\ln(\Delta c_1/\Delta c_2)} \to 0$（分母趋向 $\infty$）。而算术平均 $\to \Delta c_1/2$。对数平均远小于算术平均——算术平均在这里严重高估。

---

**练习 3**（反常积分）

(a) 某膜后清洗的污染物释放速率为 $r(t) = \frac{A}{(1+t)^2}$（$A > 0$，$t$ 为清洗时间，小时）。求总释放量 $\int_0^\infty r(t)\,dt$。

(b) 如果释放速率改为 $r(t) = \frac{A}{1+t}$，总释放量是否有限？

(c) 判断 $\int_0^\infty t\,e^{-t}\,dt$ 的收敛性并求值。

**答案提示**：(a) $\int_0^\infty \frac{A}{(1+t)^2}\,dt = A\left[-\frac{1}{1+t}\right]_0^\infty = A(0 - (-1)) = A$。收敛。 (b) $\int_0^\infty \frac{A}{1+t}\,dt = A\ln(1+t)\big|_0^\infty = \infty$。发散。衰减太慢。 (c) 分部积分：$\int_0^T te^{-t}\,dt = [-te^{-t}]_0^T + \int_0^T e^{-t}\,dt = -Te^{-T} + (1 - e^{-T})$。当 $T \to \infty$，$Te^{-T} \to 0$（L'Hôpital 法则），所以答案 $= 1$。

---

**练习 4**（综合——从数据到 LMTD）

一个纳滤膜组件的入口和出口处测得如下数据：

| 位置 | 料液浓度 (g/L) | 渗透液浓度 (g/L) |
|------|----------------|------------------|
| 入口 | 10.0 | 0.5 |
| 出口 | 4.0 | 0.5 |

(a) 计算 $\Delta c_1$、$\Delta c_2$ 和 $\Delta c_{\text{lm}}$。

(b) 如果已知膜面积 $A = 2\;\text{m}^2$，总传质系数 $K = 1.5 \times 10^{-5}\;\text{m/s}$，利用 $\dot{N} = K \cdot A \cdot \Delta c_{\text{lm}}$ 估算总溶质传质速率。

(c) 如果用算术平均代替对数平均，传质速率会被高估多少百分比？

**答案提示**：(a) $\Delta c_1 = 9.5$，$\Delta c_2 = 3.5$，$\Delta c_{\text{lm}} = \frac{9.5 - 3.5}{\ln(9.5/3.5)} = \frac{6.0}{\ln(2.714)} = \frac{6.0}{0.999} \approx 6.01\;\text{g/L}$。 (b) $\dot{N} = 1.5 \times 10^{-5} \times 2 \times 6.01 \times 10^3 = 0.180\;\text{g/s}$（注意单位换算：g/L = g/dm³，需转为 g/m³）。 (c) $\Delta c_{\text{am}} = (9.5 + 3.5)/2 = 6.5$。高估比例 $(6.5 - 6.01)/6.01 \approx 8.2\%$。

---

**练习 5**（挑战——证明对数平均不等式）

对于 $a, b > 0$ 且 $a \neq b$，对数平均定义为：

$$
L(a, b) = \frac{a - b}{\ln a - \ln b}
$$

证明 $\sqrt{ab} \leq L(a, b) \leq \frac{a+b}{2}$。

**提示（右侧不等式）**：不妨设 $a > b$，令 $r = a/b > 1$。需证 $\frac{r-1}{\ln r} \leq \frac{r+1}{2}$，即 $2(r-1) \leq (r+1)\ln r$。令 $h(r) = (r+1)\ln r - 2(r-1)$。验证 $h(1) = 0$ 且 $h'(r) = \ln r + \frac{1}{r} + 1 - 2 = \ln r - 1 + \frac{1}{r}$。再证 $h'(r) \geq 0$ 对 $r \geq 1$ 成立（提示：$h'(1) = 0$ 且 $h''(r) = \frac{1}{r} - \frac{1}{r^2} = \frac{r-1}{r^2} \geq 0$）。左侧不等式类似处理。

---

> **下一课预告**：第十课将进入多变量世界的入口——偏导数与多元函数初步。从这里开始，你手里的函数不再只有一个旋钮可以调——温度、压力、浓度、流速，所有变量同时在场。偏导数是你在这个多旋钮世界里的第一盏灯。
````
