
## On-policy Prediction with  Approximation

Ta đến với các giải pháp xấp xỉ hàm. Đối với tabular method, nơi khi ta update một state, các state khác không bị ảnh hưởng nên do đó có thể học được chính xác giá trị của các hàm đó. Ở đây, với giả định rằng state space lớn hơn nhiều với weight, ta cần định nghĩa một độ đo lỗi dự đoán. Tuy nhiên không phải state nào cũng được quan tâm bằng nhau, ta định nghĩa $\mu (s)$ là phân phối độ quan tâm của ta đối với state $s$, ta có độ đo Value Error, viết là $\overline{\text{VE}}$ như

$$
\overline{\text{VE}}(\mathbf{w}) \doteq \sum_{s \in \mathcal{S}} \mu(s) \left[ v_\pi(s) - \hat{v}(s, \mathbf{w}) \right]^2.
$$
Tiếp đến ta chỉ xét trường hợp episodic.
Gọi $\eta(s)$ là số lần trung bình state $s$ được đến thăm (trung bình tính trên một episode), $h(s)$ là xác suất state $s$ là state ban đầu. Rõ ràng $\eta (s)$ phải thỏa mãn phương trình sau:
$$
\eta(s) = h(s) + \sum_{\bar{s}} \eta(\bar{s}) \sum_{a} \pi(a \mid \bar{s}) p(s \mid \bar{s}, a), \qquad \text{for all } s \in \mathcal{S}.
$$
Trong trường hợp có discounting, chỉ cần thêm $\gamma$ vào second term là xong. Giải hệ phương trình trên (giải kiểu gì thì chịu), ta được:
$$
\begin{align}
\mu(s) &= \frac{\eta(s)}{\sum_{s'} \eta(s')}, && \text{for all } s \in \mathcal{S}.
\end{align}
$$

Mục tiêu lý tưởng của $\overline{\text{VE}}$ là tìm *global optimum* $w^*$, tức là $\overline{\text{VE}}(\textbf{w}^*) \leq \overline{\text{VE}}(\textbf{w})$. Tuy nhiên trong thực tế với các xấp xỉ hàm phức tạp, đặc biệt là với mạng neural network, rất khó để đạt được *global optimum*, *local optimum* là điều thường xuyên chúng ta có được (hoặc có khả năng là diverge in limit luôn).

Ta có công thức cập nhật gradient như sau:
$$
\begin{align}
\mathbf{w}_{t+1} \doteq \mathbf{w}_t + \alpha \left[ U_t - \hat{v}(S_t, \mathbf{w}_t) \right] \nabla \hat{v}(S_t, \mathbf{w}_t).
\end{align}
$$

Với $U_t$ là xấp xỉ cho $v(\textbf{w})$ vì ta khó có khả năng có giá trị chính xác.

==Lưu ý==:
- $U_t$ có thể được thay bằng $G_t$ 
- $G_t$ thường dùng trong Monte Carlo, nếu dùng bootstrapping, tức là $U_t$ lúc này cũng phụ thuộc vào $\hat{v}(S_t, \textbf{w}_t)$. Lúc này ta bỏ qua gradient từ $U_t$ và cách làm này được gọi là **semi gradient** method.
- semi gradient không robustly converge như gradient method, trong trường hợp linear thì có, nhưng nó có ưu điểm là học khá nhanh.

---

## On-policy Control with  Approximation

==Giả mã==: Semi-gradient SARSA, ước lượng $\hat{q} \approx q_*$

$$
\begin{align*}
&\textbf{Input: } \hat{q}: \mathcal{S} \times \mathcal{A} \times \mathbb{R}^d \to \mathbb{R} \text{ (differentiable action-value function)} \\
&\text{Algorithm parameters: step size } \alpha > 0, \text{ small } \varepsilon > 0 \\
&\text{Initialize value-function weights } \mathbf{w} \in \mathbb{R}^d \text{ arbitrarily (e.g., } \mathbf{w} = 0 \text{)} \\[1em]

