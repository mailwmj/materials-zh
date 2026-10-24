---
theme: ../theme
title: "L04: Spec-Driven Development"
info: |
  EECS 498 AASE — 第04讲
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 规格驱动开发

## 第04讲 · 2026年9月10日

<!--
在开始构建之前的最后一个技巧。今天：IDK + 规格说明 + 架构师模式。
今天之后，他们拥有了Lab01和后续构建所需的一切。红色
-->

---
layout: default
---

<div class="label">当前位置</div>

# 今天：提示词杠杆

<div class="mt-6 space-y-2">

- L03 给了你三个杠杆和一个指令：要具体
- 仍占主导地位的失败原因是缺少上下文
- 结构良好的提示词也会改变你管理上下文的方式

</div>

<div class="caption mt-6">具体是那个指令。今天它变成了一种纪律。</div>

<!--
不要重新讲解三大杠杆。一句话带过，然后继续。第三个要点是
值得强调的那个：一个规格说明会定义它自己的文件边界，所以编写规格说明本身就是上下文管理，
无论你是否这样称呼它。
-->

---
layout: default
---

<div class="label">四件事</div>

# 今天你带走的东西

<v-clicks>

- 精确提示词的词汇
- 将提示词扩展到单个请求之外的结构
- 何时值得编写规格说明，何时不值得
- 架构师模式，一次性运行规格说明

</v-clicks>

<div class="caption mt-6">这里的所有内容都面向 Lab01 和随后的构建。</div>

<!--
四次点击，四件事，对任何一个都不展开详细说明。每个要点在后面都会
有属于自己的模块。这张幻灯片的存在是为了让学生在讲座转向时
知道自己在什么位置。
-->

---
layout: section
---

# IDK

## 信息密集关键词

<!--
分隔页。说出一次缩写的全称。IDK 不是一个他们已经
接触过的术语，而且它即将出现在每一张幻灯片上。绿色
-->

---
layout: two-col
---

## 模糊

```text
Add another way
to output the data.
```

<div class="text-sm opacity-70 mt-4">模型猜测每一个细节。</div>

::right::

## 富含 IDK

```text
CREATE output_format.py with
  format_as_html(...) -> str
UPDATE main.py: ADD html
  to --output-format choices
```

<div class="text-sm opacity-70 mt-4">没有什么需要猜测。通常一次就对了。</div>

<!--
同样的代码库，同样的上下文，同一个任务的两个提示词。

读左边的模糊提示词，然后列出模型必须猜测的内容：一个文本文件
而不是 HTML，没有 CLI 变更，一个本应属于函数的 print()。这些猜测中的任何一个
都可能是错的。

然后是右边，那里没有什么需要猜测的。

这句话值得记住：这是一个你不断用不同词语重新运行
的提示词和一个一次就成功的提示词之间的区别。
-->

---
layout: default
---

<div class="label">动作关键词</div>

# 词汇表

<div class="grid grid-cols-2 gap-4 mt-6 items-stretch">
  <div class="card">
    <div class="flex items-center gap-3 mb-2">
      <ph-plus-circle-bold class="text-2xl text-amber-600 shrink-0" />
      <div class="font-semibold">CREATE · ADD</div>
    </div>
    <div class="text-sm opacity-70">CREATE 创建不存在的东西。ADD 向已存在的东西追加内容。</div>
  </div>
  <div class="card">
    <div class="flex items-center gap-3 mb-2">
      <ph-pencil-simple-bold class="text-2xl text-blue-600 shrink-0" />
      <div class="font-semibold">UPDATE · REPLACE</div>
    </div>
    <div class="text-sm opacity-70">UPDATE 原地修改。REPLACE 替换实现。</div>
  </div>
  <div class="card">
    <div class="flex items-center gap-3 mb-2">
      <ph-minus-circle-bold class="text-2xl text-blue-600 shrink-0" />
      <div class="font-semibold">DELETE · REMOVE</div>
    </div>
    <div class="text-sm opacity-70">DELETE 移除整个东西。REMOVE 移除一行或一个参数。</div>
  </div>
  <div class="card">
    <div class="flex items-center gap-3 mb-2">
      <ph-copy-bold class="text-2xl text-blue-600 shrink-0" />
      <div class="font-semibold">MOVE · MIRROR</div>
    </div>
    <div class="text-sm opacity-70">MOVE 重新定位代码。MIRROR 将模式复制到新文件中。</div>
  </div>
