---
theme: ../theme
title: "L11: 护栏：错误、重试与审批门控"
info: |
  EECS 498 AASE — 讲座 11
  Applied Agentic Software Engineering, UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# 护栏：错误、重试与审批门控

## 讲座 11 · 2026 年 10 月 6 日

<!--
方向：约 1 分钟。

下午好。Apply 构建今晚 11:59 到期。

今天讲护栏。护栏是模型周围的代码，决定两件事：当某件事失败时发生什么，以及当模型请求做某件有风险的事情时发生什么。
-->

---
layout: default
---

<div class="label">一轮，三个检查点</div>

# 护栏放在哪里

<img src="./assets/diagrams/d1-loop.svg" class="mx-auto mt-4" style="max-height:19rem" />

<div class="caption mt-2">护栏是你代码中的一个检查。模型无法用言语绕过它。</div>

<!--
方向：约 3 分钟。

这是整场讲座的一张幻灯片。你构建的循环的一轮。模型调用。它请求一个工具。某人审批。工具运行。结果返回给模型。

护栏是你代码中某一点的检查。它在每一轮运行，无论模型是否配合。系统提示词中的一行不是护栏，因为模型可以忽略它。

三个部分。第一部分是失败：模型调用失败，或工具失败，这两者需要相反的处理。第二部分是你审批时的情况。第三部分是审批门控，当其他东西为你回答时：代码中的规则、第二个模型、或一个称为 Jev 的小型类型化分类器。
-->

---
layout: section
---

# 第一部分：当某事失败时

## 错误与重试

<!--
方向：第一部分约 25 分钟。

第一部分。某事失败了。首先我们对失败进行分类，然后观看 Codex 今天早上针对一个死端点测量的结果，然后我们编写一个你可以放入自己智能体中的重试循环。
-->

---
layout: default
---

<div class="label">在处理之前先分类</div>

# 三种失败

<img src="./assets/diagrams/d3-triage.svg" class="mx-auto mt-2" style="max-height:21rem" />

<!--
方向：约 4 分钟。

某件事失败了。在采取任何措施之前，先问三件事中哪一件失败了，因为每件事得到不同的答案。

模型调用。429 速率限制、来自网关的 502、超时。没有回复返回，所以什么也没发生，重新发送相同的请求是安全的。这是自动重试唯一所属的地方。

你的请求。400、401 密钥错误、长于上下文窗口的对话。端点理解了你并说不行。重新发送相同的字节，你会得到同样的不行。重试只会浪费时间。停止并告诉用户修复什么。

工具。退出状态 1，文件不存在。那不是你的框架失败。那是给模型的信息，所以它作为工具的结果返回，模型决定下一步做什么。L09 的连续错误限制是后盾，如果它永远无法恢复。

提问：黑客马拉松 1 中 Toolkit 偶尔出现的 502。它在哪个框？给一个答案。

模型调用。等待，然后重新发送。
-->

---
layout: default
---

<div class="label">10 月 6 日实测 · 一个对所有请求返回 502 的模型端点</div>

# 一个死端点，30 个请求

<img src="./assets/diagrams/d2-retries.svg" class="mx-auto mt-2" style="max-height:19rem" />

<div class="text-xl mt-2 text-center">Codex 0.160.0 · <b>6 次尝试 × 5 次发送 = 30</b> · 然后退出状态 1</div>

<!--
方向：约 5 分钟。10 月 6 日使用 Codex 0.160.0 测量，指向一个本地服务器，该服务器对每个请求返回 502 Bad Gateway。原始日志在 assets/live/captures 中。内部五次是从时间中读取的，不是 Codex 打印的。设置是 request_max_retries 和 stream_max_retries，每个模型提供者在 config.toml 中。401 行为来自 rust-v0.160.0 源码：codex-api/src/api_bridge.rs 将 401 映射为 UnexpectedStatus，protocol/src/error.rs 中的 retry_delay 将其视为可重试。内部循环 codex-client/src/retry.rs 只重试 429、5xx 和网络错误，所以 401 是六个独立请求。

今天早上我们将 Codex 指向一个模型端点，它已关闭，并记录到达它的每个请求。每个蓝色勾是一个请求，爆发下的数字是 Codex 的尝试次数。

Codex 在 24 秒内发送了 30 个请求，然后放弃并以状态 1 退出。

