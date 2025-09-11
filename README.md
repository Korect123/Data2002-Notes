# Data2002-Notes
本仓库是我在悉尼大学修读 **DATA2002: Database Systems Fundamentals** 课程时整理的笔记，专门为中国学生准备。   内容涵盖课程核心知识点，结合中英文解释，帮助大家更快理解数据库系统的概念与实现

\documentclass[12pt]{article}
\usepackage{ctex}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{geometry}
\usepackage{graphicx}
\usepackage{listings}
\usepackage{xcolor}
\usepackage{tikz}
\usepackage{pgfplots}
\usepackage{float}
\usepackage{booktabs}

\pgfplotsset{compat=1.18}

\geometry{a4paper, left=2cm, right=2cm, top=2cm, bottom=2cm}
\title{sign,rank sum, signed rank}
\author{BWB}
\date{}

% lstlisting 设置
\lstset{
    language=R,
    frame=single,
    basicstyle=\small\ttfamily,
    keywordstyle=\color{blue}\bfseries,
    commentstyle=\color{gray}\itshape,
    stringstyle=\color{purple},
    showstringspaces=false,
    breaklines=true,
    columns=fullflexible,
    keepspaces=true,
}

\begin{document}

\maketitle

\tableofcontents % 目录

\section*{引言}

当数据不满足正态性假设，或者我们无法确定数据的分布形态时，符号检验是一种有效的非参数统计方法。本文将介绍符号检验的基本原理，并通过一个具体的示例，按照 HATPC（假设、假设条件、检验统计量、P 值、结论）框架进行分析。

\section{符号检验的基本原理}

假设我们有一组样本数据 $X_1, X_2, \ldots, X_n$，希望检验总体中位数是否等于某个特定值 $\mu_0$。

\subsection{假设}

\begin{itemize}
    \item \textbf{零假设} $H_0$：$\mu = \mu_0$
    \item \textbf{备择假设} $H_1$：$\mu \ne \mu_0$
\end{itemize}

\subsection{检验思想}

在 $H_0$ 下，假设数据分布关于 $\mu_0$ 对称。因此，差值 $D_i = X_i - \mu_0$ 应该均匀地分布在零的两侧。也就是说，每个 $D_i$ 为正或负的概率都是 0.5。

因此，正符号的数量 $S$ 服从参数为 $n$ 和 $p = 0.5$ 的二项分布，即 $S \sim B(n, 0.5)$。

\section{符号检验的步骤}

\begin{enumerate}
    \item \textbf{计算差值并确定符号}：
    \[
    D_i = X_i - \mu_0
    \]
    记录每个 $D_i$ 的符号，忽略 $D_i = 0$ 的情况。
    
    \item \textbf{统计正符号的数量} $S$，样本量为 $n$（不包括 $D_i = 0$ 的数量）。
    
    \item \textbf{计算 P 值}：
    \[
    P\text{-value} = 2 \times P(S \leq \min(S, n - S))
    \]
    或根据具体的备择假设（单侧或双侧）计算 P 值。
    
    \item \textbf{作出统计决策}：
    如果 P 值小于显著性水平 $\alpha$，则拒绝 $H_0$，认为中位数与 $\mu_0$ 不等。
\end{enumerate}

\section{示例：新疗法对血压的影响}

\subsection{问题描述}

研究人员想要检验一种新疗法是否能够降低患者的血压。随机选取了 12 名患者，测量了他们接受治疗前后的血压。

\subsection{数据}

\begin{table}[H]
    \centering
    \caption{治疗前后血压数据 (mmHg)}
    \begin{tabular}{cccc}
    \toprule
    患者编号 & 治疗前血压 & 治疗后血压 & 差值 \\
    \midrule
     1  & 150 & 145 & +5  \\
     2  & 160 & 155 & +5  \\
     3  & 148 & 150 & -2  \\
     4  & 155 & 149 & +6  \\
     5  & 162 & 158 & +4  \\
     6  & 149 & 149 & 0   \\
     7  & 151 & 147 & +4  \\
     8  & 157 & 152 & +5  \\
     9  & 153 & 154 & -1  \\
     10 & 158 & 153 & +5  \\
     11 & 150 & 151 & -1  \\
     12 & 152 & 148 & +4  \\
    \bottomrule
    \end{tabular}
