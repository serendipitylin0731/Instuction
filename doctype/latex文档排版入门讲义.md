---
tags: Instruction
---

# LaTeX 文档排版入门讲义

LaTeX 是一种用于编写**高质量排版文档**的工具，尤其适合数学公式、课程报告、实验报告和论文。

它和 Word 的使用方式不同。使用 Word 时，我们通常直接调整文字的外观；使用 LaTeX 时，我们先编写带有命令的 `.tex` 源文件，再通过编译得到 PDF 文档。

例如：

```latex
\section{勾股定理}

对于直角三角形，有
\[
    a^2+b^2=c^2.
\]
```

其中 `\section{}` 表示一节标题，`\[...\]` 表示单独占一行的数学公式。编译之后，它们会被自动排版成对应的标题和公式。

可以简单理解成：

```text
.tex 源文件 → LaTeX 编译器 → PDF 文档
```

---

## 使用准备

编写 LaTeX 需要一个编辑器和一个编译环境。

如果只是第一次接触，可以使用支持 LaTeX 的在线编辑器，创建空白项目后直接编写和编译；如果需要在本地长期使用，也可以安装 TeX Live 或 MiKTeX，并在 vscode 中安装 LaTeX 相关扩展。

本篇讲义中的中文示例使用 `ctexart` 文档类，建议选择 XeLaTeX 进行编译。

> tip：LaTeX、编辑器和编译器不是同一个东西。`.tex` 文件用于保存源代码，编辑器用于编写文件，XeLaTeX 等编译器负责把源文件转换成 PDF。

---

## 第一个 LaTeX 文档

新建一个名为 `main.tex` 的文件，写入下面的内容：

```latex
\documentclass[UTF8]{ctexart}

\title{我的第一篇 LaTeX 文档}
\author{作者姓名}
\date{\today}

\begin{document}

\maketitle

\section{开始}

Hello, LaTeX!

这是我的第一篇中文 LaTeX 文档。

\end{document}
```

使用 XeLaTeX 编译后，就可以得到 `main.pdf`。

这份文件可以分成两个部分：

```text
\documentclass ... \begin{document} 之前：导言区
\begin{document} ... \end{document}：正文区
```

### 1. 导言区

导言区用于设置文档类型、加载功能包和填写文档信息。

```latex
\documentclass[UTF8]{ctexart}

\title{文档标题}
\author{作者姓名}
\date{\today}
```

其中：

- `\documentclass[UTF8]{ctexart}` 表示创建一篇支持中文的普通文章
- `\title{}` 设置标题
- `\author{}` 设置作者
- `\date{}` 设置日期
- `\today` 表示自动使用编译当天的日期

如果不想显示日期，可以写成：

```latex
\date{}
```

### 2. 正文区

真正显示在 PDF 中的内容应当放在：

```latex
\begin{document}

正文内容

\end{document}
```

`\maketitle` 用于把导言区设置的标题、作者和日期显示出来。如果不写这条命令，它们不会自动出现在正文中。

---

## LaTeX 命令的基本结构

LaTeX 命令通常以反斜杠 `\` 开头。

例如：

```latex
\section{一级章节}
\textbf{粗体文字}
\frac{1}{2}
```

其中花括号 `{}` 中通常放置命令处理的内容，方括号 `[]` 中通常放置可选设置。

例如：

```latex
\documentclass[UTF8]{ctexart}
```

可以拆成：

```text
\documentclass：命令
[UTF8]：可选参数
{ctexart}：必须提供的参数
```

LaTeX 命令区分大小写。例如，`\LaTeX` 和 `\latex` 并不是同一个命令。

---

## 注释

百分号 `%` 后面的内容会被 LaTeX 忽略，可以用于编写注释。

```latex
这是正文。 % 这里是注释，不会显示在 PDF 中
```

如果一行开头就是 `%`，那么整行都不会参与排版：

```latex
% \section{暂时不用的章节}
```

注释适合用于解释代码，或者暂时关闭某一行内容。

---

## 文档结构

LaTeX 可以自动管理章节标题的字号、编号和层级。

```latex
\section{一级章节}
\subsection{二级章节}
\subsubsection{三级章节}
```

例如：

```latex
\section{实验目的}

这里填写实验目的。

\section{实验原理}

\subsection{理论基础}

这里填写理论基础。

\subsection{实验方法}

