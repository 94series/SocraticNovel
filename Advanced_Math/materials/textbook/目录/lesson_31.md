# 第31课：ODE系统与定性分析——耦合世界的相图

> **第五部分 · 常微分方程（ODE）· 收尾课**
> 从单个方程走向耦合系统，从精确求解走向定性判断。线性代数与ODE在此汇流。

---

## 前置知识

为了跟上这节课，你需要熟悉：

- **特征值与特征向量**（第21–22课）：给定矩阵 $A$，能求 $A\mathbf{v} = \lambda\mathbf{v}$。
- **矩阵对角化**（第22课）：$A = PDP^{-1}$，其中 $D$ 是对角矩阵，$P$ 的列是特征向量。
- **复数特征值**（第23课）：$\lambda = a \pm bi$ 意味着旋转和缩放。
- **一阶ODE $\frac{dx}{dt} = \alpha x$ 的解**（第24–25课）：$x(t) = x_0 e^{\alpha t}$。
- **二阶ODE的求解**（第26–28课）：特征方程、齐次解、非齐次解。

这些线索在今天全部汇聚。你即将看到它们不是彼此独立的知识点，而是同一幅图景的不同侧面。

---

## 1. 开场：两种离子，一张膜，一个耦合的世界

在前面的课程中，我们研究的ODE只有一个未知函数。膜上的浓度 $c(t)$ 衰减，弹簧的位移 $x(t)$ 振荡——都是单一的量在变化。

但真实的膜过程几乎从来不只涉及一种物质。

考虑一张荷电纳滤膜。钠离子 Na$^+$ 和氯离子 Cl$^-$ 同时透过膜。它们不是各走各的——由于电中性约束，钠离子跑得快时会拽着氯离子加速，氯离子跑得慢时会拖住钠离子减速。两种离子的传输**耦合**在一起。

设 $c_1(t)$ 为膜内某位置Na$^+$的浓度偏差（相对于平衡值），$c_2(t)$ 为Cl$^-$的浓度偏差。一个简化的线性模型可以写成：

$$
\frac{dc_1}{dt} = -3c_1 + c_2
$$

$$
\frac{dc_2}{dt} = 2c_1 - 4c_2
$$

第一个方程说：Na$^+$浓度的变化率取决于自身的扩散衰减（$-3c_1$），加上Cl$^-$的电场拖曳效应（$+c_2$）。第二个方程说：Cl$^-$的变化率取决于自身的衰减（$-4c_2$），加上Na$^+$的拖曳（$+2c_1$）。

注意：$c_1$ 的变化率不仅依赖于 $c_1$，还依赖于 $c_2$。反过来也一样。这就是**耦合**——每个变量的命运都牵连着另一个变量。

这种耦合不是膜科学的特例，而是普遍规律。生态学中捕食者和猎物的数量相互影响。电路中电容和电感的电荷与电流互相耦合。化学反应网络中各物质浓度彼此关联。耦合是复杂系统的本质特征。

现在看看这两个方程的形式。如果我们定义向量和矩阵：

$$
\mathbf{c}(t) = \begin{bmatrix} c_1(t) \\ c_2(t) \end{bmatrix}, \qquad A = \begin{bmatrix} -3 & 1 \\ 2 & -4 \end{bmatrix}
$$

那么两个方程可以压缩为一个：

$$
\mathbf{c}'(t) = A\,\mathbf{c}(t)
$$

或者更简洁地：

$$
\mathbf{c}' = A\mathbf{c}
$$

**这就是线性ODE系统的标准形式。**

停下来感受一下这个时刻。你在第22课学对角化时，可能在想"这些抽象操作有什么用"。现在你看到了——耦合的微分方程系统，用矩阵一笔写下。接下来你将看到，**对角化就是解耦**，**特征值就是衰减/增长率**，**特征向量就是系统运动的本征方向**。

线性代数不是ODE的一个辅助工具。线性代数**就是**线性ODE系统的本体。

---

## 2. 求解的核心思想：从耦合到解耦

面对 $\mathbf{c}' = A\mathbf{c}$，我们的策略是什么？

回忆一阶标量方程 $x' = \alpha x$ 的解是 $x(t) = x_0 e^{\alpha t}$。自然的猜测：向量方程的解也许形如

$$
\mathbf{c}(t) = \mathbf{v}\, e^{\lambda t}
$$

其中 $\mathbf{v}$ 是一个常数向量，$\lambda$ 是一个标量。代入 $\mathbf{c}' = A\mathbf{c}$：

$$
\lambda \mathbf{v}\, e^{\lambda t} = A\mathbf{v}\, e^{\lambda t}
$$

消去 $e^{\lambda t}$（它永远不为零）：

$$
A\mathbf{v} = \lambda\mathbf{v}
$$

