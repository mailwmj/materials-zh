# transcript-analytics，带规范

L04 实时演示运行的代码库。它是 L03 使用的同样程序，同样的起点，有两
个添加：一个 `pyproject.toml`，所以项目可以声明依赖，以及 `spec.md`，
演示在一个粘贴中交给 Aider 的五部分规范。

L03 一次输入一个句子，观看一个两文件更改失败。
L04 先写下整个东西，一次运行完成。同样的模型，同样的程序，不同的
计划量。

## 获取工作副本

```sh
./reset
```

这在你的克隆根目录旁边构建一个一次性副本在 `demo-scratch/L04-transcript-analytics`，
设置 aider 的配置，用 `uv sync` 安装依赖，使其成为一个 git 仓库，所以
`/undo` 有效，并检查程序运行和 matplotlib 导入。它在完成时打印完整
路径，所以你永远不必计算它。随时再次运行它以重新开始。不要在这个目录
中运行 aider：这里的配置文件是故意不加点的（`aider.conf.yml`，不是
`.aider.conf.yml`），以便 aider 忽略它们，这个副本保持原始。

机器上的第一个 `./reset` 下载 matplotlib 和 pyyaml，需要大约十五秒。
之后的每一个从 uv 的缓存提供，需要大约一秒。

然后：

```sh
cd ../../demo-scratch/L04-transcript-analytics
uv run main transcript.txt
uv run main transcript.txt --top 5 --speakers
```

`--summarize` 是唯一需要模型的东西。`./reset` 写入指向它的 `.env`。
如果没人在监听，它打印 `summary unavailable`，其余报告仍然工作，
故意的。

## 五个文件，以及两个缺失的

| 文件 | 拥有 |
|------|------|
| `src/main.py` | 计数，和打印报告 |
| `src/arg_parse.py` | 每个命令行标志 |
| `src/constants.py` | 停用词列表和调优值 |
| `src/data_types.py` | 分析传递的形状 |
| `src/llm.py` | 对模型的唯一调用 |

`src/chart.py` 和 `src/output_format.py` 不存在。编写它们是演示。
规范在其 Ending context 部分中说明了这一点，这是学生跳过以及决定
模型允许触及什么的那个部分。

## 规范

在运行任何东西之前阅读 [`spec.md`](spec.md)。它很短，五个部分中的
每一个都有其位置：

1. **高层目标。** 一句话。为什么这一切。
2. **中层目标。** 三个结果，你可以通过运行程序检查。
3. **实现说明。** 模型否则必须猜测的工程判断。"使用 matplotlib 和
   pyyaml，它们已经是依赖。" "从不调用 `plt.show()`。" "标志住在
   `arg_parse.py` 中。"
4. **上下文。** 开始和结束文件列表，带有 `(new file)` 和 `(readonly)`
   标记。
5. **有序低层任务。** 依赖顺序中的四个 IDK 丰富的步骤。

注意 Implementation Notes 买你什么。那一节中的每一行都是某些东西，
否则你会在事后写在一个审查评论中。把它放在规范中，你跳过审查回合。

还注意到任务 3 命名 `src/arg_parse.py`。这是 L03 的教训写下来：
标志住在那个文件中，所以一个只说"向 main.py 添加一个 `--chart` 标志"
的规范产生一个接受任何东西的程序。规范是你停止模型必须知道那一点的
地方。

## 自己运行演示

```sh
./reset && cd ../../demo-scratch/L04-transcript-analytics
aider src/main.py src/arg_parse.py
```

命令行上的两个文件，不是四个。`.aider.conf.yml` 设置
`edit-format: architect`，所以 aider 以 Architect Mode 出现，没有
标志，它携带一个 `read:` 列表，保存规范的 Beginning 上下文的其余
部分：`data_types.py`、`constants.py`、`llm.py`、`pyproject.toml` 和
`transcript.txt`。那些是规范标记 `(readonly)` 的，而 `read:` 是 aider
对同样想法的说法。命令行保存规范说可以更改的两个文件。

然后，在 aider 中：

1. 粘贴整个 `spec.md`。所有五个部分，一条消息。
2. **计划停止并等待。** `auto-accept-architect: false` 是故意设置的。
   在接受它之前阅读计划。还没有任何东西被写入磁盘，所以这是整个工作
   流程中注意到模型误解你的最便宜的地方。
3. 接受。编辑器将计划变成文件：`src/output_format.py` 和
   `src/chart.py` 创建，`src/arg_parse.py` 和 `src/main.py` 更新。
4. 运行它：

   ```sh
   uv run main transcript.txt --chart bar --output-format json
   ```

   你应该在 stdout 上得到 JSON 和磁盘上的 `chart.png`。

`./reset` 将其放回，以便你可以再次运行它。

## 实际要观察的内容

**如果 aider 询问"Add file to the chat?"，说不。** 输入 `S` 跳过
所有它们。默认是 Yes，在 Architect Mode 中 Yes 是昂贵的答案：添加
文件重新运行 architect，第二个回复通常是"你可以如描述继续"而不是
一个计划，编辑器然后从那个构建而不是从你阅读的计划构建。你得到比你
要求的更少的文件，任何地方都没有错误。这个演示的配置 upfront 加载
每个只读文件，所以问题不应该出现，但知道为什么答案重要是值得的，因为
它会在其他地方出现。

**阅读计划。** 这是这个工作流程中唯一便宜修复的工件。一个误读规范
的计划用一个按键拒绝你；同样的误解在编辑后捕获花费你一个 `/undo`，
一个重写，以及另外几分钟的模型时间。

**检查标志实际被定义了。** 值得知道的失败是安静的一个：一个退出 0
和什么都不做的运行。两个命令告诉你区别：

```sh
uv run main transcript.txt --output-format json | head -3
uv run main transcript.txt | head -3
```

如果那两个打印同样的东西，标志被接受并忽略。那比崩溃更糟，因为没有
退出代码会告诉你。

**与 L03 比较。** 同样的代码库，同样的模型，L04 要求的更改比 L03
不能 landing 的更大。区别是这次文件列表、顺序和约束在模型看到它们
之前写下来。

## 在 4B 上尝试

```sh
./reset --model 4b
```

同样的规范，两个座上的更小模型。这是规范是否在做工作的诚实测试：
4B 有更少的容量来推断你遗漏了什么，所以规范中任何模糊的东西都显示
为输出中错误的东西。如果 4B 落地规范和 9B 落地规范，规范正在 carrying
its weight。

## 配置

`aider.conf.yml`、`aider.model.settings.yml` 和 `aider.model.metadata.json`
是你在 `aider-practice` 中的同样的三个文件，针对这个项目。`./reset`
将它们复制到工作副本中，前面有点。与 L03 副本的区别值得一看：
`edit-format: architect` 和 `editor-model` 设置两个座位，
`auto-accept-architect: false` 是使计划停止的原因，`num_ctx` 更大，
因为一个规范加上四个文件加上一个计划比 L03 曾经一次持有的更多。

元数据文件故意将两个本地模型的价格定在 frontier 价格，所以 `/tokens`
向你显示如果付费，会话会花费什么。

`matplotlibrc` 将 matplotlib 的后端固定为 `Agg`。那不是风格选择。
在 macOS 上，默认后端打开一个窗口，模型自己添加的 `plt.show()` 会
阻塞程序，直到有人关闭它。用 `Agg`，`savefig()` 仍然写入 PNG，没有
任何东西阻塞。
