---
theme: ../../lectures/theme
title: "Lab04: 到达计算机：Shell、MCP 和浏览器"
info: |
  EECS 498 AASE, Lab04
  How a harness reaches a computer: the shell, MCP, the browser and prompt injection, Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 到达计算机：Shell、MCP 和浏览器

## Lab04 · 2026 年 10 月 12–13 日

<!--
讲解：大约 1 分钟。

下午好。在 L11 中我们涵盖了工具调用失败时会发生什么，以及谁在运行
之前说是。今天是工具本身：框架如何实际到达你的计算机。

今天没有演示也没有作业，只有这个，然后是出勤表格。幻灯片上的所有
内容都是在 10 月 7 日从真实工具捕获的：Codex 和 Claude Code 发送给
它们的模型的请求， wire 上的真实 MCP 服务器，以及由框架驱动的真实
浏览器。
-->

---
layout: default
class: v2
---

# 今天的日程

<div class="fig"><div>

<table class="map">
<tbody>
<tr><td>第一部分：shell</td></tr>
<tr><td>第二部分：MCP</td></tr>
<tr><td>第三部分：浏览器</td></tr>
<tr><td>第四部分：提示注入</td></tr>
<tr><td>问题，然后是出勤表格</td></tr>
</tbody>
</table>

</div></div>

<div class="source">今天只有幻灯片：不需要安装任何东西，也不需要交任何东西。</div>

<!--
讲解：大约 1 分钟。

这是今天。三个部分，每一个是框架到达你的计算机的一种方式，然后是
提示注入，它贯穿所有三个。没有作业，不需要安装任何东西。出勤表格
在最后。
-->

---
layout: section
---

# 第一部分：Shell

## 它如何运行 · 它花费什么 · 它能到达什么

<!--
讲解：第一部分大约 25 分钟。

第一部分。Shell 是每个框架都有的唯一工具，所以我们从那里开始。
-->

---
layout: section
---

# 短命令

## 第一部分 · shell

<!--
讲解：短命令大约 8 分钟。

两种命令，它们工作方式不同。首先是普通类型：一个运行、结束并打印
某些东西的命令。pytest、git status、grep。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 短命令</div>

# 一个命令，端到端

<div class="lede">模型只询问。框架在你的计算机上运行命令并发送回它打印的内容。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d03-shell-loop-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d03-shell-loop-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d03-shell-loop-2.svg" /></template>
</v-switch>
</div>

<div class="source">每个框架中的相同循环，从最大的到 100 行的一个。</div>

<!--
讲解：大约 2 分钟。两次点击。

一个命令，从头到尾。模型不能运行任何东西。它写一个工具调用，说：
运行 pytest -q。

点击。框架获取那个字符串并在你的计算机上运行它，就像你输入终端一样。
同样的事情。

点击。命令打印的任何内容，加上它的退出代码，作为文本返回给模型。
退出代码 1 意味着测试失败，模型决定下一步做什么。

这就是短命令的全部工具：它运行，它结束，它的输出返回。接下来，那个
工具调用实际是什么样子。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 短命令</div>

# Shell 工具的输入

<div class="lede">模型写的不仅仅是命令：一个给你的描述，以及等待多长时间。</div>

<div class="fig"><div class="grid grid-cols-2 gap-8 items-center">
<div>

```json
{"name": "Bash",
 "input": {
   "command": "pytest -q",
   "description": "Run the test suite",
   "timeout": 120000
 }}
```

</div>
<div>

<table class="map compact">
<tbody>
<tr><td><code>command</code></td><td>运行什么</td></tr>
<tr><td><code>description</code></td><td>询问你批准时你看到的内容</td></tr>
<tr><td><code>timeout</code></td><td>等待多长时间，以 ms 为单位</td></tr>
<tr><td><code>run_in_background</code></td><td>用于不会结束的命令</td></tr>
</tbody>
</table>

</div>
</div></div>

<div class="source">Claude Code 2.1.293 的 Bash 工具，10 月 7 日捕获。</div>

<!--
讲解：大约 2 分钟。

这是模型写它想要运行测试的工具调用，在真实框架的格式中。

命令只是输入之一。模型还写一个描述，用 plain words，那就是当框架
询问你批准命令时你看到的内容。工具自己的指令说用户经常在不看命令
的情况下阅读描述，所以描述是模型关于它自己的命令的主张。L11 的批准
规则适用：阅读命令，不是解释。

timeout 是框架等待多长时间，默认两分钟。run_in_background 用于不会
结束的命令，这是本部分的第二半。

那么当一个任务需要超过一个命令时会发生什么？
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 短命令</div>

# 一次一个工具调用

<div class="lede">通常每个命令是它自己的工具调用。每个结果在模型选择下一个命令之前返回给它。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d05a-one-call-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d05a-one-call-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d05a-one-call-2.svg" /></template>
</v-switch>
</div>

<div class="source">一个说明性示例：shell() 代表框架的 shell 工具。</div>

<!--
讲解：大约 2 分钟。两次点击。

你问模型为什么测试失败。它需要两个命令：git status，看看什么改变了，
和测试套件。

它要求第一个。框架在你的计算机上运行 git status，获取四行返回，并
将那四行交给模型。

点击。蓝色条是模型在工作：它读取四行并决定接下来运行什么。然后它
要求测试。框架运行 pytest，所有三百行测试日志返回给模型。

点击。所以两个命令花费两次通过模型的行程，每个命令打印的每一行现在
都在它的上下文中，无论模型是否需要它。

这就是每个框架默认的工作方式。Code 模式改变了它。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 短命令</div>

# Code 模式：从代码调用的工具

<div class="lede">在 code 模式中，模型的工具调用是一个程序。框架运行整个程序，一个结果返回：程序打印的任何内容。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d05b-code-block-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d05b-code-block-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d05b-code-block-2.svg" /></template>
</v-switch>
</div>

