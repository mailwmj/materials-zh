---
theme: ../theme
title: "L03：三大要素：上下文、模型、提示词"
info: |
  EECS 498 AASE —— 第 03 讲
  Applied Agentic Software Engineering，UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 三大要素

## 第 03 讲 · 2026 年 9 月 8 日

<!--
L02 = 底层原理。L03 = 之上的框架。
这是他们将在整个职业生涯中使用的讲座。
-->

---
layout: default
---

<div class="label">第 2 周 · 环境检查</div>

# `aider-practice` 状态

<div class="mt-6 space-y-2">

- 模型服务运行了吗？
- Aider 能连接到它吗？
- 环境障碍：权重拉不下来、服务器 Aider 连不上、机器太小？

</div>

<div class="caption mt-6">本周随时答疑，我们任何人都可以帮你做环境设置。<strong>9 月 11 日周五</strong>是环境检查和全部十六节课的截止日期。</div>

<!--
三次举手示意。这三种故障都很常见，没有一种是学生的错。
收集名字去答疑时间，而不是在教室里调试。
-->

---
layout: section
---

# 三大要素

## 上下文 · 模型 · 提示词

<!--
L02 给他们讲了分词、上下文窗口、幻觉、能力与知识。那是底层原理。
这是上面的框架。
每个会话的三个选择。一个错了，另外两个再努力也救不了你。
-->

---
layout: default
---

<div class="label">三个杠杆</div>

# 每一次 AI 编程会话

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-files-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">上下文</div>
    <div class="text-sm opacity-70 flex-1">模型现在能看到什么？</div>
  </div>
  <div class="card">
    <ph-cpu-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">模型</div>
    <div class="text-sm opacity-70 flex-1">用哪个 LLM？能力 + 知识。</div>
  </div>
  <div class="card">
    <ph-chat-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">提示词</div>
    <div class="text-sm opacity-70 flex-1">意图表达得有多清楚？</div>
  </div>
</div>

<!--
在这里逐一定义三个要素，在靶心图出现之前。
上下文：文件、历史、代码库地图、系统提示词。有限的。
模型：一行配置，不是供应商忠诚度。
提示词：由输出的一致性评判，而不是措辞的巧妙。
-->

---
layout: default
---

<div class="label">靶心</div>

# 三者的重叠区域

<div style="display:grid; grid-template-columns:7fr 5fr; gap:2.5rem; align-items:center; margin-top:0.5rem">
<div>
<svg viewBox="20 15 410 390" style="width:100%; max-height:340px" role="img"
     aria-label="三个重叠的圆分别标记为 Context、Model 和 Prompt，三者共同重叠的区域高亮为靶心。">
  <g fill="var(--c-primary)" fill-opacity="0.07" stroke="var(--c-primary)" stroke-width="1.75">
    <circle cx="155" cy="145" r="110" />
    <circle cx="295" cy="145" r="110" />
    <circle cx="225" cy="266.24" r="110" />
  </g>
  <path d="M 186.52 163.19
           A 110 110 0 0 1 263.48 163.19
           A 110 110 0 0 1 225 229.85
           A 110 110 0 0 1 186.52 163.19 Z"
        fill="var(--c-highlight)" stroke="var(--c-amber)" stroke-width="2.5" />
  <text x="225" y="189" style="font: 600 11px var(--font-mono); letter-spacing:0.04em"
        fill="var(--c-amber)" text-anchor="middle">BULLSEYE</text>
  <g style="font: 600 17px var(--font-mono)" fill="var(--c-primary)" text-anchor="middle">
    <text x="105" y="138">Context</text>
    <text x="345" y="138">Model</text>
    <text x="225" y="322">Prompt</text>
  </g>
  <g style="font: 600 13px var(--font-mono)" fill="var(--c-ink-muted)" text-anchor="middle">
    <circle cx="280" cy="218" r="12" fill="var(--c-bg-0)" stroke="var(--c-rule-strong)" />
    <text x="280" y="222.5">1</text>
    <circle cx="170" cy="218" r="12" fill="var(--c-bg-0)" stroke="var(--c-rule-strong)" />
    <text x="170" y="222.5">2</text>
    <circle cx="225" cy="108" r="12" fill="var(--c-bg-0)" stroke="var(--c-rule-strong)" />
    <text x="225" y="112.5">3</text>
  </g>
