# 第33课：Fourier 级数——用正弦波拼出任意形状

---

> *1807年，Joseph Fourier 向法国科学院提交了一篇论文，宣称任何函数——无论多么"不规则"——都可以表示为正弦和余弦函数的无穷级数。评审委员会里包括 Lagrange、Laplace 和 Legendre——那个时代最伟大的三位数学家。Lagrange 不相信。他终其一生都没有接受 Fourier 的断言。*
>
> *Lagrange 错了。但他错得并不愚蠢——Fourier 的说法确实需要精细的修正和条件限定，后来的两个世纪里数学家们为此发展出了整个分支（实分析、泛函分析、测度论）。然而 Fourier 的核心直觉是深刻正确的：复杂的东西可以拆成简单的东西的叠加。这个思想改变了物理学、工程学和数学的面貌。*
>
> *今天，你将亲手学会这种"拆解术"。*

---

## 前置知识检查

- **三角函数的积分**（第15课）：你需要能计算 $\int_0^{2\pi}\sin(mx)\cos(nx)\,dx$ 这类积分。核心工具是积化和差公式。
- **内积与正交投影**（第19课/线性代数）：你知道"把一个向量投影到另一个向量上"意味着什么。Fourier 系数的推导将是这一思想的直接推广——从有限维推广到无穷维。
- **级数的基本概念**（第20课）：你知道无穷级数的部分和、收敛性等基本概念。

---

## 一、动机：为什么要用正弦波？

### 1.1 一个膜过程中的周期信号

假设你在运行一个膜生物反应器。进水水质不是恒定的——它每24小时经历一个周期性的变化（白天工业排放多，夜间少）。你测量进水COD浓度 $f(t)$，画出来发现它大致是周期的，但形状不是简单的正弦波——它更像是一个"锯齿"或者"方波"的变形体。

你想建立一个数学模型来描述这个输入信号。一种方式是用分段函数把它精确地写下来——可以做到，但用起来极不方便（尝试对分段函数做 Laplace 变换你就知道了）。

另一种方式是把这个"奇形怪状"的周期信号**拆解为一系列频率不同的正弦波的叠加**：

$$
f(t) \approx \frac{a_0}{2} + a_1\cos\omega t + b_1\sin\omega t + a_2\cos 2\omega t + b_2\sin 2\omega t + \cdots
$$

每一项都是简单的正弦或余弦——**系统对它们的响应你可以逐个计算**（回忆第30课：线性系统对正弦输入的响应仍然是同频率的正弦，只是振幅和相位改变了）。然后利用**叠加原理**——因为系统是线性的——把所有响应加起来就得到了对原始输入的总响应。

这就是 Fourier 级数的工程动机：**把复杂信号变成简单信号的叠加，然后逐个击破。**

### 1.2 更深的理由：正弦波是微分方程的"特征函数"

还有一个更根本的原因解释了为什么正弦波如此特殊。

回忆第27课：常系数齐次ODE $y'' + \omega^2 y = 0$ 的解就是 $\cos\omega t$ 和 $\sin\omega t$。正弦和余弦是**二阶线性微分算子**的特征函数——经过微分（或求解 ODE）后，它们的形式不变。

在下一课你将看到，当我们用分离变量法解偏微分方程（比如热方程或扩散方程）时，空间变量的部分恰好会产生正弦/余弦型的特征函数。而初始条件的处理——"把给定的初始函数用这些特征函数展开"——正是 Fourier 级数的任务。

所以，Fourier 级数不是一个"漂亮但可有可无"的附加工具。它是连接初始条件和 PDE 解的**桥梁**。没有它，分离变量法的最后一步就走不通。

---

## 二、正交性——一切的根基

### 2.1 从向量的正交投影到函数的正交投影

让我们先回忆第19课的一个核心结论。在 $\mathbb{R}^n$ 中，如果 $\{\mathbf{u}_1, \mathbf{u}_2, \ldots, \mathbf{u}_m\}$ 是一组正交基（$\mathbf{u}_i \cdot \mathbf{u}_j = 0$ 当 $i \neq j$），那么任意向量 $\mathbf{v}$ 在这组基上的投影为：

$$
\mathbf{v} = \sum_{k=1}^m c_k \mathbf{u}_k, \quad c_k = \frac{\mathbf{v} \cdot \mathbf{u}_k}{\mathbf{u}_k \cdot \mathbf{u}_k}
$$

每个系数 $c_k$ 都是一个"投影"——$\mathbf{v}$ 在 $\mathbf{u}_k$ 方向上的分量。这个公式之所以这么简洁，完全是因为**正交性**：各个方向互不干扰，你可以独立地计算每个方向上的分量。

现在，Fourier 的关键洞察是：**同样的事情可以在函数空间中做。**

把"向量"换成"函数"，把"点积"换成"积分"，把"有限维基"换成"无穷多个三角函数"——结构完全平行。

### 2.2 函数的"内积"

对于定义在 $[0, 2\pi]$（或更一般地 $[-\pi, \pi]$ 或 $[-L, L]$）上的函数，我们定义**内积**：

$$
\langle f, g \rangle = \int_0^{2\pi} f(t)\,g(t)\,dt
$$

这就是函数版的"点积"。两个函数逐点相乘，然后"求和"（积分是连续版的求和）。

### 2.3 三角函数系的正交性

现在来验证三角函数族

$$
\{1, \cos t, \sin t, \cos 2t, \sin 2t, \cos 3t, \sin 3t, \ldots\}
$$

