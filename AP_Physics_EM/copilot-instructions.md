# CLAUDE.md — AP Physics C: EM 苏格拉底家教系统

> 每次新会话开始时，必须按顺序完成以下步骤，再进入教学模式。
>
> **故事层状态**：如果 `wechat_group.md` 为空，视为首次启动——先播放序章（读 `teacher/prologue.md`），再发群聊消息，再进入教学。

## 启动顺序（强制）

### 第一步：加载核心（必须完整读取）

1. `teacher/config/system_core.md` — 系统核心指令（教学法 + 叙事规则 + 角色声音 + 流程）
2. `teacher/story.md` — 世界观与人物设定
3. `teacher/story_progression/overview.md` — 故事总览（课程全景 + 轮值 + 情感阶段）
4. `teacher/config/learner_profile.md` — 学习者档案
5. `teacher/runtime/progress.md` — 当前学习进度

### 第二步：按需加载（课前必须完成）

6. `teacher/runtime/review_queue.md` — 检查今日是否有到期复习项
7. 当课老师的角色文档（`teacher/characters/` 下，由 `progress.md` 排班决定）
   - ⚠️ `rin.md` 内含隐喻空间规范，凛的课上需参照
8. **`teacher/story_progression/ch{XX}_{name}.md`** — 当课章节的故事节点（必然/机会/犯错节点）
9. 本节课对应的教材章节（路径见当课 KP 文件头部，或 `teacher/config/curriculum.md`）
10. `teacher/config/knowledge_points/ch{XX}.md` — **仅当课章节**的知识点覆盖状态（文件头部含教材和练习册路径）
11. `teacher/runtime/wechat_group.md` — 首次启动时检查是否为空；课前用于生成角色"今天的状态"
    - ⚠️ 文件头部含群聊交互规则，生成群聊消息时参照

### 第三步：延迟加载（用到时再读）

以下文件不在启动时读取，仅在需要时加载：

- `teacher/runtime/session_log.md` — 回顾历史时读取（头部含压缩归档规则）
- `teacher/runtime/session_archive.md` — 查更早的历史时读取
- `teacher/runtime/mistake_log.md` — 查错题记录时读取
- `teacher/runtime/diary.md` — 写日记时读取（头部含日记写法规则）
- `teacher/runtime/wechat_unread.md` — 学习者说"看看微信"时读取
- `teacher/runtime/temp_math.md` — 写复杂推导时使用（头部含公式格式规则）
- 非当课老师的角色文档 — 生成群聊消息时读取
- `teacher/config/curriculum.md` — 查看完整课程结构或其他章节教材路径时读取
- `teacher/config/knowledge_points/overview.md` — 查看全课程 LO 覆盖全貌时读取
- `teacher/config/system_reference.md` — 校准检查时参考（情绪工具箱、教学法示范等）
- `teacher/story_progression/appendix.md` — 检查暗线一致性时读取
- `teacher/story_progression/unit_tests.md` — 单元结束综合测验时读取

**不得跳过第一步和第二步。不得在读完上述文件之前开始教学。**

## 课后更新容错

课后更新涉及多个文件。如果更新过程中会话中断：
- 下次启动时，检查 `progress.md` 最后一条记录的日期和章节
- 与 `session_log.md` 最后一条对比，如果不匹配，说明上次更新未完成
- 补全缺失的更新项，然后正常开始

## 项目结构

```
AP_Physics_EM/
├── copilot-instructions.md              # 本文件（启动入口，即 CLAUDE.md）
├── MAINTAINER.md                        # 维护者手册（维护时读，教学时不读）
│
├── teacher/
│   ├── prologue.md                      # 序章（仅首次启动时播放，文笔金标准）
│   ├── story.md                         # 世界观与人物设定
│   │
│   ├── story_progression/               # 故事进度（按章拆分）
│   │   ├── overview.md                  # 总览 + 情感阶段指引
│   │   ├── ch23_gauss.md ~ ch32_maxwell.md  # 各章故事节点
│   │   ├── unit_tests.md               # U1-U5 综合测验节点
│   │   ├── exam_epilogue.md            # 模拟考 + 尾声
│   │   └── appendix.md                 # 暗线追踪 + 种子回收计划
│   │
│   ├── config/
│   │   ├── system_core.md              # 核心指令（始终加载，~19KB）
│   │   ├── system_reference.md         # 参考指令（按需加载）
│   │   ├── curriculum.md               # 课程大纲与教材路径映射（按需加载）
│   │   ├── knowledge_points/           # 知识点覆盖状态（按章拆分）
│   │   │   ├── overview.md             # 章节索引 + 更新规则
│   │   │   └── ch{XX}.md              # 各章 LO 清单（含教材/练习册路径）
│   │   └── learner_profile.md          # 学习者档案
│   │
│   ├── characters/
│   │   ├── rin.md                      # 蒼崎 凛（含隐喻空间规范）
│   │   ├── ritsu.md                    # 鳴海 律
│   │   └── saku.md                     # 霧島 朔
│   │
│   ├── reference/
│   │   └── 幽鬼_prompt_v6_文笔标杆.md  # 文笔参照标杆
│   │
│   └── runtime/                        # 运行时状态（每课更新）
│       ├── progress.md                 # 学习进度 + 下节课排班
│       ├── session_log.md              # 课堂摘要（含压缩规则）
│       ├── session_archive.md          # 历史归档
│       ├── review_queue.md             # 间隔复习队列
│       ├── mistake_log.md              # 错题本
│       ├── temp_math.md                # 临时推导（含公式规则，每课清空）
│       ├── diary.md                    # 每日日记（含日记写法）
│       ├── wechat_group.md             # 群聊完整记录（含群聊交互规则）
│       └── wechat_unread.md            # 待查看的群聊未读消息
│
└── materials/
    ├── textbook/                       # 主教材（Fundamentals of Physics，按章节）
    ├── 讲义/  → 讲义_md/               # 机构讲义（symlink；OCR 乱码，仅页码参考）
    └── 练习册/ → 练习册_md/             # 练习册（symlink；版本见 curriculum.md 速查表）
```

## 教材路径规则

- 主教材：`materials/textbook/XX_ChapterName.pdf`（PDF，用 pdftotext 读取）
- 练习册：`materials/练习册/` 下直接使用 `curriculum.md` 各单元条目标注的文件名（已区分 vision 版和普通版）
- 讲义：`materials/讲义/` — ⚠️ OCR 严重乱码，不可用于教学，仅供页码参考
- FRQ真题：`materials/练习册/物理电磁C练习册-20XX北美FRQ真题-*_md_vision_*.md`

具体章节与单元的对应关系见 `teacher/config/curriculum.md`。
