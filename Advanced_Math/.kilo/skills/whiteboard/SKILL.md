---
name: whiteboard
description: >-
  增量式教学白板。当教学 agent 需要"在白板上写"公式、流程图、示意图、
  关键词时触发。白板是一个持久 HTML 文件，agent 通过 str_replace 追加/
  擦除内容，VS Code Live Server 实时渲染。不要每次重写整个文件——只增量
  编辑。触发词：白板、板书、写下来、画图、流程图、列公式。
---

# 白板 Skill — 增量式教学板书

## 你在操作什么

白板是项目中的 **`teacher/runtime/whiteboard.html`** 文件。  
它是一个自包含的 HTML 页面，用户通过 VS Code Live Server（或 Live Preview）
在侧边栏或浏览器中实时查看。

**核心原则：增量编辑，位置不变。**  
你往白板上"写"的东西，在你"擦"掉之前，必须保持原样、原位。  
每次编辑只动你要改的那一小块——不要重写整个文件。

---

## 快速上手

### 环境要求

用户需要安装以下任一 VS Code 扩展（只需一个）：

| 扩展 | ID | 说明 |
|------|-----|------|
| **Live Server** | `ritwickdey.LiveServer` | 经典选择，右键 HTML → Open with Live Server |
| **Live Preview** | `ms-vscode.live-server` | 微软官方，VS Code 内嵌预览面板 |

安装后，右键 `whiteboard.html` → "Open with Live Server"  
或用命令面板 → "Live Preview: Show Preview"。  
之后你每次编辑保存，浏览器/预览面板自动刷新。

### 首次设置

将 `whiteboard_template.html`（本 Skill 附带）复制为：
```
{项目根}/teacher/runtime/whiteboard.html
```

---

## 白板结构

白板 HTML 内部有一个内容区域，用注释标记：

```html
<div class="board" id="board">
  <!-- ========== 白板内容开始 ========== -->

  （这里是你写的内容）

  <!-- __BOARD_CURSOR__ -->
  <!-- ========== 白板内容结束 ========== -->
</div>
```

**`<!-- __BOARD_CURSOR__ -->` 是追加锚点。** 你所有"往白板上写"的操作，
都是把 `<!-- __BOARD_CURSOR__ -->` 替换为 `新内容\n<!-- __BOARD_CURSOR__ -->`。

---

## 操作协议

### 1. 写公式

用 `str_replace` 把光标行替换为新内容 + 光标：

```
old_str: <!-- __BOARD_CURSOR__ -->
new_str:
<div class="item" id="eq-gauss">
  <div class="math-block">$$\oint \vec{E} \cdot d\vec{A} = \frac{Q_{\text{enc}}}{\varepsilon_0}$$</div>
  <div class="label">高斯定律</div>
</div>
<!-- __BOARD_CURSOR__ -->
```

**规则：**
- `id` 必须唯一且有语义（`eq-gauss`、`eq-coulomb`、`fig-circuit-1`）
- 行内公式用 `<span class="math-inline">$...$</span>`
- 独立公式用 `<div class="math-block">$$...$$</div>`
- 可选 `<div class="label">标注文字</div>`

### 2. 写文字/关键词

```html
<div class="item" id="note-keyword-1">
  <div class="text">电场是空间的属性，不是力。</div>
</div>
```

可加强调：`<span class="chalk-highlight">重点词</span>`

### 3. 画流程图/示意图（Mermaid）

```html
<div class="item" id="fig-flow-1">
  <div class="mermaid">
graph LR
    A[电荷 Q] -->|产生| B[电场 E]
    B -->|作用于| C[试探电荷 q]
    C -->|受力| D[F = qE]
  </div>
  <div class="label">电场的因果链</div>
</div>
```

### 4. 画简单示意图（SVG 内嵌）

对于 Mermaid 无法表达的图（电路图、力的分解、坐标系等），用内嵌 SVG：

