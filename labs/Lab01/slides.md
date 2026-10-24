---
theme: ../../lectures/theme
title: "Lab01: Aider 的工作原理"
info: |
  EECS 498 AASE, Lab01
  Design and build a pair-programmer, Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# Aider 的工作原理

## Lab01 · 设计你的结对程序员 · 2026 年 9 月 14–15 日



<!--
两部分。第一部分拆解他们从 Lab00 开始一直在使用的工具，
深度足以让他们能在任何框架中识别出相同的设计决策。
第二部分让他们动手构建。

深度剖析是现在的重点，不是作业预告的热身，所以不要压缩它来赶时间。
办公时间处理剩下的内容。
-->

---
layout: default
---

<div class="label">Lab01</div>

# 日程

1. Aider 深度剖析
2. Hackathon 1
3. 下一个作业
4. 办公时间

<!--
在教室安静下来时展示这张幻灯片并保持展示。议程是第一块的内容，
不是前面额外的片段。

大声说出后半部分是真正的办公时间：他们工作，你巡视，今天没有人
需要交任何东西。整个学期被告知实验课以检查结束的学生不会相信，
除非你亲口说出来。幻灯片不再这么说，这需要你亲口说明。

不要预览构建 beyond 这个词本身。揭示仍然由你在第三部分完成。
-->

---
layout: section
---

# 框架

## 你和模型之间的程序

<!--
根据讲师指示在 2026-09-13 新增的章节幻灯片：框架是实验课的第一部分，
应该作为一个整体呈现。它下面有八张幻灯片。
他们已经使用过的四个框架，然后是一个框架是什么以及模型永远不会接触
什么。Aider 被画成一个，然后它的回复是什么样子：编辑块，它出现的两种
形状，以及从文本中恢复一个动作的一般问题。然后 Aider 缺少什么，以及
他们熟悉的工具拥有什么。

示例在定义之前，所以这个词落在他们熟悉的工具上，然后才获得抽象形状。
回复幻灯片从中间移到了这里，因为回复是框架的"答案"牌，之前 sits 在一个
关于缓存的章节中。

这个章节存在是因为十六次练习课给了他们肌肉记忆，但 none of the model，
而"框架"是他们需要的词：它是他们在 Stage 1 构建的东西，在 Analyze 中
扩展的东西。
-->

---
layout: default
---

<div class="label">你已经知道的框架</div>

# 你已经使用过四个框架 {.assert}

| | 放在模型前面 | 对回复做什么 |
|---|---|---|
| ChatGPT | 你的消息，以及它记录的笔记 | 展示给你 |
| Cursor | 你指向的文件，以及一个索引 | 你接受的 diff |
| Claude Code | 它自己读取的文件，以及你保存的笔记 | 如果允许则编辑或运行命令 |
| Aider | 你添加的文件，以及一张地图 | 写入，然后提交 |

**一种模型在四个工具后面。框架是不同的部分。**

<!--
根据讲师指示打开章节，所以这个词落在他们熟悉的工具上，然后才获得定义。
他们中的三个在课程之前使用过，从未称之为框架。读一行：程序放在模型
前面的是什么，以及对返回的文本做了什么。每一行都是同样的两个工作，
下一张幻灯片命名它们。

ChatGPT 的记忆是最清晰的例子。它感觉像模型在记住你。是框架在记笔记
并粘贴到提示中，两张幻灯片后解释原因：模型在会话之间什么都不保留，
所以任何持久化的东西都是框架重新发送的。Claude Code 的笔记是同样的
技巧从另一边：你保存的文件在每次会话开始时被读取。

第三列读作批准层。Cursor 等待接受。Claude Code 询问，或者不询问，
取决于某人选择的权限设置。Aider 不等待：它写入并提交，/undo 是恢复。
他们的构建必须在写入前询问，这是 packet 的规则，这一列是同样的决策
以四种不同的方式做出的地方。

不要更深入地进入 Claude Code  beyond 它的行。它决定读取什么和运行什么
是明天的讲座，带有实时演示。
-->

---
layout: default
---

<div class="label">什么是框架</div>

# 框架是模型周围的一切 {.assert}

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-stack-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">组装</div>
    <div class="text-sm opacity-70 flex-1">挑选模型将看到的每一个词。</div>
  </div>
  <div class="card">
    <ph-chat-text-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">回答</div>
    <div class="text-sm opacity-70 flex-1">模型写一条回复。文本，仅此而已。</div>
  </div>
  <div class="card">
    <ph-pencil-simple-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">应用</div>
    <div class="text-sm opacity-70 flex-1">决定回复可以改变什么，然后进行改变。</div>
  </div>
</div>

<div class="caption mt-6">蓝色是框架。琥珀色是模型。</div>

**现在拆解一个，稍后构建一个。**

<!--
根据 2026-09-13 讲师指示重新设计；它替换的两列表格读起来令人困惑。
三张卡片按照一个回合发生的顺序排列，中间那张是模型的全部贡献。

最后一张幻灯片的两列标题是两张蓝色卡片。组装是放在模型前面的东西。
应用是对回复做的事情。大声说出这三个词： taught half 的最后一张幻灯片
又增加了三个，并询问六个中有哪些属于模型。只有一个，答案。

他们机器上的每一个效果都是蓝色卡片对琥珀色文本采取的行动。
这是整个实验课依赖的句子。

如果有人问模型、端点或配置：MODEL-POLICY.md，这不是今天的主题。
推荐 qwen3.5:9b，允许 4B，任何兼容的端点。
-->

---
layout: default
---

<div class="label">什么是框架</div>

# 模型永远不会接触什么

- 你的磁盘
- 你的 shell
- 你上一次的会话

**文本输入，文本输出。框架处理其余部分。**

<!--
三个否定，每一个都是某人做出的设计决策，而不是模型的限制。

第三个是令人惊讶的那个：会话之间没有记忆。连续性是框架重新发送历史，
这就是为什么历史在后面的幻灯片中消耗他们的 token 预算，以及为什么
ChatGPT 似乎记得他们，两张幻灯片前，是它的框架在记笔记。

4B 和 9B 被要求不同的回复形状，整个文件 against diffs。这就是为什么
同学的会话看起来和他们的不同，以及为什么他们稍后看到的 trace 可能与
他们在实践中运行的不匹配。
值得在这里说，不值得一张幻灯片。
-->

---
layout: default
---

<div class="label">Aider 就是一个</div>

# Aider、模型和代码仓库

<div class="mt-2">
<svg viewBox="0 0 900 330" style="width:100%;max-height:330px" role="img"
     aria-label="Aider 位于你的代码仓库和模型之间。Aider 读取文件，将提示文本发送到模型，接收回复文本，然后写入并提交。没有线条将模型连接到代码仓库。">
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

<div class="caption">模型不接触任何文件。</div>

<!--
这是为手头的工具绘制的定义。请房间里的人找到写入文件的箭头。
没有一条线离开琥珀色盒子。模型产生了文本，一个程序决定如何处理它。

