# 第15课：矩阵运算与矩阵乘法——变换的复合

---

> **"数学中最深刻的操作往往也是最不直观的。矩阵乘法的定义看起来如此古怪——为什么不是逐个元素相乘？——恰恰因为它编码了一个关于世界的深刻事实：变换可以接连作用，而它们的总效果本身又是一个变换。"**

---

## 前置知识检查

在开始这节课之前，请确认你对以下内容有基本的把握：

- **向量的线性组合**（第5课）：给定向量 $\mathbf{a}_1, \mathbf{a}_2, \ldots, \mathbf{a}_n$ 和标量 $c_1, c_2, \ldots, c_n$，你能理解 $c_1\mathbf{a}_1 + c_2\mathbf{a}_2 + \cdots + c_n\mathbf{a}_n$ 的几何含义。
- **矩阵–向量乘法**（第7课）：你知道 $A\mathbf{x}$ 是 $A$ 的列向量的线性组合，权重取自 $\mathbf{x}$ 的分量。
- **线性变换的概念**（第13–14课）：你理解一个从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性变换 $T$ 可以用一个 $m \times n$ 矩阵 $A$ 来表示，即 $T(\mathbf{x}) = A\mathbf{x}$。
- **二维旋转和缩放矩阵**（第14课）：你见过旋转角 $\theta$ 对应的矩阵 $R_\theta$ 和均匀缩放矩阵 $kI$。

如果上面有任何一项让你犹豫，不要着急，回头翻翻对应的课次。这节课的核心论证依赖于"矩阵乘向量 = 列的线性组合"这一关键直觉——如果你已经牢固掌握这一点，那么接下来的一切都是它自然的延伸。

---

## 一、矩阵加法与数乘：一个"自然"的开始

让我们从最朴素的问题开始：如果矩阵就是一个按行列排列的数表，那么两个同样大小的数表相加，最自然的做法是什么？

答案几乎不需要思考——逐个元素相加。

如果 $A$ 和 $B$ 都是 $m \times n$ 矩阵，那么

$$
(A + B)_{ij} = a_{ij} + b_{ij}
$$

就这么简单。类似地，如果 $r$ 是一个标量，那么

$$
(rA)_{ij} = r \cdot a_{ij}
$$

也完全符合直觉：把每个元素都放大 $r$ 倍。

这两个运算之所以"自然"，是因为它们尊重了矩阵最朴素的身份——数据的矩形容器。你有一张温度分布表格，我也有一张，两张表格相加就是对应位置的温度求和。膜科学的研究者对此再熟悉不过了：当你在有限差分网格上叠加两个浓度场时，你做的正是矩阵加法。

**这些运算满足你所期望的一切代数律：**

设 $A, B, C$ 为同样大小的矩阵，$r, s$ 为标量，则：

- $A + B = B + A$（加法交换律）
- $(A + B) + C = A + (B + C)$（加法结合律）
- $A + 0 = A$（零矩阵是加法单位元）
- $r(A + B) = rA + rB$（数乘对矩阵加法的分配律）
- $(r + s)A = rA + sA$（数乘对标量加法的分配律）
- $r(sA) = (rs)A$（数乘的结合律）

这些性质的证明无一例外地归结为"逐个元素验证"——因为加法和数乘都是逐元素定义的，每个元素都是实数，实数的代数律传递上来就行了。

**如果世界到此为止**，矩阵就只是一种方便的记账工具——一种带下标的高级表格。但世界不会到此为止。

---

## 二、大问题：矩阵乘法为什么不是逐元素相乘？

现在让我想问你一个问题。假设有人让你"发明"矩阵乘法。你看着两个同样大小的矩阵 $A$ 和 $B$，最本能的反应是什么？

大多数人的第一反应是：逐个元素相乘呗。

$$
(A \odot B)_{ij} = a_{ij} \cdot b_{ij}
$$

这叫做 **Hadamard 乘积**（或逐元素乘积），它确实存在，在某些领域（比如信号处理、神经网络中的逐元素激活）偶尔有用。但它**不是**线性代数中的标准矩阵乘法。

为什么？

让我们用一个思想实验来揭示答案。

### 思想实验：机器人手臂

想象一个平面上的机器人手臂。第一个关节做一次变换——比如绕原点旋转 $30°$。第二个关节再做一次变换——比如沿 $x$ 轴方向拉伸 $2$ 倍。

问题来了：**这两次变换的总效果是什么？**

你手里有两个矩阵，$A$ 描述第二次变换，$B$ 描述第一次变换。一个输入向量 $\mathbf{x}$ 先被 $B$ 变换成 $B\mathbf{x}$，然后再被 $A$ 变换成 $A(B\mathbf{x})$。

你想要的是：**找一个矩阵 $C$，使得对所有输入 $\mathbf{x}$ 都有 $C\mathbf{x} = A(B\mathbf{x})$。**

这个矩阵 $C$ 就是我们要定义的"矩阵乘积" $AB$。

现在你看到了关键：矩阵乘法的定义不是来自"数表"的视角，而是来自"变换"的视角。矩阵不仅仅是数据容器——**矩阵是变换的编码**。矩阵乘法编码的是**变换的复合**。

逐元素乘法完全无法捕捉这层含义。两个旋转矩阵逐元素相乘后得到的东西，不对应任何有意义的几何操作。

让我用一句话概括这个核心洞见，请你把它刻在心里：

> **矩阵乘法 $AB$ 的定义，是唯一使得 $A(B\mathbf{x}) = (AB)\mathbf{x}$ 对所有 $\mathbf{x}$ 成立的定义。**

一切怪异的计算规则——行乘列、内维度必须匹配——都是这个要求的必然后果。

---