&\textbf{Loop for each episode:} \\
&\quad S, A \leftarrow \text{initial state and action of episode (e.g., } \varepsilon\text{-greedy)} \\
&\quad \textbf{Loop for each step of episode:} \\
&\qquad \text{Take action } A, \text{ observe } R, S' \\
&\qquad \textbf{If } S' \text{ is terminal:} \\
&\qquad\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha \left[ R - \hat{q}(S, A, \mathbf{w}) \right] \nabla \hat{q}(S, A, \mathbf{w}) \\
&\qquad\quad \text{Go to next episode} \\
&\qquad \text{Choose } A' \text{ as a function of } \hat{q}(S', \cdot, \mathbf{w}) \text{ (e.g., } \varepsilon\text{-greedy)} \\
&\qquad \mathbf{w} \leftarrow \mathbf{w} + \alpha \left[ R + \gamma \hat{q}(S', A', \mathbf{w}) - \hat{q}(S, A, \mathbf{w}) \right] \nabla \hat{q}(S, A, \mathbf{w}) \\
&\qquad S \leftarrow S' \\
&\qquad A \leftarrow A'
\end{align*}
$$

==Giả mã==: n-step SARSA
$$
\begin{align*}
&\textbf{Input: } \hat{q}: \mathcal{S} \times \mathcal{A} \times \mathbb{R}^d \to \mathbb{R} \text{ (differentiable action-value function)} \\
&\textbf{Input: } \pi \text{ (if estimating } q_\pi \text{)} \\
&\text{Algorithm parameters: step size } \alpha > 0,\ \varepsilon > 0,\ \text{an integer } n > 0 \\
&\text{Initialize weights } \mathbf{w} \in \mathbb{R}^d \text{ arbitrarily (e.g., } \mathbf{w} = 0 \text{)} \\
&\text{Store } (S_t, A_t, R_t) \text{ modulo } n+1 \\[1em]

&\textbf{Loop for each episode:} \\
&\quad S_0 \leftarrow \text{initial non-terminal state} \\
&\quad A_0 \sim \pi(\cdot \mid S_0) \text{ or } \varepsilon\text{-greedy wrt } \hat{q}(S_0, \cdot, \mathbf{w}) \\
&\quad T \leftarrow \infty \\
&\quad \textbf{For } t = 0, 1, 2, \ldots: \\
&\qquad \textbf{If } t < T: \\
&\qquad\quad \text{Take action } A_t \\
&\qquad\quad \text{Observe and store } R_{t+1},\ S_{t+1} \\
&\qquad\quad \textbf{If } S_{t+1} \text{ is terminal:} \\
&\qquad\quad\quad T \leftarrow t + 1 \\
&\qquad\quad \textbf{else:} \\
&\qquad\quad\quad A_{t+1} \sim \pi(\cdot \mid S_{t+1}) \text{ or } \varepsilon\text{-greedy wrt } \hat{q}(S_{t+1}, \cdot, \mathbf{w}) \\[0.5em]

&\qquad \tau \leftarrow t - n + 1 \quad (\tau \text{ is the time being updated}) \\
&\qquad \textbf{If } \tau \ge 0: \\
&\qquad\quad G \leftarrow \sum_{i = \tau+1}^{\min(\tau+n, T)} \gamma^{i - \tau - 1} R_i \\
&\qquad\quad \textbf{If } \tau + n < T:\quad G \leftarrow G + \gamma^n \hat{q}(S_{\tau+n}, A_{\tau+n}, \mathbf{w}) \\
&\qquad\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha \left[ G - \hat{q}(S_\tau, A_\tau, \mathbf{w}) \right] \nabla \hat{q}(S_\tau, A_\tau, \mathbf{w}) \\
&\text{Until } \tau = T - 1
\end{align*}
$$
---

## Average Reward: A New Problem Setting for Continuing Tasks

Ta định nghĩa average reward như sau:
$$
\begin{align*}
r(\pi) &\doteq \lim_{h \to \infty} \frac{1}{h} \sum_{t=1}^{h} \mathbb{E}[R_t \mid S_0, A_{0:t-1} \sim \pi] \\
       &= \lim_{t \to \infty} \mathbb{E}[R_t \mid S_0, A_{0:t-1} \sim \pi], \\
       &= \sum_s \mu_\pi(s) \sum_a \pi(a \mid s) \sum_{s', r} p(s', r \mid s, a) r,
\end{align*}
$$
Và hãy nhớ $\mu(s)$ là steady state solution. Tức là thỏa mãn phương trình:
$$
\sum_s \mu_\pi(s) \sum_a \pi(a \mid s) p(s' \mid s, a) = \mu_\pi(s').
$$
Đây là điều kiện cho steady state distribution của MDP.
==Giả mã==: Differential semi-gradient SARSA cho ước lượng $\hat{q} \approx q_*$ 