没有人配置 30。它是两个嵌套的重试循环。Codex 打印外层循环：Reconnecting，1 of 5，最多 5 of 5。那是六次尝试。在每次尝试内部，请求最多发送五次，等待时间翻倍：0.2、0.4、0.9、1.5 秒。尝试之间的等待也翻倍。六乘以五是三十。

这是从这张幻灯片得到的关键点。每一层重试都会将其下的层数相乘。你的智能体可能也有相同的两层而不知道，这就是下一张幻灯片。

退出状态与数量一样重要。它以状态 1 放弃，所以运行 codex exec 的脚本或 CI 作业可以将失败运行与良好运行区分开。当你的智能体放弃时，它以非零状态退出并说明原因。

我们也向它发送了一个 400。它只发送了一次就停止了。这是上一张幻灯片的分类，做对了。

401，错误的密钥，走向另一个方向。Codex 将其运行通过相同的五次重新连接，尽管尝试之间没有任何变化。按上一张幻灯片的分类，这是你的请求，重试它只会浪费时间。一个已发布的框架得到这两个中的一个错误。
-->

---
layout: default
---

<div class="label">现场 · 两个终端</div>

# 观看 Codex 放弃

<div class="grid grid-cols-2 gap-8 mt-6">
<div>

**左侧：一个总是失败的模型**

```text
$ python3 fake-endpoint.py 8799 502
```

- 五次爆发，间隔增长
</div>
<div>

**右侧：真实的 Codex，指向它**

```text
$ ./codex-502.sh 8799
```

- `Reconnecting... 1/5` 到 `5/5`
- 最后一行：`codex exit status: 1`
</div>
</div>

<div class="caption mt-6">约 25 秒。两个脚本都在 assets/live 中。</div>

<!--
方向：约 3 分钟。从 lectures/L11/assets/live 运行。先启动左侧面板。它打印 "listening on 127.0.0.1:8799, answering 502" 然后等待。在 Codex 在右侧面板运行之前，不会出现更多内容。codex-502.sh 需要 PATH 中有 codex，并使用一次性的 CODEX_HOME，所以演示者的配置和登录不会被触及。上课前运行一次。10 月 6 日花了 25 秒。备选方案：assets/live/captures/codex-502-endpoint.txt 和 codex-502-codex.txt。

两个面板。左侧，一个回答每个请求为 502 的假模型端点，并打印每个请求到达的时间。右侧，真实的 Codex，配置为将其作为模型提供者。

观看左侧面板。请求以五次爆发到达，爆发内的间隔增长。观看右侧面板计算重新连接次数直到五次。大约 25 秒后它放弃，最后一行是退出状态。
-->

---
layout: default
---

<div class="label">为你的智能体</div>

# 你拥有的重试循环

```python
RETRY = {429, 500, 502, 503, 504}

def call_model(send, request, deadline_s=60):
    start, delay = time.monotonic(), 0.5
    while True:
        try:
            return send(request)               # 使用 max_retries=0 构建的客户端
        except ModelHTTPError as e:
            if e.status not in RETRY:
                raise                          # 400、401、太长：停止
            wait = e.retry_after or delay * random.uniform(0.5, 1.5)
            if time.monotonic() - start + wait > deadline_s:
                raise                          # 放弃，并以非零状态退出
            time.sleep(wait)
            delay = min(delay * 2, 16)
```

<!--
方向：约 4 分钟。草图，不是库。ModelHTTPError 代表你的客户端引发的任何内容，携带状态和任何 Retry-After 值。

十五行中的五个决策。

只重试没有发生任何事情的状态码。其他所有内容立即引发。

通过翻倍退避，加上抖动。抖动是随机因子。如果这个房间里的三十个智能体同时命中同一个 429，都恰好等待一秒，它们都会在同一时刻回来并得到另一个 429。

在端点发送时使用 Retry-After。在 429 上，服务器告诉你何时回来。

整个调用的截止时间，不是尝试次数。五次尝试在四分之一秒和 16 秒时意义非常不同，你的用户感受到的是秒数，不是尝试次数。

以及一个循环。那是发送行上的注释。OpenAI Python SDK 默认重试两次，所以将你的客户端包装在你自己的循环中，你的每一次尝试就是三个请求。使用 max_retries 设置为零构建客户端并拥有这个循环，或两者都计算。

