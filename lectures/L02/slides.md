---
theme: ../theme
title: "L02：LLM 如何编程（底层原理）"
info: |
  EECS 498 AASE —— 第 02 讲
  Applied Agentic Software Engineering，UMich Fall 2026
layout: title
class: text-left
transition: fade
mdc: true
highlighter: shiki
---

# LLM 如何编程（底层原理）

## 第 02 讲 · 2026 年 9 月 3 日

<!--
打开黑盒。理论课。今天没有现场编程。
回报在下一讲（L03）落地，届时三大要素框架将登场。
-->

---
layout: statement
---

理解了底层原理的开发者拥有*巨大的*优势。

<!--
开场白：为什么需要理论。当它不工作时，思维模型才是决胜因素。
-->

---
layout: default
---

<div class="label">四个核心概念</div>

# 今天的底层原理

<div class="grid grid-cols-2 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-text-aa-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">分词</div>
    <div class="text-sm opacity-70 flex-1">模型如何"看"你的代码。</div>
  </div>
  <div class="card">
    <ph-resize-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">上下文窗口</div>
    <div class="text-sm opacity-70 flex-1">唯一重要的资源。</div>
  </div>
  <div class="card">
    <ph-eye-slash-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">幻觉</div>
    <div class="text-sm opacity-70 flex-1">模型为什么会产生虚构内容。</div>
  </div>
  <div class="card">
    <ph-scales-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">能力 vs 知识</div>
    <div class="text-sm opacity-70 flex-1">模型改进的两个维度。</div>
  </div>
</div>

---
layout: section
---

# 分词

## 模型看到的内容

---
layout: default
---

<div class="label">BPE 直觉</div>

# 是 tokens，不是 words

<div class="mt-6">

- 模型看不到字符
- 模型看不到单词
- 模型看到的是 **tokens** —— 它在训练中见过的片段

</div>

<div class="caption mt-4">字节对编码：合并最常见的相邻配对，重复约 100K 词表大小。</div>

---
layout: default
---

<div class="label">字节对编码，实操演示</div>

# `getOrderById` 去了哪里

```text
1. 从字符开始
   g e t O r d e r B y I d                    12 个 tokens

2. 合并最常见的相邻配对。重复，约 100K 次
   e+r -> er      g+et -> get      O+rder -> Order

3. 在训练中幸存下来的合并结果就是词表
   get  Order  By  Id                          4 个 tokens
```

<div class="caption mt-4">没有人选择了这四个。它们只是足够常见，挣到了一个位置。</div>

---
layout: default
---

<div class="label">o200k_base · GPT-4o 和 5 的编码方式</div>

# 真实的 tokens，真实的 ID

<div style="font-family:'IBM Plex Mono',monospace;font-size:1.05em;color:var(--c-ink-soft);background:var(--c-code-bg);border:1px solid var(--c-rule);border-radius:6px;padding:.5rem .8rem;margin-top:1.4rem;display:inline-block">const order = await getOrderById(orderId);</div>

<div style="display:flex;flex-wrap:wrap;gap:.4rem;margin-top:1.2rem">
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">1671</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(46,91,255,.09);border:1px solid var(--c-rule);color:var(--c-ink)">const</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">2569</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:var(--c-bg-2);border:1px solid var(--c-rule);color:var(--c-ink)">·order</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">314</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(46,91,255,.09);border:1px solid var(--c-rule);color:var(--c-ink)">·=</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">4021</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:var(--c-bg-2);border:1px solid var(--c-rule);color:var(--c-ink)">·await</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">717</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(217,119,6,.16);border:1px solid rgba(217,119,6,.55);color:var(--c-ink)">·get</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">4861</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(217,119,6,.16);border:1px solid rgba(217,119,6,.55);color:var(--c-ink)">Order</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">1582</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(217,119,6,.16);border:1px solid rgba(217,119,6,.55);color:var(--c-ink)">By</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">906</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(217,119,6,.16);border:1px solid rgba(217,119,6,.55);color:var(--c-ink)">Id</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">33050</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(46,91,255,.09);border:1px solid var(--c-rule);color:var(--c-ink)">(order</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">906</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:var(--c-bg-2);border:1px solid var(--c-rule);color:var(--c-ink)">Id</div></div>
<div style="display:flex;flex-direction:column;min-width:0"><div style="font-size:.58em;text-align:center;color:var(--c-ink-muted);font-family:'IBM Plex Mono',monospace;padding-bottom:.3rem">2245</div><div style="font-family:'IBM Plex Mono',monospace;font-size:1.02em;white-space:pre;padding:.4rem .55rem;border-radius:6px;background:rgba(46,91,255,.09);border:1px solid var(--c-rule);color:var(--c-ink)">);</div></div>
</div>