$$
\begin{align*}
&\textbf{Input: } \hat{q}: \mathcal{S} \times \mathcal{A} \times \mathbb{R}^d \to \mathbb{R} \text{ (differentiable action-value function)} \\
&\text{Algorithm parameters: step sizes } \alpha > 0,\ \beta > 0 \\
&\text{Initialize value-function weights } \mathbf{w} \in \mathbb{R}^d \text{ arbitrarily (e.g., } \mathbf{w} = 0 \text{)} \\
&\text{Initialize average reward estimate } \bar{R} \in \mathbb{R} \text{ (e.g., } \bar{R} = 0 \text{)} \\[1em]

&\text{Initialize state } S,\ \text{and action } A \\[0.5em]

&\textbf{Loop for each step:} \\
&\quad \text{Take action } A,\ \text{observe } R,\ S' \\
&\quad \text{Choose } A' \text{ as a function of } \hat{q}(S', \cdot, \mathbf{w})\ \text{(e.g., } \varepsilon\text{-greedy)} \\
&\quad \delta \leftarrow R - \bar{R} + \hat{q}(S', A', \mathbf{w}) - \hat{q}(S, A, \mathbf{w}) \\
&\quad \bar{R} \leftarrow \bar{R} + \beta \delta \\
&\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha \delta \nabla \hat{q}(S, A, \mathbf{w}) \\
&\quad S \leftarrow S' \\
&\quad A \leftarrow A'
\end{align*}
$$