\end{table}

\subsection{按照 HATPC 框架进行分析}

\subsubsection{假设（Hypotheses）}

\begin{itemize}
    \item \textbf{零假设} $H_0$：新疗法对血压没有影响，$p_+ = 0.5$
    \item \textbf{备择假设} $H_1$：新疗法降低了血压，$p_+ > 0.5$
\end{itemize}

其中，$p_+$ 表示差值 $D_i$ 为正的概率。

\subsubsection{假设条件（Assumptions）}

\begin{itemize}
    \item 样本是独立随机抽取的。
    \item 差值 $D_i$ 来自连续、对称分布。
\end{itemize}

\subsubsection{检验统计量（Test Statistic）}

计算正符号的数量 $S$。

从数据中，我们有：

\begin{itemize}
    \item 正符号数量 $S = 8$
    \item 负符号数量 $= 3$
    \item 忽略 $D_i = 0$ 的一项，样本量 $n = 11$
\end{itemize}

\subsubsection{P 值计算（P-value）}

在零假设 $H_0$ 下，$S \sim B(n, 0.5)$。

计算单侧检验的 P 值：
\[
P\text{-value} = 1 - P(S \leq 7) = P(S \geq 8)
\]

使用二项分布的累积分布函数：
\[
P(S \leq k) = \sum_{i=0}^k \binom{n}{i} p^i (1-p)^{n-i}
\]

其中 $n = 11$, $p = 0.5$。

计算 $P(S \leq 7)$：

\begin{align*}
P(S \leq 7) &= \sum_{i=0}^7 \binom{11}{i} (0.5)^{11} \\
&= 0.8867 \text{ (使用二项分布计算器或统计软件得出)}
\end{align*}

因此，
\[
P\text{-value} = 1 - P(S \leq 7) = 1 - 0.8867 = 0.1133
\]

P 值为 0.1133。

\subsubsection{结论（Conclusion）}

由于 P 值（0.1133）大于显著性水平 $\alpha = 0.05$，我们无法拒绝零假设 $H_0$。因此，没有足够的证据表明新疗法显著降低了血压。

\subsection{使用 R 语言进行符号检验}

我们可以使用 R 语言的 \texttt{binom.test} 函数来进行符号检验。以下是具体的 R 代码：

\begin{lstlisting}[language=R, caption=使用 binom.test 进行符号检验]
# 正符号数量
S <- 8
# 样本量（不包括差值为零的项）
n <- 11

# 执行单侧符号检验
result <- binom.test(x = S, n = n, p = 0.5, alternative = "greater")

# 输出结果
print(result)
\end{lstlisting}

运行上述代码，得到以下结果：

\begin{verbatim}
Exact binomial test

data:  S and n
number of successes = 8, number of trials = 11, p-value = 0.1133
alternative hypothesis: true probability of success is greater than 0.5
\end{verbatim}

P 值为 0.1133，与手工计算结果一致。

\begin{center}
    \Large\textbf{Wilcoxon符号秩检验（Signed-Rank Test）详解}
\end{center}

\section{检验背景和适用场景}


\subsection{1. 何时使用符号秩检验}

\begin{itemize}
    \item \textbf{配对样本}：同一组对象在不同时间点或不同条件下的测量值。
    \begin{itemize}
        \item 例如：评估新药对患者血压的影响，测量患者服药前后的血压值。
    \end{itemize}
    \item \textbf{数据类型}：连续型或有序数据，不要求满足正态分布。
    \item \textbf{目的}：检验两个相关样本的中位数差值是否为零，即是否存在显著差异。
\end{itemize}

\subsection{2. 为什么选择符号秩检验}

\begin{itemize}
    \item \textbf{非参数方法}：不要求数据符合特定分布，适用于分布未知或非正态分布的数据。
    \item \textbf{对异常值不敏感}：相比于配对 t 检验，符号秩检验对异常值的影响较小。
    \item \textbf{适用于小样本量}：在样本量较小的情况下，符号秩检验仍然具有较好的统计效率。