Aider 区域内的三个盒子，加上提交，它们是三张卡片：组装请求，
解析和应用回复，记录它。接下来的三张幻灯片是中间那个盒子：回复
是什么样子，以及框架必须从中恢复什么。

这是 Aider 的图片，不是他们构建的必需架构。他们将在下周绘制自己的版本。
-->

---
layout: default
---

<div class="label">回复</div>

# 编辑形状的回复

```text
path/to/file.py
<<<<<<< SEARCH
the lines exactly as they appear on disk
=======
the lines that replace them
>>>>>>> REPLACE
```

**一个块，一个文件。没有任何东西强制集合必须是完整的。**

<!--
问一个这样的块不能告诉你什么。它命名一个文件。需要三个文件的更改
需要三个块，格式中没有任何东西强制模型产生全部三个。
一个读懂一个格式良好的块的学生看到一个已完成的功能。

磁盘上还没有任何变化。这是聊天回复中的提案。
-->

---
layout: default
---

<div class="label">另一种形状</div>

# 两种回复形状，每个模型设置一种 {.assert}

| | 整个 | diff |
|---|---|---|
| 模型返回 | 整个文件，每次 | 搜索和替换块 |
| 失败方式 | 删除你从未提到的行 | 与磁盘上的行不匹配 |
| 成本 | 每一行，每次编辑 | 只改变的行 |
| 设置为 | 4B | 9B，如果你切换了它 |

**一个框架设置，每个模型一行。这就是为什么会话不同。**

<!--
根据 2026-09-13 讲师指示添加，late，为了更多内容。最后一张幻灯片
显示了一种形状。这是另一种，也是他们大多数一直在运行的：实践设置
文件给 4B 和 9B `edit_format: whole`，9B 上有"try: diff"。

Whole 在小模型上可靠，因为没有什么可匹配的，而且昂贵，因为每次
编辑都重新发送整个文件。它通过遗漏失败：模型重写文件并静默删除
一行它没有考虑的内容，这就是 L01 demo 的第一个提示所做的，没人
注意到。Diff 便宜且响亮地失败：搜索文本必须与磁盘完全匹配，当它
不匹配时，块被拒绝并被告知模型。

粗体行是重点。形状不是模型的属性。它是框架读取的设置文件中的一
行，他们的构建在做同样的事情，决定回复必须是什么形状。
-->

---
layout: default
---

<div class="label">一般问题</div>

# 通过文本通道的动作

| 模型返回 | 框架需要 |
|---|---|
| 散文 | 文件路径 |
| 散文 | 精确区域 |
| 散文 | 替换文本 |

<div class="caption">搜索块、工具调用 JSON、标记输出。一个问题。</div>

**你的解析器是做什么将回复变成编辑，或拒绝它的。**

<!--
从格式中退一步。无论模型被要求产生什么，它都在产生文本，框架必须
从中恢复一个机器可执行的指令。搜索和替换块是其中一种编码。函数调用
是另一种。标记输出是第三种。

它们都以同样的三种方式失败：什么都不解析，它解析但命名了不存在的
东西，或者它解析但是错的。它们的错误处理必须区分它们，因为每种情况
的恢复是不同的。

这就是为什么 packet 要求匹配语义和路径验证作为独立的需求，而不是
一个。
-->

---
layout: default
---

<div class="label">Aider 缺少什么</div>

# Aider 没有什么

| Aider 没有 | 你在哪里见过它 |
|---|---|
| 周围的编辑器 | Cursor |
| 你输入时的补全 | Cursor |
| 超越会话的笔记 | ChatGPT, Claude Code |
| 自己寻找的文件 | Cursor, Claude Code |

**每一行都是一个框架功能。它们都不是模型。**

<!--
这张幻灯片下面的问题是他们中的一些人已经携带了两周的问题：为什么
每个人都在谈论的工具感觉比这个更聪明？同样的模型。它缺少的是模型
周围的机制，这里的每一行都是某人编写代码的决策。

第三行值得一个节拍。默认情况下，Aider 会话之间没有任何东西传递，
ChatGPT 或 Claude Code 会话之间也没有。一个框架为你保留笔记，另一
个读取你保留的笔记，两者都将它们粘贴到提示中。连续性总是框架重新
发送，这就是他们自己的构建运行的条件。

第四行是接下来重要的一个。Aider 不去寻找文件；他们命名它们。下一
节是关于它猜测的那一个地方，以及为什么那个猜测 passed for intelligence。

不要超过第四行。当模型选择下一个动作时会发生什么，是明天的讲座，
带有实时演示，L05 在这副牌在 taught half 结束时留下的开放问题上
打开。

他们的构建 by design 也缺乏所有四行。Packet 排除了地图，不要求
编辑器、补全或记忆。
-->

---
layout: section
---

# 上下文

## 谁决定模型看到什么

<!--
根据 2026-09-13 讲师指示重新设计。章节之前打开的是 Aider 图表，
它属于框架，命令幻灯片坐在章节标题前面，读起来像是一个 stray。两者
现在都在它们所属的位置，章节读起来像是一个问题：对于提示的每个部分，
谁选择了它，你还是框架。

L02 和 L03 教授了窗口、四个来源、外科选择和 /tokens。不要重新教授
它们。这里的切割是不同的：不是提示中有什么，而是谁把它放在那里，
然后框架自己选择的那一部分，也就是 passed for intelligence 的部分。
-->

---
layout: default
---

<div class="label">两个选择者</div>

# 谁选择每个部分

| 提示部分 | 选择者 |
|---|---|
| 系统提示 | Aider，按模式 |
| 添加和只读文件 | 你，按命令 |
| 仓库地图 | Aider，按排名 |
| 历史 | 你开始，Aider 修剪它 |
| 你的消息 | 你 |

**你从未触碰的两行。其中一行 passed for intelligence。**

<!--
与 L03 列出的部分相同，切割方式不同。读右列并排序行：你的，
Aider 的，共享的。

你的：文件 和消息。Aider 的：系统提示，在 ask 和 code 之间切换时
改变，以及地图，他们从未输入过命令。共享的：历史，他们一次写一个
回合，Aider 用弱模型总结，当它超过限制时。

模型没有行，两张幻灯片后解释原因：在 Aider 中它只能写入。
当它想要一个文件时，它只能在散文中提到名称，Aider 的正则表达式
和你的是做其余部分。

粗体行指向接下来的两张幻灯片。在 Aider 拥有的两行中，系统提示是
无聊的，地图是愚弄他们的那个。
-->

---
layout: default
---

<div class="label">你的那一半</div>

# 你控制什么

| | |
|---|---|
| 安排上下文 | `/add` `/read-only` `/drop` `/clear` `/tokens` |
| 更改你的文件 | `/code` `/undo` |

<div class="caption">纯文本意味着 <code>/code</code>。默认写入。</div>

**大部分界面安排上下文。两个命令写入。**

<!--
根据 2026-09-13 移到上下文章节下；它之前坐在章节标题前面。
它是最后一张幻灯片的 human 的一半，作为命令。

整套命令中只有两个接触代码仓库，其中一个是撤销。这种不对称是他们
即将构建的批准层，在他们已经使用的工具中可见。Aider 在写入前不询问；
他们的构建必须。

