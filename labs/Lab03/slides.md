---
theme: ../../lectures/theme
title: "Lab03: 我们在哪里，以及开始使用 Aider"
info: |
  EECS 498 AASE, Lab03
  What students told us, where the course is going, then learning Aider with Codex, Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 我们在哪里，以及开始使用 Aider

## Lab03 · 2026 年 10 月 5–6 日

<!--
根据 2026-10-01 重建，并在 2026-10-05 讲师指示下再次重建。这取代了
"构建智能体"实验课。

两部分。大约 15 分钟的讲解：我们正在改变什么以及为什么，基于你们在
Lab02 表格中告诉我们的兴趣。然后是实验课本身：在 Luna 上设置 Codex，
用 peanuts 映射 Aider，讨论你们发现了什么，以及为 Aider 设计的第一个
功能。

Apply 构建截止日期是明天（星期二）晚上 11:59。在这里说一次。
-->

---
layout: default
---

<div class="label">Lab03</div>

# 今天的日程

1. 我们正在改变什么
2. 设置：Codex 和 Aider
3. 用 Codex 学习 Aider
4. 为 Aider 设计技能

<!--
大约 30 秒。说出形状：二十分钟我们讲解，然后其余部分是你们的，
在你们的笔记本电脑上，在 Aider 代码库中。

出勤表格在最后，它询问你们有哪些 AI 订阅，所以诚实地回答那个。
-->

---
layout: section
---

# 我们正在改变什么

## 你们感兴趣。我们想看看我们能做什么。

---
layout: default
---

<div class="label">我们正在改变什么 · Lab02 表格</div>

# 前沿工具，善用

<div class="text-2xl mt-2" style="color:var(--c-ink)">你们最想学的东西：善用<em>工具</em>。</div>

<div class="grid grid-cols-2 gap-10 mt-6 items-center">
  <div>
    <div class="font-sans font-semibold" style="font-size:3.5rem;line-height:1;color:#1A1A1A">大约一半</div>
    <div class="text-lg mt-2">你们在学什么上的答案</div>
  </div>
  <div class="flex flex-col gap-3">
    <div class="card text-sm italic">"如何推动前沿模型和框架的极限"</div>
    <div class="card text-sm italic">"更真实的（现代的）编码智能体工作流"</div>
    <div class="card text-sm italic">"实际工程师如何使用它的工作流"</div>
  </div>
</div>

<!--
大约一分钟。

49 个你们回答了"如果你整个课程都有 Codex，你想学什么？"我们阅读了
每一个答案并手动排序。大约 23 个，最大的群体，是关于善用工具：真实
工作流、好习惯、可靠结果、充分利用它们。下一个最常见的 是设计和规范
（8），编排和技能（7），以及管理上下文和 token（5）。

注意几乎没有人问用它构建什么。你们问如何使用它。今天的实验课是我们
的第一个答案：Codex 教你一个不是你写的代码库。

三个引用来自三个不同的学生。没有人被命名。
-->

---
layout: default
---

<div class="label">我们正在改变什么</div>

# 更专注于工具

<div class="text-2xl mt-2" style="color:var(--c-ink)">从今天起你们使用 <em>Codex</em>，实验课教你们善用它。</div>

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-terminal-window-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">每个人都使用 Codex</div>
    <div class="text-sm opacity-70 flex-1">在 Luna 上，用你们的课程密钥</div>
  </div>
  <div class="card">
    <ph-laptop-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">实验课是动手的</div>
    <div class="text-sm opacity-70 flex-1">你们的笔记本电脑，真实代码</div>
  </div>
  <div class="card">
    <ph-target-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">我们教善用它</div>
    <div class="text-sm opacity-70 flex-1">先学习代码，然后设计</div>
  </div>
</div>

<!--
大约 90 秒。三个具体的改变。

每个人都使用 Codex。一个前沿编码智能体，在你们已经有的 U-M GPT Toolkit
密钥上，用 Luna 模型。同样的工具和同样的模型给每个人，所以对一个人
有用的东西对房间也有效。

实验课是动手的。实验课现在大部分时间把 Codex 放在你们手中，在真实
代码上，工作人员在房间里走动。今天是第一个。

