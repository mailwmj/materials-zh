---
theme: ../theme
title: "L13: 浏览器扩展与可短信交互的智能助手"
info: |
  EECS 498 AASE — 讲座 13
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 浏览器扩展与可短信交互的智能助手

## 讲座 13 · 2026 年 10 月 13 日

<!--
方向：约 1 分钟。

下午好。在实验课04 中你们看到了模型如何查看网页并点击它。今天我们将打开你自己的 Chrome 中执行此操作的工具，然后我们将一个框架转变为你可以从手机短信交互的东西。

幻灯片上关于扩展的所有内容都是本周在我的机器上从它们的清单和代码中读取的。
-->

---
layout: default
class: v2
---

# 今日安排

<div class="fig"><div>

<table class="map">
<tbody>
<tr><td>第一部分：一个扩展，多种框架</td></tr>
<tr><td>演示</td></tr>
<tr><td>中场休息</td></tr>
<tr><td>第二部分：可短信交互的智能助手</td></tr>
<tr><td>问题</td></tr>
</tbody>
</table>

</div></div>

<div class="source">实验课04 的浏览器部分是背景：页面的三个视图，以及通过引用点击。</div>

<!--
方向：约 1 分钟。

两个部分。第一部分是你已经使用的 Chrome 中的浏览器扩展：哪些程序可以驱动它，它们如何到达它，以及它被允许做什么。然后一个演示。

休息后，第二部分：为什么你会想要一个始终运行的框架，然后我们逐块构建一个，直到你可以从手机短信交互它。这就是 OpenClaw 和 Hermes Agent。

两部分都要注意的一件事：框架永远不会改变。它是你熟悉的同一个循环。改变的是它的触及范围，首先进入你的浏览器，然后进入你的手机。
-->

---
layout: section
---

# 第一部分：一个扩展，多种框架

## 谁在驱动你的 Chrome，以及它们如何到达它

<!--
方向：第一部分约 15 分钟。

在实验课04 中你们看到了模型如何查看和点击页面。今天问题不同：扩展存在于你登录的 Chrome 中，几个不同的程序可以驱动它。哪些程序，以及它们如何进入？
-->

---
layout: default
class: v2
---

<div class="where"><b>浏览器</b> · 路径</div>

# 一个工具调用，从模型到你的标签页

<div class="lede">框架将每个浏览器工具调用传递给你 Chrome 中的一个扩展。扩展在标签页上执行操作，并以相同的方式将结果发回。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-path-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-path-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-path-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-path-3.svg" /></template>
</v-switch>
</div>

<div class="source">从 Claude Code 2.1.296 和 Claude in Chrome 扩展 1.0.100 简化，10 月 13 日读取。code.claude.com/docs/en/chrome。</div>

<!--
方向：约 2 分钟。三次点击。

模型像请求任何东西一样请求浏览器操作：一个工具调用。点击这个，读取那个页面。框架接收它，像任何其他工具调用一样。

点击。框架无法直接触及 Chrome，所以它将调用传递给一个存在于你 Chrome 中的扩展。这就是与实验课04 的区别。Playwright 启动了自己的全新浏览器。这是你已经在使用的 Chrome，以你的身份登录。

点击。扩展在标签页上执行工作：它点击、输入、读取页面。

点击。结果以调用的方式返回。扩展拍摄屏幕截图或读取页面文本，并将其传回框架。框架将其作为工具结果交给模型，像任何其他工具的输一样。模型从不直接与扩展对话。所有内容在两个方向上都通过框架。

到目前为止，一个框架。但扩展不关心哪个程序发送调用。
-->

---
layout: default
class: v2
---

<div class="where"><b>浏览器</b> · 谁驱动它</div>

# 谁可以驱动扩展

<div class="lede">你 Chrome 中的一个扩展接受来自其自身侧边栏、桌面应用、Claude Code，甚至云会话的工具调用。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-drivers-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-drivers-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-drivers-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-drivers-3.svg" /></template>
  <template #4><img src="./assets/diagrams/l13-drivers-4.svg" /></template>
</v-switch>
</div>

<div class="source">code.claude.com/docs/en/chrome · 讲师 Mac 上桌面应用和 Claude Code 的原生消息文件，10 月 13 日。</div>

<!--
方向：约 3 分钟。四次点击。

一个扩展坐在你的 Chrome 中，以你的身份登录，在你的标签页上执行操作。这张幻灯片的问题：谁可以向它发送工具调用？

点击。扩展本身。它在 Chrome 的侧边栏中有自己的聊天。你在页面旁边输入给 Claude，扩展就是整个框架。

点击。Claude 桌面应用。同一个扩展，从应用驱动。

点击。Claude Code，在你的终端或 VS Code 内部：claude --chrome，或会话中的 /chrome。这是你在编码时会使用的那个。更改前端，然后让它检查页面。

