---
theme: ../theme
title: "L05: Modern agentic CLIs"
info: |
  EECS 498 AASE — 第05讲
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 现代智能体 CLI

## 第05讲 · 2026年9月15日

<!--
两个实验课班次都已经开过课了。不要预览 Lab01 也不要揭示
项目构建；他们都已经有了。这节课 survey Lab01 开启的类别。

Claude Code 只出现在这台机器上。房间里没有人需要
账户，也没有幻灯片要求提供账户。
-->

---
layout: default
---

<div class="label">当前位置</div>

# Lab01 留下了一个未解决的问题 {.assert}

你按谁选择了每个部分对提示词进行了排序。

| 提示词部分 | 选择者 |
|---|---|
| 系统提示词 | Aider，按模式 |
| 添加和只读文件 | 你，按命令 |
| 代码库地图 | Aider，按排名 |
| 历史记录 | 你开始，Aider 修剪它 |
| 你的消息 | 你 |

**你从未碰过的两行。其中一行冒充了智能。**

<!--
三十秒的回忆，仅此而已。周一的房间昨天做了这个，周二的
九十分钟前完成了，所以这是一个指针而不是回顾。

Lab01 的表格， unchanged。它在这里是因为整节课都是关于那个
右列，而且他们昨天的答案即将变得更长。

读右边的列然后停止。不要回答下一张幻灯片。
-->

---
layout: default
---

<div class="label">Aider 是其中之一</div>

# Aider、模型与代码库

<div class="mt-2">
<svg viewBox="0 0 900 330" style="width:100%;max-height:330px" role="img"
     aria-label="Aider 位于你的代码库和模型之间。Aider 读取文件，将提示文本发送给模型，接收回复文本，然后写入和提交。没有任何线连接模型和代码库。">
  <defs>
    <marker id="ar-b" markerWidth="9" markerHeight="9" refX="8" refY="3" orient="auto"><path d="M0,0 L0,6 L8,3 z" fill="var(--c-primary)" /></marker>
    <marker id="ar-a" markerWidth="9" markerHeight="9" refX="8" refY="3" orient="auto"><path d="M0,0 L0,6 L8,3 z" fill="var(--c-amber)" /></marker>
  </defs>
  <rect x="300" y="8" width="300" height="58" rx="8" fill="var(--c-highlight)" stroke="var(--c-amber)" stroke-width="2.5" />
  <text x="450" y="34" text-anchor="middle" style="font:600 17px var(--font-mono)" fill="var(--c-amber)">the model</text>
  <text x="450" y="53" text-anchor="middle" style="font:400 12.5px var(--font-sans)" fill="var(--c-ink-soft)">text in, text out</text>
  <rect x="40" y="128" width="820" height="92" rx="10" fill="var(--c-bg-1)" stroke="var(--c-rule-strong)" />
  <text x="58" y="151" style="font:500 11px var(--font-mono); letter-spacing:0.14em" fill="var(--c-ink-muted)">AIDER</text>
  <rect x="70" y="160" width="220" height="44" rx="6" fill="var(--c-bg-0)" stroke="var(--c-primary)" stroke-width="1.5" />
  <rect x="340" y="160" width="220" height="44" rx="6" fill="var(--c-bg-0)" stroke="var(--c-primary)" stroke-width="1.5" />
  <rect x="610" y="160" width="220" height="44" rx="6" fill="var(--c-bg-0)" stroke="var(--c-primary)" stroke-width="1.5" />
  <g style="font:500 14.5px var(--font-mono)" fill="var(--c-ink)" text-anchor="middle">
    <text x="180" y="187">assemble request</text>
    <text x="450" y="187">parse and apply</text>
    <text x="720" y="187">commit</text>
  </g>
  <rect x="40" y="256" width="820" height="54" rx="10" fill="var(--c-bg-1)" stroke="var(--c-rule-strong)" />
  <text x="450" y="289" text-anchor="middle" style="font:500 15px var(--font-mono)" fill="var(--c-ink)">your files  ·  git repository</text>
  <g stroke="var(--c-amber)" stroke-width="2" fill="none">
    <path d="M180,158 V100 H350 V70" marker-end="url(#ar-a)" />
    <path d="M550,68 V100 H450 V156" marker-end="url(#ar-a)" />
  </g>
  <g style="font:500 12.5px var(--font-sans)" fill="var(--c-amber)" text-anchor="middle">
    <text x="264" y="91">prompt text</text>
    <text x="502" y="91">reply text</text>
  </g>
  <g stroke="var(--c-primary)" stroke-width="2" fill="none">
    <path d="M110,252 V210" marker-end="url(#ar-b)" />
    <path d="M450,208 V250" marker-end="url(#ar-b)" />
    <path d="M720,208 V250" marker-end="url(#ar-b)" />
  </g>
  <g style="font:400 12.5px var(--font-sans)" fill="var(--c-ink-muted)">
    <text x="122" y="236">reads</text>
    <text x="462" y="236">writes</text>
    <text x="732" y="236">records</text>
  </g>