这正是特征值方程！所以 $\mathbf{c}(t) = \mathbf{v}e^{\lambda t}$ 是 $\mathbf{c}' = A\mathbf{c}$ 的解，当且仅当 $\lambda$ 是 $A$ 的特征值、$\mathbf{v}$ 是对应的特征向量。

**每一对特征值-特征向量都提供一个解。** 如果 $A$ 是 $n \times n$ 的且有 $n$ 个线性无关的特征向量 $\mathbf{v}_1, \ldots, \mathbf{v}_n$（对应特征值 $\lambda_1, \ldots, \lambda_n$），那么通解是：

$$
\mathbf{c}(t) = c_1\mathbf{v}_1 e^{\lambda_1 t} + c_2\mathbf{v}_2 e^{\lambda_2 t} + \cdots + c_n \mathbf{v}_n e^{\lambda_n t}
$$

常数 $c_1, \ldots, c_n$ 由初始条件 $\mathbf{c}(0)$ 决定。

这个结果告诉你一件深刻的事：**解沿着特征向量的方向分解，每个方向上独立地指数增长或衰减，速率由对应的特征值决定。**

---

## 3. 精讲例题：解耦离子传输系统

回到开篇的离子传输问题。$A = \begin{bmatrix} -3 & 1 \\ 2 & -4 \end{bmatrix}$，初始条件 $\mathbf{c}(0) = \begin{bmatrix} 5 \\ 3 \end{bmatrix}$。

**第一步：求特征值。**

$$
\det(A - \lambda I) = (-3-\lambda)(-4-\lambda) - 2 = \lambda^2 + 7\lambda + 10 = (\lambda + 2)(\lambda + 5) = 0
$$

$$
\lambda_1 = -2, \qquad \lambda_2 = -5
$$

两个都是负的——这意味着两个方向上的运动都在衰减。物理上完全合理：离子浓度偏差最终应该回到零。

**第二步：求特征向量。**

$\lambda_1 = -2$：解 $(A + 2I)\mathbf{v} = \mathbf{0}$：

$$
\begin{bmatrix} -1 & 1 \\ 2 & -2 \end{bmatrix} \mathbf{v} = \mathbf{0} \implies v_1 = v_2 \implies \mathbf{v}_1 = \begin{bmatrix} 1 \\ 1 \end{bmatrix}
$$

$\lambda_2 = -5$：解 $(A + 5I)\mathbf{v} = \mathbf{0}$：

$$
\begin{bmatrix} 2 & 1 \\ 2 & 1 \end{bmatrix} \mathbf{v} = \mathbf{0} \implies 2v_1 + v_2 = 0 \implies \mathbf{v}_2 = \begin{bmatrix} 1 \\ -2 \end{bmatrix}
$$

**第三步：写通解。**

$$
\mathbf{c}(t) = c_1 \begin{bmatrix} 1 \\ 1 \end{bmatrix} e^{-2t} + c_2 \begin{bmatrix} 1 \\ -2 \end{bmatrix} e^{-5t}
$$

**第四步：用初始条件确定常数。**

$$
\mathbf{c}(0) = c_1\begin{bmatrix} 1 \\ 1 \end{bmatrix} + c_2 \begin{bmatrix} 1 \\ -2 \end{bmatrix} = \begin{bmatrix} 5 \\ 3 \end{bmatrix}
$$

解方程组 $c_1 + c_2 = 5$，$c_1 - 2c_2 = 3$，得 $c_1 = 13/3$，$c_2 = 2/3$。

**最终解：**

$$
\begin{bmatrix} c_1(t) \\ c_2(t) \end{bmatrix} = \frac{13}{3} \begin{bmatrix} 1 \\ 1 \end{bmatrix} e^{-2t} + \frac{2}{3} \begin{bmatrix} 1 \\ -2 \end{bmatrix} e^{-5t}
$$

展开为分量形式：

$$
c_1(t) = \frac{13}{3}e^{-2t} + \frac{2}{3}e^{-5t}, \qquad c_2(t) = \frac{13}{3}e^{-2t} - \frac{4}{3}e^{-5t}
$$

**理解这个解。** 它由两个"模态"叠加而成：

- **慢模态**（$\lambda_1 = -2$）：沿方向 $\begin{bmatrix} 1 \\ 1 \end{bmatrix}$ 以时间常数 $\tau_1 = 1/2$ 衰减。在这个模态中，Na$^+$ 和 Cl$^-$ 以**相同的比例**一起衰减——它们步调一致。
- **快模态**（$\lambda_2 = -5$）：沿方向 $\begin{bmatrix} 1 \\ -2 \end{bmatrix}$ 以时间常数 $\tau_2 = 1/5$ 衰减。在这个模态中，Na$^+$ 增加时 Cl$^-$ 减少——它们反向变化。