点击。一个不在你 Mac 上的会话，在云端运行。它仍然可以驱动你桌台上的 Chrome。

四个不同的程序，一个扩展，一套登录。如果两个浏览器连接，你用 /chrome 选择一个。那么调用如何到达那里？两种方式。
-->

---
layout: default
class: v2
---

<div class="where"><b>浏览器</b> · 它们如何到达它</div>

# 两种到达扩展的方式

<div class="lede">Chrome 在你的 Mac 上启动的一个辅助程序，或 Anthropic 通过互联网的中继。云会话只能使用中继。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-routes-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-routes-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-routes-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-routes-3.svg" /></template>
</v-switch>
</div>

<div class="source">从讲师 Mac 上 10 月 13 日显示的内容简化（Claude Code 2.1.296，扩展 1.0.100）以及 code.claude.com/docs/en/chrome。</div>

<!--
方向：约 3 分钟。三次点击。

左侧，想要发送工具调用的程序：你 Mac 上的 Claude Code 或桌面应用，以及云中的一个会话。右侧，你 Chrome 中的扩展。

点击。第一种方式留在你的 Mac 上。当你打开集成时，Claude Code 留下一个小文件，告诉 Chrome 关于一个辅助程序的信息。当扩展询问它时，Chrome 启动辅助程序，框架通过它到达扩展。没有任何内容离开你的机器。

点击。第二种方式通过 Anthropic 的中继，在互联网上。框架和扩展都向外连接它，它在它们之间传递消息。云中的会话没有其他方式进入，因为它不在你的 Mac 上。在我的 Mac 上，即使安装了辅助程序，Claude Code 也使用中继。

点击。双方都用你的 claude.ai 账户登录。这就是中继知道哪个 Chrome 是你的方式，也是为什么文档说扩展和 Claude Code 必须登录到同一个账户。

Codex 的扩展，我们一会儿会看到，只有第一种方式。首先：调用到达后扩展能做什么？
-->

---
layout: default
class: v2
---

<div class="where"><b>浏览器</b> · 权限</div>

# 扩展可以做什么

<div class="lede">它的清单请求每个站点的 DevTools 协议。</div>

<div class="fig"><div>

<table class="map compact">
<colgroup><col style="width:17rem" /><col /></colgroup>
<thead><tr><th>权限</th><th>它允许什么</th></tr></thead>
<tbody>
<tr><td><code>debugger</code></td><td>任何标签页上的 DevTools 协议：点击、输入、屏幕截图、运行 JavaScript</td></tr>
<tr><td><code>&lt;all_urls&gt;</code></td><td>每个站点，包括你登录的站点</td></tr>
<tr><td><code>scripting</code></td><td>在页面中的自己的脚本</td></tr>
<tr><td><code>tabs</code>, <code>tabGroups</code></td><td>打开标签页并将它们保持在一个组中</td></tr>
</tbody>
</table>

</div></div>

<div class="source">Claude in Chrome 1.0.100 的 manifest.json，10 月 13 日读取，总共 16 个权限。当它连接时，Chrome 显示自己的横幅："$1" 开始调试此浏览器。</div>

<!--
方向：约 2 分钟。

每个 Chrome 扩展都有一个 manifest.json，列出它被允许做什么，Chrome 强制执行它。这个请求了十六个权限。其中六个解释它如何工作。

debugger 是最大的一个。它给扩展 Chrome DevTools 协议，DevTools 面板使用的相同协议，在任何标签页上。Chrome 的文档称它为远程调试协议的替代传输。有了它你可以点击、输入、拍摄屏幕截图并在页面中运行任何 JavaScript。因为这是如此大的权限，Chrome 在窗口顶部显示一个横幅，当它连接时：Claude 开始调试此浏览器。如果你使用过扩展，你已经看到那个条。

all_urls 意味着它可以在每个站点上执行此操作，不是一个列表。包括你的银行、你的电子邮件、Canvas。

scripting 让它将自己的脚本放入页面。tabs 和 tabGroups 让它打开标签页并将它们保持在一起。幻灯片上没有显示：与计算机上程序对话和保存下载（如 GIF 录制）的权限。

以及浏览器是谁的？
-->

---
layout: default
class: v2
---

<div class="where"><b>浏览器</b> · 你的登录</div>

# 一个带有你的登录的扩展

<div class="lede">它可以到达你登录的每个站点。四件事阻止它。</div>

<div class="fig"><div class="grid grid-cols-2 gap-6">
<div class="card"><div class="col-head">你的登录</div><p>它共享你浏览器的登录状态：电子邮件、Canvas、你的银行</p></div>
<div class="card"><div class="col-head">它自己的标签页</div><p>它打开的标签页进入会话的一个组</p></div>
<div class="card"><div class="col-head">逐站点</div><p>它在每个新站点前询问。某些类型被阻止</p></div>
<div class="card"><div class="col-head">它为你停止</div><p>在登录页面或 CAPTCHA 上，它等待你</p></div>
</div></div>

