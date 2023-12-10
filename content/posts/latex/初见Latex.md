---
title: "初见Latex"
date: 2023-07-27T22:09:53+08:00
type: "post"
---
今天下载好 

``` 
kpathsea: Running mktexfmt tex.fmt
tcfmgr: config file `tcfmgr.map' (usually in $TEXMFMAIN/texconfig) not found (ls-R missing?).
fmtutil: config file `fmtutil.cnf' not found.
```


```
\emph{param}
```
此命令会让 param 变为斜体，因为 \emph 的默认设置为 italic 斜体

| Command | Declaration | Meaning |
| --- | --- | --- |
| \textrm{...} | \rmfamily | roman family |
| \textsf{...} | \sffamily | sans-serif family |
| \texttt{...} | \ttfamily | typewriter family |
| \textbf{...} | \bfseries | bold-face |
| \textmd{...} | \mdseries | medium |
| \textit{...} | \itshape | italic shape |
| \textsl{...} | \slshape | slanted shpe |
| \textsc{...} | \scshape | small caps shape |
| \textup{...} | \upshape | upright shape |
| \textnormal{...} | \normalfont | default font |


## 用户自定义宏
```latex
\documentclass {article}
\usepackage {xspace}
\newcommand{\TUG} {\TeX\ Users Group\xspace}

\begin{document}
\section{The \TUG}
The \TUG is an organization for people who use \TeX\ or \LaTeX.

\TUG
\end{document}
```

### 带参数的用户自定义宏
用一个可选参数来控制，#1 代表第一个参数的使用，#2 代表第二个参数的使用，以此类推

```latex
\document{article}
\newcommand{\keyword}[1]{\textbf{#1}}

\begin{document}
\keyword{Grouping} by curly braces limits the \keyword{scope}
of \keyword{declarations}.

\end{document}
```

## box
``` latex
\parbox[alignment]{width}{text}

\parbox[alignment] [height] [inner alignment] {width} {height}
```

texlive-latex


Tex Live
MiKTex

Texworks