快模态先消亡（因为 $|-5| > |-2|$）。长时间后，系统行为由慢模态主导——两种离子以 $1:1$ 的比例同步衰减到零。

**这就是对角化的物理意义：找到系统的本征运动模态，每个模态独立地演化。**

---

## 4. 对角化就是解耦——用矩阵语言重述

让我用更系统化的语言把上面的求解过程重新表述一遍，因为这个结构在所有线性系统中反复出现。

设 $A$ 可对角化：$A = PDP^{-1}$，其中 $P = [\mathbf{v}_1 \;\; \mathbf{v}_2]$，$D = \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}$。

做变量替换 $\mathbf{c}(t) = P\mathbf{y}(t)$，即用特征向量基来表示状态向量。代入 $\mathbf{c}' = A\mathbf{c}$：

$$
P\mathbf{y}' = A(P\mathbf{y}) = (PDP^{-1})P\mathbf{y} = PD\mathbf{y}
$$

左乘 $P^{-1}$：

$$
\mathbf{y}' = D\mathbf{y}
$$

写开来：

$$
y_1' = \lambda_1 y_1, \qquad y_2' = \lambda_2 y_2
$$

**耦合消失了！** 在特征向量坐标下，系统变成了两个独立的标量方程。每个方程的解是 $y_k(t) = y_k(0)\,e^{\lambda_k t}$，毫无难度。

最后变换回原坐标：$\mathbf{c}(t) = P\mathbf{y}(t)$。

整个过程可以图解为：


原坐标（耦合的）          特征向量坐标（解耦的）
lua复制代码   c' = Ac        变量替换         y' = Dy
                 --------->

[c₁]   [-3  1] [c₁]         [y₁]   [λ₁  0] [y₁]
[c₂] = [ 2 -4] [c₂]         [y₂] = [ 0 λ₂] [y₂]
csharp复制代码                             ↓ 独立求解
   c(t) = P y(t)
                 <---------  y₁(t) = y₁(0)e^{λ₁t}
                  变换回去    y₂(t) = y₂(0)e^{λ₂t}

rust复制代码
**对角化的本质是找到一个坐标系，在其中复杂的耦合系统变成了简单的独立系统。** 这是线性代数送给微分方程最珍贵的礼物。

---

## 5. 复特征值的情况：旋转登场

如果特征值是复数呢？我们在第23课已经为此做好了准备。

**例题 2.** 求解 $\mathbf{x}' = A\mathbf{x}$，其中 $A = \begin{bmatrix} -2 & -5 \\ 2 & -2 \end{bmatrix}$，$\mathbf{x}(0) = \begin{bmatrix} 3 \\ 0 \end{bmatrix}$。

**特征值：**

$$
\det(A - \lambda I) = (-2-\lambda)^2 + 10 = \lambda^2 + 4\lambda + 14 = 0
$$

$$
\lambda = \frac{-4 \pm \sqrt{16 - 56}}{2} = -2 \pm i\sqrt{10}
$$

复特征值！记 $\lambda = -2 + i\sqrt{10}$，$\bar{\lambda} = -2 - i\sqrt{10}$。

**特征向量：** 对 $\lambda = -2 + i\sqrt{10}$：

$$
(A - \lambda I)\mathbf{v} = \begin{bmatrix} -i\sqrt{10} & -5 \\ 2 & -i\sqrt{10} \end{bmatrix}\mathbf{v} = \mathbf{0}
$$

从第二行：$2v_1 = i\sqrt{10}\,v_2$，取 $v_2 = 2$，得 $v_1 = i\sqrt{10}$。

$$
\mathbf{v} = \begin{bmatrix} i\sqrt{10} \\ 2 \end{bmatrix} = \underbrace{\begin{bmatrix} 0 \\ 2 \end{bmatrix}}_{\text{Re}\,\mathbf{v}} + i \underbrace{\begin{bmatrix} \sqrt{10} \\ 0 \end{bmatrix}}_{\text{Im}\,\mathbf{v}}
$$

**构造实解。** 利用 $e^{(a+bi)t} = e^{at}(\cos bt + i\sin bt)$：

$$
\mathbf{v}e^{\lambda t} = \begin{bmatrix} i\sqrt{10} \\ 2 \end{bmatrix} e^{-2t}(\cos\sqrt{10}\,t + i\sin\sqrt{10}\,t)
$$

取实部和虚部，得到两个线性无关的实解：

$$
\mathbf{y}_1(t) = e^{-2t}\left[\begin{bmatrix} 0 \\ 2 \end{bmatrix}\cos\sqrt{10}\,t - \begin{bmatrix} \sqrt{10} \\ 0 \end{bmatrix}\sin\sqrt{10}\,t\right]
$$