</svg>
</div>

<div class="caption">模型不触碰任何文件。</div>

<!--
Lab01 的幻灯片，如他们所见。不要重新教授它：让他们命名三个
盒子然后继续。

三个盒子是六个步骤中的五个，这就是把它放回去的意义。组装请求。模型回答。解析回复并应用
它。记录它。没有人昨天把它们画成一条带编号的线，
下一张幻灯片是这样的。
-->

---
layout: default
---

<div class="label">循环，写出来</div>

# 六个步骤，按顺序

<div class="mt-4 text-base">

<div class="grid grid-cols-6 gap-2 items-stretch">
  <div class="card"><div class="text-sm font-semibold">1 组装</div><div class="text-xs opacity-70 mt-1">你和 Aider，按表格</div></div>
  <div class="card"><div class="text-sm font-semibold">2 回答</div><div class="text-xs opacity-70 mt-1">模型。文本输出</div></div>
  <div class="card"><div class="text-sm font-semibold">3 解析</div><div class="text-xs opacity-70 mt-1">Aider，按固定规则</div></div>
  <div class="card"><div class="text-sm font-semibold">4 应用</div><div class="text-xs opacity-70 mt-1">Aider，否则它拒绝</div></div>
  <div class="card" style="border-color:var(--c-amber)"><div class="text-sm font-semibold" style="color:var(--c-amber)">5 修复</div><div class="text-xs opacity-70 mt-1">仅当4失败时</div></div>
  <div class="card"><div class="text-sm font-semibold">6 提交</div><div class="text-xs opacity-70 mt-1">Aider，每次</div></div>
</div>

</div>

<div class="caption mt-6">一个请求，从头到尾，顺序从不变化。步骤2是唯一一个属于模型的。</div>

<!--
这张幻灯片之前的图是三个盒子。这和一条线是同一回事，
因为论点需要一个顺序来指向。

六个步骤中的五个在那张图上。步骤5是它没有画出来的，下一张幻灯片是原因。

需要强调的数字：六个中只有一个属于模型。大声问出来
而不是说出来。在其他每一步，决定下一步发生什么的是
他们，或者某人提前写下的规则。
-->

---
layout: default
---

<div class="label">回环的那一个</div>

# 修复是插入，不是阶段

<div class="mt-4 space-y-2">

- 不匹配的块是**正常的**，不是例外
- Aider 将失败写回作为提示词，步骤2再次运行
- 最多三次，然后它放弃并告诉你

</div>

<div class="caption mt-6">它在步骤2重新进入。它从不选择不同的步骤，也从不选择不同的文件。</div>

<!--
用这些原话来说：不匹配的块是正常的，不是
例外。模型引用它想要替换的行，引用必须
与文件完全匹配。一行它记错了，块就不
适用。Aider 将错误发回作为下一个提示词并再次运行步骤2，最多
三次，然后它停止并告诉你。

这是保持"Aider有一个循环"说法诚实的幻灯片，二十分钟
后，所以不要跳过它。

