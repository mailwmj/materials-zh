---
theme: ../theme
title: "L06: From pair-coding to agent orchestration"
info: |
  EECS 498 AASE — 第06讲
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 从结对编程到智能体编排

## 第06讲 · 2026年9月17日

<!--
今天有三个部分。这节课之后发布的项目包，
如何与你必须理解的模型的代码一起工作，以及
他们程序已有的一个命令如何成长为工具调用和智能体循环。
-->

---
layout: default
---

<div class="label">今天</div>

# 三个部分

1. 项目：`ai-assistant`
2. 与模型协作
3. 从编辑块到工具调用

<!--
命名这三个然后继续。第一个是后勤，应该很快。第二个是
应该花时间在上面。第三个在 Analyze 开始的地方结束。
-->

---
layout: section
---

# 项目

## `ai-assistant`，10月6日截止

---
layout: default
---

<div class="label">项目构建</div>

# 你自己的结对编程伙伴

- 一个关于你选择的文件的终端对话
- 模型提出编辑，你批准
- 每个批准的变更变成一个提交

<div class="caption mt-6">Aider，你已经使用了三周的工具。现在你写一个。</div>

<!--
保持这个通用。他们已经使用 Aider 三周了，所以他们知道结对编程伙伴做什么：你选择文件，要求一个更改，阅读 diff，
它进入 git。他们现在正在用 Python 写一个小型的，
针对一个模型端点。

它在一个单独的目标代码库上工作，永远不会在它自己的代码库上。

它故意不做什么：自己找文件，运行命令，或
选择它的下一步。记住这一点。今天的最后部分正好添加
了这些东西。

他们用 Aider 构建它，而且它从不调用 Aider。告诉他们先读 EXAMPLES.md。它逐会话展示完成的程序。
-->

---
layout: default
---

<div class="label">SPEC.md 第3节</div>

# 工作人员提供什么和你决定什么

| 工作人员提供 | 你决定 |
|---|---|
| 合同：7个特性 | 模块、接口和状态 |
| 一个 starter 循环，`bin/assistant`，`bin/test` | 估计器、修剪策略、文件分隔符 |
| `ENDPOINT.md`，以及 `SPEC.md` §5 中的检查 | 错误消息和提交所有权 |
| | 你的测试 |

<!--
Starter 是 assistant/cli.py 中的一个 echo 循环，/quit 已经连接。它是一个开始的地方，不是一个必须保留的设计。

数据包不如他们可能期望的那么具体，这是有意为之。
SPEC.md 说程序从外部做什么。右边的一切都是
他们的，而评分表的需求行为为 settling 这些决定付钱，
而不是重复规格说明。没有规定的架构和没有隐藏的测试套件。

这对今天的中间很重要。当规格说明没有命名一个决定时，
他们必须命名一个，而一个名字是让他们和模型能够谈论它的
东西。
-->

---
layout: default
---

<div class="label">README.md</div>

# 工作顺序

1. 阅读数据包，`EXAMPLES.md` 优先
2. 设计和图，冷审查
3. 增量，每个规格说明先写
4. 现场证据、使用、反思

<div class="caption mt-6">设计在应用程序代码之前提交。</div>

<!--
步骤2：design/SYSTEM.md 和三张图（组件和数据流，一个请求序列，编辑生命周期），使用 DESIGN-GUIDE.md 作为检查清单。
然后一个新的 Aider 会话在没有起草它的聊天的情况下审查文档。这是找到缺失内容的最快方法。

步骤3：每个增量之前 specs/ 中的一个增量规格说明，来自五部分
模板。只加载该增量需要的文件并在移动之前验证。
在一次性目标代码库上尝试程序，永远不会在这个上。

步骤4：SPEC.md 第5节中的现场编辑提示词协议，每次尝试都记录，USAGE.md，REFLECTION.md，以及图更新以匹配他们构建的内容。
-->

---
layout: default
---

<div class="label">RUBRIC.md</div>

# 项目构建如何评分

| 一半 | 分数 |
|---|---:|
| 规格说明和设计 | 50 |
| 实现和测试 | 50 |

- 代码之前的规格说明，从提交顺序读取
- 没有隐藏的测试或要求
- 没有痛苦分

<!--
一半分数是写作。需求、架构、增量规格说明、
三张图、代码之前的规格说明和反思。

代码之前的规格说明从 git 历史评分，所以他们提交的顺序是
证据。

