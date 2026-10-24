---
theme: ../theme
title: "L01：课程介绍 + AI 编程版图"
info: |
  EECS 498 AASE —— 第 01 讲
  Applied Agentic Software Engineering，UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 课程介绍 + AI 编程版图

## 第 01 讲 · 2026 年 9 月 1 日

<!--
标题页。气氛打开。今天是第一天。
讲座脉络：定向 → 落地理念 → 讨论 → 然后（且仅然后）工具。
-->

---
layout: statement
---

微笑。

<!--
第一拍。仅一个字。让他们真的做——等着看。
具身认知：姿态和表情能提升认知表现。
-->

---
layout: statement
---

微笑。*你属于这里。*

<!--
第二拍，追加。现在给出理由。
邻里肯定：转过身告诉他们。然后继续。
-->

---
layout: default
---

<div class="label">路线图 · 80 分钟</div>

# 今天的内容

<v-clicks>

- **一个目标，三个动词** —— 本学期我们将做什么
- **课程如何运行** —— 资源在哪里、评分方式、课程要求
- **AI 编程版图** —— 发展历程、2026 年的现状
- **代码策展人理念** —— 思维模型
- **讨论** —— 两个问题
- **工具** —— Aider，以及它带来的感受
- **本周内容** —— `aider-practice` + 环境检查 + 你要构建的东西

</v-clicks>

<!--
落地脉络。注意：工具排在最后。我们通过先讲原则来"赚取"使用工具的资格。
-->

---
layout: section
---

# 课程形态

## 应用 · 分析 · 创造

<!--
过渡：既然我们已经明确了目标，接下来是 15 周的课程结构。
-->

---
layout: default
---

<div class="label">课程脉络 · 15 周</div>

# 一个目标。三个动词。

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-user-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">应用 · 第 1–3 周</div>
    <div class="text-sm opacity-70 flex-1">使用编程智能体构建一个项目。用 Aider 连接你自己的 endpoint → 成为结对程序员。</div>
  </div>
  <div class="card">
    <ph-code-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">分析 · 第 4–7 周</div>
    <div class="text-sm opacity-70 flex-1">让自己脱离循环。工具、停止条件，然后测量哪里出了问题。</div>
  </div>
  <div class="card">
    <ph-tree-structure-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">创造 · 第 8–15 周</div>
    <div class="text-sm opacity-70 flex-1">将其打造成助手。记忆、方法，以及一种不必依赖终端就能访问它的方式。</div>
  </div>
</div>

<!--
创造部分用琥珀色：那是终点，是幻灯片的"高光时刻"。
大声念出这句话：从第 1 周到第 15 周，这全部是一个代码库。
-->

---
layout: section
---

# 课程运行方式

## 网站 · 工作人员 · 成绩 · 日历

<!--
节奏加快。教学大纲承载细节；这是地图。但在诚信和黑客松晚上要放慢速度。
-->

---
layout: default
---

<div class="label">五个地方</div>

# 资源位置

|| | |
|---|---|
|| **课程网站** `eecs498-aase.github.io` | 教学大纲 + 日程。日期和政策以此为准 |
|| **Ed** `edstem.org` | 提问 **和公告**。这是我们联系你的地方 |
|| **GitHub** `aase-student` | 你收到的所有内容。每个练习对应你自己的代码库。**请接受邮件中的邀请** |
|| **Canvas** | 成绩 |
|| **播客**（可选） | 每节课前有一个 Brief，课后有一个 Deep Dive |

<div class="caption mt-6">讲座会被录制（CAEN + Zoom）。播客节目由课程材料 AI 生成，使用 <em>去识别化</em> 的逐字稿。不会出现任何人名。想删除某段内容？发送邮件到工作人员地址，我们会处理。</div>

<!--
 Plainly 说明录制政策，不要隐瞒。Ed 是他们必须实际查看的平台——播客公告已经在那里了。
-->

---
layout: default
---

<div class="label">工作人员 · 一个邮箱</div>

# 你面对的人

- **Marcus Darden** —— 授课教师
- **Rafe Symonds**，**Zilin Wang** —— 研究生教学助理
- **Rushil Kagithala** —— 助教

<div class="mt-6">

`eecs-aase-staff@umich.edu` 可以联系到以上所有人。

</div>

<div class="caption mt-4">个人事务（延期、特殊照顾、硬件问题）→ 发邮件。全班都能受益的内容 → 发到 Ed。</div>

<!--
答疑时间从本周开始；确认的时间和地点会发布在 Ed 上。
-->

---
layout: default
---

<div class="label">评分 · 无笔试</div>

# 评分方式