<div class="source">code.claude.com/docs/en/chrome · support.claude.com/en/articles/12902428：阻止成人和盗版站点，金融站点首先询问。</div>

<!--
方向：约 2 分钟。

实验课04 中的 Playwright 启动了一个没有人登录的全新浏览器。扩展在你的浏览器中工作。文档 plainly 说明：它共享你浏览器的登录状态，所以它可以到达你已登录的任何站点。

三件事围绕它建立。

它自己的标签页。它不会接管你正在阅读的标签页。它打开的标签页进入一个与会话关联的 Chrome 标签组，它在其中工作。

逐站点。它第一次想要在某个站点上执行操作时，它会询问你，你可以为该会话允许该站点。某些类型的站点被 outright 阻止：成人和盗版。金融站点总是先询问。为了决定站点是哪种类型，扩展用一个返回类别的 Anthropic 服务检查地址。

它为你停止。在登录页面或 CAPTCHA 上，它暂停并请你自己处理。

记住实验课04 中的致命三合一：私有数据、不可信内容和一种发送数据出去的方式。一个在你已登录浏览器中的扩展在一个标签页中拥有全部三个。这四件事是站在它们和你的账户之间的东西。

Codex 也有一个扩展。它以非常不同的方式做同样的工作。
-->

---
layout: default
class: v2
---

<div class="where"><b>浏览器</b> · 比较</div>

# Codex 中的相同想法

<div class="lede">Codex 有自己的 Chrome 扩展，通过第一种方式到达：你 Mac 上的一个辅助程序。</div>

<div class="fig"><div>

<table class="map compact">
<colgroup><col style="width:13rem" /><col /><col /></colgroup>
<thead><tr><th></th><th>Chrome 中的 Claude</th><th>Codex 的 Chrome 扩展</th></tr></thead>
<tbody>
<tr><td>驱动者</td><td>其侧边栏、桌面应用、Claude Code、云会话</td><td>其侧边栏、Codex 应用和 CLI</td></tr>
<tr><td>通过什么到达</td><td>Mac 上的辅助程序，或 Anthropic 的中继</td><td>Mac 上的辅助程序</td></tr>
<tr><td>模型写入</td><td>每个操作一个工具调用</td><td>一个简短的 JavaScript 程序</td></tr>
<tr><td>还请求</td><td>这些都不是</td><td>你的历史记录、书签和会话</td></tr>
</tbody>
</table>

</div></div>

<div class="source">10 月 13 日在讲师的 Mac 上读取：Chrome 中的 Claude 1.0.100 和 OpenAI 的 ChatGPT 扩展 1.26.901.11451 · learn.chatgpt.com/docs/chrome-extension。</div>

<!--
方向：约 2 分钟。

Codex 的 Chrome 扩展在我机器上的同一个 Chrome 中，是相同的想法：一个扩展在你已登录的浏览器中，由多个程序驱动。它自己的侧边栏，以及 Codex 应用和命令行。

它只通过第一种方式到达，通过 Chrome 在你的 Mac 上启动的一个辅助程序。没有中继，所以云中的任何东西都无法驱动它。

最大的区别是模型写入的内容。Claude 的模型为每个操作生成一个工具调用：点击，然后读取，然后输入。Codex 的模型获得一个工具并编写一个简短的 JavaScript 程序，针对浏览器 API：找到按钮，点击它，读取结果。这是实验课04 中的代码模式，应用于浏览器。

它请求更多内容：你的历史记录、书签和最近会话。它的文档说它在每个新站点前询问，历史记录没有始终允许选项。

现在让我们观看一个工作。
-->

---
layout: section
---

# 演示

<!--
方向：约 17 分钟，四个步骤。左侧终端中的 Claude Code，右侧的 Brave，以你的身份登录，安装了 Claude 扩展。上课前：在 aider-practice 的 main 上，运行 bin/taskrweb start（http://127.0.0.1:5001）并检查 bin/taskr list 显示两个任务。在 aider-practice 中启动 claude --chrome，检查它连接，并在 /chrome 中选择这台笔记本电脑上的浏览器（assets/demo-runbook.md 说明如何判断是哪一个）。未排练：上课前运行所有四个步骤一次，然后 git checkout taskrweb/app.py 恢复 main。

让我们观看一个工作。左侧，终端中的 Claude Code。右侧，我的浏览器，以我的身份登录。我使用 Brave。它基于 Chromium，与 Chrome 相同的代码，所以 Chrome 扩展可以在其中运行。

方向：步骤 1，约 2 分钟。运行 claude --chrome，然后 /chrome。

/chrome 显示连接到我账户的每个浏览器。它们只是编号的，所以我选择这台笔记本电脑上的那个。这是关于谁驱动扩展的幻灯片中的选择。