<div class="caption mt-8">42 个字符对应 11 个 tokens。四个琥珀色的恰好是上一张幻灯片预测的结果，而 `Id` 是 token 906，*两次*都是。</div>

<!--
这是上一张幻灯片的回报：getOrderById 确实分解为 get / Order / By / Id，数字在这里。

如果有时间可以指出的两点。芯片中间的圆点是一个前导空格，它是 token 的一部分，所以 " order" 和 "order" 是不同的 tokens。而 "(order" 是一个 token：分词器把括号和单词一起吞掉了，这就是为什么 token 数量很难猜测。

如果有人问：这些 ID 来自 o200k_base。qwen3.5 使用不同的词表，所以数字不同。思路是一样的。
-->

---
layout: default
---

<div class="label">两个后果</div>

# 分词带来的代价

<div class="grid grid-cols-2 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-currency-dollar-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">成本单位</div>
    <div class="text-sm opacity-70 flex-1">冗长的代码更贵——在供应商那里是美元，在自己的机器上是 RAM 和秒数。</div>
  </div>
  <div class="card">
    <ph-eye-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">注意力单位</div>
    <div class="text-sm opacity-70 flex-1">每个 token 都在争夺注意力。你的标识符被拆成常见片段，所以它们没有特殊权重。</div>
  </div>
</div>

---
layout: two-col
---

## 9 个 tokens

```python
def f(x):
    return x * 2
```

::right::

## 约 35 个 tokens

```python
def calculate_double_of_input_value(
    input_value: int
) -> int:
    """Return value times two."""
    return input_value * 2
```

<!--
同一个函数。冗长版本贵 4 倍。
不是"更 informative"——更贵且更分散注意力。
-->

---
layout: section
---

# 上下文窗口

## 唯一重要的资源

---
layout: default
---

<div class="label">窗口内外</div>

# 模型看到的内容

<div class="mt-6 space-y-2">

- **窗口内**：文件、历史记录、代码库地图、系统提示词
- **窗口外**：*什么都没有*
- 没有数据库。没有网页抓取。没有检索

</div>

<div class="caption mt-6">这对工程师来说是关于 LLM 最重要的事实。</div>

---
layout: default
---

<div class="label">为什么更大 ≠ 更好</div>

# 大上下文的三重代价

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-currency-dollar-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">成本</div>
    <div class="text-sm opacity-70 flex-1">按 token 计费，或自己机器上的 RAM 和秒数。注意力是二次方的：2 倍上下文 ≈ 4 倍工作量。</div>
  </div>
  <div class="card">
    <ph-clock-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">延迟</div>
    <div class="text-sm opacity-70 flex-1">笔记本上第一个 token 要等几十秒，而一个简洁的提示词一开始就能输出。</div>
  </div>
  <div class="card">
    <ph-warning-bold class="text-3xl text-rose-600 mb-3" />
    <div class="font-semibold mb-1">质量</div>
    <div class="text-sm opacity-70 flex-1">大海捞针：更大可能*更差*。</div>
  </div>
</div>

<!--
强调令人意外的一项——质量下降。
-->

---
layout: default
---

<div class="label">生成的一个步骤</div>

# 权衡整个窗口