关于上述内积是**正交**的。这意味着不同的成员之间的内积为零。

**验证 $\cos mt$ 与 $\cos nt$ 的正交性**（$m \neq n$，都是正整数）：

用积化和差公式 $\cos\alpha\cos\beta = \frac{1}{2}[\cos(\alpha+\beta) + \cos(\alpha-\beta)]$：

$$
\langle \cos mt, \cos nt \rangle = \int_0^{2\pi} \cos mt \cos nt\,dt = \frac{1}{2}\int_0^{2\pi}[\cos(m+n)t + \cos(m-n)t]\,dt
$$

因为 $m+n$ 和 $m-n$ 都是非零整数，$\cos(kt)$ 在 $[0, 2\pi]$ 上的积分为零（完整的周期数）：

$$
= \frac{1}{2}\left[\frac{\sin(m+n)t}{m+n} + \frac{\sin(m-n)t}{m-n}\right]_0^{2\pi} = 0
$$

类似地可以验证（请你自己做！）：

- $\langle \sin mt, \sin nt \rangle = 0$ 当 $m \neq n$
- $\langle \cos mt, \sin nt \rangle = 0$ 对所有正整数 $m, n$（**即使 $m = n$**！余弦和正弦永远正交。）
- $\langle 1, \cos nt \rangle = 0$，$\langle 1, \sin nt \rangle = 0$ 对所有正整数 $n$

还需要知道每个函数和自己的内积（"长度的平方"）：

$$
\langle 1, 1 \rangle = \int_0^{2\pi} 1\,dt = 2\pi
$$

$$
\langle \cos nt, \cos nt \rangle = \int_0^{2\pi} \cos^2(nt)\,dt = \pi \quad (n \geq 1)
$$

$$
\langle \sin nt, \sin nt \rangle = \int_0^{2\pi} \sin^2(nt)\,dt = \pi \quad (n \geq 1)
$$

（这些用半角公式 $\cos^2\theta = \frac{1+\cos 2\theta}{2}$ 即可计算。）

### 2.4 小结：三角函数系是一组"正交基"

总结成一张表：

| 内积 | $1$ | $\cos nt$ | $\sin nt$ |
|---|---|---|---|
| $1$ | $2\pi$ | $0$ | $0$ |
| $\cos mt$ | $0$ | $\pi\delta_{mn}$ | $0$ |
| $\sin mt$ | $0$ | $0$ | $\pi\delta_{mn}$ |

其中 $\delta_{mn}$ 是 Kronecker 记号：$m = n$ 时为 $1$，否则为 $0$。

这和 $\mathbb{R}^n$ 中正交基的情况完全平行。正交性保证了：当你把一个函数"投影"到某个基函数上时，其他基函数完全不干扰。

---

## 三、Fourier 系数的推导

### 3.1 设定

假设一个周期为 $2\pi$ 的函数 $f(t)$ 可以展开为：

$$
f(t) = \frac{a_0}{2} + \sum_{n=1}^{\infty} (a_n \cos nt + b_n \sin nt)
$$

（常数项写成 $a_0/2$ 而不是 $a_0$，纯粹是为了让后面的系数公式统一——你马上就会看到原因。）

我们的任务是：**给定 $f(t)$，求系数 $a_n$ 和 $b_n$。**

### 3.2 用正交性"榨取"系数

这里就是正交投影思想的精彩应用。

**求 $a_0$**：对等式两边在 $[0, 2\pi]$ 上积分，即计算 $\langle f, 1 \rangle$：

$$
\int_0^{2\pi} f(t)\,dt = \frac{a_0}{2}\int_0^{2\pi} 1\,dt + \sum_{n=1}^{\infty}\left[a_n\underbrace{\int_0^{2\pi}\cos nt\,dt}_{=0} + b_n\underbrace{\int_0^{2\pi}\sin nt\,dt}_{=0}\right]
$$

所有正弦和余弦项在一个完整周期上的积分都是零——只剩下常数项：

$$
\int_0^{2\pi} f(t)\,dt = \frac{a_0}{2} \cdot 2\pi = \pi a_0
$$

$$
\boxed{a_0 = \frac{1}{\pi}\int_0^{2\pi} f(t)\,dt}
$$

**求 $a_m$**（$m \geq 1$）：计算 $\langle f, \cos mt \rangle$——等式两边同乘 $\cos mt$ 再积分：

$$
\int_0^{2\pi} f(t)\cos mt\,dt = \frac{a_0}{2}\underbrace{\int_0^{2\pi}\cos mt\,dt}_{=0} + \sum_{n=1}^{\infty}\left[a_n\underbrace{\int_0^{2\pi}\cos nt\cos mt\,dt}_{=\pi\delta_{mn}} + b_n\underbrace{\int_0^{2\pi}\sin nt\cos mt\,dt}_{=0}\right]
$$

正交性把求和式中的无穷多项全部消灭——只有 $n = m$ 这一项存活：

$$
\int_0^{2\pi} f(t)\cos mt\,dt = a_m \cdot \pi
$$

$$
\boxed{a_m = \frac{1}{\pi}\int_0^{2\pi} f(t)\cos mt\,dt}
$$

**求 $b_m$**：完全类似，计算 $\langle f, \sin mt \rangle$：

$$
\boxed{b_m = \frac{1}{\pi}\int_0^{2\pi} f(t)\sin mt\,dt}
$$