修复是整个序列中唯一的后向箭头，这就是为什么它不在
Lab01的图上：三个盒子从左到右无法画出它。

使它成为固定循环而不是决策的两件事：它总是
在同一个步骤重新进入，并且它重新进入时使用的动作库和
开始时的一样。第二次尝试不会产生新的可能性。

说出重试限制。三次，然后停止。有计数器的循环
仍然是循环，而且它仍然不做选择。
-->

---
layout: statement
---

谁选择*下一个动作*？

<!--
这就是整个乐器。一个问题，问任何工具，现在或三年后
当今天的所有名字都变了。

大声说出来：这些幻灯片上的名字会很快过时，而这个问题
不会。
-->

---
layout: section
---

# 自主性梯度

## 五个层级，以及你一直站在哪里

---
layout: default
---

<div class="label">梯度</div>

# 五个层级，不是排名 {.assert}

<div class="mt-2">
<svg viewBox="0 0 900 300" style="width:100%;max-height:300px" role="img"
     aria-label="五个层级从左到右上升：自动补全、代码旁边的聊天、命令行结对编程、智能体命令行、多个智能体同时运行。一条分界线落在第三层和第四层之间，标记模型开始选择下一个动作的位置。">
  <line x1="60" y1="262" x2="860" y2="262" stroke="var(--c-rule-strong)" stroke-width="2" />
  <g stroke="var(--c-rule-strong)" fill="var(--c-bg-1)">
    <rect x="70" y="222" width="140" height="34" rx="6" />
    <rect x="230" y="186" width="140" height="70" rx="6" />
    <rect x="390" y="150" width="140" height="106" rx="6" />
    <rect x="550" y="114" width="140" height="142" rx="6" />
    <rect x="710" y="78" width="140" height="178" rx="6" />
  </g>
  <rect x="390" y="150" width="140" height="106" rx="6" fill="var(--c-primary)" fill-opacity="0.14" stroke="var(--c-primary)" stroke-width="2" />
  <g style="font:600 12px var(--font-mono); letter-spacing:0.1em" fill="var(--c-ink-muted)" text-anchor="middle">
    <text x="140" y="282">1</text>
    <text x="300" y="282">2</text>
    <text x="460" y="282">3</text>
    <text x="620" y="282">4</text>
    <text x="780" y="282">5</text>
  </g>
  <g style="font:500 13px var(--font-sans)" fill="var(--c-ink)" text-anchor="middle">
    <text x="140" y="214">completion</text>
    <text x="300" y="178">chat beside code</text>
    <text x="460" y="142">pair-programmer</text>
    <text x="620" y="106">agentic CLI</text>
    <text x="780" y="70">many at once</text>
  </g>
  <line x1="540" y1="40" x2="540" y2="262" stroke="var(--c-amber)" stroke-width="2.5" stroke-dasharray="6 5" />
  <text x="540" y="30" text-anchor="middle" style="font:600 12px var(--font-mono); letter-spacing:0.1em" fill="var(--c-amber)">THE MODEL STARTS CHOOSING</text>
</svg>
</div>

<div class="caption">蓝色层级是你在两周内花费的地方。M1 是这个层级；M2 和 M3 是第4层级。</div>

<!--
快走一遍，每层一句话。细节在 content.md 中，他们
不需要全部记住。

虚线是幻灯片。第1到第3层在自动化程度上有所不同。
第4层在谁决定上有所不同。这是种类的变化，不是程度的变化。

Lab01 为项目构建命名了三个里程碑：M1 结对编程，M2 智能体，M3 助手。这些是一个代码库经历的形状；这些层级是
任何工具所在的位置。M1 是第3层，M2 穿过虚线到第4层，M3
停留在那里并使其强化。说一次，这样两个阶梯不会竞争。
周一的房间在没有里程碑名称的情况下听了旧结尾，所以
像新的一样说出来。
-->

---
layout: default
---

<div class="label">第3层</div>

# 你在第3层 {.assert}