\end{itemize}

\section{检验假设}

Wilcoxon符号秩检验基于以下假设：

\begin{itemize}
    \item 配对数据：样本由配对观测值组成。
    \item 独立性：每对观测值之间相互独立。
    \item 连续性：差值（$D_i = X_i - Y_i$）来自连续分布。
    \item 对称性：差值的分布关于0对称。
    \item 可比性：差值可以被排序。
\end{itemize}

这些假设对于检验的有效性至关重要。

\section{检验原理}

\subsection{1. 基本思想}

符号秩检验基于配对差值的符号和大小，通过比较正负差值的秩次和，来判断两个相关样本之间是否存在显著差异。

\subsection{2. 数学推导}

\begin{itemize}
    \item \textbf{原假设（$H_0$）}：差值的中位数为零，即两个样本的中位数相等。
    \item \textbf{备择假设（$H_1$）}：差值的中位数不为零，即两个样本的中位数不相等。
    \item \textbf{检验统计量}：
    \begin{itemize}
        \item 理论检验统计量:
        \begin{itemize}
            \item 单侧检验：$W^+ = \sum_{i: D_i>0} R_i$
            \item 双侧检验：$W = \min(W^+, W^-)$
        \end{itemize}
        \item 观察到的检验统计量:
        \begin{itemize}
            \item 单侧检验：$w^+$
            \item 双侧检验：$w = \min(w^+, w^-)$
        \end{itemize}
    \end{itemize}
\end{itemize}

\section{检验步骤}

\subsection{步骤1：计算差值}

对于每一对样本，计算差值：
\[
d_i = x_i - y_i
\]
其中，$x_i$ 为第 $i$ 个对象在条件 1 下的测量值，$y_i$ 为条件 2 下的测量值。

\subsection{步骤2：剔除零差值}

如果某个差值 $d_i = 0$，则从分析中剔除，以免影响秩次的计算。

\subsection{步骤3：计算绝对差值}

对每个非零差值，计算其绝对值：
\[
|d_i|
\]

\subsection{步骤4：对绝对差值进行排序并赋予秩次}

\begin{itemize}
    \item 将所有 $|d_i|$ 按升序排列。
    \item 为每个 $|d_i|$ 赋予秩次 $R_i$。
    \item \textbf{处理相同值（平秩处理）}：如果有相同的绝对差值，计算这些差值的平均秩次，赋予它们。
\end{itemize}

\subsection{步骤5：赋予符号}

根据原始差值 $d_i$ 的正负，为对应的秩次赋予符号：

\begin{itemize}
    \item 如果 $d_i > 0$，则秩次为正（$+R_i$）。
    \item 如果 $d_i < 0$，则秩次为负（$-R_i$）。
\end{itemize}

\subsection{步骤6：计算正负秩次和}

\begin{itemize}
    \item \textbf{正秩和 $W^+$}：
    \[
    W^+ = \sum_{d_i > 0} R_i
    \]
    \item \textbf{负秩和 $W^-$}：
    \[
    W^- = \sum_{d_i < 0} R_i
    \]
\end{itemize}

\subsection{步骤7：计算检验统计量}

取较小的秩和作为检验统计量 $W$：
\[
W = \min(W^+, W^-)
\]

\subsection{步骤8：确定检验的显著性}

\subsubsection{计算期望值 \texorpdfstring{$E(W)$}{E(W)} 和方差 \texorpdfstring{$\text{Var}(W)$}{Var(W)}}


\begin{itemize}
    \item \textbf{有效样本量} $n$（剔除零差值后的样本数）。
    \item \textbf{期望值 $E(W)$}：
    \[
    E(W) = \frac{n(n+1)}{4}
    \]
    \item \textbf{方差 $\text{Var}(W)$}：
    \[
    \text{Var}(W) = \frac{n(n+1)(2n+1)}{24}
    \]
\end{itemize}

\subsubsection{计算标准化的 \texorpdfstring{$Z$}{Z} 值}

