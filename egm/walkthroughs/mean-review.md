# mean-review：给予审查者刻薄的许可

## 它解决什么问题

代码审查经常因为礼貌而变得软弱。审查者说"这看起来不错"因为他们不想打扰作者，或者因为他们不知道问题在哪里。

`mean-review` 通过运行四次脚本强制扫描来解决这个问题：结构、安全、性能和可维护性。脚本强制执行这些扫描，因此审查者不能跳过它们。结果是更严格、更一致的审查。

## 它如何触发

当用户提到以下内容时，此技能激活：
- 代码审查
- 审查 PR
- "审查此代码"
- "检查我的更改"

## SKILL.md 逐节解析

### Frontmatter

```yaml
name: mean-review
description: 当用户想要严格审查代码更改时，运行四次脚本强制扫描。
```

- **name** — 技能的短名称
- **description** — 触发器词和技能功能的摘要

### 正文

正文分为五个步骤：

1. **设置** — 创建文件夹、复制模板、运行 git diff
2. **运行四次扫描** — 按顺序运行结构、安全、性能、可维护性扫描
3. **处理阻塞问题** — 如果有阻塞问题，停止并通知人类
4. **修复和重新运行** — 人类修复问题，重新运行审查
5. **完成** — 当所有扫描通过时标记审查完成

每个步骤都指定了智能体应该做什么、输入是什么、输出是什么。

## SKILL.md 之外的内容

### Assets

- `assets/mean-review-ledger-template.md` — MEAN-REVIEW.md 的模板

### Scripts

- `scripts/enforced_scans.py` — 执行四次扫描的脚本

### Evals

- `evals/trigger-evals.json` — 触发测试

## 工作示例

以下是一个完整示例，展示 mean-review 在实际代码上的工作流程。

假设你刚刚使用 `egm-implement` 实现了一个 webhook 重试功能。你启动 `mean-review`：

**步骤 1.** 技能运行 `enforced_scans.py`。脚本报告：

```
$ git diff --name-only main...HEAD | python3 ~/.claude/skills/mean-review/scripts/enforced_scans.py --stdin
shipyard/notify/worker.py:88  silent-except  except Exception swallows the error (no raise, no log)
shipyard/notify/worker.py:41-62  10-line  17 code lines with no comment
shipyard/notify/worker.py:30  fn-length  drain() body is 64 lines (limit 50)
shipyard/notify/outbox.py:22  weak-name  data (parameter)
skipped deploy/worker.yaml: no rules for .yaml files
skipped docs/designs/webhook-retries.md: no rules for .md files
skipped docs/egm/webhook-retries/IMPLEMENT.md: no rules for .md files
4 finding(s) in 7 scanned file(s); 3 skipped
```

**步骤 2.** 步骤 3 说要在每个跳过的文件上手动运行扫描，这些文件仍然存在，因此模型检查所有三个。YAML 是部署配置，是声明式的，两个 Markdown 文件是 prose。没有什么可标记的。

**步骤 3.** 阅读完整的 `worker.py`，模型发现扫描无法看到的真正问题：worker 在 HTTP 调用返回*之前*将事件标记为 `sent`，因此两者之间的崩溃会丢失事件，这正是该功能存在以防止的失败。

**步骤 4.** 模型将此列表附加到 `docs/egm/webhook-retries/MEAN-REVIEW.md` 作为 Pass 1。

**步骤 5.**

```
1. [correctness] shipyard/notify/worker.py:71 Event is marked sent before the POST returns; a crash in between drops it, which is the bug this feature exists to fix. Fix: mark sent only after a 2xx, inside the same transaction as the attempt record.
2. [correctness] shipyard/notify/outbox.py:40 `get_due()` returns events ordered by id, but the doc promises per-order ordering by creation time. Fix: order by (order_id, created_at), and test it.
3. [correctness][enforced] shipyard/notify/worker.py:88 `except Exception:` swallows everything, including the database error that would tell you the outbox is broken. Fix: catch the HTTP client's error only; log and re-raise the rest.
4. [readability][enforced] shipyard/notify/worker.py:41-62 The backoff calculation has no comment saying why the base is 30 seconds or why the cap is 1 hour. Fix: two lines stating the policy and pointing at the design doc section.
5. [readability][enforced] shipyard/notify/worker.py:30 `drain()` is 64 lines and claims, sends, and reschedules. Fix: split send and reschedule into their own functions.
6. [nit][enforced] shipyard/notify/outbox.py:22 `data` says nothing. Fix: `payload`.
```

发现 1 不在 `IMPLEMENT.md` 的漂移日志中，发现 2 与设计文档相矛盾。这正是步骤 1 中的交叉检查的目的。

人类修复所有六个问题，模型将其记录在 `### Pass 1 disposition` 下。重新运行，保存为 Pass 2，返回两个琐碎修改。完成。

## 技能与文章的对比

文章提供了提示词、10 行规则、"要求严格的 readability"和循环。技能添加了：
- 作用域规则
- 全文件读取规则
- 脚本背后的三次强制扫描
- 声明式豁免
- 优先待办清单格式
- 语气规则
- 与设计文档和 `IMPLEMENT.md` 的交叉检查
- 保留每次扫描的账本

## 审查问题和练习

1. 为什么技能坚持逐字使用框架提示词？写一个合理的改述，并解释它如何让审查变得宽松。
2. 在你为本课程编写的代码上运行 `enforced_scans.py`。选择一个 `10-line` 发现并编写它想要的注释。这个注释有用，还是规则 misfire 了？如果 misfire 了，这个 span 是声明式的吗？
3. 脚本将非 Python 发现标记为 `[heuristic]`。找到一个 JavaScript 或 shell 代码片段，其中行规则给出了错误的答案（误报或遗漏），并解释解析器会如何处理不同。
4. 待办清单将正确性放在可读性之前，但 10 行规则被称为至关重要。论证一个无注释的 20 行代码块既是正确性风险也是样式问题。
5. 用户以理由拒绝一个发现，步骤 1 说在没有新证据的情况下不要再次提出它。描述一个代码变更会算作新证据，另一个不会。
6. 在同一个差异上要求模型进行两次代码审查：一次普通，一次使用 Rensin 的框架提示词。计算每次的发现数量并分类。除了语气，还有什么变化？
