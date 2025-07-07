- Ngoài `itemize` và `enumerate` còn có `description
- `\item` cũng có thể đôi label được, ví dụ: `\item[!]`, `\item[$\blacksquare$]`, `\item[NOTE]`, `\item[]`.
- list có thể nest vào với nhau, tối đa là 4 level.
- Hoàn toàn có thể custom lại các label ở những level trong list, ví dụ:
```latex
\renewcommand{\labelenumii}{\arabic{enumi}.\arabic{enumii}}
\renewcommand{\labelenumiii}{\arabic{enumi}.\arabic{enumii}.\arabic{enumiii}}
\renewcommand{\labelenumiv}{\arabic{enumi}.\arabic{enumii}.\arabic{enumiii}.\arabic{enumiv}}

% tương tự cho list
\renewcommand{\labelenumii}{...}
\renewcommand{\labelenumiii}{...}
\renewcommand{\labelenumiv}{...}
```
Trong đó: 

| Level   | `enumerate` label commands | `itemize` label commands |
| ------- | -------------------------- | ------------------------ |
| Level 1 | `\labelenumi`              | `\labelitemi`            |
| Level 2 | `\labelenumii`             | `\labelitemii`           |
| Level 3 | `\labelenumiii`            | `\labelitemiii`          |
| Level 4 | `\labelenumiv`             | `\labelitemiv`           |
Tức là chỉ cần `\renewcommand` thì khi có nested list ở các level sâu hơn thì label sẽ tự động được đổi

Có thể custom thêm về các thông số khác, chẳng hạn:

```latex
\documentclass{article}
\usepackage{layouts}
\begin{document}
\begin{figure}
\listdiagram
\caption{The \LaTeX{} parameters which define typesetting and layout of lists.} 
\end{figure}
\end{document}
```

![[LaTeX_list_parameters-plain.svg]]
À trước tiên thì:
```latex
\begin{list}{label}{settings}
```
- `{label}`: là định dạng của nhãn (label) cho mỗi `\item`
- `{settings}`: là **toàn bộ khối cấu hình** như `\usecounter`, `\setlength`, `\let`, v.v.

```latex
\documentclass{article}
\begin{document}
\newcounter{boxlblcounter}  
\newcommand{\makeboxlabel}[1]{\fbox{#1.}\hfill}% \hfill fills the label box
\newenvironment{boxlabel}
  {\begin{list}
    {\arabic{boxlblcounter}}
    {\usecounter{boxlblcounter}
     \setlength{\labelwidth}{3em}
     \setlength{\labelsep}{0em}
     \setlength{\itemsep}{2pt}
     \setlength{\leftmargin}{1.5cm}
     \setlength{\rightmargin}{2cm}
     \setlength{\itemindent}{0em} 
     \let\makelabel=\makeboxlabel
    }
  }
{\end{list}}

\newcommand{\randomtext}{Hello, here is some text without a meaning. Hello, here is some text without a meaning. Hello, here is some text without a meaning.}

\noindent\randomtext

\begin{boxlabel}
\item \randomtext
\item \randomtext
\item \randomtext
\end{boxlabel}
\end{document}
```

Dòng `\newcommand{\makeboxlabel}[1]{\fbox{#1.}\hfill}` có nghĩa là:

| Thành phần         | Giải nghĩa                                      |
| ------------------ | ----------------------------------------------- |
| `\newcommand{...}` | Định nghĩa lệnh mới (macro)                     |
| `\makeboxlabel`    | Tên macro do bạn đặt (tùy chọn)                 |
| `[1]`              | Macro này nhận **1 đối số** (số 1 trong `[1]`)  |
| `{...}`            | Nội dung thực hiện khi gọi `\makeboxlabel{abc}` |
| #1                 | Đối số, như kiểu truyền biến á                  |

--- 

Có thể dùng gói `\enumitem` để chỉnh sửa định dạng của list, ví dụ:
```latex
\documentclass{article}
\usepackage{enumitem}
\begin{document}

\newcommand{\randomtext}{Hello, here is some text without a meaning. Hello, here is some text without a meaning.}

\newcommand{\shortrandomtext}{Hello, here is some text.}
% Create a custom list based on enumerate
% It is called "myitems"
% We'll create a list that is 3 levels deep
\newlist{myitems}{enumerate}{3}

% Configure the behaviour of level 1 entries
% NOTE: we use the list counter "myitemsi"
\setlist[myitems, 1]
{label=\arabic{myitemsi}., %1., 2., 3., ...
leftmargin=\parindent,
rightmargin=10pt
}

% Configure the behaviour of level 2 entries
% NOTE: we use the list counter "myitemsii"
\setlist[myitems, 2]
{label=\arabic{myitemsi}.\arabic{myitemsii}, %1.1, 1.2, 1.3...
leftmargin=15pt,
rightmargin=15pt}

% Configure the behaviour of level 3 entries
% NOTE: we use the list counter "myitemsiii"
\setlist[myitems, 3]
% Use a label of 1.1:<kern>(a), 1.1:<kern>(b) etc  
{label=\arabic{myitemsi}.\arabic{myitemsii}:\kern1.5pt(\alph{myitemsiii}),
leftmargin=30pt,
rightmargin=30pt}

\randomtext
\begin{myitems}
\item \randomtext
    \begin{myitems}
    \item \randomtext
        \begin{myitems}
        \item \randomtext
        \item \randomtext
        \end{myitems}
    \item \shortrandomtext
    \item \shortrandomtext
    \end{myitems}
\item \randomtext
\end{myitems}
\end{document}
```

Trong đó:
```latex
\setlist[myitems, 3]
{label=\arabic{myitemsi}.\arabic{myitemsii}:\kern1.5pt(\alph{myitemsiii}),
leftmargin=30pt,
rightmargin=30pt}
```
là chỉnh sửa list `myitems`, ở level thứ `3`.