当它最终引发时，对话未受影响。这是 L07 的失败流规则，从另一侧来看：没有任何半成品进入历史记录，用户可以再次尝试。
-->

---
layout: default
---

<div class="label">工具侧</div>

# 超时不是无操作

```text
step 6   run_command   taskr add "buy milk"
         killed by the harness after 30 s
```

- 它保存了吗？框架无法知道
- 重试它，可能有两个
- 返回："30 秒后超时。它可能已运行。"

<div class="caption mt-6">只有在运行两次等同于运行一次时才重试调用。</div>

<!--
方向：约 3 分钟。

现在是工具侧。你的框架在 30 秒后杀死一个命令。taskr add 在被杀死之前保存了任务吗？你不知道。它可能写入了 ~/.taskr.json 然后挂起。

如果你的框架自行重试工具调用，用户现在可能有 buy milk 两次。失败的模型调用没有做任何事情，所以重新发送是安全的。超时的工具调用可能已经完成了一切。

这个词是幂等：运行两次等同于运行一次。读取是幂等的。taskr list 是。taskr add 不是，git push、发送邮件或扣款也不是。

所以框架永远不要自行重试工具。它返回超时作为结果，说命令可能已运行，模型可以在再次执行前用 taskr list 检查。

提问：命名一个你的智能体本周运行的、运行两次不安全的命令。
-->

---
layout: section
---

# 第二部分：你审批什么

## 当智能体询问你时

<!--
方向：第二部分约 10 分钟。

第二部分。当智能体询问你时。审批提示前的人能判断什么和不能判断什么，以及 Codex 和 Aider 如何询问。
-->

---
layout: two-col
---

## 一个编辑

```text
write_file(path, content)
```

变更在调用中。你读取一个 diff，git 可以将其恢复。

::right::

## 一个命令

```text
run_command(command)
```

调用是一个程序。你看到运行什么，看不到它 touches 什么。

<div class="caption mt-10">相同的 y/n 提示，两种不同的工作。</div>

<!--
方向：约 2 分钟。

审批提示要求你做两件事之一。

批准一个编辑，调用携带变更。diff 是你可以阅读和判断的东西，git 可以恢复它。

批准一个命令，调用携带一个程序。文本告诉你运行什么，没有告诉你它 touches 什么。git 不覆盖 shell 命令。主目录中的 rm、pip install、curl piped into sh：你的仓库不记录任何这些。

这周 Codex 问你的所有内容都是这两者之一。
-->

---
layout: default
---

<div class="label">命令提示隐藏的三件事</div>

# 提示不显示什么

|| 你看到 | 你看不到 |
|---|---|
|| "清理测试状态" | 它是由编写命令的模型编写的 |
|| `python -c "...main(['add', ...])"` | 它保存到用户的 `~/.taskr.json` |
|| `pytest`，在第 12 步 | `tests/conftest.py`，在第 11 步写入 |

<div class="caption mt-6">审批运行中的命令状态。永远不要批准解释。</div>

<!--
方向：约 4 分钟。第 2 行和第 3 行来自 L09 的排练运行 2 和 3，9 月 29 日。

三件你无法从提示中判断的事情。

模型的 stated 原因。Codex 通常在询问前写一行。那一行来自编写命令的同一个模型。它不是命令做该行所说内容的证据。

一个有副作用的测试，来自 L09 的排练运行。它看起来像一个测试，它也是。它还运行真实的 add，保存到用户的任务列表。两次运行都在其中留下了两个垃圾任务，因为排练批准了所有这些命令，因为它们的文本中没有提到主目录。

一个指向文件的命令。pytest 运行仓库中的每个 conftest.py。如果第 11 步写了一个，在第 12 步批准 pytest 就是批准该文件的任何内容。make 和 npm test 是一样的。

所以你审批的单位是一个状态中的命令：文本，加上自从你上次读取仓库以来写入的所有内容。

提问：举手，如果你本周批准了一个 python -c 或测试运行，而不知道它会写入什么。
-->

---
layout: default
---

<div class="label">Codex 0.160.0 · Aider 5dc9490b</div>

# Codex 和 Aider 询问方式不同

<div class="grid grid-cols-2 gap-8 mt-6">
<div>

**Codex：一个沙箱，然后一个提示**