我们教善用它。你们要求最多的部分。一个强大的模型可以为你编写代码。
技能是使用它来理解你没有写的代码，然后你自己做出设计决策。这就是
今天练习的。
-->

---
layout: default
---

<div class="label">我们正在改变什么</div>

# 大代码库，从 Aider 开始

<div class="text-2xl mt-2" style="color:var(--c-ink)">小模型意味着小仓库。更好的工具意味着真实的 <em>Aider</em>。</div>

<div class="grid grid-cols-2 gap-8 mt-8 items-stretch">
  <div class="card">
    <div class="font-semibold mb-3">直到现在</div>
    <div class="text-sm mb-2">Aider 在小模型上</div>
    <div class="text-sm mb-2">你们从零构建的仓库</div>
    <div class="text-sm">适合模型能处理的大小</div>
  </div>
  <div class="card">
    <div class="font-semibold mb-3">从今天起</div>
    <div class="text-sm mb-2">Luna 上的 Codex</div>
    <div class="text-sm mb-2">真实的 Aider：20,000 行 Python</div>
    <div class="text-sm">加上 12,000 行测试</div>
  </div>
</div>

<div class="caption mt-6">今天你们开始向它添加 Claude Code 功能。你们中的两个 exactly 提出了这个。</div>

<!--
大约 2 分钟。这是方向。

直到现在，项目是你们自己构建的小仓库，因为你们在小模型上使用 Aider，
工作必须适合那个模型能处理的内容。那是那些工具的正确大小。

现在你们有 Luna 上的 Codex，范围上升很多。所以不是从零开始的小项目，
你们在真实的 Aider 中工作：你们从第 1 周开始使用的开源工具。大约
20,000 行 Python，12,000 行测试（在提交 5dc9490b 检查于 10 月 4 日）。
那是真实的工程：已经存在的代码，别人写的，你们在接触之前必须理解的
代码。

今天你们开始向它添加 Claude Code 功能，从技能开始。课程在 Aider 中
停留多久，以及之后是什么，还没有决定。

你们中的两个在 Lab02 表格上独立 suggested exactly this：
"use codex to take aider open source code and make it super sophisticated"
和 "how to build my own agent like codex that is stronger than aider"。
没有人被命名。

说出什么还没有决定：这如何分成项目，每个要求什么，以及日期。那稍后
来。今天是开始，不是一个评分的项目。
-->

---
layout: default
---

<div class="label">我们正在改变什么</div>

# 先学习，然后设计，然后构建

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-book-open-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">1. 学习</div>
    <div class="text-sm opacity-70 flex-1">Codex 教你代码</div>
  </div>
  <div class="card">
    <ph-pencil-ruler-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">2. 设计</div>
    <div class="text-sm opacity-70 flex-1">你做决策</div>
  </div>
  <div class="card">
    <ph-hammer-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">3. 构建</div>
    <div class="text-sm opacity-70 flex-1">小步骤，好提示</div>
  </div>
</div>

<div class="caption mt-6">跳过步骤 1，模型在步骤 2 中为你做每一个决策。</div>

<!--
大约 90 秒。这就是我们希望你以任何代码库在 Aider 中工作的方式，今天
和以后，顺序重要。

先学习。使用 Codex 作为老师：问它代码如何工作，让它向你展示文件名和
行号，并检查它告诉你的内容。

然后设计。你不能对你没有理解的代码做出设计决策。如果你尝试，你会
在不知不觉中将决策交给模型，它将设计一切。这就是我们试图避免的失败。

然后构建，小步骤，带着你的设计提示。不是一个要求整个功能的提示。
One-shot 提示给你模型的设计，不是你的。

今天的其余部分按照这三个步骤的顺序进行。
-->

---
layout: default
---

<div class="label">我们正在改变什么</div>

# 我们仍在弄清楚什么

<div class="text-2xl mt-2" style="color:var(--c-ink)">大多数变得好的是<em>使用</em>，而使用很难教。</div>

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-chalkboard-teacher-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">如何教工具</div>
    <div class="text-sm opacity-70 flex-1">Beyond "use them a lot"</div>
  </div>
  <div class="card">
    <ph-graduation-cap-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">学习是什么样子</div>
    <div class="text-sm opacity-70 flex-1">当模型编写代码时</div>
  </div>
  <div class="card">
    <ph-kanban-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">项目要求什么</div>
    <div class="text-sm opacity-70 flex-1">本学期稍后详述</div>
  </div>
