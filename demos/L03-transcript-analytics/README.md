# transcript-analytics

L03 实时演示运行的小代码库，以及 L04 和 L05 回归的代码库。五个 Python
文件，没有依赖，没有 API 密钥。

## 获取工作副本

```sh
./reset
```

这在你的克隆根目录旁边构建一个一次性副本在 `demo-scratch/L03-transcript-analytics`，
设置 aider 的配置，使其成为一个 git 仓库，所以 `/undo` 有效，
并检查程序仍然运行。它在完成时打印完整路径，所以你永远不必计算它。
随时再次运行它以重新开始。不要在这个目录中运行 aider：这里的配置文件
是故意不加点的（`aider.conf.yml`，不是 `.aider.conf.yml`），以便 aider
忽略它们，这个副本保持原始。

然后：

```sh
cd ../../demo-scratch/L03-transcript-analytics
python3 src/main.py transcript.txt
python3 src/main.py transcript.txt --top 5 --speakers
```

`--summarize` 是唯一需要模型的东西。`./reset` 写入指向它的 `.env`：

```sh
python3 src/main.py transcript.txt --summarize
```

如果没有人在监听，它打印 `summary unavailable`，其余报告仍然工作。
这是故意的。

## 五个文件

| 文件 | 拥有 |
|------|------|
| `src/main.py` | 计数，和打印报告 |
| `src/arg_parse.py` | 每个命令行标志 |
| `src/constants.py` | 停用词列表和调优值 |
| `src/data_types.py` | 分析传递的形状 |
| `src/llm.py` | 对模型的唯一调用 |

分割是重点。对命令行的更改触及 `arg_parse.py`。对报告的更改触及
`main.py`。大多数更改触及两者，一个只有其中一个在上下文中的 aider
会话产生一半更改，看起来已完成。

## 自己尝试演示

讲座演示是对 `qwen3.5:9b` 的五个步骤：

```sh
./reset && cd ../../demo-scratch/L03-transcript-analytics
aider src/main.py
```

然后，在 aider 中：

1. `Add a new CLI argument --output-format with choices text, json, markdown.`
   观看它写一个它从未见过的整个 `arg_parse.py`。将发明的那个与真实
   的比较：它在真实文件读取 `DEFAULT_TOP_N` 的地方硬编码 `default=10`。
   Aider 提供添加文件。**说不**（`S` 拒绝一整系列那些提供），以便
   编辑被丢弃，只有 `main.py` 编辑落地。
2. 运行程序：`python3 src/main.py transcript.txt`。它坏了，diff 看起来
   很好。标志从未定义，因为定义标志的文件不在上下文中。
   （如果模型要求文件并写任何东西，说
   `Do it in src/main.py only. Do not touch any other file.` 你落在
   同样的地方。）
3. `/undo`，然后 `/add src/arg_parse.py`，然后 **与步骤 1 同样的提示**，
   逐字。现在它保持在一起，程序运行。
4. 在每一步之后 `/tokens`，然后 `/drop` 和 `/add src/` 和 `/tokens`
   再次。观看数字，以及当你添加文件时仓库地图行缩小。
5. `/model ollama_chat/qwen3.5:4b` 并再次运行步骤 1。比较。提供者
   前缀不是可选的：bare `/model qwen3.5:4b` 不匹配
   `.aider.model.settings.yml`，aider 回退到默认值。

从演示目录的 `./reset` 将其放回，以便你可以再次运行它。

## 配置

`aider.conf.yml`、`aider.model.settings.yml` 和
`aider.model.metadata.json` 是你在 `aider-practice` 中的同样的三个
文件，针对这个项目。`./reset` 将它们复制到工作副本中，前面有点。
元数据文件故意将两个本地模型的价格定在 frontier 价格，所以 `/tokens`
向你显示如果付费，会话会花费什么。