$$
\mathbf{y}_2(t) = e^{-2t}\left[\begin{bmatrix} 0 \\ 2 \end{bmatrix}\sin\sqrt{10}\,t + \begin{bmatrix} \sqrt{10} \\ 0 \end{bmatrix}\cos\sqrt{10}\,t\right]
$$

通解 $\mathbf{x}(t) = c_1\mathbf{y}_1(t) + c_2\mathbf{y}_2(t)$，由初始条件确定 $c_1, c_2$。

**关键理解。** 解中出现了 $e^{-2t}$（衰减包络）乘以 $\cos\sqrt{10}\,t$ 和 $\sin\sqrt{10}\,t$（振荡）。这就是**衰减振荡**——系统一边旋转一边收缩，螺旋地趋向原点。

规律非常清晰：**特征值 $\lambda = a \pm bi$ 中，$a$ 控制衰减/增长，$b$ 控制振荡频率。**

---

## 6. 相平面：不解方程，看见全貌

现在我们来到这节课的下半场——也许是最具实用价值的部分。

一个 $2 \times 2$ 的ODE系统 $\mathbf{x}' = A\mathbf{x}$ 描述的是一个点在平面上的运动。在每个时刻 $t$，状态向量 $\mathbf{x}(t) = \begin{bmatrix} x_1(t) \\ x_2(t) \end{bmatrix}$ 对应平面上的一个点。随着 $t$ 变化，这个点画出一条曲线——这条曲线叫做**轨迹**（trajectory）。

如果我们把所有可能的初始条件对应的轨迹都画出来，就得到了**相图**（phase portrait）——它是系统在 $x_1$-$x_2$ 平面上的完整行为地图。


相图的概念：
x₂
^
|     \  |  /
|      \ | /
|       |/
|--------*-------> x₁    原点是平衡点
|       /|
|      / | 
|     /  |  
|
powershell复制代码
相图最重要的特征是**平衡点**（equilibrium point）。对于 $\mathbf{x}' = A\mathbf{x}$（$A$ 可逆时），唯一的平衡点是原点 $\mathbf{x} = \mathbf{0}$（因为只有 $\mathbf{x} = \mathbf{0}$ 时 $\mathbf{x}' = \mathbf{0}$）。

问题是：轨迹是趋向原点（稳定）还是远离原点（不稳定）？是直线运动还是螺旋运动？

答案完全由 $A$ 的特征值决定。

---

## 7. 平衡点的分类：特征值的判决

这是整门课中最值得你牢记的一张表。对于 $2 \times 2$ 实矩阵 $A$ 的两个特征值 $\lambda_1, \lambda_2$：

### 情况一：两个实特征值，同号为负

$\lambda_1 < \lambda_2 < 0$。

两个方向都在衰减，所有轨迹趋向原点。原点是**稳定节点**（stable node / attractor）。

轨迹沿 $\mathbf{v}_2$（较慢衰减的方向）趋近，因为快模态先消亡。


markdown复制代码 稳定节点

   x₂
    ^  v₂（慢衰减方向）
 \  | /
  \ |/

-----*-----> x₁
/|
/ | 
v₁（快衰减方向）
所有轨迹 --> 原点
powershell复制代码
### 情况二：两个实特征值，同号为正

$0 < \lambda_1 < \lambda_2$。

两个方向都在增长，所有轨迹远离原点。原点是**不稳定节点**（unstable node / repeller）。相图和稳定节点一样，但箭头方向反转。

### 情况三：两个实特征值，异号

$\lambda_1 < 0 < \lambda_2$。

一个方向在衰减，另一个在增长。原点是**鞍点**（saddle point）。

沿 $\mathbf{v}_1$ 方向（负特征值）的轨迹趋近原点；沿 $\mathbf{v}_2$ 方向（正特征值）的轨迹远离原点。一般轨迹先朝原点靠近，然后掉头远离——像骑马时的鞍面。


markdown复制代码 鞍点

   x₂
    ^
    |    v₁（吸引方向）

\    |    /
\   |   /
-----*--/---> x₁
\ | /
|/     v₂（排斥方向）
|
ruby复制代码
鞍点永远是不稳定的——只要初始条件不恰好在吸引方向上，轨迹最终都会沿排斥方向发散。

### 情况四：共轭复特征值，实部为负

$\lambda = a \pm bi$，$a < 0$。

衰减振荡。轨迹螺旋地收缩到原点。原点是**稳定螺旋点**（stable spiral）。


sql复制代码 稳定螺旋点

   x₂
    ^
  .---.
 /  .  \
| / * \ |
 \  '  /
  '---'

------+-------> x₁
ruby复制代码
### 情况五：共轭复特征值，实部为正

$\lambda = a \pm bi$，$a > 0$。