</div>

<div class="caption mt-6">我们将与你们一起解决这些问题。最后的表格是第一步。</div>

<!--
大约 2 分钟。对此 straight。

如何教工具。使某人擅长 Codex 的大多数东西来自使用它，很多，在真实
工作上，并注意它在哪里出错。那很难放在讲座中。今天是我们第一次尝试
一个围绕它构建的实验课。

学习是什么样子。如果模型可以编写大部分代码，工作代码不再显示你理解
了什么。我们当前的赌注：你先学习代码，用智能体教你，然后你做出设计
决策。今天按这种方式构建。我们不确定它是对的。

项目要求什么。我们知道方向，这是下一张幻灯片。每个项目要求你们交什么，
以及何时，稍后。我们会在你需要之前告诉你。
-->

---
layout: section
---

# 设置：Codex 和 Aider

---
layout: default
---

<div class="label">设置 · Codex</div>

# Toolkit 上的 Codex

<div class="text-3xl mt-12 font-semibold"><a href="https://its.umich.edu/computing/ai/codex-classroom">its.umich.edu/computing/ai/codex-classroom</a></div>

<div class="text-xl mt-6">从顶部跟随 ITS 页面。你的密钥已经存在。</div>

<div class="caption mt-10">模型：<code>gpt-6-luna</code>。</div>

<!--
大约一分钟说，然后房间里最多 15 分钟的设置。你们中的许多人之前做过
实验课。如果你做了，帮助一个邻居。

打开 ITS Codex for the Classroom 页面并从顶部跟随它。它涵盖密钥
（Canvas 花名册上的每个学生已经有一个），Mac、Linux 和 Windows 的
安装指南，以及如何检查你剩下的 $25。

每个人在 gpt-6-luna 上使用 Codex，中等努力。不是你计划上的 Claude Code，
也不是 Cursor。gpt-6-luna 在 10 月 1 日和再次在 10 月 5 日在 Toolkit
上的 Codex 中验证。

对于工作人员，如果学生卡住了：直接指南是 KB 15550（Mac，Linux）
https://teamdynamix.umich.edu/TDClient/30/Portal/KB/Article/15550/Codex-Setup-for-Mac-Linux
和 KB 15551（Windows），以及 10 月 1 日测试的配置在 notes.md 中。
-->

---
layout: default
---

<div class="label">设置 · Codex</div>

# 已经在使用 Codex

<div class="text-xl mt-2" style="color:var(--c-ink)">保留你自己的设置。将课程密钥添加为一个<em>配置文件</em>。</div>

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

```toml
# add to ~/.codex/config.toml
[model_providers.toolkit]
name = "UM GPT Toolkit"
base_url = "https://api.toolkit.umgpt.umich.edu/v1"
env_key = "UMGPT_TOOLKIT_API_KEY"
```

</div>
<div>

```toml
# new file ~/.codex/toolkit.config.toml
model = "gpt-6-luna"
model_provider = "toolkit"
model_reasoning_effort = "medium"
```

</div>
</div>

<div class="text-lg mt-4"><code>codex --profile toolkit</code> 使用课程密钥。Plain <code>codex</code> 留在你自己的帐户上。</div>

<div class="caption mt-4">将你的课程密钥放在 UMGPT_TOOLKIT_API_KEY 中。在 Codex 0.160 上测试。</div>

<!--
大约一分钟。只适用于已经用自己的 ChatGPT 帐户使用 Codex 的学生。
其他人跟随上一张幻灯片的 ITS 页面。

ITS 指南将 Toolkit 设置为你的默认提供者，这将替换你已经使用的设置。
配置文件避免了这一点。提供者块放在你现有的 ~/.codex/config.toml 中。
配置文件是它自己的文件，~/.codex/toolkit.config.toml，Codex 将其
layered on top of config.toml 当你运行 codex --profile toolkit（或
-p toolkit）时。Plain codex 继续使用你自己的帐户和模型。