/help 列出了比这更多的命令，集合在版本之间变化。如果需要，现场运行
而不是背诵。
-->

---
layout: default
---

<div class="label">当模型想要一个文件时</div>

# 在 Aider 中，模型只能写入 {.assert}

| | Aider | Claude Code |
|---|---|---|
| 模型的动词 | 写入 | 读取、搜索、编辑、运行 |
| 想要一个文件 | 在散文中提到它的名称 | 调用读取工具 |
| 谁让它进入 | Aider 发现名称，你说是 | 权限规则 |
| 什么进入提示 | 文件，每回合重新读取 | 文件，一次，当时的副本 |

**Aider 做的其他一切都是围绕一个只能写入的模型添加的内容。**

<!--
根据 2026-09-13 讲师指示添加，late：这两个工具以不同方式处理模型
想要的文件，差异值得一张幻灯片。

从第一行开始。Aider 中的模型有一个动词。它的回复是编辑块，牌组中
的所有其他内容，地图，文件提及，lint 和测试循环，shell 建议，是
Aider 围绕它添加的机制，使那个动词更好地落地。它们都不是模型在决定
任何事情。

第二行要慢慢说。当 Aider 的模型"想要一个文件"时，它没有要求任何
东西。它在散文中写了一个文件名，Aider 中的一个正则表达式找到了它，
Aider 在屏幕上放了"Add file to the chat?"，一个人说是。模型没有决定
读取文件。它提到了一个名称。

Claude Code 的模型确实决定了：它发出一个读取调用，框架运行它，受
于某人配置的权限规则。这是真正的差异，谁选择，这是这门实验课最接近
明天的主题。停在文件。读取后循环如何继续是 L05。

第四行是维护差异，中间部分是关于它的。Aider 的副本每回合从磁盘重建，
所以它不会过时。Claude Code 的是历史中的一个快照，当前直到文件改变，
之后过时，直到模型再次读取它。
-->

---
layout: default
---

<div class="label">Aider 的一半</div>

# 它知道您从未添加过的文件 {.assert}

```text
package/store.py:
⋮
│class Store:
│    def add(self, title, tags=()) -> Task:
│    def remove(self, task_id: int) -> bool:
⋮
│    def search(self, text, tag=None) -> list[Task]:
⋮

package/models.py:
│@dataclass
│class Task:
⋮
```

<div class="caption mt-4">只有签名。 <code>⋮</code> 是被削减的内容。</div>

**这就是为什么它似乎知道你的代码。**

<!--
房间里每个人都有过这一刻：他们添加了一个文件，要求一个更改，回复
提到了一个来自他们从未提及的文件的类。那不是模型知道代码库。是这
个块，每回合发送。

指向竖线：那是 aider 引用源代码行。指向点：那是它决定不花费 token
的所有内容。只有签名，没有主体，所以模型可以命名一个它从未读取的
函数。

第一次会话是它看起来最聪明的时候，有一个原因。没有添加文件，
Aider 将地图的 token 预算翻倍，所以地图在他们没有给它任何东西的
时候最大。

问这是做什么的。答案在下一张幻灯片：它是 aider 猜测你会输入 /add
什么。
-->

---
layout: default
---

<div class="label">排名</div>

# 仓库地图的来源

<div class="mt-2">
<svg viewBox="0 0 880 250" style="width:100%" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker id="rg" markerWidth="9" markerHeight="9" refX="8" refY="3" orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L8,3 z" fill="#7A8099"/>
    </marker>
  </defs>
  <text x="30" y="26" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#7A8099">1. parse every file for what it defines and what it uses</text>
  <rect x="132" y="43" width="116" height="34" rx="17" fill="#F1EFE5" stroke="#DEDCD0"/>
  <text x="190" y="65" text-anchor="middle" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#1B2547">cli.py</text>
  <rect x="132" y="183" width="116" height="34" rx="17" fill="#F1EFE5" stroke="#DEDCD0"/>
  <text x="190" y="205" text-anchor="middle" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#1B2547">store.py</text>
  <rect x="292" y="113" width="116" height="34" rx="17" fill="#F1EFE5" stroke="#DEDCD0"/>
  <text x="350" y="135" text-anchor="middle" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#1B2547">models.py</text>
  <g stroke="#7A8099" stroke-width="1.3" fill="none" marker-end="url(#rg)">
    <path d="M190,77 L190,181"/>
    <path d="M240,72 L296,118"/>
    <path d="M240,188 L296,142"/>
  </g>
  <text x="200" y="133" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">calls</text>
  <text x="258" y="84" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">uses</text>
  <text x="258" y="186" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">uses</text>
  <text x="30" y="238" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">models.py is used by both, so it wins</text>

  <text x="520" y="26" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#7A8099">2. rank by what points at them</text>
  <text x="520" y="67" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#1B2547">models.py</text>
  <rect x="632" y="53" width="150" height="16" rx="2" fill="#2E5BFF"/>
  <text x="790" y="67" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">0.44</text>
  <text x="520" y="111" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#1B2547">store.py</text>
  <rect x="632" y="97" width="112" height="16" rx="2" fill="#2E5BFF"/>
  <text x="790" y="111" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">0.33</text>
  <text x="520" y="155" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:12px;fill:#1B2547">cli.py</text>
  <rect x="632" y="141" width="78" height="16" rx="2" fill="#E8E6DA"/>
  <text x="790" y="155" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">0.23</text>

  <line x1="512" y1="168" x2="840" y2="168" stroke="#D97706" stroke-width="1.3" stroke-dasharray="4 3"/>
  <text x="520" y="186" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#D97706">budget ends here</text>
  <text x="520" y="238" text-anchor="start" style="font-family:'IBM Plex Mono',monospace;font-size:11px;fill:#7A8099">cli.py is left out entirely</text>
</svg>
</div>

<div class="caption mt-2">PageRank，渲染直到预算结束。</div>

**线以下是不可见的，无论多么相关。**

<!--
在幻灯片之外，值得说：排名向你刚刚提到的文件倾斜，aider 将列表
渲染下来，直到 token 用完。

Plainly 说出排名规则：许多其他文件依赖的文件得分高，因为它可能是
你需要知道的文件。

虚线是重点，粗体行说明了它。不是"这是你的代码库，"它是"这是你的
代码库能装下的部分，"虚线以下的任何东西对模型都是不可见的。同样的
猜测在上一张幻灯片上看起来像 intelligence，对虚线以下的所有东西都是
沉默的。867 行 aider 代码猜测你会输入 /add 什么。
-->

---
layout: default
---

<div class="label">预算</div>

# 一个共享的 token 预算

