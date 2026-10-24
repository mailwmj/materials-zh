---
theme: ../theme
title: "L10: 技能与大象-金鱼模型"
info: |
  EECS 498 AASE — 讲座 10
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 技能与大象-金鱼模型

## 讲座 10 · 2026 年 10 月 1 日

<!--
下午好。

上周的课表有变。今天原定内容是部署本地模型，改为技能与大象-金鱼模型，这就是你们昨晚收到阅读材料的原因。部署本地模型的内容没有取消；它会出现在未来几周的课程计划中。

今天没有现场演示，也不需要安装任何东西。屏幕上的所有内容都是你可以直接打开的文件，技能本身也已经放在 materials 仓库中了。

方向：今天没有现场演示。屏幕上的所有数字都来自讲师截至 10 月 1 日的 EGM 技能文件以及课程工作区中的 docs/egm，因此不依赖模型的行为表现。
-->

---
layout: default
---

<div class="label">今日内容</div>

# 两个部分

1. 技能
2. 大象-金鱼模型

<div class="caption mt-6">阅读材料: Rensin, "Elephants, Goldfish, and the New Golden Age of Software Engineering"</div>

<!--
今天分为两部分。技能，是一种将方法打包的方式，让智能体在需要时能够调用。以及大象-金鱼模型，这是一套设计方法，将以技能的形式交给你们。

昨晚已经发送了 Dave Rensin 的文章。

*[提问: 读过这篇文章的请举手。读到金鱼部分的请举手。]*

两种都可以。后半部分包含你需要的内容。如果你读了，请注意我实现的版本中，文章没有明确说明的地方我做了哪些选择。
-->

---
layout: default
---

<div class="label">实验课01, 讲座05, 讲座07</div>

# 组装、回答、应用

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-stack-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">组装</div>
    <div class="text-sm opacity-70 flex-1">模型看到的每一个字</div>
  </div>
  <div class="card">
    <ph-chat-text-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">回答</div>
    <div class="text-sm opacity-70 flex-1">输出文本，仅此而已</div>
  </div>
  <div class="card">
    <ph-pencil-simple-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">应用</div>
    <div class="text-sm opacity-70 flex-1">框架对输出执行操作</div>
  </div>
</div>

<div class="caption mt-8">今天的内容在第一张卡片中。</div>

<!--
你们已经三次绘制过这个模型了。实验课01、讲座05 和 讲座07 都是在字节层面。一个框架组装请求，模型以文本回答，框架再应用它。

今天的一切都发生在第一张卡片中。技能是关于什么内容进入组装请求、以及何时进入的决策。大象-金鱼模型本质上是一个关于会话能看到什么的决策。记住这三张卡片。中场休息时会要求你们将技能放入第一张卡片中。
-->

---
layout: section
---

# 技能

## 智能体需要时加载的方法

<!--
第一部分。技能。

大约十五分钟。什么是磁盘上的技能，必需文件里有什么，它与工具有何不同，以及给智能体太多技能会付出什么代价。记住这个代价。它是中场休息的问题。
-->

---
layout: default
---

<div class="label">定义</div>

# 什么是技能

智能体在任务需要时加载的文件夹。

- **指令** - SKILL.md，始终存在
- **资源** - 它可能读取的文件
- **脚本** - 它可能运行的代码

<div class="caption mt-6">为模型阅读而写，不是为程序调用而写。</div>

<!--
这是定义。技能是智能体在任务需要时加载的文件夹。仅此而已。

文件夹内部最多有三种东西。指令，存在于名为 SKILL.md 的文件中，每个技能都有这个文件。资源，是指令可能发送模型去读取的文件：模板、清单、更长的说明。以及脚本，是某些步骤应该每次都相同时智能体运行的代码。

 caption 是需要记住的部分。技能中的所有内容都是为模型阅读而写。没有 API，没有函数签名。技能的受众是模型，就像 README 的受众是下一位工程师一样。
-->

---
layout: default
---

<div class="label">格式</div>

# 磁盘上的技能

