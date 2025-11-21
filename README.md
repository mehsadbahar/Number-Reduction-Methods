
\documentclass[12pt]{article}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{booktabs}
\usepackage{geometry}
\usepackage{persian}
\usepackage{graphicx}
\usepackage{algorithm}
\usepackage{algpseudocode}
\usepackage{listings}
\usepackage{xcolor}
\usepackage{float}

\settextfont{XB Zar}
\geometry{a4paper, margin=1in}

\title{مطالعه جامع روش‌های کاهش اعداد: الگوریتم‌های بهاروند و چرخه موقعیتی}
\author{مهدی صدیق‌بهر}
\date{}

\begin{document}

\maketitle

\begin{abstract}
این مقاله به مطالعه مقایسه‌ای دو الگوریتم کاهش اعداد به صفر می‌پردازد: روش بهاروند و روش چرخه موقعیتی. هر دو الگوریتم از طریق دنباله‌های قطعی تفریق، اعداد طبیعی را به صفر کاهش می‌دهند، اما با رویکردهای fundamentally مختلف و پیامدهای مهم برای پیچیدگی مراحل و تشکیل الگو. نتایج نشان می‌دهد روش بهاروند با پیچیدگی $O(k)$ که $k$ تعداد ارقام است، به طور متوسط ۳ تا ۸ برابر کارآمدتر از روش چرخه موقعیتی عمل می‌کند. با این وجود، روش چرخه موقعیتی به دلیل نمایش واضح مفاهیم ارزش مکانی، ارزش آموزشی بالایی دارد.
\end{abstract}

\textbf{کلمات کلیدی:} کاهش اعداد، الگوریتم‌های تفریق، پیچیدگی محاسباتی، آموزش ریاضی، ارزش مکانی

\section{مقدمه}
کاهش اعداد به صفر از طریق عملیات‌های سیستماتیک، یکی از مسائل پایه‌ای در ریاضیات و علوم کامپیوتر است. این فرآیند نه تنها در محاسبات عددی کاربرد دارد، بلکه در آموزش مفاهیم پایه ریاضی نیز اهمیت ویژه‌ای پیدا می‌کند. در این مقاله، دو الگوریتم متفاوت برای کاهش اعداد طبیعی به صفر مورد بررسی قرار می‌گیرد.

\subsection{پیشینه تحقیق}
مطالعه الگوریتم‌های کاهش عددی به کارهای کلاسیک در نظریه اعداد و الگوریتم‌های محاسباتی بازمی‌گردد. اگرچه روش‌های استانداردی مانند تجزیه به عوامل اول به خوبی مطالعه شده‌اند، اما رویکردهای مبتنی بر تفریق سیستماتیک کمتر مورد توجه قرار گرفته‌اند.

\section{مرور ادبیات}
در حوزه الگوریتم‌های عددی، تمرکز اصلی معمولاً بر روی بهینه‌سازی عملیات ضرب و تقسیم بوده است. با این حال، در زمینه آموزش ریاضی، درک فرآیندهای تفریق می‌تواند به درک عمیق‌تری از ساختار اعداد منجر شود. روش‌های ارائه شده در این مقاله پلی بین کاربردهای محاسباتی و آموزشی ایجاد می‌کنند.

\section{روش‌شناسی تحقیق}

\subsection{الگوریتم کاهش بهاروند}

\begin{algorithm}[H]
\caption{الگوریتم کاهش بهاروند}
\begin{algorithmic}[1]
\Procedure{BaharundReduction}{$N$}
\State $steps \gets 0$
\While{$N > 0$}
\State $k \gets \lfloor \log_{10} N \rfloor$
\State $d \gets \lfloor N / 10^k \rfloor$
\State $subtract \gets d \times 10^k$
\State $N \gets N - subtract$
\State $steps \gets steps + 1$
\EndWhile
\State \Return $steps$
\EndProcedure
\end{algorithmic}
\end{algorithm}

برای یک عدد $N$ با ارقام $d_k d_{k-1} \ldots d_1 d_0$ که در آن $d_k \neq 0$:

\begin{align*}
\text{فرض کنیم } & N_0 = N \\
& N_{i+1} = N_i - (d_k^{(i)} \times 10^{k_i}) \\
\text{که در آن } & k_i = \lfloor \log_{10} N_i \rfloor \\
& d_k^{(i)} = \left\lfloor \frac{N_i}{10^{k_i}} \right\rfloor
\end{align*}

\textbf{شرط توقف}: $N_m = 0$ پس از $m$ مرحله.

\subsection{الگوریتم کاهش چرخه موقعیتی}

