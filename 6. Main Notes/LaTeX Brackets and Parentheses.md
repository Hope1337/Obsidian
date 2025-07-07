| Type                        | LaTeX markup          | Renders as            |     |
| --------------------------- | --------------------- | --------------------- | --- |
| Parentheses; round brackets | `(x+y)`               | $(x+y)$               |     |
| Brackets; square brackets   | `[x+y]`               | $[x+y]$               |     |
| Braces; curly brackets      | `\{ x+y \}`           | ${x+y}$               |     |
| Angle brackets              | `\langle x+y \rangle` | $\langle x+y \rangle$ |     |
| Double pipes                | `\|x+y\|`             | $\|x+y\|$             |     |

`\left` và `\right` được dùng để tự động chỉnh cỡ ngoặc, phải gồm cả 2 chứ không được dùng cái này bỏ cái kia (bắt buộc phải đi theo cặp).

==Lưu ý==: Khi làm việc với môi trường nhiều dòng, bạn sẽ không được mở ngoặc ở dòng này rồi đóng lại ở dòng kia, ví dụ code bên dưới đây sẽ tạo ra lỗi:
$$
\begin{align*}
y  = 1 + & \left(  \frac{1}{x} + \frac{1}{x^2} + \frac{1}{x^3} + \ldots  \\
  & \quad  + \frac{1}{x^{n-1}} + \frac{1}{x^n} \right)
\end{align*}
$$Giải pháp là dùng "invisible" bracket, tức là thêm `&` vào trước `\left` hoặc `\right`:
$$
\begin{align*}
y  = 1 + & \left(  \frac{1}{x} + \frac{1}{x^2} + \frac{1}{x^3} + \ldots \right. \\
  &\left. \quad + \frac{1}{x^{n-1}} + \frac{1}{x^n} \right)
\end{align*}
$$

Có thể điều chỉnh thủ công kích thước của ngoặc

| LaTeX markup                                                |                                                             |
| ----------------------------------------------------------- | ----------------------------------------------------------- |
| `\bigl( \Bigl( \biggl( \Biggl(`                             | $\bigl( \Bigl( \biggl( \Biggl($                             |
| `\bigr] \Bigr] \biggr] \Biggr]`                             | $\bigr] \Bigr] \biggr] \Biggr]$                             |
| `\bigl\{ \Bigl\{ \biggl\{ \Biggl\{`                         | $\bigl\{ \Bigl\{ \biggl\{ \Biggl\{$                         |
| `\bigl \langle \Bigl \langle \biggl \langle \Biggl \langle` | $\bigl \langle \Bigl \langle \biggl \langle \Biggl \langle$ |
| `\bigr \rangle \Bigr \rangle \biggr \rangle \Biggr \rangle` | $\bigr \rangle \Bigr \rangle \biggr \rangle \Biggr \rangle$ |
| `\big\| \Big\| \bigg\| \Bigg\|`                             | $\big\| \Big\| \bigg\| \Bigg\|$                             |
| `\bigl \lceil \Bigl \lceil \biggl \lceil \Biggl \lceil`     | $\bigl \lceil \Bigl \lceil \biggl \lceil \Biggl \lceil$     |
| `\bigr \rceil \Bigr \rceil \biggr \rceil \Biggr \rceil`     | $\bigr \rceil \Bigr \rceil \biggr \rceil \Biggr \rceil$     |
| `\bigl \lfloor \Bigl \lfloor \biggl \lfloor \Biggl \lfloor` | $\bigl \lfloor \Bigl \lfloor \biggl \lfloor \Biggl \lfloor$ |
| `\bigr \rfloor \Bigr \rfloor \biggr \rfloor \Biggr \rfloor` | $\bigr \rfloor \Bigr \rfloor \biggr \rfloor \Biggr \rfloor$ |