方向：步骤 2，约 6 分钟。先自己打开 http://127.0.0.1:5001，让房间看到占位符。提示词："添加一个 / 页面，列出 taskr 的任务，然后在浏览器中打开 http://127.0.0.1:5001 并检查它渲染时没有控制台错误。" 它要求时批准对 taskrweb/app.py 的编辑。

这是最常见的用途：更改前端，然后检查你构建的内容。这是来自 aider-practice 的 taskr web 应用，在我的笔记本电脑上运行。现在它没有页面。你在第 11 课中构建第一个页面。

方向：在它出现的每个点上指出。如果 localhost 没有显示第一个站点提示，说明并指向步骤 3 中的它。

观察三件事。顶部的条：Claude 开始调试此浏览器。那是 debugger 权限，浏览器放置了它，不是 Claude。然后一个新的标签组。它没有触碰我正在阅读的标签页。以及它第一次在某个站点上执行操作时，它询问我。

方向：当它回答时，在第二个终端中运行 bin/taskr list 并比较。如果构建出错或运行时间长，停止它并在 aider-practice 中运行 git switch l13-demo-fallback：服务器重新加载一个完成的列表页面，你要求 Claude 检查那个页面。

它更改了代码，然后读取页面和控制台，看看更改是否有效。与运行测试相同的循环，对于输出是页面的代码。

方向：步骤 3，约 5 分钟。提示词："打开 github.com/eecs498-aase/aider 并告诉我有多少拉取请求是打开的。" 将窗口保持在仓库页面，远离通知和私有仓库。

现在一个我登录的站点。实验课04 中的 Playwright 启动了一个没有人登录的浏览器。这个是我的，所以 GitHub 知道我是谁，模型也通过页面知道。这就是带有你的登录的扩展，几分钟前的幻灯片。

方向：步骤 4，约 5 分钟。提示词："将你刚才在 taskr 页面上的检查写为一个 Playwright 测试，在一个新文件中。" 打开文件。不要运行它。

最后一个。我们刚刚观看的每一步都是一个模型调用。一旦它找到了通过你页面的路径，它可以将该路径写为一个测试，测试在每次推送时运行，没有模型。休息后我们会回到这一点。

方向：如果扩展无法连接，说明，跳到中场休息，休息后在 assets/captures/claude-in-chrome 中查看捕获。
-->

---
layout: default
class: v2
---

# 中场休息

<div class="fig"><div class="prompt">

你会用浏览器自动化做什么？

你的框架能用一个浏览器做什么而之前不能做的？

</div></div>

<!--
好了，Zoom，休息时间。

方向：约 5 分钟，与邻居一起。收集两三个答案，不附名字，休息后读回来。倾听浏览器添加了什么而 shell 没有的：查看它刚刚构建的页面、到达没有 API 的站点、在你登录的地方执行操作。
-->

---
layout: default
class: v2
---

<div class="where"><b>浏览器</b> · 用例</div>

# 浏览器自动化是做什么的

<div class="lede">三种工作：检查你构建的内容、到达没有 API 的内容，以及在没有模型的情况下重复它。</div>

<div class="fig"><div class="grid grid-cols-3 gap-6">
<div class="card"><div class="col-head">检查你构建的内容</div><p>打开你刚刚更改的页面，读取控制台，尝试表单</p></div>
<div class="card"><div class="col-head">到达没有 API 的内容</div><p>任何你登录的站点：提取数据，填写表单</p></div>
<div class="card"><div class="col-head">在没有模型的情况下重复它</div><p>将一次运行转变为在每次推送上运行的测试</p></div>
</div></div>

<div class="source">code.claude.com/docs/en/chrome 列出的用途：实时调试、设计检查、web 应用测试、已登录应用、数据提取、任务自动化 · playwright.dev/docs/test-agents。</div>

<!--
好了，Zoom，我们回来了。

方向：约 3 分钟。先读回两三个休息答案，不附名字，并将它们分类到这三个中。

检查你构建的内容。编码框架最常见的用途。它更改了你的前端，所以它打开页面，读取控制台错误，填写表单并查看发生了什么。与运行测试相同的循环，对于输出是页面的代码。

到达没有 API 的内容。你的课程站点、管理仪表板、工作中的表单。如果你登录了，它可以到达，这也是为什么它是危险的。

在没有模型的情况下重复它。模型每次都在循环中是缓慢的，并在每一步花费标记。一旦它找到了通过你应用的路径，让它将该路径写为一个测试，一个 Playwright 脚本，并在每次推送上运行，没有模型。Playwright 现在发布的代理正是这样做的：一个规划测试，一个编写它，一个在页面更改时修复它。

现在第二部分。到目前为止你一直在键盘旁观看。如果你不在呢？
-->

---
layout: section
---

# 第二部分：可短信交互的智能助手

## 为什么需要一个始终运行的框架

<!--
方向：第二部分约 30 分钟。