</svg>
</div>
<div style="display:flex; flex-direction:column; gap:1.3rem; font-size:0.85em">
<div><strong>1 · 模型 + 提示词，无上下文。</strong>对一个它从未读过的文件进行干净、自信的编辑。<em>今天的演示。</em></div>
<div><strong>2 · 上下文 + 提示词，无模型。</strong>它需要的一切都有了，但中途还是失去了主线。</div>
<div><strong>3 · 上下文 + 模型，提示词模糊。</strong>正确的文件，强大的模型，它构建了一些合理但你从未要求的东西。</div>
</div>
</div>

<!--
先在空气中画出来，然后让幻灯片确认。三个圆，交点为靶心。
按顺序讲解三个接近的区域，在你说之前先问他们认为哪一个最常见。
区域 1 是 1:00 的演示，所以现在就命名它，他们会在发生时认出它。
区域 2 和 3 是诚实的：弱模型和模糊提示词都会产生看起来像进展的输出。
-->

---
layout: statement
---

三中取二*不够*。

<!--
他们刚读的图表的点睛之笔。 Flatly 说出来，然后继续。
在其他两个杠杆上的努力无法弥补你漏掉的那个。
-->

---
layout: default
---

<div class="label">当 AI 出错时，依次询问</div>

# 诊断

1. **上下文** —— 文件在窗口中吗？
2. **模型** —— 它足够强大吗？足够新吗？
3. **提示词** —— 意图清楚吗？

<div class="caption mt-6">大多数学生责怪模型。这通常不是原因。</div>

<!--
这节课的关键点。当有东西回来错误时，在你碰任何东西之前按顺序问这三个。
大多数学生责怪模型。从统计上看，这是最不常见的原因。
通常是上下文。
告诉他们现在在 Lab01 下周会用到这个。
-->

---
layout: section
---

# 上下文

## 你能最直接控制的杠杆

<!--
他们能最直接控制的杠杆，也是造成最多麻烦的那个。
接下来的二十分钟主要在这里。
几周后他们会自己实现这一点。
-->

---
layout: default
---

<div class="label">四个来源</div>

# 你的上下文里有什么

<v-clicks>

- **显式添加的文件** —— 你 `/add` 的内容
- **代码库地图** —— 自动生成的结构摘要
- **聊天历史** —— 积累到 `/clear`
- **系统提示词** —— Aider 给模型的指令

</v-clicks>

<!--
添加的文件是模型唯一能编辑的文件。说两遍。
代码库地图是自动生成的摘要，不是添加文件的替代品。
聊天历史积累到 /clear。这是第二个陷阱，两张幻灯片之后。
系统提示词是 Aider 自己的，默认对现在来说没问题。
-->

---
layout: statement
---

全部添加进来比添加正确的内容*更差*。

