
---
## Part I: Tabular methods

Trong phần này tác giả trình bày một trường hợp đặc biệt khi không gian trạng thái của bài toán "đủ nhỏ" để có thể trình bày ra một bảng.

Nhắc lại, mục tiêu của một Agent RL là tìm policy tối ưu $\pi^*$ sao cho:
$$
\pi^* = \arg\max_{\pi}v_{\pi}(s), \space \forall s \in \mathcal{S}  
$$
- Nếu bạn thắc mắc thì *có tồn tại một $\pi^*$ sao cho $v_{\pi*}(s)\geq v(s)_{\pi}, \space \forall s$*   
- Hàm $v$ được gọi là *value function* ($V$ hoa được dùng cho trường hợp bảng) và hàm $q$ được gọi là *action value function* ($Q$ hoa được dùng cho trường hợp bảng). 

#### Công thức Incremental 

Gọi $Q_n$ là giá trị action value ước tính tại bước thứ $n$, tức là $n-1$ bước trước đã được lấy để ước tính, bước hiện tại $n$ là bước đa đang đứng, $n-1$ bước trước được lấy ra làm số liệu ước tính. Tóm lại ta có:
$$
\begin{align}
Q_n &= \frac{R_1 + R_2 + \dots + R_{n-1}}{n-1}\\
Q_{n+1} &= \frac{R_1 + R_2 + \dots + R_{n}}{n}\\
&= \frac{1}{n}(R_n + \sum_{i=1}^{n-1}R_i)\\
&= \frac{1}{n}(R_n + (n-1)\frac{1}{n-1}\sum_{i=1}^{n-1}R_i)\\
&= \frac{1}{n}(R_n + (n-1)Q_{n-1}) \\
&= Q_n + \frac{1}{n}[R_n - Q_n]
\end{align}
$$
Trong cài đặt, người ta sẽ thay $\frac{1}{n}$ thành $\alpha$, và công thức này sẽ được bắt gặp rất nhiều lần sau này: $Q[s] = Q[s] + \alpha(R + Q[s])$, điều này cũng được áp dụng với value function. Lưu ý thêm là công thức bên trên đang tập trung vào một action duy nhất để đơn giản hóa kí hiệu.

Khi step size $\alpha \in (0,1]$, $Q_{n+1}$ sẽ là trung bình cộng có trọng số của tất cả các $R_i$ trước đó.

$$
\begin{align*}
Q_{n+1} &= Q_n + \alpha \left[ R_n - Q_n \right] \\
        &= \alpha R_n + (1 - \alpha) Q_n \\
        &= \alpha R_n + (1 - \alpha) \left[ \alpha R_{n-1} + (1 - \alpha) Q_{n-1} \right] \\
        &= \alpha R_n + (1 - \alpha) \alpha R_{n-1} + (1 - \alpha)^2 Q_{n-1} \\
        &= \alpha R_n + (1 - \alpha) \alpha R_{n-1} + (1 - \alpha)^2 \alpha R_{n-2} + \cdots \\
        &\quad + (1 - \alpha)^{n-1} \alpha R_1 + (1 - \alpha)^n Q_1 \\
        &= (1 - \alpha)^n Q_1 + \sum_{i=1}^{n} \alpha (1 - \alpha)^{n - i} R_i.
\end{align*}

$$
==Chứng minh==


==Lưu ý==: Step size $\alpha$ mặc dù giống như learning rate $\eta$, có thể chọn theo kinh nghiệm, tuy nhiên về mặt lý thuyết để đảm bảo dãy hội tụ thì cần có hai điều kiện: 
$$
\begin{align}
\sum_{n=1}^{\infty} \alpha_n(a) &= \infty, \space(1)\\
\sum_{n=1}^{\infty}\alpha^2_n(a) &< \infty, \space(2)
\end{align}
$$  
$(1)$ đảm bảo step size đủ lớn để vượt qua bất kì điều kiện ban đầu nào, $(2)$ đảm bảo rằng step size đủ nhỏ để hội tụ, $\frac{1}{n}$ đáp ứng cả hai điều kiện trên. Tuy nhiên trong thực tế ít ai làm vậy lắm, người tune $\alpha$ như tune learning rate vậy.

#### Finite Markov Decision Process

