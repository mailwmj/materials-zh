---
theme: ../theme
title: "L08: 黑客松 1 简报"
info: |
  EECS 498 AASE — 讲座 08
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 黑客松 1 简报

## 讲座 08 · 2026 年 9 月 24 日

<!--
两件事今天。L07 没有完成，它首先来。然后是这个：今晚的简报。

手边放一台笔记本电脑。最后十分钟是一个设置检查，现在发现的问题比在签到处发现的便宜。

说明：这个 deck 从聊天休息开始，在 L07 deck 的其余部分之后。如果 L07 早完成了，早开始这里。
-->

---
layout: default
---

<div class="label">聊天休息</div>

# 更强模型的习惯

今晚 Aider 运行一个强得多的模型，仍然在 32K 预算上。
写一行：一个来自 aider-practice 的、在更强模型下 *更重要* 的习惯，和一个更不重要的习惯。
准备好说为什么。

<!--
好了，Zoom，我们休息一下。

五分钟。每个人在聊天中写一行。

当他们回来时，接受三个或四个答案。"更重要"一侧的好答案：在接受之前阅读 diff，因为强模型比你检查它更快地写出看似合理的代码；保持聊天小；小步提交。

要落地的点：一个更好的模型将瓶颈移到了你阅读和决定。它不会移除它。

好了，Zoom，我们回来了。
-->

---
layout: default
---

<div class="label">今晚</div>

# 晚上概览

|| | ||
|---|---|
|| **时间** | 今晚，7 到 9 PM ||
|| **地点** | Leinweber 1355 ||
|| **工作** | 个人 ||
|| **权重** | 你成绩的 9% ||
|| **携带** | 你的笔记本电脑和充电器 ||

<!--
今晚，七点到九点，在 Leinweber 1355。两个小时，独自一人，它是课程的 9%。

你在自己的笔记本电脑上工作。今晚没有实验室机器，所以带上充电器。两个小时电池不是一个值得打的赌。
-->

---
layout: statement
---

今晚是关于 **taskr** 的。其余的在你的仓库中的 *七点*。

<!--
关于任务，我将要说的一切。它是关于 taskr 的。你用它的东西在签到时给你的仓库中，不在今天的任何幻灯片上。

请不要问我要更多。每个人的答案都是一样的：七点。
-->

---
layout: default
---

<div class="label">格式</div>

# 晚上如何工作

- **模块化**：尽你所能做好
- **根据证据评判**，而不是功能数量
- **简报** 在你的仓库中
- **提交** 通过在 9 PM 前推送

<!--
它有意是模块化的。尽你所能，做好它。做得少但正确，胜过开始一切。

它根据证据评判：你能展示什么在工作，你能解释什么。没有分数细分，功能数量不是衡量标准。清楚地说明什么不工作比希望没有人尝试它更有价值。

关于任务你需要知道的一切都在你仓库中的简报里。先阅读它。

提交意味着在九点前推送到你的仓库。安全分支，我将谈到它，随着你进行推送，但在最后你自己推送主分支。
-->

---
layout: section
---

# 模型

## gpt-5.6-luna，在你自己的密钥上

<!--
章节分隔页。模型和密钥。
-->

---
layout: default
---

<div class="label">今晚改变什么</div>

# 从 qwen 到 gpt-5.6-luna

|| | 到目前为止 | 今晚 ||
|---|---|---|
|| **模型** | `qwen3.5:9b` | `gpt-5.6-luna` ||
|| **运行在** | 你的笔记本电脑 | OpenAI，通过 U-M ||
|| **上下文** | 32K token | 32K，按设计 ||
|| **类别** | 小的，开放权重 | 前沿，托管 ||
|| **速度** | 你的硬件 | 大约每秒 80 个 token ||

<!--
你迄今为止驾驶的所有东西都是一个小开放模型，主要是你自己机器上的 9B，有 32K 上下文。

今晚每一个模型调用都去 gpt-5.6-luna。它是一个 OpenAI 模型，通过 U-M GPT Toolkit 提供服务。它是一个不同类别的模型，在代码方面强得多，当我们昨天测量它时，端到端产生大约每秒 80 个 token。

这就是为什么今晚是两个小时的真正构建而不是两个小时与模型斗争。一行没有改变，那是下一页。

一个奇怪之处。Toolkit 以一次爆发在最后传递流式回复。一个长答案几秒钟什么都不显示，然后所有都显示。那是网关，不是你的代码。

说明：数字来自黑客松/toolkit-models.md，9 月 23 日测量。
-->

---
layout: statement
---

一个 *强* 模型，在你知道的 **32K** 预算上。