## 三、矩阵乘法的定义：从变换复合推导

好，让我们严格地把这个定义推导出来。这不是从天上掉下来的公式——这是一步步、自然而然逼出来的。

### 设定

设 $A$ 是 $m \times n$ 矩阵，$B$ 是 $n \times p$ 矩阵，$B$ 的各列为 $\mathbf{b}_1, \mathbf{b}_2, \ldots, \mathbf{b}_p$。

取 $\mathbb{R}^p$ 中的任意向量 $\mathbf{x} = (x_1, x_2, \ldots, x_p)^T$。

### 第一步：$B\mathbf{x}$ 是什么？

由矩阵–向量乘法的列线性组合定义：

$$
B\mathbf{x} = x_1 \mathbf{b}_1 + x_2 \mathbf{b}_2 + \cdots + x_p \mathbf{b}_p
$$

这是 $\mathbb{R}^n$ 中的一个向量。

### 第二步：$A(B\mathbf{x})$ 是什么？

把上面的结果喂给 $A$。因为矩阵–向量乘法是线性的（这是第13课的核心结论！），所以：

$$
A(B\mathbf{x}) = A(x_1 \mathbf{b}_1 + x_2 \mathbf{b}_2 + \cdots + x_p \mathbf{b}_p)
$$

$$
= x_1 A\mathbf{b}_1 + x_2 A\mathbf{b}_2 + \cdots + x_p A\mathbf{b}_p
$$

### 第三步：认出结果的形式

注意上面的表达式：它是向量 $A\mathbf{b}_1, A\mathbf{b}_2, \ldots, A\mathbf{b}_p$ 的线性组合，权重恰好是 $x_1, x_2, \ldots, x_p$——也就是 $\mathbf{x}$ 的各分量。

但这正是一个矩阵乘向量的形式！如果我们把 $A\mathbf{b}_1, A\mathbf{b}_2, \ldots, A\mathbf{b}_p$ 排成一个矩阵的各列，那么这个矩阵乘以 $\mathbf{x}$ 就等于 $A(B\mathbf{x})$。

因此，我们**被迫**定义：

$$
AB = \begin{bmatrix} A\mathbf{b}_1 & A\mathbf{b}_2 & \cdots & A\mathbf{b}_p \end{bmatrix}
$$

**这就是矩阵乘法的定义。** $AB$ 的第 $j$ 列，就是用 $A$ 去变换 $B$ 的第 $j$ 列。

你看到了吗？我们没有"选择"这个定义——这个定义是被"变换的复合应当也是一个变换"这个要求**强迫**出来的。

### 尺寸条件

$A$ 是 $m \times n$，$B$ 是 $n \times p$。$A$ 能作用于 $\mathbf{b}_j$ 的前提是 $\mathbf{b}_j \in \mathbb{R}^n$，即 $B$ 的行数等于 $A$ 的列数。结果 $A\mathbf{b}_j \in \mathbb{R}^m$，所以 $AB$ 是 $m \times p$ 矩阵。

记忆口诀如下——用一个 ASCII 尺寸图：


A        B     =    AB
(m × n)(n × p)  =  (m × p)
↑   ↑
必须相等
↓
这就是
"内维度匹配"
powershell复制代码
外面的两个维度 $m$ 和 $p$ 存活下来，成为乘积的维度。

---

## 四、行–列计算法则：实际动手的利器

上面的列定义是理论上最有洞察力的视角。但当你坐在桌前用笔算的时候，逐列计算并不总是最方便的。你需要一个逐元素的公式。

**行–列法则 (Row-Column Rule)**：

$(AB)_{ij}$——即乘积 $AB$ 第 $i$ 行第 $j$ 列的元素——等于 $A$ 的第 $i$ 行与 $B$ 的第 $j$ 列对应元素相乘后求和：

$$
(AB)_{ij} = \sum_{k=1}^{n} a_{ik} b_{kj} = a_{i1}b_{1j} + a_{i2}b_{2j} + \cdots + a_{in}b_{nj}
$$

用一幅 ASCII 图来说明：


css复制代码       B 的第 j 列
          ↓
    [ b_1j ]
    [ b_2j ]
    [ b_3j ]
      ...

A 的第 i 行 → [ a_i1  a_i2  a_i3  ... ]
(AB)_ij = a_i1 * b_1j + a_i2 * b_2j + a_i3 * b_3j + ...
powershell复制代码
$A$ 的第 $i$ 行"横着走"，$B$ 的第 $j$ 列"竖着走"，它们在交叉点相遇，做一次**点积**（内积）。

这就是为什么你经常听到"**行乘列**"这三个字。

---

## 五、精讲例题 1：手算一个矩阵乘法

**例题**：计算 $AB$，其中

$$
A = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, \quad B = \begin{bmatrix} 5 & 6 \\ 7 & 8 \end{bmatrix}
$$

**方法一：列定义法**

$B$ 的两列分别是 $\mathbf{b}_1 = \begin{bmatrix} 5 \\ 7 \end{bmatrix}$，$\mathbf{b}_2 = \begin{bmatrix} 6 \\ 8 \end{bmatrix}$。

$$
A\mathbf{b}_1 = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}\begin{bmatrix} 5 \\ 7 \end{bmatrix} = \begin{bmatrix} 1 \cdot 5 + 2 \cdot 7 \\ 3 \cdot 5 + 4 \cdot 7 \end{bmatrix} = \begin{bmatrix} 19 \\ 43 \end{bmatrix}
$$

$$
A\mathbf{b}_2 = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}\begin{bmatrix} 6 \\ 8 \end{bmatrix} = \begin{bmatrix} 1 \cdot 6 + 2 \cdot 8 \\ 3 \cdot 6 + 4 \cdot 8 \end{bmatrix} = \begin{bmatrix} 22 \\ 50 \end{bmatrix}
$$