|| | 占你成绩的比例 |
|---|---|
|| **行政分** —— 讲座 + 实验课出勤，加分项 | 10% |
|| **应用** —— 第 1–3 周，构建到第 4 周末 | 18% |
|| **分析** —— 第 4–7 周 | 22.5% |
|| **创造** —— 第 8–15 周 | 49.5% |

<div class="mt-6">

其余 90% 按 **20 / 25 / 55** 的比例分配。

</div>

<!--
"关卡"框架很重要：它消除了"我及格了吗"的焦虑，并让提前尝试成为最自然的选择。
-->

---
layout: default
---

<div class="label">日历</div>

# 28 场讲座 · 13 节实验课 · 3 个晚间活动

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-chalkboard-teacher-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">讲座</div>
    <div class="text-sm opacity-70 flex-1">周二 + 周四，3:00–4:30，LCSIB 2365</div>
  </div>
  <div class="card">
    <ph-wrench-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">实验课</div>
    <div class="text-sm opacity-70 flex-1">周一 3:30（GGBL 2153）或周二 12:30（DOW 1005）。内容相同，任选一个参加</div>
  </div>
  <div class="card">
    <ph-moon-stars-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">黑客松</div>
    <div class="text-sm opacity-70 flex-1"><strong>晚间</strong>，全体参加，第 4 · 7 · 15 周。第一个：<strong>9 月 24 日周四</strong></div>
  </div>
</div>

<div class="caption mt-6">今晚就把 9 月 24 日放进日历。如果某个晚上你真的无法参加，尽早告诉我们，我们会安排补做方案。</div>

<!--
现在就落实晚间活动的承诺。教室里有人有工作或有孩子，需要三周的警告，而不是三天。
-->

---
layout: default
---

<div class="label">课程要求</div>

# 简短版

- **出勤** —— 占你成绩的 **10%**，60% 讲座 / 40% 实验课。使用 Poll Everywhere，应用或浏览器均可。**达到 70% 即得满分**
- **迟交作业** —— 每天扣 10%，最多 3 天，之后为零。没有宽限日
- **独立完成** —— 可以自由讨论想法，但绝不分享代码或提示词
- **费用** —— *零*。没有教科书，没有 LLM 开销。如果你的笔记本无法运行模型，CAEN 会提供。详情见 Ed

<!--
费用用衬线体强调：这是让本地模型政策显得公平的承诺，也是为什么 nobody 需要订阅。
-->

---
layout: statement
---

如果你能*解释*它，它就是你的。

<!--
今天的重点幻灯片。放慢速度。
AI 使用在这里是必需的，不是被容忍的。被评分的是理解能力：
能解释你提交的任何一行代码，记录工具/模型/提示词。如果你无法解释它，它就不是你的——这涉及荣誉准则。
-->

---
layout: section
---

# 我们的位置

## 2026 年秋季

<!--
过渡：在我们继续深入计划之前，先确定我们在行业中的位置。
-->

---
layout: default
---

<div class="label">四波浪潮</div>

# 简史

<v-clicks>

- **第一波 —— 自动补全**（2021–22）。GitHub Copilot。一个文件，你来驾驶。
- **第二波 —— 聊天**（2022–23）。ChatGPT，然后是 IDE 内的聊天。提问、粘贴、调整。
- **第三波 —— 智能体工具**（2023–25）。Cursor 和 Aider，然后是 Claude Code 和 Codex。多文件编辑；工具运行命令。
- **第四波 —— 编排**（2025–至今）。多个智能体，长时间运行，有人监督。这是我们创造阶段的目标。

</v-clicks>

<!--
自动补全早于第一波——Kite、TabNine——但 Copilot 才是它成为关键的时刻，所以时钟从那里开始。
大多数学生是在第三波进入的。我们将在 12 月前进入第四波。
-->

---
layout: statement
---

 hype 很陡峭。现实*更有趣*。

<!--
诚实时刻。AI 并没有取代程序员。一类新的工作流程出现了。
学会与 AI 合作的开发者的生产力大幅提升。
-->

---
layout: section
---

# 休息五分钟

## 和邻座聊聊

<!--
过渡：暂停一下。在我们叠加理念之前，先将其与你自己的经验联系起来。
-->

---
layout: default
---

<div class="label">5 分钟 · 邻座交流</div>

# 交流心得

<div class="mt-6 text-base">

> 回想一下你最近一次用 AI 工具写代码的时候——或者你最近一次*没有*用 AI 写代码的时候。什么感觉有成效？什么感觉令人沮丧？当你完成时，你是觉得*学到*了什么，还是只是*得到了一个答案*？