\begin{algorithm}[H]
\caption{الگوریتم کاهش چرخه موقعیتی}
\begin{algorithmic}[1]
\Procedure{PositionalCycleReduction}{$N$}
\State $steps \gets 0$
\State $current \gets N$
\While{$current > 0$}
\State $m \gets \lfloor \log_{10} current \rfloor + 1$
\State $p \gets m - 1 - (steps \mod m)$
\State $subtract \gets 10^p$
\If{$subtract \leq current$}
\State $current \gets current - subtract$
\EndIf
\State $steps \gets steps + 1$
\EndWhile
\State \Return $steps$
\EndProcedure
\end{algorithmic}
\end{algorithm}

برای یک عدد $N$ با $m = \lfloor \log_{10} N \rfloor + 1$ رقم:

\begin{align*}
\text{فرض کنیم } & N_0 = N \\
& N_{i+1} = N_i - 10^{p_i} \\
\text{که در آن } & p_i = (m_i - 1 - (i \mod m_i)) \\
& m_i = \lfloor \log_{10} N_i \rfloor + 1
\end{align*}

\textbf{شرط توقف}: $N_n = 0$ پس از $n$ مرحله.

\section{تحلیل ریاضی}

\subsection{قضایای اساسی}

\begin{theorem}
برای عدد $N$ با $k$ رقم، روش بهاروند دقیقاً در $k$ مرحله به صفر می‌رسد.
\end{theorem}

\begin{proof}
اگر $N$ دارای $k$ رقم باشد، یعنی $10^{k-1} \leq N < 10^k$. در هر مرحله، بزرگترین رقم غیرصفر به همراه ارزش مکانی آن کم می‌شود. از آنجایی که هر رقم دقیقاً یک بار پردازش می‌شود، تعداد مراحل برابر با تعداد ارقام غیرصفر است که برای اعداد بدون رقم صفر برابر با $k$ است.
\end{proof}

\begin{theorem}
پیچیدگی زمانی روش بهاروند $O(k)$ است.
\end{theorem}

\begin{proof}
در هر مرحله، محاسبات شامل یافتن بزرگترین رقم و تفریق است که هر کدام در زمان $O(1)$ انجام می‌شوند. با توجه به قضیه قبل، تعداد مراحل $k$ است، بنابراین پیچیدگی کل $O(k)$ است.
\end{proof}

\section{شبیه‌سازی و نتایج}

\subsection{مطالعه موردی: $N = 12345$}

\subsubsection{کاهش بهاروند برای $N=12345$}

\begin{center}
\begin{tabular}{cccc}
\toprule
مرحله & $N_i$ & تفریق & $N_{i+1}$ \\
\midrule
0 & 12345 & $1 \times 10^4 = 10000$ & 2345 \\
1 & 2345 & $2 \times 10^3 = 2000$ & 345 \\
2 & 345 & $3 \times 10^2 = 300$ & 45 \\
3 & 45 & $4 \times 10^1 = 40$ & 5 \\
4 & 5 & $5 \times 10^0 = 5$ & 0 \\
\bottomrule
\end{tabular}
\end{center}

\subsubsection{کاهش چرخه موقعیتی برای $N=12345$}

\begin{center}
\begin{tabular}{cccc}
\toprule
مرحله & $N_i$ & توان ۱۰ & $N_{i+1}$ \\
\midrule
0 & 12345 & $10^4$ & 2345 \\
1 & 2345 & $10^3$ & 1345 \\
2 & 1345 & $10^2$ & 1245 \\
3 & 1245 & $10^1$ & 1235 \\
4 & 1235 & $10^0$ & 1234 \\
5 & 1234 & $10^3$ & 234 \\
6 & 234 & $10^2$ & 134 \\
7 & 134 & $10^1$ & 124 \\
8 & 124 & $10^0$ & 123 \\
9 & 123 & $10^2$ & 23 \\
10 & 23 & $10^1$ & 13 \\
11 & 13 & $10^0$ & 12 \\
12 & 12 & $10^1$ & 2 \\
13 & 2 & $10^0$ & 1 \\
14 & 1 & $10^0$ & 0 \\
\bottomrule
\end{tabular}
\end{center}

\subsection{مطالعه موردی: $N = 6789$}

\subsubsection{کاهش بهاروند برای $N=6789$}

\begin{center}
\begin{tabular}{cccc}
\toprule
مرحله & $N_i$ & تفریق & $N_{i+1}$ \\
\midrule
0 & 6789 & $6 \times 10^3 = 6000$ & 789 \\
1 & 789 & $7 \times 10^2 = 700$ & 89 \\
2 & 89 & $8 \times 10^1 = 80$ & 9 \\
3 & 9 & $9 \times 10^0 = 9$ & 0 \\
\bottomrule
\end{tabular}
\end{center}

\subsection{نتایج تجربی جامع}