$$
AB = \begin{bmatrix} 19 & 22 \\ 43 & 50 \end{bmatrix}
$$

**方法二：行–列法则**

$$
(AB)_{11} = 1 \cdot 5 + 2 \cdot 7 = 19
$$

$$
(AB)_{12} = 1 \cdot 6 + 2 \cdot 8 = 22
$$

$$
(AB)_{21} = 3 \cdot 5 + 4 \cdot 7 = 43
$$

$$
(AB)_{22} = 3 \cdot 6 + 4 \cdot 8 = 50
$$

两种方法得到相同的结果，当然——它们本来就是同一个定义的两种计算方式。

---

## 六、关键教学时刻：用二维旋转亲手验证

理论说得够多了。现在让我们做一件能带来真正信念的事情——**用具体的旋转矩阵来验证矩阵乘法确实对应变换的复合**。

### 回顾：旋转矩阵

将平面上的点绕原点逆时针旋转角度 $\theta$ 的变换，其标准矩阵为：

$$
R_\theta = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}
$$

### 实验

先旋转 $30°$，再旋转 $45°$。总效果应该是旋转 $75°$。

如果矩阵乘法真的编码了变换的复合，那么 $R_{45°} \cdot R_{30°}$ 应该等于 $R_{75°}$。

（注意顺序！先做的变换 $R_{30°}$ 在右边，后做的 $R_{45°}$ 在左边。这就像穿衣服——先穿的在里面。）

让我们算。需要的三角函数值：

- $\cos 30° = \frac{\sqrt{3}}{2}$，$\sin 30° = \frac{1}{2}$
- $\cos 45° = \frac{\sqrt{2}}{2}$，$\sin 45° = \frac{\sqrt{2}}{2}$
- $\cos 75° = \cos(45° + 30°) = \cos 45° \cos 30° - \sin 45° \sin 30° = \frac{\sqrt{6} - \sqrt{2}}{4}$
- $\sin 75° = \sin(45° + 30°) = \sin 45° \cos 30° + \cos 45° \sin 30° = \frac{\sqrt{6} + \sqrt{2}}{4}$

计算 $R_{45°} R_{30°}$：

$$
R_{45°} R_{30°} = \begin{bmatrix} \frac{\sqrt{2}}{2} & -\frac{\sqrt{2}}{2} \\ \frac{\sqrt{2}}{2} & \frac{\sqrt{2}}{2} \end{bmatrix} \begin{bmatrix} \frac{\sqrt{3}}{2} & -\frac{1}{2} \\ \frac{1}{2} & \frac{\sqrt{3}}{2} \end{bmatrix}
$$

$(1,1)$ 元素：$\frac{\sqrt{2}}{2} \cdot \frac{\sqrt{3}}{2} + (-\frac{\sqrt{2}}{2}) \cdot \frac{1}{2} = \frac{\sqrt{6}}{4} - \frac{\sqrt{2}}{4} = \frac{\sqrt{6} - \sqrt{2}}{4}$

$(2,1)$ 元素：$\frac{\sqrt{2}}{2} \cdot \frac{\sqrt{3}}{2} + \frac{\sqrt{2}}{2} \cdot \frac{1}{2} = \frac{\sqrt{6}}{4} + \frac{\sqrt{2}}{4} = \frac{\sqrt{6} + \sqrt{2}}{4}$

$(1,2)$ 元素：$\frac{\sqrt{2}}{2} \cdot (-\frac{1}{2}) + (-\frac{\sqrt{2}}{2}) \cdot \frac{\sqrt{3}}{2} = -\frac{\sqrt{2}}{4} - \frac{\sqrt{6}}{4} = -\frac{\sqrt{6} + \sqrt{2}}{4}$

$(2,2)$ 元素：$\frac{\sqrt{2}}{2} \cdot (-\frac{1}{2}) + \frac{\sqrt{2}}{2} \cdot \frac{\sqrt{3}}{2} = -\frac{\sqrt{2}}{4} + \frac{\sqrt{6}}{4} = \frac{\sqrt{6} - \sqrt{2}}{4}$

因此：

$$
R_{45°} R_{30°} = \begin{bmatrix} \frac{\sqrt{6}-\sqrt{2}}{4} & -\frac{\sqrt{6}+\sqrt{2}}{4} \\ \frac{\sqrt{6}+\sqrt{2}}{4} & \frac{\sqrt{6}-\sqrt{2}}{4} \end{bmatrix} = R_{75°}
$$

**它成了。**

三角恒等式 $\cos(\alpha + \beta) = \cos\alpha\cos\beta - \sin\alpha\sin\beta$ 和 $\sin(\alpha + \beta) = \sin\alpha\cos\beta + \cos\alpha\sin\beta$ 没有从天上掉下来——它们是矩阵乘法的**自然后果**。或者反过来说，矩阵乘法的行–列法则恰好以一种精致的方式把和角公式编码在 $2 \times 2$ 矩阵的四个位置上。

> **动手建议**：用 Python 验证上面的计算。以下代码只需要 NumPy：
>
> ```python
> import numpy as np
> 
> def R(theta_deg):
>     t = np.radians(theta_deg)
>     return np.array([[np.cos(t), -np.sin(t)],
>                      [np.sin(t),  np.cos(t)]])
> 
> print("R(45) @ R(30) =")
> print(R(45) @ R(30))
> print("\nR(75) =")
> print(R(75))
> ```
>
> 你会看到两个矩阵在浮点精度内完全一致。**亲眼看到这个，比任何证明都有说服力。**

---

## 七、$AB \neq BA$：秩序很重要

