#### `\renewcommand`
Định nghĩa một lệnh mới (kiểu như macro)
```latex
\renewcommand{\tenlenh}[số lượng đối số]{hành vi}
```
- `\tenlenh`: tên lệnh muốn định nghĩa lại.
- `số lượng đối số`: (tùy chọn) số lượng tham số mà lệnh nhận (tối đa là 9).
- `hành vi`: nội dung hoặc hành vi mới của lệnh, có thể là một dãy liên tiếp các lệnh được định nghĩa từ trước stack lại với nhau.

==Ví dụ==: Định nghĩa lại lệnh `\emph` để in đậm (thay vì in nghiêng)
```latex
\renewcommand{\emph}[1]{\textbf{#1}}
```
- `#1` là đối số, kiểu như truyền biến vậy. Khi dùng `\emph{hello}` thì `hello` sẽ thay cho `#1`

---

#### `\newenvironment`
Định nghĩa một môi trường mới
```latex
\newenvironment{name}[num][default]{begin-code}{end-code}
```
- `name`: tên môi trường muốn tạo, tên môi trường không cần đặt dấu `\` ở phía trước.
- `[num]`: (tùy chọn) số lượng tham số mà môi trường nhận 
- `[default]`: (tùy chọn) giá trị mặc định của tham số đầu tiên nếu không cung cấp. **Lưu ý là latex chỉ hỗ trợ giá trị default cho tham số đầu tiên**.
- `begin-code`: mã sẽ thực thi khi `\begin{name}` được gọi
- `end-code`: mã sẽ thực thi khi `\end{name}` được gọi

Ngoài lề: nếu muốn môi trường nhiều tham số mặc định thì dùng gói `xparse`:
```latex
\usepackage{xparse}

\NewDocumentEnvironment{myenv}{O{default1} O{default2}} % O = optional argument with default
  {\textbf{Arg1: #1}\\\textit{Arg2: #2}\\}
  {}
```
==Ví dụ==:
```latex
\newenvironment{myquote}
  {\begin{quote}\itshape}
  {\end{quote}}

% sử dụng
\begin{myquote}
Đây là một câu trích dẫn có định dạng riêng.
\end{myquote}
```