增长振荡。轨迹向外螺旋。原点是**不稳定螺旋点**（unstable spiral）。

### 情况六：纯虚特征值

$\lambda = \pm bi$，$a = 0$。

纯振荡，没有衰减也没有增长。轨迹是闭合的椭圆。原点是**中心**（center）。


lua复制代码 中心

   x₂
    ^
  .---.
 /     \
|   *   |
 \     /
  '---'

------+-------> x₁
ruby复制代码
这是保守系统的特征——无阻尼弹簧、理想钟摆、无耗散的离子振荡。

---

## 8. 汇总判据表

$$
\boxed{
\begin{array}{lll}
\textbf{特征值类型} & \textbf{平衡点类型} & \textbf{稳定性} \\[4pt]
\lambda_1 < \lambda_2 < 0 & \text{稳定节点} & \text{渐近稳定} \\[2pt]
0 < \lambda_1 < \lambda_2 & \text{不稳定节点} & \text{不稳定} \\[2pt]
\lambda_1 < 0 < \lambda_2 & \text{鞍点} & \text{不稳定} \\[2pt]
a \pm bi,\; a < 0 & \text{稳定螺旋} & \text{渐近稳定} \\[2pt]
a \pm bi,\; a > 0 & \text{不稳定螺旋} & \text{不稳定} \\[2pt]
\pm bi\;(a = 0) & \text{中心} & \text{稳定（非渐近）}
\end{array}
}
$$

**核心原则：特征值实部的符号决定稳定性。虚部决定是否振荡。**

- 所有特征值实部 $< 0$ → 渐近稳定（轨迹趋向原点）
- 任何一个特征值实部 $> 0$ → 不稳定（轨迹远离原点）
- 实部 $= 0$（且无正实部特征值）→ 中性稳定（不趋近也不远离）

这个判据不需要你解方程。**你只需要算特征值，看一眼符号，就知道系统的长期命运。** 这就是定性分析的力量——用最少的计算获取最关键的信息。

---

## 9. 精讲例题：不解方程，只看特征值

**例题 3.** 判断以下系统的平衡点类型和稳定性（不需要求通解）。

(a) $A = \begin{bmatrix} -1 & 2 \\ 0 & -3 \end{bmatrix}$

$A$ 是上三角矩阵，特征值就是对角线元素：$\lambda_1 = -1$，$\lambda_2 = -3$。都是负的。→ **稳定节点**，渐近稳定。

(b) $A = \begin{bmatrix} 1 & -5 \\ 1 & -1 \end{bmatrix}$

$\text{tr}(A) = 0$，$\det(A) = -1 + 5 = 4$。特征方程 $\lambda^2 - 0 \cdot \lambda + 4 = 0$，$\lambda = \pm 2i$。纯虚特征值。→ **中心**，稳定但非渐近稳定。轨迹是围绕原点的闭合椭圆。

(c) $A = \begin{bmatrix} 2 & -1 \\ 3 & -2 \end{bmatrix}$

$\text{tr}(A) = 0$，$\det(A) = -4 + 3 = -1$。特征方程 $\lambda^2 - 1 = 0$，$\lambda = \pm 1$。一正一负。→ **鞍点**，不稳定。

注意在(b)和(c)中，迹（trace）都等于零，但行列式的符号不同，导致了完全不同的行为。这引出一个有用的快捷判据：

> **对于 $2 \times 2$ 矩阵 $A$：** 令 $\tau = \text{tr}(A)$，$\Delta = \det(A)$。特征值由 $\lambda^2 - \tau\lambda + \Delta = 0$ 给出。
>
> - $\Delta < 0$ → 鞍点（特征值一正一负）
> - $\Delta > 0$ 且 $\tau < 0$ → 稳定（节点或螺旋，取决于 $\tau^2 - 4\Delta$ 的符号）
> - $\Delta > 0$ 且 $\tau > 0$ → 不稳定
> - $\Delta > 0$ 且 $\tau = 0$ → 中心

---

## 10. 画相图：把判断变成图像

让我们为一个具体的系统画出完整的相图。

**例题 4.** $A = \begin{bmatrix} 1 & -2 \\ 3 & -4 \end{bmatrix}$。

**特征值：** $\text{tr} = -3$，$\det = -4 + 6 = 2$。$\lambda^2 + 3\lambda + 2 = (\lambda+1)(\lambda+2) = 0$。

$\lambda_1 = -1$，$\lambda_2 = -2$。都是负的 → **稳定节点**。

**特征向量：**

$\lambda_1 = -1$：$(A + I)\mathbf{v} = \begin{bmatrix} 2 & -2 \\ 3 & -3 \end{bmatrix}\mathbf{v} = \mathbf{0}$，$\mathbf{v}_1 = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$。