<div class="source">一个说明性示例：shell() 代表框架的 shell 工具，从 Python 调用。Anthropic 的"Code execution with MCP"（2025 年 11 月 4 日）和 Cloudflare 的"Code Mode"（2025 年 9 月 26 日）描述了同样的想法。</div>

<!--
讲解：大约 2 分钟。两次点击。

同样的任务：为什么测试失败。在 code 模式中，模型不要求一个命令。
它的工具调用是一个它刚刚写的短 Python 程序。shell 是框架的 shell
工具，可以从代码调用。程序运行 git status，运行测试，并打印状态和
测试日志的末尾。

点击。框架运行整个程序。每个 shell 调用在你的计算机上运行，它的
输出返回给程序，不是模型：git status 的四行，pytest 的三百行。三百
行留在程序内部。

点击。当程序结束时，无论它打印什么都作为单一结果返回给模型，大约
二十五行。

一块代码输入，一个结果输出。接下来，同样的运行在上一张幻灯片的车道
中。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 短命令</div>

# 在同样的车道中 Code 模式

<div class="lede">模型被调用一次。命令在它等待时运行，只有打印的结果到达它的上下文。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d05-code-mode-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d05-code-mode-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d05-code-mode-2.svg" /></template>
  <template #3><img src="./assets/diagrams/d05-code-mode-3.svg" /></template>
</v-switch>
</div>

<div class="source">与最后两张幻灯片同样的说明性示例。</div>

<!--
讲解：大约 2 分钟。三次点击。

同样的任务，同样的三个车道作为一个调用。模型将程序作为一个工具调用发送。

点击。框架运行程序。每个 shell 调用在你的计算机上运行，它的输出返回
给程序，不是模型。模型在这一切发生时根本不被调用。

点击。当程序结束时，模型只得到它打印的内容：大约二十五行。

点击。比较一次一个调用，两张幻灯片前：一次通过模型而不是两次，上下
文中的二十五行而不是三百行。任务需要的命令越多，这节省的越多。

这就是短命令。接下来，另一种：永远不会结束的命令。
-->

---
layout: section
---

# 长时间运行的命令

## 第一部分 · shell

<!--
讲解：长时间运行的命令大约 12 分钟。

到目前为止，每个命令在一两秒内结束。有些需要几分钟，有些永远不会
结束。首先，框架如何等待一个。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 长时间运行的命令</div>

# 不会结束的命令

<div class="lede">从外部，慢命令和永不结束的命令看起来一样。框架必须决定何时停止等待。</div>

<div class="fig"><div class="grid grid-cols-2 gap-6">
<div class="card"><div class="col-head">服务器</div><p><code>npm run dev</code>, <code>python -m taskrweb</code></p></div>
<div class="card"><div class="col-head">监视器</div><p><code>tsc --watch</code>, 任何在保存时重新运行的东西</p></div>
<div class="card"><div class="col-head">等待输入</div><p>密码提示、分页器、是或否</p></div>
<div class="card"><div class="col-head">只是慢</div><p>完整构建、长测试套件</p></div>
</div></div>

<div class="source">每个框架都需要对所有四个的答案。</div>

<!--
讲解：大约 2 分钟。

四种命令不会快速返回。

服务器：一个 dev 服务器运行直到你停止它。你的 Apply taskr web 应用
是其中一个。

监视器：编译器或测试运行器在 watch 模式中，每次文件改变时重新运行。

等待输入：密码提示，像 git log 打开的分页器，一个想要是或否的问题。
这些是 stuck，不是慢。

只是慢：一个大构建或完整的测试套件，最终会完成。

框架不能通过等待区分这些。三十秒的静默后，慢构建和服务器看起来完全
一样。所以每个框架必须决定何时停止等待，以及对仍在运行的进程做什么。

首先，停下来询问人的命令。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 长时间运行的命令</div>

# 等待输入的命令

<div class="lede">默认情况下没有人在键盘前，所以框架阻止程序停下来询问。</div>

<div class="fig"><div>

<table class="map compact">
<colgroup><col style="width:20rem" /><col /></colgroup>
<thead><tr><th>程序想要</th><th>框架做什么</th></tr></thead>
<tbody>
<tr><td>一个分页器 (<code>git log</code>)</td><td>设置 <code>PAGER=cat</code>，所以它打印并退出</td></tr>
<tr><td>颜色和进度条</td><td>设置 <code>TERM=dumb</code> 和 <code>NO_COLOR=1</code></td></tr>
<tr><td>一个编辑器或提示 (<code>git rebase -i</code>)</td><td>告诉模型不要运行它们</td></tr>
</tbody>
</table>

</div></div>

<div class="source">Codex 0.161 源代码：每个命令得到 TERM=dumb, NO_COLOR=1 和 PAGER=cat · Claude Code 2.1.293 Bash 工具描述：不支持交互式标志。</div>

<!--
讲解：大约 2 分钟。

最狡猾的命令停下来等待一个人。

git log 打开一个分页器并等待你按 q。框架可以为每个命令将 PAGER 设置
为 cat，所以 git log 只是打印并退出。

绘制颜色和进度条的程序用转义码填充输出，模型必须读取。框架告诉它们
没有 fancy terminal。

一些打开一个编辑器或问一个问题，比如 git rebase dash i。没有人在键盘
前，所以框架的工具描述 plain 告诉模型不要运行它们。

如果你构建一个 shell 工具，你需要同样的防御，否则你的框架将在第一个
git log 上挂起。

这处理了等待人的命令。接下来，只是慢的命令。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 长时间运行的命令</div>

# 等待有截止日期

