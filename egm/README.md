# EGM：以五项技能体现的大象-金鱼模型

此文件夹包含五项智能体技能，它们将一种设计方法转化为编程智能体可遵循的内容，另有一组页面将这些技能拆解开来，帮助你学习技能是如何构建的。安装这些技能并使用它们。然后阅读这些页面，并对这些选择提出质疑。

## 方法的来源

大象-金鱼模型（Elephant-Goldfish Model）由 Dave Rensin 提出。他在《大象、金鱼与软件工程的新黄金时代》（2026 年 4 月）中描述了这一模型，内容基于他的 Google 团队在 AI 辅助下构建软件所学到的东西。请先阅读这篇文章。它是这些技能所实现的方法规范，而且任何摘要都不如原文写得好。

简要版本：

- **设计即代码。** 当模型编写大部分代码时，人类可以支持的艺术品就是设计。因此，设计判断必须在任何代码存在之前就体现在文档中。
- **大象** 是一个长时间的、富含上下文的 AI 会话（以及它帮助你编写的设计文档）。它会记住你和它的每一次争论。
- **金鱼** 是一个全新的、没有记忆的会话。它只知道页面上写的内容。
- **喂饱大象；用金鱼测试。** 如果一个全新的会话无法仅凭你的文档理解、审查和实现，那么这份文档就依赖于一旦聊天结束就会消失的上下文。

这些技能是本课程对该方法的实现。如果某个技能添加了文章未提及的内容（如账本文件、脚本或关卡），该技能的演练页面会告诉你。

## 五项技能

| 技能 | 文章 | 功能 | 输出 |
|---|---|---|---|
| [`peanuts`](walkthroughs/peanuts.md) | 引导（"peanuts 和 hay"） | 为大型代码库构建自底向上的 README 文件树，每个级别都有一个人工审查关卡 | 每个目录一个 `README.md`，`PEANUTS.md` |
| [`elephant`](walkthroughs/elephant.md) | 阶段 1 和 2 | 运行无代码设计对话，然后编写四部分设计文档 | `docs/designs/<slug>.md`，`ELEPHANT.md` |
| [`goldfish`](walkthroughs/goldfish.md) | 阶段 3，步骤 5 到 7 | 并行将文档发送给三个全新审查者，并循环直到只剩琐碎的修改 | `GOLDFISH.md`，审查者报告 |
| [`egm-implement`](walkthroughs/egm-implement.md) | 阶段 4，步骤 8 | 只构建文档列出的文件，并在现实与文档不符时停止 | 代码，`IMPLEMENT.md` |
| [`mean-review`](walkthroughs/mean-review.md) | 阶段 4，步骤 9 | 尽可能严格地审查差异，进行四次脚本强制扫描 | `MEAN-REVIEW.md`，每次扫描一份待办清单 |

这些账本（`ELEPHANT.md`、`GOLDFISH.md`、`IMPLEMENT.md`、`MEAN-REVIEW.md`）共同保存在 `docs/egm/<slug>/` 中，每个功能一个文件夹。slug 是设计文档文件名去掉 `.md` 后的部分，`peanuts` 之后的四项技能都依赖于它。

## 它们如何配合

```
  大型遗留代码库？
        |
        v
   /peanuts ----> 每个目录中的 README.md（+ PEANUTS.md）
        |                  |
        |   （步骤 1 读取 README 而非源代码）
        v                  v
  一个想法 ----> /elephant ----> docs/designs/<slug>.md
                   |                    |
                   v                    v
             ELEPHANT.md           /goldfish  <------------+
                                        |                  |
                          三个全新审查者并行              |
                                        |                  |
                                  GOLDFISH.md      编辑文档，再次运行
                                        |                  |
                          只剩琐碎修改？---- 否 --------+
                                        |
                                      是
                                        |
                       人类签字确认（或记录跳过-单独）
                                        |
                                        v
                                 /egm-implement ----> 代码 + IMPLEMENT.md
                                        |
                          （偏差？停止，先修复文档）
                                        |
                                        v
                                  /mean-review ----> MEAN-REVIEW.md，修复，再次运行
```

每个箭头都是你自己启动的交接。没有任何技能会为你运行下一个。这是刻意的：它们之间的空白是人类做决定的地方。

## 此文件夹中的内容

```
egm/
├── README.md                  你在这里
├── anatomy-of-a-skill.md      一项技能是如何构建的，以这五项为例
├── walkthroughs/              每个技能一页
│   ├── peanuts.md
│   ├── elephant.md
│   ├── goldfish.md
│   ├── egm-implement.md
│   └── mean-review.md
└── skills/                    技能本身；复制这些进行安装
    ├── elephant/
    ├── goldfish/
    ├── egm-implement/
    ├── mean-review/
    └── peanuts/
```