你可能见过人们给运行在家中 Mac mini 上的助手发短信：OpenClaw、Hermes Agent。先讲它们如何工作，再讲为什么。它解决什么问题？
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 问题</div>

# 会话在哪里结束

<div class="lede">框架只在你会话打开时存在。大多数你想要帮助的事情发生在它关闭时。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-day-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-day-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-day-2.svg" /></template>
</v-switch>
</div>

<div class="source">一个说明性的日子。</div>

<!--
方向：约 3 分钟。两次点击。

这里是一天。黄色条是你的会话：你有一段时间打开框架在你面前的小时数，比如十点到四点。在这个条内，它是一个很棒的工具。在外面，它不存在。

点击。现在是你实际上想要帮助的事情。凌晨两点，CI 在 main 上失败。七点四十五分，你在公交车上有一个修复的想法。六点半，审查者要求对你的拉取请求进行更改。晚上十点，你记得明天有一个截止日期。

点击。这些中的每一个都发生在会话关闭时。没有人在那里启动它。即使它在运行，它也无法联系到你，它不会记得你上周提到的截止日期。

这就是智能助手解决的问题。让我们准确命名缺失的内容。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 问题</div>

# 智能助手需要什么

<div class="lede">框架今天无法做的四件事，以及修复每件事的组件。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-needs-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-needs-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-needs-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-needs-3.svg" /></template>
  <template #4><img src="./assets/diagrams/l13-needs-4.svg" /></template>
</v-switch>
</div>

<div class="source">这四个组件是 OpenClaw、Hermes Agent 和带通道的 Claude Code 构建的。</div>

<!--
方向：约 3 分钟。四次点击。

左侧，你今天使用的框架，用四句话描述。每句都是它错过上一张幻灯片上某事的原因。

点击。它只在你坐在它前面时运行。所以它必须保持运行：一台不关闭的机器。

点击。你只能从你的笔记本电脑联系它。所以你必须能够从任何地方给它发短信：一个通道。

点击。它只在你询问时执行操作。所以它必须在某事发生时先行：触发器。

点击。它在每个会话后忘记你。所以它必须记住：记忆。

四个需求，四个组件。注意列表上没有的是什么：一个更智能的模型，或一个不同的框架。循环保持完全相同。我们将逐个添加组件。但首先必须改变一件事：不会有人在键盘前。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 1：无键盘</div>

# 从程序驱动框架

<div class="lede">框架可以从程序而不是人获取其消息，并将答案作为 JSON 发回。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-headless-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-headless-1.svg" /></template>
</v-switch>
</div>

<div class="source">Claude Code 的 stream-json 标志。Claude 桌面应用启动 claude 时使用这两个标志，根据讲师 Mac 上的 ps，10 月 13 日。Codex 有 codex exec --json。</div>

<!--
方向：约 2 分钟。一次点击。

今天框架从你在终端中输入获取输入，并向你显示它打印的内容。

点击。为了构建智能助手，一个程序必须执行输入。两个框架都支持它。Claude Code 有两个标志：以 JSON 接收消息，并以 JSON 发送答案。Codex 有带 JSON 选项的 codex exec。

你现在可能正在使用一个。这是我构建这些幻灯片时进程列表中的内容：Claude 桌面应用以完全相同的两个标志启动相同的 claude 程序。该应用程序是一个聊天窗口，为你输入框架。甚至批准按钮也通过同一管道。

将聊天窗口替换为读取你短信的程序，你就有了一个机器人的开始。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 2：始终运行</div>

# 一台不关闭的机器

<div class="lede">将框架放在一台永不关闭的机器上，一天中的每个事件都落在它里面。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-alwayson-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-alwayson-1.svg" /></template>
</v-switch>
</div>

<div class="source">同一个说明性的日子。</div>

<!--
方向：约 2 分钟。一次点击。

回到那一天。你的笔记本电脑打开几个小时。关闭盖子，框架停止，以及上一张幻灯片中的程序。

点击。所以智能助手生活在一台不关闭的机器上：家中的 Mac mini、一个小服务器，或云中的一个容器。现在时间线上的每个事件都在它运行时发生。

Mac mini 受欢迎有一个原因，我们两张幻灯片后会看到：iMessage 只从 Mac 工作。

这是我家里的样子。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 2：始终运行</div>

# 家里不关闭的机器

<img src="./assets/photos/l13-home-minis.jpg" class="mx-auto mt-2 rounded" style="max-height:20rem" />

<div class="source">讲师 2026 年 10 月的家中的书桌。</div>

<!--
方向：约 1 分钟。说明每台机器运行什么。

这是我家中书桌。三台 Mac mini，插电并一直开着。它们中的任何一台都不会关闭盖子。

然而，运行是不够的。它在运行，你在公交车上。你的消息如何到达它？
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 3：通道</div>

# 通道是一个启动轮次的 MCP 服务器