| | 在第3层 |
|---|---|
| 你移交 | 一个有界编辑任务 |
| 你选择 | 文件，以及任务在哪里结束 |
| 它自动化 | 匹配、修复、测试、提交 |
| 你检查 | 在任务之间 |

<div class="caption">在你划定的边界内的真实自动化。</div>

<!--
不要让第3层听起来原始。Aider 运行命令，在测试输出上循环，
并提交。它不做的是选择下一个任务。

如果有人问"所以 Aider 不是智能体的，"反驳：它有这些部分，我们
正要正确地检查这个说法。
-->

---
layout: default
---

<div class="label">权衡</div>

# 更高的层级花费更多来问更少 {.assert}

<div class="grid grid-cols-2 gap-6 mt-6">
  <div class="card">
    <div class="flex items-center gap-3 mb-2">
      <ph-trend-up-bold class="text-3xl text-blue-600 shrink-0" />
      <div class="font-semibold">单位工作的成本</div>
    </div>
    <div class="text-sm opacity-70">一次搜索、读取五个文件并三次运行测试的运行比一个有界编辑花费更多的注意力。</div>
  </div>
  <div class="card">
    <div class="flex items-center gap-3 mb-2">
      <ph-trend-down-bold class="text-3xl text-amber-600 shrink-0" />
      <div class="font-semibold">你自己做的决定</div>
    </div>
    <div class="text-sm opacity-70">每个完成结果的决策更少，而且你在每个结果上花费的注意力更少。</div>
  </div>
</div>

<!--
这种权衡就是整个梯度。其他一切都是细节。

不要量化它。你大声说出的任何倍数下个月都会是错误的，
有人会把它引用回来给你。
-->

---
layout: default
---

<div class="label">选择一个层级</div>

# 第3层适合你可以命名的改动 {.assert}

一个你可以指着的文件中的两行修复在第3层更便宜、更安全。
为它支付第4层的价格买不到任何东西。

**技能是选择层级，而你无法选择你看不到的东西。**

<!--
这是针对的反射：伸手去拿最自主的工具，因为
它是最自主的工具。

问他们什么时候宁愿有有界版本。你希望他们给出的答案
涉及到确切知道要改变什么，或者他们关心的爆炸半径。
-->

---
layout: section
---

# 什么让工具具有智能体特性

## 四个原语，以及一个你可以应用的测试

---
layout: default
---

<div class="label">定义</div>

# 四个原语，不是一种感觉 {.assert}

<div class="grid grid-cols-4 gap-4 mt-6">
  <div class="card">
    <ph-arrows-clockwise-bold class="text-2xl text-blue-600 mb-2" />
    <div class="font-semibold mb-1">循环</div>
    <div class="text-sm opacity-70">无需询问就采取另一步骤。</div>
  </div>
  <div class="card">
    <ph-wrench-bold class="text-2xl text-blue-600 mb-2" />
    <div class="font-semibold mb-1">工具</div>
    <div class="text-sm opacity-70">超出发出文本的动作。</div>
  </div>
  <div class="card">
    <ph-list-checks-bold class="text-2xl text-amber-600 mb-2" />
    <div class="font-semibold mb-1">规划</div>
    <div class="text-sm opacity-70">将目标分解为步骤。</div>
  </div>
  <div class="card">
    <ph-brain-bold class="text-2xl text-amber-600 mb-2" />
    <div class="font-semibold mb-1">记忆</div>
    <div class="text-sm opacity-70">有些东西在回合之后存活下来。</div>
  </div>
</div>

<div class="caption">"智能体"被用来表示"最近的"。这四个是可检查的。</div>

<!--
读它们一次。下一张幻灯片是工作发生的地方，所以不要停留。
-->

---
layout: default
---

<div class="label">测试</div>

# Aider 拥有全部四个 {.assert}

| 原语 | 它在 Aider 中已经在哪里 |
|---|---|
| 循环 | 失败的编辑变成下一个提示词；测试可以反馈 |
| 工具 | 读取、写入、提交、运行你配置的命令 |
| 规划 | 用散文起草的计划，停下来供人阅读 |
| 记忆 | 会话历史，加上你从未添加过的代码的代码库地图 |