</div>

<div class="caption mt-6">这个问题贯穿本课程的所有内容。</div>

---
layout: section
---

# 代码策展人

## 理念

<!--
过渡：保留你和邻座讨论的内容。这是一个重新定义它的思维模型。
-->

---
layout: two-col
---

## 打字员

<div class="mt-6 space-y-4">
  <div class="flex gap-3 items-start">
    <ph-keyboard-bold class="text-3xl text-blue-600 mt-1 flex-shrink-0" />
    <div>
      <div class="font-semibold">编写代码行</div>
      <div class="text-sm opacity-70">语法、格式、模板代码。</div>
    </div>
  </div>
  <div class="flex gap-3 items-start">
    <ph-clock-bold class="text-3xl text-blue-600 mt-1 flex-shrink-0" />
    <div>
      <div class="font-semibold">花时间在"怎么做"上</div>
      <div class="text-sm opacity-70">实现细节。</div>
    </div>
  </div>
  <div class="flex gap-3 items-start">
    <ph-user-bold class="text-3xl text-blue-600 mt-1 flex-shrink-0" />
    <div>
      <div class="font-semibold">拥有每一次击键</div>
      <div class="text-sm opacity-70">"正确"意味着"我正确地输入了它。"</div>
    </div>
  </div>
</div>

::right::

## 策展人

<div class="mt-6 space-y-4">
  <div class="flex gap-3 items-start">
    <ph-compass-bold class="text-3xl text-amber-600 mt-1 flex-shrink-0" />
    <div>
      <div class="font-semibold">明确意图</div>
      <div class="text-sm opacity-70">做什么和为什么，而不是怎么做。</div>
    </div>
  </div>
  <div class="flex gap-3 items-start">
    <ph-magnifying-glass-bold class="text-3xl text-amber-600 mt-1 flex-shrink-0" />
    <div>
      <div class="font-semibold">评估输出</div>
      <div class="text-sm opacity-70">审查、重定向、接受。</div>
    </div>
  </div>
  <div class="flex gap-3 items-start">
    <ph-shield-check-bold class="text-3xl text-amber-600 mt-1 flex-shrink-0" />
    <div>
      <div class="font-semibold">拥有结果</div>
      <div class="text-sm opacity-70">"正确"意味着"它能用，我能为它辩护"——不管是谁输入的。</div>
    </div>
  </div>
</div>

<!--
理念的转变。同一个工程师，不同的时间分配。
-->

---
layout: statement
---

**KISS**：保持提示词聚焦；抵制过度解释的冲动。

<!--
不是网络协议。本课程中的 KISS = 清晰胜于冗长。
-->

---
layout: section
---

# PollEv

<!--
仅 presenter 提示——切换到持有活动的浏览器标签页。
将其上线， aloud 读出问题和所有四个选项，给大约 60 秒，
展示结果，然后回到幻灯片。
-->

---
layout: two-col
---

## 不好

<div style="min-height:200px">
<div class="text-sm opacity-60 mb-1">写新代码</div>

```text
Please could you help me write
a Python function that iterates
through a list of integers,
checks if each one is even,
and if it is even adds it to
a new list... actually I think
a list comprehension is cleaner
but I'm not sure...
```

</div>

<div class="text-sm opacity-60 mb-1">追踪错误</div>

```text
The tests are failing. Something
with dates? Or the timezone
maybe? Can you look at it and
just make it work.
```

::right::

## 好

<div style="min-height:200px">
<div class="text-sm opacity-60 mb-1">写新代码</div>

```text
Write a function that extracts
the even numbers from a list.
```

</div>

<div class="text-sm opacity-60 mb-1">追踪错误</div>

```text
test_parse_date fails on
"2026-02-30". Make parse_date
raise ValueError on invalid
dates instead of returning None.
```

<!--
B 几乎总是产生更好的结果。模型会推断其余部分。
添加限定条件会叠加歧义，而不是解决它。
-->

---
layout: default
---

<div class="label">预览 · L03</div>

# 三大要素

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-files-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">上下文</div>
    <div class="text-sm opacity-70 flex-1">AI 开始工作时知道什么？</div>
  </div>
  <div class="card">
    <ph-cpu-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">模型</div>
    <div class="text-sm opacity-70 flex-1">用哪个 AI？能力 vs 成本 vs 速度。</div>
  </div>
  <div class="card">
    <ph-chat-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">提示词</div>
    <div class="text-sm opacity-70 flex-1">如何明确表达你想要什么？</div>
  </div>
</div>

<div class="caption mt-6">L03 会深入讲解。今天只是预览。</div>