<div class="mt-2">
<svg viewBox="0 0 960 300" style="width:100%;max-height:300px" role="img"
     aria-label="两个水平条显示一个输入预算。在早期回合，指令、仓库地图、选定文件、历史和你的消息 fit with headroom left。在后面的回合，历史已经增长，你的消息不再适合限制内。">
  <line x1="830" y1="40" x2="830" y2="272" stroke="var(--c-rule-strong)" stroke-width="2" stroke-dasharray="5 5" />
  <text x="830" y="28" text-anchor="middle" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">BUDGET LIMIT</text>
  <text x="118" y="106" text-anchor="end" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">EARLY TURN</text>
  <text x="118" y="228" text-anchor="end" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">LATER TURN</text>
  <g stroke="var(--c-rule-strong)">
    <rect x="130" y="64" width="110" height="76" fill="var(--c-bg-2)" />
    <rect x="240" y="64" width="100" height="76" fill="var(--c-bg-2)" />
    <rect x="340" y="64" width="230" height="76" fill="var(--c-primary)" fill-opacity="0.16" stroke="var(--c-primary)" />
    <rect x="570" y="64" width="90" height="76" fill="var(--c-primary)" fill-opacity="0.07" stroke="var(--c-primary)" />
    <rect x="660" y="64" width="90" height="76" fill="var(--c-bg-2)" />
    <rect x="750" y="64" width="80" height="76" fill="none" stroke-dasharray="4 4" />
  </g>
  <g style="font:500 11.5px var(--font-mono)" fill="var(--c-ink-soft)" text-anchor="middle">
    <text x="185" y="106">instructions</text>
    <text x="290" y="106">repo map</text>
    <text x="455" y="106">selected files</text>
    <text x="615" y="106">history</text>
    <text x="705" y="106">message</text>
    <text x="790" y="106">headroom</text>
  </g>
  <g stroke="var(--c-rule-strong)">
    <rect x="130" y="186" width="110" height="76" fill="var(--c-bg-2)" />
    <rect x="240" y="186" width="100" height="76" fill="var(--c-bg-2)" />
    <rect x="340" y="186" width="230" height="76" fill="var(--c-primary)" fill-opacity="0.16" stroke="var(--c-primary)" />
    <rect x="570" y="186" width="260" height="76" fill="var(--c-primary)" fill-opacity="0.07" stroke="var(--c-primary)" />
    <rect x="830" y="186" width="90" height="76" fill="var(--c-rose)" fill-opacity="0.12" stroke="var(--c-rose)" stroke-dasharray="4 4" />
  </g>
  <g style="font:500 11.5px var(--font-mono)" fill="var(--c-ink-soft)" text-anchor="middle">
    <text x="185" y="228">instructions</text>
    <text x="290" y="228">repo map</text>
    <text x="455" y="228">selected files</text>
    <text x="700" y="228">history</text>
  </g>
  <text x="875" y="228" text-anchor="middle" style="font:500 11.5px var(--font-mono)" fill="var(--c-rose)">message</text>
  <text x="875" y="284" text-anchor="middle" style="font:500 11.5px var(--font-mono)" fill="var(--c-rose)">does not fit</text>
</svg>
</div>

<div class="caption">一个预算。你的消息是停止适应的原因。</div>

**你的构建决定丢弃什么。Packet 没有决定。**

<!--
这是唯一值得保留的上下文幻灯片，因为它是 L02 和 L03 没有展示的
部分。他们看到窗口是有限的，他们阅读了 /tokens。他们还没有看到它
在会话中填满。

走两个条。早期回合没有任何问题；后面的回合是历史增长后的同一提示，
停止适应的是他们自己的消息。它上面的所有东西都在他们打字之前花掉了。

地图也 charge 这个预算，在 4B 上可以是窗口的三分之一，花费 token
描述模型然后无法在内部看到的文件。这就是为什么 aider-practice 备忘
单说要关掉它，以及为什么地图超出构建范围。Plainly 说出来，所以没人
在第 4 周浪费在上面。

然后粗体行，这是这张幻灯片仍然在这里的整个原因。问他们会先丢弃什么
以及什么必须生存。他们的 Stage 1 合同保留系统指令、当前请求和添加的
文件内容，并拒绝 oversized required input 而不是静默修剪它。那个拒绝
是他们必须写下来并测试的设计决策，packet 没有为他们做出。
-->

---
layout: section
---

# 维护提示

## 没有任何东西传递，所以每个部分每回合都重建

<!--
根据 2026-09-13 讲师指示重新设计，late：提示的结构以及 Aider 如何
管理它，以及关于缓存什么都没有。五张幻灯片。提示从上到下。Aider 如何
保持每个部分最新。 Architect 模式作为第二个 fresh 构建的提示。为模型
写的错误。编辑落地之后。成本幻灯片（"越高改变成本越高"）在同一天晚上
被 cut；前缀重用现在是一句 notes 的话。

从这里消失："编辑形状的回复"和"通过文本通道的动作"，现在坐在框架
章节中，回复属于哪里；"文件更新如何到达"和"它记住什么"，合并成"编辑
落地之后"；以及"静默撤销"，被 cut。

用一句话打开，其他所有内容都遵循它。模型在回合之间什么都不保留。
第十二回合发送第一回合发送的所有内容， plus more。这不是一个要绕过的
限制，是他们自己的构建运行的条件。
-->

---
layout: default
---

<div class="label">按顺序</div>

# 提示，从上到下

<div class="mt-2">
<svg viewBox="0 0 900 340" style="width:100%;max-height:280px" role="img"
     aria-label="Aider 提示的八个部分从上到下堆叠：系统提示、示例、只读文件、仓库地图、历史、添加的文件、你的消息、格式提醒。顶部四个很少改变；底部四个每回合都改变。侧面的箭头读作第一个 token 到最后一个 token。">
  <defs>
    <marker id="pr-ar" markerWidth="9" markerHeight="9" refX="8" refY="3" orient="auto"><path d="M0,0 L0,6 L8,3 z" fill="var(--c-ink-muted)" /></marker>
  </defs>
  <rect x="300" y="24" width="360" height="40" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="480" y="48.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">system prompt</text>
  <rect x="300" y="68" width="360" height="26" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="480" y="85.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">examples</text>
  <rect x="300" y="98" width="360" height="30" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="480" y="117.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">read-only files</text>
  <rect x="300" y="132" width="360" height="34" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="480" y="153.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">repo map</text>
  <rect x="300" y="170" width="360" height="48" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="480" y="198.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">history</text>
  <rect x="300" y="222" width="360" height="48" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="480" y="250.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">added files</text>
  <rect x="300" y="274" width="360" height="32" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="480" y="294.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">your message</text>
  <rect x="300" y="310" width="360" height="22" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="480" y="325.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">format reminder</text>
  <path d="M286,24 H280 V94 H286" fill="none" stroke="var(--c-rule-strong)" stroke-width="1.5" />
  <text x="270" y="63" text-anchor="end" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">RARELY</text>
  <path d="M286,98 H280 V166 H286" fill="none" stroke="var(--c-rule-strong)" stroke-width="1.5" />
  <text x="270" y="136" text-anchor="end" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">WHEN YOU ASK</text>
  <path d="M286,170 H280 V332 H286" fill="none" stroke="var(--c-rule-strong)" stroke-width="1.5" />
  <text x="270" y="255" text-anchor="end" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">EVERY TURN</text>
  <line x1="700" y1="38" x2="700" y2="318" stroke="var(--c-ink-muted)" stroke-width="1.5" marker-end="url(#pr-ar)" />
  <text x="714" y="36" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">FIRST TOKEN</text>
  <text x="714" y="330" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">LAST TOKEN</text>