$\lambda_2 = -2$：$(A + 2I)\mathbf{v} = \begin{bmatrix} 3 & -2 \\ 3 & -2 \end{bmatrix}\mathbf{v} = \mathbf{0}$，$\mathbf{v}_2 = \begin{bmatrix} 2 \\ 3 \end{bmatrix}$。

**画相图的步骤：**

1. 画出两条特征向量方向（直线 $L_1$：方向 $(1,1)$，$L_2$：方向 $(2,3)$）。这些直线上的轨迹是直线运动——沿特征方向直奔原点。

2. $L_1$ 上的衰减速率为 $e^{-t}$（慢），$L_2$ 上为 $e^{-2t}$（快）。所以 $L_2$ 是"快方向"，$L_1$ 是"慢方向"。

3. 一般轨迹：起初快模态和慢模态都有贡献。随着时间推移，快模态先消亡，轨迹越来越接近慢方向 $L_1$。最终所有轨迹**切线地**沿 $L_1$ 方向进入原点。


perl复制代码 x₂
  ^
  |   /  L₂ (快, λ₂=-2)
  |  / 
  | /  .....
  |/..........  一般轨迹先弯曲，最终沿 L₁ 进入原点

----*-----------/-------> x₁
/|          /
/ |    L₁ (慢, λ₁=-1)
/  |
shell复制代码
> **动手建议：** 下面的Python代码能画出漂亮的相图。请亲手运行它。
>
> ```python
> import numpy as np
> import matplotlib.pyplot as plt
> from scipy.integrate import solve_ivp
>
> A = np.array([[1, -2], [3, -4]])
>
> def system(t, x):
>     return A @ x
>
> fig, ax = plt.subplots(figsize=(8, 8))
> # 从多个初始点出发画轨迹
> for angle in np.linspace(0, 2*np.pi, 24, endpoint=False):
>     for r in [1, 2, 3]:
>         x0 = [r*np.cos(angle), r*np.sin(angle)]
>         sol = solve_ivp(system, [0, 8], x0, dense_output=True,
>                         max_step=0.05)
>         t = np.linspace(0, 8, 300)
>         x = sol.sol(t)
>         ax.plot(x[0], x[1], 'b-', lw=0.5, alpha=0.6)
>         ax.annotate('', xy=(x[0][-1], x[1][-1]),
>                     xytext=(x[0][-2], x[1][-2]),
>                     arrowprops=dict(arrowstyle='->', color='b'))
>
> # 画特征向量方向
> for v, label in [([1,1], 'v₁ (slow)'), ([2,3], 'v₂ (fast)')]:
>     v = np.array(v, dtype=float)
>     v = v / np.linalg.norm(v) * 3.5
>     ax.plot([-v[0], v[0]], [-v[1], v[1]], 'r--', lw=1.5)
>
> ax.set_xlim(-4, 4); ax.set_ylim(-4, 4)
> ax.set_aspect('equal'); ax.grid(True, alpha=0.3)
> ax.set_title('Phase portrait: stable node')
> plt.show()
> ```

---

## 11. 定性分析的威力：一个更复杂的例子

**例题 5.** 一个简化的膜生物反应器模型中，基质浓度 $s$ 和微生物浓度 $b$ 在平衡点附近的线性化方程为：

$$
\begin{bmatrix} s' \\ b' \end{bmatrix} = \begin{bmatrix} -2.5 & -0.8 \\ 1.5 & 0.2 \end{bmatrix} \begin{bmatrix} s \\ b \end{bmatrix}
$$

不解方程，判断系统的长期行为。

**计算：** $\tau = -2.5 + 0.2 = -2.3$，$\Delta = (-2.5)(0.2) - (-0.8)(1.5) = -0.5 + 1.2 = 0.7$。

判别式 $\tau^2 - 4\Delta = 5.29 - 2.8 = 2.49 > 0$。

特征值为实数：$\lambda = \frac{-2.3 \pm \sqrt{2.49}}{2} = \frac{-2.3 \pm 1.578}{2}$。

$$
\lambda_1 \approx -0.361, \qquad \lambda_2 \approx -1.939
$$

两个都是负实数 → **稳定节点**。系统渐近稳定，基质浓度和微生物浓度的偏差都会衰减回平衡值。

如果反应器的某个参数变化，使得特征值变成 $\lambda = -1.15 \pm 0.5i$（$\tau^2 - 4\Delta < 0$），那就变成了**稳定螺旋**——浓度在回到平衡值的过程中会经历衰减振荡。物理上，这对应"基质-微生物"之间的负反馈产生了振荡：基质多 → 微生物增长 → 基质被消耗 → 微生物减少 → 基质恢复……如此往复，振幅逐渐衰减。