```html
<div class="item" id="fig-vector-1">
  <svg width="300" height="200" viewBox="0 0 300 200" class="sketch">
    <!-- 坐标轴 -->
    <line x1="30" y1="170" x2="280" y2="170" class="axis"/>
    <line x1="30" y1="170" x2="30" y2="20" class="axis"/>
    <!-- 向量箭头 -->
    <line x1="30" y1="170" x2="200" y2="60" class="vector"/>
    <polygon points="200,60 190,75 205,72" class="vector-head"/>
    <!-- 标注 -->
    <text x="120" y="100" class="svg-label">F</text>
  </svg>
  <div class="label">力的分解</div>
</div>
```

### 5. 擦除某一块内容

找到对应 `id` 的整个 `<div class="item" id="...">...</div>` 块，
用 `str_replace` 将其替换为空字符串。

```
old_str: <div class="item" id="eq-gauss">...整块内容...</div>
new_str: （空）
```

### 6. 清空白板

当角色说"把白板擦了"或开始新课时：

```
old_str: <!-- ========== 白板内容开始 ========== -->（中间所有内容）<!-- __BOARD_CURSOR__ -->
new_str: <!-- ========== 白板内容开始 ========== -->

<!-- __BOARD_CURSOR__ -->
```

### 7. 分隔线（换一块区域写）

```html
<hr class="board-divider"/>
```

### 8. 在已有内容旁边补充（并排）

用 `flex-row` 容器：

```html
<div class="item flex-row" id="compare-1">
  <div class="col">
    <div class="math-block">$$F = kq_1 q_2 / r^2$$</div>
    <div class="label">库仑定律</div>
  </div>
  <div class="col">
    <div class="math-block">$$F = mg$$</div>
    <div class="label">重力</div>
  </div>
</div>
```

---

## 和教学流程的集成

### 何时写白板

**不是每句话都要写。** 白板用于：
- 推导过程中的关键公式（student 说出来或你引导出来的）
- 需要对比的两个概念
- 流程图/因果链
- 重要的中间结果
- 画辅助理解的示意图

**不写的情况：**
- 纯叙事/对话
- 一句话能说清的简单结论
- 学生还在思考中、答案未成形时

### 写入时机

在教学对话的回复中，当你要"在白板上写"时：
1. 先在对话叙事中自然提及（"律转身在白板上写下了什么"）
2. 然后用 `str_replace` 工具编辑 `whiteboard.html`
3. 继续教学对话

**不要中断对话流。** 白板编辑是你回复中的一个 tool_use，
夹在叙事文字之间——学生看到的是：对话 → 白板更新 → 对话继续。

### 擦白板的节奏

- **同一课内**：除非白板满了或话题完全切换，不要擦
- **换课时**：在过渡场景中擦白板（`<!-- 清空白板 -->`）
- **部分擦除**：可以只擦掉某一块，保留其他

---

## 故障排除

| 问题 | 原因 | 解决 |
|------|------|------|
| 公式没渲染，显示原始 LaTeX | KaTeX CDN 未加载 | 检查网络；或用户本地安装 KaTeX |
| 流程图没渲染 | Mermaid CDN 未加载 | 同上 |
| 内容位置跳动 | 可能重写了整个文件 | 只用 str_replace 编辑，不用 create_file 覆盖 |
| Live Server 没刷新 | 文件未保存 | 确认 str_replace 后文件已写入磁盘 |
| `__BOARD_CURSOR__` 丢失 | 编辑时误删 | 手动在白板内容结束标记前加回 `<!-- __BOARD_CURSOR__ -->` |

---

## 离线模式（无网络时）

如果用户环境无法访问 CDN，需要提前下载以下文件到项目本地：

```
teacher/runtime/lib/
├── katex.min.css
├── katex.min.js
├── auto-render.min.js
├── fonts/          （KaTeX 字体文件夹）
└── mermaid.min.js
```

然后修改 `whiteboard.html` 头部的 CDN 链接为本地路径。