</svg>
</div>

**顶部稳定，底部易变。顺序是一个设计决策。**

<!--
Aider 发送它们的八个部分，从源代码中读取。括号按更改频率分组：
系统提示及其 worked examples 几乎从不更改，只读文件和地图当它们要求
某些东西时更改，底部四个每个回合都更改。

工作粗体行而不是各部分。每回合更改的部分被故意放在最后。一个句子
解释原因，没有幻灯片：服务器跳过重新读取它已经看到的开头，所以
stable-first 使大多数回合更便宜。把顺序放上去，让房间里的人在你说
之前猜测。

他们的构建组装同样的事情，packet 要求他们说进去什么以及按什么顺序。
这张幻灯片是论证答案是设计决策而不是细节。
-->

---
layout: default
---

<div class="label">每个回合</div>

# Aider 如何保持每个部分最新

| 部分 | 每个回合，Aider |
|---|---|
| 系统提示 | 按模式交换：ask, code, architect |
| 只读文件 | 从磁盘重新读取 |
| 仓库地图 | 围绕你刚刚命名的重新排名 |
| 历史 | 添加你的消息和它的回复，长时总结 |
| 添加的文件 | 从磁盘重新读取，整个 |
| 你的消息 | 附加格式提醒，窗口几乎满时丢弃 |

**没有任何东西被记住。每一行都重建。**

<!--
讲师要求的幻灯片：不是提示中有什么，而是 Aider 在每回合之间对每个
部分做了什么。每一行都是从 0.86.2 源代码中读出的。

系统提示按模式选择，ask 模式也删除 worked examples。只读和添加的文件
每回合从磁盘重新读取，所以每个文件只有一个副本，它是当前的。地图围绕
刚刚输入的 文件名和标识符重新排名。历史通过每个回合的两半增长，超过
token 限制时由弱模型总结。格式提醒粘在学生的自己消息末尾，没有空间时
被留下。

问他们的构建有哪些行。系统提示，文件，历史和消息。没有地图，提醒是
他们决定。然后问他们中的哪些他们必须每回合重新读取，以及为什么：答案
是文件，原因是本节的最后一张幻灯片。接下来：architect 模式，用同样的
部分构建第二个提示。
-->

---
layout: default
---

<div class="label">两个提示</div>

# Architect 模式以干净的状态启动编辑器 {.assert}

<div class="mt-2">
<svg viewBox="0 0 900 350" style="width:100%;max-height:280px" role="img"
     aria-label="两个提示并排。Architect 提示是全栈：一个计划系统提示、只读文件、仓库地图、历史、添加的文件、你的消息，它的回复是一个计划。Editor 提示是三部分：编辑格式系统提示、添加的文件，以及作为消息传递的计划。没有历史，没有地图。">
  <defs>
    <marker id="ar-plan" markerWidth="9" markerHeight="9" refX="8" refY="3" orient="auto"><path d="M0,0 L0,6 L8,3 z" fill="var(--c-amber)" /></marker>
  </defs>
  <text x="70" y="34" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">ARCHITECT · THE MODEL YOU TALK TO</text>
  <rect x="70" y="52" width="300" height="36" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="220" y="74.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">system prompt: plan, do not edit</text>
  <rect x="70" y="92" width="300" height="26" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="220" y="109.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">read-only files</text>
  <rect x="70" y="122" width="300" height="30" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="220" y="141.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">repo map</text>
  <rect x="70" y="156" width="300" height="44" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="220" y="182.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">history</text>
  <rect x="70" y="204" width="300" height="44" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="220" y="230.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">added files</text>
  <rect x="70" y="252" width="300" height="30" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="220" y="271.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">your message</text>
  <rect x="70" y="294" width="300" height="34" rx="4" fill="var(--c-highlight)" stroke="var(--c-amber)" stroke-width="2" />
  <text x="220" y="315.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">its reply: a plan</text>
  <text x="530" y="34" style="font:500 11.5px var(--font-mono); letter-spacing:0.12em" fill="var(--c-ink-muted)">EDITOR · A SECOND CALL, STARTED EMPTY</text>
  <rect x="530" y="52" width="300" height="36" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="680" y="74.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">system prompt: the edit format</text>
  <rect x="530" y="92" width="300" height="44" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="680" y="118.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">added files</text>
  <rect x="530" y="140" width="300" height="34" rx="4" fill="var(--c-highlight)" stroke="var(--c-amber)" stroke-width="2" />
  <text x="680" y="161.5" text-anchor="middle" style="font:500 12.5px var(--font-mono)" fill="var(--c-ink)">the plan</text>
  <text x="530" y="202" style="font:400 13px var(--font-sans)" fill="var(--c-ink-soft)">no history · no map</text>
  <path d="M372,311 H450 V157 H524" fill="none" stroke="var(--c-amber)" stroke-width="2" marker-end="url(#ar-plan)" />
  <text x="460" y="324" style="font:400 13px var(--font-sans)" fill="var(--c-ink-soft)">handed over as a message</text>
</svg>
</div>

**两个提示。第二个是 fresh 构建的：文件和计划，没有其他内容。**

<!--
根据 2026-09-13 讲师指示添加，late。他们在第 6 课中使用过这个，课程
说这是两次模型调用。这是两个提示的内容，从最后两张幻灯片的同样部分
构建。

从源代码：当计划被接受时，Aider 创建第二个 coder，空历史，没有仓库
地图，编辑格式系统提示，以及计划作为唯一的用户消息。Editor 看到文件
和计划。没有其他内容。Architect 自己的提示是全栈，它在整个会话中保持
一种模式，所以它的系统提示从不交换。

说为什么它这样构建。一个充满历史的长提示是决定做什么的好提示，是
产生精确编辑的坏提示。拆分工作给每个调用它需要的提示：整体图片给
计划，短焦点提示给编辑。这就是 architect 模式在小模型上得分更好的
原因，它是一个结构决策，不是模型特性。

`auto-accept-architect: false` 在实践仓库中是为什么它在第二次调用前
停止询问的原因。他们的构建可以在一个增量中做同样的事情：用完整上下文
计划，用小上下文编辑。
-->

---
layout: default
---

<div class="label">失败作为输入</div>

# 为模型写的错误

Aider 回答模型，不是你。

- 失败的块，引用回来
- 与文件几乎匹配的行
- 如果工作已经存在的说明

**错误消息是一个提示。**

<!--
当一个块应用失败时，返回的不是给人的消息。它是为模型在下一回合阅读
而写的：这是你的块，这里是几乎匹配的行，如果替换文本已经在文件中，
一个问题询问块是否真的需要。

这个重新框架本身值得一张幻灯片。在一个唯一执行器是文本的系统中，
错误消息是另一个提示，它的工作是让下一次尝试更好，而不是描述出了什么
问题。