- `--sandbox`: `read-only`, `workspace-write`, `danger-full-access`
- `--ask-for-approval`: `on-request` 或 `never`
- 只有离开沙箱的内容才询问
</div>
<div>

**Aider：只有一个提示**

- `(Y)es/(N)o/(D)on't ask again [Yes]`
- 回车表示是
- `--yes-always`: shell 命令得到 *否*
- "Don't ask again" 表示否
</div>
</div>

<!--
方向：约 4 分钟。Codex 标志来自 0.160.0 的 codex --help，实验室 03 安装的版本。Aider 行号在 5dc9490b：confirm_ask 在 aider/io.py:807，--yes-always 答案在 :867，never_prompts 在 :823 检查并在 :903 添加。

Codex 将问题分成两部分。沙箱限制命令可以触及的内容：只读、工作区内的写入、或一切。审批策略说明当命令需要离开沙箱时谁被询问。使用 on-request，是你。使用 never，没有人，失败直接返回给模型。使用空配置运行 codex doctor 报告默认按请求审批，文件系统和网络受限。实验室 03 的配置没有设置审批，所以那是你一直运行的默认设置。

Aider 没有沙箱，只有提示，以及三个大多数人都会搞错的细节。默认是 yes，所以回车运行命令。--yes-always 并不意味着对所有事情都说 yes。Shell 命令需要明确的 yes，使用 --yes-always 时，它们得到 no。那是第 867 行：有人决定无人值守的 Aider 不应该运行命令。"Don't ask again" 不是允许规则。它存储那个确切的命令并回答 no，现在和会话中的每个 later 时间。它是一个由疲惫的人编写的拒绝规则，当 Aider 退出时它消失了。

Codex 还有一个标志，--approve-for-me。它将这些提示发送给一个审查智能体而不是你。那是第三部分。
-->

---
layout: default
---

<div class="label">中场休息</div>

# 你没有阅读就批准的内容

挑选一件 Codex 或 Claude Code 这周要求你批准而你未经阅读就批准的事情。

一个程序需要知道什么才能为你回答它？

<!--
好了，Zoom，休息时间。

方向：约 5 分钟，纸上或与邻居一起。如果你有 Codex 的会话历史，可以看。在房间里走动，收集两三个读回来，不附名字。倾听那些命名命令文本之外的内容的答案：文件包含什么、用户要求什么、它是否可以撤销。
-->

---
layout: section
---

# 第三部分：审批门控

## 当其他东西回答时

<!--
好了，Zoom，我们回来了。

方向：第三部分约 27 分钟。

先一个字。Analyze 阶段结束时的阶段门控是一个评分测试。审批门控是回答审批提示的代码，使人不必这样做。今天只是第二个。
-->

---
layout: default
---

<div class="label">为什么需要审批门控</div>

# 大多数提示得到 yes

<div class="grid grid-cols-3 gap-8 mt-10">
  <div><div class="font-sans font-semibold" style="font-size:3.2rem;line-height:1;color:#1A1A1A">93%</div>
       <div class="text-lg mt-2">的 Claude Code 提示被批准</div></div>
  <div><div class="font-sans font-semibold" style="font-size:3.2rem;line-height:1;color:#1A1A1A">720</div>
       <div class="text-lg mt-2">10,000 次 Codex 操作中离开沙箱的次数</div></div>
  <div><div class="font-sans font-semibold" style="font-size:3.2rem;line-height:1;color:#1A1A1A">7</div>
       <div class="text-lg mt-2">这 720 次中被审查器拒绝的次数</div></div>
</div>

<div class="caption mt-10">一个几乎总是 yes 的提示停止被阅读。</div>

<!--
方向：约 3 分钟。先读回两三个休息答案，不附名字。

Anthropic 测量 Claude Code 用户批准 93% 的权限提示。这来自他们 2026 年 3 月 25 日关于自动模式的工程文章。

OpenAI 关于 Codex 自动审查的文章，2026 年 4 月 30 日，给出了 10,000 次操作的内部快照。9,280 次在沙箱内运行，从不询问任何人。720 次需要审批。他们的审查器批准了 713 次，拒绝了 7 次。在这 7 次中，四次通过更安全的路径继续，三次停下来询问用户。

而这对人的影响。OpenAI 发现相当一部分内部用户对每个以 python 开头的命令都有一个允许规则。到会话的第 30 步，提示是一个按键，不是一个决策。其他东西必须回答这些中的大多数。
-->