### 一个让人清醒的事实

在实数的世界里，$3 \times 5 = 5 \times 3$。乘法交换律是如此基本，以至于你不会意识到它的存在。

但矩阵的世界不同。矩阵乘法**一般不交换**：

$$
AB \neq BA \quad \text{（通常情况下）}
$$

这不是一个技术性的例外——这是矩阵乘法的**核心特征**。

### 物理直觉：先旋转再缩放 vs. 先缩放再旋转

让我们用一个具体的例子来感受。取两个变换：

- $R$：绕原点逆时针旋转 $90°$
- $S$：沿 $x$ 轴方向拉伸 $2$ 倍（$y$ 方向不变）

矩阵分别是：

$$
R = R_{90°} = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}, \quad S = \begin{bmatrix} 2 & 0 \\ 0 & 1 \end{bmatrix}
$$

**计算 $RS$（先缩放，再旋转）**：

$$
RS = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}\begin{bmatrix} 2 & 0 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & -1 \\ 2 & 0 \end{bmatrix}
$$

**计算 $SR$（先旋转，再缩放）**：

$$
SR = \begin{bmatrix} 2 & 0 \\ 0 & 1 \end{bmatrix}\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix} = \begin{bmatrix} 0 & -2 \\ 1 & 0 \end{bmatrix}
$$

$RS \neq SR$。

为什么？让我们追踪一个具体的向量 $\mathbf{e}_1 = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$：

**路径一（先缩放，再旋转，即 $RS$）**：

$$
\mathbf{e}_1 \xrightarrow{S} \begin{bmatrix} 2 \\ 0 \end{bmatrix} \xrightarrow{R} \begin{bmatrix} 0 \\ 2 \end{bmatrix}
$$

先沿 $x$ 轴拉长到 $2$，再旋转 $90°$——终点在 $y$ 轴正方向，距原点 $2$。

**路径二（先旋转，再缩放，即 $SR$）**：

$$
\mathbf{e}_1 \xrightarrow{R} \begin{bmatrix} 0 \\ 1 \end{bmatrix} \xrightarrow{S} \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

先旋转到 $y$ 轴上（距原点 $1$），再沿 $x$ 轴缩放——但此时向量已经站在了 $y$ 轴上，$x$ 方向的缩放对它毫无影响！


路径一 (RS):                      路径二 (SR):
y                                 y
|  (0,2)                          |  (0,1)
|   *                             |   *
|                                 |
|                                 |
+-------> x                       +-------> x
(1,0)→(2,0)→旋转→(0,2)           (1,0)→旋转→(0,1)→缩放→(0,1)
powershell复制代码
看到了吗？缩放在旋转之前做还是之后做，效果截然不同，因为缩放是"方向敏感的"——它只拉伸 $x$ 方向。旋转改变了向量的朝向之后，缩放作用的"目标"就变了。

**这就是不可交换性的物理本质：当变换是方向敏感的，操作的顺序就是有意义的。**

生活中到处都是这样的例子。先穿袜子再穿鞋，跟先穿鞋再穿袜子，结果完全不同。先过滤悬浮物再测浊度，跟先测浊度再过滤，得到的数据含义不同。在膜过程中，先对进料液进行预处理（化学变换 $A$），再通过膜进行截留（物理变换 $B$），与反过来操作，分离效率可能天差地别。

### 什么时候 $AB = BA$？

有一些特殊情况：

1. **$B = I$**（单位矩阵）：$AI = IA = A$。单位变换与任何变换都可交换——这就像"什么都不做"跟任何操作都不冲突。
2. **$B = kI$**（数量矩阵，即均匀缩放）：$(kI)A = k(IA) = kA = A(kI)$。均匀缩放从各个方向均匀地放大或缩小，不管你先做什么变换再缩放、还是先缩放再做变换，结果一样。
3. **$A$ 和 $B$ 表示绕同一轴的旋转**：两个旋转角度相加，加法交换。
4. **$A$ 和 $B$ 是同一个矩阵的幂**：$A^j A^k = A^{j+k} = A^k A^j$。

除了这些特殊情况，**默认假设 $AB \neq BA$**。在你用矩阵乘法求解任何问题时，始终注意操作顺序。

---

## 八、矩阵乘法的基本性质

尽管矩阵乘法不满足交换律，但它满足另外一些重要的性质。设 $A$、$B$、$C$ 的尺寸使得下面的乘法和加法都有定义，$r$ 为标量，则：

1. **结合律**：$A(BC) = (AB)C$
2. **左分配律**：$A(B + C) = AB + AC$
3. **右分配律**：$(B + C)A = BA + CA$
4. **数乘相容性**：$r(AB) = (rA)B = A(rB)$
5. **单位元**：$I_m A = A = A I_n$（其中 $A$ 是 $m \times n$）

结合律的证明特别优雅——你可以用两种方式来理解它：

**方式一（变换视角）**：$A(BC)$ 和 $(AB)C$ 都代表"先做 $C$，再做 $B$，最后做 $A$"这三个变换的复合。函数复合是结合的（这是集合论中的基本事实），所以矩阵乘法也是。

**方式二（列定义法）**：令 $C$ 的各列为 $\mathbf{c}_1, \ldots, \mathbf{c}_p$。由乘积的列定义，$BC$ 的第 $j$ 列是 $B\mathbf{c}_j$，于是 $A(BC)$ 的第 $j$ 列是 $A(B\mathbf{c}_j)$。但根据矩阵乘法的定义，$A(B\mathbf{c}_j) = (AB)\mathbf{c}_j$，这正是 $(AB)C$ 的第 $j$ 列。

结合律意味着我们可以放心地写 $ABC$ 而不需要括号——不管怎么分组，结果一样。