Aider 会这样做最多三次然后放弃。他们的构建根本不必重试，但他们必须
决定，packet 对失败路径评分。
-->

---
layout: default
---

<div class="label">落地</div>

# 编辑落地之后

<div class="mt-2">
<svg viewBox="0 0 900 340" style="width:100%;max-height:280px" role="img"
     aria-label="提示栈，两部分被呼出。历史逐字保留，所以它仍然保存模型自己的回复，引用旧行。添加的文件每回合从磁盘重建，所以它们保存当前文本，没有 diff，没有旧副本。">
  <rect x="80" y="24" width="300" height="40" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="230" y="48.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">system prompt</text>
  <rect x="80" y="68" width="300" height="26" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="230" y="85.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">examples</text>
  <rect x="80" y="98" width="300" height="30" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="230" y="117.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">read-only files</text>
  <rect x="80" y="132" width="300" height="34" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="230" y="153.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">repo map</text>
  <rect x="80" y="170" width="300" height="48" rx="4" fill="var(--c-highlight)" stroke="var(--c-amber)" stroke-width="2" />
  <text x="230" y="198.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">history</text>
  <rect x="80" y="222" width="300" height="48" rx="4" fill="var(--c-primary)" fill-opacity="0.12" stroke="var(--c-primary)" />
  <text x="230" y="250.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">added files</text>
  <rect x="80" y="274" width="300" height="32" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="230" y="294.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">your message</text>
  <rect x="80" y="310" width="300" height="22" rx="4" fill="var(--c-bg-2)" stroke="var(--c-rule-strong)" />
  <text x="230" y="325.7" text-anchor="middle" style="font:500 13px var(--font-mono)" fill="var(--c-ink)">format reminder</text>
  <line x1="382" y1="194" x2="420" y2="194" stroke="var(--c-amber)" stroke-width="2" />
  <text x="430" y="190" style="font:600 14px var(--font-sans)" fill="var(--c-ink)">逐字保留</text>
  <text x="430" y="210" style="font:400 13px var(--font-sans)" fill="var(--c-ink-soft)">它自己的回复仍然在这里，引用旧行</text>
  <line x1="382" y1="246" x2="420" y2="246" stroke="var(--c-primary)" stroke-width="2" />
  <text x="430" y="242" style="font:600 14px var(--font-sans)" fill="var(--c-ink)">每回合从磁盘重建</text>
  <text x="430" y="262" style="font:400 13px var(--font-sans)" fill="var(--c-ink-soft)">当前文本，没有 diff，没有旧副本</text>
</svg>
</div>

**从未被告知改变了什么。展示现在什么是真的，并留下它引用的内容。**

<!--
从"文件更新如何到达"和"它记住什么"合并而来，讲师发现很难作为两张
幻灯片呈现。同样的栈作为本节的其余部分，两部分被呼出。

一旦学生理解提示被重建后的 问题：编辑落地后，模型被告知，还是文本
在它下面改变？两者，第二个更重要。

文件块从磁盘重建，所以每个文件只有一个副本，它是当前的。永远不会
发送 diff，也不会保留旧副本。Aider 还向历史添加一行，用学生的声音，
说它用给定的 hash 提交了。这就是模型被告知的全部。

历史是另一半。模型自己的回复逐字保留，它们充满了引用旧文本的编辑块。
所以它对先前版本的记录恰好是它选择改变的行，以及周围没有任何东西，
对过去的有偏采样。这就是为什么 Aider 自己的提示警告模型其他消息可能
包含过时的内容。它无法修复，只能警告。

两个值得说的后果。他们在另一个窗口中自己编辑的文件在下一回合被无
通知拾取。课程模型上的 /undo 不告诉模型任何东西，所以它的历史仍然
保存它提议的编辑，文件不再保存。静默撤销幻灯片今晚从牌组中移除；事
实留在这里，以及为什么他们的规范要求撤销是一个 owned, current-session
操作，在干净的目标上。
-->

---
layout: section
---

# Hackathon 1

## 在你自己构建的一个功能，在一个晚上

<!--
根据 2026-09-14 讲师请求新增的章节幻灯片，所以 hackathon 读起来像
是它自己的块，而不是落地幻灯片的尾部。它下面两张幻灯片：格式和工作
区。总共三四分钟。
-->

---
layout: default
---

<div class="label">2026 年 9 月 24 日星期四</div>

# 格式

- 两小时，你自己的项目
- 在房间里揭示的提示
- 离开前提交
- 与构建分开评分

<!--
总共三分钟，包括后勤。房间、时间和工作区中的模型由工作人员宣布。
不要在这里发明任何内容。

Plainly 说出便利路线：冲突和便利现在去找工作人员，而不是那一周。
-->

---
layout: default
---

<div class="label">Hackathon 1</div>

# Hackathon 工作区

- 浏览器 VS Code，已经设置好
- Aider 指向课程端点
- 你的代码仓库已经克隆
- 一个 Python 参考包，离线

<!--
带有 token 数字的说明和关于带来一个运行两个的构建的结束行从这张
幻灯片上在 2026-09-14 讲师请求时被移除。两点仍然值得说；它们是你
说的，不是幻灯片的。

没有人安装任何东西，没有人需要网络。工作区打开时工具已经指向正确的
地方，他们自己的代码仓库在里面。

说明是幻灯片的重点，它直接连接回预算块。图片中的参考包是大部分
Python 文档，转换后模型可以阅读，它大约是房间里任何上下文窗口的
八十倍。它被分成足够小的部分，一次添加一个。

所以练习不是文档是否可用。是哪几页，选择好是完成和未完成的区别。
这就是有成绩附加的上下文预算。

从当前工作人员公告中宣布房间、开始时间和补考路径。不要在这里发明
任何内容。

重量幻灯片在 2026-09-13 被 cut，数字是你说的或不说。为了记录：
hackathon 1 是 20 个 Apply 部分中的 10 个，所以一半的 Apply 成绩
against 四分之一的构建和四分之一的练习课。它不再在玻璃上。

粗体行是准备说明，它是这里唯一改变他们今晚做什么的东西。提示要求
他们自己代码上的一个功能，在房间里揭示，所以一个不运行的代码仓库
无法开始。那天晚上 mid-refactor 的任何人有糟糕得多的两小时。
-->

---
layout: section
---

# 结对程序员

## Stage 1 要求什么

<!--
揭示。他们已经花了两周驾驶一个框架，最后一小时拆解一个；现在他们
构建一个。

以它是什么开头，而不是文书工作。Packet 带有要求，它们可以阅读。
-->

---
layout: default
---

<div class="label">Stage 1 · 项目</div>

# 从零开始构建一个框架

你最后一小时的一切，由你编写。

| | |
|---|---|
| 它持有 | 你选择的一组文件 |
| 它询问 | 你配置的模型端点 |
| 它写入 | 只有在你批准之后 |

<div class="caption">没有 Aider 的 Aider。同样的工作，你的代码。</div>
<div class="caption">截止日期：<strong>2026 年 10 月 6 日星期二，晚上 11:59</strong>。</div>