<div class="lede">通道将你的消息推入会话，模型用通道的回复工具回答。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-channel-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-channel-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-channel-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-channel-3.svg" /></template>
</v-switch>
</div>

<div class="source">Claude Code 通道，自 2026 年 3 月以来的研究预览版：code.claude.com/docs/en/channels。</div>

<!--
方向：约 4 分钟。三次点击。

Claude Code 的这个版本称为通道，它构建在你实验课04 中看到的 MCP 之上。通道是一个 MCP 服务器，像任何其他服务器一样插入框架。

你发短信：测试通过了吗？通道拾取消息并将其交给框架。

点击。框架将它放在模型前面，标记它来自哪里：这来自 iMessage。

点击。模型运行测试，然后通过调用通道提供的工具来回答：reply。框架将回复传递给通道，它出现在你的手机上。

点击。这里的新内容。在实验课04 中，每个 MCP 服务器都等待框架调用它。这个先启动轮次。相同的插件，但服务器先行。

这对于 Telegram 很容易，因为它有一个 API。iMessage 不是。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 3：通道</div>

# iMessage 如何进入

<div class="lede">Apple 没有 iMessage API。Mac 上的一个桥接读取消息应用的数据库并告诉应用发送什么。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-imessage-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-imessage-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-imessage-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-imessage-3.svg" /></template>
</v-switch>
</div>

<div class="source">Claude Code 的 iMessage 通道和 OpenClaw 使用的 imsg，两者都在 2026 年 10 月读取。两者都需要 macOS 完全磁盘访问和自动化权限。</div>

<!--
方向：约 3 分钟。三次点击。

Apple 没有发布用于发送或阅读你 iMessage 的 API。那么任何人都如何构建 iMessage 机器人？

在一台登录到你 Apple 账户的 Mac 上，消息应用将每条消息，发送和接收，保存到磁盘上的一个数据库文件中。

点击。桥接读取该文件并监视新消息。文件中的新消息是框架的新消息。

点击。为了发送，桥接告诉消息应用发送文本，就像你输入的一样。

点击。没有 API。桥接读取应用自己的数据库并驱动应用。你必须手动给它两个 macOS 权限：一个读取文件，一个控制消息。

所以 "它只是云中的 Claude Code" 的修正：对于 iMessage，某物必须是一台保持开启的 Mac。Telegram、Discord 和 Slack 机器人可以 anywhere 运行，因为这些服务有 API。

所以现在你可以给它发短信。但它仍然只回答。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 4：触发器</div>

# 触发器：让它先行

<div class="lede">触发器在某事发生时启动一轮，因此助手可以在你询问之前给你发短信。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-triggers-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-triggers-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-triggers-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-triggers-3.svg" /></template>
</v-switch>
</div>

<div class="source">说明性的。OpenClaw 的心跳每 30 分钟运行一次（默认）（docs.openclaw.ai）；Hermes Agent 的网关运行其 cron 作业。</div>

<!--
方向：约 3 分钟。三次点击。

通道只在你在你书写时启动一轮。触发器在某事发生时启动一轮。这里的触发器是来自 CI 的 webhook：main 刚刚失败。框架将其转变为模型的消息，带有一条常驻指令：找出原因。

点击。模型做如果你询问它会做的事情。它运行测试并读取失败。

点击。然后它通过通道给你发短信：main 是红色的，test_dates。

点击。没有人询问。这就是聊天机器人和智能助手之间的区别。日程表以相同的方式工作：每天早上八点，检查我的日历和电子邮件，并给我发短信说明什么重要。

有一种日程表值得按名称了解，因为每个始终运行的智能助手都有一个：心跳。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 4：触发器</div>

# 心跳

<div class="lede">心跳按计划唤醒智能助手检查一个短列表。大多数时候它什么也找不到并保持安静。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-heartbeat-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-heartbeat-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-heartbeat-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-heartbeat-3.svg" /></template>
</v-switch>
</div>

<div class="source">docs.openclaw.ai/gateway/heartbeat：默认每 30 分钟一次（Anthropic 订阅每小时一次），一个短清单，无事时 NO_REPLY。</div>

<!--
方向：约 3 分钟。三次点击。

webhook 需要外部的东西调用。你关心的很多事情不呼叫任何人：审查评论、日历中的截止日期。所以智能助手自己检查。

左侧，它的清单。你编写它，它很短：CI 在 main 上是否是绿色的、任何新的审查评论、明天是否有任何到期。

点击。每 30 分钟，心跳启动一轮。模型阅读列表并检查每个项目。大多数时候没有什么可说的。它用一个特殊的词回答，NO_REPLY，智能助手不发送任何内容。你永远不会看到这些轮次。

点击。在十一点，它找到了 something：审查者要求对拉取请求 42 进行更改。这次模型写了一个真实的答案，它发到你的手机上。

