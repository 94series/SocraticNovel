# MAINTAINER.md — 膜材料数学 苏格拉底家教系统 维护手册

> 本手册面向需要修改、扩展或维护本系统的人员。
> 如果你只是使用系统学习，不需要阅读本文件。

---

## 文件架构概览

```
SocraticNovel/
├── copilot-instructions.md          # 系统入口（AI 自动读取）
├── MAINTAINER.md                    # 本文件
├── META_PROMPT.md                   # 系统生成指南（参考用）
├── teacher/
│   ├── prologue.md                  # 序章（首次启动播放）
│   ├── story.md                     # 世界观（每次启动加载）
│   ├── story_progression/           # 故事进度（按章拆分）
│   │   ├── overview.md              # 总览（始终加载，~4KB）
│   │   ├── ch{XX}_{name}.md         # 各章节点（仅当课加载，~2-3KB）
│   │   ├── unit_tests.md            # 综合测验节点
│   │   ├── exam_epilogue.md         # 模拟考 + 尾声
│   │   └── appendix.md              # 暗线追踪 + 种子回收
│   ├── config/
│   │   ├── system_core.md           # 核心指令（始终加载，~19KB）
│   │   ├── system_reference.md      # 参考指令（按需加载，~9KB）
│   │   ├── curriculum.md            # 课程大纲（延迟加载）
│   │   ├── knowledge_points/        # 知识点覆盖状态
│   │   │   ├── overview.md          # 章节索引
│   │   │   └── ch{XX}.md            # 各章 LO 清单（含教材路径）
│   │   └── learner_profile.md       # 学习者档案
│   ├── characters/
│   │   ├── leibniz.md               # 莱布尼茨
│   │   ├── gibbs.md                 # 吉布斯
│   │   └── shenyao.md               # 沈遥
│   ├── reference/                   # 参考材料（可选）
│   └── runtime/
│       ├── progress.md              # 当前进度
│       ├── session_log.md           # 课堂日志（含压缩规则）
│       ├── session_archive.md       # 归档
│       ├── review_queue.md          # 复习队列
│       ├── mistake_log.md           # 错题记录
│       ├── temp_math.md             # 临时数学笔记
│       ├── diary.md                 # 日记
│       ├── wechat_group.md          # 群聊（首次启动检测依赖此文件为空）
│       └── wechat_unread.md         # 未读消息
│       ├── whiteboard.html          # 教学白板（Live Server 实时渲染）
└── materials/
    ├── textbook/目录/               # 主教材（lesson_00.md ~ lesson_44.md）
    └── 练习册/                      # 练习册（待补充）
```

---

## Context 预算管理

**硬约束**：启动必须加载的文件总量 ≤ 55KB。

| 层级 | 文件 | 预算 | 当前大小 |
|------|------|------|---------|
| 始终加载 | system_core.md | ≤22KB（硬上限） | ~19KB |
| 始终加载 | story.md | 8-12KB | ~8KB |
| 始终加载 | overview.md | 3-5KB | ~4KB |
| 始终加载 | learner_profile.md | 1-2KB | ~0.4KB |
| 始终加载 | progress.md | ~1KB | ~0.3KB |
| **启动小计** | | **≤40KB** | **~32KB** |
| 按需加载 | 角色文件 | 10-15KB | ~18KB |
| 按需加载 | 当课 story_progression | 2-4KB | ~2-3KB |
| 按需加载 | 当课 knowledge_points | 2-3KB | ~0.3KB |
| 按需加载 | review_queue + wechat_group | 2-5KB | ~0.4KB |

### 超限处理

如果 `system_core.md` 超过 22KB：
1. 找出不是每轮对话都用的规则
2. 移入 `system_reference.md`
3. 在 system_core.md 中保留索引条目

---

## 常见维护任务

### 1. 添加新课

需要修改以下文件（按顺序）：

1. **`materials/textbook/目录/lesson_{XX}.md`** — 添加教材内容
2. **`teacher/config/curriculum.md`** — 在课程结构表中添加条目
3. **`teacher/config/knowledge_points/ch{XX}.md`** — 创建新的 LO 清单文件
   - 文件头部嵌入教材路径：`materials/textbook/目录/lesson_{XX}.md`
   - 列出本课学习目标，全部标记为 `[ ]`