**整个分析只用了迹和行列式——没有解过一个方程。**

---

## 12. ODE部分的完整地图

让我们站在高处回顾整个ODE旅程。


┌─────────────────────────────────────────────────┐
│              ODE 的完整地图                       │
│                                                   │
│  起点：dc/dt = -kc（第24课）                      │
│    ↓                                              │
│  一阶方程：分离变量（第25课）                     │
│    ↓                                              │
│  二阶齐次方程：特征方程（第26–27课）              │
│    ↓                                              │
│  二阶非齐次方程：y = y_h + y_p（第28课）         │
│    ↓                                              │
│  级数解法（第29课）、Laplace变换（第30课）        │
│    ↓                                              │
│  ODE系统：x' = Ax → 特征值/向量（本课）          │
│    ↓                                              │
│  定性分析：相图、稳定性判据（本课）               │
└─────────────────────────────────────────────────┘
ruby复制代码
**三条主线贯穿始终：**

1. **解析方法**——找到精确的公式解。从最简单的指数函数 $ce^{kt}$ 出发，到特征方程、待定系数法、常数变易法、级数解……每种方法打开一扇新的门，覆盖更广的方程类型。

2. **代数结构**——线性方程的解具有 $y = y_h + y_p$ 的结构；ODE系统的解通过对角化分解为独立模态。线性代数的语言在ODE中原封不动地重现。

3. **定性分析**——不解方程就能判断系统行为。方向场（第24课）→ 相线（第24课）→ 相平面与相图（本课）。特征值的实部判定稳定性，虚部判定振荡性。

这三条线不是三种互相替代的方法——它们是三种互相补充的视角。解析解给你精确，代数结构给你统一性，定性分析给你全局洞察。掌握ODE就是掌握在这三种视角之间灵活切换的能力。

---

## 13. 展望：从ODE到PDE

我们走了很远。但现在请你思考一个问题。

在整个ODE部分，未知函数只依赖于**一个**自变量——通常是时间 $t$（或空间的一个方向 $x$）。$c(t)$ 是膜上某个固定位置处浓度随时间的变化。$c(x)$ 是某个固定时刻浓度随空间的分布。

但真实的膜传质过程中，浓度 $c$ 同时随时间和空间变化：$c = c(x, t)$。

一个扩散过程的完整描述不是

$$
\frac{dc}{dt} = -kc \qquad \text{（ODE：只有时间变化）}
$$

而是

$$
\frac{\partial c}{\partial t} = D\frac{\partial^2 c}{\partial x^2} \qquad \text{（PDE：时间和空间同时变化）}
$$

这是**偏微分方程**（Partial Differential Equation，PDE）——其中包含了对不同变量的偏导数。

从ODE到PDE，就像从一维的数轴到二维的平面。ODE描述的是沿着一条线的变化；PDE描述的是在一片区域上的变化。ODE的解是曲线；PDE的解是曲面。

但好消息是：**你在ODE部分学到的工具不会作废。** 恰恰相反，求解PDE的最重要方法之一——**分离变量法**——其核心思想就是把PDE拆解为若干个ODE，然后用你已经掌握的ODE技术逐个求解。Fourier级数、特征函数展开——这些PDE的核心工具，都直接建筑在ODE的地基之上。

所以ODE不是一个独立的章节，而是通往更广阔世界的阶梯。你现在站在阶梯的顶端，准备踏入下一个层次。

> *"ODE描述的是在一个维度上的变化。但真实的膜传质同时在空间和时间上变化——这就把我们带进了偏微分方程的世界。"*

---

## 练习

**练习1.** 求解ODE系统 $\mathbf{x}' = A\mathbf{x}$，其中 $A = \begin{bmatrix} 4 & -3 \\ 2 & -1 \end{bmatrix}$，$\mathbf{x}(0) = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$。画出相图的草图。

**提示：** 特征方程 $\lambda^2 - 3\lambda + 2 = 0$，$\lambda_1 = 1$，$\lambda_2 = 2$。两个正特征值 → 不稳定节点。特征向量 $\mathbf{v}_1 = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$（对应 $\lambda = 1$），$\mathbf{v}_2 = \begin{bmatrix} 3 \\ 2 \end{bmatrix}$（对应 $\lambda = 2$）。通解 $\mathbf{x}(t) = c_1\begin{bmatrix} 1 \\ 1 \end{bmatrix}e^t + c_2\begin{bmatrix} 3 \\ 2 \end{bmatrix}e^{2t}$。由 $\mathbf{x}(0)$：$c_1 + 3c_2 = 1$，$c_1 + 2c_2 = 2$，解得 $c_2 = -1$，$c_1 = 4$。轨迹从原点附近沿 $\mathbf{v}_2$ 方向快速远离。

---