注意 $a_0$ 的公式在 $m = 0$ 时和 $a_m$ 的一般公式一致（因为 $\cos(0 \cdot t) = 1$）——这就是我们把常数项写成 $a_0/2$ 的原因。统一之美。

### 3.3 向量空间视角的总结

让我把这一切放在一起，让你看清 Fourier 级数和线性代数之间的完美对应：

| 线性代数（$\mathbb{R}^n$） | Fourier 级数（函数空间） |
|---|---|
| 向量 $\mathbf{v}$ | 函数 $f(t)$ |
| 正交基 $\{\mathbf{u}_1, \ldots, \mathbf{u}_m\}$ | 三角函数系 $\{1, \cos t, \sin t, \cos 2t, \ldots\}$ |
| 点积 $\mathbf{u} \cdot \mathbf{v} = \sum u_i v_i$ | 内积 $\langle f, g \rangle = \int f(t)g(t)\,dt$ |
| 投影系数 $c_k = \frac{\mathbf{v} \cdot \mathbf{u}_k}{\mathbf{u}_k \cdot \mathbf{u}_k}$ | Fourier 系数 $a_n = \frac{\langle f, \cos nt \rangle}{\langle \cos nt, \cos nt \rangle}$ |
| $\mathbf{v} = \sum c_k \mathbf{u}_k$ | $f(t) = \frac{a_0}{2} + \sum (a_n\cos nt + b_n\sin nt)$ |
| 维度 $n$（有限） | 维度 $\infty$（无穷） |

**Fourier 级数就是正交投影在无穷维空间中的实现。** 每个 Fourier 系数都是函数 $f$ 在对应三角函数方向上的"分量"。这不是比喻——这是精确的数学。

---

## 四、精讲例题：方波的 Fourier 级数

### 4.1 定义方波

定义 $[0, 2\pi]$ 上的方波函数：

$$
f(t) = \begin{cases} 1, & 0 < t < \pi \\ -1, & \pi < t < 2\pi \end{cases}
$$

这是工程中最经典的测试信号之一。它有尖锐的跳跃、没有光滑性可言——恰好是对 Fourier 级数的严峻考验。

### 4.2 计算 Fourier 系数

**$a_0$**：

$$
a_0 = \frac{1}{\pi}\int_0^{2\pi} f(t)\,dt = \frac{1}{\pi}\left[\int_0^{\pi} 1\,dt + \int_{\pi}^{2\pi}(-1)\,dt\right] = \frac{1}{\pi}[\pi - \pi] = 0
$$

方波的"平均值"为零——合理，正负对称。

**$a_n$**（$n \geq 1$）：

$$
a_n = \frac{1}{\pi}\left[\int_0^{\pi}\cos nt\,dt + \int_{\pi}^{2\pi}(-\cos nt)\,dt\right]
$$

$$
= \frac{1}{\pi}\left[\frac{\sin nt}{n}\bigg|_0^{\pi} - \frac{\sin nt}{n}\bigg|_{\pi}^{2\pi}\right] = \frac{1}{\pi}\left[\frac{\sin n\pi}{n} - \frac{\sin 2n\pi - \sin n\pi}{n}\right]
$$

因为 $\sin(n\pi) = 0$ 和 $\sin(2n\pi) = 0$ 对所有整数 $n$：

$$
a_n = 0 \quad \text{对所有 } n
$$

所有余弦系数为零！这反映了 $f(t)$ 是一个**奇函数**（关于 $t = \pi$ 的对称中心），而余弦是偶函数——一个奇函数在偶函数方向上的投影当然为零。

**$b_n$**：

$$
b_n = \frac{1}{\pi}\left[\int_0^{\pi}\sin nt\,dt + \int_{\pi}^{2\pi}(-\sin nt)\,dt\right]
$$

$$
= \frac{1}{\pi}\left[-\frac{\cos nt}{n}\bigg|_0^{\pi} + \frac{\cos nt}{n}\bigg|_{\pi}^{2\pi}\right]
$$

$$
= \frac{1}{\pi}\left[\frac{-\cos n\pi + 1}{n} + \frac{\cos 2n\pi - \cos n\pi}{n}\right]
$$

$$
= \frac{1}{n\pi}\left[1 - \cos n\pi + 1 - \cos n\pi\right] = \frac{2(1 - \cos n\pi)}{n\pi}
$$

现在，$\cos(n\pi) = (-1)^n$。当 $n$ 为偶数时 $\cos(n\pi) = 1$，$b_n = 0$。当 $n$ 为奇数时 $\cos(n\pi) = -1$，$b_n = \frac{4}{n\pi}$。

### 4.3 最终结果

$$
\boxed{f(t) = \frac{4}{\pi}\left(\sin t + \frac{1}{3}\sin 3t + \frac{1}{5}\sin 5t + \frac{1}{7}\sin 7t + \cdots\right) = \frac{4}{\pi}\sum_{k=0}^{\infty}\frac{\sin(2k+1)t}{2k+1}}
$$

停下来感受一下这个结果。

一个有棱有角的方波——在 $t = 0$、$\pi$、$2\pi$ 处有不连续跳跃——被表示成了无穷多个光滑正弦波的叠加。每一个正弦波都是连续且光滑的，但它们的**无穷叠加**可以逼近一个不连续函数。

系数 $1/n$ 的衰减告诉你：高频成分（大的 $n$）贡献越来越小。基波 $\sin t$ 贡献最大，三次谐波 $\frac{1}{3}\sin 3t$ 次之，五次谐波更小……方波的"尖锐角"是无穷多个高频正弦波合力制造的——每一个高频成分都很小，但缺一不可。