**但要注意**：结合律加上不可交换性意味着你可以在 $ABC$ 中自由加括号，但**不能**交换 $A$、$B$、$C$ 的左右顺序。

### 更多的"预警"

线性代数初学者还需要警惕两件在实数中理所当然但在矩阵中不成立的事：

- **消去律不成立**：$AB = AC$ 不能推出 $B = C$（即使 $A \neq 0$）。
- **零因子可能存在**：$AB = 0$ 不能推出 $A = 0$ 或 $B = 0$。

举个例子：

$$
A = \begin{bmatrix} 1 & 1 \\ 2 & 2 \end{bmatrix}, \quad B = \begin{bmatrix} 1 \\ -1 \end{bmatrix}
$$

$$
AB = \begin{bmatrix} 1+(-1) \\ 2+(-2) \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}
$$

$A \neq 0$，$B \neq 0$，但 $AB = 0$。秘密在于 $A$ 的两列是平行的——它把整个平面压缩成了一条直线，而 $B$ 恰好落在被压缩为零的方向上。

---

## 九、转置矩阵：镜中的自己

### 定义

给定 $m \times n$ 矩阵 $A$，它的**转置** $A^T$ 是 $n \times m$ 矩阵，定义为：

$$
(A^T)_{ij} = a_{ji}
$$

也就是说，$A$ 的行变成 $A^T$ 的列，$A$ 的列变成 $A^T$ 的行。

$$
A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix} \implies A^T = \begin{bmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \end{bmatrix}
$$

几何上，转置相当于将矩阵沿主对角线做镜像翻转。

### 转置的性质

设 $A, B$ 的尺寸使下面的运算有意义，$r$ 为标量，则：

1. $(A^T)^T = A$
2. $(A + B)^T = A^T + B^T$
3. $(rA)^T = rA^T$
4. $(AB)^T = B^T A^T$

前三条很自然。**第四条是最重要也最容易记错的**：乘积的转置等于转置的乘积，但**顺序要反过来**。

为什么顺序要反？回想矩阵乘法代表变换的复合。$AB$ 表示"先做 $B$，再做 $A$"。转置操作在某种意义上"反转"了变换的方向。当你反转两个依次进行的操作时，你需要把顺序颠倒——就像回放录像带一样。

$(AB)^T = B^T A^T$——先反转 $A$，再反转 $B$。

推广到多个矩阵：

$$
(ABC)^T = C^T B^T A^T
$$

### 转置的更深层含义（预告）

在后面的课程中，你会发现转置的意义远不止"行列互换"：

- **内积**：$\mathbf{x}^T \mathbf{y}$ 是两个向量 $\mathbf{x}$ 和 $\mathbf{y}$ 的内积（点积）。
- **对称矩阵**：$A = A^T$ 的矩阵在物理和工程中无处不在——它们是"自伴算子"的矩阵表示，拥有一整套美丽的理论（实特征值、正交特征向量分解……）。
- **最小二乘法**：法方程 $A^T A \mathbf{x} = A^T \mathbf{b}$ 是数据拟合的核心工具。

现在，先记住转置的定义和基本性质就够了。

---

## 十、精讲例题 2：矩阵乘法的顺序陷阱

**例题**：设 $A = \begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}$，$B = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}$。

(a) 计算 $AB$ 和 $BA$，并解释几何含义。

(b) 验证 $(AB)^T = B^T A^T$。

**解答**：

**(a)**

$A$ 是一个**非均匀缩放**：$x$ 方向放大 $2$ 倍，$y$ 方向放大 $3$ 倍。

$B$ 是一个**顺时针旋转 $90°$**（你可以验证 $B\mathbf{e}_1 = \begin{bmatrix} 0 \\ -1 \end{bmatrix}$，$B\mathbf{e}_2 = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$）。

$$
AB = \begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}\begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix} = \begin{bmatrix} 0 & 2 \\ -3 & 0 \end{bmatrix}
$$

$AB$：先旋转 $90°$，再缩放。看看 $\mathbf{e}_1$：先被旋转到 $\begin{bmatrix} 0 \\ -1 \end{bmatrix}$，再被缩放成 $\begin{bmatrix} 0 \\ -3 \end{bmatrix}$。

$$
BA = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}\begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix} = \begin{bmatrix} 0 & 3 \\ -2 & 0 \end{bmatrix}
$$

$BA$：先缩放，再旋转。$\mathbf{e}_1$ 先被缩放成 $\begin{bmatrix} 2 \\ 0 \end{bmatrix}$，再被旋转成 $\begin{bmatrix} 0 \\ -2 \end{bmatrix}$。

结果不同！$AB$ 的 $(2,1)$ 元素是 $-3$，而 $BA$ 的 $(2,1)$ 元素是 $-2$。

关键在于：缩放是"方向敏感"的。旋转改变了向量的朝向，导致缩放作用于不同的分量。

**(b)**

$$
(AB)^T = \begin{bmatrix} 0 & 2 \\ -3 & 0 \end{bmatrix}^T = \begin{bmatrix} 0 & -3 \\ 2 & 0 \end{bmatrix}
$$

$$
B^T A^T = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}\begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix} = \begin{bmatrix} 0 & -3 \\ 2 & 0 \end{bmatrix}
$$

相等。验证完毕。

---

## 十一、前瞻：用矩阵乘法描述离散化扩散方程

现在让我给你看一个来自膜科学研究者日常世界的例子——这个例子将在第36课（微分方程的矩阵方法）中被充分展开，但我想现在就让你感受到矩阵乘法的威力远不止旋转和缩放。

### 问题：一维扩散