没有痛苦分是评分表自己的短语。更多的提示词、更多的失败
和更长的日志什么也得不到。一个成功的有效工作流程可以获得满分。
未完成功能的测试作为严格的 xfail 留在套件中，从不
删除或削弱。
-->

---
layout: default
---

<div class="label">日期</div>

# 日期和交付物

| 时间 | 事项 |
|---|---|
| 这节课之后 | 数据包，在你的代码库中 |
| 任何应用程序代码之前 | `design/SYSTEM.md` 和图表 |
| 9月24日周四，晚上 | Hackathon 1，单独的 |
| 10月6日周二，晚上11:59 | 项目构建截止 |

<div class="caption mt-6">一个代码库，整个学期。提交你的 Aider 历史。</div>

<!--
Hackathon 1 是一个单独的两小时评估，在一个提供的应用程序上。它不需要一个完成的项目构建。

迟交的作业在三天内每天损失10%。推送。一个只存在于笔记本上的提交不是提交。

同一个代码库贯穿 Analyze 和 Create，所以保留历史。
Aider 不提交 .aider.chat.history.md，所以他们在每个会话结束时自己提交它，而且从不把它添加到 Aider 的可编辑上下文中。
-->

---
layout: section
---

# 与模型协作

## 理解它写什么和它说什么

---
layout: default
---

<div class="label">你现在的工作</div>

# 谁理解代码

- 模型写大多数行
- 你拥有决策和结果
- 在合适层级上的理解

<!--
这是课程建立在的转变。手写代码已经减少了，
逐行阅读每一行也是如此。没有人能跟上模型
逐行阅读的速度，而且这不是目标。目标是知道代码决定什么以及它是否做你 intended 的事。"模型写了它"仍然不是任何评分表的行的答案。

所以工程工作转移了。它转移到计划、提示词，最重要的是
转移到理解返回的内容：代码，还有代码周围的散文，
模型说它做了什么的地方。
-->

---
layout: default
---

<div class="label">上下文存在于两个地方</div>

# 你脑海中记住的

- 设计以及为什么是这样
- 你在构建中的位置
- 什么实际被验证了
- 模型只声称了什么

<div class="caption mt-6">模型在会话之间不保留这些中的任何一个。</div>

<!--
每个会话模型从你给它的文件开始。你是记得修剪策略是上周二决定的、增量3
做了一半、"所有测试通过"来自模型而不是来自 bin/test 的人。

最后两个要点是危险的组合。已验证的和声称的在
聊天窗口中看起来完全一样。把它们分开是你的工作，而且它是在你的脑海中完成的，除非你给它一个更好的存放地方。
-->

---
layout: default
---

<div class="label">困难部分</div>

# 在合适距离上的判断

| 距离 | 它看起来像什么 |
|---|---|
| 太近 | 阅读它写的每一行 |
| 太远 | 接受你从未检查过的内容 |
| 恰到好处 | 检查合同和结果 |

<div class="caption mt-6">不是每一行。每一个决策。</div>

<!--
这真的很困难，而且没有人第一周就做对。

太近，你以人类速度在机器速度写作的模型后面阅读，你仍然错过了重要的决策。太远，你橡皮图章批准，这就是一个 stale-preview bug 如何进入一个"工作"的项目构建。

恰到好处意味着你知道哪些决策重要，你检查每一个都被
遵守，你让测试和 diff 携带细节。你不能在没有一个指向决策的方法的情况下做到这一点。也就是下一张幻灯片。
-->

---
layout: statement
---

工作关系需要*共享名称*。

<!--
停顿。这部分的其他部分是一个构造、参考点，然后
还有什么做同样的工作的问题。
-->

---
layout: default
---

<div class="label">你定义的一个构造</div>

# 参考点

| 名称 | 它指向什么 |
|---|---|
| `F6` | 预览和应用编辑 |
| `design/SYSTEM.md` §6 | 你的安全决策在哪里 |
| `specs/03-preview.md` | 交付它的增量 |
| `test_stale_preview_refused` | 什么检查它 |

<div class="caption mt-4">一个短的、稳定的名称，你、模型和工作人员以相同的方式解析它。</div>

<!--
参考点是项目状态的关键字。它是密集的："F6" 用一个特征承载到桌子对面的人或进入提示词，而一个工具链可以在没有粘贴段落的情况下解析它。