<div class="lede">框架等待最多 2 分钟。然后它停止等待，但命令继续运行，框架在它结束时告诉模型。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d04e-deadline-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d04e-deadline-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d04e-deadline-2.svg" /></template>
</v-switch>
</div>

<div class="source">从 Claude Code 2.1.293 的 Bash 工具简化（默认超时 2 分钟，最多 10 分钟），10 月 7 日捕获，及其文档。</div>

<!--
讲解：大约 2 分钟。两次点击。

普通情况。模型要求测试套件，框架运行它。没有人告诉模型这个套件需要
三分钟。

点击。框架等待，默认两分钟。模型在发出调用时可以要求最多十分钟。当
时间到了，pytest 仍在继续，框架不杀死它。它停止等待，告诉模型命令
仍在运行，并给它一个名字：task 7。

点击。模型继续其他工作。当 pytest 最终退出时，框架用结果唤醒模型，
同样的输出它本来可以通过等待获得。

所以慢命令不会永远阻塞模型。但模型仍然坐在两分钟的命令前，它可以
已经猜测是慢的。接下来： upfront 说出来。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 长时间运行的命令</div>

# 在后台启动

<div class="lede">当模型知道一个命令不会结束时，它 upfront 说出来。框架立即用 task id 回答。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d04b-long-command-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d04b-long-command-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d04b-long-command-2.svg" /></template>
</v-switch>
</div>

<div class="source">从 Claude Code 2.1.293 的 Bash 工具简化（run_in_background），10 月 7 日捕获。</div>

<!--
讲解：大约 1 分钟。两次点击。

一个 dev 服务器永远不会退出，所以等待两分钟是毫无意义的。模型要求
它在后台。框架启动进程：绿色条是它运行多长时间。

点击。框架根本不等待。它立即回答：已启动，task 7。

点击。模型继续工作，服务器继续运行，直到某些东西停止它。从这一点
开始，task id 是模型如何到达它。

接下来：模型可以对仍在运行的命令做什么。
-->

---
layout: default
class: v2
---

<div class="where"><b>Shell</b> · 长时间运行的命令</div>

# 进入运行中命令的四种方式

<div class="lede">一个运行中的命令有输出、输入、端口和停止。模型通过框架到达每一个，通过 task id。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d04i-reach-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d04i-reach-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d04i-reach-2.svg" /></template>
  <template #3><img src="./assets/diagrams/d04i-reach-3.svg" /></template>
  <template #4><img src="./assets/diagrams/d04i-reach-4.svg" /></template>
</v-switch>
</div>

<div class="source">从 Claude Code 2.1.293 的 Monitor 和 TaskStop 以及 Codex 0.160.0 的 write_stdin 简化，10 月 7 日捕获。</div>

<!--
讲解：大约 5 分钟。四次点击。

Task 7 在后台运行。模型可以用它做什么？一个运行中的程序只有几个
方式进入，对你和模型都一样：它打印什么，它读取什么，它监听 的网络
端口如果它是一个服务器，以及停止它。模型从不接触进程。它询问框架，
并通过 task id 命名它。

点击。它的输出。模型询问框架监视 task 7。程序打印的每一新行都到
达框架，框架在它到达时将它传递给模型。每一行是模型上下文中的一条
消息，所以真正的监视过滤，例如只带有 Error 的行。

点击。它的输入。模型写入程序的输入，就像在它的键盘上打字一样：这里
y 和 Enter，回答一个是或否问题，它返回程序接下来打印的任何内容。
Python 会话或调试器以同样的方式工作，并在写入之间保持其状态。框架
必须在一个终端中启动程序。它也是一个风险：无论一个提示在询问什么
人，模型现在可以回答。

点击。它的端口。一个服务器监听端口，这里 5173。模型不需要特殊工具
来做到这一点：它像任何其他命令一样运行 curl，curl 发送请求，响应
作为 curl 的输出返回。浏览器是另一种方式，在第三部分。

点击。停止。模型询问框架停止 task 7，框架停止它。如果模型忘记，
框架在默认十分钟后停止后台命令。模型可以要求最多两个小时。

这就是 shell：它在你的计算机上运行，它花费返回的任何输出的成本，
框架决定等待多长时间以及模型如何到达运行中的进程。接下来：其他人
写的工具。
-->

---
layout: section
---

# 第二部分：MCP

## 其他人写的工具

<!--
讲解：休息前大约 13 分钟，休息后 10 分钟。

第二部分。MCP 是框架如何获得其他人编写和运行的 工具。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 它是什么</div>

# MCP 是什么

<div class="lede">MCP 是框架如何从其他程序获得更多工具，并将它们交给模型。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d02-mcp-tools-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d02-mcp-tools-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d02-mcp-tools-2.svg" /></template>
</v-switch>
</div>

<div class="source">MCP，Model Context Protocol，是一个开放标准（modelcontextprotocol.io），由 Anthropic 在 2024 年 11 月宣布，现在在 Linux Foundation 下。</div>

<!--
讲解：大约 2 分钟。两次点击。

这里关于 MCP 的一切。框架有一个它给模型的工具列表：shell，读取文件，
编辑文件。这些是写入框架的。

点击。一个 MCP 服务器是另一个有自己工具的程序。GitHub 的服务器有
list_issues、add_issue_comment 等等。框架连接到它，询问它的工具，并
将它们添加到列表中。

点击。模型看到一个列表。它无法分辨哪些工具来自框架，哪些来自 MCP，
它也不需要。当它调用 list_issues 时，框架将调用传递给 GitHub 的
服务器。

所以 MCP 不是一个模型，不是一个框架。它是框架插入更多工具的标准方式。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 为什么一个标准</div>

# 没有 MCP，每个框架写自己的

<div class="lede">Codex、Claude Code 和 ChatGPT 都会写自己的 GitHub 代码，以及他们自己的 Slack 代码，等等。</div>