```text
elephant/
├── SKILL.md                       required
├── references/
│   └── design-doc-sections.md     read on demand
├── assets/
│   ├── design-doc-template.md     copied into output
│   └── elephant-ledger-template.md
└── evals/
    └── trigger-evals.json         tests the description
```

<div class="caption mt-6">讲师的大象技能。这里没有 scripts/。存在于 .claude/skills/ 或 .agents/skills/ 中。</div>

<!--
这是真实的大象技能，截至今天的版本。一个以技能命名的文件夹。里面有一个必需文件 SKILL.md。该文件顶部是几行 YAML 前端元数据，其余是用 Markdown 编写的指令，供模型阅读。

其他一切都是可选的，文件夹名称是约定俗成的。references 存放模型只在 SKILL.md 告诉它时才读取的材料；这里是一份关于设计文档每个部分应该包含什么以及每个部分可能出错的地方的页面。assets 存放技能在输出中使用而非读取指导的文件；这里是指空白设计文档和空白分类账。evals 存放十二个请求，六个应该加载该技能，六个接近但不应该加载，这就是测试描述的方式。scripts 文件夹会存放智能体运行而非阅读的代码。Elephant 没有，因为设计对话中的任何内容都不应该是确定性的，但其他三个 EGM 技能中有。

文件夹存放的位置决定了谁能使用它。Claude Code 在 .claude/skills 中查找，在你的主目录中查找个人技能，或在项目中查找共享技能。Codex 在 .agents/skills 中查找。

没有运行时，没有注册表，没有 SDK。如果你会写 README，你就会写技能。这就是它们广泛传播的主要原因之一。
-->

---
layout: default
---

<div class="label">SKILL.md 的顶部</div>

# 前端元数据

```yaml
name: elephant
description: Runs a no-code design conversation for a new
  feature, refactor, or system change, then writes the
  four-section design doc that becomes the source of truth.
  Use when the user says "design a feature", "let's plan X",
  "spec this out", or otherwise opens a non-trivial design
  problem, even if they did not ask for a document.
  Hand the finished doc to /goldfish. Not for one-line
  fixes or questions about how existing code works.
```

<div class="caption mt-4">讲师的大象技能，精简版。</div>

<!--
这是一个真实示例的顶部，你将在后半部分见到的大象技能。两个字段很重要。name，也是你手动用斜杠调用它的方式。以及 description。

像模型阅读描述一样阅读它。第一句话用第三人称说明技能做什么。第二句话说明何时使用它，并列出了用户可能输入的短语，加上 "even if they did not ask for a document"，这推动模型比 otherwise 更主动地去使用它。最后一句话说明何时不使用它。这也不是装饰。这是模型在决定是否加载其余部分之前看到的唯一部分。
-->

---
layout: default
---

<div class="label">SKILL.md 的其余部分</div>

# SKILL.md 的主体

```markdown
# Elephant: grow the design before you grow the code
## EGM operating reflexes (shared preamble)
## Hard rules: do not break these
## Workflow
### Step 2: The "No Code" rule (the interview)
### Step 3: The sycophant defense
### Step 5: Build the design doc, section by section
    Before drafting each one, read its entry in
    references/design-doc-sections.md
## Anti-patterns
## Why this skill exists
```

<div class="caption mt-4">只有标题，以及将模型导向 references/ 的那一行。</div>

<!--
前端元数据下方是主体，主体只是 Markdown 指令。这些是大象的标题，有些步骤省略了。

它读起来像为同事写的运行手册。适用于整个会话的思维定势。永远不要破坏的硬性规则，比如本次会话中不写代码。编号工作流，模型在过程中保持的清单。要避免的错误。以及技能存在的原因，因为当模型知道规则的目的时，它会更好地遵守规则。

看第 5 步下的缩进行。那是主体指向 references 中一个文件的指针。模型只在到达该步骤时才读取章节指南，而不是之前。技能可以像描述隐藏整个主体一样，在自身内部隐藏细节。在休息后记住这一点。
-->

---
layout: statement
---

**描述**决定*何时*加载。

主体决定*如何*工作。