</div>

<div class="caption mt-4">成对才是重点。ADD ≠ CREATE。</div>

<!--
四对，八个动词。读这些配对，不要逐个定义每个动词。

CREATE 和 ADD 是那对重要的，这也是为什么它是琥珀色卡片。
ADD 追加到已存在的东西，CREATE 生成全新的。选错了
模型就会做错事。
-->

---
layout: statement
---

`[动作] [位置]: [细节]`

<!--
这个模式。可重复。紧凑。可预测。黄色
-->

---
layout: default
---

<div class="label">示例</div>

# 实践中的模式

```text
CREATE main.py with
  analyze_transcript(filepath: str) -> TranscriptAnalysis

UPDATE output_format.py: ADD
  format_as_yaml() after format_as_json()

DELETE main.py: REMOVE
  the unused print_debug() function
```

<!--
三个示例，作为指令而不是幻灯片上的文本阅读。

重点是成本。这个模式写起来比一段段落更短，不是
更长，而且通常一次就成功，而不是花费三轮
迭代。
-->

---
layout: default
---

<div class="label">IDK 为何有效 — L02 回调</div>

# 缩小采样分布

- 模糊提示词 → 宽分布 → 模型选择训练先验
- 精确提示词 → 窄分布 → 模型选择你说的内容
- IDK 有效是因为*机制*，而不是英语流利度

<!--
L02 的回调，以及这张阻止 IDK 听起来像风格
偏好的幻灯片。自回归采样：模糊提示词使分布
保持宽阔，所以模型选择训练数据中最常见的补全。
精确提示词缩小了它，所以模型选择符合你约束的内容。

清楚地说出最后一点。IDK 有效是因为机制，而不是因为
模型更好地理解了英语。
-->

---
layout: section
---

# 提示词层级

## 高 · 中 · 低

<!--
分隔页。高、中、低，然后前进
-->

---
layout: default
---

<div class="label">三个层级</div>

# 在哪里操作

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-cloud-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">高</div>
    <div class="text-sm opacity-70 flex-1">"让它更好。"自然，但有风险。</div>
  </div>
  <div class="card">
    <ph-target-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">中 · 甜蜜点</div>
    <div class="text-sm opacity-70 flex-1">"UPDATE chart.py: word_count_bar_chart() — 上四分位数绿色，降序排列。"</div>
  </div>
  <div class="card">
    <ph-keyboard-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">低</div>
    <div class="text-sm opacity-70 flex-1">"第47行，把 X 替换成 Y。"冗长，像打字员。</div>
  </div>
</div>

<div class="caption mt-6">甜蜜点随模型变化。前沿模型容忍更高层级；4B 模型需要更详细。</div>

<!--
甜蜜点用琥珀色——它是幻灯片的重点。

标题是校准点：不要给他们一个固定的层级， handed
他们养成注意今天模型需要哪个层级的习惯。橙色
-->

---
layout: section
---

# 计划 = 提示词

## 规划瓶颈

<!--
分隔页。命名它并前进。论点在下一张幻灯片，所以不要
在这里开始阐述。
-->

---
layout: default
---

<div class="label">2024 vs 2026</div>

# 瓶颈已经转移

<v-clicks>

- **2024**: AI 能理解我想要什么吗？
- **2026**: 我*能*说清楚我想要什么吗？
- 输出质量受限于计划质量
- AI = 执行者。你 = 架构师。
- 人类承包商在规格说明模糊时会提问。LLM 会猜测。

</v-clicks>

<!--
整个论点，五次点击。2024 问的是 AI 能否理解
你。2026 问的是你能否说清楚你想要什么。

点击四是主张。AI 是执行者，你是架构师，而
规划是工程工作。

点击五是定价的不对称之处。得到模糊规格说明的人类承包商
会问澄清问题。LLM 不会，它会猜测。所以
歧义在这里的成本比与人合作时更高，而编写精确
规格说明不是额外的工作。这就是工作本身。

下一张陈述幻灯片是这张的回报。不要在这里说它。
-->

---
layout: statement
---

规格说明**就是**提示词。

<!--
前一张幻灯片的回报。不是你在提示之前生产的工件。规格说明就是指令。

