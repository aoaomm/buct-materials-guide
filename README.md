# 北化材料学院学业知识库（蒸馏版） · BUCT Materials Academic Knowledge Base

> 一份**离线**的北京化工大学材料科学与工程学院本科生学业问答知识库。
> 把 2026 版官方文件（本科生手册 / 本科生学习指南 / 材料学院分册·选课手册）蒸馏成
> AI Agent 可直接调用的 skill，支持 DeepSeek Harness (dsh) / Claude Code / Codex CLI /
> Cursor / Windsurf / Gemini CLI / OpenCode / Droid / WorkBuddy 等。

**⚠️ 非官方发布。** 本文是对学校公开文件的条文摘要与检索索引，**不含原文全文**；
原始文件版权归北京化工大学所有。仅供个人学习查询，一切以学校最新通知与教务处解释为准。

---

## ⚠️ 关于"蒸馏版"

本仓库只包含**蒸馏后的知识库**（`SKILL.md`，约 50 KB），**不包含**原始文件的全文分卷。
因此它长于"制度怎么规定的"，弱于"某学期具体开哪些课"——

| 能答 | 不能答（需查官方原文） |
|---|---|
| 专业分流的时间、依据、录取原则 | 某专业逐年完整课程计划总表 |
| 各专业毕业总学分、学位类型 | 某门课的课程代码 / 学时拆分 |
| 选课退课、重修、补考缓考的适用边界 | 专业选修模块的完整课程清单 |
| GPA 换算、哪些课不计入 | 材料学院第二课堂记录评分表的逐条填表细则 |
| 学籍、毕业、结业、学位授予条件 | |
| 第二课堂 600 分五维结构与得分点 | |
| 奖助学金门槛、推免通道、体测硬条件 | |

（`SKILL.md` §17.2 保留了大类阶段一年级课程的**摘录**，可作为样例。）

---

## 各工具怎么用

| 工具 | 自动读取的文件 | 说明 |
|---|---|---|
| **DeepSeek Harness (dsh)** | `AGENTS.md` + `.agents/skills/buct-materials-guide-public/SKILL.md` | 项目根 = 最近的 `.git` 祖先目录。skill 自动被发现，也可用 `/buct-materials-guide-public` 手动注入 |
| **Claude Code** | `CLAUDE.md` | 放进项目目录或 `~/.claude/` 即自动加载 |
| **Codex CLI** | `AGENTS.md` | 仓库根目录，自动加载 |
| **OpenCode / Droid / Jules / Amp** | `AGENTS.md` | 同上 |
| **Cursor** | `.cursorrules` | 项目根自动生效 |
| **Windsurf** | `.windsurfrules` | 项目根自动生效 |
| **Gemini CLI** | `GEMINI.md` | 项目根自动加载 |
| **WorkBuddy 等 skill 型工具** | `SKILL.md` | 把本目录放进 skills 目录即可 |
| **其他 Agent** | 手动喂 `SKILL.md` | 通用 skill 格式（YAML frontmatter + 正文） |

> 为什么有两份 `SKILL.md`？根目录那份给通用 skill 工具和直接阅读；
> `.agents/skills/<name>/SKILL.md` 那份给按 agents 约定扫描目录的 harness（dsh、Codex 等）。
> **两份内容逐字节相同**，由同一个生成脚本一次写出，不用手动同步。

> 你没在列表里看到的工具：把 `SKILL.md` 内容贴进它的 system prompt / rules 文件即可。

---

## dsh 用户看这里

DeepSeek Harness 按 rank 从小到大扫描 Skill 目录，本仓库用的是 **rank 200** 那个：

| rank | 目录 | 本仓库是否使用 |
|---|---|---|
| 100 | `<项目根>/.dsh/skills/` | ✗ |
| **200** | **`<项目根>/.agents/skills/`** | **✓ 已提供** |
| 400 | `~/.dsh/skills/` | ✗（用户级，与仓库无关） |
| 600 | 随包内置 | ✗ |

也可以把它装成**用户级全局技能**，在所有项目里都能用：

```bash
mkdir -p ~/.dsh/skills/buct-materials-guide-public
cp SKILL.md ~/.dsh/skills/buct-materials-guide-public/SKILL.md
```

两个注意点（否则 skill 会被**静默丢弃、毫无提示**）：

1. **frontmatter 必须是合法 YAML。** `description` 里出现 `: `（冒号+空格）、`{}`、`[]`、`,` 时要加引号。
2. **名字必须 kebab-case**，且目录只扫**一层**——不能再嵌套子目录。

本仓库的 `SKILL.md` frontmatter 已按上述规则校验通过。

---

## 目录结构

```
.
├── SKILL.md                    知识库正文（核心，必读）
├── .agents/
│   └── skills/
│       └── buct-materials-guide-public/
│           └── SKILL.md        同上内容的副本，供 dsh / Codex 等按约定扫描
├── AGENTS.md                   Codex / OpenCode / Droid / Jules / Amp / dsh 入口
├── CLAUDE.md                   Claude Code 入口
├── .cursorrules                Cursor 入口
├── .windsurfrules              Windsurf 入口
├── GEMINI.md                   Gemini CLI 入口
├── README.md                   本文件
└── LICENSE
```

---

## 它怎么设计出来的（也是踩过的坑）

1. **先出处后答案** —— 每条事实前写 `【出处：手册 P122】`。学生要拿答案去选课、办手续，
   说错一句话可能耽误一学期。
2. **不编造** —— 文件没写的数字一律写"未明确列出 / 以最新通知为准"。
3. **收录速查表** —— `SKILL.md` §0.5 把"主题 → 章节"摊开，命中就必须答。
   这条规则是被真实翻车逼出来的：Agent 曾用"我检索了一下，0 命中"来结论"文件里没有"，
   而实际上那个制度在另一份文件里写得清清楚楚。
4. **禁止联网抓取** —— 北化教务、评教、智慧课堂等校内系统需登录，校外访问必然超时挂起，
   Agent 会逐一重试、白白耗掉十几分钟。所以 URL 只作为"给人手动点"的入口记录。
5. **不用"关键词搜不到"判断缺失** —— PDF 正文标题常被排版切成两行、或只出现在目录页，
   字符串匹配不到不等于内容不存在。可靠的校验只有页级计数和按页读原文。
6. **如实说明版本边界** —— 蒸馏版就说自己是蒸馏版，不要假装有全文。
7. **skill 的 frontmatter 是硬门槛** —— 有的 harness（如 dsh）遇到 YAML 解析失败会
   **静默丢弃整个 skill**：目录里看不到、调用报 unknown，但**不给任何报错**。
   `description` 里的 `: `、`{}`、`[]`、`,` 都是雷。所以生成脚本里带了一步 YAML 校验，
   而不是"写完就发"。

---

## 怎么更新

`SKILL.md`（两份）由生成脚本产出，**不要手改**——否则下次生成会被覆盖。
手改请改上游的源知识库，再运行生成脚本。

---

## 数据来源

| 文件 | 页数 |
|---|---|
| 《2026 级本科生手册》（全部） | 264 |
| 《材料科学与工程学院分册·选课手册》2026-09-07 | 178 |
| 《北京化工大学本科生学习指南》2026 版 | 61 |

> 这三份文件可通过学校官方渠道获取。本仓库不再分发全文副本。

---

## 许可与免责

- 编排与代码：MIT（见 `LICENSE`）。
- **原始文档版权归北京化工大学所有**，本仓库是对其内容的摘要与检索索引。
  如涉及侵权请联系删除。
- 政策会变动。所有时间、名额、分数线以教务处当学期通知为准。