点击。注意事项：每次心跳都是一个模型调用，无论是否找到任何内容。每 30 分钟就是每天 48 次调用。所以保持列表简短，并给它活跃时间。OpenClaw 让你跳过你睡觉的小时，并在每次心跳中以全新会话运行，这样它不会每次都重新发送整个对话。

这就是让它感觉活着的因素。它是一个 cron 作业和一个清单，有保持安静的权利。

还剩一个需求。明天早上，它会记得这些中的任何一个吗？
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 步骤 5：记忆</div>

# 持久的记忆

<div class="lede">在会话结束之前，智能助手写下它学到的东西。下一个会话首先阅读它。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-memory-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-memory-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-memory-2.svg" /></template>
</v-switch>
</div>

<div class="source">说明性的。OpenClaw 在 Markdown 文件文件夹中保留 MEMORY.md 和 SOUL.md；Hermes Agent 保留 MEMORY.md 和 USER.md。</div>

<!--
方向：约 3 分钟。两次点击。

星期一，你告诉它两件事：你的考试在周四，保持短信简短。然后会话结束。通常，这就消失了。

点击。所以在它结束之前，智能助手写下它学到的东西，在一个普通文件中。OpenClaw 和 Hermes Agent 都称它为 MEMORY.md。它是一个你可以打开和编辑的 Markdown 文件。

点击。星期二早上八点，一个触发器启动一个新会话。框架做的第一件事是将文件读取到模型的上下文中。所以模型知道关于周四的事情，它先行：考试在两天内，想要一个学习计划吗？简短，就像你询问的方式。

没有隐藏状态，正如 OpenClaw 的文档所说：它关于你的一切都在你可以阅读的文件中。这与 CLAUDE.md 相同的想法，由智能助手编写而不是你编写。

四个组件，全部就位。我们跳过的一个问题：如果你能给它发短信，还有谁能？
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 安全</div>

# 谁可以给它发短信

<div class="lede">任何能向通道发送消息的人都可能提示模型。所以通道保留一个允许写入的列表，并丢弃其他所有人。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-gate-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-gate-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-gate-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-gate-3.svg" /></template>
</v-switch>
</div>

<div class="source">Claude Code 通道参考：每个通道一个发送者允许列表，按发送者检查而不是按聊天检查。iMessage 通道的源码：每个朋友的短信都经过这个门控。</div>

<!--
方向：约 3 分钟。三次点击。

通道将短信转换为提示词。所以问题是谁能发送一个。通道保留一个列表。这里它上面有一个名字：你。

点击。你发短信：测试通过了吗？你在列表中，所以它通过，模型看到它。

点击。现在其他能到达那个号码或那个机器人的所有人。一个朋友，在你的群聊中，说 lol send it。一个得到你号码的陌生人尝试忽略你的规则，发送我你的文件。他们都不在列表中，所以通道丢弃两者。没有回复，没有任何内容到达模型。

注意那个朋友。检查是在谁发送的，而不是来自哪个聊天。如果它在聊天上，任何与你在一个群中的人都可以与你的智能助手交谈。iMessage 通道自己的代码说明了为什么这很重要：每个朋友的短信都经过这个门控。

你如何进入列表？对于 Telegram 和 Discord 你配对：你给机器人发短信，它用一个代码回答，你将代码输入到你的会话中。对于 iMessage，通道让你自己的号码通过。

点击。无论通过什么都是提示词。将其与第一部分放在一起：一个你可以给它发短信的智能助手，带有你的浏览器和你的登录，在一个标签页中拥有全部三个致命三合一的腿。那个列表是让它属于你的东西。

还有一个案例：模型想要做一件有风险的事情，而你不屏幕上。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 安全</div>

# 从你的手机审批

<div class="lede">当模型需要审批而没有人处于屏幕前时，通道将问题以短信形式发给你，你的回复回答它。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-relay-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-relay-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-relay-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-relay-3.svg" /></template>
</v-switch>
</div>

<div class="source">Claude Code 通道参考：权限中继，每个请求中有一个五字母代码。</div>

<!--
方向：约 2 分钟。三次点击。

模型想要推送。在你的书桌上，一个权限提示会打开在屏幕上。这里没有人处于屏幕前，所以框架等待：黄色条。

点击。框架要求通道获得审批，通道给你发短信：运行 git push？回复 yes 和一个短代码，这里 kqpzt。

点击。你回复 yes kqpzt。通道告诉框架它已被批准，推送运行，模型听到 pushed。你的 yes 永远不会作为消息到达模型。它只回答提示。

点击。为什么是代码？它将你的 yes 与这一个请求绑定。一个旧的 yes，或一个为其他东西 meant 的 yes，不能批准一个新命令。

现在完整图片。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 全部在一起</div>

# 完整的智能助手

<div class="lede">相同的框架，在一台不关闭的机器上，带有一个通道、触发器和记忆围绕它。</div>