4. **`teacher/config/knowledge_points/overview.md`** — 更新章节索引
5. **`teacher/story_progression/ch{XX}_{name}.md`** — 创建故事节点文件
   - 包含：角色状态 + 必然节点 + 机会节点 + 群聊脉搏
6. **`teacher/story_progression/overview.md`** — 更新轮值表和情感阶段
7. **`teacher/config/curriculum.md`** 的轮值表 — 如涉及教师变更

**注意**：
- 新课编号不能与现有课号冲突
- 如果插入中间位置，后续所有 ch{XX}.md 文件需要重新编号
- `copilot-instructions.md` 中的轮值表和情感阶段也需要更新

### 2. 修改课程顺序

如果需要调整课的顺序：

1. 重命名 `knowledge_points/ch{XX}.md` 文件
2. 重命名 `story_progression/ch{XX}_{name}.md` 文件
3. 更新 `curriculum.md` 中的课程列表
4. 更新 `overview.md` 中的轮值表
5. 更新 `copilot-instructions.md` 中的轮值表和情感阶段
6. 更新 `appendix.md` 中的暗线种子触发课号

### 3. 修改角色

修改 `teacher/characters/{角色名}.md` 时需注意：

- **"ta 说话的样子"** 段落是最关键的——运行时 AI 每次加载角色文件都会读它
- **"过往信息碎片"** 的释放顺序必须与 `overview.md` 的情感阶段和 `appendix.md` 的种子回收计划一致
- 修改角色声音后，同步更新 `system_core.md` 中的"角色声音速查"段落
- 修改角色的隐喻空间规范后，规则嵌入在角色文件头部（而非 system_reference.md）

### 4. 更新教学轮值

修改轮值需要同步以下文件：

| 文件 | 修改内容 |
|------|---------|
| `teacher/story_progression/overview.md` | 轮值表 |
| `teacher/config/curriculum.md` | 轮值表 |
| `copilot-instructions.md` | "当课老师判断"段落中的轮值表 |

### 5. 更新情感阶段

修改情感阶段需要同步：

| 文件 | 修改内容 |
|------|---------|
| `teacher/story_progression/overview.md` | 情感阶段指引（完整版） |
| `copilot-instructions.md` | "情感阶段快速参考"表 |
| `teacher/story_progression/appendix.md` | 群聊温度计 |

### 6. 清理运行时文件

系统使用一段时间后，运行时文件会积累内容。需要定期清理：

| 文件 | 清理策略 |
|------|---------|
| `session_log.md` | 超过 50 条记录后，最早的 10 条自动移入 `session_archive.md` |
| `review_queue.md` | 完成的复习项目可以删除（保留最近 20 条） |
| `mistake_log.md` | 已掌握的错题标记后保留（不要删除——学习者可能想回顾） |
| `wechat_group.md` | 不清理——它是首次启动检测的依据 |
| `wechat_unread.md` | 课后阅读后清空 |

**⚠️ 警告**：不要手动清空 `wechat_group.md`——这会触发首次启动流程，重新播放序章。

### 7. 修改系统规则

修改教学规则或叙事规则时：

1. **修改 `system_core.md`** — 如果规则每轮对话都用
2. **修改 `system_reference.md`** — 如果规则只在特定场景触发
3. 检查是否有规则需要"嵌入"到运行时文件头部（见规则嵌入策略）
4. 修改后检查文件大小是否超过预算

**规则嵌入策略速查**：

| 规则 | 嵌入位置 |
|------|---------|
| 隐喻空间规范 | 对应角色文件头部 |
| 群聊交互规则 | `wechat_group.md` 头部 |
| 日记写法 | `diary.md` 头部 |
| 数学公式处理 | `temp_math.md` 头部 |
| 文件压缩规则 | `session_log.md` 头部 |
| 情感阶段指引 | `story_progression/overview.md` |
### 8. 白板相关