**练习2.** 不解方程，仅通过计算迹和行列式，判断以下矩阵对应的ODE系统 $\mathbf{x}' = A\mathbf{x}$ 的平衡点类型。

(a) $A = \begin{bmatrix} -1 & 3 \\ -3 & -1 \end{bmatrix}$

(b) $A = \begin{bmatrix} 2 & 1 \\ -1 & 0 \end{bmatrix}$

(c) $A = \begin{bmatrix} -3 & 4 \\ -2 & 1 \end{bmatrix}$

**提示：** (a) $\tau = -2$，$\Delta = 1 + 9 = 10$，$\tau^2 - 4\Delta = 4 - 40 < 0$。复特征值，实部 $= \tau/2 = -1 < 0$。**稳定螺旋。**
(b) $\tau = 2$，$\Delta = 0 + 1 = 1$，$\tau^2 - 4\Delta = 4 - 4 = 0$。重特征值 $\lambda = 1 > 0$。**不稳定节点**（退化情况）。
(c) $\tau = -2$，$\Delta = -3 + 8 = 5$，$\tau^2 - 4\Delta = 4 - 20 < 0$。复特征值，实部 $= -1 < 0$。**稳定螺旋。**

---

**练习3.** 对于系统 $\mathbf{x}' = \begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}\mathbf{x}$：

(a) 求特征值。(b) 不解方程，判断原点是什么类型的平衡点。(c) 求通解并验证你的判断。

**提示：** (a) $\lambda = \pm i$。(b) 纯虚特征值 → **中心**，闭合轨道。(c) 通解 $\mathbf{x}(t) = c_1\begin{bmatrix} \cos t \\ -\sin t \end{bmatrix} + c_2\begin{bmatrix} \sin t \\ \cos t \end{bmatrix}$。轨迹确实是椭圆。注意这个矩阵就是第23课的旋转矩阵 $\begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}$（顺时针旋转 $90°$）——一切自洽。

---

**练习4.** 一个膜反应器中两种溶质的耦合传输模型为：

$$
\mathbf{c}' = \begin{bmatrix} -k_1 & \alpha \\ \alpha & -k_2 \end{bmatrix}\mathbf{c}
$$

其中 $k_1, k_2 > 0$ 是各自的衰减常数，$\alpha > 0$ 是耦合强度。证明：当 $\alpha < \sqrt{k_1 k_2}$ 时系统渐近稳定。当 $\alpha = 0$（无耦合）时，给出通解的物理解释。

**提示：** $\tau = -(k_1+k_2) < 0$，$\Delta = k_1 k_2 - \alpha^2$。当 $\alpha < \sqrt{k_1 k_2}$ 时 $\Delta > 0$，且 $\tau < 0$，特征值实部全为负 → 渐近稳定。当 $\alpha = 0$ 时，矩阵是对角的：$c_1(t) = c_1(0)e^{-k_1 t}$，$c_2(t) = c_2(0)e^{-k_2 t}$——两种溶质独立衰减，互不影响。

---

**练习5（开放题）.** 用Python画出以下三个矩阵对应系统的相图，亲眼看到稳定节点、鞍点和稳定螺旋的区别：

$$
A_1 = \begin{bmatrix} -2 & 1 \\ 0 & -3 \end{bmatrix}, \quad A_2 = \begin{bmatrix} 1 & -2 \\ 1 & -1 \end{bmatrix}, \quad A_3 = \begin{bmatrix} -1 & -4 \\ 1 & -1 \end{bmatrix}
$$

**提示：** 用第10节的Python代码模板，只替换矩阵 $A$。$A_1$：$\lambda = -2, -3$（稳定节点）。$A_2$：$\tau = 0$，$\Delta = -1 + 2 = 1$，$\lambda = \pm i$（中心）。$A_3$：$\tau = -2$，$\Delta = 1+4 = 5$，$\tau^2 - 4\Delta = 4 - 20 < 0$，$\lambda = -1 \pm 2i$（稳定螺旋）。在你的屏幕上，你会看到直线收敛、闭合椭圆和向内的螺旋三种截然不同的几何图案——特征值的三种面貌。

---

> **下一部分预告（第六部分：Fourier分析与PDE）**：ODE处理的是只依赖一个变量的函数。但膜内的浓度 $c(x,t)$ 同时依赖于空间 $x$ 和时间 $t$。要描述它，我们需要偏微分方程。而求解PDE最经典的武器——Fourier级数——基于一个惊人的想法：**任何函数都可以分解为正弦和余弦的叠加。** 这把我们带回了Euler公式 $e^{i\theta} = \cos\theta + i\sin\theta$，带回了线性叠加原理，带回了特征值问题——但在一个全新的、无穷维的舞台上。一切你已经学过的东西，都将以更宏大的形式重现。