<div class="fig"><img src="./assets/diagrams/d07-n-by-m-0.svg" /></div>

<div class="source">四个框架和四个服务：十六个集成，每个由框架的制作者编写和维护。</div>

<!--
讲解：大约 1 分钟。

为什么不让每个框架自己调用 GitHub 的 API？它可以。但必须有人写那个
代码，并保持它工作。

没有标准，Codex 写一个 GitHub 集成，Claude Code 写另一个，ChatGPT
第三个，Slack、Postgres 和其他所有东西也一样。四个和四个的十六个
集成，每个略有不同，每个等待那个框架的下一个版本。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 为什么一个标准</div>

# 有了 MCP，服务写一次

<div class="lede">GitHub 写一个 MCP 服务器。每个会说 MCP 的框架都可以使用它。</div>

<div class="fig"><img src="./assets/diagrams/d07-hub-0.svg" /></div>

<div class="source">每个框架说一次 MCP，每个服务写一个服务器：八块而不是十六块。这个想法是从 Language Server Protocol 借来的。</div>

<!--
讲解：大约 1 分钟。

有了 MCP，每个框架学习 MCP 一次，每个服务写一个服务器。四加四。

GitHub 最知道它自己的 API，所以 GitHub 写服务器。当 GitHub 添加一个
功能时，它向它的服务器添加一个工具，每个框架在下次连接时看到它。
没有人等待框架版本。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 它如何运行</div>

# MCP 在两个调用中

<div class="lede">框架询问服务器它的工具，然后调用一个。模型从不与它对话。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d09-mcp-calls-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d09-mcp-calls-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d09-mcp-calls-2.svg" /></template>
</v-switch>
</div>

<div class="source">两个调用是 MCP 规范中的 tools/list 和 tools/call。工具名称来自 GitHub 的 MCP 服务器；调用和结果是说明性的。</div>

<!--
讲解：大约 3 分钟。两次点击。

这里关于 MCP 的一切，以 GitHub 的服务器为例。三条车道：模型、框架
和服务器。

一。当框架启动时，它询问服务器：你有什么工具？服务器用一个列表回答：
list_issues、add_issue_comment，等等，每个有一个短描述和它需要的输入。

点击。二。框架将这些工具放入模型的工具列表中，在它自己的工具如 shell
旁边。从模型的侧面，一个 MCP 工具看起来与任何其他工具完全一样。注意
服务器写的描述现在逐字进入模型的提示。休息后我们回到这一点。

点击。三。模型决定为 aider 仓库调用 list_issues。框架将其转换为第二个
调用：用这些参数运行 list_issues。服务器做工作，可能调用 GitHub 自己
的 API，并发送回结果，作为文本返回给模型。

就是这样。两个调用：列出工具，调用一个。模型从不直接与服务器对话。
框架总是在中间，这就是 L11 中批准坐的地方。

接下来：那个工具列表实际包含什么。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 它如何运行</div>

# 服务器告诉模型什么

<div class="lede">对于每个工具：一个名称，何时使用它的描述，以及它需要的参数。</div>

<div class="fig"><div class="tight">

```json
{"name": "list_issues",
 "description": "List issues in a GitHub repository with optional
                 filtering by labels, states, and custom fields.",
 "inputSchema": {"type": "object",
   "properties": {
     "owner": {"type": "string", "description": "The owner of the repository"},
     "repo":  {"type": "string", "description": "The name of the repository"},
     "state": {"type": "string", "description": "Filter by issue state (open, closed, all)"}},
   "required": ["owner", "repo"]},
 "annotations": {"readOnlyHint": true}}
```

</div></div>

<div class="source">来自 GitHub MCP 服务器源（github/github-mcp-server）的 list_issues，修剪到它的七个参数中的三个。框架将这一点复制到模型的工具列表中。</div>

<!--
讲解：大约 2 分钟。

当一个工具在 GitHub 的服务器中列出时，这是它是什么样子。真实的，
来自服务器的源，修剪。

一个名称，模型调用它。

一个描述，用 plain English。这是唯一告诉模型何时使用工具的东西，
所以它是一个提示，由 GitHub 编写。

一个输入模式：参数，它们的类型，每个的短描述，以及哪些是必需的。
Owner 和 repo 是必需的，state 是可选的。模型在调用工具时填写这些，
框架可以在发送任何东西之前检查它们。

以及一个注释，说工具只读取。框架可以使用它来跳过询问你批准，尽管
规范说只信任来自你信任的服务器的注释。

框架将所有这些复制到模型的工具列表中，与它自己的工具格式相同。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 它如何运行</div>

# MCP 服务器运行在哪里

<div class="lede">一个本地服务器是框架在你的计算机上启动的程序。一个远程服务器在其他人的机器上运行。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d08-mcp-parts-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d08-mcp-parts-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d08-mcp-parts-2.svg" /></template>
</v-switch>
</div>

<div class="source">MCP 规范，2026-07-28 · modelcontextprotocol.io</div>

<!--
讲解：大约 2 分钟。两次点击。

一个 MCP 服务器实际运行在哪里？两个地方，图片是重点。

你的计算机，用框架在上面。

点击。一个本地服务器只是一个程序。当框架启动时，它也启动服务器，
在你的计算机上，它们通过程序的输入和输出对话。Playwright 的浏览器
服务器以这种方式工作，Excalidraw 服务器绘制这些图表也是。它作为你
运行，用你的文件。

点击。一个远程服务器在其他人的机器上运行。GitHub 的官方服务器是
一个：框架通过 HTTPS 连接到它，像任何 web API，GitHub 运行它。没有
任何东西可安装，你的登录去 GitHub，不是框架。

无论哪种方式，框架都在中间，模型只看到工具。