当你自己写代码时，一个东西的名字住在代码中，你可以去看。当一个模型根据你的计划和提示词写代码时，名字必须住在模型、评分者和你都能指向的地方。
否则每一个都在解释一个句子。

在上下文预算下，这种压缩不是整洁。它使对话
适合。
-->

---
layout: default
---

<div class="label">从规划这节课</div>

# 会话中的决策编码

<div style="background:#16181c;color:#e4e4e4;font-family:'IBM Plex Mono',ui-monospace,monospace;border-radius:8px;padding:18px 22px;font-size:0.8rem;line-height:1.5">
<div style="color:#8a8f98;margin-bottom:8px">model: decisions before I write the slides</div>
<div style="display:grid;grid-template-columns:4.5rem 1fr;row-gap:0">
<div style="padding:5px 0;border-bottom:1px solid #5a606b;color:#7aa2f7">Code</div><div style="padding:5px 0;border-bottom:1px solid #5a606b;color:#7aa2f7">Decision</div>
<div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e0af68">D1</div><div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e4e4e4">The live demo is Claude Code in a repository</div>
<div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e0af68">D2</div><div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e4e4e4">The approval layer moves out of this lecture</div>
<div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e0af68">D3</div><div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e4e4e4">READ and RUN use made-up markers, not F5's</div>
<div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e0af68">D4</div><div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e4e4e4">The tool-call build-up is slides, not live code</div>
<div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e0af68">D5</div><div style="padding:5px 0;border-bottom:1px solid #2e333b;color:#e4e4e4">The chat break becomes a discussion question</div>
</div>
<div style="color:#8a8f98;margin-top:8px">… D6 to D18</div>
</div>

<!--
这是基于用 Claude Code 规划这节课建模的。会话的设置是这样的
每当模型呈现三个或更多的决策、选项或发现时，
每一个都得到一个短的编码：D 代表决策，O 代表选项，Q 代表问题。真正的表格运行到 D18。

指出关于模型的没有任何改变。一个约定文件中的一行
告诉它编号事物。回报在下一张幻灯片上。
-->

---
layout: default
---

<div class="label">回报</div>

# 用代码回答

<div style="background:#16181c;color:#e4e4e4;font-family:'IBM Plex Mono',ui-monospace,monospace;border-radius:8px;padding:18px 22px;font-size:0.8rem;line-height:1.5">
<div style="display:grid;grid-template-columns:4.5rem 1fr;row-gap:0;row-gap:2px">
<div style="color:#9ece6a">you</div><div>D1 yes. D3 yes. D4 no, do READ live. D5 yes.</div>
<div style="height:10px"></div><div></div>
<div style="color:#7aa2f7">model</div><div>D1, D3, D5 kept.</div>
<div></div><div>D4 changed: READ is live, RUN stays on slides.</div>
<div></div><div>D2 still open. Keep it out?</div>
<div style="height:10px"></div><div></div>
<div style="color:#9ece6a">you</div><div>D2 yes.</div>
</div>
</div>

<div class="caption mt-6">十八个决策中的五个，在两行中。</div>

<!--
没有代码，第一个回复是一个段落："我同意演示想法和
标记，但我宁愿读部分 live..." 模型必须猜测
每个条款是关于什么的。用代码，每个条款正好落在
一个决策上，模型的答案以同样的方式可检查。它甚至发现 D2 从未被回答。

这是小规模的工作关系。模型用名称呈现，
你用名称回答，没有任何东西在两者之间丢失。
-->

---
layout: default
---

<div class="label">DESIGN-GUIDE.md 第1节</div>

# 数据包留给你的命名

- SPEC 命名其7个特性
- 你命名你的验收标准
- 你的每个名称都 traceable 到一个特性

<div class="caption mt-6">"重复 SPEC.md 第4节什么也得不到。" RUBRIC.md</div>

<!--
这就是为什么数据包不那么具体。SPEC.md 在特性处停止。标记
估计器、修剪策略、分隔符、如何标记拥有的提交、
每个错误说什么：这些是决策，DESIGN-GUIDE 第1节要求
每一个作为带有自己稳定 ID 的验收标准，映射到它服务的
特性。评分表称这种映射为可追溯并为它支付10分。

格式是他们的。像 F6-3 代表"一个 stale 预览被拒绝"这样的东西
可以。重要的是它很短，它从不改变含义，并且它出现在
设计、增量规格说明、测试名称和提交中。
-->