<!--
这是让房间惊讶的幻灯片，所以让它发生。他们期望一个十字列。

任何配置过 lint 和测试命令的人都看过循环运行。问
是否有人看过；从学生口中说出来效果更好。
-->

---
layout: statement
---

区别在于*谁选择*。

<!--
在这里停顿。这是整节课转向的句子。

不是哪些原语存在。人类仍然持有哪些。
六个步骤的回调：1 组装，你和 Aider 按表格。2 回答，模型，文本输出。3 解析，Aider 按固定规则。4 应用，Aider，否则拒绝。5 修复，仅当4失败，它在2重新进入。6 提交，Aider，每次。六个中只有一个属于模型。在其他每一步，选择者是你或某人提前写下的规则。
-->

---
layout: default
---

<div class="label">移动</div>

# 一个选择改变了层级 {.assert}

在第3层，你选择文件、边界和是否运行计划。

将下一个动作的选择从工具链移到模型，
人们觉得关于智能体工具令人惊讶的一切都随之而来。

<div class="caption">一个改变。不是一个新的智能类别。</div>

<!--
值得清楚地说明：不需要新的模型能力来跨越这条线。
同一个模型，给它一个工具菜单和一个让它选择的循环，表现得像
一种不同类型的软件。
-->

---
layout: section
---

# 聊天休息

## 五分钟，关于你已经知道的工具

---
layout: default
---

<div class="label">聊天休息 · 5 分钟</div>

# 放弃一个选择 {.assert}

两周的 taskr Aider 使用。每次仍然有三个选择是你的：
它看到哪些文件，任务在哪里结束，计划是否运行。

将**正好一个**交给模型。

**然后说在你放心让它在你关心的代码库上运行之前你必须构建什么。** 移交和护栏哪个工作量更大？

<!--
**"好了，Zoom，我们休息了。"**

五分钟。巡视。

每个人命名一个能力；那一半很容易。将房间分开的那一半
是护栏，这节课后半部分是一个一小时的工作示例。如果答案中没有人到达那里，说出来并用它作为过渡。

**"好了，Zoom，我们回来了。"**
-->

---
layout: section
---

# PollEv

<div class="mt-5 font-mono text-2xl text-blue-600">https://PollEv.com/marcusdarden365</div>

<!--
出勤问题，大约一分钟，计入分数。完整文本在
`assets/poll-everywhere.md`。

切换到浏览器标签页，设置实时，大声朗读问题和所有四个选项
 aloud，给它六十秒，展示结果，回来。

这个 div 是刻意的：主题将 section 副标题大写，而大写的
URL 路径是不同的路径。不要把它"修复"成 h2。
-->

---
layout: section
---

# 同样的请求，上一层级

## 现场，在这台机器上

---
layout: default
---

<div class="label">请求</div>

# 你知道这在第3层花了什么 {.assert}

```text
Add a --priority flag to taskr's add command.
```

你添加了三个文件，因为你没有添加的文件不在上下文中。
你框定了任务。你阅读了 diff。你自己发现了缺失的写入。

<!--
与第三课相同的请求，刻意为之。一个任务在两个层级是唯一诚实的
可用比较，在八十分钟内。

没有人需要 taskr 回顾。他们已经在其中生活了两周。
-->

---
layout: default
---

<div class="label">现场演示</div>

# 没有人需要输入 /add {.assert}

<div class="grid grid-cols-3 gap-5 mt-6">
  <div class="card">
    <div class="font-semibold mb-1">它搜索</div>
    <div class="text-sm opacity-70">找到参数解析的位置，无需被告知。</div>
  </div>
  <div class="card">
    <div class="font-semibold mb-1">它读取</div>
    <div class="text-sm opacity-70">跟踪代码到实际触及的文件。</div>
  </div>
  <div class="card">
    <div class="font-semibold mb-1">它运行</div>
    <div class="text-sm opacity-70">执行测试并对它们所说的内容做出反应。</div>
  </div>
</div>

