<h1 id="Tool" align="center">LaTeX</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. LaTeX 基础](#1-latex-基础)
  - [1.1. TeX Live + VS Code 搭建环境](#11-tex-live-vs-code-搭建环境)
  - [1.2. Hello, LaTeX](#12-hello-latex)
  - [1.3. 文章元信息](#13-文章元信息)
  - [1.4. 章节与段落](#14-章节与段落)
    - [1.4.1. 换行](#141-换行)
  - [1.5. 目录](#15-目录)
  - [1.6. 图片](#16-图片)
  - [1.7. 表格](#17-表格)
    - [1.7.1. 三线表](#171-三线表)
  - [1.8. 浮动体](#18-浮动体)
  - [1.9. 数学公式](#19-数学公式)

<!-- /code_chunk_output -->

# 1. LaTeX 基础

> 参考文献
> [VS CODE+LATEX 完全解决方案（2020年11月6号）](https://zhuanlan.zhihu.com/p/106167792)
> [一份其实很短的 LaTeX 入门文档](https://liam.page/2014/09/08/latex-introduction/)

## 1.1. TeX Live + VS Code 搭建环境

1. 下载 TeX Live 腾讯云：https://mirrors.cloud.tencent.com/CTAN/systems/texlive/Images/texlive2020.iso
2. 以管理员权限执行 install-tl-windows.bat 文件 
(p.s. 可以不安装 TeXwords)
3. 目录 texlive/2020/bin/win32/ 加入环境变量 PATH
4. VS Code 安装 LaTeX workshop 插件


## 1.2. Hello, LaTeX
```latex
\documentclass{article}
% 这里是导言区
\begin{document}
Hello, $L^AT_EX$
\end{document}
```
结果：
> $$
> Hello, L^AT_EX
> $$

- `\documentclass{article}` 是一个控制序列 documentclass，{article} 是其的一个参数。表示调用名为 article 的文档类(documentclass)。
  > 控制序列：是以反斜杠 \ 开头，以第一个空格或非字母字符结束的一串文字。TeX 对控制序列的大小写是敏感的。
  > 文档类：即是 TeX 系统的一些格式的集合，不同的文档类在输出效果上会有差别。
  > 部分控制序列还有被方括号 [] 包括的可选参数。
- `% 这里是导言区` 是 TeX 的注释。使用 `\%` 来表示百分号本身。
- 导言区: 从 `\documentclass{article}` 开始到 `\begin{document}` 之前的部分被称为导言区。
  > 导言区是对整篇文档进行设置的区域，在导言区出现的控制序列，往往会影响整篇文档的格式。
- `\begin{document} \end{document}` begin-end 这两个控制序列要成对出现。
  > 这两个控制序列以及他们中间的内容被称为环境。它们的参数必须一致，称为环境名。
  > 只有在 document 环境中的内容，才会被正常输出到文档中去或是作为控制序列对文档产生影响。在 `\end{document}` 之后插入任何内容都是无效的。

**中文解决方案**
使用 CTeX 宏集来处理中英文混排的文档：以 UTF-8 编码保存，使用 XeLaTeX 编译。
```latex
\documentclass[UTF8]{ctexart}
% 这里是导言区
\begin{document}
你好, $L^AT_EX$
\end{document}
```
结果：
> $$
> 你好, L^AT_EX
> $$

## 1.3. 文章元信息

```latex
\documentclass[UTF8]{ctexart}
% 这里是导言区
\title{你好, $L^AT_EX$}
\author{aBadString}
\date{\today}
% 这里是导言区
\begin{document}
\maketitle
你好, $L^AT_EX$
\end{document}
```
> 结果：
> ![](../images/LaTeX/你好LaTeX.png)

- 导言区可以定义了标题、作者、日期等信息
- 使用 `\maketitle` 可以将在导言区中定义的标题、作者、日期按照预定的格式展现出来。

## 1.4. 章节与段落

```latex
\documentclass[UTF8]{ctexart}
% 这里是导言区
\title{你好, $L^AT_EX$}
\author{aBadString}
\date{\today}
% 这里是导言区
\begin{document}
\maketitle

\section{你好中国}
中国在East Asia.

    \subsection{Hello Beijing}
    北京是capital of China.

        \subsubsection{Hello Dongcheng District}
            \paragraph{Tian'anmen Square}
            is in the center of 
                \subparagraph{Chairman Mao}
                is in the center of 天安门广场。
    
    \subsection{Hello 山东}
        \paragraph{山东大学} is one of the best university in 山东。

\end{document}
```
> 结果：
> ![](../images/LaTeX/章节与段落.png)

文档类 article/ctexart 中，定义了五个控制序列来调整行文组织结构：
- `\section{ }`
- `\subsection{ }`
- `\subsubsection{ }`
- `\paragraph{ }`
- `\subparagraph{ }`

### 1.4.1. 换行
```latex
% 无换行，无空格
中国在East Asia.中国在East Asia.

% 一个换行符解释为一个空格
中国在East Asia.
中国在East Asia.

% 两个换行符解释为一个换行
中国在East Asia.

中国在East Asia.
```
> 结果：
> ![](../images/LaTeX/换行.png)


## 1.5. 目录

```latex
\documentclass[UTF8]{ctexart}
% 这里是导言区
\title{你好, $L^AT_EX$}
\author{aBadString}
\date{\today}
% 这里是导言区
\begin{document}
\maketitle
\tableofcontents

\section{你好中国}
中国在East Asia.

    \subsection{Hello Beijing}
    北京是capital of China.

        \subsubsection{Hello Dongcheng District}
            \paragraph{Tian'anmen Square}
            is in the center of 
                \subparagraph{Chairman Mao}
                is in the center of 天安门广场。
    
    \subsection{Hello 山东}
        \paragraph{山东大学} is one of the best university in 山东。

\end{document}
```
> 目录之后紧接正文
> ![](../images/LaTeX/目录之后紧接正文.png)

使用 `\tableofcontents` 控制序列来插入命令。
- 当 `\maketitle` 在 `\tableofcontents` 前面时，先是标题、文章元信息，然后是目录，目录后面紧接正文。
- 当 `\maketitle` 在 `\tableofcontents` 后面时，目录单独出现在第一页，换页后第二页是标题、文章元信息、正文。


## 1.6. 图片

graphicx 宏包提供的 `\includegraphics` 命令可以用于插入图片

```latex
\documentclass{article}
\usepackage{graphicx}
\begin{document}
\includegraphics[width = 0.1\textwidth]{moon - sky - snow.png}
\end{document}
```

- {} 中的参数是图片的路径名
- [] 中是可选参数，width = 0.1\textwidth 表示图片宽度设置为页面宽度的 0.1 倍，高度等比例缩放。

## 1.7. 表格
```latex
\begin{tabular}{|lc|r|}
\hline
操作系统 & 发行版 & 编辑器 \\
Windows & MikTeX & TexMakerX \\
\hline
Unix/Linux & teTeX & Kile \\
\hline
Mac OS & MacTeX & TeXShop \\
\hline
通用 & TeX Live & TeXworks \\
\hline
\end{tabular}
```
> ![](../images/LaTeX/表格.png)

- `\begin{tabular} \end{tabular}` 中间是表格的内容
- `\begin{tabular}{|lc|r|}` 第二个参数表示每一列的对齐方式（居左l、居中c、居右r）和列之间竖线（用 | 表示竖线）
- `\hline` 表示横线
- `&` 用于分列
- `\\` 用于换行

### 1.7.1. 三线表

使用 booktabs 宏包来制作三线表格。

```latex
\documentclass{article} 
% 提供命令 \toprule、\midrule、\bottomrule
\usepackage{booktabs}

\begin{document}
 
%经典三线表
\begin{table}[h]
% 标题  表 1: 各种操作系统上的 LaTeX 编辑器
\caption{各种操作系统上的 LaTeX 编辑器}
\centering % 把表居中

% 一共四列，内容全部居中，中间无竖线
\begin{tabular}{ccc}
\toprule % 第一道横线
操作系统 & 发行版 & 编辑器 \\
\midrule % 第二道横线 
Windows & MikTeX & TexMakerX \\
Unix/Linux & teTeX & Kile \\
Mac OS & MacTeX & TeXShop \\
通用 & TeX Live & TeXworks \\
\bottomrule % 第三道横线
\end{tabular}

\end{table}
 
\end{document}
```
> ![](../images/LaTeX/三线表.png)


## 1.8. 浮动体

figure 和 table 环境可以自动调整位置的环境，称作浮动体(float)。
- htbp 选项用来指定插图的理想位置，这几个字母分别代表 here, top, bottom, float page
- `\centering` 用来使图表居中
- `\caption` 命令设置图表标题，LaTeX 会自动给浮动体的标题加上编号。

## 1.9. 数学公式

数学模式需要引入宏包 `\usepackage{amsmath}`
LaTeX 的数学模式有两种：
- 行内模式 (inline)
- 行间模式 (display)

```latex
行内公式：$a^2 + b^2 = c^2$.

行间公式: \[ a^2 + b^2 = c^2. \]

编号行间公式：
\begin{equation}
a^2 + b^2 = c^2.
\end{equation}
```
> ![](../images/LaTeX/行内公式与行间公式.png)

行内公式的标点，应该放在数学模式的限定符之外；而行间公式则应该放在数学模式限定符之内。

**上下标**
```latex
$ a^2 - a^{cd} - x_0 - x_{seq} $
```
> $ a^2 - a^{cd} - x_0 - x_{seq} $

**分式**
```latex
行内 $ \frac{1}{a+b} $
\[ \frac{1}{a+b} \]

行内 $ \dfrac{1}{a+b} $
\[ \tfrac{1}{a+b} \]
```
> 行内 $ \frac{1}{a+b} $
> \[ \frac{1}{a+b} \]
> 行内 $ \dfrac{1}{a+b} $
> \[ \tfrac{1}{a+b} \]

可以发现，行内分式字体会缩小，可以使用 \dfrac 保持原字体大小。\tfrac 可以使行间公式字体缩小。

**根式**
```latex
\sqrt{a*b}
```
> $$ \sqrt{a*b} $$
