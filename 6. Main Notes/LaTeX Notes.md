## Miscellaneous 
- `\thanks{}` để thêm foot note
- `\verb||`: hiển thị lệnh thay vì thực thi

- Latex có hỗ trợ:
	- `\part{part}`
	- `\chapter{chapter}`
	- `\section{section}`
	- `\subsection{subsection}`
	- `\subsubsection{subsubsection}`
	- `\paragraph{paragraph}`
	- `\subparagraph{subparagraph}`
	Lưu ý part và chapter chỉ có trong book hoặc report

- `verbatim`: như `\verb`, nhưng là environment nên phạm vi rộng hơn.
- `\begingroup`: thiết lập lệnh hoạt động trong phạm vi cục bộ

---

## Compile
- latex có hỗ trợ compile ra: DVI, PS, PDF


---

## Formating
- `\emph`
- `\par` thay cho hai space xuống dòng
- -`\raggedright`, an alternative to using the `flushleft` environment
- `\raggedleft`, an alternative to using the `flushright` environment
- `\centering`, an alternative to using the `center` environment
- không dùng environment thì phải có `\begingroup` và `\endgroup`
- `\setlength{\parindent}{20pt}`
- `\noindent`

---

## List
- Ngoài `itemize` và `enumerate` còn có `description
- `\item` cũng có thể đôi label được, ví dụ: `\item[!]`, `\item[$\blacksquare$]`, `\item[NOTE]`, `\item[]`.
- list có thể nest vào với nhau, tối đa là 4 level.
- Hoàn toàn có thể custom lại các label ở những level trong list, ví dụ:
```latex
\renewcommand{\labelenumii}{\arabic{enumi}.\arabic{enumii}}
\renewcommand{\labelenumiii}{\arabic{enumi}.\arabic{enumii}.\arabic{enumiii}}
\renewcommand{\labelenumiv}{\arabic{enumi}.\arabic{enumii}.\arabic{enumiii}.\arabic{enumiv}}
```
Trong đó: 

| Level   | `enumerate` label commands | `itemize` label commands |
| ------- | -------------------------- | ------------------------ |
| Level 1 | `\labelenumi`              | `\labelitemi`            |
| Level 2 | `\labelenumii`             | `\labelitemii`           |
| Level 3 | `\labelenumiii`            | `\labelitemiii`          |
| Level 4 | `\labelenumiv`             | `\labelitemiv`           |
Có thể custom thêm về 