Suggested reading order: [anatomy-of-a-skill.md](anatomy-of-a-skill.md)
first, then the walkthroughs in pipeline order (peanuts, elephant, goldfish,
egm-implement, mean-review). Keep the matching `SKILL.md` open beside each
walkthrough.

## 安装技能

每个技能是一个包含 `SKILL.md` 的文件夹。安装一个技能意味着将该文件夹放在你的 harness 寻找技能的位置。就这样。

你需要 `git` 和 `python3`（3.9 或更新版本，仅标准库）来运行三个辅助脚本。

### Claude Code

个人安装，可在你机器上的每个项目中使用：

```sh
git clone --depth 1 https://github.com/eecs498-aase/materials.git
mkdir -p ~/.claude/skills
cp -R materials/egm/skills/* ~/.claude/skills/
```

或者安装到一个项目中，这样每个克隆它的人都会获得它们，只需将其复制到该项目的 `.claude/skills/` 中并提交。

启动 `claude`，运行 `/skills`，五个技能应该会被列出。输入
`/elephant` 直接启动一个，或者直接描述你想要什么（"在我们写任何东西之前设计 webhook 重试"），让模型从其描述中选择技能。如果一个技能从不自行触发，描述是第一件要检查的事情。

### Codex、OpenClaw 和其他读取 SKILL.md 的 harness

`SKILL.md` 格式是一个开放规范（[agentskills.io](https://agentskills.io/specification)），许多 harness 都读取它。一个常见的共享位置是项目中的 `.agents/skills/` 或主目录中的 `~/.agents/skills/`。Codex 和 OpenClaw 都在那里查找：

```sh
mkdir -p ~/.agents/skills
cp -R materials/egm/skills/* ~/.agents/skills/
```

其他工具（Gemini CLI、OpenCode、Cursor、VS Code 中的 GitHub Copilot 等）各自记录自己的文件夹。列表位于 [agentskills.io/clients](https://agentskills.io/clients)。这些技能将其 frontmatter 保持为规范中的字段，因此它们可以在规范加载的任何地方加载。

有两个部分是 harness 特定的。`goldfish` 需要一种启动全新子智能体的方法（其 `references/runtimes.md` 涵盖了多个 harness，以及如何手动操作）。调用语法也因 harness 而异。Claude Code 和其他几个将每个技能暴露为斜杠命令，如 `/goldfish`；如果你的不支持，则命名该技能或让描述匹配。

### Aider

Aider 没有技能加载器，因此你就是加载器。通过手动将技能主体添加到聊天中，从你的仓库克隆中（上面的 `git clone` 行）：

```
/read-only path/to/materials/egm/skills/elephant/SKILL.md
```

将 `path/to/materials` 替换为你的克隆位置。

注意这需要什么。Aider 现在将其发送的每个请求的整个主体都发送出去，无论你是否需要它，并持续发送它，直到你 `/drop` 该文件。具有真正技能支持的 harness 只在模型要求更多之前发送单行描述。解剖页面解释了这种差异，这也是重点所在。

对于 Aider 中的 `goldfish`，每个审查者是一个全新的 `aider` 会话，从未见过你的设计聊天。使用 `/read-only` 给它文档，并从 `skills/goldfish/assets/goldfish-reviewer-prompts.md` 粘贴一个提示词。

## 无风险试用

创建一个包含一些真实代码文件的临时 git 仓库，并在一个小功能上运行 `/elephant`。如果你真的争论，第一次设计对话需要 20 到 30 分钟。然后在其编写的文档上运行 `/goldfish`，阅读三个陌生人对此的看法。

`elephant` 和 `goldfish` 只在 `docs/` 下写入。`peanuts` 在每个目录中写入 `README.md`，并在树的根目录写入 `PEANUTS.md`。`egm-implement` 更改代码，且只更改设计文档中列出的文件。

在实际仓库中，在分支上运行 `peanuts`。它在每个范围内目录中写入 `README.md`，并可能覆盖已有的文件，因此你需要 `git diff` 显示更改内容，并有简单的方式返回。

## 每个技能还附带触发测试

每个技能都有 `evals/trigger-evals.json`：十二个现实的请求，其中一半应该加载该技能，另一半不应该（故意的接近 misses）。它们是衡量描述是否有效的起点。解剖页面有一个使用它们的练习。