<!--
所以描述是触发器。如果它模糊，技能永远不会触发，或者它在错误的请求上触发。如果它写得好，智能体会在用户说 "let's plan this" 时自主调用该方法，无需任何人输入斜杠命令。

编写技能的大部分技巧就在那一段中。第三人称，它做什么，何时使用，人们实际说的话。
-->

---
layout: default
---

<div class="label">方法可以存放的位置</div>

# 指令的四个家

|| 家 | 进入请求 |
|---|---|
|| 系统提示词, AGENTS.md | 每个请求 |
|| 工具定义 | 每个请求，以 schema 形式 |
|| 技能 | 当任务需要时 |
|| 你自己的头脑 | 当你重新输入时 |

<!--
大象的指令还能放在哪里？四个地方。

系统提示词，或项目文件如 AGENTS.md 或 CLAUDE.md，框架会将其粘贴进去。它进入每个请求，无论是否相关。

工具。讲座07 向你展示了工具定义也以 JSON schema 的形式随每个请求一起发送。但工具是框架执行的动作。"先设计再编码" 不是一个动作。它是一种工作方式。

技能。方法只随任务需要一起传递。

你们中大多数人在黑客马拉松 1 中使用的一个：你自己的头脑。你在每个会话中将方法重新输入聊天，每次略有不同，然后在晚上忘掉一半。

本课程的设计文档称技能为可移植上下文加触发器。这句话值得记住。
-->

---
layout: default
---

<div class="label">不是一回事</div>

# 技能与工具

|| | 工具 | 技能 |
|---|---|---|
|| 它是什么 | 函数 | 指令 |
|| 谁执行 | 框架运行它 | 模型遵循它 |
|| 在请求中 | 完整 schema，始终存在 | 描述，然后是主体 |
|| 返回什么 | 结果 | 一种工作方式 |

<div class="caption mt-6">工具做某事。技能改变模型决定做什么的方式。</div>

<!--
因为它们容易被混淆，这里将它们并列放在一起。

工具是一个函数。你们在讲座07 中见过它们：名称、描述、参数的 JSON schema。模型发出调用，框架运行真实代码：读取文件、运行命令。返回的是结果，一条工具消息，模型对其推理。

技能是指令。使用技能时没有任何东西执行。模型阅读它，改变的是模型在任务剩余部分的行为：它开始面试你而不是写代码。

它们的成本也不同。每个工具的完整 schema 随每个请求一起传递，因为模型必须知道参数才能调用它。技能以描述的形式传递，直到模型要求主体。

它们在两个地方相交。首先，技能的指令通常告诉模型使用哪些工具以及何时使用，所以技能通常是一种使用工具的方法。其次，这是休息的问题：模型必须通过某种方式获得主体。

*[提问: 如果没有任何东西执行，技能的主体是如何进入对话的？]*

方向：给出一个或两个猜测，不要解决它。这是中场休息的第二个问题。
-->

---
layout: default
---

<div class="label">五种 EGM 技能，实测</div>

# 知晓一切的成本

|| 智能体持有的内容 | 字符数 | 大约 |
|---|---:|---:|
|| 五个描述 | 3,659 | 900 个标记 |
|| 五个完整的 SKILL.md 文件 | 70,697 | 18,000 个标记 |

<div class="caption mt-6">按大约每标记四个字符计算。二十个技能是这个的四倍。</div>

<!--
这就是触发器重要的原因。这些是讲师截至今天的五个 EGM 技能。

五个描述总共大约三千六百个字符。算作 900 个标记。五个完整的 SKILL.md 文件大约七万一千个字符，约一万八千个标记。差十九倍，还没有计算每个 SKILL.md 旁边存放的模板和参考页面。

记住讲座09。L07 跟踪中的提示词在九步中从 471 个标记增长到 3252，这足以需要一个标记预算。一万八千个标记的方法，在用户输入任何内容之前，是一个不同的问题。而且没有人停在五个技能。一个工作设置会有二十个或四十个。

记住这个数字。它是中场休息的问题。
-->

---
layout: default
---

<div class="label">开放格式</div>

# 一份 SKILL.md，多种框架