<!--
Luna 接受超过一百万个 token 的上下文。今晚你得到三万二千，你整个学期在 Aider 和在模型调用你自己的代码中工作的相同预算。

这是故意的。Aider 设置为 32K，并在聊天超过它时警告你。你仓库中的模型调用拒绝任何超过 32K 的请求，而那个限制不是你可以更改的。所以你继续做你在 aider-practice 以来一直在做的事情：只有你在聊天中正在处理的文件，其余丢弃，任务之间清除聊天。

为什么要带走一个百万 token 的窗口？因为决定什么放在模型前面是这门课程关于的技能。一个巨大的窗口让草率的习惯隐藏，直到会话变慢、昂贵和困惑。今晚你得到一个好得多的模型，你仍然以以前的方式工作。
-->

---
layout: default
---

<div class="label">你的密钥</div>

# Toolkit 密钥及其预算

- **哪里**：toolkit.umgpt.umich.edu
- **登录**：你的 U-M 账户
- **信用**：$25，全部预算
- **模型**：只有 `gpt-5.6-luna`

<!--
密钥来自 toolkit.umgpt.umich.edu。用你的 U-M 账户登录并在该页面上创建一个密钥。

它携带 25 美元的信用。那是你今晚的每一次模型调用的预算。

Luna 是唯一的模型。仓库已经为它设置好了。不要将 Aider 切换到更大的。它不会帮助你，它会使预算快十倍消耗。

在到达之前创建密钥并保存在你可以粘贴的地方。
-->

---
layout: default
---

<div class="label">给定：taskr/model.py</div>

# 一个对模型的请求

```python
body = {"model": "gpt-5.6-luna", "messages": messages}
if tools:
    body["tools"] = tools                   # 模型可能请求的动作
headers = {
    "Content-Type": "application/json",
    "Authorization": f"Bearer {api_key}",   # 你的 Toolkit 密钥
}
request = urllib.request.Request(
    base_url + "/chat/completions",         # base_url 以 /v1 结尾
    data=json.dumps(body).encode(), headers=headers)
with urllib.request.urlopen(request) as response:
    reply = json.load(response)
message = reply["choices"][0]["message"]    # content，或 tool_calls
```

<div class="caption mt-4">你调用它：<code>call(messages, base_url=..., model=..., api_key=..., tools=...)</code></div>

<!--
这是整个模型调用，它已经在你的仓库中，在 taskr/model.py 中，每一步都有注释。你不写它。你调用它。

从上到下。body 命名模型并携带对话，加上当你传递它们时的工具：模型可能请求你的代码运行的动作。那是 L07 的请求对象。

密钥放在 Authorization 头部中，作为单词 Bearer 然后是密钥。这是人们自己写时搞错的那一行。

它 POST 到你的 base URL 加上 /chat/completions。base URL 已经以 /v1 结尾。

回复的消息在第一个选择中。要么它 content 中有文本，要么它有 tool_calls，因为模型想要一个动作运行。

你的代码传递消息、工具和来自你的 .env 的设置，并得到那个消息回来。一个错误的密钥返回为"model HTTP error 401"，而且密钥本身永远不会出现在错误中。
-->

---
layout: default
---

<div class="label">9 月 23 日测量</div>

# $25 买什么

|| 运行 | 费用 ||
|---|---|
|| 整个晚上的脚本化运行 | $0.24 ||
|| 典型的晚上，估计 | 大约 $1 ||
|| 繁重的晚上，估计 | $2 到 $4 ||

<div class="caption mt-6">花掉全部 $25 的方法：一遍又一遍地调用模型，没有任何东西阻止它。</div>

<!--
昨天我们运行了一个脚本化的强学生通过整个晚上，Aider 保持在你将有的相同 32K。总计：24 美分。最大的单个请求大约是 22k token，所以 32K 从不碍事。

一个典型的晚上大约一美元。有人多次运行所有东西是两到四美元。

所以有一种方法花掉全部 $25，它是你阻止的：任何东西一遍又一遍地调用模型，没有东西停止它，每次发送一个更长的对话。每次调用比上一次花费更多，一个运行整个晚上的可以花掉整个密钥。

Aider 打印每条消息的成本。瞥一眼它。
-->

---
layout: default
---

<div class="label">与 Luna 一起工作</div>

# 好好使用强模型

- **32K 预算**：`/drop` 文件，任务之间 `/clear`
- **小提交**：每个一个工作步骤
- **"将文件添加到聊天中？"**：通常 N

<!--
在 32K 内，Aider 需要和以前一样的照顾。只保留你在聊天中正在处理的文件。丢弃其余。任务之间清除。如果 Aider 警告聊天超过 32K，那是你清除的提示，不是说是。

