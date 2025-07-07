
## Tables with a fixed width
- `\usepackage{array}`
```latex
\begin{center}
\begin{tabular}{ | m{5em} | m{1cm}| m{1cm} | } 
  \hline
  cell1 dummy text dummy text dummy text& cell2 & cell3 \\ 
  \hline
  cell1 dummy text dummy text dummy text & cell5 & cell6 \\ 
  \hline
  cell7 & cell8 & cell9 \\ 
  \hline
\end{tabular}
\end{center}
```
- `m` ở đây là middle

Nếu muốn control size của toàn bộ bảng, dùng gói ``tabularx``:

```latex
\begin{tabularx}{0.8\textwidth} { 
  | >{\raggedright\arraybackslash}X 
  | >{\centering\arraybackslash}X 
  | >{\raggedleft\arraybackslash}X | }
 \hline
 item 11 & item 12 & item 13 \\
 \hline
 item 21  & item 22  & item 23  \\
\hline
\end{tabularx}
```
- Cú pháp `>{...}X`: trước tiên là định nghĩa cột kiểu `X`, tức là giãn tự động dựa trên lệnh được chỉ định từ trước. `>{...}` là specify lệnh cần làm. `\arraybackslash` cho phép dùng `\\` để xuống dòng.

---
## Cell nhiều hàng hoặc nhiều cột
- Dùng `\multicolumn` và `\multirow`
- Để dùng được thì gói là `multirow` nhé=)))))))
Ví dụ:

```latex
\documentclass{article}
\usepackage{multirow}
\begin{document}
	\begin{tabular}{ |p{3cm}||p{3cm}|p{3cm}|p{3cm}|  }
	 \hline
	 \multicolumn{4}{|c|}{Country List} \\
	 \hline
	 Country Name or Area Name& ISO ALPHA 2 Code &ISO ALPHA 3 Code&ISO numeric Code\\
	 \hline
	 Afghanistan   & AF    &AFG&   004\\
	 Aland Islands&   AX  & ALA   &248\\
	 Albania &AL & ALB&  008\\
	 Algeria    &DZ & DZA&  012\\
	 American Samoa&   AS  & ASM&016\\
	 Andorra& AD  & AND   &020\\
	 Angola& AO  & AGO&024\\
	 \hline
	\end{tabular}

	\begin{center}
	\begin{tabular}{ |c|c|c| } 
	\hline
	col1 & col2 & col3 \\
	\hline
	\multirow{3}{4em}{Multiple row} & cell2 & cell3 \\ 
	& cell5 & cell6 \\ 
	& cell8 & cell9 \\ 
	\hline
	\end{tabular}
	\end{center}

\end{document}
```



==Xoay bảng==:
- Dùng gói `rotating` và bọc trong môi trường `sideways`

---

## Multi-page tables
- Dùng gói `longtable`, ví dụ:
```latex
\documentclass{article}
\usepackage{longtable}

\begin{document}
 
 \begin{longtable}[c]{| c | c |}
 \caption{Long table caption.\label{long}}\\

 \hline
 \multicolumn{2}{| c |}{Begin of Table}\\
 \hline
 Something & something else\\
 \hline
 \endfirsthead

 \hline
 \multicolumn{2}{|c|}{Continuation of Table \ref{long}}\\
 \hline
 Something & something else\\
 \hline
 \endhead

 \hline
 \endfoot

 \hline
 \multicolumn{2}{| c |}{End of Table}\\
 \hline\hline
 \endlastfoot

 Lots of lines & like this\\
 Lots of lines & like this\\
 Lots of lines & like this\\
 Lots of lines & like this\\
 Lots of lines & like this\\
 Lots of lines & like this\\
 Lots of lines & like this\\
 Lots of lines & like this\\
 ...
 Lots of lines & like this\\
 \end{longtable}
```

| Lệnh            | Xuất hiện ở đâu?                                                 | Dùng để làm gì?                             |
| --------------- | ---------------------------------------------------------------- | ------------------------------------------- |
| `\endfirsthead` | Kết thúc phần đầu tiên trên **trang đầu**                        | Hiển thị header trên **trang đầu tiên**     |
| `\endhead`      | Kết thúc phần đầu lặp lại cho các trang tiếp theo                | Header sẽ xuất hiện ở **trang 2 trở đi**    |
| `\endfoot`      | Kết thúc phần chân bảng lặp lại cho **mọi trang trừ trang cuối** | Có thể để dòng gạch ngang, v.v.             |
| `\endlastfoot`  | Phần chân bảng chỉ hiển thị ở **trang cuối cùng**                | Tổng kết, hoặc chỉ đơn giản là một dòng kết |