- **Claude Code** - 格式的发源地
- **Codex CLI 及其他** - 读取相同的文件夹
- **Aider** - 还没有技能

<div class="caption mt-6">规范: agentskills.io。每个工具记录它查找文件夹的位置。</div>

<!--
Anthropic 为 Claude Code 发布了技能，然后将格式作为开放规范发布。其他几个框架现在读取相同的文件夹布局，包括 OpenAI 的 Codex CLI，它在 .agents/skills 中查找。每个框架查找的位置不同，所以请检查你使用的工具的文档。

本课程的最后一行是：Aider，你们五周来一直在使用的工具，目前没有技能的概念。它有一个你可以传入的约定文件，这就是前两张幻灯片中的始终在提示词中的选项。决定何时加载技能的工具不存在于其中。

方向：不要在这里展开 Aider 缺失的部分。Analyze 的改动即将到来，由讲师宣布。一句话带过即可。
-->

---
layout: section
---

# 大象-金鱼模型

## 先设计，后编码

<!--
现在讲方法本身。大象-金鱼模型。

休息前十五分钟。文章的三个部分，一句话论点，以及大象：漫长的设计会话及其产生的文档。金鱼，即使方法有效的那一半，在休息后讲解，因为它被证明与休息问题属于同一类决策。
-->

---
layout: default
---

<div class="label">阅读材料</div>

# Rensin 文章的三个部分

|| 部分 | 论点 |
|---|---|
|| 1 | 工具，而非玩具 |
|| 2 | 大象-金鱼模型 |
|| 3 | 如今我们都是管理者 |

<div class="caption mt-6">Dave Rensin, 杰出工程师, Google. drensin.medium.com</div>

<!--
Dave Rensin 是 Google 的杰出工程师，过去几年领导了 Google 内部好好使用 AI 的推动。这篇文章是他的所学。三个部分。

第一部分：工具，而非玩具。在将模型用作研究员之前，先将其用作审问者。在请求输出之前，与它商定验收标准。然后在全新的会话中运行真实查询，将两者作为输入。他的观点是，大多数人将他们的判断外包出去，并称之为生产力。

第二部分是我们今天花费时间讲解的模型。

第三部分：如今我们都是管理者。你将像管理者管理人一样管理智能体：设定结果，限定权限，说明何时升级，然后放心地走开。他建议参加首次管理者课程，而且他不是在开玩笑。

如果你读了这篇文章，请注意第一部分已经包含了完整的微型思想。审问、定义完成、然后全新会话。第二部分将这一点扩展到一个功能。
-->

---
layout: statement
---

*设计*就是新代码。

<!--
这是他的论点。设计文档才是重要的产物。代码是其下游产物，越来越自动生成，越来越不透明。如果文档正确，代码可以重新生成。如果文档错误，再多的好代码也救不了你。

你已经听过这个观点的某个版本。讲座04 是指令驱动开发。你的 Apply 构建一半分数在规范和设计上。EGM 是让设计足够好以承载这份重量的方法。
-->

---
layout: default
---

<div class="label">EGM 第一步</div>

# 大象

- **文档通过前不写代码**
- **先访谈，再提议**
- **模型先起草**，你来反驳
- **一份文档**，四个部分

<!--
大象是一个拥有大量上下文的长时间运行会话。设计在这里成长。讲师版本的四条规则。

不写代码。不是函数，不是伪代码，直到设计文档通过休息后你将看到金鱼测试。

先访谈。模型在提出任何建议之前先问你问题。这是 Rensin 的审问者想法。

模型先起草，你来反驳。技能称之为 "谄媚防御"。如果你先提出方案，模型倾向于同意你。如果它必须在听到你的方案之前承诺一个建议，你会得到一个真正的第二意见。

输出是一份包含四个部分的文档，下一页。
-->

---
layout: default
---

<div class="label">大象写什么</div>

# 四段式设计文档

|| 部分 | 内容 |
|---|---|
|| 问题 | 三到五句 plain language |
|| 技术方案 | 框图，尽量少用术语 |
|| 备选方案 | 被否决的想法，以及原因 |
|| 详细实现 | 每个文件，每次变更 |