<!--
Plainly 说出一个句子版本：一个终端结对程序员，在你选择的文件上，
与你配置的端点对话，从不编辑而不询问。Packet 中其他所有内容都是
那个句子下的细节。

它不是 Aider 的克隆，也不需要是。没有仓库地图，没有 lint 循环，
没有 architect 模式。它必须有他们刚刚观看的脊柱。

如果有人问它有多大：比他们害怕的小，规范工作是决定它需要多长的
部分。

幻灯片下的日期是学生在牌组中看到构建截止日期的唯一地方。Plainly
说出来，而不是跳过它。
-->

---
layout: default
---

<div class="label">Stage 1 · 它如何运行</div>

# 实现之前先写规范

你在写代码之前写需求，提交历史必须显示它。

| 你写的内容 | 何时 | 涵盖 |
|---|---|---|
| 系统设计 | 一次，实现之前 | 配置、运行契约、七个功能 |
| 三个图表 | 与设计一起 | 组件、序列、编辑生命周期 |
| 增量规范 | 在你选择的每个切片之前 | 有序、可测试、上下文范围 |

<div class="caption">没有提供规范。</div>

<!--
这是 Stage 1 的整体形状，以及大多数人通过以代码开始而做错的事情。
没有人 handed them a specification：他们写自己的需求，自己的接受标准，
和自己的增量。

历史是证据，不是官僚主义。在设计之后提交的设计描述什么也不赚，他们
无法在最后重建它。

逐行。需求必须涵盖配置和执行契约以及七个功能，可观察的结果，边缘
情况和排除，每一个都必须追溯到他们自己的接受标准。图表描述他们的
系统，不是 Aider：复制今天早上的图片什么也不赚，Mermaid 足够了，标签
必须 legible 和准确。本节中没有长度要求。

增量不是提交，不是单一更改。它是构建的一个相当大的切片，他们自己
 breakout 和定义，有可观察的结果，它自己的范围上下文和自己的有序
任务。他们见过的最接近的东西是 aider-practice 课，除了这里他们写
课程。

他们选择有多少个以及边界在哪里。Packet 明确说明这一点，rubric 评分
集合是否以 sensible 顺序覆盖整个构建，而不是是否有很多。
-->

---
layout: default
---

<div class="label">评分</div>

# 规范与设计

**100 个构建点中的 50 个。**

| 标准 | 分数 |
|---|---:|
| 需求和接受标准 | 10 |
| 架构和界面设计 | 10 |
| 增量规范 | 10 |
| 三个图表 | 4 / 3 / 3 |
| 代码前的规范 | 5 |
| 改变和为什么 | 5 |

<!--
不要逐行读表。唯一值得说的一件事：他们设计但没有实现的部分只要
标记就保持其设计分数。

架构分数包括解释工具执行、替换端点客户端和非终端界面放在哪里，
而不构建它们。

最后两行从 git 历史而不是文档中读取。代码前的规范在提交顺序上评分，
改变和为什么是他们开始的规范和他们完成的系统之间的调和。有效的设计
不需要发明的失败。
-->

---
layout: default
---

<div class="label">评分</div>

# 实现与测试

**100 个构建点中的 50 个。**

| 标准 | 分数 |
|---|---:|
| 功能正确性 | 25 |
| 自定义测试用例 | 9 |
| 失败路径测试 | 8 |
| 端到端运行和实时证据 | 5 |
| 操作说明 | 3 |

<div class="caption"><code>RUBRIC.md</code> 携带每一行。</div>

<!--
100 分跨越两个 50 分的部分。这些是构建点，不是课程成绩百分比：
构建是课程的 4.5%。

功能正确性是七个功能，在 RUBRIC.md 中逐行评分。部分正确性 exists 对于
部分工作的行为，所以未完成的功能不是零。

测试数量和覆盖本身不赚任何分数。测试在断言结果和隔离文件系统时赚
取分数。弱化断言以使套件变绿 is scored as what it is。

配置坐在 F1 的五分之内，不是一个门。

工作人员在截止日期读取代码仓库快照，运行他们自己的测试，并通过
ACCEPTANCE.md。没有隐藏套件，也没有规范中没有的要求。常规测试使用
fakes 而不是实时模型；实时检查单独记录并 opt in，诚实的报告失败的
实时尝试仍然赚取其证据分数。

把它们送到 RUBRIC.md 而不是逐行读。它命名每个行评分的需求 ID。
-->

---
layout: default
---

<div class="label">你保留的内容</div>

# 证据链

| | |
|---|---|
| 提交 | 代码之前的规范 |
| 会话 | 什么上下文，什么模型，发生了什么 |
| 调和 | 设计在哪里改变了，以及为什么 |

<!--
这些在昨晚之前无法重建，这就是为什么它值得一分钟在这里。提交和
调和是规范幻灯片上的最后两个评分行。

会话和模型披露是必需的，但本身不赚取分数。它们让工作人员跟随证据
的其余部分，所以没有它们的提交在任何主张无法检查的地方失去分数。
-->

---
layout: default
---

<div class="label">模型规则</div>

# 规范的模型规则

- 起草、 critique、图表、代码、测试
- 推荐 9B，允许 4B
- 次要模型也记录
- 允许任何兼容的主机

**不同的提供者不是不同的模型。**

<!--
确切的模型 ID 和观察会话例外在作业 packet 中。从那里读取它们，
而不是从记忆中。

被排除的模式是前沿模型用于设计，后面跟着本地模型用于代码。保留
会话，披露真实别名和任何你知道的量化。
-->

---
layout: section
---

# 自主性，以及你的框架走向何方

## 框架变得更好。你将自己构建它。

<!--
根据 2026-09-15 讲师指示重写。本节旧的 opening 为 Aider 和弱模型
辩护，两个论点已经在 L01 到 L04 中提出，所以那三张幻灯片消失了：
"这是领域开始的地方"，"Aider 适合学习"和"模型 intentionally 弱"。
章节现在打开在自主性：一个框架决定什么，从 2023 年起模型被 handed
了多少，以及这门课程在一个代码仓库中达到这些里程碑的事实。之后的
三张幻灯片被保留并收紧。

五张幻灯片。一个框架决定什么，以及谁在 2023 年 against 2025 年决定
它。三个里程碑，M1 到 M3，按顺序在这里构建。一个代码仓库到十二月。
你将知道 Claude Code 如何工作。同一个循环，更多的机制，结束于 L05
留下的内容。

每张幻灯片的笔记以"Next:"行结尾，说出下一张幻灯片携带什么，所以
没有什么早说一张幻灯片。
-->

---
layout: default
---

<div class="label">自主性</div>

# 自主性是 handed to the model 的决策 {.assert}

| 框架决定 | 2023 年，Aider | 2025 年，一个 agentic CLI |
|---|---|---|
| 模型看到什么 | 你，用 `/add` | 模型询问，并读取它 |
| 对回复做什么 | 一个编辑，先展示给你 | 一个工具调用，对照规则检查 |
| 接下来发生什么 | 你输入下一个请求 | 模型选择下一个动作 |
| 什么在会话中幸存 | 什么都没有 | 笔记、记忆、它可以恢复的运行 |