想象一根细管中的溶质扩散。Fick 第二定律告诉我们浓度 $u(x,t)$ 的演化：

$$
\frac{\partial u}{\partial t} = D \frac{\partial^2 u}{\partial x^2}
$$

其中 $D$ 是扩散系数。

### 离散化

将空间分成 $n$ 个等间距的网格点 $x_1, x_2, \ldots, x_n$，间距为 $h$。在每个时刻，浓度分布可以用一个向量 $\mathbf{u}(t) = \begin{bmatrix} u_1(t) \\ u_2(t) \\ \vdots \\ u_n(t) \end{bmatrix}$ 来表示。

二阶导数的中心差分近似为：

$$
\frac{\partial^2 u}{\partial x^2}\bigg|_{x_i} \approx \frac{u_{i-1} - 2u_i + u_{i+1}}{h^2}
$$

于是，整个方程可以写成：

$$
\frac{d\mathbf{u}}{dt} = \frac{D}{h^2} L \mathbf{u}
$$

其中 $L$ 是一个**三对角矩阵**：

$$
L = \begin{bmatrix} -2 & 1 & 0 & \cdots & 0 \\ 1 & -2 & 1 & \cdots & 0 \\ 0 & 1 & -2 & \cdots & 0 \\ \vdots & & \ddots & \ddots & 1 \\ 0 & \cdots & 0 & 1 & -2 \end{bmatrix}
$$

（为简洁起见，这里假设两端边界浓度固定为零。）

### 矩阵乘法的意义

看看 $L\mathbf{u}$ 做了什么。$L\mathbf{u}$ 的第 $i$ 个分量是：

$$
(L\mathbf{u})_i = u_{i-1} - 2u_i + u_{i+1}
$$

这正是有限差分版的二阶导数！**矩阵 $L$ 把"对每个网格点做二阶差分"这件事编码成了一次矩阵–向量乘法。**

如果你用显式 Euler 方法做时间推进，取时间步长 $\Delta t$：

$$
\mathbf{u}(t + \Delta t) \approx \mathbf{u}(t) + \frac{D \Delta t}{h^2} L \mathbf{u}(t) = \left(I + \frac{D \Delta t}{h^2} L\right) \mathbf{u}(t)
$$

令 $M = I + \frac{D \Delta t}{h^2} L$，则：

$$
\mathbf{u}(t + \Delta t) = M \mathbf{u}(t)
$$

走 $k$ 步之后：

$$
\mathbf{u}(t + k\Delta t) = M^k \mathbf{u}(t)
$$

**扩散过程的时间演化，就是矩阵的幂次作用。**

$M^k$ 是矩阵 $M$ 自乘 $k$ 次——这正是矩阵乘法的结合律保证我们可以做的事。而第36课将告诉你，理解 $M$ 的特征值和特征向量（第25–28课），就能把 $M^k$ 的计算变成指数衰减的叠加——扩散的模态分解。

> **给膜科学研究者的一句话**：上面的矩阵 $L$ 在你未来的研究中会反复出现。它不仅描述扩散，还出现在膜传质模型的数值模拟、浓差极化的计算、甚至反应–扩散方程的稳定性分析中。每当你在有限差分或有限体积法中写出"系数矩阵"时，你其实就是在用矩阵乘法的语言来描述物理定律。

> **动手建议**：用 Python 构造一个 $20 \times 20$ 的矩阵 $L$，设 $D=1$，$h = 0.05$，$\Delta t = 0.001$，初始浓度为中心高两端低的"帽子函数"，反复乘以 $M$ 并画出浓度分布随时间的变化。你会亲眼看到浓度峰被"抹平"的过程——这就是扩散。
>
> ```python
> import numpy as np
> import matplotlib.pyplot as plt
> 
> n = 20
> h = 1.0 / (n + 1)
> D = 1.0
> dt = 0.0005
> 
> # 构造三对角矩阵 L
> L = np.zeros((n, n))
> for i in range(n):
>     L[i, i] = -2
>     if i > 0:    L[i, i-1] = 1
>     if i < n-1:  L[i, i+1] = 1
> 
> M = np.eye(n) + (D * dt / h**2) * L
> 
> # 初始条件：中间高两端低
> u = np.zeros(n)
> u[n//2 - 2 : n//2 + 2] = 1.0
> 
> # 时间演化
> plt.figure(figsize=(8, 5))
> x = np.linspace(h, 1 - h, n)
> for step in [0, 50, 200, 800]:
>     if step == 0:
>         plt.plot(x, u, label=f"step {step}", linewidth=2)
>     u_temp = u.copy()
>     for _ in range(step if step > 0 else 0):
>         u_temp = M @ u_temp
>     plt.plot(x, u_temp, label=f"step {step}", linewidth=2)
> 
> plt.xlabel("x")
> plt.ylabel("concentration u")
> plt.title("Diffusion via matrix power M^k")
> plt.legend()
> plt.show()
> ```

---

## 十二、矩阵乘法的多种视角（总结）

在结束这节课之前，让我把矩阵乘法的几种等价视角汇总一下。同一个运算，从不同角度看，会有不同的计算便利和概念透明度。

### 视角 1：列定义（变换复合）

$AB$ 的第 $j$ 列 $= A \cdot (B \text{ 的第 } j \text{ 列})$

这是概念上最深刻的视角：每一列独立地被 $A$ 变换。

### 视角 2：行–列法则（逐元素计算）

$(AB)_{ij} = A \text{ 的第 } i \text{ 行} \cdot B \text{ 的第 } j \text{ 列}$（点积）

这是手算时最常用的方式。

### 视角 3：行定义

$AB$ 的第 $i$ 行 $= (A \text{ 的第 } i \text{ 行}) \cdot B$

