# 规范：transcript-analytics 的图表和结构化输出

## 高层目标

向 transcript analytics CLI 添加词频可视化和结构化文件输出。

## 中层目标

- CREATE 输出格式化器，将完成的分析渲染为纯文本、
  JSON、Markdown 和 YAML。
- CREATE 图表函数，将 top-word 计数绘制为条形、饼图或线
  图并将图像保存到 PNG 文件。
- ADD `--chart` 和 `--output-format` 到命令行，并使 `main.py`
  调用新函数。

每一个都可以独立检查：用新标志运行程序并查看输出。

## 实现说明

- 用散文计划。命名每个文件，说什么在里面以及为什么，然后停止。
  不要在计划中编写代码；写两次是获得一次的最慢方法。
- 下面的 Context 部分是此更改的完整文件列表。不要
  要求任何其他文件，不要命名一个不在那两个列表上的文件。
- `matplotlib` 和 `pyyaml` 已经是 `pyproject.toml` 中的依赖。使用
  它们。不要添加任何其他依赖，不要编辑 `pyproject.toml`。
- 图表写入文件，从不显示。调用 `savefig()`。不要调用
  `plt.show()`，不要在代码中选择后端。
- 标志住在 `src/arg_parse.py` 中。那个文件拥有这个程序中的每个命令行
  参数，所以在其他地方添加的标志不存在。
- 遵循项目中已有的模式：每个签名上的类型提示，每个函数上的文档字符串，
  标准库导入首先。
- `TranscriptAnalysis.summary` 是 `None` 除非传递了 `--summarize`，所以
  每个格式化器必须 survive an absent summary。
- 保持现有纯文本报告完全像今天读取的那样。`--output-format
  text` 是默认的，它的输出绝不能改变。

## 上下文

### 开始上下文

- `src/main.py`
- `src/arg_parse.py`
- `src/data_types.py`（只读）
- `src/constants.py`（只读）
- `src/llm.py`（只读）
- `pyproject.toml`（只读）
- `transcript.txt`（只读）

### 结束上下文

- `src/main.py`（更新）
- `src/arg_parse.py`（更新）
- `src/output_format.py`（新文件）
- `src/chart.py`（新文件）

## 有序低层任务

1. CREATE `src/output_format.py`：定义 `format_as_text`、`format_as_json`、
   `format_as_md` 和 `format_as_yaml`。**所有四个接受同样的两个
   参数并返回一个字符串：** `(analysis: TranscriptAnalysis,
   show_speakers: bool) -> str`。三个不需要 `show_speakers` 的仍然
   接受它，所以任务 4 可以以同样的方式调用它们中的任何一个。将
   `render_text()` 的主体从 `src/main.py` 移动到 `format_as_text()`。
   `format_as_json` 使用 `dataclasses.asdict`；`format_as_yaml` 使用
   `yaml.safe_dump`；`format_as_md` 写一个 top words 的 Markdown 表。
2. CREATE `src/chart.py`：定义 `create_bar_chart(analysis:
   TranscriptAnalysis, path: str) -> None`、`create_pie_chart(...)` 和
   `create_line_chart(...)` 用同样的两个参数。每一个绘制
   `analysis.top_words`，标记坐标轴，用 `analysis.source` 标题图形，并
   用 `savefig()` 保存到 `path`。条形图是水平的并按降序排序。
3. UPDATE `src/arg_parse.py`：ADD `--output-format` 带有选择 `text`、
   `json`、`md`、`yaml`，默认为 `text`。ADD `--chart` 带有选择
   `bar`、`pie`、`line` 和没有默认值。ADD `--chart-file`，默认为
   `chart.png`。
4. UPDATE `src/main.py`：REMOVE `render_text()`。ADD 来自
   `output_format` 和 `chart` 的导入。在 `main()` 中，构建一个 dict，
   将每个 `--output-format` 选择映射到其格式化器函数，查找
   `args.output_format`，并调用结果为
   `formatter(analysis, args.speakers)`。dict 中的每个值是一个 plain
   函数引用，接受那两个参数；不要将它们中的任何一个包装在 lambda 中。
   打印它返回的内容。当 `args.chart` 设置时，以同样的方式查找匹配的
   图表函数并用 `analysis` 和 `args.chart_file` 调用它。