<div class="caption mt-6">Four sections.</div>

<!--
四个部分。

问题，用三到五句 plain language 说明。如果你无法如此简要地陈述它，说明你还没有理解它。

技术方案，在框图层面，尽量少用术语。

备选方案。你否决的每一个想法及其原因。人们通常会跳过这一部分，但它最能救你，因为六周后某人，可能就是你，会再次提出那个被否决的想法。

详细实现，最长的部分。每个变更的文件以及文件中的变更。Rensin 明确说明文档列举每个文件。这就是让代码稍后可以针对它生成的原因。
-->

---
layout: default
---

<div class="label">中场休息</div>

# 组装问题

二十个技能，每个 SKILL.md 约 4,000 个标记。

1. 在第 1 轮，每个技能在请求中的哪个位置？
2. 在完整 SKILL.md 出现之前会发生什么？
3. 它落在哪一轮的组装中？

<div class="caption mt-6">纸上，五分钟。画出请求草图。</div>

<!--
好了，Zoom，休息时间。

问题。你的框架每轮组装一个请求。给它二十个技能，每个 SKILL.md 大约四千个标记。纸上：在第 1 轮，每个技能在请求中的哪个位置？在技能的完整文本出现之前必须发生什么？以及它落在哪一轮的组装中？

画出请求的框图：system、tools、messages。把技能放在某个地方。

方向：在房间里走动。寻找三个答案。所有内容都在系统提示词中：询问这在二十个技能时花费多少。在用户命名技能之前什么都没有：询问模型如何知道它存在。以及索引后加载的答案：询问他们说哪个消息承载主体。
-->

---
layout: section
---

# 回到组装

## 技能放在哪里

<!--
好了，Zoom，我们回来了。

让我们将技能放入第一张卡片。先给出答案要点，让每个人都知道我们要去哪里，然后是其背后的三个层级，然后是一个技能跨越两个真实请求的跟踪。
-->

---
layout: default
---

<div class="label">中场休息</div>

# 休息答案要点

|| 问题 | 答案 |
|---|---|
|| 第 1 轮：每个技能在哪里？ | 名称和描述，系统提示词 |
|| 主体出现之前是什么？ | 模型调用加载工具 |
|| 哪一轮的组装？ | 下一轮，作为工具结果 |

<div class="caption mt-6">所有二十个主体都在系统提示词中：大约 80,000 个标记，每个请求。</div>

<!--
答案要点，简短版本，然后我们拆解它。

一。在第 1 轮，每个技能只贡献其名称和描述，作为系统提示词中的列表。二十个技能，两三千个标记。

二。在主体出现之前，模型必须请求它，它只能以模型向框架请求任何东西的方式请求：工具调用。这就是休息前我悬而未决的问题的答案。使用技能时没有任何东西执行，但某些东西执行来加载它。

三。主体作为该调用的结果，在下一轮组装中，作为消息中的工具结果出现。

而 caption 是显而易见答案的成本。二十个主体，每个四千个标记，是八万个标记，在每个请求中，在用户输入任何内容之前。
-->

---
layout: default
---

<div class="label">答案</div>

# 三级加载

|| 层级 | 进入组装 | 五个 EGM 技能 |
|---|---|---:|
|| 名称、描述 | 每个请求 | ~900 个标记 |
|| SKILL.md 主体 | 模型请求后 | ~3,500 每个 |
|| references、scripts | 主体指向时 | 它读取的内容 |

<div class="caption mt-6">Anthropic 的术语：渐进式披露。</div>

<!--
三个层级。

第一级：每个技能的姓名和描述都进入每个请求，通常在系统提示词中作为一个简短列表。这是 900 个标记。它是一个索引，是它让模型知道技能存在的原因。

第二级：SKILL.md 的主体只在模型请求后进入。不是在用户输入请求的那一轮。在之后的某一轮。

第三级：references 和 assets 中的文件，以及脚本的输出，只有主体告诉模型去获取它们时才进入。你在休息前看到大象第 5 步中的那一行。一个长模板可以在磁盘上坐一百个会话，从不花费一个标记。