---
layout: default
---

<div class="label">三样东西可以回答</div>

# 三种审批门控

|| | 代码规则 | 审查模型 | 类型化分类器 |
|---|---|---|---|
|| 判断 | 文本 | 意图和效果 | 意图和效果 |
|| 返回 | 匹配或不匹配 | 一个决策，一个原因 | 概率 |
|| 每次调用 | 免费 | 一次模型调用 | 70 到 500 毫秒，供应商 |
|| 示例 | 前缀规则 | Codex 自动审查 | Jev |

<!--
方向：约 2 分钟。

代码规则是 L09 承诺的内容：匹配请求的调用，拒绝获胜，任何未匹配的询问。确定性的、免费的，你可以阅读它们。它们匹配文本，所以 python 的允许规则无论 conftest.py 说什么都会触发。

审查模型是第二次 LLM 调用，阅读对话和提议的动作并决定。它可以判断意图和效果，它给智能体一个原因，它每次看到一个动作就花费一次模型调用。

类型化分类器是最新的选项。Jev，来自 TypeSafe。你问它类型化的问题，它返回概率，不是文本。

没有已发布的系统单独使用其中之一。代码先行，模型只看到代码无法决定的内容。
-->

---
layout: default
---

<div class="label">两个已发布的审查模型</div>

# 自动模式和自动审查

|| | Claude Code 自动模式 | Codex 自动审查 |
|---|---|---|
|| 到达它的内容 | 通过允许列表和仓库内编辑 | 只有离开沙箱的内容 |
|| 审查器 | Sonnet 4.6，两个阶段 | GPT-5.4 Thinking，低精力 |
|| 一个拒绝 | 一个工具结果，带原因 | 一个工具结果，带原因 |
|| 停止运行 | 连续 3 次拒绝 | 连续 3 次拒绝 |

<!--
方向：约 3 分钟。来源：Anthropic 工程，"我们如何构建 Claude Code 自动模式"，2026 年 3 月 25 日。OpenAI 对齐博客，"智能体动作的自动审查"，2026 年 4 月 30 日，以及 Codex 自动审查文档。

两者都将代码放在模型之前。Claude Code 允许一组固定的工具，这些工具不能更改任何东西，加上你自己的允许规则，以及项目内的文件编辑，因为 git 可以恢复它们。进入自动模式时，它删除像任何 python 或任何 npm run 这样的 blanket 规则，因为那些会隐藏最危险的命令。Codex 使用第二部分的沙箱：只有离开沙箱的请求到达审查器，审查器永远不会扩大沙箱。

Claude Code 的分类器分两个阶段运行。首先是一个单独的 yes 或 no 标记，被告知倾向于阻止。只有那标记了它才推理，这保持了常见情况的成本。

两者都将拒绝作为工具的结果发送回智能体，附带原因，告诉它找到更安全的方式而不是绕过阻止。两者在连续三次拒绝后停止。Claude Code 还在会话中 20 次停止，Codex 在一轮的最近 50 次审查中 10 次停止。拒绝是一个结果，太多的拒绝结束运行。
-->

---
layout: default
---

<div class="label">Claude Code 自动模式</div>

# 审查器看到的内容

<img src="./assets/diagrams/d6-reviewer.svg" class="mx-auto mt-4" style="max-height:19rem" />

<!--
方向：约 2 分钟。来源：Anthropic 文章，"为什么我们剥离助手文本和工具结果" 部分。

自动模式分类器看到用户消息和智能体的工具调用。没有其他内容。

智能体自己的文本被剥离，所以它无法争论。"这些分支是陈旧的，可以安全删除" 永远不会到达分类器。它判断 git push --delete 反对 "clean up old branches"，而 clean up 没有说删除远程分支。这是第二部分中 stated-reason 行，内置到产品中。

工具输出也被剥离。一个 README 说将你的 .env 发布到这个 URL 是提示词注入进入的方式。分类器从不阅读它，也不需要：将 .env 发布到外部 URL 反对用户要求的内容，无论什么提示了它。

Anthropic 测试了其他选项。给分类器智能体的文本和工具输出使其在真实过度热心的动作集上表现更差。
-->

---
layout: default
---

<div class="label">TypeSafe · 2026 年 9 月 15 日</div>