这里填写实验方法。
```

如果不希望某个标题带有自动编号，可以在命令后添加 `*`：

```latex
\section*{致谢}
```

但是无编号标题通常不会自动出现在目录中。

### 自动生成目录

在希望显示目录的位置添加：

```latex
\tableofcontents
```

目录需要根据文档中的章节信息生成，因此有时第一次编译后目录还是空的，再编译一次就会正常显示。

---

## 段落与换行

### 1. 新建段落

在源文件中留出一个空行，就可以开始新的段落。

```latex
这是第一段文字。

这是第二段文字。
```

只在源文件中按一次回车，通常不会开始新段落：

```latex
第一行文字，
第二行文字。
```

上面的两行在 PDF 中一般仍会排在同一个段落中。

### 2. 强制换行

如果确实需要在同一段中换行，可以使用：

```latex
第一行文字。\\
第二行文字。
```

普通正文中不要用很多 `\\` 代替分段，建立新段落时直接留出空行更合适。

### 3. 空格

LaTeX 会把源文件中连续的多个普通空格当成一个空格，也不会因为源代码缩进而自动增加正文缩进。

```latex
这些      空格在排版时通常会被合并。
```

因此不需要通过反复按空格键来调整位置。文档的缩进和间距应当交给 LaTeX 统一处理。

---

## 常用文字格式

| 想要的效果 | LaTeX 写法 |
|---|---|
| 粗体 | `\textbf{文字}` |
| 斜体 | `\textit{文字}` |
| 强调 | `\emph{文字}` |
| 等宽字体 | `\texttt{文字}` |
| 下划线 | `\underline{文字}` |

例如：

```latex
\textbf{这是粗体文字}

\textit{这是斜体文字}

\emph{这是需要强调的文字}

\texttt{main.tex}

\underline{这是带下划线的文字}
```

一般情况下，正文的字体、字号和行距应当统一设置，不建议在每句话中手动调整。

---

## 特殊字符与转义

下面这些字符在 LaTeX 中具有特殊含义：

```text
#  $  %  &  _  {  }  ~  ^  \
```

如果想把它们作为普通文字显示，通常需要在前面添加反斜杠：

```latex
\#  \$  \%  \&  \_  \{  \}
```

例如：

```latex
正确率为 95\%。

文件名为 student\_list.txt。
```

反斜杠 `\` 本身可以使用下面的命令显示：

```latex
\textbackslash
```

波浪号和脱字符可以分别写成：

```latex
\textasciitilde
\textasciicircum
```

> tip：忘记转义 `%`、`&` 和 `_` 是初学时最常见的报错原因之一。

---

## 列表

### 1. 无序列表

无序列表使用 `itemize` 环境，每一项以 `\item` 开头。

```latex
\begin{itemize}
    \item 苹果
    \item 香蕉
    \item 西瓜
\end{itemize}
```

### 2. 有序列表

有序列表使用 `enumerate` 环境。

```latex
\begin{enumerate}
    \item 打开源文件
    \item 编写 LaTeX
    \item 编译并检查 PDF
\end{enumerate}
```

LaTeX 会自动生成并维护序号，在中间增加或删除一项时，不需要手动修改后面的编号。

### 3. 多级列表

列表环境可以继续嵌套：

```latex
\begin{itemize}
    \item 水果
    \begin{itemize}
        \item 苹果
        \item 香蕉
    \end{itemize}
    \item 饮料
\end{itemize}
```

每个 `\begin{...}` 都必须有对应的 `\end{...}`。

---

## 数学公式

数学公式是 LaTeX 最常用的功能之一。

### 1. 行内公式

放在正文中的短公式，可以写在一对 `$` 中：

```latex
对于直角三角形，有 $a^2+b^2=c^2$。
```

公式会和文字显示在同一行中。

### 2. 行间公式

需要单独占一行的公式，可以写在 `\[...\]` 中：

```latex
\[
    a^2+b^2=c^2
\]
```

如果公式需要自动编号，可以使用 `equation` 环境：

```latex
\begin{equation}
    E=mc^2
\end{equation}
```

### 3. 上标与下标

`^` 表示上标，`_` 表示下标。

```latex
$x^2$

$a_1$

$x^{100}$