---
layout: default
---

<div class="label">人类对模型</div>

# 按名称提示

<div class="grid grid-cols-2 gap-6">
<div>

**按意译**

```text
fix the approval stuff so
it's safer
```

</div>
<div>

**按名称**

```text
specs/03, task 2 only.
Deliver F6-3.
Make test_stale_preview_refused
pass. Edit preview.py only.
```

</div>
</div>

<!--
左边给模型一个句子来解释。它会解释它，并且
它会挑选自己关于"更安全"的想法。

右边给它一个增量、一个任务边界、一个标准、一个决定它是否完成的测试，以及它可以触碰的文件。这些中的每一个都是一个模型可以在其上下文中解析的名称，并且每一个都是学生之后可以检查的东西。

一个意译需求的提示词会漂移，而模型无法
分辨。ID 不会漂移。
-->

---
layout: default
---

<div class="label">模型对人类</div>

# 用你的名称要求回答

```text
Before I review: which criteria does this change deliver,
what did you run, and what did you not verify?
```

```text
Delivers    F6-3
Touched     assistant/preview.py
Ran         nothing
Unverified  F6-3 against a real target repository
```

<!--
名称在两个方向上都工作。理解模型输出的大部分工作是
分离它做了什么和它说它做了什么。用你的术语要求报告，这种分离就是一个检查清单而不是阅读练习。

"Ran: nothing" 是你正在寻找的那一行。没有问，那会是一个
段落说变更正确处理了 stale 预览。

然后检查它。名称使主张可检查。它们不使它们成为真的。
-->

---
layout: default
---

<div class="label">人类对人类，人类对工具链</div>

# 一行描述项目状态

```text
specs/03 · F6-3 done · F6-4 red: test_write_failure_restores
```

- 给工作人员或邻居的消息
- 新会话的第一行
- 提交消息

<!--
那一行是一个完整的项目状态陈述，而且它没有任何意译。Office hours 的工作人员知道打开什么。一个新的 Aider 或 Claude Code 会话知道要加载什么。提交历史读作决策记录。

这也是"你脑海中记住的"的答案。用这些术语写下来，
它停止只活在脑海中。
-->

---
layout: default
---

<div class="label">在哪里失效</div>

# 名称如何失效

- 意译替换了 ID
- 模型发明了一个 ID
- 命名的文件不在上下文中
- 名称在构建中途改变了含义

<!--
意译：提示词说"修剪规则"模型选择一个。
发明的 ID：模型很高兴引用 F6-7 当没有 F6-7 时。只使用
解析到文件一行的名称。

不在上下文中：一个名称只有它能解析到的东西一样好。如果 SYSTEM.md
没有加载，"第6节"是一个猜测。检查会话能看到什么。

改变含义：在构建中途重新编号标准会破坏引用它们的每个测试名称和提交。添加新 ID，不要重用旧的。
-->

---
layout: default
---

<div class="label">现场演示</div>

# 代码库中的 Claude Code

- 按参考点命名任务
- 用这些名称要求报告
- 检查每个声明

<!--
切换到终端并驱动它。
演示内容待写。

叙述检查，而不是打字。每次模型做一个声明，大声说出来
它是被验证的还是仅仅被声明的。
-->

---
layout: default
---

<div class="label">五分钟与邻居</div>

# 阅读模型输出的其他方式

参考点是一种构造。

命名**一个其他习惯或构造**，使模型的输出更容易理解，或以其他方式促进沟通。

<div class="caption mt-6">每人一行。我们将收集五个。</div>

<!--
**"好了，Zoom，我们休息了。"**

巡视。五分钟。

**"好了，Zoom，我们回来了。"**

花五分钟并写在黑板上。如果房间安静，给出种子：
- 在任何编辑之前要求一个计划
- 每个回复的固定形状
- 什么没有被验证的列表
- 小 diff，一次一个任务
- 测试作为法官，不是模型的摘要
- 模型始终加载的约定文件
- 模型必须保持最新的图
- 引用 ID 的提交消息

结束时指出这些中的每一个都是工程师定义的构造，
不是模型的一个特性。
-->

---
layout: section
---

# 从编辑块到工具调用

## 成长你的程序拥有的一个命令

---
layout: default
---

<div class="label">SPEC.md F5</div>

# 你的程序拥有的一个命令

```text
greet.py
<<<<<<< SEARCH
print("Hello")
=======
print("Hi")
>>>>>>> REPLACE
```