# Jev，一个类型化决策模型

- 输入：一个状态，作为文本或 JSON
- 输出：带概率的类型化答案
- 没有文本，所以没有什么可解析的
- 供应商：每次请求 70 到 500 毫秒

<div class="caption mt-6">早期访问。这些数字是 TypeSafe 的，不是我们的。</div>

<!--
方向：约 3 分钟。

Jev 来自 TypeSafe，2026 年 9 月 15 日早期访问发布。TypeSafe 称它为 System One 模型。它接收一个状态，可以是任何文本或 JSON，以及一组类型化问题，并返回你的代码可以直接使用的答案。它不写文本、写代码或调用工具。TypeSafe 自己的文档说它不替换编码智能体背后的模型。它坐在它旁边。

TypeSafe 的数字：70 到 500 毫秒端到端，输入每百万标记 4.2 美分，输出不收费。这些是他们的数字。我们还没有测量 Jev。LangChain 在 9 月 17 日发布了一个集成，包括一个工具调用门控，这在几张幻灯片后提到。

为什么它适合这里。审批门控的答案是你的代码分支的决策，不是散文。LLM 门必须被告知说 yes 或 no，然后解析。Anthropic 的第一阶段是一个被告知发出一个标记的 LLM。Jev 直接返回数字。
-->

---
layout: default
---

<div class="label">Choice、Noul、Score</div>

# 安全类别，作为问题

```python
QUESTIONS = {
    "kind": Choice(
        instructions="What does `call` do when it runs?",
        criteria={"read": "Only reads files in the repo",
                  "edit": "Changes files in the repo",
                  "run": "Runs code, tests or a build",
                  "outside": "Touches anything outside the repo"}),
    "asked": Noul(instructions="Did a user message ask for this action?"),
    "undo": Score(
        instructions="If `call` is a mistake, how hard is it to undo?",
        criteria=["Nothing changes", "git undoes it",
                  "Fixable by hand", "Cannot be undone"]),
}
```

<!--
方向：约 3 分钟。代码针对 typesafe_sdk 0.7.1 编写，并在 10 月 6 日检查 against 一个本地 API 占位符，检查请求形状和其他所有内容。没有显示 Jev 输出，因为没有测量。

三种问题类型，每种一个，全部在一个请求中。

Choice 从一组中挑选一个选项，每个选项有一个概率和置信度。Noul 是一个 yes 或 no 问题，回答为 yes 的概率。Score 将状态放置在你编写的级别上，从零开始编号，答案可以落在两个级别之间。

这些是你在提示前问自己但不说出口的问题。这是什么类型的动作？有人要求这个吗？如果错了撤销它有多难？写下来，它们是安全类别。

它们是你编写和可以阅读的 plain English。策略住在你可以审查和更改的地方。
-->

---
layout: default
---

<div class="label">代码优先，然后 Jev，然后人</div>

# 代码和 Jev 中的审批门控

<img src="./assets/diagrams/d4-gate.svg" class="mx-auto mt-2" style="max-height:20rem" />

<!--
方向：约 3 分钟。

这是组合的图片。

代码先行，对于你可以从文本决定的所有内容，永远不能出错。仓库外的路径被拒绝。仓库内的读取被允许。没有人，人或模型，被询问任何一方。

其余的去 Jev，带三个问题。Jev 用概率回答，你代码中的阈值将它们转化为三个结果之一。中间结果是一个人。

为什么两者都有。代码 alone 只能匹配文本，所以它无法判断用户是否要求了什么。Jev alone 是一个模型，有时它会错在代码可以确定决定的事情上。规则获取确定的情况，Jev 获取判断调用，人获取没有人确定的内容。
-->

---
layout: default
---

<div class="label">同一个门控，用 Python</div>

# 阈值是代码

```python
def gate(call, user_msgs):
    if outside_repo(call) or denied_by_rule(call):
        return "deny"                      # 代码：没有询问任何人
    a = jev.system_one(state={"user": user_msgs, "call": call},
                       questions=QUESTIONS).answers
    undo, asked, kind = a["undo"].score, a["asked"].noul, a["kind"]
    if undo >= 2 and asked < 0.5:
        return "deny"                      # 难以撤销，不是被要求的
    if kind.choice == "read" and kind.confidence > 0.9:
        return "allow"
    if undo < 1 and asked > 0.9:
        return "allow"
    return "ask"                           # 中间带
```