$a_{i+1}$
```

当上标或下标只有一个字符时，可以省略花括号；包含多个字符时，必须用 `{}` 把它们组成一个整体。

例如，`x^10` 实际上只会把 `1` 放在上标中，正确写法是：

```latex
x^{10}
```

### 4. 分数与根号

分数使用 `\frac{分子}{分母}`：

```latex
\[
    \frac{a+b}{c+d}
\]
```

根号使用 `\sqrt{}`：

```latex
\[
    \sqrt{x}
    \qquad
    \sqrt[3]{x}
\]
```

其中 `\sqrt[3]{x}` 表示三次根号。

### 5. 求和、乘积、极限与积分

```latex
\[
    \sum_{i=1}^{n} i
    \qquad
    \prod_{i=1}^{n} i
    \qquad
    \lim_{x\to 0} \frac{\sin x}{x}
    \qquad
    \int_{a}^{b} f(x)\,\mathrm{d}x
\]
```

其中：

| 命令 | 含义 |
|---|---|
| `\sum` | 求和符号 |
| `\prod` | 连乘符号 |
| `\lim` | 极限 |
| `\int` | 积分 |
| `\to` | 右箭头 |
| `\mathrm{d}` | 使用正体字母 d |

### 6. 常用希腊字母

```latex
\[
    \alpha,\ \beta,\ \gamma,\ \theta,\ \lambda,\ \mu,\ \pi,\ \sigma,\ \omega
\]

\[
    \Gamma,\ \Delta,\ \Theta,\ \Lambda,\ \Pi,\ \Sigma,\ \Omega
\]
```

大写希腊字母通常把命令的首字母改成大写，例如 `\delta` 对应 `\Delta`。

### 7. 括号

普通括号可以直接输入：

```latex
\[
    (a+b)
\]
```

如果括号中包含分数等较高的内容，可以使用 `\left` 和 `\right` 让括号自动调整大小：

```latex
\[
    \left(
        \frac{a}{b}
    \right)
\]
```

同样的方法也适用于方括号和大括号：

```latex
\[
    \left[ \frac{a}{b} \right]
    \qquad
    \left\{ \frac{a}{b} \right\}
\]
```

### 8. 多行公式

在导言区加载 `amsmath`：

```latex
\usepackage{amsmath}
```

然后使用 `align` 环境排列多行公式：

```latex
\begin{align}
    (a+b)^2
        &= a^2+2ab+b^2, \\
    (a-b)^2
        &= a^2-2ab+b^2.
\end{align}
```

`&` 用于指定各行对齐的位置，通常放在等号前；`\\` 表示结束当前一行。

如果不需要公式编号，可以使用 `align*`：

```latex
\begin{align*}
    x+y &= 10, \\
    x-y &= 2.
\end{align*}
```

### 9. 分段函数

`cases` 环境可以用于编写分段函数：

```latex
\[
f(x)=
\begin{cases}
    x,  & x \ge 0, \\
    -x, & x < 0.
\end{cases}
\]
```

### 10. 矩阵

```latex
\[
A=
\begin{pmatrix}
    1 & 2 \\
    3 & 4
\end{pmatrix}
\]
```

矩阵中使用 `&` 分隔每一列，使用 `\\` 结束每一行。

常见矩阵环境包括：

| 环境 | 外侧符号 |
|---|---|
| `matrix` | 无括号 |
| `pmatrix` | 圆括号 |
| `bmatrix` | 方括号 |
| `vmatrix` | 单竖线 |
| `Vmatrix` | 双竖线 |

---

## 功能包

LaTeX 的许多功能通过功能包提供。功能包使用 `\usepackage{}` 在导言区加载。

例如：

```latex
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{booktabs}
\usepackage{hyperref}
```

这些功能包分别用于：

| 功能包 | 常见用途 |
|---|---|
| `amsmath` | 多行公式、分段函数等数学排版 |
| `amssymb` | 更多数学符号 |
| `graphicx` | 插入图片 |
| `booktabs` | 绘制更美观的表格横线 |
| `hyperref` | 为目录、引用和网址添加链接 |

功能包必须放在 `\begin{document}` 之前加载。

---

## 插入图片

先在导言区加载：

```latex
\usepackage{graphicx}
```

然后在正文中使用：

```latex
\begin{figure}[htbp]
    \centering
    \includegraphics[width=0.7\textwidth]{images/example.png}
    \caption{示例图片}
    \label{fig:example}