说出来，然后停止说话。让它沉淀。
-->

---
layout: default
---

<div class="label">决策规则</div>

# 何时使用规格说明

<div class="grid grid-cols-2 gap-6 mt-6 items-stretch">
  <div class="card">
    <ph-check-bold class="text-3xl text-emerald-600 mb-3" />
    <div class="font-semibold mb-1">是</div>
    <div class="text-sm opacity-70 flex-1">3+ 文件协调 · 相互依赖的步骤 · 可复现性 · 从零开始构建 · 归档</div>
  </div>
  <div class="card">
    <ph-x-bold class="text-3xl text-rose-600 mb-3" />
    <div class="font-semibold mb-1">否</div>
    <div class="text-sm opacity-70 flex-1">单个 bug 修复 · 单函数重构 · "既然你在这里，也把 X 做了"</div>
  </div>
</div>

<div class="caption mt-6">如果你会为人类承包商编写设计文档，就写一个规格说明。</div>

<!--
两个语义强调：翡翠色（适用）和玫瑰色（不适用）。唯一一张配得上它的幻灯片。

标题就是规则。说出来，然后让他们在休息时应用它。
-->

---
layout: default
---

<div class="label">你熟悉的代码库</div>

# taskr

<div class="mt-2">

你一直生活在其中的任务管理器。`task.py` 是记录，`store.py` 是磁盘上的 JSON，`cli.py` 是命令。目前它只能 `add` 和 `list`。

</div>

<div class="text-sm mt-4">

- `taskr done 3` — 标记任务完成
- `taskr rm 3` — 删除任务
- `taskr list --tag work` — 按标签过滤
- `taskr add "..." --due fri` — 截止日期，在列表中显示
- `taskr list --json` — 机器可读的输出

</div>

<div class="caption mt-4">选一个，或者带你自己的。无论哪种方式，命名文件和函数。</div>

<!--
这存在是因为"想一个改动"是一个冷启动，而休息只有
五分钟。五个选项，这样没有人花其中两分钟来选择。

它们故意不同大小，这就是校准点。`done`
涉及所有三个文件，因为任务还没有 done 字段：那就是
高层提示词失效的那个。`list --json` 是一个函数，而且
`to_dict()` 已经存在，所以它能承受高得多的层级。

如果房间需要一个推动，大声说出来："你能把层级提到多高"
的答案几乎总是模型。
-->

---
layout: statement
---

<div class="label">聊天休息 · 5 分钟</div>

选一个你想对 `taskr` 做的改动。用 动作 位置: 细节 的格式写出来，然后是*仍然一次成功的最高层级*。

<!--
0:35。固定的休息时间。完整提示词在 overview.md 的 chat_break 字段中。

 purposeful taskr：两周的 aider 练习意味着他们每个人都认识
cli.py、store.py 和 task.py，并且已经针对它们校准了一个模型。没有人被不熟悉的代码
卡住，这是五分钟的休息无法承受的。

把他们推向仍然有效的最高层级，而不是最安全的。
重点是校准，"什么推动了你的层级"是值得在 0:39 大声问出的问题：答案几乎总是模型。

他们今晚可以在 Aider 中运行它，这就是为什么它是 taskr 而
不是某个假设的东西的全部原因。
-->

---
layout: section
---

# PollEv

<div class="mt-5 font-mono text-2xl text-blue-600">https://PollEv.com/marcusdarden365</div>

<!--
URL 在幻灯片上，这样后排的学生可以不用询问就加入。在回答期间保持显示。

它是一个 div 而不是通常的 section 副标题是有原因的：主题
会将它们大写，而大写的 URL 路径不是同一个路径。不要
把它"修复"回 h2。

讲师提示：切换到浏览器中保存活动的标签页，设置
为实时，朗读问题和所有四个选项，给大约 60
秒，展示结果，然后回到幻灯片。

这一次计入出勤，不像 L01 的测试运行。说出来。

问题、选项、答案以及结果出来后该说的话在
assets/poll-everywhere.md。这是 L02 的海中捞针，答案
是他们即将被教授的上下文部分的论点。
-->

---
layout: section
---

# 规格说明结构

## 五个部分

<!--
分隔页。五个部分。前进。
-->

---
layout: default
---

<div class="label">结构</div>