<!--
方向：约 3 分钟。一个教学草图，在 10 月 6 日检查 against typesafe_sdk 0.7.1 和一个本地 API 占位符。Jev 是否允许在 Analyze 构建中是规范的决定，不是这场讲座的。不要承诺它。

作为代码的图片。首先规则。然后一个 Jev 请求，带三个问题。然后阈值：拒绝难以撤销且没有人要求的内容，允许高置信度读取和用户要求的廉价操作，并将所有其他内容发送给人。

从第二部分走一个调用通过它。写入用户任务列表的 python -c 测试。它的文本中没有任何内容离开仓库，所以没有规则触发。Jev 只有在状态说明 main with add 写入什么时才能捕获它。Jev 看到你放入状态的内容，像任何审查器一样。

这给你两件事。一个原因：Jev 返回数字，所以智能体得到的消息是你编写的，来自触发的任何阈值，每个工具调用仍然恰好得到一个结果。以及一个保证。那是两张幻灯片后。
-->

---
layout: default
---

<div class="label">在哪里画线</div>

# 一个分数，两个策略

<div class="text-lg mt-1">两条线将一个概率转化为三个答案：运行它、询问一个人、阻止它。</div>

<div class="mt-12 text-base" style="display:grid;grid-template-columns:17rem 1fr 10rem;column-gap:1.5rem;row-gap:3.4rem;align-items:center">
  <div><b>TypeSafe 食谱，严格</b></div>
  <div style="position:relative;height:1.6rem;background:var(--c-bg-2);border-radius:4px">
    <div style="position:absolute;top:0;bottom:0;display:flex;align-items:center;justify-content:center;color:#fff;font-size:.75rem;font-weight:600;left:70%;right:0;background:var(--c-rose);border-radius:0 4px 4px 0">blocked</div>
    <div style="position:absolute;top:1.85rem;transform:translateX(-50%);font-size:.8rem;color:var(--c-ink-muted);white-space:nowrap;left:70%">block at 0.70</div>
    <div style="position:absolute;left:74%;top:-.4rem;bottom:-.4rem;width:3px;background:var(--c-ink)"></div>
    <div style="position:absolute;left:74%;top:-1.6rem;transform:translateX(-50%);font-size:.85rem;font-weight:700;color:var(--c-ink)">0.74</div>
  </div>
  <div>被阻止</div>
  <div><b>TypeSafe 食谱，宽松</b></div>
  <div style="position:relative;height:1.6rem;background:var(--c-bg-2);border-radius:4px">
    <div style="position:absolute;top:0;bottom:0;display:flex;align-items:center;justify-content:center;color:#fff;font-size:.75rem;font-weight:600;left:85%;right:0;background:var(--c-rose);border-radius:0 4px 4px 0">blocked</div>
    <div style="position:absolute;top:1.85rem;transform:translateX(-50%);font-size:.8rem;color:var(--c-ink-muted);white-space:nowrap;left:85%">block at 0.85</div>
    <div style="position:absolute;left:74%;top:-.4rem;bottom:-.4rem;width:3px;background:var(--c-ink)"></div>
    <div style="position:absolute;left:74%;top:-1.6rem;transform:translateX(-50%);font-size:.85rem;font-weight:700;color:var(--c-ink)">0.74</div>
  </div>
  <div>人决定</div>
  <div><b>LangChain 中间件</b></div>
  <div style="position:relative;height:1.6rem;background:var(--c-bg-2);border-radius:4px">
    <div style="position:absolute;top:0;bottom:0;display:flex;align-items:center;justify-content:center;color:#fff;font-size:.75rem;font-weight:600;left:0;width:50%;background:var(--c-emerald);border-radius:4px 0 0 4px">runs</div>
    <div style="position:absolute;top:0;bottom:0;display:flex;align-items:center;justify-content:center;color:#fff;font-size:.75rem;font-weight:600;left:50%;right:0;background:var(--c-rose);border-radius:0 4px 4px 0">blocked</div>
    <div style="position:absolute;top:1.85rem;transform:translateX(-50%);font-size:.8rem;color:var(--c-ink-muted);white-space:nowrap;left:50%">block at 0.5</div>
  </div>
  <div>从不询问</div>