\end{figure}
```

其中：

- `figure` 表示图片浮动环境
- `\centering` 表示居中
- `width=0.7\textwidth` 表示图片宽度为正文宽度的 70%
- `{images/example.png}` 是图片相对于 `.tex` 文件的路径
- `\caption{}` 设置图片标题
- `\label{}` 为图片设置引用标签

在其他位置可以使用下面的写法引用这张图片：

```latex
如图 \ref{fig:example} 所示。
```

LaTeX 会自动填写正确的图片编号。

> tip：`\label{}` 最好放在对应的 `\caption{}` 后面，否则可能引用到错误的编号。

`[htbp]` 表示允许 LaTeX 尝试把图片放在当前位置、页面顶部、页面底部或单独的浮动页。它是排版建议，不保证图片一定停留在源代码所在的位置。

---

## 表格

一个基础表格可以写成：

```latex
\begin{tabular}{lcr}
    姓名 & 学号 & 成绩 \\
    张三 & 001 & 90 \\
    李四 & 002 & 95
\end{tabular}
```

其中 `{lcr}` 用于设置三列的对齐方式：

```text
l：左对齐
c：居中对齐
r：右对齐
```

`&` 用于分隔每一列，`\\` 用于结束当前行。

如果需要带有标题和编号的表格，可以使用 `table` 环境，并通过 `booktabs` 功能包绘制表格线：

```latex
\begin{table}[htbp]
    \centering
    \caption{学生成绩}
    \label{tab:score}
    \begin{tabular}{lcc}
        \toprule
        姓名 & 学号 & 成绩 \\
        \midrule
        张三 & 001 & 90 \\
        李四 & 002 & 95 \\
        \bottomrule
    \end{tabular}
\end{table}
```

使用 `\toprule`、`\midrule` 和 `\bottomrule` 之前，需要在导言区加载：

```latex
\usepackage{booktabs}
```

引用表格的方法和引用图片相同：

```latex
具体结果见表 \ref{tab:score}。
```

---

## 标签与交叉引用

LaTeX 可以自动管理章节、公式、图片和表格的编号。

先使用 `\label{}` 设置标签：

```latex
\section{实验结果}
\label{sec:result}

\begin{equation}
    E=mc^2
    \label{eq:energy}
\end{equation}
```

再使用 `\ref{}` 引用编号：

```latex
实验结果见第 \ref{sec:result} 节。