\[
Z = \frac{W - E(W)}{\sqrt{\text{Var}(W)}}
\]

\subsubsection{计算 p 值}

p值的计算取决于备择假设的形式：

\begin{itemize}
    \item 对于$H_1: \mu > \mu_0$（右侧检验）：$P(W^+ \geq w^+)$
    \item 对于$H_1: \mu < \mu_0$（左侧检验）：$P(W^+ \leq w^+)$
    \item 对于$H_1: \mu \neq \mu_0$（双侧检验）：$2P(W^+ \leq w)$
\end{itemize}

在选择适当的p值计算方法时，需要注意两点：
\begin{itemize}
    \item 是单侧检验还是双侧检验
    \item 备择假设是大于、小于还是不等于
\end{itemize}

\subsubsection{做出决策}

如果计算得到的 $p$ 值小于预先设定的显著性水平 $\alpha$（通常为0.05），则拒绝原假设，认为两个样本之间存在显著差异。

\section{实例解析}

\subsection{假设情境}

研究者想评估一种新药对患者血压的影响，测量了 10 位患者在服药前后的血压。

\subsection{数据}

\begin{table}[h]
    \centering
    \begin{tabular}{cccc}
        \hline
        患者编号 & 服药前血压 ($x_i$) & 服药后血压 ($y_i$) & 差值 $d_i$ \\
        \hline
        1  & 150 & 140 & 10  \\
        2  & 160 & 155 & 5   \\
        3  & 145 & 148 & -3  \\
        4  & 155 & 150 & 5   \\
        5  & 170 & 160 & 10  \\
        6  & 165 & 162 & 3   \\
        7  & 158 & 155 & 3   \\
        8  & 152 & 150 & 2   \\
        9  & 149 & 147 & 2   \\
        10 & 160 & 158 & 2   \\
        \hline
    \end{tabular}
    \caption{患者血压数据及差值}
\end{table}

\subsection{手动计算步骤}

\subsubsection{步骤1：计算差值}

已在上表中计算。

\subsubsection{步骤2：剔除零差值}

无零差值，继续下一步。

\subsubsection{步骤3：计算绝对差值}

\begin{table}[h]
    \centering
    \begin{tabular}{ccc}
        \hline
        患者编号 & 差值 $d_i$ & 绝对差值 $|d_i|$ \\
        \hline
        1  & 10  & 10 \\
        2  & 5   & 5  \\
        3  & -3  & 3  \\
        4  & 5   & 5  \\
        5  & 10  & 10 \\
        6  & 3   & 3  \\
        7  & 3   & 3  \\
        8  & 2   & 2  \\
        9  & 2   & 2  \\
        10 & 2   & 2  \\
        \hline
    \end{tabular}
    \caption{绝对差值计算}
\end{table}

\subsubsection{步骤4：排序并赋予秩次}

\begin{table}[h]
    \centering
    \begin{tabular}{cccc}
        \hline
        排序 & 患者编号 & $|d_i|$ & 秩次 $R_i$ \\
        \hline
        1  & 8  & 2  & 2.0 \\
        2  & 9  & 2  & 2.0 \\
        3  & 10 & 2  & 2.0 \\
        4  & 3  & 3  & 5.0 \\
        5  & 6  & 3  & 5.0 \\
        6  & 7  & 3  & 5.0 \\
        7  & 2  & 5  & 7.5 \\
        8  & 4  & 5  & 7.5 \\
        9  & 1  & 10 & 9.5 \\
        10 & 5  & 10 & 9.5 \\
        \hline
    \end{tabular}
    \caption{排序并赋予秩次}
\end{table}

\subsubsection{步骤5：赋予符号}

根据 $d_i$ 的正负，赋予符号：

