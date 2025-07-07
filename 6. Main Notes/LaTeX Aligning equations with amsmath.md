## split
$$
\begin{equation}
\begin{split}
A & = \frac{\pi r^2}{2}\\
 & = \frac{1}{2} \pi r^2
\end{split}
\end{equation}
$$

Cái này khác `align` ở chỗ tất cả các dòng đều được đánh 1 số, tuy nhiên lại được tách dòng ra. `split` phải được bọc bên trong `equation`

---
## multiline
Ngoài ra còn có thể có một công thức multi line:
$$
\begin{multline*}
p(x) = 3x^6 + 14x^5y + 590x^4y^2 + 19x^3y^3\\ 
- 12x^2y^4 - 12xy^5 + 2y^6 - a^3b^3
\end{multline*}
$$
---

## align
$$
\begin{align*}
x&=y           &  w &=z              &  a&=b+c\\
2x&=-y         &  3w&=\frac{1}{2}z   &  a&=b\\
-4 + 5x&=2+y   &  w+2&=-1+w          &  ab&=cb
\end{align*}
$$

---
## gather

Dùng khi muốn căn giữa tất cả các phương trình.
$$
\begin{gather*} 
2x - 5y =  8 \\ 
3x^2 + 9y =  3a + c
\end{gather*}
$$
