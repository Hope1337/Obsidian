
## What is tensor?

> What is "tensor"?

Tensor is an object that is **invariant** under a change of coordinates, and has **components** that change in a **special**, **predictable** way under a change of coordinates.

Hãy xem xét ví dụ sau đây: một vector trong không gian 2D. Mặc định, khi biểu diễn vector dưới dạng một vector cột sau đây:
$$
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
$$
Ta mặc định rằng hệ tọa độ đang là hệ chuẩn:
$$
\begin{bmatrix}
0 \ 1 \\
1 \ 0
\end{bmatrix}
$$
Còn nhớ lúc học đại học, ta có công thức "chuyển hệ tọa độ" sau:
$$
u = Dv
$$
Trong đó vector $u$ là vector thuộc hệ tọa độ chuẩn, $D$ là basis mới và $v$ là vector $u$ trong góc nhìn của basis $D$. 

==Ngoài lề==: Cá nhân mình không thích gọi công thức trên là công thức chuyển hệ tọa độ lắm, bởi vì nhắc đến việc "chuyển", mình thường nghĩ đến việc transform từ cái cũ sang cái mới. Trong khi đó công thức trên lại mang nghĩa rằng ta đang convert $v$ về hệ chuẩn. Điều này cũng gây cho mình rất nhiều hiểu lầm khi dùng ma trận chuyển cơ sở $D$.

Vậy công thức này nghĩa là gì? Có phải nó "dịch chuyển" vector ban đầu $u$ của ta về thành một vector $v$ hay ngược lại? 

Theo mình đều không phải, vector $u$ hay vector $v$ chỉ là cách nhìn của hai hệ tọa độ vào một object thực sự, mình tạm gọi object này là $o$. Tưởng tượng bạn vẽ một hình tròn lên bảng. Sau đó bạn kẻ một lưới và bạn dùng tọa độ của lưới đó để xác định vị trí của hình tròn ban đầu. Vậy có thể gọi vector $u$ là "cách nhìn" của bạn về vị trí của $o$. Tương tự nếu một người khác vẽ một grid hoàn toàn khác, lúc này tọa độ của $o$ trong "cách nhìn" của họ lại là $v$. Rõ ràng $o$ vẫn nằm đó, nó vẫn như vậy chứ không di chuyển, thứ thay đổi duy nhất đó là tọa độ trong các hệ tọa độ khác nhau, vậy $o$ là **invariant** và các tọa độ $u$, $v$ chứa các components của $o$, tuy nó không invariant nhưng sự thay đổi là **predictable** (qua ma trận chuyển hệ tọa độ).

---
## Covectors

Covector là một hàm số nhận đầu vào là vector và trả ra một số thực thuộc trường số mà components của vector đầu vào định nghĩa trên.

Covectors phải đáp ứng được tính linearity, tức là:
$$
\alpha(n\vec{v} + m\vec{w}) = n\alpha(\vec{v}) + m\alpha(\vec{w})
$$
Vậy các covectors này tạo thành một vector space $(V^*, S, +, .)$. Lưu ý rằng đầu vào của các covectors là các vector, cũng tạo thành một vector space. Tức là ở đây có hai vector space riêng biệt, đừng nhầm lẫn!

---

==Important==:
- Vector là invariant
- Vector components không invariant
==Tương tự==:
- Covector là invariant
- Covector components không invariant

Vậy điều này nghĩa là gì?

Vector có thể hình dung như object