==Lưu ý==: discounting setting là vô ích:
$$
\begin{align*}
J(\pi) &= \sum_s \mu_\pi(s) v^\gamma_\pi(s) 
\quad\text{(where } v^\gamma_\pi \text{ is the discounted value function)} \\
&= \sum_s \mu_\pi(s) \sum_a \pi(a \mid s) \sum_{s'} \sum_r p(s', r \mid s, a) [r + \gamma v^\gamma_\pi(s')] 
\quad\text{(Bellman Eq.)} \\
&= r(\pi) + \sum_s \mu_\pi(s) \sum_a \pi(a \mid s) \sum_{s'} \sum_r p(s', r \mid s, a) \gamma v^\gamma_\pi(s') 
\quad\text{(from 10.7)} \\
&= r(\pi) + \gamma \sum_{s'} v^\gamma_\pi(s') \sum_s \mu_\pi(s) \sum_a \pi(a \mid s) p(s' \mid s, a) 
\quad\text{(from 3.4)} \\
&= r(\pi) + \gamma \sum_{s'} v^\gamma_\pi(s') \mu_\pi(s') 
\quad\text{(from 10.8)} \\
&= r(\pi) + \gamma J(\pi) \\
&= r(\pi) + \gamma r(\pi) + \gamma^2 J(\pi) \\
&= r(\pi) + \gamma r(\pi) + \gamma^2 r(\pi) + \gamma^3 r(\pi) + \cdots \\
&= \frac{1}{1 - \gamma} r(\pi).
\end{align*}
$$
Chỉ số của các công thức trên hãy xem trong sách.

==Giả mã==: differential semi-gradient n-step SARSA

$$
\begin{align*}
&\textbf{Input: } \hat{q}: \mathcal{S} \times \mathcal{A} \times \mathbb{R}^d \to \mathbb{R},\ \text{a differentiable function} \\
&\text{Policy } \pi,\ \text{step size } \alpha > 0,\ \beta > 0,\ \text{positive integer } n \\
&\text{Initialize } \mathbf{w} \in \mathbb{R}^d\ \text{arbitrarily (e.g., } \mathbf{w} = 0 \text{)} \\
&\text{Initialize average reward estimate } \bar{R} \in \mathbb{R} \text{ (e.g., } \bar{R} = 0 \text{)} \\
&\text{Store } (S_t, A_t, R_t)\ \text{modulo } n+1 \\[1em]

&\text{Initialize and store } S_0\ \text{and } A_0 \\[0.5em]

&\textbf{Loop for each step } t = 0, 1, 2, \dots:\\
&\quad \text{Take action } A_t \\
&\quad \text{Observe and store } R_{t+1},\ S_{t+1} \\
&\quad \text{Select and store } A_{t+1} \sim \pi(\cdot \mid S_{t+1})\ \text{or } \varepsilon\text{-greedy wrt } \hat{q}(S_{t+1}, \cdot, \mathbf{w}) \\
&\quad \tau \leftarrow t - n + 1 \quad \text{(time whose estimate is being updated)} \\
&\quad \textbf{If } \tau \ge 0: \\
&\qquad \delta \leftarrow \sum_{i=\tau+1}^{\tau+n} (R_i - \bar{R}) + \hat{q}(S_{\tau+n}, A_{\tau+n}, \mathbf{w}) - \hat{q}(S_\tau, A_\tau, \mathbf{w}) \\
&\qquad \bar{R} \leftarrow \bar{R} + \beta \delta \\
&\qquad \mathbf{w} \leftarrow \mathbf{w} + \alpha \delta \nabla \hat{q}(S_\tau, A_\tau, \mathbf{w})
\end{align*}
$$

---

## Off-policy Methods with Approximation

> The extension to function approximation turns out to be significantly different and harder for off-policy learning than it is for on-policy learning


> The challenge of off-policy learning can be divided into two parts, one that arises in the tabular case and one that arises only with function approximation. The first part of the challenge has to do with the target of the update (not to be confused with the target policy), and the second part has to do with the distribution of the updates. The techniques related to importance sampling developed in Chapters 5 and 7 deal with the first part; these may increase variance but are needed in all successful algorithms, tabular and approximate. The extension of these techniques to function approximation are quickly dealt with in the first section of this chapter.

Đối với function approximation, nhiều vấn đề hơn cần được xử lý. Mở rộng off policy cũng rất khó để thực hiện hơn là on policy. 

Sự khó khăn của off policy có thể được chia làm hai phần:
- Target of update
- Distribution of update
Trong những phần trước, các kĩ thuật liên quan tới importance sampling đã được phát triển để khắc phục khó khăn đầu tiên, tuy có làm tăng variance nhưng là cần thiết để thiết kế một thuật toán thành công. 

Nhắc lại công thức chút:

$$
\begin{align*}
\delta_t &\doteq R_{t+1} + \gamma \hat{v}(S_{t+1}, \mathbf{w}_t) - \hat{v}(S_t, \mathbf{w}_t), \\
\delta_t &\doteq R_{t+1} - \bar{R}_t + \hat{v}(S_{t+1}, \mathbf{w}_t) - \hat{v}(S_t, \mathbf{w}_t).
\end{align*}
$$
Cái ở trên là cho episodic discounted, ở dưới là cho continuing average. 

One step expected SARSA:

$$
\begin{align*}
\mathbf{w}_{t+1} &\doteq \mathbf{w}_t + \alpha \delta_t \nabla \hat{q}(S_t, A_t, \mathbf{w}_t), \quad \text{with} \\
\delta_t &\doteq R_{t+1} + \gamma \sum_a \pi(a \mid S_{t+1}) \hat{q}(S_{t+1}, a, \mathbf{w}_t) - \hat{q}(S_t, A_t, \mathbf{w}_t), \quad \text{(episodic)} \\
\delta_t &\doteq R_{t+1} - \bar{R}_t + \sum_a \pi(a \mid S_{t+1}) \hat{q}(S_{t+1}, a, \mathbf{w}_t) - \hat{q}(S_t, A_t, \mathbf{w}_t). \quad \text{(continuing)}
\end{align*}
$$
Ở n-step, công thức là:

$$
\begin{align*}
\mathbf{w}_{t+n} &\doteq \mathbf{w}_{t+n-1} + \alpha \rho_{t+1} \cdots \rho_{t+n-1} 
\left[ G_{t:t+n} - \hat{q}(S_t, A_t, \mathbf{w}_{t+n-1}) \right] 
\nabla \hat{q}(S_t, A_t, \mathbf{w}_{t+n-1}), \\
\\
\text{with} \quad 
G_{t:t+n} &\doteq R_{t+1} + \cdots + \gamma^{n-1} R_{t+n} + \gamma^n \hat{q}(S_{t+n}, A_{t+n}, \mathbf{w}_{t+n-1}), 
\quad \text{(episodic)} \\
G_{t:t+n} &\doteq R_{t+1} - \bar{R}_t + \cdots + R_{t+n} - \bar{R}_{t+n-1} 
+ \hat{q}(S_{t+n}, A_{t+n}, \mathbf{w}_{t+n-1}), 
\quad \text{(continuing)}
\end{align*}
$$

Bên cạnh đó, ta cũng có công thức không hề dựa vào importance sampling (n-step backup tree)
$$
\begin{align*}
\mathbf{w}_{t+n} &\doteq \mathbf{w}_{t+n-1} + \alpha 
\left[ G_{t:t+n} - \hat{q}(S_t, A_t, \mathbf{w}_{t+n-1}) \right] 
\nabla \hat{q}(S_t, A_t, \mathbf{w}_{t+n-1}), \\
G_{t:t+n} &\doteq \hat{q}(S_t, A_t, \mathbf{w}_{t-1}) + 
\sum_{k=t}^{t+n-1} \delta_k \prod_{i=t+1}^{k} \gamma \pi(A_i \mid S_i),
\end{align*}
$$

---

## The Deadly Triad

==Lưu ý==: Bất ổn định hoặc thậm chí là phân kì lúc huấn luyện xuất hiện bất kì khi nào cả ba yếu tố bên dưới đây gặp mặt nhau:
- **Function approximation**
- **Bootstrapping**
- **Off-policy training**

Nếu ta có thể tránh một trong ba yếu tố trên, ta có thể tránh sự bất ổn định lúc huấn luyện. Vậy ta có thể bỏ qua yếu tố nào không?

- **Function approximation**: Rõ ràng là không thể, bởi ta cần scale giải pháp lên cho các bài toán lớn hơn rất nhiều, noi mà những giải pháp như tabular sẽ không thể đáp ứng nổi do số chiều là quá lớn.
- **Bootstrapping**: Có thể tránh, nhưng với sự đánh đổi về mặt hiệu quả sử dụng dữ liệu, tức là ta cần nhiều data hơn để train. Tuy nhiên cái giá này cũng là lớn, ta cũng muốn vẫn giữ nó trong bộ công cụ của mình.
- **Off-policy training**: Có thể, nhưng cũng đi kèm với những cái giá. Ví dụ có những ứng dụng nhiều policy cần được học song song trong lấy data từ một behavior policy duy nhất.

---

## Linear Value-function Geometry

Giả sử vector trọng số $\textbf{w}=(w_1, w_2)^T$ và không gian trạng thái là $\mathcal{S} \{ s_1, s_2, s_3\}$. Tức là không gian $v$ thực sự là không gian 3 chiều trong khi $\textbf{w}$ chỉ có khả năng cover một mặt phẳng 2D (giả sử $v(\textbf{w})$ là hàm linear, hình dạng có thể phức tạp hơn khi là các hàm khác như ANN) trong khi $v$ thực sự nằm ngoài mặt phẳng.

![[Pasted image 20250713041433.png]]

Ta có:
- $\overline{PBE}$ : Mean Square Projected Bellman Error
- $\overline{BE}$ : Mean Square Bellman Error
- $\overline{VE}$ : Mean Square Value Error
- $\overline{TDE}$ : Mean Square TD Error
- $\overline{RE}$ : Mean Square Return Error

Như hình trên minh họa, $w_*$ cho mỗi độ đo là khác nhau.

==Lưu ý==:  
- $\overline{TDE}(\textbf{w}) \leq \overline{TDE}(\textbf{w}_*)$ , tức là thâm chí đối với giá trị $v$ chính xác, $v$ vẫn không phải minimum cho độ đo này.

---

## Super confusing

![[Pasted image 20250713162944.png]]

![[Pasted image 20250713163003.png]]

Tác giả đưa ra hai ví dụ trên, trong cả hai lần đọc thì mình đều bối rối. 
Tuy nhiên sau một lúc suy ngẫm thì mình đã hiểu ra, thực ra A1 và A2 trong ví dụ 2 vẫn được update độc lập với nhau, sự "gộp chung state" diễn ra do hàm $v$ đang có ít tham số hơn state thực sự.


![[Pasted image 20250713163130.png]]

Bên cạnh đó, tác giả cũng đưa ra định nghĩa learnable và không learnable, tuy nhiên mình còn rất nhiều thắc mắc xoay quanh mục này.

Nhớ lại mục tiêu cuối cùng của chúng ta là tìm policy tối ưu, để làm như vậy, một cách khả dĩ là ước lượng $v$, hơn nữa ta cần một thang đo cho việc ước lượng của ta.

Trong hình, tác giả bảo rằng hai MDP cho ra hai $\overline{VE}$ khác nhau, tức là nếu ta đang ở MDP 1 nhưng thang đo lại cho ra độ đo của MDP 2 thì kết quả là vô dụng. Tuy nhiên mình rất thắc mắc một điều là chẳng phải $\overline{BE}$ cũng cho cùng một giá trị cho một data distribution đó sao? Ví dụ có hai MDP khác nhau ẩn bên dưới thì sao? $\overline{BE}$ vẫn có tác dụng chứ? 

Mình cũng không rõ, tuy nhiên mình nghĩ là do cách trình bày của tác giả hơi khó theo dõi. Theo mình, do ta chỉ nhận được observation mà không biết về MDP nên ta ưu tiên các độ đo mà có thể được suy ra từ distribution, thế là được. Vậy còn nếu nhiều MDP cho ra cùng một data distribution thì sao? Cũng không sao, ta chỉ ước lượng thôi, và ít nhất cho tới nay đây là thứ tốt nhất ta có thể làm rồi, hãy nhớ lại maximum likelihood cũng làm theo cách này.

---

## Eligibility Traces

Bạn còn nhớ $\lambda$ trong $\text{TD}(\lambda)$ không? Đúng rồi, đây là eligibility traces đó. Này là một kĩ thuật cơ bản trong RL, gần như hầu hết TD learning hoặc Q learning đều có thể kết hợp được với nó để tạo ra một thuật toán tổng quát hơn. Ngoài ra, Eligibility Traces còn có thể giúp Monte Carlo method học online, mặc dù không tương đương trong mọi tình huống nhưng đủ tốt trong thực tế.

Đối với n-step update như của Monte Carlo hoặc TD, update phải đợi các reward và state trong tương lai để update, đây gọi là forward view. Eligibility traces cho ta một cách tiếp cận backward view.

Nhớ lại công thức tính n-step return là:
$$
\begin{align}
G_{t:t+n} &\doteq R_{t+1} + \gamma R_{t+2} + \cdots + \gamma^{n-1} R_{t+n} + \gamma^n \hat{v}(S_{t+n}, \mathbf{w}_{t+n-1}), \quad 0 \leq t \leq T - n,
\end{align}
$$

Tuy nhiên, thay vì chỉ update một lần return như vậy, ta có thể lấy trung bình của nhiều return ở nhiều step khác nhau:
$$
\frac{1}{2}G_{t:t+2}  + \frac{1}{2}G_{t:t+4}
$$
Miễn sao trọng số tổng lại bằng một là được, một update như vậy gọi là *compound update*. Ta định nghĩa $\lambda$ return như sau:
$$
\begin{align}
G_t^\lambda \doteq (1 - \lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_{t:t+n}.
\end{align}
$$
==Note==:
$$
\begin{align}
\sum_{n=1}^{\infty} \lambda^{n-1} = \lambda^0 + \lambda^1 + \lambda^2 + \cdots = \frac{1}{1 - \lambda} \quad (|\lambda| < 1)
\end{align}
$$
Lưu ý tổng trên bằng 1 chỉ trong trường hợp dãy vô hạn, dãy hữu hạn thì tổng cuối cùng sẽ bằng $1 - \lambda ^N$.
Đối với trường hợp hữu hạn:
![[Pasted image 20250713201825.png]]
Tức công thức là:
$$
\begin{align}
G_t^\lambda = (1 - \lambda) \sum_{n=1}^{T - t - 1} \lambda^{n - 1} G_{t:t+n} + \lambda^{T - t - 1} G_t,
\end{align}
$$
Để ý, nếu $\lambda = 1$, công thức update sẽ y hệt Monte Carlo, nếu $\lambda = 0$, công thức sẽ giống hệt $TD$, hay chính xác hơn là $TD(0)$. 

==Note==: $\lambda$ return offer một cách khác để smooth update như n-step update.

![[Pasted image 20250713203118.png]]
Như đã trình bày ở trên, cách làm này là forward view.

---
## TD($\lambda$)

TD($\lambda$) cho ta ba lợi thế so với thuật toán off-line ở trên:
- Update $\textbf{w}$ tại mỗi step, giúp observation mới được sử dụng ngay
- Phân phối tính toán đều tại mỗi bước thay vì đợi tới cuối mới update cực nặng như Monte Carlo
- Có thể áp dụng cho cả continuing

Ta có một số định nghĩa sau đây:
$$
\begin{align}
\mathbf{z}_{-1} &\doteq \mathbf{0}, \\
\mathbf{z}_t &\doteq \gamma \lambda \mathbf{z}_{t-1} + \nabla \hat{v}(S_t, \mathbf{w}_t), \quad 0 \leq t \leq T\\
\delta_t &\doteq R_{t+1} + \gamma \hat{v}(S_{t+1}, \mathbf{w}_t) - \hat{v}(S_t, \mathbf{w}_t).\\
\mathbf{w}_{t+1} &\doteq \mathbf{w}_t + \alpha \, \delta_t \, \mathbf{z}_t.
\end{align}
$$
Dễ thấy nếu $\textbf{w}$ không thay đổi suốt một episode, công thức cập nhật trên sẽ tương đương với công thức off-line như trên. Cách làm này gọi là backward view

![[Pasted image 20250713204321.png]]

---

## True Online TD($\lambda$)

Thuật toán TD($\lambda$) ở trên chỉ là một xấp xỉ do các credit (đạo hàm cũ) được dùng đi dùng lại trong khi $\textbf{w}$ thực sự đã được update. Tất nhiên là ta có một cách để chỉnh lại đó là mỗi bước update, ta revert và update lại tất cả đạo hàm để tạo lại các $\textbf{w}$ mới, tất nhiên là việc này tốn một chi phí rất lớn:
$$
\begin{align*}
\begin{array}{cccccc}
\mathbf{w}_0^0 &                 &                 &                 &        &        \\
\mathbf{w}_1^0 & \mathbf{w}_1^1 &                 &                 &        &        \\
\mathbf{w}_2^0 & \mathbf{w}_2^1 & \mathbf{w}_2^2 &                 &        &        \\
\mathbf{w}_3^0 & \mathbf{w}_3^1 & \mathbf{w}_3^2 & \mathbf{w}_3^3 &        &        \\
\vdots         & \vdots         & \vdots         & \vdots         & \ddots &        \\
\mathbf{w}_T^0 & \mathbf{w}_T^1 & \mathbf{w}_T^2 & \mathbf{w}_T^3 & \cdots & \mathbf{w}_T^T
\end{array}
\end{align*}
$$
Đây là dãy $\textbf{w}$ tạo ra được ở mỗi bước, mà thực ra ta cũng chỉ cần $\textbf{w}_t^t$ ở cuối. Giờ đặt lại kí hiệu:
$$
\textbf{w}_t \doteq \textbf{w}_t^t
$$
Ta có:
$$
\begin{align}
\mathbf{w}_{t+1} &\doteq \mathbf{w}_t + \alpha \delta_t \mathbf{z}_t + \alpha \left( \mathbf{w}_t^\top \mathbf{x}_t - \mathbf{w}_{t-1}^\top \mathbf{x}_t \right) \left( \mathbf{z}_t - \mathbf{x}_t \right),\\
\mathbf{z}_t &\doteq \gamma \lambda \mathbf{z}_{t-1} + \left( 1 - \alpha \gamma \lambda \mathbf{z}_{t-1}^\top \mathbf{x}_t \right) \mathbf{x}_t.
\end{align}
$$
Chứng minh sao thì chịu, trong sách cũng bảo là rất phức tạp để trình bày cho nên họ cũng lược bỏ.

==Giả mã==:
$$
\begin{align}
&\text{Initialize: } \mathbf{w} \in \mathbb{R}^d \quad (\text{e.g., } \mathbf{w} = \mathbf{0}) \\
&\text{Loop for each episode:} \notag \\
&\quad \mathbf{x} \leftarrow \text{initial feature vector of first state} \\
&\quad \mathbf{z} \leftarrow \mathbf{0} \\
&\quad V_{\text{old}} \leftarrow 0 \\
&\quad \text{Loop for each step of episode:} \notag \\
&\quad\quad \text{Choose } A \sim \pi \\
&\quad\quad \text{Take action } A, \text{ observe reward } R \text{ and next feature vector } \mathbf{x}' \\
&\quad\quad V \leftarrow \mathbf{w}^\top \mathbf{x} \\
&\quad\quad V' \leftarrow \mathbf{w}^\top \mathbf{x}' \\
&\quad\quad \delta \leftarrow R + \gamma V' - V \\
&\quad\quad \mathbf{z} \leftarrow \gamma \lambda \mathbf{z} + \left(1 - \alpha \gamma \lambda \mathbf{z}^\top \mathbf{x} \right) \mathbf{x} \\
&\quad\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha \left( \delta + V - V_{\text{old}} \right) \mathbf{z} - \alpha \left( V - V_{\text{old}} \right) \mathbf{x} \\
&\quad\quad V_{\text{old}} \leftarrow V' \\
&\quad\quad \mathbf{x} \leftarrow \mathbf{x}' \\
&\quad \text{until } \mathbf{x}' = \mathbf{0} \quad (\text{terminal state}) \notag
\end{align}
$$


Ngoài ra, có một trace khác là Dutch trace, hiệu quả hơn. Cơ mà khó quá không đọc đâu=))))

==Note==: Tương tự, eligibility trace và true online cũng có thể được mở rộng ra với SARSA:

==Giả mã==: eligibility
$$
\begin{align}
&\text{Initialize: } \mathbf{w} = (w_1, \ldots, w_d)^\top, \quad \mathbf{z} = \mathbf{0} \in \mathbb{R}^d \\
&\text{Loop for each episode:} \notag \\
&\quad S \leftarrow \text{initial state} \\
&\quad A \sim \pi(\cdot \mid S) \quad \text{or } \varepsilon\text{-greedy w.r.t. } \hat{q}(S, \cdot, \mathbf{w}) \\
&\quad \mathbf{z} \leftarrow \mathbf{0} \\
&\quad \text{Loop for each step of episode:} \notag \\
&\quad\quad \text{Take action } A, \text{ observe } R, S' \\
&\quad\quad \delta \leftarrow R \\
&\quad\quad \text{Loop for } i \in \mathcal{F}(S, A): \notag \\
&\quad\quad\quad \delta \leftarrow \delta - w_i \\
&\quad\quad\quad z_i \leftarrow z_i + 1 \quad \text{(accumulating traces)} \\
&\quad\quad\quad \text{or } z_i \leftarrow 1 \quad \text{(replacing traces)} \\
&\quad\quad \text{If } S' \text{ is terminal:} \notag \\
&\quad\quad\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha \delta \mathbf{z} \\
&\quad\quad\quad \text{Go to next episode} \\
&\quad\quad A' \sim \pi(\cdot \mid S') \quad \text{or near-greedy w.r.t. } \hat{q}(S', \cdot, \mathbf{w}) \\
&\quad\quad \text{Loop for } i \in \mathcal{F}(S', A'): \notag \\
&\quad\quad\quad \delta \leftarrow \delta + \gamma w_i \\
&\quad\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha \delta \mathbf{z} \\
&\quad\quad \mathbf{z} \leftarrow \gamma \lambda \mathbf{z} \\
&\quad\quad S \leftarrow S'; \quad A \leftarrow A'
\end{align}
$$

==Giả mã==: True online
$$
\begin{align}
&\text{Initialize: } \mathbf{w} \in \mathbb{R}^d \quad (\text{e.g., } \mathbf{w} = \mathbf{0}) \\
&\text{Loop for each episode:} \notag \\
&\quad S \leftarrow \text{initial state} \\
&\quad A \sim \pi(\cdot \mid S) \quad \text{or near-greedy from } S \text{ using } \mathbf{w} \\
&\quad \mathbf{x} \leftarrow \mathbf{x}(S, A) \\
&\quad \mathbf{z} \leftarrow \mathbf{0} \\
&\quad Q_{\text{old}} \leftarrow 0 \\
&\quad \text{Loop for each step of episode:} \notag \\
&\quad\quad \text{Take action } A, \text{ observe } R, S' \\
&\quad\quad A' \sim \pi(\cdot \mid S') \quad \text{or near-greedy from } S' \text{ using } \mathbf{w} \\
&\quad\quad \mathbf{x}' \leftarrow \mathbf{x}(S', A') \\
&\quad\quad Q \leftarrow \mathbf{w}^\top \mathbf{x} \\
&\quad\quad Q' \leftarrow \mathbf{w}^\top \mathbf{x}' \\
&\quad\quad \delta \leftarrow R + \gamma Q' - Q \\
&\quad\quad \mathbf{z} \leftarrow \gamma \lambda \mathbf{z} + \left( 1 - \alpha \gamma \lambda \mathbf{z}^\top \mathbf{x} \right) \mathbf{x} \\
&\quad\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha \left( \delta + Q - Q_{\text{old}} \right) \mathbf{z} - \alpha \left( Q - Q_{\text{old}} \right) \mathbf{x} \\
&\quad\quad Q_{\text{old}} \leftarrow Q' \\
&\quad\quad \mathbf{x} \leftarrow \mathbf{x}' \\
&\quad\quad A \leftarrow A' \\
&\quad \text{until } S' \text{ is terminal} \notag
\end{align}
$$