作为单独文件的配置文件是当前 Codex 的工作方式。旧版本使用 config.toml
内部的 [profiles.toolkit] 表，新版本不再接受它们。如果 --profile 不被
识别，更新 Codex。

将你的课程密钥放在 UMGPT_TOOLKIT_API_KEY 中（~/.zshrc 或 ~/.bashrc），
使用 ITS 给你的密钥。

课程工作使用 gpt-6-luna，你自己的项目使用任何你想要的。

在 Codex 0.160.0 上测试：codex exec -p toolkit 在 Toolkit 提供者上的
gpt-6-luna 上运行。
-->

---
layout: default
---

<div class="label">设置 · Aider</div>

# 你的 Aider 副本

1. 像你的其他课程仓库一样发送
2. EGM 技能已经在内部
3. 在代码仓库根目录打开 Codex

<div class="caption mt-6">Elephant、Goldfish、peanuts 和其他，在 <code>.agents/skills/</code> 中，Codex 寻找的地方。</div>

<!--
大约一分钟。

你们每个人从课程中获得你自己的私人 Aider 副本，就像你获得 ai-assistant
仓库一样：aase-student/aider-<uniqname>，从模板 eecs498-aase/aider
生成。模板是 Aider 在上游提交 5dc9490b，这副牌中的每一个行号引用的
那个。我们在发送之前在它中安装了 EGM 技能套件：peanuts、elephant、
goldfish、egm-implement 和 mean-review，在 .agents/skills/ 下，这是
Codex 为项目技能寻找的地方。你不安装任何东西。Aider 自己的 CI 和
release 工作流被移除，所以推送运行 nothing。

克隆它，cd 到它，并在那里开始 codex。问它"你有什么技能？"五个应该回来。