</div>

<div class="caption" style="margin-top:3rem">概率没有移动。策略移动了。</div>

<!--
方向：约 2 分钟。条形图按比例绘制。食谱给出每个策略的阻止线，但没有审查线，只知道 0.74 高于宽松的那个，所以条形图没有绘制审查线。LangChain 的 0.5 适用于它自己的问题，risky or insufficiently authorized，所以它的条形图没有 0.74 标记。

规则给你匹配或不匹配。审查器给你 yes 或 no。概率免费给你第三个答案：不确定。那就是你发送给人的带。

TypeSafe 的护栏食谱将其写为两个阈值：一条审查线，在其上是一条阻止线。他们的示例：Jev 给一条越狱消息 0.74 的概率。那是顶部两个条形上的黑色标记。在他们的严格策略下，阻止线是 0.70，所以它被阻止。在宽松策略下是 0.85，所以它去一个人。概率没有移动。策略移动了，策略是你代码中的两个数字。用他们的话说，TypeSafe 提供评估，你的应用程序拥有决策。

LangChain 的 AutoModeMiddleware，在 langchain-typesafe 0.0.1a3 中，在 0.5 有一条线，没有人。它的文档字符串说它阻止 risky 调用并且不请求审批。其中一个选择值得复制：如果分类器调用失败，工具不运行。这是第一部分的再次出现，从门控的一侧。
-->

---
layout: default
---

<div class="label">审批门控不能承诺什么</div>

# 测量，不是信任

|| Claude Code 自动模式 | 安全操作被阻止 | 过度热心放行 |
|---|---|---|
|| 两个阶段 | 0.4% | 17% |
|| 仅第一阶段 | 8.5% | 6.6% |

|| Codex 自动审查 | 拒绝 |
|---|---|
|| 合成过度热心操作 | 90.3% |
|| 合成提示词注入 | 99.3% |

<div class="caption mt-6">审批门控是一个分类器。分类器被评估。</div>

<!--
方向：约 3 分钟。

Anthropic 完整自动模式管线的数字。在 10,000 次真实工具调用上，它阻止了 0.4% 的安全操作。在他们事故日志中的 52 个真实过度热心操作上，它放行了 17%。仅第一阶段放行了 6.6%，但阻止了 8.5% 的安全流量。那个交易是两行中的整个设计问题。

OpenAI 自动审查的数字：它拒绝了 90.3% 的合成过度热心操作和 99.3% 的合成提示词注入。用他们的话说，它不应该被视为安全保证，红队测试发现了它可以被说服批准的情况。

他们的建议，当有太多审查时，是首先修复边界：像 cargo test 这样的窄规则，而不是 python。门控不是放宽沙箱的理由。

这个幻灯片上的每个数字都来自一个评估：一个标记的动作集，一个安全操作阻止率，一个危险操作放行率。你的审批门控是你的智能体中第一个值得这样测量的事物之一。
-->

---
layout: default
---

<div class="label">这周</div>

# 这周的日期

|| 时间 | 内容 |
|---|---|
|| 今晚，11:59 PM | Apply 构建到期 |
|| 周四 | Analyze 规范，在 L12 之后 |
|| 你的实验课04 之前 | 技能设计文档起草完成 |
|| 10 月 12 日或 13 日 | 实验课04 |

<!--
方向：约 2 分钟。

首先今晚。Apply 构建在 11:59 到期。

周四你在 L12 结束时获得 Analyze 规范。

在你的实验课04 部分之前，有一份技能设计文档起草并提交。你在实验课03 用大象开始了它。实验课04 带它通过一轮金鱼并进入代码。
-->

---
layout: default
---

<div class="label">带走这些问题</div>

# 三个问题

1. 你的智能体重试什么，重试多久？
2. 哪些审批可以用代码回答？
3. 谁回答中间带？

<!--
方向：约 2 分钟。不要回答它们。

三个给你的设计文档。

你的智能体重试什么，重试多久？计算层数，包括 SDK 的。

你这周给出的哪些审批可以用没有模型的代码规则回答？

当你的智能体在没有人处于提示前的运行时，谁回答中间带？
-->

---
layout: end
---

<!--
问题。

在周四之前，记下你的智能体要求你批准的内容以及你未经阅读就批准的内容。那个列表是你的第一个规则文件。

周四见。
-->