---
layout: section
---

# 讨论

## 两个问题

<!--
过渡：我们已经明确了原则。两个问题来检验它。
-->

---
layout: default
---

<div class="label">每个问题一分钟 · 然后我们讨论</div>

# 两个问题

<v-clicks>

1. 如果 AI 能写代码，你为什么还需要 EECS 281？
2. 你什么时候*不*想用 AI 写代码？

</v-clicks>

<!--
Q1 → 计算机科学基础知识仍然重要；你评估输出。
Q2 → 学习、安全关键、深度维护。
-->

---
layout: section
---

# 工具

## 为什么是 Aider —— 目前

<!--
过渡：我们已经讨论了原则。现在看工具——而且仅仅是因为原则在前。
-->

---
layout: default
---

<div class="label">工具选择 · 教学考虑</div>

# 为什么在应用阶段用 Aider？

<v-clicks>

- **三大要素都是显式的。** 没有 GUI 隐藏上下文、模型或提示词决策。
- **支持 OpenAI 兼容 API。** 所以它能与*你*提供的模型对话。
- **足够小，可以重建。** 而且你将在第 3 周开始这样做。

</v-clicks>

<!--
强调原因 2：你的模型是一行配置，而不是一个依赖。
这就是本课程免费的原因。
Claude Code 将在 L05 作为演示出现。无需账户。 Plainly 说明。
-->

---
layout: section
---

# AI 结对编程

## 第一印象

<!--
过渡：几分钟——不是教程——感受一下 AI 结对编程的实际体验。
环境设置在 Setup Lab；命令是 `aider-practice`。今天，只需感受这个循环。
-->

---
layout: default
---

<div class="label">3 个提示词 · 约 5 分钟</div>

# 这个循环的感受

```text
Prompt 1 → "Refactor parse_args() to handle no-args case
            cleanly; print usage and exit."

Prompt 2 → "Add a unit test for the no-args path."

Prompt 3 → "Replace the if-chain in dispatch() with a
            dictionary lookup."
```

<div class="caption mt-4">三个协调的改动。测试通过。我一行代码都没有写。</div>

---
layout: statement
---

我描述了结果。模型产生了它们。我*做出判断*。

<!--
这就是协作形态。不是命令——是形态。
-->

---
layout: section
---

# 本周内容

## `aider-practice` · 环境检查 · 构建项目

<!--
过渡：设置靠你自己。以下是本周的具体安排。
-->

---
layout: default
---

<div class="label">课后 · aider-practice · 十六个指导课程</div>

# 每节课都是一个失败的测试

<v-clicks>

- **第 1 部分，第 1–10 课** 本周完成 —— 在 `qwen3.5:4b` 上构建 `taskr` 的命令行
- **第 2 部分，第 11–16 课** 下周完成 —— 在同一数据上构建 Web 前端。推荐使用 `9b`，但不是必需的
- 你驾驶 Aider 直到测试通过。*你永远不需要怀疑自己是否完成了*
- **环境检查 + 全部十六节课截止日期：9 月 11 日周五**
- **Lab01** 9 月 14 日周一 / 9 月 15 日周二 —— 转折实验课

</v-clicks>

<div class="caption mt-6">你在 Setup Lab 中已经运行了 Aider。之后卡住了？答疑时间——我们任何人都可以帮你，不只是我。时间和地点周五前发布在 Ed 上。</div>

---
layout: default
---

<div class="label">构建项目 · 今天定形状，L03 出规格</div>

# 你的第一个项目

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-package-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">你要构建什么</div>
    <div class="text-sm opacity-70 flex-1">一个**结对程序员**。每个人都构建同样的东西：**七个固定功能**，仅凭我们给你的规格从零开始编写</div>
  </div>
  <div class="card">
    <ph-clock-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">时间</div>
    <div class="text-sm opacity-70 flex-1">在**第 3–6 周**内构建。截止日期：**10 月 6 日周二，11:59 PM**</div>
  </div>
  <div class="card">
    <ph-check-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">你要提交什么</div>
    <div class="text-sm opacity-70 flex-1">通过**功能关卡**的代码，加上你的规格、会话日志和简短反思</div>
  </div>
</div>

<div class="caption mt-6">这是形状，不是规格。规格——或其发布日期——将在 **L03，9 月 8 日周二** 给出。这周没什么可做的：你在做 `aider-practice`。</div>

---
layout: statement
---

Aider 会消失。*原则*不会。

<!--
持久的是框架。工具来来去去。Claude Code 和 Codex 已经是继任者；如果有人问起， Plainly 说明。
-->

---
layout: end
---
