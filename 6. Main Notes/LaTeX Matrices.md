
| Type                             | $\LaTeX$ markup                                             | Renders as                                                |
| -------------------------------- | ----------------------------------------------------------- | --------------------------------------------------------- |
| Plain                            | `\begin{matrix}   1 & 2 & 3\\   a & b & c   \end{matrix}`   | $\begin{matrix}   1 & 2 & 3\\   a & b & c   \end{matrix}$ |
| Parentheses;  <br>round brackets | `\begin{pmatrix}   1 & 2 & 3\\   a & b & c   \end{pmatrix}` | $\begin{pmatrix} 1 & 2 & 3\\  a & b & c  \end{pmatrix}$   |
| Brackets;  <br>square brackets   | `\begin{bmatrix}   1 & 2 & 3\\   a & b & c   \end{bmatrix}` | $\begin{bmatrix}  1 & 2 & 3\\  a & b & c  \end{bmatrix}$  |
| Braces;  <br>curly brackets      | `\begin{Bmatrix}   1 & 2 & 3\\   a & b & c   \end{Bmatrix}` | $\begin{Bmatrix} 1 & 2 & 3\\  a & b & c  \end{Bmatrix}$   |
| Pipes                            | `\begin{vmatrix}   1 & 2 & 3\\   a & b & c   \end{vmatrix}` | $\begin{vmatrix} 1 & 2 & 3\\  a & b & c  \end{vmatrix}$   |
| Double pipes                     | `\begin{Vmatrix}   1 & 2 & 3\\   a & b & c   \end{Vmatrix}` | $\begin{Vmatrix} 1 & 2 & 3\\  a & b & c  \end{Vmatrix}$   |

Nếu muốn định nghĩa ma trận gì khác thì cứ tạo ngoặc rồi gói nó vào `matrix` là được:

| $\LaTeX$ markup                                                                          | Renders as                                                                         |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `\left\lceil   \begin{matrix}   1 & 2 & 3\\   a & b & c   \end{matrix}   \right\rceil`   | $\left\lceil  \begin{matrix} 1 & 2 & 3\\  a & b & c  \end{matrix}  \right\rceil$   |
| `\left\langle   \begin{matrix}   1 & 2 & 3\\   a & b & c   \end{matrix}   \right\rvert`  | $\left\langle  \begin{matrix}  1 & 2 & 3\\  a & b & c  \end{matrix}  \right\rvert$ |
| `\left\langle   \begin{matrix}   1 & 2 & 3\\   a & b & c   \end{matrix}   \right\rangle` | $\left\langle  \begin{matrix} 1 & 2 & 3\\  a & b & c  \end{matrix}  \right\rangle$ |

Ngoài ra có thể dùng `inline` matrix và `smallmatrix`
```latex
\documentclass{article}
\usepackage{amsmath}
\begin{document}
\noindent Trying to typeset an inline matrix here:
 $\begin{pmatrix}
  a & b\\ 
  c & d
\end{pmatrix}$,  
but it looks too big, so let's try 
$\big(\begin{smallmatrix}
  a & b\\
  c & d
\end{smallmatrix}\big)$ 
instead.
\end{document}
```