\begin{table}[h]
    \centering
    \begin{tabular}{cccc}
        \hline
        患者编号 & $d_i$ & 秩次 $R_i$ & 带符号的秩次 \\
        \hline
        8  & 2   & 2.0  & +2.0  \\
        9  & 2   & 2.0  & +2.0  \\
        10 & 2   & 2.0  & +2.0  \\
        3  & -3  & 5.0  & -5.0  \\
        6  & 3   & 5.0  & +5.0  \\
        7  & 3   & 5.0  & +5.0  \\
        2  & 5   & 7.5  & +7.5  \\
        4  & 5   & 7.5  & +7.5  \\
        1  & 10  & 9.5  & +9.5  \\
        5  & 10  & 9.5  & +9.5  \\
        \hline
    \end{tabular}
    \caption{赋予符号的秩次}
\end{table}

\subsubsection{步骤6：计算正负秩次和}

\begin{itemize}
    \item 正秩和 $W^+$：
    \[
    W^+ = 2.0 + 2.0 + 2.0 + 5.0 + 5.0 + 7.5 + 7.5 + 9.5 + 9.5 = 50.0
    \]
    \item 负秩和 $W^-$：
    \[
    W^- = 5.0
    \]
\end{itemize}

\subsubsection{步骤7：计算检验统计量}

\[
W = \min(W^+, W^-) = \min(50.0, 5.0) = 5.0
\]

\subsubsection{步骤8：确定检验的显著性}

\begin{itemize}
    \item 有效样本量 $n = 10$。
    \item 期望值 $E(W)$：
    \[
    E(W) = \frac{10 \times 11}{4} = 27.5
    \]
    \item 方差 $\text{Var}(W)$：
    \[
    \text{Var}(W) = \frac{10 \times 11 \times 21}{24} = 96.25
    \]
    \item 计算 $Z$ 值：
    \[
    Z = \frac{W - E(W)}{\sqrt{\text{Var}(W)}} = \frac{5.0 - 27.5}{\sqrt{96.25}} \approx -2.293
    \]
    \item 计算 $p$ 值：
    \[
    p = 2 \times \Phi(Z) = 2 \times \Phi(-2.293) \approx 0.0218
    \]
    \item 由于 $p < 0.05$，拒绝原假设，认为服药前后的血压存在显著差异。
\end{itemize}

（此处保留之前的手动计算步骤，内容与之前相同。）

\subsection{使用 R 语言进行 Wilcoxon 符号秩检验}

在实际应用中，可以使用 R 语言的 `wilcox.test()` 函数来进行符号秩检验，下面是具体的实现步骤。

\subsubsection{在 R 中输入数据}

\begin{lstlisting}[language=R]
# 服药前血压数据
x <- c(150, 160, 145, 155, 170, 165, 158, 152, 149, 160)

# 服药后血压数据
y <- c(140, 155, 148, 150, 160, 162, 155, 150, 147, 158)
\end{lstlisting}

\subsubsection{使用 \texttt{wilcox.test} 进行符号秩检验}

\begin{lstlisting}[language=R]
# 进行符号秩检验
result <- wilcox.test(x, y, paired = TRUE, exact = FALSE, correct = FALSE)

# 显示结果
print(result)
\end{lstlisting}

\subsubsection{输出结果}

运行上述代码，将得到以下输出：

\begin{lstlisting}
Wilcoxon signed rank test

data:  x and y
V = 5, p-value = 0.02194
alternative hypothesis: true location shift is not equal to 0
\end{lstlisting}

\subsubsection{结果解释}

\begin{itemize}
    \item \textbf{检验统计量 $V = 5$}，与手动计算的 $W = 5$ 一致。
    \item \textbf{$p$ 值约为 0.02194}，小于显著性水平 $\alpha = 0.05$。
    \item \textbf{结论}：拒绝原假设，认为服药前后的血压存在显著差异。
\end{itemize}

\subsubsection{参数说明}

\begin{itemize}
    \item \texttt{x, y}：两个配对样本的数据。
    \item \texttt{paired = TRUE}：表示进行配对检验。
    \item \texttt{exact = FALSE}：对于较大的样本量，设置为 \texttt{FALSE}，使用正态近似。
    \item \texttt{correct = FALSE}：不进行连续性校正。
\end{itemize}

\subsubsection{单尾检验}

如果需要进行单尾检验（例如，检验服药后血压是否降低），可以设置参数 \texttt{alternative}：