**重置白板**：清空 `whiteboard.html` 中 `白板内容开始` 和 `白板内容结束` 注释之间的所有内容，
只保留 `<!-- __BOARD_CURSOR__ -->` 标记。不要删除文件本身。

**白板不渲染**：
1. 确认 VS Code 已安装 Live Server 或 Live Preview 扩展
2. 确认已通过 Live Server 打开（不是直接双击文件）
3. 检查网络——KaTeX 和 Mermaid 从 CDN 加载
4. 如果无网络，参见白板 Skill 的"离线模式"段落

**修改白板样式**：直接编辑 `whiteboard.html` 中的 `<style>` 部分。
不要修改 `<!-- ========== 白板内容开始 ========== -->` 等标记注释。

---

## 关键文件之间的依赖关系

```
copilot-instructions.md (入口)
  ├── system_core.md (核心规则)
  │   ├── 苏格拉底教学法
  │   ├── 叙事规则 + 角色声音速查
  │   ├── 上课流程 (P0-P5)
  │   ├── 课后更新流程
  │   └── 防漂移机制
  ├── story.md (世界观)
  ├── overview.md (轮值 + 情感阶段)
  ├── learner_profile.md (学习者档案)
  ├── progress.md (当前进度)
  │
  ├── [按需] 角色文件 ← 头部嵌入隐喻空间规则
  ├── [按需] ch{XX}.md (story_progression) ← 当课故事节点
  ├── [按需] ch{XX}.md (knowledge_points) ← 当课知识点 + 教材路径
  ├── [按需] wechat_group.md ← 头部嵌入群聊规则
  ├── [按需] review_queue.md ← 复习队列
  │
  └── [延迟] 其他运行时文件
```

**修改上游文件时，必须检查下游依赖是否需要同步修改。**

---

## 首次启动检测机制

`wechat_group.md` 为空（0字节）= 首次启动 → 加载 `prologue.md` → 播放序章。

**触发条件**：文件存在但无内容
**恢复条件**：第一次课后更新时写入群聊消息

如果你需要重置系统（重新播放序章）：
1. 清空 `wechat_group.md` 的内容（但保留文件）
2. 清空 `progress.md`
3. 清空所有运行时文件
4. **不要删除 `wechat_group.md` 文件本身**——copilot-instructions.md 的启动检测依赖它的存在

---

## 教材路径规则

- 主教材路径嵌入在 `teacher/config/knowledge_points/ch{XX}.md` 文件头部
- 练习册路径同样嵌入在对应 knowledge_points 文件头部
- `curriculum.md` 中有总路径 `materials/textbook/目录/`，但具体课的路径在 knowledge_points 文件中
- 运行时 AI 不需要每次都读 `curriculum.md`，除非需要查看完整课程结构

---

## 故障排除

| 问题 | 可能原因 | 解决方法 |
|------|---------|---------|
| 首次启动没有播放序章 | `wechat_group.md` 已有内容 | 清空该文件内容 |
| 序章质量下降 | 文笔标准未在 system_core.md 中明确 | 检查"散文标准"段落 |
| 角色说话方式偏离 | 角色文件的"说话的样子"段落不够具体 | 重写该段落，增加 do/don't 示例 |
| 教学规则漂移 | 对话太长，AI 丢失早期规则 | 用户触发 `/校准` |
| 群聊温度不符 | overview.md 中的情感阶段指引不够明确 | 细化群聊温度描述 |
| 课后更新不完整 | 会话中断 | 下次启动时 AI 自动补全（容错机制） |
| system_core.md 超限 | 规则过多 | 移出低频规则到 system_reference.md |
| 新课的故事节点缺失 | 添加新课时遗漏 Step 5 | 检查 story_progression/ 目录 |
| 白板公式不渲染 | CDN 未加载或浏览器缓存 | 刷新页面；检查网络；考虑本地化 KaTeX |
| 白板内容丢失 | agent 用了 create_file 而非 str_replace | 从 git 恢复；提醒 agent 只用 str_replace |
| __BOARD_CURSOR__ 标记丢失 | 编辑时误删 | 在白板内容结束注释前手动加回标记 |