接下来：框架如何实际发送服务器一个调用。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 它如何运行</div>

# 框架如何发送一个调用

<div class="lede">一行 JSON。一个本地服务器从它的输入读取它；一个远程服务器通过 HTTPS 接收它。</div>

<div class="fig"><div class="tight">

```text
the harness writes:
{"jsonrpc": "2.0", "id": 3, "method": "tools/call",
 "params": {"name": "list_issues",
            "arguments": {"owner": "Aider-AI", "repo": "aider"}}}
the server answers:
{"jsonrpc": "2.0", "id": 3,
 "result": {"content": [{"type": "text", "text": "[{\"number\": 4521, …}]"}]}}
```

</div></div>

<div class="source">MCP 规范：JSON-RPC 2.0，本地服务器的 stdin 和 stdout 上每行一个消息。修剪；结果是说明性的。</div>

<!--
讲解：大约 2 分钟。

所以框架如何实际向 MCP 服务器发送命令？它不像听起来那么神奇。

对于一个本地服务器，框架自己启动了程序，所以它持有程序的输入和输出，
shell 使用的同样的管道。调用一个工具，它向程序的输入写入一行 JSON：
调用 list_issues，用这些参数。服务器做工作并向后打印一行 JSON。id，3，
匹配答案到问题。

对于一个远程服务器，像 GitHub 的，它是同样的 JSON，通过 HTTPS 请求
发送而不是管道。

答案的 content 是框架作为工具结果传递给模型的内容。这里，一个问题
列表作为文本。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 不止工具调用</div>

# 不止工具调用

<div class="lede">服务器还可以处理登录、共享数据、问你问题，以及添加工具。</div>

<div class="fig"><div class="grid grid-cols-2 gap-6">
<div class="card"><div class="col-head">登录</div><p>登录一次；token 留在服务器中</p></div>
<div class="card"><div class="col-head">数据</div><p>文件和记录，不只是工具</p></div>
<div class="card"><div class="col-head">问题</div><p>问你它缺少的东西</p></div>
<div class="card"><div class="col-head">更新</div><p>没有框架发布的新工具</p></div>
</div></div>

<div class="source">MCP 规范：授权（OAuth）、资源、 elicit 和列表更改通知。</div>

<!--
讲解：大约 2 分钟。

工具调用是 MCP 使用的大部分，但服务器可以做四个更多有用的事情。

登录。需要你帐户的服务器自己处理登录。你点击通过一个普通登录页面
一次，在 Codex 中是 codex mcp login，服务器保持 token。模型从不看到
它，这在第四部分很重要。

数据。除了工具，服务器可以提供资源：文件、记录和文档，框架可以拉入
对话。

问题。在调用中间，服务器可以停下来问你它需要的东西，比如你指的是
哪个仓库，而不是猜测。

更新。服务器可以告诉框架它的工具列表改变了，所以新工具出现，没有
任何人更新框架。

接下来：MCP 何时是正确的选择。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 它花费什么</div>

# MCP 服务器或 CLI

<div class="lede">如果框架有一个 shell，模型已经知道的 CLI 通常更便宜。</div>

<div class="fig"><div class="grid grid-cols-2 gap-8">
<div class="card">
<div class="col-head">gh 通过 shell</div>
<p>模型已经知道它</p>
<p>每个请求不添加任何东西</p>
<p>需要 shell 和你的登录</p>
</div>
<div class="card">
<div class="col-head">GitHub MCP 服务器</div>
<p>类型化参数，描述的工具</p>
<p>在没有 shell 的地方工作</p>
<p>自己处理你的登录</p>
</div>
</div></div>

<div class="source">Playwright MCP 的 README 现在告诉编码工具使用它的 CLI 作为"更节省 token" · M. Zechner，2025 年 11 月 2 日 · S. Willison 关于技能，2025 年 10 月 16 日。</div>

<!--
讲解：大约 3 分钟。

记住第三个选项：模型通过 shell 运行一个 CLI 如 gh。

gh 通过 shell。模型在训练中看过 gh 和 git 成千上万次，所以它根本
不需要定义。它像每个其他命令一样运行。它什么都不花费，直到它运行。
但它需要 shell 和那台机器上的你的 GitHub 登录。

MCP 服务器。类型化输入和输出。它在没有 shell 的应用程序中工作，像
ChatGPT。它处理它自己的登录，所以模型从不看到你的 token。它的工具
定义伴随每个请求，除非主机隐藏它们。

对于有 shell 的框架，CLI 通常赢，构建这些工具的人这样说。Playwright，
我们在第三部分使用，现在告诉编码工具使用它的 CLI 而不是它的 MCP 服务器，
因为它更便宜。

当没有 shell 时，当登录必须远离模型时，或者当没有 CLI 时，MCP 赢。
接下来：那些定义实际花费多少。
-->

---
layout: default
class: v2
---

<div class="where"><b>MCP</b> · 它花费什么</div>

# 按需加载工具

<div class="lede">框架只向模型发送工具名称。当模型需要一个工具时，它搜索，框架加载那一个定义。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d11-deferred-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d11-deferred-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d11-deferred-2.svg" /></template>
  <template #3><img src="./assets/diagrams/d11-deferred-3.svg" /></template>
</v-switch>
</div>

<div class="source">Playwright MCP 0.0.83 的 25 个工具，10 月 7 日捕获，o200k token · Anthropic 测量 77K token 降到 8.7K（2025 年 11 月 24 日）。</div>

<!--
讲解：大约 2 分钟。三次点击。

模型可以调用的每个工具都有一个定义：它的名称，一个描述，和它的参数。
通常所有它们都进入每个请求。按需加载发送更少。这里 Playwright 的
MCP 服务器已连接，有 25 个工具。