# 规格说明文档

<v-clicks>

1. **高层目标** — "是什么"和"为什么"，一般性陈述
2. **中层目标** — 3-5 个可衡量的结果
3. **实现说明** — 依赖项、标准、约束
4. **上下文** — 起始和结束文件
5. **有序的低层任务** — 富含 IDK，按依赖排序

</v-clicks>

<!--
地图，每次点击一个部分。命名每一个然后继续，它们会在
接下来的三张幻灯片上展开。

值得大声说出来：这是一个 markdown 文件，不是工具或表单。
他们今天可以在任何编辑器中编写一个。
-->

---
layout: default
---

<div class="label">第1到第3部分</div>

# 目标和说明

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-compass-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">高层</div>
    <div class="text-sm opacity-70 flex-1">一句话说明原因。"为转录分析 CLI 添加词频可视化和结构化文件输出。"</div>
  </div>
  <div class="card">
    <ph-check-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">中层</div>
    <div class="text-sm opacity-70 flex-1">三到五个结果，每个都可以独立验证。图表函数、输出格式化器、两个新的 CLI 标志。</div>
  </div>
  <div class="card">
    <ph-wrench-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">说明</div>
    <div class="text-sm opacity-70 flex-1">使用 matplotlib，它已经是依赖项。遵循项目中的模式。简要注释每个新函数。</div>
  </div>
</div>

<div class="caption mt-6">目标说明完成时是什么样子。说明说明模型不能猜测什么。</div>

<!--
说明是你发挥工程判断力的地方。任何你在
评审中会纠正的内容，放在这里并跳过评审轮次。
-->

---
layout: default
---

<div class="label">第4部分 · 最常被跳过的部分</div>

# 上下文：起始 + 结束

```text
### 起始上下文
- src/main.py
- src/data_types.py（只读）
- pyproject.toml（只读）

### 结束上下文
- src/main.py（已更新）
- src/chart.py（新文件）
- src/output_format.py（新文件）
```

<div class="caption mt-4">消除歧义。模型确切知道什么在范围内。</div>

<!--
最常被省略的部分和最重要的部分，这就是标签所说的。照此说明。

起始上下文是模型可以读取和编辑的内容。结束上下文是
之后应该存在什么内容。新文件标注（新文件），不可修改的标注（只读）。

这是阻止模型进入你从未打算让它打开的文件的
部分。那句话就是这个部分存在的原因。
-->

---
layout: default
---

<div class="label">第5部分 · 有序任务</div>

# 复合序列

```text
1. CREATE output_format.py with
   format_as_text/json/md/yaml

2. CREATE chart.py with
   create_bar/pie/line_chart —
   colors: top green, bottom red

3. UPDATE main.py: ADD --chart,
   --output-format, call new fns
```

<div class="caption mt-4">任务3引用任务1的输出，因为任务1先运行。</div>

<!--
按顺序读这三个任务，然后读标题。任务3可以引用
format_as_json()，因为任务1已经运行了。序列携带了
依赖关系，所以模型不必推断它们。

停留在这张幻灯片上来说明这一点。没有单独的幻灯片讲它。

用他们已经熟悉的方式收尾：先有类型，然后是使用它们的函数，然后是调用点。
-->

---
layout: section
---

# 架构师模式

## 规格说明在行动中

<!--
桥梁：他们现在有了五个部分。看一个拥有全部五个部分的规格说明
在一台笔记本上的模型上从头到尾运行。
错误出现在演示之后，而不是之前。不要在这里预览它们。
-->

---
layout: default
---

<div class="label">推理 + 编辑</div>

# Aider 中的架构师模式

```bash
aider --architect \
      --model ollama_chat/qwen3.5:9b \
      --editor-model ollama_chat/qwen3.5:9b
```

- **架构师** — 用散文起草计划
- **编辑器** — 将计划转化为编辑
- `auto-accept-architect: false` — 计划停止。阅读它。

<div class="caption mt-4">两个角色用同一个模型。演示的配置设置了全部三个，所以在屏幕上你会看到只是 <code>aider src/main.py src/arg_parse.py</code>。</div>

<!--
在他们形成误解之前纠正它：两个角色，不是两个模型。同一个
模型担任两个角色没问题，在笔记本上这通常就是你想要的。一个小模型
做一项工作明显比做两项工作更好，这就是全部的优势所在。9B 架构师配 4B 编辑器是一个值得做一次的实验，
不是一个要求。