这是视角 1 的"对偶"：每一行独立地用 $B$ 做右乘。

### 视角 4：外积展开（高级，简略提及）

如果将 $A$ 按列分块，$B$ 按行分块：

$$
A = \begin{bmatrix} \mathbf{a}_1 & \mathbf{a}_2 & \cdots & \mathbf{a}_n \end{bmatrix}, \quad B = \begin{bmatrix} \mathbf{r}_1^T \\ \mathbf{r}_2^T \\ \vdots \\ \mathbf{r}_n^T \end{bmatrix}
$$

则：

$$
AB = \mathbf{a}_1 \mathbf{r}_1^T + \mathbf{a}_2 \mathbf{r}_2^T + \cdots + \mathbf{a}_n \mathbf{r}_n^T
$$

每个 $\mathbf{a}_k \mathbf{r}_k^T$ 是一个**秩 $1$ 矩阵**（外积），$AB$ 是 $n$ 个秩 $1$ 矩阵的叠加。这个视角在第31课（SVD 分解）中将大放异彩。

---

## 十三、精讲例题 3：转置与乘法的综合

**例题**：设 $\mathbf{u} = \begin{bmatrix} 2 \\ -1 \\ 3 \end{bmatrix}$，$\mathbf{v} = \begin{bmatrix} 1 \\ 4 \\ -2 \end{bmatrix}$。

(a) 计算 $\mathbf{u}^T \mathbf{v}$（内积，$1 \times 1$ 矩阵，即标量）。

(b) 计算 $\mathbf{u}\mathbf{v}^T$（外积，$3 \times 3$ 矩阵）。

(c) 验证 $(\mathbf{u}\mathbf{v}^T)^T = \mathbf{v}\mathbf{u}^T$。

**解答**：

**(a)** $\mathbf{u}^T \mathbf{v}$ 是一个 $1 \times 3$ 行向量乘以一个 $3 \times 1$ 列向量：

$$
\mathbf{u}^T \mathbf{v} = \begin{bmatrix} 2 & -1 & 3 \end{bmatrix}\begin{bmatrix} 1 \\ 4 \\ -2 \end{bmatrix} = 2 \cdot 1 + (-1) \cdot 4 + 3 \cdot (-2) = 2 - 4 - 6 = -8
$$

**(b)** $\mathbf{u}\mathbf{v}^T$ 是一个 $3 \times 1$ 列向量乘以一个 $1 \times 3$ 行向量：

$$
\mathbf{u}\mathbf{v}^T = \begin{bmatrix} 2 \\ -1 \\ 3 \end{bmatrix}\begin{bmatrix} 1 & 4 & -2 \end{bmatrix} = \begin{bmatrix} 2 & 8 & -4 \\ -1 & -4 & 2 \\ 3 & 12 & -6 \end{bmatrix}
$$

注意这是一个秩 $1$ 矩阵——每一行都是 $\begin{bmatrix} 1 & 4 & -2 \end{bmatrix}$ 的标量倍。

**(c)**

$$
(\mathbf{u}\mathbf{v}^T)^T = \begin{bmatrix} 2 & -1 & 3 \\ 8 & -4 & 12 \\ -4 & 2 & -6 \end{bmatrix}
$$

$$
\mathbf{v}\mathbf{u}^T = \begin{bmatrix} 1 \\ 4 \\ -2 \end{bmatrix}\begin{bmatrix} 2 & -1 & 3 \end{bmatrix} = \begin{bmatrix} 2 & -1 & 3 \\ 8 & -4 & 12 \\ -4 & 2 & -6 \end{bmatrix}
$$

相等。这验证了 $(AB)^T = B^T A^T$，此处 $A = \mathbf{u}$，$B = \mathbf{v}^T$。

---

## 十四、本课的哲学反思

让我在课的最后暂停一下，跟你聊几句这节课真正教给我们的东西。

矩阵加法和数乘是"显然"的。逐元素操作。没有任何意外。这种操作把矩阵当作被动的数据容器，就像一个放满数字的抽屉。

矩阵乘法是"不显然"的。行乘列。内维度必须匹配。不满足交换律。这一切古怪的规则之所以存在，是因为矩阵乘法把矩阵当作**主动的操作者**——变换。当两个变换接连作用时，它们的总效果由矩阵乘法编码。

这种从"容器"到"操作者"的视角转变，是线性代数中最重要的思想跃迁之一。一旦你理解了这一点，你就能回答一个折磨了无数初学者的问题："矩阵乘法的定义为什么这么奇怪？"

它不奇怪。它是唯一正确的定义——**唯一使得变换的复合仍然是一次矩阵–向量乘法的定义**。

在膜科学研究中，你将不断地组合变换：坐标变换后求梯度，离散化后做时间推进，基底变换后做谱分析。每一次组合，背后都是一次矩阵乘法。从这节课开始，你手里多了一个强大的工具，而你也知道了这个工具为何如此塑造。

---

## 练习

### 练习 1（基础计算）

设

$$
A = \begin{bmatrix} 1 & -1 \\ 2 & 3 \end{bmatrix}, \quad B = \begin{bmatrix} 4 & 0 \\ -2 & 5 \end{bmatrix}
$$

(a) 计算 $AB$ 和 $BA$，验证 $AB \neq BA$。

(b) 计算 $(AB)^T$ 和 $B^T A^T$，验证它们相等。

**答案提示**：$AB = \begin{bmatrix} 6 & -5 \\ 2 & 15 \end{bmatrix}$，$BA = \begin{bmatrix} 4 & -4 \\ 8 & 17 \end{bmatrix}$。对 (b)，直接按定义转置 $AB$，再分别算 $B^T A^T$ 比较。

---