如果课程副本还没有到达你：克隆 https://github.com/Aider-AI/aider，
然后从 materials 仓库复制技能，如 materials/egm/README.md 描述
（在克隆中 cp -R materials/egm/skills/* .agents/skills/）。
-->

---
layout: section
---

# 学习 Aider

## 技能优先，然后 peanuts，然后你自己的问题

---
layout: default
---

<div class="label">学习 Aider · 1 of 3</div>

# 你代码仓库中的五个技能

| 技能 | 它做什么 | 它写什么 |
|---|---|---|
| **peanuts** | 自下而上映射代码库 | 每个文件夹一个 README，`PEANUTS.md` |
| **elephant** | 设计对话，没有代码 | 设计文档，`ELEPHANT.md` |
| **goldfish** | 新鲜的 reviewers 测试文档 | `GOLDFISH.md` |
| **egm-implement** | 从批准的文档构建 | 代码，`IMPLEMENT.md` |
| **mean-review** | 一个敌对的代码审查 | `MEAN-REVIEW.md` |

<div class="caption mt-4">在运行之前阅读 <code>.agents/skills/</code> 中的每个 <code>SKILL.md</code>，以及它在 <a href="https://github.com/eecs498-aase/materials/tree/main/egm/walkthroughs">materials/egm/walkthroughs/</a> 中的演练。</div>

<!--
大约 10 分钟。学生在运行任何东西之前阅读。

技能是 Codex 遵循的一组指令。如果你在不知道的情况下运行一个，你不知道
它要对你的代码仓库做什么，以及它何时停止并等待你。所以先阅读全部五个。
每个 SKILL.md 是 120 到 180 行。

在每个旁边阅读课程 materials 仓库中的演练：
https://github.com/eecs498-aase/materials/tree/main/egm/walkthroughs
每个技能有一个演练（peanuts.md、elephant.md、goldfish.md、egm-implement.md、
mean-review.md），每个解释技能解决什么问题以及为什么它以这种方式构建。
按该流水线顺序阅读它们。

egm/anatomy-of-a-skill.md，上一级，解释一个技能文件是如何组合在一起的，
如果你想要先读那个。

在每个中寻找什么：
- 顶部的描述。它决定 Codex 何时选择技能。
- 硬规则。它永远不会做什么。
- 步骤，以及它停止并等待你的地方。那些停止是你的审查门。
- 它保留的 ledger（PEANUTS.md、ELEPHANT.md 等等）。那个文件是它如何
  记住它在哪里，以及你如何也看到它。Elephant 和其他在 docs/egm/<feature>/ 中保留它们。

然后检查 Codex 看到它们：问"你有什么技能？"全部五个应该回来。如果一个
缺失，文件夹不在 .agents/skills/<name>/SKILL.md。

今天你使用 peanuts 和 elephant。其他三个稍后使用。
-->

---
layout: default
---

<div class="label">学习 Aider · 2 of 3</div>

# 用 peanuts 映射

```text
Run the peanuts skill on aider/, excluding aider/website. Write one README at a time and stop after each.
```

1. 阅读 peanuts 写的 README
2. 问 Codex 教你那一部分
3. 与 Codex 迭代，直到 README 清晰和正确
4. 批准它，然后下一个

<div class="grid grid-cols-2 gap-4 mt-4">
  <div class="card"><div class="text-xs font-semibold opacity-60 mb-1">跟进</div><div class="text-sm italic">"base_coder.py 做什么，其他 coders 继承什么？展示给我。"</div></div>
  <div class="card"><div class="text-xs font-semibold opacity-60 mb-1">迭代</div><div class="text-sm italic">"检查 aider/coders/README.md 对抗代码。重写任何不清楚或错误的东西。"</div></div>
</div>

<!--
大约 25 分钟。这是大多数学习发生的地方。

运行：peanuts 写三个 README，aider/coders/ 和 aider/resources/ 先，
然后是 aider/ 本身，从下面两个加上大约 40 个顶级模块构建。排除
aider/website：它是文档站点，没有排除 peanuts 计划十一个目录而不是
三个。一次要求一个 README 保持每个审查足够小，可以正确处理。

学生不手动阅读代码。Codex 阅读它并教他们。
目标是他们知道每个文件夹的结构至少和 README 一样好。

对于每个 README，按这个顺序：

1. 阅读 peanuts 写的 README。它很短。
2. 问 Codex 教你 Aider 的那一部分。这个文件做什么？什么调用它？
   为什么它这样分割？继续问跟进，直到你可以没有 README 解释文件夹。
   左卡是一个例子。
3. 与 Codex 在 README 上迭代，直到它清晰和正确，在同一个会话中：
   右卡是一个起始提示。Codex 重新阅读代码并重写任何错误、缺失或
   模糊的东西。使用你在步骤 2 中学到的东西：如果 README 不会教你那
   部分，说出来并让 Codex 修复它。如果一个改变让你惊讶，问为什么。
4. 批准它（告诉 Codex，或在 PEANUTS.md 中设置状态）并让 peanuts
   写下一个。

Peanuts 自己的 SKILL.md 警告大约一半的叶子描述是错误的，因为在叶子
处模型只有代码。发现那些是重点：这是你如何学习代码库。检查的 README
是每个后来的会话，你的或 Codex 的，开始的地方。

下一张幻灯片是没有 README 的同样习惯：问 Codex 关于代码中的任何东西。

没有在 Luna 上排练。计划（三个目录）在 10 月 5 日检查。
base_coder.py 是 edit-format coders 在 aider/coders/ 子类化的基类，
所以跟进例子有一个真实的答案。
-->

---
layout: default
---

<div class="label">学习 Aider · 3 of 3</div>

# Codex 作为老师

<div class="text-2xl mt-2" style="color:var(--c-ink)">问关于<em>任何东西</em>。Codex 阅读代码来回答。</div>

<div class="grid grid-cols-2 gap-4 mt-6">
  <div class="card"><div class="text-xs font-semibold opacity-60 mb-1">代码</div><div class="text-sm italic">"Aider 在哪里构建它发送的提示？"</div></div>
  <div class="card"><div class="text-xs font-semibold opacity-60 mb-1">设计</div><div class="text-sm italic">"为什么编辑块而不是工具调用？"</div></div>
  <div class="card"><div class="text-xs font-semibold opacity-60 mb-1">测试</div><div class="text-sm italic">"测试如何在没有真实模型的情况下运行？"</div></div>
  <div class="card"><div class="text-xs font-semibold opacity-60 mb-1">如果</div><div class="text-sm italic">"如果我删除 repomap.py 什么会坏？"</div></div>
</div>

<div class="caption mt-4">继续问，直到你可以没有 Codex 解释它。</div>

<!--
大约 10 分钟，以及任何代码库之后的习惯。

这是学习这个大小代码库的主要方式。你不需要一个好问题。问什么让你
好奇，并跟进任何返回的内容。Codex 搜索和阅读代码来回答， peanuts
README 就位时它从地图开始。

四张卡片是起点，不是工作表：
- 代码：某物在哪里发生，什么调用什么。
- 设计：为什么它这样构建，替代方案是什么。
- 测试：Aider 如何检查自己。
- 如果：什么依赖什么。

两个习惯使它工作。要文件名和行号，所以你知道东西住在哪里。并在
你自己的水平问跟进："像我没有看过这段代码一样解释它"，"把它画成
一个图表"，"问我关于它的三个问题，告诉我什么我错了"。

工作人员的答案，在提交 5dc9490b：系统提示在 fmt_system_prompt
（base_coder.py:1174）中构建，消息在 format_chat_chunks（:1226）中。
Aider 的模型用文本中的编辑块回复，每个模型一种格式，而不是工具调用。
测试用 fake 替换 coder.send（tests/basic/test_coder.py:462）。
base_coder 从 aider/repomap.py 导入 RepoMap 并在 :498 构建它，给
模型一张代码仓库地图。notes.md 中的 trace 参考有其余部分。
-->

---
layout: section
---

# 为 Aider 设计技能

---
layout: default
---

<div class="label">为 Aider 设计技能</div>

# Aider 中的技能

<div class="text-2xl mt-2" style="color:var(--c-ink)">Aider 没有技能加载器。今天你<em>设计</em>一个。</div>

<div class="grid grid-cols-2 gap-8 mt-8 items-stretch">
  <div class="card">
    <div class="font-semibold mb-3">今天的 Aider</div>
    <div class="text-sm mb-2">你手动添加 SKILL.md</div>
    <div class="text-sm mb-2">整个主体，每个请求</div>
    <div class="text-sm">直到你丢弃它</div>
  </div>
  <div class="card">
    <div class="font-semibold mb-3">Claude Code 和 Codex</div>
    <div class="text-sm mb-2">只有描述，每个请求</div>
    <div class="text-sm mb-2">模型要求主体</div>
    <div class="text-sm">匹配时加载</div>
  </div>
</div>

<!--
大约 2 分钟。

技能是我们要添加到 Aider 的第一个东西。你在 L10 遇到它们，它们是表格
中第二最想学的主题。

今天在 Aider 中，使用技能的 唯一方法是用 /read-only 手动添加它的
SKILL.md。然后 Aider 在每个请求中发送整个主体，直到你丢弃它。那花费
token 并挤满上下文。

Claude Code 和 Codex 以级别做。每个请求只携带每个技能的名称和一行
描述。当任务匹配时，模型要求主体，只有那时它才加载。那是 L10 中的
"三个加载级别"。

所以功能：Aider 找到技能，告诉模型什么可用，并在需要时加载一个。它
如何做每一个是你的设计。
-->

---
layout: default
---

<div class="label">为 Aider 设计技能</div>

# 今天的其余部分

1. 学习提示在哪里构建
2. Elephant：决定它应该如何工作
3. 用你的设计构建一个小步骤

<div class="caption mt-6">不要 one-shot 提示。你今天不会完成，这很好。</div>

<!--
会话的其余部分，工作人员在房间里走动。

1. 先学习。问 Codex Aider 在哪里构建它发送的提示以及只读文件在哪里。
   检查它说什么。你不能设计一个你还没有阅读的代码的加载器。
2. 在 Codex 中运行 elephant 并进行设计对话。你做决策：技能在磁盘上
   住在哪里，Aider 如何找到它们，每个请求中放什么，模型如何要求主体，
   以及当两个技能匹配时发生什么。模型可以建议。你选择。
3. 然后针对你的设计构建一个小步骤。不是"向 Aider 添加技能"。每个
   步骤一个提示，每个提示携带一个你做出的决策。

我们不做什么：一个构建整个功能的提示。Luna 可能产生一些运行的东西。
那将是 Luna 的设计，你不会知道它为什么工作。

你今天不会完成。这很好。这是 Aider 工作的开始，不是一个评分的项目，
什么需要稍后到来。
-->

---
layout: end
---