<div class="caption mt-4">来自模型的文本。你的代码解析它并行动，在是的时候。</div>

<!--
从另一个方向看这个问题。模型写文本。程序找到一个
它认识的标记，拉出参数，并在询问后用它们在世界上做一些事情。

那是一个命令。他们的程序正好有一个。这节课剩下部分的问题是
当添加第二个时会发生什么。
-->

---
layout: default
---

<div class="label">结对编程伙伴卡壳的地方</div>

# 当模型需要一个文件时

```text
you     why does greet() print twice?
model   I can't tell without main.py, which calls greet().
you     /add main.py
you     ok, look again
```

<div class="caption mt-4">模型知道它需要什么。只有你能获取它。</div>

<!--
这在使用 Aider 时 constantly 发生，而且会与他们的程序发生。模型
看到一个文件，推理它，并说它接下来需要什么。Aider 不能
自己读取一个文件。它可以提供一个来添加，但你仍然必须说是
并重新发送。他们的 Stage 1 程序更严格：只有 /add 把文件放在
模型面前。

所以在这个交换中，人没有做任何思考。模型命名了文件。人把它打了回去。
-->

---
layout: default
---

<div class="label">问题</div>

# 为什么不让它读取

- 它已经命名了它需要的文件
- 读取不会改变磁盘上的任何东西
- 你只是转发而已

<div class="caption mt-6">所以给它一个询问的方式。</div>

<!--
首先认真对待反对意见。我们让用户选择文件是有目的的：
它保持上下文小并保持用户在控制中。这些都是真正的原因。

但读取是最安全的动作。磁盘上没有任何东西改变，
而且没有任何东西可以撤销。如果模型已经可以命名文件，
让人把这个名字复制到 /add 增加了一个步骤和零判断。

所以我们让它询问。和它已经询问编辑的方式一样。
-->

---
layout: default
---

<div class="label">添加一个命令</div>

# 一个 READ 命令

```text
I need to see the tests before I change anything.
<<<<<<< READ
tests/test_cli.py
>>>>>>> END
```

<div class="caption mt-4">你的代码读取文件并将其内容发回。</div>

<!--
同样的想法，不同的标记。模型请求一个文件。工具链读取它并
将其内容放在下一条消息中。

与 /add 比较。/add 是用户选择。READ 是模型选择，因为
它片刻前看到的东西。

这不在 Stage 1 规格说明中，这里的标记是为课 lecture 虚构的。重点是它需要多么少。
-->

---
layout: default
---

<div class="label">添加另一个</div>

# 一个 RUN 命令

```text
Let me check the suite first.
<<<<<<< RUN
bin/test
>>>>>>> END
```

<div class="caption mt-4">你的代码运行它并将其输出发回。</div>

<!--
再一次。模型请求一个命令。工具链运行它并返回它
打印的内容。

标记它：这是第一个改变机器而不是文件的命令，
而 git 不会撤销它。在最后回到这一点。
-->

---
layout: default
---

<div class="label">工具链成长</div>

# 三个命令的分发器

```python
for block in parse_blocks(reply):
    if block.kind == "EDIT":
        result = propose_edit(block.path, block.search, block.replace)
    elif block.kind == "READ":
        result = read_file(block.path)
    elif block.kind == "RUN":
        result = run_command(block.command)
    messages.append({"role": "user", "content": result})
```

<!--
逐行走它。一个解析器，每个命令一个分支，每个分支以相同的方式结束：一个结果回到对话中。

问房间第四个命令会是什么。搜索，列出一个目录，获取一个
URL。每一个都是另一个 elif。让他们注意到分支看起来都一样。
-->

---
layout: default
---

<div class="label">模式</div>

# 每个命令的一种形态

| 块 | 名称 | 参数 |
|---|---|---|
| `SEARCH/REPLACE` | `edit_file` | path, search, replace |
| `READ` | `read_file` | path |
| `RUN` | `run_command` | command |

<div class="caption mt-6">一个名称加参数。那就是一个*工具调用*。</div>

<!--
这是需要放慢速度的步骤。剥离标记，每个命令都是
同样的两件事：哪一个，以及用什么。

一旦你看到这一点，自定义标记只是一个笨拙的编码。向模型描述每个
命令为一个名称、一个描述和一个参数模式，让它以结构化格式回复
而不是。那就是函数调用。我们没有添加一个新想法。我们
泛化了他们的程序已经有的一个想法。
-->