框架只发送名称：所有 25 个 94 个 token，加上它自己的一个工具 search。

点击。模型想要点击一个按钮，所以它搜索 click。框架加载一个匹配的
定义，browser_click 完整，257 个 token。

点击。现在模型可以调用它。框架将调用传递给 Playwright 的服务器，用
tools/call，与任何 MCP 调用一样，结果返回。

点击。没有这个，所有 25 个定义，5,250 个 token，进入每个请求，每个
回合，无论模型是否接触浏览器。

它与 L10 中的技能同样的想法：名字先，需要时细节。Anthropic 测量了
同样的移动在大约七万七千 token 的定义降到不到九。Codex 更进一步：它
的 MCP 工具根本不在提示中，模型通过搜索代码中的列表找到它们。

这就是 MCP：对服务器的两次调用，在每个请求中按 token 支付。接下来：
到达最多的通道，浏览器。
-->

---
layout: section
---

# 第三部分：浏览器

## 页面、像素和树

<!--
讲解：第三部分大约 15 分钟。

为什么框架需要浏览器？检查一个它刚刚构建的 web 页面，读取只有
JavaScript 才出现的页面，以及使用没有 API 的 web 应用程序。这里的
每一个捕获都是 Playwright 的 MCP 服务器在 10 月 7 日驱动 Aider 文档
上的真实浏览器。
-->

---
layout: default
class: v2
---

<div class="where"><b>Browser</b> · 它如何运行</div>

# 三种方式看一个页面

<div class="lede">屏幕截图显示布局，树命名什么可以点击，DOM 保存一切。</div>

<div class="fig"><div class="grid grid-cols-2 gap-6 items-center">
<div>
<div class="col-head">像素：屏幕截图</div>
<img src="./assets/img/aider-docs.png" class="shot" />
</div>
<div class="tight">
<div class="col-head">可访问性树</div>

```yaml
- navigation "Main" [ref=e7]:
  - list [ref=e8]:
    - listitem [ref=e9]:
      - link "Installation" [ref=e13]
```

<div class="col-head mt-4">DOM，通过 JavaScript</div>

```js
() => [...document.querySelectorAll('nav a')]
  .slice(1, 5).map(a => a.getAttribute('href'))
// ["/docs/install/optional.html", …]
```

</div>
</div></div>

<div class="source">10 月 7 日捕获 · Playwright MCP 0.0.83 在 aider.chat/docs：browser_take_screenshot, browser_snapshot, browser_evaluate。</div>

<!--
讲解：大约 3 分钟。

一个页面，模型可以看它的三种方式。所有三个是同一个页面的真实输出。

屏幕截图。模型将它作为图像读取，所以它看到你看到的：布局、颜色、
什么重叠什么。

可访问性树，浏览器为屏幕阅读器构建的结构。每个元素用它的角色和名称，
以及一个短句柄如 e13，模型可以用来行动。完全没有像素。

DOM，通过模型编写和框架在页面中运行的 JavaScript。这从菜单中拉出四
个链接，你在屏幕截图中看不到它们，因为那个菜单是折叠的。DOM 有一切，
隐藏部分包括在内。

真实的框架混合它们。Chrome 中的 Claude 读取树并采取屏幕截图。接下来：
它如何行动在它看到的之上。
-->

---
layout: default
class: v2
---

<div class="where"><b>Browser</b> · 它如何运行</div>

# 通过引用点击

<div class="lede">模型点击树中的一个名称，不是像素，回复说它点击了什么。</div>

<div class="fig">
<v-switch>
<template #0>
<div class="grid grid-cols-2 gap-6 items-center w-full tight">
<div>

```yaml
- navigation "Main" [ref=e7]:
  - list [ref=e8]:
    - listitem [ref=e9]:
      - link "Installation" [ref=e13]
```

</div>
<img src="./assets/img/aider-docs.png" class="shot" />
</div>
</template>
<template #1>
<div class="grid grid-cols-2 gap-6 items-center w-full tight">
<div>

```yaml
      - link "Installation" [ref=e13]
```

```json
browser_click {"element": "Installation link",
               "target": "e13"}
```

</div>
<img src="./assets/img/aider-docs.png" class="shot" />
</div>
</template>
<template #2>
<div class="grid grid-cols-2 gap-6 items-center w-full tight">
<div>

```json
browser_click {"element": "Installation link",
               "target": "e13"}
```

```js
// Ran Playwright code
await page
  .getByRole('navigation', { name: 'Main' })
  .getByRole('link', { name: 'Installation' })
  .click();
```

</div>
<img src="./assets/img/aider-install.png" class="shot" />
</div>
</template>
</v-switch>
</div>

<div class="source">10 月 7 日捕获 · Playwright MCP 0.0.83：快照、调用和回复。计算机使用通过屏幕坐标点击。</div>

<!--
讲解：大约 3 分钟。两次点击。

一次点击， exactly as it happened。

树给 Installation 链接一个句柄，e13。在右边，页面。

点击。模型调用 browser_click 用两个东西。target 是句柄。element 是
plain-English 描述，它是如果有人被要求批准点击时他们看到的内容。模型
写两者，所以那个描述是模型关于 e13 是什么的主张。

点击。回复显示真正发生了什么。它没有点击一个像素。它找到名为 Main 的
导航，然后名为 Installation 的链接，并点击那个。在右边，它登陆的页面。

另一种点击方式是通过屏幕坐标，这是计算机使用做的。它适用于任何应用，
当布局移动时它会点击错误的东西。句柄只在页面改变之前有效，然后模型
再次查看。

那么哪种看的方式更便宜？
-->

---
layout: default
class: v2
---

<div class="where"><b>Browser</b> · 它如何运行</div>

# 浏览器工具，并排