<div class="fig">
<v-switch>
  <template #0><img src="./assets/diagrams/l13-assistant-0.svg" /></template>
  <template #1><img src="./assets/diagrams/l13-assistant-1.svg" /></template>
  <template #2><img src="./assets/diagrams/l13-assistant-2.svg" /></template>
  <template #3><img src="./assets/diagrams/l13-assistant-3.svg" /></template>
</v-switch>
</div>

<div class="source">这部分的组件，在一张图片中。</div>

<!--
方向：约 2 分钟。三次点击。

将组件放在一起。中间，相同的框架，由程序驱动，在一台不关闭的机器上。

点击。一个通道，所以你可以给它发短信。

点击。触发器，所以它可以先行。

点击。记忆，所以它明天认识你。

这就是完整的东西。你听说过的每个始终运行的智能助手都是这张图片。不同之处在于谁运行它。
-->

---
layout: default
class: v2
---

<div class="where"><b>智能助手</b> · 比较</div>

# 包装它的三种方式

<div class="lede">相同的组件，三种托管方式。</div>

<div class="fig"><div>

<table class="map compact">
<colgroup><col style="width:11rem" /><col /><col /><col /></colgroup>
<thead><tr><th></th><th>OpenClaw/Hermes Agent</th><th>Claude Remote Control</th><th>OpenAI Dots</th></tr></thead>
<tbody>
<tr><td>运行在</td><td>你运行的机器</td><td>你的计算机，保持开启</td><td>OpenAI 的云</td></tr>
<tr><td>循环</td><td>他们自己的，或另一个框架</td><td>Claude Code</td><td>OpenAI 的，在 GPT-6 Astra 上</td></tr>
<tr><td>你通过以下方式联系它</td><td>iMessage、Telegram、Slack</td><td>Claude 应用</td><td>ChatGPT、Slack、Teams</td></tr>
<tr><td>它先行</td><td>心跳、cron</td><td>会话调度的任务</td><td>在后台处理目标工作</td></tr>
</tbody>
</table>

</div></div>

<div class="source">docs.openclaw.ai · hermes-agent.nousresearch.com · code.claude.com/docs/en/remote-control · OpenAI Dots，2026 年 9 月 29 日发布（TechCrunch、Al Jazeera）。2026 年 10 月 13 日读取。</div>

<!--
方向：约 4 分钟。

每个始终运行的智能助手都是上一张幻灯片的图片。不同之处在于谁托管它。

第一列：你托管它。OpenClaw 和 Hermes Agent 是开源的。你在不关闭的机器上运行它们，家中的 Mac mini 或服务器。OpenClaw 最初是 Clawdbot，变成了 Moltbot，自一月以来一直是 OpenClaw。它的中心是一个它称为 Gateway 的长时间运行进程，一次拥有每个聊天应用，大约 35 个，从 iMessage 通过 imsg。它有自己的智能体循环，或者它可以将轮次移交给 Codex 或 Claude Code。它的记忆是普通 Markdown 文件，它的心跳是我们刚刚看到的那个。Hermes Agent，来自 Nous Research，二月以相同的形状启动：一个也运行 cron 作业的网关进程，以及两个小文件加搜索的记忆。你拥有所有东西，包括安全。

第二列：你自己的计算机托管它。Claude Remote Control 是最轻量的版本。没有网关和没有通道。你在笔记本电脑上启动一个正常的 Claude Code 会话，远程控制开启，手机上的 Claude 应用成为同一会话的窗口。Claude 在你的机器上保持运行，带有你的文件。它甚至可以保持 Chrome 开启，所以从你的手机你可以让它使用你书桌上的浏览器，第一部分和第二部分一起。注意事项在上面的行中：你的计算机必须保持开启，并且只有会话本身调度了某些东西时它才先行。

第三列：OpenAI 托管它。OpenAI 在两周前的 DevDay 上宣布了 Dots。一个点是 ChatGPT 内部的一个始终运行的智能体，在 GPT-6 Astra 上运行，从 OpenAI 自己的云中工作。你给它一个目标并连接它可能使用的应用，它在你的对话之间继续处理目标。你通过 ChatGPT、Slack 和 Teams 联系它，短信 arriving。没有什么需要你自己运行，也没有什么你可以看到内部。

向下读取列，每次都是相同的四个组件：一个循环、一台不关闭的机器、一种联系它的方式，以及一种先行的方式。选择是谁运行机器。
-->

---
layout: default
class: v2
---

# 相同的框架，新的进出方式

<div class="fig"><div class="prompt">

今天框架永远不会改变。扩展和通道是新的进出方式。

</div></div>

<!--
方向：剩余的时间。

从开始的线索，最后一次。循环是你从 L06 以来知道的：模型请求一个工具，框架运行它。扩展给了它你的浏览器。通道给了它你的手机。一台不关闭的机器和一些触发器将它转变为一个智能助手。没有任何内容改变了框架。

问题。
-->

---
layout: end
---