能量关系如式 \ref{eq:energy} 所示。
```

也可以使用 `\pageref{}` 引用对应内容所在的页码：

```latex
详细推导见第 \pageref{eq:energy} 页。
```

修改文档结构后，LaTeX 会自动更新这些编号，不需要手动逐项修改。

为了方便管理，可以给不同类型的标签添加不同前缀：

```text
sec: 章节
fig: 图片
tab: 表格
eq:  公式
```

交叉引用有时需要编译两次。第一次编译记录标签信息，第二次编译才会把正确编号填入正文。

---

## 插入网址

在导言区加载：

```latex
\usepackage{hyperref}
```

正文中可以使用：

```latex
\url{https://www.latex-project.org/}
```

或者为链接指定显示文字：

```latex
\href{https://www.latex-project.org/}{LaTeX Project}
```

---

## 参考文献

篇幅较短时，可以先使用 `thebibliography` 环境手动管理参考文献：

```latex
\begin{thebibliography}{9}

\bibitem{lamport}
Leslie Lamport.
\textit{LaTeX: A Document Preparation System}.

\bibitem{knuth}
Donald E. Knuth.
\textit{The TeXbook}.

\end{thebibliography}
```

正文中使用 `\cite{}` 引用：

```latex
LaTeX 最初由 Leslie Lamport 开发 \cite{lamport}。
```

其中 `lamport` 是这条文献的标签，只用于源文件内部识别，不会直接显示在正文中。

当参考文献很多时，通常会把文献信息单独存放在 `.bib` 文件中，再使用 BibTeX 或 Biber 管理。本篇先掌握 `\bibitem` 和 `\cite` 即可。

---

## 编译过程

如果本地已经安装 XeLaTeX，可以在终端中进入 `main.tex` 所在的文件夹，然后执行：

```bash
xelatex main.tex
```

编译成功后，文件夹中会出现 `main.pdf`，同时还可能生成 `.aux`、`.log`、`.out`、`.toc` 等辅助文件。

这些辅助文件用于保存目录、引用和编译信息，一般不需要手动编辑。

如果文档中包含目录或交叉引用，通常需要连续编译两次：

```bash
xelatex main.tex
xelatex main.tex
```

不同编辑器也可能提供一键编译按钮，其本质仍然是调用对应的 LaTeX 编译器。

---

## 常见报错

### 1. `Undefined control sequence`

表示 LaTeX 遇到了无法识别的命令。

常见原因包括：

- 命令拼写错误
- 命令大小写错误
- 忘记加载对应的功能包

例如使用 `\includegraphics` 之前，需要加载：

```latex
\usepackage{graphicx}
```

### 2. `Missing $ inserted`

通常表示在普通文本中直接使用了只能出现在数学环境中的符号，或者忘记转义下划线 `_`。

```latex
% 错误
student_list.txt

% 正确
student\_list.txt
```

### 3. `File ... not found`

表示找不到图片、子文件或功能包。

遇到图片找不到时，可以依次检查：

- 文件名和后缀是否正确
- 英文大小写是否一致
- 图片路径是否相对于当前主文件
- 图片是否真的位于对应文件夹中

### 4. `Environment ... undefined`

表示使用了未定义的环境，通常是环境名拼写错误，或者缺少功能包。

例如 `align` 和 `cases` 环境需要：

```latex
\usepackage{amsmath}
```

### 5. 出现问号 `??`

如果引用编号或目录显示为 `??`，可以先再编译一次。

如果仍然没有恢复，再检查 `\label{}` 和 `\ref{}` 中的标签是否完全一致。

### 6. `\begin` 与 `\end` 不匹配

下面这种写法会报错：

```latex
\begin{itemize}
    \item 第一项
\end{enumerate}
```

开始和结束的环境名必须相同：

```latex
\begin{itemize}
    \item 第一项
\end{itemize}
```

---

## 推荐的文件结构

内容较少时，可以把所有内容写在一个 `main.tex` 中。

文档变长后，可以按照下面的方式整理：

```text
project/
├── main.tex
├── chapters/
│   ├── introduction.tex
│   └── experiment.tex
├── images/
│   ├── setup.png
│   └── result.png
└── references.bib
```

在 `main.tex` 中使用 `\input{}` 插入其他 `.tex` 文件：

```latex
\input{chapters/introduction}
\input{chapters/experiment}
```

这样可以把不同章节和图片分开管理，同时仍然从 `main.tex` 统一编译整篇文档。

---

## 综合例子

下面是一篇包含标题、目录、公式、表格和引用的完整文档：

````latex
\documentclass[UTF8]{ctexart}

\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{graphicx}
\usepackage{booktabs}
\usepackage{hyperref}

\title{LaTeX 示例报告}
\author{作者姓名}
\date{\today}

\begin{document}

\maketitle
\tableofcontents

\section{实验目的}

本实验用于学习 \LaTeX{} 的基本排版方法。

\section{实验原理}
\label{sec:theory}

质能方程为
\begin{equation}
    E=mc^2.
    \label{eq:energy}
\end{equation}

式 \ref{eq:energy} 中，$E$ 表示能量，$m$ 表示质量，$c$ 表示光速。

\section{实验步骤}

\begin{enumerate}
    \item 编写源文件
    \item 使用 XeLaTeX 编译
    \item 检查生成的 PDF
\end{enumerate}

\section{实验结果}

\begin{table}[htbp]
    \centering
    \caption{实验数据}
    \label{tab:data}
    \begin{tabular}{ccc}
        \toprule
        编号 & 质量/kg & 能量/J \\
        \midrule
        1 & 1.0 & $9.0\times10^{16}$ \\
        2 & 2.0 & $1.8\times10^{17}$ \\
        \bottomrule
    \end{tabular}
\end{table}

根据第 \ref{sec:theory} 节中的公式，可以计算得到表 \ref{tab:data} 中的结果。

\section*{总结}

本次实验学习了文档结构、数学公式、表格和交叉引用。

\end{document}
````

如果暂时记不住所有命令，可以先掌握下面这些内容：

```text
\section{}                 创建章节
\textbf{}                  设置粗体
$...$                      编写行内公式
\[...\]                    编写行间公式
\begin{} ... \end{}        创建一个环境
\label{} 与 \ref{}         设置和使用交叉引用
```

阅读 LaTeX 源文件时，可以按照这样的顺序：

**先看导言区确定文档类型和功能包，再看正文中的章节结构，最后查看各个环境和命令中的具体内容。**

实际编写时也不需要从空白文件开始。可以先复制第一个最小示例，确保它能够正常编译，再逐步加入公式、图片和表格。每增加一部分就编译一次，这样出现错误时更容易找到原因。

---

> Edit: _serendipity