<div class="caption">上下文组装变成了程序通过行动做的事情。</div>

<!--
在这里切换到终端并驱动它。`assets/demo-runbook.md` 中的运行手册；
在这个后面没有录像，所以如果网络断开，请参阅 `assets/demo-fallback.md`。

叙述决策，而不是打字。每次它选择一个文件，大声说出来
没有人告诉它。

在交付前排练。它今天实际做什么是关于今天的
构建的事实，而不是这些幻灯片可以承诺的。
-->

---
layout: default
---

<div class="label">追踪记录</div>

# 每个结果选择下一步 {.assert}

```text
search "add_argument"   → three hits, one in cli.py
read cli.py             → finds the parser
read store.py           → finds the save path
edit cli.py, task.py, store.py
run tests               → 1 failed
edit store.py
run tests               → passed
```

那个顺序没有任何人提前写下来。

<!--
将其标记为说明性形状，而不是他们刚刚观看的运行记录。现场运行是证据；这是模式。

悬挂的问题是下一节回答的：谁阻止一个运行错误的
运行？
-->

---
layout: default
---

<div class="label">诚实</div>

# 今天的运行是关于今天的事实 {.assert}

你刚刚看到的是这个模型和这个构建在这个代码库上，
今天下午所做的事情。

**运行糟糕比运行顺利更有用。不要把它救得太快。**

<!--
无论演示是好是坏都要这样说，并在之后而不是
之前说，这样它才不会读作预防性的借口。

速度不是教训。一个工具更快地到达同一个地方是一种
便利；一个工具决定去哪里是一个不同的对象。
-->

---
layout: section
---

# 工具链仍然决定什么

## 演示隐藏的部分

---
layout: default
---

<div class="label">菜单</div>

# 模型从一个它没有编写的菜单中选择 {.assert}

<div class="mt-2">
<svg viewBox="0 0 900 290" style="width:100%;max-height:290px" role="img"
     aria-label="模型坐在工具链内部。工具链暴露一组固定的工具，应用权限，并持有停止条件。模型只从暴露的工具中选择。">
  <rect x="40" y="22" width="820" height="242" rx="12" fill="var(--c-bg-1)" stroke="var(--c-rule-strong)" stroke-width="2" />
  <text x="62" y="48" style="font:600 11px var(--font-mono); letter-spacing:0.14em" fill="var(--c-ink-muted)">THE HARNESS, WHICH SOMEBODY WROTE</text>
  <rect x="330" y="64" width="240" height="56" rx="8" fill="var(--c-highlight)" stroke="var(--c-amber)" stroke-width="2.5" />
  <text x="450" y="98" text-anchor="middle" style="font:600 16px var(--font-mono)" fill="var(--c-amber)">the model picks</text>
  <g stroke="var(--c-rule-strong)" fill="var(--c-bg-0)">
    <rect x="70" y="168" width="170" height="44" rx="6" />
    <rect x="260" y="168" width="170" height="44" rx="6" />
    <rect x="450" y="168" width="170" height="44" rx="6" />
    <rect x="640" y="168" width="170" height="44" rx="6" />
  </g>
  <g style="font:500 14px var(--font-mono)" fill="var(--c-ink)" text-anchor="middle">
    <text x="155" y="195">search</text>
    <text x="345" y="195">read</text>
    <text x="535" y="195">edit</text>
    <text x="725" y="195">run command</text>
  </g>
  <g stroke="var(--c-primary)" stroke-width="2" fill="none" stroke-dasharray="4 4">
    <path d="M400,120 L160,164" />
    <path d="M430,120 L348,164" />
    <path d="M470,120 L532,164" />
    <path d="M500,120 L718,164" />
  </g>
  <text x="450" y="242" text-anchor="middle" style="font:500 13px var(--font-sans)" fill="var(--c-ink-soft)">permissions gate these  ·  a stop condition ends the loop</text>
</svg>
</div>

<div class="caption">有人决定存在搜索工具，而数据库客户端不存在。</div>