第三点是需要强调的。aider-practice 第6课有意将其设为 false，
所以计划停下来等待。磁盘上还没有任何内容，所以这是
工作流中最便宜的地方来捕捉误解了你的模型。

标记它然后继续：计划，然后行动，然后检查是一个循环。他们在第5周写一个。
-->

---
layout: default
---

<div class="label">现场演示</div>

# 看它运行

<v-clicks>

1. `spec.md` 在屏幕上。走过五个部分。
2. `aider src/main.py src/arg_parse.py` — 架构师模式，无标志。
3. 粘贴规格说明。架构师用散文写一个计划。
4. **大声朗读计划，然后按 `y`。** 磁盘上还没有任何内容。
5. 四个文件：`output_format.py`，`chart.py` 新文件；`arg_parse.py`，`main.py` 已更新。
6. `uv run main transcript.txt --chart bar --output-format json`

</v-clicks>

<!--
六次点击。步骤4是节拍，也是你不能剪掉的。

在启动任何东西之前展示规格说明并走过它的五个部分。否则
房间里的学生在不知道要求了什么的情况下看着输出滚动。
将横幅与另一屏幕上的规格说明对比。两个文件可编辑，
五个只读，那就是他们差点跳过的上下文部分。
Aider 实现（只读）；这不是给你的笔记。

步骤4：磁盘上什么都没有。如果它误解了你，这只需要一个
按键。编辑之后它需要一个 /undo 和四分钟的模型时间。

如果 aider 问"Add file to the chat?"，回答 S。默认是 Yes 和 Yes
会重新运行架构师，这会丢失计划。

如果它卡住了，切换到录像。不要在房间里调试 Python。完整的运行手册：assets/demo-runbook.md。
-->

---
layout: section
---

# 常见错误

## 在你写自己的之前

<!--
桥梁：你已经看过一个干净的规格说明运行了。在你写自己的之前，评分者 consistently 看到的四件事出了问题。
-->

---
layout: default
---

<div class="label">常见规格说明错误</div>

# 评分者看到的情况

<v-clicks>

- **模糊的中层目标** — "让它更好"悄悄溜进来了
- **缺少上下文部分** — 模型触碰了它不应该触碰的文件
- **任务顺序错误** — 任务1引用任务3
- **任务中的代码** — 实现，而不是指令

</v-clicks>

<div class="caption mt-6">规格说明是沟通工件。这样对待它们。</div>

<!--
四次点击，清楚地说你会看到全部四个。

指令该写代码的地方是实现，这是最常见的。他们在
任务中写入了实现而不是描述它。

标题是框架。一个规格说明是一个沟通工件，所以测试是
另一个人是否能执行它。
-->

---
layout: default
---

<div class="label">回顾</div>

# 带走什么

<v-clicks>

- **IDK** 命名了动作和位置，这缩小了采样分布
- **中层** 是甜蜜点，甜蜜点随模型移动
- **计划 = 提示词。** 规格说明是指令，不是关于它的文档
- **有序任务** 复合，所以后面的步骤依赖前面的步骤
- **架构师模式** 将规划与编辑分开。一次一个工作
- **规格说明在三个或更多协调文件上才值得花费成本**

</v-clicks>

<!--
六次点击，每行一个，不做展开。他们已经听过一次了。
这是他们写下来的版本。

如果时间短，点击通过并只强调第三个：规格说明
是指令，不是关于它的文档。
-->

---
layout: default
---

<div class="label">给房间的</div>

# 三个问题

<div class="mt-6 space-y-3">

1. 规格说明和一个非常长的提示词有什么区别？
2. 你会为一个 bug 修复写规格说明吗？它什么时候变得过度？
3. 为什么把"注释每个函数"放在规格说明里，而不是指望它？

</div>

<!--
如果时间允许，大声回答三个中的两个。问题3是
值得强调的那个：你不写下来的标准是模型
从未拥有的标准。
-->

---
layout: default
---

<div class="label">本周</div>

# Lab01 + 项目构建

<v-clicks>

