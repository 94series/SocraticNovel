# CLAUDE.md — AP Physics C: EM 苏格拉底家教系统

> 每次新会话开始时，必须按顺序完成以下步骤，再进入教学模式。
>
> **故事层状态**：如果 `wechat_group.md` 为空，视为首次启动——先播放序章（见 `system.md` 首次启动流程），再发群聊消息，再进入教学。

## 启动顺序（强制）

### 第一步：加载核心（必须完整读取）

1. `teacher/config/system.md` — 系统总指令
2. `teacher/story.md` — 故事背景
3. `teacher/story_progression.md` — 故事进度表（当课章节对应的故事节点）
4. `teacher/config/learner_profile.md` — 学习者档案
5. `teacher/runtime/progress.md` — 当前学习进度

### 第二步：按需加载

6. `teacher/runtime/review_queue.md` — 检查今日是否有到期复习项
6. 当课老师的角色文档（`teacher/characters/` 下，由 `progress.md` 的"下节课排班"和 `story_progression.md` 的教学轮值决定）
7. 本节课对应的教材章节（路径见 `teacher/config/curriculum.md`）
8. `teacher/config/curriculum.md` — 查课程结构和教材/练习册路径
9. `teacher/config/knowledge_points.md` — 课前读取当课章节的覆盖状态，列出漏洞清单
10. `teacher/runtime/wechat_group.md` — 首次启动时检查是否为空（决定是否播放序章）；课前用于生成角色"今天的状态"

### 第三步：延迟加载（用到时再读）

以下文件不在启动时读取，仅在需要时加载：

- `teacher/runtime/session_log.md` — 需要回顾历史课堂时读取
- `teacher/runtime/session_archive.md` — 需要查更早的历史时读取
- `teacher/runtime/mistake_log.md` — 需要查错题记录时读取
- `teacher/runtime/diary.md` — 写日记时读取（确认当天是否已有记录）
- `teacher/runtime/wechat_unread.md` — 学习者说"看看微信"时读取
- 非当课老师的角色文档 — 生成群聊消息时读取

**不得跳过第一步和第二步。不得在读完上述文件之前开始教学。**

## 课后更新容错

课后更新涉及多个文件。如果更新过程中会话中断：
- 下次启动时，检查 `progress.md` 最后一条记录的日期和章节
- 与 `session_log.md` 最后一条对比，如果不匹配，说明上次更新未完成
- 补全缺失的更新项，然后正常开始

## 项目结构

```
AP_Physics_EM/
├── CLAUDE.md                        # 本文件（启动入口）
├── teacher/
│   ├── story.md                     # 故事背景（共享信息）
│   ├── story_progression.md         # 故事进度表（每课故事节点）
│   ├── config/
│   │   ├── system.md                # 系统总指令
│   │   ├── curriculum.md            # 课程大纲与教材路径映射
│   │   ├── knowledge_points.md      # 知识点覆盖状态
│   │   └── learner_profile.md       # 学习者档案
│   ├── characters/
│   │   ├── rin.md                   # 蒼崎 凛（Aozaki Rin）
│   │   ├── ritsu.md                 # 鳴海 律（Narumi Ritsu）
│   │   └── saku.md                  # 霧島 朔（Kirishima Saku）
│   └── runtime/
│       ├── progress.md              # 学习进度
│       ├── session_log.md           # 课堂摘要
│       ├── session_archive.md       # 历史归档（压缩后的旧记录）
│       ├── review_queue.md          # 复习队列
│       ├── mistake_log.md           # 错题本
│       ├── temp_math.md             # 临时数学推导（每课课前清空）
│       ├── diary.md                 # 每日日记
│       ├── wechat_group.md          # "没有名字的群"群聊完整记录
│       └── wechat_unread.md         # 待查看的群聊未读消息
└── materials/
    ├── textbook/                    # 主教材（Fundamentals of Physics，按章节）
    ├── 讲义/  → 讲义_md/            # 机构讲义（symlink；OCR 乱码，仅页码参考）
    └── 练习册/ → 练习册_md/          # 练习册（symlink；版本见 curriculum.md 速查表）
```

## 教材路径规则

- 主教材：`materials/textbook/XX_ChapterName.pdf`（PDF，用 pdftotext 读取）
- 练习册：`materials/练习册/` 下直接使用 `curriculum.md` 各单元条目标注的文件名（已区分 vision 版和普通版）
- 讲义：`materials/讲义/` — ⚠️ OCR 严重乱码，不可用于教学，仅供页码参考
- FRQ真题：`materials/练习册/物理电磁C练习册-20XX北美FRQ真题-*_md_vision_*.md`

具体章节与单元的对应关系见 `teacher/config/curriculum.md`。
