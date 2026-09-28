# 太原理工大学本科毕业设计（论文）LaTeX 模板

是本人在写毕业论文时用到并优化的一个符合学校要求的一个可直接编译的中文本科毕业论文 LaTeX 模板，包含封面、中英文摘要、自动目录、五章正文、参考文献与致谢的完整结构。模板正文为占位说明文字，便于对照格式逐段替换。

## 编译

依赖 TeX Live（或 MiKTeX）完整发行版，无需额外安装宏包。

```bash
latexmk -pdf main.tex
```

或手动四步编译（首次编译后需生成参考文献）：

```bash
pdflatex main.tex
bibtex   main
pdflatex main.tex
pdflatex main.tex
```

编译产物为 `main.pdf`。使用 `latexmk -c` 可清理中间文件。

> 注意：本模板使用 **pdfLaTeX** 编译。若在 Overleaf 上使用，请在 Menu → Compiler 中选择 `pdfLaTeX`。

## 目录结构

```
main.tex              主控文件：页面设置、字体、标题样式、页码规则、目录样式
references.bib        参考文献数据库（BibTeX 格式，内置条目仅为格式示例）
cover.pdf             封面（学校统一格式，2 页，无页码）
images/
  overall_framework.png   示例框架图（替换）
chapters/
  introduction.tex      第 1 章  绪论
  related_work.tex      第 2 章  相关理论与技术基础
  method.tex            第 3 章  方法设计
  experiment.tex        第 4 章  实验与结果分析
  conclusion.tex        第 5 章  总结与展望
```

模板正文约 5 页。各章均只保留二级标题，需要三级细分时使用 `\subsubsection{}`。

## 排版约定

模板已按学校格式要求预设好以下规则，一般不需要改动：

| 项目 | 设置 |
| --- | --- |
| 纸张 / 页边距 | A4，上 3.3cm、下 2.3cm、左 2.8cm、右 2.3cm |
| 正文字体字号 | 宋体 12pt，行距 1.5 倍，首行缩进 2 字符 |
| 章节标题 | 一级标题居中 15pt 加粗，二级 14pt，三级 12pt |
| 页码 | 封面无页码，摘要与目录用罗马数字，正文从阿拉伯数字 1 开始 |
| 目录 | 标题居中，各行带虚线引导符，最多显示到三级标题 |
| 参考文献 | `unsrt` 样式，10.5pt，按正文引用顺序排列 |
| 图片表格标题 | 标签与标题之间不加冒号（`labelsep=space`） |

正文中引用参考文献统一使用 `\upcite{key}` 命令，效果为上标数字，定义在 `main.tex` 中：

```latex
依据实验结果\upcite{vaswani2017attention}，本文方法……
```

## 换为自己的论文

1. **封面**：`cover.pdf` 为学校统一格式的空白表单，请用学校下发的文件填写本人信息后替换，或直接在其中填写。
2. **任务书**（可选）：模板默认不插入任务书。需要时把 `task.pdf` 放到根目录，取消 `main.tex` 中对应 `\includepdf` 的注释。
3. **标题与作者**：修改 `main.tex` 中的 `\title{}` 与 `\author{}`，以及中文摘要页与英文摘要页顶部的题目文字（这两处题目是手写排版的，需分别修改）。
4. **中文摘要**：修改 `main.tex` 中「摘 要」部分及其后的关键词。
5. **英文摘要**：修改 `main.tex` 中 `Abstract` 部分及其后的 `Key words`。
6. **各章正文**：直接在 `chapters/*.tex` 中把【】占位说明替换为自己的内容。新增章节时在 `chapters/` 下建文件，并在 `main.tex` 中用 `\input{chapters/文件名}` 引入。
7. **插图**：把图片放入 `images/`，替换 `images/overall_framework.png`，或新增：

   ```latex
   \begin{figure}[htbp]
       \centering
       \includegraphics[width=0.8\textwidth]{images/your-figure.png}
       \caption{图片标题}
       \label{fig:your-label}
   \end{figure}
   ```

   正文中用 `如图 \ref{fig:your-label} 所示` 引用。

8. **表格**：模板使用 `booktabs` 三线表，正文中不要使用竖线：

   ```latex
   \begin{table}[htbp]
     \centering
     \caption{表格标题}
     \label{tab:your-label}
     \begin{tabular}{lccc}
       \toprule
       \textbf{方法} & \textbf{指标一} & \textbf{指标二} & \textbf{指标三} \\
       \midrule
       方法 A & 0.0000 & 0.0000 & 0.0000 \\
       方法 B & 0.0000 & 0.0000 & 0.0000 \\
       \bottomrule
     \end{tabular}
   \end{table}
   ```

9. **参考文献**：编辑 `references.bib`，把示例条目替换为本人实际引用的文献。文献格式需符合学校要求（模板当前为 `unsrt`，按引用顺序编号）。
10. **致谢**：修改 `main.tex` 末尾的「致 谢」部分。
11. **附件**（可选）：模板末尾的附件部分默认注释掉了。把附件 PDF 放入根目录，取消注释并改成实际文件名。

## 常见问题

**中文不显示或显示为方框**：确认使用 TeX Live / MiKTeX 完整版，不要用精简版发行版。

**参考文献编号是问号 `[?]`**：需要先运行一次 `pdflatex`，再运行 `bibtex main`，然后再运行两次 `pdflatex`。

**目录页码不对**：正常现象，多编译几次即可收敛；或统一用 `latexmk -pdf main.tex` 自动处理。

**页码从摘要开始乱掉**：页码规则集中在 `main.tex` 中，依次使用 `\pagenumbering{gobble}`（无页码）、`{roman}`（罗马数字）、`{arabic}`（阿拉伯数字）切换，新增页面时注意保持顺序。

**图片路径报错找不到文件**：`\includegraphics` 的路径相对 `main.tex` 所在目录书写，如 `images/xxx.png`，Windows 下也请使用正斜杠 `/`。

## 说明

- 封面为太原理工大学 2026 届本科生毕业设计（论文）统一格式文件，版权归学校所有。
- 模板中的示例图片与示例文献条目仅用于演示排版格式，与任何真实研究无关。
- 内容部分由`WorkBuddy`生成。