<svg viewBox="0 0 900 250" class="w-full mt-4" role="img" aria-label="一个 token 正在生成，带有指向窗口中已有每个 token 的加权链接">
<path d="M450 82 Q 276 145 102 176" fill="none" stroke="var(--c-primary)" stroke-width="1.9" opacity="0.22"/>
<path d="M450 82 Q 334 145 218 176" fill="none" stroke="var(--c-primary)" stroke-width="5.2" opacity="0.59"/>
<path d="M450 82 Q 392 145 334 176" fill="none" stroke="var(--c-primary)" stroke-width="1.5" opacity="0.17"/>
<path d="M450 82 Q 450 145 450 176" fill="none" stroke="var(--c-primary)" stroke-width="2.1" opacity="0.24"/>
<path d="M450 82 Q 508 145 566 176" fill="none" stroke="var(--c-primary)" stroke-width="1.6" opacity="0.19"/>
<path d="M450 82 Q 566 145 682 176" fill="none" stroke="var(--c-primary)" stroke-width="5.8" opacity="0.64"/>
<path d="M450 82 Q 624 145 798 176" fill="none" stroke="var(--c-primary)" stroke-width="4.5" opacity="0.51"/>
<rect x="50" y="176" width="104" height="46" rx="7" fill="var(--c-bg-1)" stroke="var(--c-rule)"/>
<text x="102" y="205" text-anchor="middle" fill="var(--c-ink)" style="font-family:'IBM Plex Mono',monospace;font-size:19px">import</text>
<rect x="166" y="176" width="104" height="46" rx="7" fill="var(--c-bg-1)" stroke="var(--c-rule)"/>
<text x="218" y="205" text-anchor="middle" fill="var(--c-ink)" style="font-family:'IBM Plex Mono',monospace;font-size:19px">httpx</text>
<rect x="282" y="176" width="104" height="46" rx="7" fill="var(--c-bg-1)" stroke="var(--c-rule)"/>
<text x="334" y="205" text-anchor="middle" fill="var(--c-ink)" style="font-family:'IBM Plex Mono',monospace;font-size:19px">…</text>
<rect x="398" y="176" width="104" height="46" rx="7" fill="var(--c-bg-1)" stroke="var(--c-rule)"/>
<text x="450" y="205" text-anchor="middle" fill="var(--c-ink)" style="font-family:'IBM Plex Mono',monospace;font-size:19px">resp</text>
<rect x="514" y="176" width="104" height="46" rx="7" fill="var(--c-bg-1)" stroke="var(--c-rule)"/>
<text x="566" y="205" text-anchor="middle" fill="var(--c-ink)" style="font-family:'IBM Plex Mono',monospace;font-size:19px">=</text>
<rect x="630" y="176" width="104" height="46" rx="7" fill="var(--c-bg-1)" stroke="var(--c-rule)"/>
<text x="682" y="205" text-anchor="middle" fill="var(--c-ink)" style="font-family:'IBM Plex Mono',monospace;font-size:19px">httpx</text>
<rect x="746" y="176" width="104" height="46" rx="7" fill="var(--c-bg-1)" stroke="var(--c-rule)"/>
<text x="798" y="205" text-anchor="middle" fill="var(--c-ink)" style="font-family:'IBM Plex Mono',monospace;font-size:19px">.</text>
<rect x="392" y="32" width="116" height="50" rx="8" fill="var(--c-amber)"/>
<text x="450" y="65" text-anchor="middle" fill="var(--c-bg-0)" style="font-family:'IBM Plex Mono',monospace;font-size:21px;font-weight:600">get</text>
</svg>

<div class="caption mt-4">线条越粗，权重越高。它依赖 `httpx` 和那个点，但为了找到这一点，它对*每个* token 都进行了评分。</div>

<!--
一步步讲解：模型在选择 "httpx." 后面跟什么，为此它对窗口中已有的每个 token 进行评分，而不仅仅是附近的那些。
粗细代表注意力权重。然后为下一张幻灯片做铺垫：在 "get" 之后的 token 会重复整个画面，之后也是如此。
-->

---
layout: default
---

<div class="label">为什么成本增长比上下文更快</div>

# 每个 token 都关注其他所有 token

|| 窗口中的 tokens 数 | 评分的配对对数 | 相对工作量 |
|---|---:|---:|
|| 1K | 1,000,000 | 1× |
|| 2K | 4,000,000 | 4× |
|| 8K | 64,000,000 | 64× |
|| 32K | 1,024,000,000 | *1024×* |

<div class="caption mt-6">上下文翻倍，工作量翻四倍。窗口是预算，不是容器。</div>

<!--
要落地的数字：32 倍的文本不是 32 倍的代价，而是 1000 倍。
这就是为什么"直接把仓库粘贴进来"在笔记本上行不通。
-->

---
layout: default
---

<div class="label">/add 的教学意义</div>

# Aider 的摩擦就是课程

```text
/add main.py        → 文件进入上下文
/drop main.py       → 文件离开上下文
/tokens             → 精确查看窗口中有什么
/clear              → 重置历史（保留文件）
```

<div class="caption mt-4">这不是限制。这是你本来就该有的纪律。</div>

---
layout: default
---

<div class="label">长会话</div>

# 上下文漂移

<div class="mt-6 space-y-2">

- 历史不断积累：你发送的每一条消息，它返回的每一条消息
- 五十条消息后，窗口中大部分都是*死重*
- 模型无法区分什么是现在真实的，什么是第 3 条消息时真实的

</div>