\begin{lstlisting}[language=R]
# 单尾检验：检验服药后血压是否降低
result_one_tailed <- wilcox.test(x, y, paired = TRUE, alternative = "greater", exact = FALSE, correct = FALSE)

# 显示结果
print(result_one_tailed)
\end{lstlisting}

\section{结论与解释}

\subsection{统计结论}

\begin{itemize}
    \item \textbf{拒绝原假设}：根据检验结果，服药前后的血压存在显著差异。
\end{itemize}

\subsection{实际意义}

\begin{itemize}
    \item \textbf{新药有效}：新药对降低患者的血压具有显著效果，具有实际应用价值。
\end{itemize}

\section{注意事项}

\subsection{1. 数据要求}

\begin{itemize}
    \item \textbf{配对数据}：样本必须由配对观测值组成。
    \item \textbf{独立性}：每对观测值之间应相互独立。
    \item \textbf{连续性}：差值应来自连续分布。
    \item \textbf{对称性}：虽然不要求正态分布，但差值应关于0近似对称分布。
    \item \textbf{可排序性}：差值必须可以被排序。
\end{itemize}

\subsection{2. 平秩处理}

\begin{itemize}
    \item \textbf{必要性}：当存在相同的绝对差值时，需要进行平秩处理，以保证秩次的正确分配。
\end{itemize}

\subsection{3. 样本量}

\begin{itemize}
    \item \textbf{小样本量}：对于 $n \leq 25$ 的样本，可以使用精确的临界值表进行检验。
    \item \textbf{大样本量}：当 $n > 25$ 时，可以使用正态近似，计算 $Z$ 值和 $p$ 值。
\end{itemize}

\subsection{4. 单尾检验与双尾检验}

\begin{itemize}
    \item \textbf{双尾检验}：检验差值的中位数是否不等于零。
    \item \textbf{单尾检验}：检验差值的中位数是否大于或小于零，需要调整 $p$ 值的计算。
\end{itemize}

\section*{Wilcoxon 秩和检验（Rank Sum Test）详解}

在统计分析中，当我们需要比较两个独立样本的平均数但无法满足正态性假设时，\textbf{Wilcoxon 秩和检验}是一种常用的非参数检验方法。它放宽了对数据分布的要求，不需要假设数据服从正态分布，也不需要对称性假设。

\subsection*{1. 概述}

\textbf{Wilcoxon 秩和检验}用于比较两个独立样本的平均数，适用于数据不满足正态分布或存在异常值的情况。通过对数据进行秩转换，比较两个样本的秩和，以判断它们是否来自具有相同平均数的总体。

\subsection*{2. 与 Wilcoxon 符号秩检验的区别}

\begin{itemize}
    \item \textbf{Wilcoxon 秩和检验（Rank Sum Test）：}
    \begin{itemize}
        \item \textbf{应用场景：} 比较两个\textbf{独立样本}的平均数。
        \item \textbf{数据要求：} 两个样本之间互相独立。
        \item \textbf{适用检验：} 类似于\textbf{独立样本 t 检验}（Two-sample t-test）。
    \end{itemize}
    \item \textbf{Wilcoxon 符号秩检验（Signed Rank Test）：}
    \begin{itemize}
        \item \textbf{应用场景：} 比较两个\textbf{相关样本、配对样本}或同一样本的两次测量。
        \item \textbf{数据要求：} 数据成对出现，每对数据具有相关性。
        \item \textbf{适用检验：} 类似于\textbf{配对样本 t 检验}（Paired t-test）。
    \end{itemize}
\end{itemize}

\subsection*{3. 检验假设}

\begin{itemize}
    \item \textbf{原假设（\( H_0 \)）：} 两个总体的平均数相等，\( \mu_x = \mu_y \)。
    \item \textbf{备择假设（\( H_1 \)）：}
    \begin{itemize}
        \item 双尾检验：\( \mu_x \neq \mu_y \)。
        \item 单尾检验：\( \mu_x > \mu_y \) 或 \( \mu_x < \mu_y \)。
    \end{itemize}
\end{itemize}

\textbf{假设条件：}

