"Triển khai Taylor của hàm trên là ..."
"Áp dụng triển khai Taylor ..."
"Xét triển khai Taylor ..."

Hồi xưa mình đọc tài liệu rất rất nhiều lần mình gặp những dòng trên. Không một lời giải thích chuỗi Taylor là gì cũng như công dụng của nó, nhưng mà gặp nhiều thì thành quen, riết rồi mình cũng tìm hiểu nó là gì.

Nói ngắn gọn, Taylor series là một cách xấp xỉ một hàm bất kì (miễn là đạo hàm được) bằng một đa thức. Xấp xỉ này tốt ở local, tức là quanh một điểm $a$ cho trước.

---

## Xây dựng chuỗi Taylor

Cho bạn một hàm $f(x)$ và có đạo hàm tới bậc $n$, giả sử rằng tồn tại một đa thức cho ta một xấp xỉ "đủ tốt", tạm gọi là $g(x)$. Giờ bảo bạn tìm $g(x)$, đố bạn biết tìm làm sao?

Để lấy ví dụ cho cụ thể hơn, ta lấy hàm $f(x)$ như sau:
$$
f(x) = \frac{1}{2}x^3 + 2x^2 + 5x - 1
$$
Giả sử ta muốn xấp xỉ $f(x)$ lanh quanh tại $0$, ta có thể thay $x$