<div class="caption mt-6">任务之间用 `/clear`。Claude Code（L05）会帮你压缩，而压缩是有损的。</div>

---
layout: statement
---

<div class="label">聊天休息 · 5 分钟</div>

和邻座聊聊：当一个长聊天*退化*时，窗口内部发生了什么？

<!--
0:35。固定的休息时间。提示词在 overview.md 的 chat_break 字段中。
回想一个与助手的长对话，越往后效果越差。你做了什么——新对话？重新解释？现在用我们刚学的术语说出来，实际上发生了什么。
不要 policing 词汇。重点是将他们自己的经验翻译成我们刚刚构建的模型。两三个回答，然后继续。
-->

---
layout: section
---

# 幻觉

## 一个误导性的词

---
layout: default
---

<div class="label">实际发生了什么</div>

# 模型在做它被训练来做的事

<v-clicks>

- 自回归采样
- 从分布中挑选最可能的下一个 token
- 没有事实验证步骤
- 没有数据库查询
- 没有"等等，这个存在吗？"

</v-clicks>

<!--
"幻觉"听起来像个错误。它不是错误——它是设计。
-->

---
layout: default
---

<div class="label">为什么是 "requests" 而不是 "httpx"</div>

# 当上下文沉默时，统计先验获胜

<v-clicks>

- 训练数据：`import requests` 比 `import httpx` 多 100 倍
- 你的项目使用 httpx
- 提示词没有提到它
- 模型输出 `requests` —— 看似合理，但对于*你的*上下文来说是错的

</v-clicks>

<!--
不是"编造"——是错用了真实的库，针对错误的真实项目。
-->

---
layout: default
---

<div class="label">代码中的三种类型</div>

# 常见的幻觉

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-function-bold class="text-3xl text-rose-600 mb-3" />
    <div class="font-semibold mb-1">虚构 API</div>
    <div class="text-sm opacity-70 flex-1">user.get_profile() —— 看似合理，不是真实的。</div>
  </div>
  <div class="card">
    <ph-clock-counter-clockwise-bold class="text-3xl text-rose-600 mb-3" />
    <div class="font-semibold mb-1">过时的知识</div>
    <div class="text-sm opacity-70 flex-1">在 2.0 代码库中使用 pandas 1.x API。</div>
  </div>
  <div class="card">
    <ph-warning-circle-bold class="text-3xl text-rose-600 mb-3" />
    <div class="font-semibold mb-1">错误的签名</div>
    <div class="text-sm opacity-70 flex-1">错误的 kwarg，错误的 dtype —— 很微妙。</div>
  </div>
</div>

---
layout: statement
---

当模型需要一个事实时，*给它看这个事实*。

<!--
管理上下文的最深层原因：准确性。不是成本。不是速度。
-->

---
layout: default
---

<div class="label">局限</div>

# 上下文无法修复什么

<div class="mt-6 space-y-2">

- "我们内部库的最新版本是什么？"
- 答案不在你的代码库里，所以任何 `/add` 都得不到它
- 模型猜测，*看似合理*，而你看不出来

</div>

<div class="caption mt-6">把事实交给它，或者构建一个查找事实的工具。你将在第 5 周构建这些。</div>

---
layout: section
---

# 能力 vs 知识

## 两个维度

---
layout: two-col
---

## 能力

<div class="mt-6 space-y-3">
  <div><strong>推理技能。</strong>多步骤、跨文件、保持约束。</div>
  <div><strong>随以下增长**：模型大小。</div>
  <div><strong>qwen3.5:4b → 9b → 前沿模型</strong>。</div>
</div>

::right::

## 知识

<div class="mt-6 space-y-3">
  <div><strong>模型阅读了什么</strong>。时效性。小众深度。</div>
  <div><strong>随以下增长**：训练数据大小 + 新鲜度。</div>
  <div><strong>小 + 新</strong> 可以击败 大 + 旧。</div>
</div>

<!--
两个维度。混淆它们 = 错误的模型选择。
-->

---
layout: default
---

<div class="label">阶梯 · 你住在最下面的两级</div>

# 为什么不是最上面一级？

<div class="grid grid-cols-3 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-number-circle-four-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">qwen3.5:4b</div>
    <div class="text-sm opacity-70 flex-1">约 2.5 GB。失败得快且失败得明显。你的起点。</div>
  </div>
  <div class="card">
    <ph-number-circle-nine-bold class="text-3xl text-blue-600 mb-3" />
    <div class="font-semibold mb-1">qwen3.5:9b</div>
    <div class="text-sm opacity-70 flex-1">Q4 时约 6 GB。课程默认设置。aider-practice 第 2 部分需要它。</div>
  </div>
  <div class="card">
    <ph-buildings-bold class="text-3xl text-amber-600 mb-3" />
    <div class="font-semibold mb-1">前沿模型</div>
    <div class="text-sm opacity-70 flex-1">别人的数据中心。在两个维度上都更强。本课程你不会运行一个。</div>
  </div>