一个 Aider 提示回答 No。当 Luna 的回复提到一个不在聊天中的文件时，Aider 问"将文件添加到聊天中？"。如果回复只是提到它，说 N。说是将回复发回而不应用它的编辑，而更改消失了。我们的测试运行因此同一个编辑花费了两次。

提交小的工作步骤。Aider 为你提交，步骤越小 undo 越容易。
-->

---
layout: section
---

# 设置和记录

## 到达之前，以及前十分钟

<!--
章节分隔页。你今晚之前做的事情，以及你坐下时发生的事情。
-->

---
layout: default
---

<div class="label">今晚之前</div>

# 到达之前

- **笔记本电脑** 和充电器
- **Python 3.10+**、git、Aider 0.86.2
- **你的 Toolkit 密钥**，已保存
- **无密码提示的 GitHub 推送**

<!--
笔记本电脑和充电器。

Python 3.10 或更新，git，和 Aider 0.86.2，你在 aider-practice 中使用的安装。如果今天 aider --version 工作，今晚工作。

你的 Toolkit 密钥，创建并保存在你可以粘贴的地方。

以及 GitHub。你的仓库在每次提交后在后台推送，所以 git 必须在不询问密码的情况下推送。SSH 是可靠的方式。ssh -T git@github.com 应该用你的名字问候你。邀请发送到你在学期开始调查中给出的 GitHub 用户名。
-->

---
layout: default
---

<div class="label">签到处</div>

# 前十分钟

1. **签到** 在表格上
2. **接受** 你的 GitHub 邀请
3. **克隆** 你的仓库
4. **运行** 设置脚本

<div class="caption mt-6">确切的步骤在今晚的屏幕上。</div>

<!--
每个人都一样。

当你坐下时在表格上签到。它是我们如何知道你在房间里，以及它如何给你一个仓库。

几分钟后 GitHub 发送一个邀请到你自己的私有仓库。接受它。

克隆它并运行设置脚本。那就是整个开始。

确切的命令今晚会在屏幕上。你现在不需要复制任何东西。
-->

---
layout: default
---

<div class="label">要求</div>

# 每个人都运行 bin/setup

1. **运行** `bin/setup`
2. **粘贴** 你的 Toolkit 密钥，当它问时
3. **检查** 每个步骤都说 ok

<div class="caption mt-6">任何东西说 PROBLEM？立刻告诉我们。</div>

<!--
每个人都必须运行 bin/setup。它不是可选的。

运行它，当它问时粘贴你的密钥，并阅读它打印的东西。它为你检查一切：密钥，模型，你的 Aider 工作的记录，以及推送到 GitHub。每个步骤应该说 ok。

如果任何东西说 PROBLEM，立刻举手。不要绕过它。第三分钟修复需要五分钟。在八点半发现，它可能花费你的证据。
-->

---
layout: default
---

<div class="label">证据</div>

# 什么被记录

- **每一个 Aider 提交** 携带它的对话
- **每一个提交** 快照到 `safety/<machine>`
- **`/undo`** 工作，且撤销的更改被保留
- **`bin/aider-log`** 列出一切

<!--
你的 Aider 转录是评分的证据，你永远不必记住提交它。Aider 做出的每个提交都携带 .aider.chat.history.md，产生该更改的对话。

每个提交还添加一个快照到一个名为 safety 的分支和你的机器，并在后台推送到 GitHub。如果你的笔记本电脑在九点死机，你的工作已经在 GitHub 上。

/undo 仍然和你所知的一样工作。撤销的更改保留在安全分支上，所以没有任何你尝试的东西丢失，你不需要小心 undo。

bin/aider-log 显示 Aider 做的每一次更改，以及它背后的提示，包括你撤销的那些。

让安全分支和钩子单独来。
-->

---
layout: default
---

<div class="label">设置检查，现在</div>

# 设置检查

```text
python3 --version        # 3.10 或更新
git --version
aider --version          # 0.86.2
ssh -T git@github.com    # 用你的名字问候你
```

<div class="caption mt-6">然后在 toolkit.umgpt.umich.edu 登录并检查你的密钥存在。</div>

<!--
笔记本电脑打开。运行这四个，然后在 Toolkit 站点登录并确保你有一个密钥。

如果其中任何一个失败就举手。工作人员在房间里走。现在修复 Python 或 SSH 需要五分钟。在签到处需要二十分钟的晚上。
-->

---
layout: end
---

<!--
问题。

"任务是什么"的答案是：在签到时你的仓库中。

今晚见。
-->