<!--
错误的直觉。aider **/*.py 看起来有帮助，其实没有。
-->

---
layout: default
---

<div class="label">技能</div>

# 外科手术式上下文

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-pencil-simple-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">更改</div>
    <div class="text-sm opacity-70 flex-1">这个任务会编辑哪些文件？只有这些文件能被编辑。</div>
  </div>
  <div class="card">
    <ph-eye-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">读取</div>
    <div class="text-sm opacity-70 flex-1">它必须先理解哪些文件？也添加这些。</div>
  </div>
  <div class="card">
    <ph-bookmark-simple-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">参考</div>
    <div class="text-sm opacity-70 flex-1">只读背景。通常代码库地图已经覆盖了。</div>
  </div>
</div>

<div class="caption mt-6">只添加这些，`/drop` 其他的，经常用 `/tokens`。上下文是设计，不是设置。</div>

<!--
三个问题，在每个任务之前问，而不是在会话开始时只问一次。
只有第一张卡片能被编辑。第二张是模型必须读懂的，才能把第一张做对。
参考通常是代码库地图已经足够的地方。
-->

---
layout: default
---

<div class="label">阅读预算</div>

# 你的 tokens 去哪了

```text
/tokens

  repo map              1,800
  chat history          3,100
  main.py               6,000
  arg_parse.py          1,300
  -----------------------------
  total                12,200   of 32,768
```

<div class="caption mt-4">在任何大操作之前和之后检查它。这是对窗口唯一诚实的视图。</div>

<!--
按来源细分，这使它成为诊断而不是计数器。
当代码库地图或历史记录是最大的一行时，那就是发现。
-->

---
layout: default
---

<div class="label">两个陷阱</div>

# 学生卡住的地方

<div class="grid grid-cols-2 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-file-x-bold class="text-3xl text-rose-600 mb-3" />
    <div class="font-semibold mb-1">缺失上下文</div>
    <div class="text-sm opacity-70 flex-1">arg_parse.py 没有被 /add → 模型只编辑 main.py → 结果损坏。</div>
  </div>
  <div class="card">
    <ph-clock-counter-clockwise-bold class="text-3xl text-rose-600 mb-3" />
    <div class="font-semibold mb-1">聊天历史渗透</div>
    <div class="text-sm opacity-70 flex-1">任务 B 继承了任务 A 的假设。修复方法：/clear。</div>
  </div>
</div>

<!--
缺失上下文是这门课程中最常见的故障。模型编辑的是在上下文中的内容，所以一个从未添加的文件无法被修复。Aider 会提示你添加它，接受这个提示就能救你。拒绝它，或者使用不会询问的工具，只有一个文件被更新：一个看起来完成了的部分更改。
历史渗透更微妙。在任务 A 上花十分钟，开始任务 B，那些消息还在那里带着假设。 /clear 重置对话并保留文件。
这为休息做铺垫。不要在这里回答它。
-->

---
layout: statement
---

<div class="label">聊天休息 · 5 分钟</div>

十个文件，一个函数要改，*三个*调用者。你如何决定添加什么？讨论你的策略。

<!--
0:35。固定的休息时间。提示词在 overview.md 的 chat_break 字段中。
没有唯一正确的答案，这就是重点：正在养成的习惯是把上下文当作设计决策，而不是设置。
要策略，不要文件列表。两三个回答，然后继续。
-->

---
layout: section
---

# 模型

## 将能力与任务匹配

<!--
上下文是你喂给模型的东西。这是同一决策的另一半。
这里有十分钟，然后是提示词。
-->

---
layout: default
---

<div class="label">四个维度</div>

# 选择

<v-clicks>

- **推理能力** —— L02 中的能力
- **速度** —— 通常与能力相关
- **成本** —— 供应商那里的美元，自己机器上的 RAM 和秒数
- **上下文窗口大小** —— 但更大 ≠ 更好（L02！）

</v-clicks>

<!--
速度和大小与推理能力相关。更聪明通常意味着更大，而在自己的硬件上这意味着更慢、更吃 RAM，而不是更贵（以美元计）。当账单消失时，权衡并没有消失。
有些模型支持 128K，有些支持 1M，而 L02 已经告诉他们更大并不自动更好。大海捞针是真实的。
-->

---
layout: default
---

<div class="label">工程问题</div>

# 这份工作的合适模型

<div class="grid grid-cols-2 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-wrench-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">单行 bug 修复</div>
    <div class="text-sm opacity-70 flex-1">一个便宜、快速的模型就能过线。在这里花费能力什么都买不到。</div>
  </div>
  <div class="card">
    <ph-tree-structure-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">三个新文件，相互依赖</div>
    <div class="text-sm opacity-70 flex-1">你拥有的最强大的模型。做错的代价超过这次调用的成本。</div>
  </div>
</div>

<div class="caption mt-6">基准测试（`aider.chat/docs/leaderboards/`）可以代理你的任务。它们不能替代它。</div>

<!--
按任务，不是按学期。这就是整张幻灯片。
排行榜值得知道，它是你任务的代理而不是替代品。
-->

---
layout: default
---

<div class="label">应用经验法则</div>

# 从会失败的模型开始

<div class="mt-6 space-y-3">

- **从 4b 开始。** 它在你能看到的地方失败，失败就是课程
- **当它停止教你，只是浪费你一个下午时，切换到 9b**

</div>

<div class="mt-10 mb-2 text-xl">一个能掩盖你错误的模型 <em>隐藏了你需要看到的错误。</em></div>

<div class="caption mt-6">用 `/model ollama_chat/qwen3.5:9b` 切换。文件和历史记录保留；新模型重新读取所有内容。</div>

<!--
大声说这是与工作场合相反的。
对大多数人来说，交叉点在第 6 节课，第一次真正的重构。
-->

---
layout: section
---

# 提示词

## 精确表达意图

<!--
最后一个杠杆，也是最短的部分，因为 L04 会专门讲它。
-->

---
layout: default
---

<div class="label">提示词光谱</div>

# 具体化程度

<div style="display:flex; justify-content:center; margin-top:0.5rem">
<svg viewBox="0 0 900 290" style="width:100%; max-height:300px" role="img"
     aria-label="一个三段光谱：左边是 under-specified（意图不足），中间是 just enough（恰到好处），右边是 over-specified（过度指定）。一个标有 more intent 的箭头从中间指向左边。">
  <g style="font: 500 11px var(--font-mono); letter-spacing:0.1em" fill="var(--c-ink-muted)" text-anchor="middle">
    <text x="160" y="42">UNDER-SPECIFIED</text>
    <text x="450" y="42">JUST ENOUGH</text>
    <text x="740" y="42">OVER-SPECIFIED</text>
  </g>
  <rect x="20" y="58" width="280" height="58" rx="8" fill="var(--c-primary)" fill-opacity="0.07" stroke="var(--c-primary)" stroke-width="1.5" />
  <rect x="310" y="58" width="280" height="58" rx="8" fill="var(--c-highlight)" stroke="var(--c-amber)" stroke-width="2.5" />
  <rect x="600" y="58" width="280" height="58" rx="8" fill="var(--c-primary)" fill-opacity="0.07" stroke="var(--c-primary)" stroke-width="1.5" />
  <g style="font: 600 17px var(--font-mono)" text-anchor="middle">
    <text x="160" y="93" fill="var(--c-primary)">OUTCOME</text>
    <text x="450" y="93" fill="var(--c-amber)">INTERFACE</text>
    <text x="740" y="93" fill="var(--c-primary)">IMPLEMENTATION</text>
  </g>
  <g style="font: 400 12px var(--font-mono)" fill="var(--c-ink-soft)" text-anchor="middle">
    <text x="160" y="146">"another way to output"</text>
    <text x="450" y="146">"format_as_html() -&gt; str"</text>
    <text x="740" y="146">"iterate the speakers dict"</text>
  </g>
  <g style="font: 400 11px var(--font-sans)" fill="var(--c-ink-muted)" text-anchor="middle">
    <text x="160" y="167">the model guesses</text>
    <text x="450" y="167">you set the seam, it writes the body</text>
    <text x="740" y="167">you did the work, it adds nothing</text>
  </g>
  <g stroke="var(--c-amber)" stroke-width="2.5" fill="var(--c-amber)">
    <line x1="520" y1="212" x2="262" y2="212" />
    <polygon points="250,212 264,205 264,219" stroke="none" />
  </g>
  <text x="385" y="240" style="font: 600 12.5px var(--font-mono)" fill="var(--c-amber)" text-anchor="middle">more intent, aim further left</text>
</svg>
</div>

<div class="caption mt-4"><strong>意图</strong>是你希望以那种方式完成的原因。它不是这条线上的一个点，它是移动目标的东西。</div>

<!--
提示词有两个部分。指令是你想要做什么。意图是你希望以那种方式完成的原因。
把意图留出来，模型就从训练数据先验中填充，一个你永远看不到的猜测。
这张幻灯片依赖的句子：规格关闭决策，意图引导你留下的那些。
说这句话，然后再解释箭头。
然后预先阻止误读：箭头不是告诉他们要更模糊，它告诉他们当你说明原因时，目标在那端变得更宽。
意图扩展了左边缘，对右边缘没有任何作用，因为它不能把你已经自己写好代码的情况救回来。
瞄准接口：命名文件、函数、签名、调用站点，然后停止。你拥有接缝，模型写主体。
必需的，不是可选的：箭头有长度，它的长度是模型能力。在 4B 上它很短。一段为什么可能把一句什么埋掉，这是 L02 的大海捞针瞄准他们自己的提示词。这学期把意图保持在一个从句内。
-->

---
layout: two-col
---

## 模糊

```text
Add another way
to output the data.
```

<div class="text-sm opacity-70 mt-4">模型猜测。也许是文本。也许是 print()。可能跳过 CLI 标志。不可预测。</div>

::right::

## 具体

```text
CREATE output_format.py:
  format_as_html(...) -> str
UPDATE main.py: ADD
  html to --output-format
```

<div class="text-sm opacity-70 mt-4">可预测。可重复。一次成功。</div>

<!--
模糊的那个设定了一个方向，把每一个具体决定都留给模型。它不是错的，它是不可预测的，而不可预测就是问题。
具体的那个写起来更长，用起来更快。可预测性是目标，不是简洁。
L04 将其形式化为 IDK 提示词。
-->

---
layout: statement
---

What > How。

<!--
指定结果，而不是实现。但 "什么" 必须足够具体才能成功。
在他们有了光谱之后变得更清晰：具体化到接口层面就停止。"What > How" 不是"要模糊，AI 会搞定的。"
-->

---
layout: default
---

<div class="label">映射回 L02</div>

# 每个杠杆都有一个底层原理

<div class="mt-6 space-y-3">

- **上下文** → 上下文窗口经济。更大不是免费的。
- **模型** → 能力 vs 知识维度。
- **提示词** → 缩小采样分布。

</div>

<div class="caption mt-6">三大要素不是随意的。它是底层原理之上的框架。</div>

<!--
演示前的回顾。每个杠杆是上一讲底层原理的一个，带有一个把手。
落地这一点：他们 ever 读到的每一条提示建议都归结为这三个之一。"添加文件"利用了窗口。"要具体"利用了先验。
-->

---
layout: section
---

# 现场演示

## 上下文作为工程决策

<!--
过渡：我们已经命名了框架，并把每个杠杆拆开了。
现在看它如何决定一个实际的编辑。
-->

---
layout: default
---

<div class="label">5 文件转录分析代码库</div>

# 演示流程

<v-clicks>

1. 只用 `src/main.py` 运行 Aider。提示词：`--output-format` 标志。
2. 它提议重写 `arg_parse.py`，一个它从未读过的文件。拒绝。
3. 运行程序。干净的 diff，`AttributeError`。然后 `/undo`。
4. `/add src/arg_parse.py`。相同的提示词。连贯了，而且能运行。
5. `/tokens`：聊天历史是最大的一行，比全部五个文件都大。
6. `/model ollama_chat/qwen3.5:4b`，`/clear`，`/drop`，`/add src/main.py`。相同的提示词。

</v-clicks>

<div class="caption mt-4">代码库是你的：`materials/demos/L03-transcript-analytics`。回家再运行一次演示。</div>

<!--
五个文件。arg_parse.py 拥有每一个命令行标志，main.py 拥有计数和打印。在开始之前大声说出来，因为整个演示都取决于它。
第 2 步是关键：它提议重写一个它从未读过的文件，屏幕上是一个 diff，所以减号行是它做错了什么的逐行清单。指出 -default=DEFAULT_TOP_N 对比 +default=10。
那里有两个提示要拒绝，不是一个。第二个默认为 Yes，接受默认会把虚构的文件写到磁盘并失去演示。
第 4 步是一句话的课程：相同的模型，相同的提示词，多一个文件。
第 6 步是有意地 drop 和 re-add。没有它，全部五个文件仍在上下文中，4B 无法重现故障。同样的错误，更快：故障从来不是模型。
如果一个步骤表现异常，说出那句话然后继续。模型只能修改在上下文中的文件。
-->

---
layout: default
---

<div class="label">带回家</div>

# 三个问题

1. 50 文件项目，跨 API 层更改认证 —— 你如何 /add？
2. 为什么添加**所有**文件会让结果*更差*？
3. 会话中切换模型 —— 什么改变了？

<div class="caption mt-6">大多数"AI 错了"的故障都归结为问题 1 或 2。</div>

<!--
不是修辞。让他们在 Lab01 之前实际回答这些问题。
问题 2 是大多数人都答错的那个：更差，不是更贵。
-->

---
layout: default
---

<div class="label">三大要素</div>

# 要点总结

<div class="mt-6 space-y-2">

- **上下文、模型、提示词。** 三个杠杆，你ever运行的每个会话
- **上下文通常是问题所在。** 大多数"AI 错了"是缺失上下文
- **模型选择是技术性的**，不是偏好。将能力与任务匹配
- **提示词说 What，不是 How。** 命名文件、命名函数、说明结果

</div>

<div class="caption mt-6">三中取二不够。在重写提示词之前先问这个问题。</div>

<!--
具体回报：/add 和 /drop 是他们在第 3 和第 4 周实现的七个功能之一。
将文件拉入上下文是另一个，多轮历史记录的修剪是第三个。
这周他们正在 drill 的行为正是他们不得不写的行为。
-->

---
layout: default
---

<div class="label">接下来</div>

# Lab01 + 构建项目

<v-clicks>

- **9 月 11 日周五** —— 环境检查，全部十六节课截止
- **9 月 14 日周一 / 9 月 15 日周二** —— Lab01：转折实验课
- **构建项目** —— 七个功能，第 3 和第 4 周。Lab01 之后开始，规格随它一起评分
- **L04 9 月 10 日周四** —— 规格驱动编程，两天后

</v-clicks>

<div class="caption mt-6">在 Lab01 出问题时使用今天的诊断。<em>上下文、模型，还是提示词？</em></div>

<!--
周五是关卡和全部十六节课，两者。
他们还没有开始构建。构建在 Lab01 之后开始，规格随它一起评分。
周四是规格驱动编程：计划等于提示词，使"要具体"可以在规模上复现的产物，以及构建之前的最后一门技术。
-->

---
layout: end
---