<div class="lede">Chrome 中的 Claude 工作像 Playwright：同样的三个视图，并通过引用点击。</div>

<div class="fig"><div class="tight">

<table class="map compact">
<colgroup><col style="width:9rem" /><col style="width:31%" /><col /><col /></colgroup>
<thead><tr><th></th><th>Playwright MCP</th><th>Chrome DevTools MCP</th><th>Chrome 中的 Claude</th></tr></thead>
<tbody>
<tr><td>屏幕截图</td><td><code>browser_take_screenshot</code></td><td><code>take_screenshot</code></td><td><code>computer</code> screenshot</td></tr>
<tr><td>树</td><td><code>browser_snapshot</code></td><td><code>take_snapshot</code></td><td><code>read_page</code></td></tr>
<tr><td>DOM</td><td><code>browser_evaluate</code></td><td><code>evaluate_script</code></td><td><code>javascript_tool</code></td></tr>
<tr><td>点击</td><td><code>browser_click</code> by ref</td><td><code>click</code> by uid</td><td><code>computer</code> by ref or x, y</td></tr>
<tr><td>打开一个页面</td><td><code>browser_navigate</code></td><td><code>navigate_page</code></td><td><code>navigate</code></td></tr>
</tbody>
</table>

</div></div>

<div class="source">Playwright MCP 0.0.83 tools/list，10 月 7 日 · Chrome DevTools MCP 工具参考 · Chrome 中 Claude 的工具定义，因为 Claude Code 通过 MCP 接收它们，10 月 8 日。</div>

<!--
讲解：大约 2 分钟。

你用 Playwright 看到的一切，其他浏览器工具也以同样的方式工作，包括
Chrome 中的 Claude。

Chrome 中的 Claude 是让你驱动你已经使用的 Chrome 的扩展。当你将它
连接到 Claude Code 时，它的工具通过 MCP 到达，像任何其他服务器一样。
我读取了它们的定义用于这张幻灯片。

逐行读。这里的每一个都有屏幕截图。每一个都有可访问性树：Playwright
称它为快照，Chrome DevTools 也是快照，Chrome 中的 Claude 称它为
read_page。每一个都可以在页面中运行 JavaScript 来到达 DOM。每一个
都通过树中的引用点击：Playwright 的 e13，DevTools 的 uid，Chrome 中
Claude 的 ref_1。Chrome 中的 Claude 也可以通过屏幕坐标点击，
computer-use 方式。

名称不同。想法是同样的三个视图和同样的点击。在任何上面学一次，你
可以阅读其余部分。

它们之间的真正差异不是工具。它是它们驱动谁的浏览器，这在两张幻灯片
后。首先，成本。
-->

---
layout: default
class: v2
---

<div class="where"><b>Browser</b> · 它花费什么</div>

# 一个页面，两种成本

<div class="lede">在这个页面上，树的成本几乎是屏幕截图的六倍。哪个更好用取决于页面。</div>

<div class="nums">
<div>
<div class="num">1,196</div>
<div class="num-label">1280 × 720 屏幕截图的 token</div>
</div>
<div>
<div class="num amber">6,802</div>
<div class="num-label">同样页面的树的 token，523 行</div>
</div>
</div>

<div class="source">屏幕截图：Anthropic 的 28 × 28 像素块。树：o200k tokenizer。WebVoyager（ACL 2024）：在 Booking 和 Google Flights 上，屏幕截图以 41 和 52 分领先于树。</div>

<!--
讲解：大约 2 分钟。

通常声称树是便宜选项，屏幕截图是昂贵的。在这个页面上，反过来。

屏幕截图：大约一千二百 token。Anthropic 用小块像素计算图像，所以成本
取决于图像大小，不是内容。

树：大约六千八百，因为文档页面有数百个链接，每一个是一行。

哪个工作更好？取决于页面。2024 年的一项研究发现，屏幕截图在视觉
网站如 Booking 和 Google Flights 上领先很多，在文本繁重的页面上大致
相等。所以在你的框架实际使用的页面上测量。

浏览器的最后一个问题：它能到达什么？
-->

---
layout: default
class: v2
---

<div class="where"><b>Browser</b> · 它能到达什么</div>

# 进入浏览器的三种方式

<div class="lede">每向外一步到达更多的你：一个 fresh 浏览器，你登录的 Chrome，你的整个屏幕。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d15-reach-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d15-reach-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d15-reach-2.svg" /></template>
</v-switch>
</div>

<div class="source">Playwright MCP README · Chrome DevTools MCP（Google，2025 年 9 月）· Vibium（J. Huggins）· Chrome 中 Claude 文档 · Anthropic 计算机使用文档。</div>

<!--
讲解：大约 2 分钟。两次点击。

框架的浏览器来自哪里决定它能到达什么。

一个 fresh 浏览器。Playwright、Google 的 DevTools 服务器和 Selenium
创建者的 Vibium 各自启动自己的浏览器，没有登录。这是检查你刚刚构建
的页面的正确工具。

点击。你自己的 Chrome。Chrome 中的 Claude 住在你已经使用的浏览器中，
所以它登录到你是的一切：电子邮件、GitHub、Canvas、你的银行。那是它
的重点，以及风险。

点击。你的整个屏幕。计算机使用根本不是浏览器工具。它看到屏幕并移动
鼠标，所以它到达你打开的每个应用程序。

它越能到达，它读取什么越重要，这是第四部分。
-->

---
layout: section
---

# 第四部分：提示注入

## 当模型读取的内容给出命令

<!--
讲解：第四部分大约 10 分钟。

第四部分，以及主题的改变。前三部分是关于每个通道能做什么。这一个
是关于所有三个中同时能出错什么。
-->

---
layout: default
class: v2
---

<div class="where"><b>Prompt injection</b> · 它是什么</div>

# 什么是提示注入