- **明天（周五 9月11日）** — Aider 设置门控 + 全部十六节课
- **9月14日周一 / 9月15日周二** — Lab01， pivot 实验课
- **10月6日周二** — 项目构建截止，晚上11:59
- **还没有** — `aider-practice` 本周结束；项目构建在 Lab01 之后开始

</v-clicks>

<!--
日期，清楚地说一次。设置门控和全部十六个 aider-practice
课程明天到期，2026年9月11日星期五。Lab01 是 2026年9月14日周一
对于周一班次，2026年9月15日周二对于周二班次。项目
构建截止于 2026年10月6日周二晚上11:59。

最后一次点击防止一周的困惑。他们还没有开始构建。
aider-practice 本周结束，项目构建在 Lab01 之后开始。
-->

---
layout: default
---

<div class="label">项目构建 · 10月6日周二截止</div>

# 七个特性

<div class="mt-2">一个终端编码助手：对话、上下文中的文件、模型编写的编辑应用在 diff 之后。</div>
<div class="text-sm mt-3">

| | 所需行为 |
|---|---|
| **F1** | 对话、预算处理、流式回复 |
| **F2** | 添加、删除、列出和清除上下文 |
| **F3** | 当前文件内容，一致渲染 |
| **F4** | 你测试过的编辑块格式提示词 |
| **F5** | 带有有用错误的解析 |
| **F6** | 精确编辑、完整预览、显式批准 |
| **F7** | 每个接受的编辑集一个提交，受保护的撤销 |

</div>

<!--
说出幻灯片没有空间的那句话：Aider 的脊柱，不是它的
功能列表，每个人都构建相同的七个。

房间里的大多数人还没有看过这个列表。它昨天在
Lab01 幻灯片中公开，没有任何东西指向它，所以大声读出全部七个
而不是假设。下一张幻灯片说今天直接贡献了七个中的哪两个，所以
这是它的设置，不是绕路。

描述是它们首先需要的部分：他们构建的是他们已经驾驶了两周的东西。F2 是 /add 和 /drop，这是 L03 的演示，
F4 是为一个小模型写的规格说明，这是下一张幻灯片。

表格与 Lab01 的完全匹配，这是刻意的。带有检查行为的完整规格说明在他们克隆的数据包仓库中。
-->

---
layout: default
---

<div class="label">两者都评分</div>

# 这贡献给什么

<div class="grid grid-cols-2 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-package-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">你项目构建的规格说明文档</div>
    <div class="text-sm opacity-70 flex-1">七个特性且没有 starter 代码是"三个或更多文件，从零开始"的情况。规格说明是必需的交付物。</div>
  </div>
  <div class="card">
    <ph-code-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">特性 F4，编辑块提示词</div>
    <div class="text-sm opacity-70 flex-1">为一个小模型写的规格说明。让它模糊，你的解析器就会拒绝输出，所以没有任何东西落在磁盘上。</div>
  </div>
</div>

<div class="caption mt-6">今天的一切即将变成你提交的代码。</div>

<!--
两张卡片都是评分作业，这就是标签。先说这一点。

左卡片：七个特性且没有 starter 代码正是决策规则中的三个或更多文件，从零开始构建的情况，所以规格说明是必需的交付物。明确说明数据包不提供应用程序代码，没有模块布局和没有测试。

右卡片是值得花时间的那个。F4 是告诉模型如何发出编辑的指令。那就是为一个小的模型受众写的规格说明，也是整个学期中最不宽容的一个。让它模糊，解析器拒绝输出，没有任何东西落在磁盘上。

然后提供：这周带着规格说明草稿来 office hours。第一个不会完美，这没关系。
-->

---
layout: default
---

<div class="label">下节课</div>

# 现代智能体 CLI

<div class="mt-6 space-y-2">

- 你今天写的同一个规格说明，交给 Claude Code 和 Codex
- 讲台上的演示。不需要账户，没有人被要求购买任何东西
- 更多的自主性，不同的行为，而且三大杠杆仍然适用

</div>

<div class="caption mt-6">你即将构建的东西的成熟版本。周二。</div>

<!--
清楚并早早地说出不需要账户这句话。有人已经在想
他们是否需要购买什么东西。这是讲台上的演示。

同样的规格说明，更自主的工具，不同的行为，而且三大杠杆仍然
适用。他们正在观看的是他们即将构建的东西的成熟版本。
-->

---
layout: end
---