Anthropic 称之为渐进式披露。你已经知道这个想法。它就像目录。
-->

---
layout: default
---

<div class="label">L07 的术语</div>

# 一个技能跨越两个请求

<div class="grid grid-cols-2 gap-10 mt-4 text-sm">
  <div>
    <div class="font-semibold mb-2">请求 n</div>
    <div class="card mb-2" style="padding:0.6rem 0.9rem"><span>system: 规则 + <b>技能索引</b></span></div>
    <div class="card mb-2" style="padding:0.6rem 0.9rem">tools: read_file, load_skill, ...</div>
    <div class="card" style="padding:0.6rem 0.9rem">user: "let's plan a priority flag"</div>
    <div class="caption mt-3">回答: 调用 load_skill("elephant")</div>
  </div>
  <div>
    <div class="font-semibold mb-2">请求 n+1</div>
    <div class="card mb-2" style="padding:0.6rem 0.9rem">system: 未更改</div>
    <div class="card mb-2" style="padding:0.6rem 0.9rem">tools: 未更改</div>
    <div class="card" style="padding:0.6rem 0.9rem">user, 然后调用, 然后<br><b>tool: elephant 的 SKILL.md 主体</b></div>
    <div class="caption mt-3">回答: 向用户提出一个问题</div>
  </div>
</div>

<!--
这是用 L07 的术语表示的两个请求。

请求 n。组装后的请求携带索引。用户说 "let's plan a priority flag for taskr." 模型将其与大象的描述进行匹配，它的答案不是给用户的文本。它是一个工具调用。load_skill 是一个占位名称。在 Claude Code 中，该工具称为 Skill。在没有该工具的框架中，它可以像对 SKILL.md 路径的简单 read_file 一样。

框架应用该调用，结果是 SKILL.md 的主体。

请求 n+1。组装现在包括作为工具消息的结果，在对话中，不在系统提示词中。模型阅读方法并开始遵循它，对于大象来说这意味着问你问题而不是写代码。

所以 "哪一轮" 的答案是：模型请求之后的下一轮。一个技能花费一次额外的往返，这就是不在每个请求上支付一万八千个标记的价格。
-->

---
layout: default
---

<div class="label">EGM 第二步</div>

# 金鱼

- **全新会话**，无记忆
- **只有文档**作为输入
- **它能构建这个吗？** 如果不能，文档不合格

<!--
回到 EGM。大象拥有所有上下文。每一个论点，每一次澄清，你在深夜做出的每一个决定。这些上下文存在于会话中，不在文档中。

金鱼正好相反。一个没有记忆的全新会话。它获得设计文档，没有其他内容。

如果金鱼能阅读文档并解释系统回来，找出差距，并说它可以在不询问任何内容的情况下构建它，文档就承载了设计。如果不能，文档是你即将失去的上下文的薄层覆盖，就在会话关闭的那一刻。

注意金鱼在今天的术语中是什么。它是一个组装决策。整个技巧是一个用几乎没有任何内容组装的请求。
-->

---
layout: default
---

<div class="label">讲师的金鱼技能</div>

# 三位金鱼评审

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-book-open-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">A: 理解</div>
    <div class="text-sm opacity-70 flex-1">解释回来</div>
  </div>
  <div class="card">
    <ph-magnifying-glass-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">B: 批判</div>
    <div class="text-sm opacity-70 flex-1">找出差距</div>
  </div>
  <div class="card">
    <ph-hammer-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">C: 就绪</div>
    <div class="text-sm opacity-70 flex-1">我能构建这个吗？</div>
  </div>
</div>

<div class="caption mt-8">并行运行。它们的报告驱动下一轮编辑。</div>

<!--
Rensin 描述了一只金鱼。讲师的技能一次运行三只，每只有不同的工作，每只在自己的全新上下文中运行。

A，理解。阅读文档并解释系统回来。如果它的解释是错误的，文档不清楚，不是读者的问题。

B，批判。找出缺失、矛盾或一笔带过的内容。

C，就绪。以必须构建它的人的身份阅读。我明天能开始吗？我需要问什么？