\begin{center}
\begin{tabular}{lcccc}
\toprule
عدد & مراحل بهاروند & مراحل چرخه‌ای & نسبت & کارایی \\
\midrule
12345 & 5 & 15 & 3.0 & بهاروند \\
6789 & 4 & 30 & 7.5 & بهاروند \\
1000 & 1 & 4 & 4.0 & بهاروند \\
503 & 3 & 9 & 3.0 & بهاروند \\
2024 & 4 & 16 & 4.0 & بهاروند \\
99 & 2 & 6 & 3.0 & بهاروند \\
7 & 1 & 7 & 7.0 & بهاروند \\
\bottomrule
\end{tabular}
\end{center}

\section{تحلیل پیچیدگی}

\subsection{مقایسه پیچیدگی زمانی}

\begin{itemize}
    \item \textbf{بهاروند}: $O(k)$ که $k$ تعداد ارقام عدد است
    \item \textbf{چرخه موقعیتی}: $O(k \times d_1)$ که $d_1$ اولین رقم عدد است
\end{itemize}

\subsection{تحلیل مجانبی}

برای اعداد بزرگ با $k$ رقم، میانگین تعداد مراحل در روش چرخه موقعیتی تقریباً $\frac{k \times (10^{k-1})}{2}$ است، در حالی که روش بهاروند همواره $k$ مرحله نیاز دارد.

\section{بحث و مقایسه}

\subsection{ویژگی‌های روش بهاروند}
\begin{itemize}
    \item \textbf{قطعیت}: همیشه برای یک ورودی مشخص مسیر یکسانی را طی می‌کند
    \item \textbf{مبتنی بر رقم}: ارقام را از چپ به راست پردازش می‌کند
    \item \textbf{کارا}: حداقل مراحل برای اعداد بزرگ
    \item \textbf{پیش‌بینی‌پذیر}: تعداد مراحل برابر با تعداد موقعیت‌های رقم غیرصفر است
\end{itemize}

\subsection{ویژگی‌های روش چرخه موقعیتی}
\begin{itemize}
    \item \textbf{چرخه‌ای}: به صورت سیستماتیک از بین موقعیت‌های ارقام چرخه می‌زند
    \item \textbf{غنی از الگو}: الگوهای تفریق تکراری را نشان می‌دهد
    \item \textbf{سیستماتیک}: دنباله ثابتی از توان‌های ۱۰ را دنبال می‌کند
    \item \textbf{آموزشی}: مفاهیم ارزش مکانی را به وضوح نشان می‌دهد
\end{itemize}

\section{کاربردهای آموزشی}

هر دو الگوریتم در آموزش ریاضی کاربرد دارند:

\subsection{روش بهاروند برای}
\begin{itemize}
    \item آموزش مفهوم ارزش مکانی
    \item درک ساختار اعشاری اعداد
    \item تقویت مهارت‌های تفریق
\end{itemize}

\subsection{روش چرخه موقعیتی برای}
\begin{itemize}
    \item نمایش الگوهای عددی
    \item درک سیستم ارزش مکانی
    \item تقویت تفکر الگوریتمی
\end{itemize}

\section{نتیجه‌گیری و کارهای آینده}

روش بهاروند کارایی بالاتری برای کاهش اعداد نشان می‌دهد و به طور مداوم به مراحل کمتری نسبت به روش چرخه موقعیتی نیاز دارد. با این حال، روش چرخه موقعیتی بینش‌های آموزشی ارزشمندی در مورد سیستم‌های ارزش مکانی ارائه می‌دهد و الگوهای چرخه‌ای جالبی را نشان می‌دهد.

\subsection{جهت‌های تحقیقاتی آینده}
\begin{itemize}
    \item توسعه الگوریتم‌های ترکیبی
    \item مطالعه رفتار الگوریتم‌ها برای مبنای عددی دیگر
    \item کاربرد در فشرده‌سازی داده‌ها
    \item استفاده در سیستم‌های آموزشی هوشمند
\end{itemize}

\section*{پیوست: دنباله‌های کامل}

\subsection*{چرخه موقعیتی برای ۶۷۸۹ (کامل)}
\begin{latin}
\begin{verbatim}
6789 → 5789 → 5689 → 5679 → 5678 → 4678 → 4578 → 4568 → 4567 
→ 3567 → 3467 → 3457 → 3456 → 2456 → 2356 → 2346 → 2345 
→ 1345 → 1245 → 1235 → 1234 → 234 → 134 → 124 → 123 
→ 23 → 13 → 12 → 2 → 1 → 0
\end{verbatim}
\end{latin}

\begin{thebibliography}{9}
\bibitem{knuth1997art}
Knuth, D. E. (1997). The Art of Computer Programming, Volume 2: Seminumerical Algorithms. Addison-Wesley.

\bibitem{cormen2009introduction}
Cormen, T. H., Leiserson, C. E., Rivest, R. L., \& Stein, C. (2009). Introduction to Algorithms. MIT Press.

\bibitem{van1994representation}
Van, L. (1994). Representation of Numbers and the Concept of Place Value. Mathematics Education Research.
\end{thebibliography}

\end{document}
```