### 练习 2（旋转复合验证）

用矩阵乘法验证：绕原点旋转 $60°$ 再旋转 $60°$ 的总效果等于旋转 $120°$。即验证 $R_{60°} \cdot R_{60°} = R_{120°}$。

**答案提示**：$\cos 60° = 1/2$，$\sin 60° = \sqrt{3}/2$，$\cos 120° = -1/2$，$\sin 120° = \sqrt{3}/2$。按行–列法则逐元素计算 $(R_{60°})^2$，检查是否与 $R_{120°}$ 一致。特别注意 $(1,1)$ 元素：$(1/2)(1/2) - (\sqrt{3}/2)(\sqrt{3}/2) = 1/4 - 3/4 = -1/2 = \cos 120°$。

---

### 练习 3（不可交换性的几何观察）

设 $R = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$（逆时针旋转 $90°$），$P = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$（投影到 $x$ 轴）。

(a) 计算 $RP$ 和 $PR$。

(b) 分别将 $RP$ 和 $PR$ 作用于向量 $\mathbf{v} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$，在纸上画出结果。用语言描述"先投影再旋转"与"先旋转再投影"的区别。

**答案提示**：$RP = \begin{bmatrix} 0 & 0 \\ 1 & 0 \end{bmatrix}$，$PR = \begin{bmatrix} 0 & -1 \\ 0 & 0 \end{bmatrix}$。$RP\mathbf{v} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}$，$PR\mathbf{v} = \begin{bmatrix} -1 \\ 0 \end{bmatrix}$。先投影再旋转：$\mathbf{v}$ 先被压到 $x$ 轴上的 $(1,0)$，再旋转到 $(0,1)$。先旋转再投影：$\mathbf{v}$ 先被旋转到 $(-1,1)$，再被投影到 $x$ 轴上的 $(-1,0)$。两个结果不同，且方向完全不同！

---

### 练习 4（扩散矩阵初探）

构造一个 $4 \times 4$ 的三对角"扩散矩阵"：

$$
L = \begin{bmatrix} -2 & 1 & 0 & 0 \\ 1 & -2 & 1 & 0 \\ 0 & 1 & -2 & 1 \\ 0 & 0 & 1 & -2 \end{bmatrix}
$$

(a) 计算 $L\mathbf{u}$，其中 $\mathbf{u} = \begin{bmatrix} 0 \\ 0 \\ 1 \\ 0 \end{bmatrix}$。解释结果的物理含义：如果只有第 $3$ 个网格点有浓度，$L\mathbf{u}$ 描述了什么？

(b) 计算 $(I + 0.1 L)\mathbf{u}$。这代表做了一步扩散时间推进后的浓度分布。跟 $\mathbf{u}$ 比较：浓度发生了什么变化？

**答案提示**：(a) $L\mathbf{u} = \begin{bmatrix} 0 \\ 1 \\ -2 \\ 1 \end{bmatrix}$。这说明：如果只有第 $3$ 号网格点有浓度 $1$，那么扩散的"驱动力"使得第 $3$ 号点的浓度倾向于降低（系数 $-2$），而向两侧的第 $2$ 号和第 $4$ 号点"泄漏"（系数 $+1$）。(b) $(I + 0.1L)\mathbf{u} = \begin{bmatrix} 0 \\ 0.1 \\ 0.8 \\ 0.1 \end{bmatrix}$。中心点浓度从 $1$ 降到了 $0.8$，旁边两点从 $0$ 升到了 $0.1$。浓度在扩散——从高浓度区域向低浓度区域流动。守恒性检查：$0 + 0.1 + 0.8 + 0.1 = 1.0$，总浓度不变！

---

### 练习 5（转置与对称矩阵）

(a) 证明：对任意矩阵 $A$（不要求方阵），$A^T A$ 和 $A A^T$ 都是对称矩阵。

(b) 如果 $A$ 是 $3 \times 2$ 矩阵，$A^T A$ 和 $A A^T$ 分别是什么大小的矩阵？

**答案提示**：(a) 对 $A^T A$ 取转置：$(A^T A)^T = A^T (A^T)^T = A^T A$，所以 $A^T A$ 是对称的。类似地处理 $AA^T$。(b) $A$ 是 $3 \times 2$，$A^T$ 是 $2 \times 3$。$A^T A$ 是 $(2 \times 3)(3 \times 2) = 2 \times 2$，$AA^T$ 是 $(3 \times 2)(2 \times 3) = 3 \times 3$。尽管两者都是对称的，但它们大小不同！在最小二乘法中，$A^T A$ 就是法方程的系数矩阵。

---

## 下节预告

在第16课中，我们将学习**矩阵的逆**——如果矩阵乘法对应变换的复合，那么矩阵的逆就对应变换的"撤销"。什么样的变换可以撤销？什么样的不行？这个问题将把我们引向行列式、秩、以及"可逆矩阵"的完整理论。

---

> **本课核心要点速览**
>
> 1. 矩阵加法和数乘是逐元素运算，自然且直观。
> 2. 矩阵乘法**不是**逐元素乘——它编码的是**变换的复合**。
> 3. $AB$ 的第 $j$ 列 $= A \cdot (B \text{ 的第 } j \text{ 列})$。
> 4. 行–列法则：$(AB)_{ij} = A \text{ 第 } i \text{ 行} \cdot B \text{ 第 } j \text{ 列}$。
> 5. **$AB \neq BA$（通常情况下）**——操作顺序重要！
> 6. 转置：$(A^T)_{ij} = a_{ji}$，$(AB)^T = B^T A^T$（顺序反转）。
> 7. 扩散方程的离散化将偏微分方程变成了矩阵–向量乘法——物理定律的代数编码。