然后三份报告回到大象会话，提出编辑建议，你再次运行金鱼。一轮又一轮，直到评审们没有剩下任何重要的内容。
-->

---
layout: default
---

<div class="label">五个技能，一种方法</div>

# EGM 技能集

|| 技能 | 步骤 | 产出 |
|---|---|---|
|| `peanuts` | 之前：喂大象 | 每个目录一个 README |
|| `elephant` | 成长设计 | 设计文档 |
|| `goldfish` | 测试文档 | `GOLDFISH.md` |
|| `egm-implement` | 按照文档构建 | `IMPLEMENT.md` |
|| `mean-review` | 彻底审查 | `MEAN-REVIEW.md` |

<!--
这是讲师实现的一套。五个技能。

peanuts 在任何事情之前运行，在现有代码库上。它自底向上遍历树，并在每个目录中写入一个简短的 README，使大象会话能够廉价地加载大型项目的结构。Peanuts 在叶子，hay 在树枝。注意这又是渐进式披露，对于代码库。

elephant 成长设计并写入文档。goldfish 测试它并保留记录，GOLDFISH.md。egm-implement 在该记录说文档通过且人工签署之前拒绝启动。然后它按照文档构建并保留分类账，IMPLEMENT.md，标记任何偏离计划的变更。mean-review 是 Rensin 的最后一步：一个单独的全新会话，被告知找出代码的每一个问题，不要客气。它将每次通过保存为自己的分类账文件中的 punch list，记录你修复了什么以及拒绝了什么，以便下一次通过知道上一次发现了什么。

每个技能通过名称交接给下一个。这种交接在他们的描述中，这就是智能体知道顺序的方式。
-->

---
layout: default
---

<div class="label">docs/egm/f26-course-design</div>

# 本课程的设计，六轮迭代

|| 内容 | 数量 |
|---|---:|
|| 金鱼轮次 | 6 |
|| 评审报告 | 18 |
|| 第 5 轮发现的矛盾 | 扣留的 `taskr` 规格 |

<div class="caption mt-6">Six rounds.</div>

<!--
这不是假设。你们正在学习的课程就是这样设计的。2026 年秋季的设计文档经历了六轮金鱼迭代。每轮三名评审，十八份报告，全部保存在工作区中。

第 5 轮的一个例子。早期草案计划将两个 taskr 课程扣留为第 7 周 gate 的新测试。批评者指出这与设计的其余部分相矛盾：全部十六节课都是必修的，后面的课程建立在前面课程的基础上，所以你不能扣留一节而不破坏后面的课程。该计划被删除，替换为 freshly written 的 gate 规格。

大象会话中的没有人看到这一点，因为会话中的每个人都知道这意味着什么。这就是金鱼的意义所在。
-->

---
layout: default
---

<div class="label">大象开场白，原文引用</div>

# 转向提示词：背景

```text
# Course Pivot

A feedback session from students has us revamping the course. This is an experimental course so that is alright.
Feedback is in instance/f26/feedback
We are still interested in many of our original design ideas.
Multifaceted pivot is upcoming

- Today's lecture is about Skills and EGM
- Break question gets at how and where skills get included during the Assemble step
- Students were given original Dave Rensin article about EGM last night to read before class today, some of them will have done that
- My implementation of EGM will be discussed and eventually given to them
  - Review the current EGM skill set and update with any best practices for skills that are newer or not implemented already
  - Add a second pass to fully document the EGM skills in a fashion that is meant for instruction so students can read, review, and learn
```

<style>
.slidev-layout pre, .slidev-layout pre code { white-space: pre-wrap !important; word-break: break-word; }
.slidev-layout .slidev-code { font-size: 0.62em !important; line-height: 1.4 !important; }
</style>

<!--
最后一块。一个真实的设计提示词，来自今天早上。这是讲师给编码智能体的提示词，逐字逐句，包括拼写错误。它被放在屏幕上是因为它被写出来给你们看的。

将其作为大象开场白阅读。第一块是背景：为什么变更，证据在哪里，什么保持不变。然后是今天的工作，包括你们刚刚回答的休息问题。

