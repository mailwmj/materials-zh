---
theme: ../theme
title: 'L15: 压缩与提示缓存'
info: |
  EECS 498 AASE, 讲座 15
  Applied Agentic Software Engineering, UMich Fall 2026
transition: fade
mdc: true
layout: none
class: story
---

# 压缩与提示缓存

<img class="story-still" src="./assets/slides/card-title-0.png" />

<style>
:global(.story) { background: #fff; }
:global(.story h1) { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; margin: 0; }
:global(.story img.story-still) { position: absolute; inset: 0; width: 100%; height: 100%; object-fit: contain; }
</style>

<!--
方向：约 1 分钟。无点击。

今天关于框架发送给模型的对话：当它不再适合时发生什么，以及什么使再次发送它变得负担得起。
-->

---
layout: none
class: story
---

# 每个请求携带什么

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-resend-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-resend-2.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-resend-3.png" /></template>
</v-switch>

<!--
方向：约 2 分钟。2 次点击。

从 L07 展示的内容开始。每轮，框架构建一个请求并将其发送给模型。请求 1 持有系统提示词，框架编写的，然后你的消息。颜色贯穿所有讲座：琥珀色是框架，玫瑰色是你，蓝色是模型，绿色是你的计算机，灰色是混合。

点击。模型在请求之间不保留任何内容。所以请求 2 与请求 1 相同，加上模型的工具调用和来自你计算机的结果。

点击。请求 3 是请求 2 再次加上另一个调用和结果。框架每次重新发送整个对话，这提出了今天两个问题。当它不再适合时发生什么？那是第一部分。谁为每次处理所有内容付费？那是第二部分。
-->

---
layout: none
class: story
---

# 今日安排

<img class="story-still" src="./assets/slides/deck-schedule-0.png" />

<!--
方向：约 30 秒。无点击。

这是今天的计划，分两部分。第一部分是压缩：当对话不再适合窗口时框架做什么。然后一个休息。第二部分是提示缓存：什么使每轮重新发送整个对话变得负担得起。然后问题。
-->

---
layout: none
class: story
---

# 第一部分：压缩

<img class="story-still" src="./assets/slides/card-part1-0.png" />

<!--
方向：约 15 秒。无点击。

第一部分。压缩。
-->

---
layout: none
class: story
---

# 一个填满的窗口

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-fill-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-fill-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-fill-2.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-fill-3.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

这里是一个 100 万标记的窗口，空的。前沿模型现在阅读那么多，这听起来像任何东西的空间。

点击。每个请求以系统提示词开始，然后你的任务。假设任务是修复一个缓慢的上传。

点击。然后是工作：工具调用、结果和回复。模型一个接一个地阅读文件，每个结果都留在对话中，因为框架重新发送所有内容。

点击。一天漫长的工作填满了甚至一百万个标记，然后下一个请求不适合。L14 在很久之前展示了一个漫长对话的成本。
-->

---
layout: none
class: story
---

# 处理满窗口的四种方法

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-options-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-options-2.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-options-3.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-options-4.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

框架有四种处理满窗口的方法。它可以停止并返回错误。这是 API 自己的做法：长于窗口的请求被拒绝，会话结束。

点击。它可以丢弃最旧的消息。简单，但最旧的消息是你的任务，所以模型忘记它被要求做什么。

点击。它可以清除旧工具结果。工具输出是编码会话的 bulk，大部分在模型使用后是陈旧的，所以框架将每个旧结果替换为一个短的占位符。

点击。或者它可以摘要并重新开始，保持系统提示词、一份摘要和最新的轮次。前三个是框架单独完成。这一个向模型请求帮助，它被称为压缩。
-->

---
layout: none
class: story
---

# 压缩如何运行

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-compact-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-compact-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-compact-2.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-compact-3.png" /></template>
  <template #4><img class="story-still" src="./assets/slides/l15-compact-4.png" /></template>
</v-switch>

<!--
方向：约 4 分钟。4 次点击。

这里是一次压缩，逐步进行。左侧是它站立时的对话：系统提示词、你的任务、文件读取、回复、你的下一条消息。

点击。框架将一条摘要指令附加到对话的末尾，而不是构建一个新请求。它在窗口满之前这样做，因为这个请求也需要空间。附加的选择在第二部分中再次重要。

点击。模型获得所有内容。

点击。模型编写一份摘要，框架以它启动新对话。

点击。框架填写其余部分：相同的系统提示词、最新消息逐字、关于之前读取文件的说明，以及触发压缩的同一条消息。说明告诉模型它打开了哪些文件，所以它可以再次读取它们。旧对话从这个请求后的每个请求中消失。
-->

---
layout: none
class: story
---

# 摘要保留什么

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-keeps-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-keeps-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-keeps-2.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-keeps-3.png" /></template>
  <template #4><img class="story-still" src="./assets/slides/l15-keeps-4.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。4 次点击。

那份摘要就是下一个请求对到目前为止的工作所拥有的全部内容，所以框架的摘要提示词要求任务需要继续的内容。左侧，对话：你的消息、模型的工作、工具输出，这是大部分内容，以及最新消息。右侧，摘要。

点击。你发送的每条消息，所以像 "永远不要触碰生产配置" 这样的规则在压缩后仍然适用。

点击。决定了什么，带相关代码：修复进入上传的重试循环。

点击。下一步，从最新消息中获取，所以新对话以相同的任务继续并且不会漂移。

点击。大部分工具输出被丢弃。文件仍在磁盘上，模型可以再次读取它们。
-->

---
layout: none
class: story
---

# 摘要丢失什么

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-lost-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-lost-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-lost-2.png" /></template>
</v-switch>

<!--
方向：约 2 分钟。2 次点击。

所以压缩后模型知道什么？压缩前，模型知道要更改的确切行。

点击。摘要必须简短，所以它将其四舍五入：修复在上传代码中的某个地方。

点击。压缩后，模型知道位置但不知道哪一行，所以它必须再次读取文件。获取一个精确的细节回来花费一次读取，一个没有人再次读取的细节就消失了。
-->

---
layout: none
class: story
---

# 什么在压缩中幸存

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-survives-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-survives-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-survives-2.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-survives-3.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

那么什么幸存了？新对话再次出现：指令、摘要、最近的消息。在它旁边，你的计算机。一些东西从对话外部回来。

点击。从磁盘重新读取的指令文件。框架将 AGENTS.md 等文件放回每个请求中，压缩或不压缩，因为它们存在于对话之外。

点击。模型可以再次读取的文件。当它需要确切的行时，模型请求文件，框架从磁盘读取它。

点击。聊天中输入的规则可能无法幸存。摘要尝试保留你的消息，但必须精确的规则属于文件。那是 L10 指令的四个家：AGENTS.md 在每个请求上读取，聊天消息只存在于对话的生命周期内。
-->

---
layout: none
class: story
---

# 压缩作为交接

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-handoff-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-handoff-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-handoff-2.png" /></template>
</v-switch>

<!--
方向：约 1 分钟。2 次点击。

这看起来应该很熟悉。在 L14 中，一个子智能体将一份报告交回主对话。

点击。压缩是从模型到自己的相同交接：较早的对话将一份摘要交给较晚的对话。

点击。每个都丢失了精确细节，所以像阅读子智能体的报告一样阅读摘要。
-->

---
layout: none
class: story
---

# 中场休息

<img class="story-still" src="./assets/slides/deck-chat-0.png" />

<!--
方向：约 5 分钟。无点击。

中场休息。五分钟后回来。
-->

---
layout: none
class: story
---

# 第二部分：提示缓存

<img class="story-still" src="./assets/slides/card-part2-0.png" />

<!--
方向：约 15 秒。无点击。

第二部分。提示缓存。
-->

---
layout: none
class: story
---

# 重新发送的成本

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-repeat-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-repeat-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-repeat-2.png" /></template>
</v-switch>

<!--
方向：约 2 分钟。2 次点击。

第一部分将对话保持在窗口内。第二部分是账单。这里是请求 1 再次出现，系统提示词和你的消息，每个后续请求添加一个调用和结果。

点击。继续到请求 8。每个请求是上一个加上更多内容，提供者在任何内容返回之前每次都运行模型处理每个标记。

点击。所以系统提示词和你的消息在每个请求中被再次处理，到请求 8 是八次。在一个漫长的会话中，重复的工作是大部分成本。
-->

---
layout: none
class: story
---

# 缓存保留什么

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-cache-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-cache-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-cache-2.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-cache-3.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

这里又是三个请求：请求 1 是系统提示词和你的消息，请求 2 和 3 每个添加一个工具调用和结果。

点击。修复方法是提供者保留其工作。请求 1 写入缓存：提供者存储处理后的开始几分钟。框架标记缓存多远，模型永远不知道。

点击。请求 2 以完全请求 1 开始，所以那部分从缓存中读取而不是再次处理。只有新的结尾被处理。

点击。请求 3 以相同的方式从缓存中读取所有请求 2。这就是整个机制。
-->

---
layout: none
class: story
---

# 缓存如何匹配

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-cache-match-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-cache-match-2.png" /></template>
</v-switch>

<!--
方向：约 2 分钟。1 次点击。

匹配必须完全一致。提供者按顺序读取请求：工具、系统提示词、消息。当下一个请求以早期请求的字节开始，该确切前缀从缓存中读取。

点击。在工具中更改了一个字节，工具和所有后续内容都未命中，即使其余部分完全相同。
-->

---
layout: none
class: story
---

# 缓存花费什么

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-cache-cost-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-cache-cost-2.png" /></template>
</v-switch>

<!--
方向：约 2 分钟。1 次点击。

与全价正常输入相比，写入缓存花费四分之一更多，从缓存读取花费十分之一或更少。一个漫长的会话大部分输入从缓存中读取，所以大部分是便宜的。

点击。它活多久：默认五分钟，每次读取重新开始时钟。那个条目写入花费四分之一更多。框架可以支付双倍来写入一个存活一小时的条目，当你离开超过五分钟时这是值得的。
-->

---
layout: none
class: story
---

# 请求顺序

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-order-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-order-2.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-order-3.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-order-4.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

由于匹配从前面开始，框架将变化最少的内容放在最前面。工具，然后系统提示词：每个会话都相同，所以它们中的任何内容都不会命名你的文件夹、你的名字或日期。

点击。接下来是指令文件如 AGENTS.md：项目内相同。

点击。然后技能和日期：会话内相同。日期每天变化，所以它来得晚。

点击。然后对话，每轮增长。Claude Code 按这些层订购其请求，Anthropic 说它围绕提示缓存构建了整个框架。
-->

---
layout: none
class: story
---

# 添加到末尾

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-appends-0.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-appends-1.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-appends-2.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-appends-3.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

缓存解释了你现在可以在任何框架中发现的习惯。请求的开始，工具和系统提示词以及到目前为止的对话，从缓存中读取，所以新内容去末尾。

点击。提醒。当磁盘上的文件更改或模式切换时，框架在下一条消息中添加提醒而不是编辑系统提示词。

点击。一个新的工具结果。即使模型在会话中途找到的工具也作为末尾的结果到达，所以开头的工具列表从不更改。

点击。一个加载的技能。L10 的三个层级：技能的名称和描述在每个请求中，它的主体只在模型请求时到达，作为末尾的工具结果。
-->

---
layout: none
class: story
---

# 什么重置缓存

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-resets-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-resets-2.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-resets-3.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/l15-resets-4.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

那么什么重置缓存？一个更改的工具定义：什么也没有读取。工具排在最前面，所以那里更改一个字节就未命中所有内容。

点击。一个不同的模型：什么也没有读取。每个模型有自己的缓存，所以在会话中途切换模型会为整个对话再次付费。

点击。一个全新的子智能体冷启动。它有自己的系统提示词和一个空对话，所以与父级的前缀没有任何匹配。

点击。一个分支读取父级的缓存，因为它以父级的准确系统提示词、工具和历史开始。这就是为什么 L14 发现启动一个分支比启动一个全新子智能体更便宜。
-->

---
layout: none
class: story
---

# 通过缓存的压缩

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/l15-compact-cache-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/l15-compact-cache-2.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/l15-compact-cache-3.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。2 次点击。

压缩是最后一次重置，框架计划的那一次。在它之前，对话以工具和系统提示词开始。摘要请求是带有摘要指令附加的相同对话，所以所有内容都从缓存中读取。这就是附加的回报。

点击。下一个请求保持工具和系统提示词并启动新对话。摘要和最近消息是新的，所以它们被写入缓存。

点击。工具和系统提示词没有更改，所以它们仍然从缓存中读取。压缩是框架计划的重新写入：它保持开头相同并只为新对话付费。
-->

---
layout: none
class: story
---

# 带走什么

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/deck-takeaways-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/deck-takeaways-2.png" /></template>
  <template #2><img class="story-still" src="./assets/slides/deck-takeaways-3.png" /></template>
  <template #3><img class="story-still" src="./assets/slides/deck-takeaways-4.png" /></template>
</v-switch>

<!--
方向：约 3 分钟。3 次点击。

框架在窗口满之前压缩，因为摘要需要空间。

点击。一份摘要保留决策并丢失细节，所以必须精确的内容属于文件。

点击。缓存奖励一个只附加的框架：稳定的部分在前，新内容在末尾。

点击。对过去的每一次编辑都重新付费：更改的工具、不同的模型、一份摘要。
-->

---
layout: none
class: story
---

# 这周

<v-switch>
  <template #0><img class="story-still" src="./assets/slides/deck-week-1.png" /></template>
  <template #1><img class="story-still" src="./assets/slides/deck-week-2.png" /></template>
</v-switch>

<!--
方向：约 2 分钟。1 次点击。

Aider 只摘要完成的轮次。你构建的循环运行在一个轮次内，所以它的消息从未被摘要，并且它们随每个工具轮次增长。

点击。每个工具轮次应该附加。将调用及其结果添加到消息的末尾，永远不要重建已经存在的内容，缓存可以做它的工作。
-->

---
layout: none
class: story
---

# 来源

<img class="story-still" src="./assets/slides/deck-sources-0.png" />

<!--
方向：无点击。在问题开始时保持它。

Claude Code 文档关于提示缓存和什么在压缩中幸存，《构建 Claude Code 的教训》，这是 Anthropic 关于围绕缓存构建它的文章，以及 Anthropic 和 OpenAI 的提示缓存文档。
-->

---
layout: none
class: story
---

# 更多来源

<img class="story-still" src="./assets/slides/deck-sources-2-0.png" />

<!--
方向：无点击。

Codex 压缩提示词的源码，Aider 聊天摘要器的源码，以及 Anthropic 关于有效上下文工程的文章。
-->

---
layout: none
class: story
---

# 问题

<img class="story-still" src="./assets/slides/deck-questions-0.png" />

<!--
方向：剩余的时间。无点击。

问题。
-->