---
layout: default
---

<div class="label">函数调用</div>

# 工具调用的形态

```json
{"tool_calls": [
  {"name": "read_file",   "arguments": {"path": "tests/test_cli.py"}},
  {"name": "run_command", "arguments": {"command": "bin/test"}},
  {"name": "edit_file",   "arguments": {"path": "greet.py", "search": "...", "replace": "..."}}
]}
```

<div class="caption mt-4">同样的三个命令，无需自定义格式即可解析。</div>

<!--
同样的表格，在 JSON 中。端点做格式化工作，工具链
得到一个它可以循环的列表而不是它必须扫描标记的文本。

没有其他改变。分发器是相同的 if/elif，按名称键控。一旦命令只是表格中的名称，添加一个就是添加一行，这就是为什么工具调用扩展到任何函数能做的任何事情。
-->

---
layout: statement
---

模型从不运行任何东西。它*询问*。

<!--
从这部分带走的句子。模型发出文本。每一个读取、每一个命令、每一个写入都是由学生编写的代码执行的，而且那个代码可以说不。
-->

---
layout: default
---

<div class="label">谁驱动</div>

# 谁发回结果

```text
you     why does bin/test fail?
model   READ tests/test_cli.py
you     (contents) keep going
model   RUN bin/test
you     (output) keep going
model   SEARCH/REPLACE greet.py
```

<div class="caption mt-4">每一个"继续"都是你，作为循环。</div>

<!--
添加了 READ 和 RUN 但没有其他改变，这是一个会话的样子。模型询问，工具链行动，然后一个人必须发回结果并说继续。

那个人没有做任何思考。他们是一个带键盘的循环。所以自动化他们。
-->

---
layout: default
---

<div class="label">将人自动化</div>

# 选择其步骤的循环

```python
while True:
    reply = model.call(messages, tools=TOOLS)
    if not reply.tool_calls:
        return reply.text          # the model is done
    for call in reply.tool_calls:
        result = execute(call)     # your code, your rules
        messages.append(result)
```

<!--
六行，"继续"消失了。那里没有任何东西说读取哪个文件，下一步是什么，或什么时候停止。模型从用户一次输入的请求中提供所有三个。

那就是一个智能体。它是四张幻灯片前的分发器，人
被 while True 替换了。
-->

---
layout: default
---

<div class="label">循环改变了什么</div>

# 循环前后

- 模型仍然只写文本
- 你的代码仍然执行每一个动作
- 模型现在选择下一步

<div class="caption mt-6">只有最后一行改变了。</div>

<!--
循环看起来像是移交控制。它比那更窄。模型
仍然只产生文本。每一个读取、运行和编辑仍然是学生
编写的代码，而且那个代码可以拒绝。移动的唯一一件事是
谁决定下一步发生什么：在他们的 Stage 1 程序中，是人；在循环中，是模型。
-->

---
layout: default
---

<div class="label">还剩下什么</div>

# 循环仍然需要什么

- 迭代和令牌的上限
- 重复调用的检测器
- 一个干净的循环中段中断
- `run_command` 之前的批准

<div class="caption mt-6">撤销覆盖文件。它不覆盖 shell。</div>

<!--
一个运行直到模型说它完成的循环有时永远不会完成。
相同的调用，相同的参数，永远。或者它决定它完成了当它没有时。
所以停止条件成为代码而不是用户按下的键。

而 RUN 是他们的安全网不覆盖的命令。/undo 是一个 git
操作，而 git 不会撤销 rm -rf、包安装或仓库之外的任何东西。他们的 Stage 1 程序在写入文件之前询问。智能体必须在运行命令之前询问。

那就是 Analyze。今天不要走更远。
-->

---
layout: default
---

<div class="label">接下来做什么</div>

# 这周和接下来两周

| 时间 | 事项 |
|---|---|
| 今晚 | 阅读 `EXAMPLES.md`，然后 SPEC |
| 任何应用程序代码之前 | 设计、图表、冷审查 |
| 9月24日周四，晚上 | Hackathon 1 |
| 10月6日周二，晚上11:59 | 项目构建截止 |

<!--
大声说出顺序。阅读、设计、审查，然后增量。

项目构建是他们练习今天中间的地方：命名自己的
验收标准，用这些名称提示 Aider，并检查返回的内容。
-->

---
layout: end
---