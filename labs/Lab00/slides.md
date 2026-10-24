---
theme: ../../lectures/theme
title: "Lab 00: 环境配置实验课"
info: |
  EECS 498 AASE — Lab 00 (环境配置实验课)
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 欢迎来到 AASE

## Lab 00 · 环境配置实验课 · 2026 年 8 月 31 日 – 9 月 1 日

<!--
今天的内容都不计分。请大声说两遍。
工作人员会逐一介绍自己，然后直接进入问卷调查——问卷需要十分钟，
你们可以在讲师讲解后续幻灯片的同时填写。
-->

---
layout: default
---

<div class="label">步骤 1 · 10 分钟 · 现在开始</div>

# 加入 Ed，完成问卷调查

<div class="mt-4 space-y-2">

1. 打开 Canvas。点击 **Ed** 链接。加入
2. 在 Ed 上找到问卷调查帖子。打开它

</div>

<div class="mt-10">

![Ed](./assets/ed-logo.png){width=92px class="rounded-2xl"}

</div>

<!--
请大声读出以下内容，而不是放在幻灯片上：
- 使用 umich.edu 登录。
- 它会询问你的 GitHub 用户名。没有用户名，周二就没有代码仓库。
- 如果你的笔记本电脑无法运行模型，请在硬件问题中说明。这个
  答案会为你分配一台能运行的机器。回答可以修改。
趁教室填问卷的时候，讲解接下来五张幻灯片。在大多数人举手之前不要开始
操作步骤。
-->

---
layout: default
---

<div class="label">模型 · Ollama</div>

# 在你的机器上运行本地 LLM

<div class="mt-4 space-y-2">

- `ollama pull` 下载模型。`ollama serve` 运行模型
- 免费、私有、无需 API 密钥。它就是你的
- 小模型可以装在笔记本电脑上。更大的模型需要 CAEN 的 GPU

</div>

<!--
趁他们填问卷的时候讲解。第一周的模型大约 3 GB；在 macOS 上打开 Ollama
应用会替换 ollama serve。如果你的笔记本带不动，问卷中的硬件问题会为你
分配一台能运行的机器。
-->

---
layout: default
---

<div class="label">工具 · Aider</div>

# 我们的编程工具

<div class="mt-4 space-y-2">

- 终端里的结对程序员：描述一个改动，它会编辑你的文件
- 每次编辑都是一次 git 提交，所以你随时可以撤销
- 它与 Ollama 运行的任何模型对话。这就是全部的技术栈

</div>

<!--
Aider 内部没有模型。它是一个客户端；你提供服务器，这也是课程不收费的
原因，也是你的模型只是一行配置而不是某个供应商的原因。不要在这里预览 L01
的内容；今天只需要让他们知道这些工具的存在。
-->

---
layout: section
---

# 操作步骤

## 按自己的节奏进行。有问题请举手。

<!--
到这里停止讲解。从这一点开始，学生们自己操作，工作人员巡视。
保留幻灯片，让他们可以自己翻阅。
-->

---
layout: default
---

<div class="label">步骤 2 · 在你的笔记本电脑上</div>

# 克隆 `aider-ollama`

```bash
git clone https://github.com/eecs498-aase/aider-ollama.git
cd aider-ollama
python3 -m pip install aider-install && aider-install
```

<div class="mt-4 space-y-2">

- 它的 **README** 有下面每一步的详细说明

</div>

<!--
把网址写在黑板上：github.com/eecs498-aase/aider-ollama
故意使用 HTTPS。今天没有人需要设置 SSH 密钥。
-->

---
layout: default
---

<div class="label">步骤 3 · 在你的笔记本电脑上</div>

# 运行一个模型

<div class="mt-4 space-y-2">

1. 从 **ollama.com/download** 安装 Ollama
2. 然后，在 `aider-ollama` 克隆目录中：

</div>

```bash
ollama serve &
ollama pull qwen3.5:4b
```

<div class="caption mt-4">大约 3 GB。等它下载完。</div>

---
layout: default
---

<div class="label">步骤 4 · 在你的笔记本电脑上</div>

# 让 Aider 指向它

```bash
cp .env.local .env
bin/check
aider
```

<div class="caption mt-6">全是绿色？你完成了最重要的部分。有红线？读一下，然后找助教。</div>

<!--
bin/check 测试 Aider 和模型之间的每一个环节。值得大声指出的一个陷阱：
一个没有设置条目的模型会得到 2048 个 token 的上下文，Aider 会截断你的
文件，模型开始发明那些实际上在几行之后的函数。这看起来像是一个愚蠢的
模型。这是配置错误，bin/check 能捕获它。
-->

---
layout: default
---

<div class="label">步骤 5 · 在 CAEN 机器上</div>

# 同样的事情，加上 GPU

```bash
bin/install-aider
bin/install-ollama

loginctl enable-linger $USER
ollama serve & ollama pull qwen3.5:9b
cp .env.local .env && bin/check
```

<div class="caption mt-4">只拉取一次。你的模型会在每台实验机器上都可用。</div>

<!--
两个安装脚本而不是一个下载，因为这些机器上没有人是 root。
其余部分是在 GPU 上重复步骤 3 和 4，模型大小翻倍。
enable-linger，只需一次，否则注销会杀死 ollama serve，它会被误认为是崩溃。
幕后不写在幻灯片上：home 是 NFS。Ollama 和模型特意放在那里，所以一次
拉取会让它们在所有机器上都可用。Aider 是不能从网络 home 启动的唯一东西，
所以 bin/install-aider 会在机器的本地磁盘上构建它，并留下一个启动器，
在没有它的机器上会重新构建，大约一分钟。如果有人问为什么在一台新机器上
第一次 `aider` 需要一分钟，这就是原因。他们永远不会注意到这一点。
还有第三种安排，你的笔记本电脑上的 Aider 通过 SSH 隧道连接到 CAEN GPU。
它在 README 里。今天不讲。
-->

---
layout: default
---

<div class="label">离开之前</div>

# 检查清单

<v-clicks>

- 在 Ed 上
- 问卷已提交
- `bin/check` 全是绿色

</v-clicks>

<div class="caption mt-6">有红色的？阅读仓库的 README，然后找助教。下周没有实验课。</div>

<!--
 plainly 说出最后一行。第 2 周是劳动节，没有实验课，设置截止日期是
9 月 11 日星期五。办公时间是这里到那里之间的场所。
-->

---
layout: end
---