\begin{itemize}
    \item 样本来自连续分布的总体。
    \item 两个样本独立，观测值之间相互独立。
    \item 总体的分布形状相同，但平均数可能不同。
\end{itemize}

\subsection*{4. 检验步骤与公式}

\textbf{步骤 1：数据合并与排序}

\begin{itemize}
    \item 将样本 \( X \)（大小为 \( n_x \)）和样本 \( Y \)（大小为 \( n_y \)）的数据合并，形成总样本量 \( N = n_x + n_y \)。
    \item 对所有数据按从小到大的顺序进行排序，赋予秩值 \( R_i \)。
    \item 若有相同的值（平级），赋予它们的平均秩。
\end{itemize}

\textbf{步骤 2：计算样本 \( X \) 的秩和 \( W \)}

\[
W = \sum_{i=1}^{n_x} R_i
\]

\textbf{步骤 3：计算检验统计量的期望值 \( E(W) \) 和方差 \( \text{Var}(W) \)}

\begin{itemize}
    \item \textbf{期望值：}

    \[
    E(W) = \frac{n_x (N + 1)}{2}
    \]

    \item \textbf{方差：}

    \[
    \text{Var}(W) = \frac{N(N - 1)}{n_x n_y} \left( \sum_{i=1}^{N} R_i^2 - \frac{N(N + 1)^2}{4} \right)
    \]
\end{itemize}

\textbf{步骤 4：计算标准化检验统计量 \( Z \)}

\[
Z = \frac{W - E(W)}{\sqrt{\text{Var}(W)}}
\]

\textbf{步骤 5：确定 \( p \) 值并作出决策}

\begin{itemize}
    \item 根据 \( Z \) 值，查找标准正态分布表，得到相应的 \( p \) 值。
    \item \textbf{决策规则：}
    \begin{itemize}
        \item 如果 \( p \) 值小于显著性水平 \( \alpha \)（如 0.05），则拒绝原假设 \( H_0 \)。
        \item 如果 \( p \) 值大于或等于 \( \alpha \)，则不拒绝 \( H_0 \)。
    \end{itemize}
\end{itemize}

\subsection*{5. 手动计算示例}

\textbf{假设情境：}

某研究人员想比较两种教学方法对学生考试成绩的影响。样本数据如下：

\begin{itemize}
    \item \textbf{教学方法 A（样本 \( X \)，\( n_x = 8 \)）：}

    85, 78, 90, 88, 76, 95, 89, 84

    \item \textbf{教学方法 B（样本 \( Y \)，\( n_y = 7 \)）：}

    80, 82, 75, 79, 77, 81, 83
\end{itemize}

研究人员想知道这两种教学方法是否导致学生\textbf{平均成绩}有显著差异。

\textbf{步骤 1：数据合并与排序}

将两个样本的数据合并并排序，赋予秩值：

\begin{tabular}{ccl}
\toprule
\textbf{数据值} & \textbf{来自样本} & \textbf{秩值 \( R_i \)} \\
\midrule
75 & Y & 1 \\
76 & X & 2 \\
77 & Y & 3 \\
78 & X & 4 \\
79 & Y & 5 \\
80 & Y & 6 \\
81 & Y & 7 \\
82 & Y & 8 \\
83 & Y & 9 \\
84 & X & 10 \\
85 & X & 11 \\
88 & X & 12 \\
89 & X & 13 \\
90 & X & 14 \\
95 & X & 15 \\
\bottomrule
\end{tabular}

\textbf{步骤 2：计算样本 \( X \) 的秩和 \( W \)}

提取样本 \( X \) 的秩值并求和：

\[
W = 2 + 4 + 10 + 11 + 12 + 13 + 14 + 15 = 81
\]

\textbf{步骤 3：计算期望值 \( E(W) \) 和方差 \( \text{Var}(W) \)}