<div class="fig"><div>

<div class="prompt" style="font-size:1.7em">

**提示注入** 是模型读取的文本，它<em>跟随它，就像你写的</em>。

</div>

<div class="grid grid-cols-3 gap-6 mt-5">
<div class="card"><div class="col-head">通过 shell</div><p>你克隆的仓库中的 README 或文件</p></div>
<div class="card"><div class="col-head">通过 MCP</div><p>工具的描述，或它获取的问题</p></div>
<div class="card"><div class="col-head">通过浏览器</div><p>它访问的任何页面上的隐藏文本</p></div>
</div>

</div></div>

<div class="source">模型接收你的请求和它的工具返回的所有内容作为一 流文本。那个流中的没有任何东西标记哪些词是你的。</div>

<!--
讲解：大约 2 分钟。

首先是一个定义。提示注入是模型读取的文本，它跟随它，就像你写的。

为什么它工作：模型得到你的请求、它的指令，以及它的工具带回的所有
内容，全部作为一 流文本。那个流中没有任何东西说哪些词来自你，哪些
来自 web 页面。所以隐藏在模型读取的内容中的指令可以看起来完全像你
写的一个。

以及今天每个通道都带来别人写的文本。通过 shell：README，或你克隆的
仓库中的任何文件。通过 MCP：工具的描述，或工具获取的问题。通过
浏览器：它访问的任何页面，包括你在屏幕上看不到的文本。

首先，一个通过 MCP 的真实案例。
-->

---
layout: default
class: v2
---

<div class="where"><b>Prompt injection</b> · 通过 MCP</div>

# 服务器在你提示中的话

<div class="lede">工具描述是你的模型的一个指令。安装一个像代码一样作为你运行的服务器。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d12-poison-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d12-poison-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d12-poison-2.svg" /></template>
</v-switch>
</div>

<div class="source">在 Invariant Labs，"Tool Poisoning Attacks," 2025 年 4 月 1 日之后。MCP 规范：工具描述不受信任，除非服务器受信任。</div>

<!--
讲解：大约 3 分钟。两次点击。

回到 MCP 在两个调用中的步骤二：服务器的工具描述逐字进入模型的提示。

这个攻击在 2025 年 4 月发布。你安装的服务器有一个名为 add 的工具。
它的描述说它添加两个数字，然后，在你从未看到的文本中，告诉模型读取
你的 ssh 密钥并将它作为一个额外的参数传递。

点击。框架用每个描述做什么：将它放在每个请求中。模型将其读作来自
工具作者的指令。

点击。模型用 2 和 3 调用 add，加上一个包含你的私钥的注释。你的批准
提示可能只显示 add 2 3。服务器获取密钥。

一个服务器可以在你安装的那天 honest，并在下周更改它的描述。

所以一个 MCP 服务器到达它自己的代码能到达的任何东西，加上你的模型，
通过它的文本。

那么什么使注入 actually 危险？三个一起。
-->

---
layout: default
class: v2
---

<div class="where"><b>Prompt injection</b> · 所有三个通道</div>

# 致命的三位一体

<div class="lede">私有数据、不受信任的内容和一个发送数据出去的方式，在一个框架中，就是数据如何被盗。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d16-trifecta-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d16-trifecta-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d16-trifecta-2.svg" /></template>
  <template #3><img src="./assets/diagrams/d16-trifecta-3.svg" /></template>
</v-switch>
</div>

<div class="source">Simon Willison，"The lethal trifecta for AI agents," 2025 年 6 月 16 日。</div>

<!--
讲解：大约 2 分钟。三次点击。

Simon Willison 在 2025 年 6 月命名了这一点。

一个可以读取你的私有数据的框架是好的。

点击。一个读取不受信任的内容的框架，像任何一个人可以提交的 web 页面
或问题，是好的。

点击。一个可以发送数据出去的框架是好的。

点击。一个拥有所有三个的框架可以被它读取的东西告诉，将你的数据发送
到某处。那就是中毒的工具描述。带走任何一条腿，攻击失败。

所以哪些工具给你的框架哪些腿？
-->

---
layout: default
class: v2
---

<div class="where"><b>Prompt injection</b> · 每个通道</div>

# 每个通道中的三位一体

<div class="lede">每个通道可以自己供应所有三条腿。切断一条腿，攻击失败。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/d17-trifecta-grid-0.svg" /></template>
  <template #1><img src="./assets/diagrams/d17-trifecta-grid-1.svg" /></template>
  <template #2><img src="./assets/diagrams/d17-trifecta-grid-2.svg" /></template>
  <template #3><img src="./assets/diagrams/d17-trifecta-grid-3.svg" /></template>
</v-switch>
</div>

<div class="source">MCP 行发生了：Invariant Labs，2025 年 5 月 26 日。公共仓库上的一个问题导致带有 GitHub MCP 服务器的框架将私有仓库数据复制到公共 pull 请求。</div>

<!--
讲解：大约 3 分钟。三次点击。

点击。Shell。私有数据：你的 ssh 密钥，每个沙盒让它读取。不受信任的
内容：你克隆的仓库中的 README。出口：curl 或 git push。沙盒中的网络
块切断这条腿。

点击。MCP。这一行真的发生了，在 2025 年 5 月。研究者在公共仓库上
提交了一个问题。一个带有 GitHub MCP 服务器的框架，被要求查看开放的
问题，读取它，跟随它，从用户的私有仓库中提取数据并将其发布在公共
pull 请求中。服务器中没有任何东西有 bug。框架只是拥有所有三条腿。

点击。浏览器。你登录的会话，它读取的每个页面，它可以提交的任何表单。

当你向框架添加一个工具时，问它添加哪条腿。

这就是今天。在你走之前，出勤表格。
-->

---
layout: end
---