方向：完整文本在 assets/pivot-prompt.md 中。它跨越两张幻灯片；不要再缩小它。讲师决定在这里讨论多少课程变更。
-->

---
layout: default
---

<div class="label">大象开场白，原文引用</div>

# 转向提示词：要求

```text
- Current plan for P2 is to have students use a stronger model to adapt Aider to become more like Claude Code. This is the Analyze phase but with a new source repo
- The old repo stops at end of P1, Pair Programmer
- Decisions that need advice
  - Which model should students be using for maximum learning opportunity
  - Which harness "
- An additional 2-3 weeks of roadmap after this pivot needs to be discussed

Also, include this prompt verbatim in this or a future lecture, as an example of design and good prompts
```

<style>
.slidev-layout pre, .slidev-layout pre code { white-space: pre-wrap !important; word-break: break-word; }
.slidev-layout .slidev-code { font-size: 0.62em !important; line-height: 1.4 !important; }
</style>

<!--
第二块是要求。一个计划的声明。开放的决策，声明为开放，以及每个决策需要的建议。然后是需要讨论而非决定的路线图。

看这些动词。"Decisions that need advice"，不是 "decide"。"Needs to be discussed"，不是 "write the roadmap"。这些词语设定了智能体有多少权限。它被允许建议；它不被允许选择。Rensin 的第三部分在一行中：在交出工作之前限定权限。

最后一行是你们阅读它的原因。

*[提问: 如果你是智能体，你会问回的第一个问题是什么？]*
-->

---
layout: two-col
---

## 做得好的地方

已确定和开放的内容分开。证据明确。要求建议，而非答案。

::right::

## 金鱼会指出的问题

没有验收标准。"Which harness" 未完成。没有声明截止日期。

<div class="caption mt-10">Rensin, 第一部分：输出前的标准。</div>

<!--
做得好的地方。它将已确定的内容与开放内容分开，因此智能体不会重新争论计划。它命名证据的位置而不是总结它。在困难的调用上它要求建议，这保留了判断权与拥有结果的人在一起。这是 Rensin 的第一部分。

金鱼会指出的问题。没有验收标准：完成的讲座是什么样的，什么使建议足够好？Harness 问题在行中间中断。它从未说明这堂课在三点，智能体必须从日期推断出来。

大象会话会问过所有这三个。这是访谈步骤，也是方法从问题开始的原因。

*[提问: 你的金鱼还会指出什么？]*
-->

---
layout: default
---

<div class="label">总结</div>

# 要点

- **技能**是可移植上下文加触发器
- **索引**始终在内；主体是挣来的
- **金鱼**是一个几乎为空的组装请求

<!--
三件事要记住。

技能是可移植上下文加触发器。描述是触发器，技巧在那里。

索引始终在请求中。主体是挣来的，在一次往返之后，当模型请求它时。

金鱼也是一个组装决策：一个只包含文档的请求，这就是它能告诉你文档是否独立站立的原因。
-->

---
layout: default
---

<div class="label">总结</div>

# 周二之前

- **Rensin 的文章**，如果还没读
- **一份 SKILL.md**，仔细阅读
- **你的描述**为一个你重复使用的方法

<!--
周二之前。

如果你还没有读文章，去读。它很短，是后半部分一切内容的来源。

仔细阅读一份 SKILL.md。讲师的 EGM 技能现在在 materials 仓库中，在 egm 下，带有一个技能解剖页面和每个技能的演练。从那里的 README 开始。

写一份描述。挑选一些你发现自己向编码智能体重复输入的内容：你希望提交信息如何书写，你希望测试如何运行。写那段描述，让智能体在正确时刻加载它。只写描述。那是困难的部分。

方向：技能和演练在 materials/egm 发布（commit 4f76140）。
-->

---
layout: end
---

<!--
问题。

如果房间安静，两个提示。今天五个技能中，哪一个会改变你黑客马拉松 1 晚上的做法？以及你当晚使用的方法的描述中会写什么？

今天之后的任何问题都发到 Ed。周二见。
-->