**模型调用没有改变形状。它周围的一切都改变了。**

<!--
每一行都是框架做出的决策，两列是它让谁做出。先读中间列：在 Aider
中他们自己两周做了这些中的每一个，模型的贡献是一个文本块。然后右列：
同样的四个决策，在每一个框架现在让模型选择并对照规则检查选择。

这就是"更自主"意味着什么，以及 2025 框架的复杂性和力量在哪里。右边
的模型通常是左边同样的模型。改变的是它自己的回合有多少是自己决定的。

不要说框架如何让模型选择下一个动作，或那花费什么。L05 花九十分钟
 exactly on that，带有实时演示。

接下来：里程碑，M1 到 M3，按顺序在这里构建。
-->

---
layout: default
---

<div class="label">里程碑</div>

# 项目里程碑

| 里程碑 | 谁采取步骤 | 当你构建它时 |
|---|---|---|
| M1. 结对程序员 | 你选择文件并批准编辑；模型写入 | Apply，从本周开始 |
| M2. 智能体 | 模型选择下一个动作；你写的规则批准它 | Analyze |
| M3. 助手 | 它在运行之间保持状态并幸存于真实工作 | Create |

**更大的模型买不到这些。每个里程碑都是你写的框架。**

<!--
三个里程碑是三个阶段，顺序是框架增长的顺序。M1 是他们这周阅读的
构建：他们选择模型看到什么，在他们说是之前没有任何东西接触磁盘。
M2 将一个决策，下一个动作，从他们移到模型，那个移动的价格是一个他们
必须写的批准层。M3 是使它 day after day 可用的东西：记忆、硬化、
不止一种方式进入。

粗体行是课程的设计。模型从现在到十二月之间不会增长，所以他们在自主
性中获得的任何东西都不是来自更大的模型。它来自他们自己代码仓库中的
框架代码。这也是为什么模型是一行配置，框架是产品。

接下来：同样的三个里程碑作为时间线，每个添加什么 in code。不要在这里
走阶段；下一张幻灯片做。
-->

---
layout: default
---

<div class="label">到十二月</div>

# 一个代码仓库到十二月

<div class="mt-4">
<svg viewBox="0 0 900 150" style="width:100%;max-height:180px" role="img"
     aria-label="一个时间线，三个阶段：Apply 是 M1，一个循环，精确编辑和 git；Analyze 是 M2，模型选择它的工具；Create 是 M3，记忆、硬化和另一种方式进入。">
  <line x1="80" y1="70" x2="820" y2="70" stroke="var(--c-rule-strong)" stroke-width="2" />
  <g fill="var(--c-primary)">
    <circle cx="170" cy="70" r="7" />
    <circle cx="450" cy="70" r="7" />
    <circle cx="730" cy="70" r="7" />
  </g>
  <g style="font:600 12px var(--font-mono); letter-spacing:0.14em" fill="var(--c-primary)" text-anchor="middle">
    <text x="170" y="48">APPLY</text>
    <text x="450" y="48">ANALYZE</text>
    <text x="730" y="48">CREATE</text>
  </g>
  <g style="font:400 13px var(--font-sans)" fill="var(--c-ink-soft)" text-anchor="middle">
    <text x="170" y="102">M1: one loop, exact edits, git</text>
    <text x="450" y="102">M2: the model picks its tools</text>
    <text x="730" y="102">M3: memory, hardening, another way in</text>
  </g>
</svg>
</div>

**没有任何东西被丢弃。每个阶段增长上一个。**

<!--
从左到右走时间线，说每个点添加什么 in code：M1 是这个月的循环，
精确编辑和 git；M2 是模型选择的工具和 gate 它们的批准层；M3 是
幸存运行的 状态、硬化，和另一种方式进入。

曾经坐在这张幻灯片上的构建建议，仍然属于房间：一个小的可检查结果
首先，并保持阶段快照。

2023-against-2025 表两张幻灯片后是工具比较在这门实验课中走多远。
L05 有实时演示；不要在这里重建它。

不要大声估计工作量。确实说 early runnable slice 是保持十二月集成不
是一个惊喜的东西。

接下来：目的地，命名。不要在这张幻灯片上说 Claude Code。点是 Apply，
Analyze 和 Create，下一张幻灯片说它们加起来是什么。
-->

---
layout: default
---

<div class="label">目的地</div>

# 你将知道 Claude Code 如何工作 {.assert}

到十二月：一个 M3 框架，在 Claude Code 的形状中，从你这周开始的
M1 结对程序员成长。

| 使用它 | 知道它 |
|---|---|
| 批准一个权限提示 | 写它执行的规则 |
| 观察它找到文件 | 写找到它的搜索 |
| 打字，文件改变 | 写改变它的解析器 |

**其他所有人都会使用它。你将编写每一层。**

<!--
从"你将知道 Claude Code 如何工作"和"十二月的框架"合并而来，late，
讲师读到他早说这张幻灯片的内容。牌组中现在有一个地方命名目的地，
它在这里。

先 lead line。命名 Claude Code，或 OpenClaw，作为一个形状，不是一个
克隆目标，也不是他们需要帐户的东西。使它成为那个形状的是他们将构建
的列表：模型选择的工具，一个批准层，幸存运行的状态，不止一种方式进入。
现在开始的设计习惯：当一个决策可能关闭这些中的一个时，留下一个边界
而不是一个实现。

然后表。说左列作为用户看到的，右列作为构建者知道的。每一行是他们
将 recognise 使用工具的时刻，配对在他们自己代码仓库中做同样工作的
代码：这个月的循环，Analyze 中的工具和权限门，Create 中的记忆和
硬化。

这门课程中没有人需要任何东西的订阅。

接下来：开放问题。不要在这里说谁选择下一个动作。
-->

---
layout: default
---

<div class="label">你将遇到的每个工具</div>

# 同样的循环，更多的机制

- 组装、回答、解析、应用、修复、提交
- 你将遇到的每个工具运行它
- 不同的是谁采取每个步骤

**留给 L05：模型如何获得选择下一个动作，以及它花费什么。**

<!--
桥梁，以及 taught half 的结束。六个步骤，今天他们跟随一个请求通过
所有它们。这种类型的每个工具，无论它的名字和谁制作它，运行同样的
六个。

问房间里的人，在他们自己的会话中，六个中有哪些属于模型。一个：答案。
在其他每一步，决定接下来发生什么的是他们，或某人提前写下的规则。

然后留下粗体行。自主性幻灯片已经说了在 M2 模型选择下一个动作。这
副牌没有说的是框架如何使这成为可能以及它花费什么，L05  exactly on
that 打开，带有实时演示。在这里说更多会 spoils it。

这是办公时间前的最后一张幻灯片。留下行。
-->

---
layout: section
---

# 办公时间

## 会话的其余部分是你们的

<!--
停止说话。工作人员从这里开始巡视。保留幻灯片，让学生可以自己翻阅。

这是你 plain 说今天结束时没有任何东西 due 的地方。他们中的大多数
仍然会期望一个 checkoff。
-->

---
layout: end
---