<!--
今天最重要的句子：模型只能从工具链暴露的内容中选择，并且只能在工具链执行的条件下。

在 Analyze 中他们编写这个盒子。在这里说；这是阶段之前的第一个具体形状。
-->

---
layout: default
---

<div class="label">安全性</div>

# 权限才是真正的安全层 {.assert}

你在实验课中被 handed 了这个形状。在人类批准之前，
磁盘上没有任何改变，无论模型说什么都成立，因为
程序强制执行它，而不是提示词。

**同一个问题扩大到更大规模：当模型可以运行命令时，哪些命令可以
无需询问就运行？**

<!--
绑定到他们被 handed 的规格说明：F6，预览、批准和应用。Lab01 的结束表格已经把它作为他们将要写的规则放在他们面前。

不要在幻灯片上回答这个问题。它是 Analyze 的，也是 Lab03 的。
-->

---
layout: statement
---

文字询问。代码和权限 *强制执行*。

<!--
他们在 Lab01 中遇到过这句话。重复就是意义：这是
整个课程不断回归的设计规则。

具体地说：提示词要求模型停留在选定的文件中，而
验证才是真正把它留在那里的东西。
-->

---
layout: default
---

<div class="label">停止</div>

# 循环需要一个停止的理由 {.assert}

一个它可以识别为已满足的目标。一个它可以花费的预算。一个中断的人类。

在第3层，任务边界是停止条件，你每次都手工
划定它。

<!--
问没有它会发生什么。你正在寻找"它永远运行"然后，
更好的是"它永远造成损害"。

这是一个设计问题，不是一个模型问题，这是整个下半场的
主线。
-->

---
layout: default
---

<div class="label">你的项目构建</div>

# 第3层是重点，不是限制 {.assert}

精确匹配。任何写入之前的一次批准。每个接受的编辑集一个提交。受保护的撤销。

这些使得失败后的状态成为你可以用一句话说出来并在测试中断言的东西。

<div class="caption">构建那个，你就构建了循环在它下面需要的东西。</div>

<!--
在这样的演示之后房间里的焦虑是他们在构建
去年的工具。直接解决它。

跳过这些机制并在上面添加一个循环只是意味着失败得更快。
-->

---
layout: default
---

<div class="label">习惯</div>

# 追踪记录就是证据 {.assert}

当运行产生错误结果时，输出不会告诉你为什么。

哪个步骤选择了错误的文件？它误读了哪个结果？它 improvisation 了哪个缺失的工具？

**"模型不擅长这个"是人们得到的答案，而且很少是证据支持的答案。**

<!--
与实验课相同的诊断纪律，应用于更长的链。在实验课中，三个杠杆是上下文、模型和提示词；在这里链更长，证据是运行本身。
-->

---
layout: default
---

<div class="label">本周</div>

# 项目构建三周后截止 {.assert}

| 时间 | 事项 |
|---|---|
| 9月17日周四 | L06 结束 Apply |
| 9月21日周一 / 9月22日周二 | Lab02，支持的构建时间 |
| 9月24日周四，晚上 | Hackathon 1，你自己的项目 |
| 10月6日周二，晚上11:59 | 项目构建截止 |

<!--
读一次然后继续。这里的每件事也在他们的代码库和日程表上。

为黑客马拉松带来一个可运行的增量；提示词在房间里揭示。
-->

---
layout: default
---

<div class="label">带走这些</div>

# 三个问题

1. 你默认伸手去拿的工具在梯度上的什么位置，是你选择的还是继承的？
2. 在你让模型无需询问就运行命令之前必须发生什么？
3. 你需要在一段追踪记录中看到什么才会相信一个运行失败的原因不是模型？

<!--
在问题期间保持这张幻灯片。第二个问题会在 Analyze 中回来，第三个问题使他们这周成为更好的工程师。
-->

---
layout: statement
---

周四，循环不再*是别人的*。

<!--
桥梁。L06 结束 Apply 并移交到 Analyze，他们在那里编写
工具链幻灯片上的盒子而不是看着它工作。
-->

---
layout: end
---