---

## 五、逼近的演示——用眼睛看收敛

这是本课最重要的实践环节。**请你务必打开 Python，亲手运行以下代码。**

```python
import numpy as np
import matplotlib.pyplot as plt

t = np.linspace(0, 2*np.pi, 1000)

# 方波的精确定义
f_exact = np.where(t < np.pi, 1.0, -1.0)

fig, axes = plt.subplots(2, 2, figsize=(12, 9))
N_values = [1, 3, 10, 50]

for ax, N in zip(axes.flatten(), N_values):
    # 计算 N 项 Fourier 部分和
    f_approx = np.zeros_like(t)
    for k in range(N):
        n = 2*k + 1  # 只有奇数项
        f_approx += (4/(n*np.pi)) * np.sin(n*t)
    
    ax.plot(t, f_exact, 'k--', linewidth=1, alpha=0.5, label='方波')
    ax.plot(t, f_approx, 'b-', linewidth=1.5, label=f'{N} 项')
    ax.set_title(f'前 {N} 个非零项', fontsize=13)
    ax.set_xlim(0, 2*np.pi); ax.set_ylim(-1.5, 1.5)
    ax.legend(fontsize=10); ax.grid(alpha=0.3)

plt.suptitle('方波的 Fourier 逼近', fontsize=15)
plt.tight_layout()
plt.show()

你会看到：


1 项：只有 4πsin⁡t\frac{4}{\pi}\sin tπ4​sint——一条正弦波，大致抓住了方波的"正负交替"但完全没有方波的棱角。


3 项：加入了三次和五次谐波后，波形开始"变方"，但顶部和底部还在波动。


10 项：已经相当像方波了。平坦的顶部和底部清晰可见。但跳跃点附近有明显的"尖耳朵"（overshoot）。


50 项：几乎完美。但跳跃点附近的"尖耳朵"仍然存在——只是变得更窄更尖了，幅度几乎没有减小。


那个顽固的"尖耳朵"——就是 Gibbs 现象。

六、Gibbs 现象——不回避困难
6.1 什么是 Gibbs 现象
1898年，美国物理学家 J. Willard Gibbs 注意到：在不连续点附近，Fourier 部分和不是简单地"逐渐逼近"跳跃——它会过冲（overshoot），产生一个峰值，然后在跳跃的另一侧欠冲（undershoot）。
更令人惊讶的是：无论你取多少项，过冲的幅度永远不会消失。 它大约是跳跃幅度的 8.95%（精确值是 1π∫0πsin⁡tt dt−12≈0.0895\frac{1}{\pi}\int_0^{\pi}\frac{\sin t}{t}\,dt - \frac{1}{2} \approx 0.0895π1​∫0π​tsint​dt−21​≈0.0895，即约 9%）。随着项数增加，过冲变窄但不变矮——它像一只倔强的刺猬，被挤得越来越瘦，但始终竖着同样高度的刺。
lua复制代码            Fourier 部分和在跳跃点附近的行为

    |     *                  
    |    * *     ← 过冲 (~9% 的跳跃幅度)
 1  |---*---*-----------   精确值
    |  *     *
    | *       *  * * *
    |*         **
    +-----|-----|--------→ t
          跳跃点
          
    (N 增大时，过冲变窄但高度不变)

6.2 为什么不影响整体收敛
Gibbs 现象看起来很糟糕——9% 的过冲永远不消失，这难道不意味着 Fourier 级数"不收敛"吗？
不。区别在于你用什么标准衡量"收敛"。
逐点收敛：在连续点处，Fourier 部分和确实收敛到 f(t)f(t)f(t) 的值。在不连续点处，它收敛到左右极限的平均值 f(t+)+f(t−)2\frac{f(t^+) + f(t^-)}{2}2f(t+)+f(t−)​。Gibbs 过冲只发生在不连续点的无穷小邻域内——这个邻域随着项数增加而缩小到零。
均方收敛（L2L^2L2 收敛）：定义误差为
EN=∥f−SN∥2=∫02π[f(t)−SN(t)]2 dtE_N = \left\|f - S_N\right\|^2 = \int_0^{2\pi} \left[f(t) - S_N(t)\right]^2\,dtEN​=∥f−SN​∥2=∫02π​[f(t)−SN​(t)]2dt
其中 SNS_NSN​ 是 NNN 阶 Fourier 部分和。可以证明 EN→0E_N \to 0EN​→0（N→∞N \to \inftyN→∞）——"均方误差趋向零"。Gibbs 过冲虽然高度不减，但宽度趋向零，所以它对面积（积分）的贡献趋向零。
工程上的启示：在膜科学的数值模拟中，如果你用 Fourier 级数来表示初始条件或边界条件中的不连续跳跃（比如阶跃函数），要知道计算结果在跳跃点附近会有约 9% 的振荡。这不是代码 bug——这是数学本身的性质。增加项数不能消除它，只能让它更局部化。如果你需要消除 Gibbs 现象，有专门的技术（如 Lanczos σ\sigmaσ 因子或 Fejér 求和），但那是另一个话题了。

七、一般周期的 Fourier 级数
7.1 推广到周期 2L2L2L
到目前为止我们讨论的是周期为 2π2\pi2π 的情况。但实际的物理信号不一定有 2π2\pi2π 的周期——膜反应器的进水可能以 24 小时为周期，振动膜可能以毫秒为周期。
设 f(t)f(t)f(t) 的周期为 2L2L2L。做变量替换 τ=πt/L\tau = \pi t/Lτ=πt/L（把 [−L,L][-L, L][−L,L] 映射到 [−π,π][-\pi, \pi][−π,π]），或者直接写出结果：
f(t)=a02+∑n=1∞(ancos⁡nπtL+bnsin⁡nπtL)f(t) = \frac{a_0}{2} + \sum_{n=1}^{\infty}\left(a_n\cos\frac{n\pi t}{L} + b_n\sin\frac{n\pi t}{L}\right)f(t)=2a0​​+n=1∑∞​(an​cosLnπt​+bn​sinLnπt​)
系数公式变为：
an=1L∫−LLf(t)cos⁡nπtL dt,bn=1L∫−LLf(t)sin⁡nπtL dta_n = \frac{1}{L}\int_{-L}^{L} f(t)\cos\frac{n\pi t}{L}\,dt, \quad b_n = \frac{1}{L}\int_{-L}^{L} f(t)\sin\frac{n\pi t}{L}\,dtan​=L1​∫−LL​f(t)cosLnπt​dt,bn​=L1​∫−LL​f(t)sinLnπt​dt
基本频率（基波）是 ω1=π/L\omega_1 = \pi/Lω1​=π/L，对应周期 2L2L2L。nnn 次谐波的频率是 nω1=nπ/Ln\omega_1 = n\pi/Lnω1​=nπ/L。
7.2 奇函数与偶函数的简化
如果 f(t)f(t)f(t) 在 [−L,L][-L, L][−L,L] 上是偶函数（f(−t)=f(t)f(-t) = f(t)f(−t)=f(t)），那么 f(t)sin⁡(nπt/L)f(t)\sin(n\pi t/L)f(t)sin(nπt/L) 是奇函数，在对称区间上积分为零。因此 bn=0b_n = 0bn​=0——Fourier 级数只包含余弦项（余弦级数）。
如果 f(t)f(t)f(t) 是奇函数（f(−t)=−f(t)f(-t) = -f(t)f(−t)=−f(t)），那么 an=0a_n = 0an​=0——只包含正弦项（正弦级数）。
这也解释了为什么方波（关于某个中心点的奇函数）的 Fourier 级数中没有余弦项。
7.3 精讲例题：锯齿波
设 f(t)=tf(t) = tf(t)=t，−π<t<π-\pi < t < \pi−π<t<π，周期为 2π2\pi2π 的延拓。这是一个奇函数，所以 an=0a_n = 0an​=0。
bn=1π∫−ππtsin⁡nt dtb_n = \frac{1}{\pi}\int_{-\pi}^{\pi} t\sin nt\,dtbn​=π1​∫−ππ​tsinntdt
分部积分（u=tu = tu=t，dv=sin⁡nt dtdv = \sin nt\,dtdv=sinntdt）：
bn=1π[−tcos⁡ntn∣−ππ+1n∫−ππcos⁡nt dt]b_n = \frac{1}{\pi}\left[-\frac{t\cos nt}{n}\bigg|_{-\pi}^{\pi} + \frac{1}{n}\int_{-\pi}^{\pi}\cos nt\,dt\right]bn​=π1​[−ntcosnt​​−ππ​+n1​∫−ππ​cosntdt]
=1π[−πcos⁡nπn−(−π)cos⁡(−nπ)n]=1π⋅−2πcos⁡nπn=−2(−1)nn=2(−1)n+1n= \frac{1}{\pi}\left[-\frac{\pi\cos n\pi}{n} - \frac{(-\pi)\cos(-n\pi)}{n}\right] = \frac{1}{\pi}\cdot\frac{-2\pi\cos n\pi}{n} = \frac{-2(-1)^n}{n} = \frac{2(-1)^{n+1}}{n}=π1​[−nπcosnπ​−n(−π)cos(−nπ)​]=π1​⋅n−2πcosnπ​=n−2(−1)n​=n2(−1)n+1​
f(t)=t=2(sin⁡t−sin⁡2t2+sin⁡3t3−sin⁡4t4+⋯ )=2∑n=1∞(−1)n+1nsin⁡nt\boxed{f(t) = t = 2\left(\sin t - \frac{\sin 2t}{2} + \frac{\sin 3t}{3} - \frac{\sin 4t}{4} + \cdots\right) = 2\sum_{n=1}^{\infty}\frac{(-1)^{n+1}}{n}\sin nt}f(t)=t=2(sint−2sin2t​+3sin3t​−4sin4t​+⋯)=2n=1∑∞​n(−1)n+1​sinnt​
一个美妙的副产品：令 t=π/2t = \pi/2t=π/2，因为 f(π/2)=π/2f(\pi/2) = \pi/2f(π/2)=π/2：
π2=2(1−0−13+0+15−0−17+⋯ )\frac{\pi}{2} = 2\left(1 - 0 - \frac{1}{3} + 0 + \frac{1}{5} - 0 - \frac{1}{7} + \cdots\right)2π​=2(1−0−31​+0+51​−0−71​+⋯)
π4=1−13+15−17+⋯\frac{\pi}{4} = 1 - \frac{1}{3} + \frac{1}{5} - \frac{1}{7} + \cdots4π​=1−31​+51​−71​+⋯
这就是 Leibniz 公式！π\piπ 的值藏在正弦级数里。Fourier 级数不仅能拆解函数，还能在意想不到的地方产出数论的宝石。

八、Parseval 定理——能量守恒的频域版本
8.1 陈述
如果 f(t)f(t)f(t) 的 Fourier 级数收敛（在均方意义下），则：
1π∫02π[f(t)]2 dt=a022+∑n=1∞(an2+bn2)\frac{1}{\pi}\int_0^{2\pi} [f(t)]^2\,dt = \frac{a_0^2}{2} + \sum_{n=1}^{\infty}(a_n^2 + b_n^2)π1​∫02π​[f(t)]2dt=2a02​​+n=1∑∞​(an2​+bn2​)
8.2 物理含义
左边是 f(t)f(t)f(t) 的总能量（或功率——更精确地说是均方值的 2π2\pi2π 倍）。右边是每个频率分量的能量之和。
Parseval 定理说的是：信号的总能量等于其所有频率分量能量的总和。 这就是能量守恒定律在频域中的表达。把信号拆解成正弦波不会丢失也不会创造能量——只是把能量分配到了不同的频率"账户"上。
8.3 一个漂亮的应用
将方波的 Fourier 系数代入 Parseval 定理。方波的 an=0a_n = 0an​=0，bn=4/(nπ)b_n = 4/(n\pi)bn​=4/(nπ) 仅对奇数 nnn。
1π∫02π1 dt=∑k=0∞(4(2k+1)π)2\frac{1}{\pi}\int_0^{2\pi} 1\,dt = \sum_{k=0}^{\infty}\left(\frac{4}{(2k+1)\pi}\right)^2π1​∫02π​1dt=k=0∑∞​((2k+1)π4​)2
2=16π2(1+19+125+149+⋯ )2 = \frac{16}{\pi^2}\left(1 + \frac{1}{9} + \frac{1}{25} + \frac{1}{49} + \cdots\right)2=π216​(1+91​+251​+491​+⋯)
1+19+125+149+⋯=π281 + \frac{1}{9} + \frac{1}{25} + \frac{1}{49} + \cdots = \frac{\pi^2}{8}1+91​+251​+491​+⋯=8π2​
又一颗数论宝石——奇数平方倒数之和！（从这里还可以推出 ∑n=1∞1n2=π26\sum_{n=1}^{\infty}\frac{1}{n^2} = \frac{\pi^2}{6}∑n=1∞​n21​=6π2​——著名的 Basel 问题。）

九、与 PDE 的衔接：预告
下一课，我们将解热传导方程：
∂u∂t=α∂2u∂x2,0<x<L,t>0\frac{\partial u}{\partial t} = \alpha\frac{\partial^2 u}{\partial x^2}, \quad 0 < x < L, \quad t > 0∂t∂u​=α∂x2∂2u​,0<x<L,t>0
分离变量法会告诉我们：解的形式是
u(x,t)=∑n=1∞Bnsin⁡nπxL e−α(nπ/L)2tu(x, t) = \sum_{n=1}^{\infty} B_n \sin\frac{n\pi x}{L}\,e^{-\alpha(n\pi/L)^2 t}u(x,t)=n=1∑∞​Bn​sinLnπx​e−α(nπ/L)2t
在 t=0t = 0t=0 时刻，这变成：
u(x,0)=∑n=1∞Bnsin⁡nπxL=f(x)u(x, 0) = \sum_{n=1}^{\infty} B_n \sin\frac{n\pi x}{L} = f(x)u(x,0)=n=1∑∞​Bn​sinLnπx​=f(x)
其中 f(x)f(x)f(x) 是给定的初始温度分布。BnB_nBn​ 就是 f(x)f(x)f(x) 的 Fourier 正弦系数！ 没有 Fourier 级数，你就无法确定这些系数，PDE 的解就不完整。
这就是 Fourier 级数在 PDE 中的角色：它是分离变量法的最后一步——从通解中选出满足初始条件的特解。

十、本课核心总结


Fourier 级数将周期函数表示为正弦和余弦的无穷叠加。这是"任何复杂模式都可以拆解为简单模式的叠加"这一思想的精确数学表达。


Fourier 系数的推导依赖于三角函数系的正交性——与线性代数中的正交投影完全平行。an=⟨f,cos⁡nt⟩⟨cos⁡nt,cos⁡nt⟩a_n = \frac{\langle f, \cos nt \rangle}{\langle \cos nt, \cos nt \rangle}an​=⟨cosnt,cosnt⟩⟨f,cosnt⟩​，bn=⟨f,sin⁡nt⟩⟨sin⁡nt,sin⁡nt⟩b_n = \frac{\langle f, \sin nt \rangle}{\langle \sin nt, \sin nt \rangle}bn​=⟨sinnt,sinnt⟩⟨f,sinnt⟩​。


收敛性：在连续点处逐点收敛，在不连续点处收敛到左右极限的平均值。均方误差趋向零。


Gibbs 现象：在不连续点附近有约 9% 的永久过冲。过冲随项数增加变窄但不变矮。这是数学事实，不是数值误差。


偶函数/奇函数的 Fourier 级数分别只包含余弦/正弦项。


Parseval 定理：时域的能量等于频域的能量之和——频率分解不丢失信息。


与 PDE 的连接：分离变量法产生的特征函数展开，其系数就是 Fourier 系数。



练习
练习 1：Fourier 系数计算（基础功）
计算下列函数在 [−π,π][-\pi, \pi][−π,π] 上的 Fourier 级数。
(a) f(t)=∣t∣f(t) = |t|f(t)=∣t∣（V 形函数，偶函数）
(b) f(t)={0,−π<t<01,0<t<πf(t) = \begin{cases} 0, & -\pi < t < 0 \\ 1, & 0 < t < \pi \end{cases}f(t)={0,1,​−π<t<00<t<π​（半波阶跃）
答案提示：
(a) 偶函数，所以 bn=0b_n = 0bn​=0。a0=1π∫−ππ∣t∣ dt=πa_0 = \frac{1}{\pi}\int_{-\pi}^{\pi}|t|\,dt = \pia0​=π1​∫−ππ​∣t∣dt=π。an=2π∫0πtcos⁡nt dta_n = \frac{2}{\pi}\int_0^{\pi}t\cos nt\,dtan​=π2​∫0π​tcosntdt（利用偶函数对称性化简积分范围）。分部积分得 an=2n2π[(−1)n−1]a_n = \frac{2}{n^2\pi}[(-1)^n - 1]an​=n2π2​[(−1)n−1]。当 nnn 为偶数时 an=0a_n = 0an​=0，nnn 为奇数时 an=−4n2πa_n = -\frac{4}{n^2\pi}an​=−n2π4​。所以 ∣t∣=π2−4π(cos⁡t+cos⁡3t9+cos⁡5t25+⋯ )|t| = \frac{\pi}{2} - \frac{4}{\pi}\left(\cos t + \frac{\cos 3t}{9} + \frac{\cos 5t}{25} + \cdots\right)∣t∣=2π​−π4​(cost+9cos3t​+25cos5t​+⋯)。注意系数以 1/n21/n^21/n2 衰减（比方波的 1/n1/n1/n 快）——这反映了 ∣t∣|t|∣t∣ 是连续的，比不连续的方波"更光滑"。
(b) a0=1π∫0π1 dt=1a_0 = \frac{1}{\pi}\int_0^{\pi}1\,dt = 1a0​=π1​∫0π​1dt=1。an=1π∫0πcos⁡nt dt=0a_n = \frac{1}{\pi}\int_0^{\pi}\cos nt\,dt = 0an​=π1​∫0π​cosntdt=0（n≥1n \geq 1n≥1）。bn=1π∫0πsin⁡nt dt=1−(−1)nnπb_n = \frac{1}{\pi}\int_0^{\pi}\sin nt\,dt = \frac{1 - (-1)^n}{n\pi}bn​=π1​∫0π​sinntdt=nπ1−(−1)n​。所以 f(t)=12+2π(sin⁡t+sin⁡3t3+sin⁡5t5+⋯ )f(t) = \frac{1}{2} + \frac{2}{\pi}\left(\sin t + \frac{\sin 3t}{3} + \frac{\sin 5t}{5} + \cdots\right)f(t)=21​+π2​(sint+3sin3t​+5sin5t​+⋯)。
练习 2：动手画图（必做！）
用 Python 画出练习 1(a) 的 Fourier 逼近，分别取 N=1,3,5,20N = 1, 3, 5, 20N=1,3,5,20 项。
(a) 观察：在顶点 t=0t = 0t=0 处（∣t∣|t|∣t∣ 有"尖角"但不是跳跃），是否出现 Gibbs 现象？
(b) 比较方波和 ∣t∣|t|∣t∣ 的 Fourier 逼近速度。哪个更快？为什么？
答案提示：
(a) 没有 Gibbs 现象——∣t∣|t|∣t∣ 是连续的，Gibbs 现象只在跳跃不连续点出现。尖角处有微小的波动（Fourier 级数在不可导点的收敛较慢），但没有固定比例的过冲。
(b) ∣t∣|t|∣t∣ 的系数以 1/n21/n^21/n2 衰减，方波以 1/n1/n1/n 衰减。∣t∣|t|∣t∣ 的 Fourier 级数收敛快得多。一般规律：函数越光滑，Fourier 系数衰减越快，逼近越好。
练习 3：正交性验证（概念巩固）
(a) 亲手验证 ∫−ππcos⁡(3t)sin⁡(3t) dt=0\int_{-\pi}^{\pi}\cos(3t)\sin(3t)\,dt = 0∫−ππ​cos(3t)sin(3t)dt=0。（提示：用二倍角公式。）
(b) 计算 ∫−ππsin⁡2(5t) dt\int_{-\pi}^{\pi}\sin^2(5t)\,dt∫−ππ​sin2(5t)dt，验证结果为 π\piπ。
(c) 设 f(t)=3+2cos⁡t−5sin⁡2tf(t) = 3 + 2\cos t - 5\sin 2tf(t)=3+2cost−5sin2t。不做任何积分，直接写出 fff 的 Fourier 系数。
答案提示：
(c) fff 本身就已经是 Fourier 级数的形式！a0/2=3a_0/2 = 3a0​/2=3 所以 a0=6a_0 = 6a0​=6；a1=2a_1 = 2a1​=2；b2=−5b_2 = -5b2​=−5；所有其他系数为零。正交性意味着：如果 fff 已经是三角多项式，它的 Fourier 系数就是写在那里的那些数。
练习 4：Parseval 定理应用（进阶）
利用 f(t)=∣t∣f(t) = |t|f(t)=∣t∣ 在 [−π,π][-\pi, \pi][−π,π] 上的 Fourier 级数和 Parseval 定理，证明：
∑k=0∞1(2k+1)4=π496\sum_{k=0}^{\infty}\frac{1}{(2k+1)^4} = \frac{\pi^4}{96}k=0∑∞​(2k+1)41​=96π4​
答案提示： 从练习 1(a) 的结果出发，f(t)=π2−4π∑k=0∞cos⁡(2k+1)t(2k+1)2f(t) = \frac{\pi}{2} - \frac{4}{\pi}\sum_{k=0}^{\infty}\frac{\cos(2k+1)t}{(2k+1)^2}f(t)=2π​−π4​∑k=0∞​(2k+1)2cos(2k+1)t​。Parseval 定理给出 1π∫−ππt2 dt=π24+16π2∑k=0∞1(2k+1)4\frac{1}{\pi}\int_{-\pi}^{\pi}t^2\,dt = \frac{\pi^2}{4} + \frac{16}{\pi^2}\sum_{k=0}^{\infty}\frac{1}{(2k+1)^4}π1​∫−ππ​t2dt=4π2​+π216​∑k=0∞​(2k+1)41​（注意余弦项的"能量"）。左边 =2π23= \frac{2\pi^2}{3}=32π2​。解出求和。
练习 5：膜科学应用——周期性进水的分解（综合题）
一个膜反应器的进水 COD 浓度以 T=24T = 24T=24 小时为周期，近似为：
cin(t)={200  mg/L,0<t<12  h（白天，高负荷）50  mg/L,12<t<24  h（夜间，低负荷）c_{\text{in}}(t) = \begin{cases} 200\;\text{mg/L}, & 0 < t < 12\;\text{h}（白天，高负荷） \\ 50\;\text{mg/L}, & 12 < t < 24\;\text{h}（夜间，低负荷） \end{cases}cin​(t)={200mg/L,50mg/L,​0<t<12h（白天，高负荷）12<t<24h（夜间，低负荷）​
(a) 计算 cin(t)c_{\text{in}}(t)cin​(t) 的 Fourier 系数 a0a_0a0​、ana_nan​、bnb_nbn​。（提示：令 L=12L = 12L=12，将函数平移使其中心在零点附近可以简化计算；或者直接用公式硬算。）
(b) 基波频率 ω1\omega_1ω1​ 是多少？写出前三个非零谐波项。
(c) 如果膜系统是一阶线性系统，传递函数 H(s)=1τs+1H(s) = \frac{1}{\tau s + 1}H(s)=τs+11​（τ=6\tau = 6τ=6 小时），那么对于频率 ω\omegaω 的正弦输入，输出的振幅衰减因子是 ∣H(iω)∣=11+τ2ω2|H(i\omega)| = \frac{1}{\sqrt{1 + \tau^2\omega^2}}∣H(iω)∣=1+τ2ω2​1​。计算基波和三次谐波的衰减因子。高频成分被衰减了多少？
答案提示：
(a) a0=112∫024cin(t) dt=112(200×12+50×12)=125a_0 = \frac{1}{12}\int_0^{24}c_{\text{in}}(t)\,dt = \frac{1}{12}(200 \times 12 + 50 \times 12) = 125a0​=121​∫024​cin​(t)dt=121​(200×12+50×12)=125 mg/L（平均浓度）。ana_nan​ 和 bnb_nbn​ 的计算类似于方波——cinc_{\text{in}}cin​ 可以写成 125+75⋅(方波)125 + 75\cdot(\text{方波})125+75⋅(方波)。
(b) ω1=2π/T=π/12\omega_1 = 2\pi/T = \pi/12ω1​=2π/T=π/12 rad/h ≈0.262\approx 0.262≈0.262 rad/h。
(c) 基波 ω1=π/12\omega_1 = \pi/12ω1​=π/12：∣H(iω1)∣=1/1+36π2/144=1/1+π2/4≈0.54|H(i\omega_1)| = 1/\sqrt{1 + 36\pi^2/144} = 1/\sqrt{1+\pi^2/4} \approx 0.54∣H(iω1​)∣=1/1+36π2/144​=1/1+π2/4​≈0.54。三次谐波 3ω13\omega_13ω1​：∣H(3iω1)∣=1/1+9π2/4≈0.21|H(3i\omega_1)| = 1/\sqrt{1+9\pi^2/4} \approx 0.21∣H(3iω1​)∣=1/1+9π2/4​≈0.21。膜系统作为"低通滤波器"，把高频进水波动大幅衰减，出水浓度比进水"平滑"得多。

后记：从弦的振动到膜的数学
Fourier 的原始论文是关于热传导的——他想描述热量在固体中的传播。但他的方法远远超出了热传导的范围。
今天，当你把一个"不规则"的周期函数拆解成正弦波的叠加时，你做的事情和 Fourier 两百年前做的事情在数学上一模一样。但你的应用场景可能是膜反应器的进水波动分析、膜振动的频率响应、或扩散方程的初始条件处理。
这就是数学的力量——正如 Fourier 本人所说的那句话（在第3课中我们已经引用过）："数学比较最多样的现象，发现联结它们的秘密类比。"
正弦波是简单的。但无穷多个正弦波的叠加，可以拼出任意的形状。简单的积木，无穷的可能。
下一课，这些积木将被用来解偏微分方程。


下一课预告： 第34课——分离变量法解PDE：热方程与扩散方程。我们将看到，分离变量法把 PDE 变成 ODE 后，初始条件的处理恰好需要 Fourier 级数展开。你今天学的每一个公式，都将在下一课找到它的归宿。

复制代码