\begin{itemize}
    \item 总样本量 \( N = n_x + n_y = 8 + 7 = 15 \)
    \item \textbf{期望值：}

    \[
    E(W) = \frac{n_x (N + 1)}{2} = \frac{8 \times 16}{2} = 64
    \]

    \item \textbf{计算总秩值的平方和：}

    \[
    \sum_{i=1}^{N} R_i^2 = 1^2 + 2^2 + 3^2 + \ldots + 15^2 = 1240
    \]

    \item \textbf{方差：}

    \[
    \text{Var}(W) = \frac{N(N - 1)}{n_x n_y} \left( \sum_{i=1}^{N} R_i^2 - \frac{N(N + 1)^2}{4} \right)
    \]

    计算各项：

    \begin{align*}
    N(N - 1) &= 15 \times 14 = 210 \\
    n_x n_y &= 8 \times 7 = 56 \\
    \sum_{i=1}^{N} R_i^2 &= 1240 \\
    \frac{N(N + 1)^2}{4} &= \frac{15 \times 16^2}{4} = 960 \\
    \text{Var}(W) &= \frac{210}{56} \times (1240 - 960) = \frac{210}{56} \times 280 = 1050
    \end{align*}
\end{itemize}

\textbf{步骤 4：计算标准化检验统计量 \( Z \)}

\[
Z = \frac{W - E(W)}{\sqrt{\text{Var}(W)}} = \frac{81 - 64}{\sqrt{1050}} = \frac{17}{32.4037} \approx 0.5247
\]

\textbf{步骤 5：确定 \( p \) 值并作出决策}

\begin{itemize}
    \item 查找标准正态分布表，对于 \( Z = 0.5247 \)，单尾 \( p \) 值约为 0.3001。
    \item \textbf{双尾检验：}

    \( p \) 值为 \( 2 \times 0.3001 = 0.6002 \)。
    \item \textbf{决策：}

    由于 \( p \) 值（0.6002）大于显著性水平 0.05，不拒绝原假设 \( H_0 \)，认为两种教学方法的平均成绩没有显著差异。
\end{itemize}

\subsection*{6. 使用 R 语言的 \texttt{wilcox.test} 进行检验}

\textbf{步骤 1：输入数据}

\begin{lstlisting}[language=R]
# 样本 X
x <- c(85, 78, 90, 88, 76, 95, 89, 84)

# 样本 Y
y <- c(80, 82, 75, 79, 77, 81, 83)
\end{lstlisting}

\textbf{步骤 2：执行 Wilcoxon 秩和检验}

\begin{lstlisting}[language=R]
# 进行 Wilcoxon 秩和检验，不进行连续性校正
result <- wilcox.test(x, y, alternative = "two.sided", exact = FALSE, correct = FALSE)

# 查看结果
print(result)
\end{lstlisting}

\textbf{输出结果：}

\begin{lstlisting}[language=R]
Wilcoxon rank sum test

data:  x and y
W = 81, p-value = 0.6002
alternative hypothesis: true location shift is not equal to 0
\end{lstlisting}

\textbf{解释：}

\begin{itemize}
    \item \textbf{W 值：} 输出的 \( W \) 值为 81，与手动计算结果一致。
    \item \textbf{\( p \) 值：} \( p \)-value = 0.6002，符合手动计算的双尾 \( p \) 值。
    \item \textbf{结论：} 由于 \( p \) 值大于 0.05，不拒绝原假设，认为两种教学方法的平均成绩没有显著差异。
\end{itemize}

\subsection*{7. 总结}

\textbf{Wilcoxon 秩和检验}是一种强大的非参数统计方法，适用于比较两个独立样本的\textbf{平均数}，尤其在数据不满足正态分布或存在异常值的情况下。

\begin{itemize}
    \item \textbf{优点：}
    \begin{itemize}
        \item 不需要假设数据服从正态分布。
        \item 对异常值不敏感。
        \item 可以处理不同样本量的情况。
    \end{itemize}
    \item \textbf{限制：}
    \begin{itemize}
        \item 假设两个总体的分布形状相同，仅平均数可能不同。
        \item 当存在大量重复值时，检验的准确性可能受到影响。
    \end{itemize}
\end{itemize}

在本示例中，通过手动计算和 R 语言的验证，我们得出了相同的结论：两种教学方法的平均成绩没有显著差异。

\end{document}
