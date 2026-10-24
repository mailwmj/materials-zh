# EECS 498 AASE 课程材料

EECS 498-016《应用智能体软件工程》课程的幻灯片和讲义，密歇根大学，2026年秋季。

## 目录结构

- `lectures/` 目录下每个讲座一个文件夹（`L01`、`L02`……），每个文件夹包含 `slides.md`、`slides.pdf` 和 `slides.pptx`。
- `labs/` 目录下每个实验课一个文件夹（`Lab00`、`Lab01`……），结构同上。
- `demos/` 存放现场演示所使用的代码库，每个演示一个文件夹，以其所属讲座命名（如 `L01-notes-cli`、`L03-transcript-analytics`）。每个文件夹都有一个 README 和一个 `reset` 脚本：运行 `./reset` 即可生成一个可随时丢弃的工作副本，已配置完毕、可立即用于 Aider。自己运行这些演示——这就是它们存在的原因。
- `egm/` 以五个可安装的技能（`peanuts`、`elephant`、`goldfish`、`egm-implement`、`mean-review`）的形式提供大象-金鱼模型（Elephant-Goldfish Model），包含每个技能的演练/实操指南，以及一页介绍技能构成的内容。请从 `egm/README.md` 开始阅读。

`slides.md` 是幻灯片所基于的 Slidev 源文件。将其放在这里是为了方便你阅读、搜索或复用内容。它依赖一个存在于本仓库之外的 theme，因此无法直接构建。

如果你只是为了获取某个幻灯片或演示，那么 `git clone --depth 1` 是你需要的命令。本仓库的大部分历史记录都是编译后的幻灯片。

幻灯片通常在各场讲座或实验课进行期间发布。如果某周缺失，说明该内容尚未发布。

## 链接

- 课程网站：https://eecs498-aase.github.io
- 答疑：Ed，或发送邮件至 eecs-aase-staff@umich.edu