</div>

<div class="caption mt-6">前沿模型能帮你掩盖问题。4B *不能*。两周后你将在同一个代码库上运行 4b 和 9b，亲自感受差距。</div>

<!--
不要断言这一点——承诺实验。每个人都在接下来的两周内在同一个代码库上运行 4b 和 9b。这比我展示的任何基准测试都更有说服力。
明确说明前沿模型是他们*不在*的那一级：它是用来命名他们放弃什么以及为什么，而不是他们能接触到的东西。
-->

---
layout: default
---

<div class="label">选择</div>

# 将模型与任务匹配

<v-clicks>

- **成本敏感的批量任务**（1000 次重复）→ 能过线的最小模型
- **推理密集型的单任务** → 你拥有的最强大的模型
- **小众库或框架** → 能力救不了你。那是*上下文*问题
- **你在上面学习的任何东西** → 在你犯错的地方失败的那一级

</v-clicks>

<div class="caption mt-6">通常在工作中是对的，通常在教室里是错的。技巧是判断你在哪个房间。</div>

---
layout: section
---

# 诊断

## 当输出错误时

---
layout: default
---

<div class="label">依次询问</div>

# 三个问题

1. **上下文** —— 模型有它需要的东西吗？
2. **幻觉** —— 它虚构了一个 API 吗？
3. **能力或知识** —— 任务太难？太新了？

<div class="caption mt-6">分词不在列表中。它存在于所有三个之下，几乎从来不是你能采取行动的。</div>

---
layout: statement
---

工具包装模型。*核心推理调用*没有查找功能。

<!--
为什么模型不直接查东西？因为核心推理不会。
工具包装它。你将在第 5 周构建这些。
-->

---
layout: default
---

<div class="label">四个底层原理</div>

# 要点总结

<div class="mt-6 space-y-2">

- **Tokens** 是成本和注意力的单位
- **上下文窗口** 是唯一重要的资源。审慎地使用它
- **幻觉** 是没有查找功能的模式补全。用上下文修复它
- **能力和知识** 是不同的维度。大模型对新鲜内容

</div>

<div class="caption mt-6">四个底层原理。这学期你遇到的每一个失败都是它们中的一个穿了戏服。</div>

---
layout: default
---

<div class="label">本周内容</div>

# `aider-practice` 第 1 部分

<div class="mt-6 space-y-2">

- 环境检查和全部十六节课截止 **9 月 11 日周五**
- 用今天的术语重读你自己的 Aider 聊天记录
- 这节课的每一个底层原理*已经在你的记录中*

</div>

<div class="caption mt-6">接下来：L03，三大要素。周二。</div>

---
layout: end
---

---
layout: default
---

<div class="label">深入了解 · 可选</div>

# YouTube 深度讲解

<div class="grid grid-cols-2 gap-6 mt-8 items-stretch">
  <div class="card">
    <ph-text-aa-bold class="text-3xl text-blue-600 mb-3" />
    <div class="text-xs uppercase tracking-widest opacity-60 mb-1" style="font-family:'IBM Plex Mono',monospace">分词</div>
    <div class="font-semibold mb-2">Let's build the GPT Tokenizer</div>
    <a href="https://www.youtube.com/watch?v=zduSFxRajkE" class="text-sm break-all">youtube.com/watch?v=zduSFxRajkE</a>
  </div>
  <div class="card">
    <ph-eye-bold class="text-3xl text-blue-600 mb-3" />
    <div class="text-xs uppercase tracking-widest opacity-60 mb-1" style="font-family:'IBM Plex Mono',monospace">注意力机制</div>
    <div class="font-semibold mb-2">Attention in transformers, step-by-step</div>
    <a href="https://www.youtube.com/watch?v=eMlx5fFNoYc" class="text-sm break-all">youtube.com/watch?v=eMlx5fFNoYc</a>
  </div>
</div>

<div class="caption mt-6">两者都不要求。两者都更深入一层。</div>

<!--
在房间解散时保持显示。不评分，不要求。
分词视频较长；告诉他们前 30 分钟是值得的部分。
-->
