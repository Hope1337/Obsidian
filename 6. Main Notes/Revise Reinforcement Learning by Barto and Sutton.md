
---
## Part I: Tabular methods

Trong phần này tác giả trình bày một trường hợp đặc biệt khi không gian trạng thái của bài toán "đủ nhỏ" để có thể trình bày ra một bảng.

Nhắc lại, mục tiêu của một Agent RL là tìm policy tối ưu $\pi^*$ sao cho:
$$
\pi^* = \arg\max_{\pi}v_{\pi}(s), \space \forall s \in \mathcal{S}  
$$
- Nếu bạn thắc mắc thì có tồn tại một $\pi^*$ sao cho $v_{\pi*}(s)\geq v(s)_{\pi}, \space \forall s$   
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
==Lưu ý==: Công thức trên là cho trường hợp đơn giản, đáng lẽ phải thay $R_i$ bằng $G_i$ mới đúng.

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
==Chứng minh==: Định lí tổng cấp số nhân hữu hạn:
Cho $r \in \mathbb{R}, \ |r| < 1$ thì:
$$
\sum_{k=0}^{n-1} r^k = \frac{1 - r^n}{1 - r}
$$

Do đó:
$$
\begin{align}
	(1-\alpha)^n + \sum_{i=1}^n\alpha(1-\alpha)^{n-i} &= (1-\alpha)^n + \alpha\frac{1 - (1-\alpha)^n}{1 - (1-\alpha)}\\
	&=(1-\alpha)^n + 1 - (1-\alpha)^n\\
	&=1
\end{align}
$$

==Lưu ý==: Step size $\alpha$ mặc dù giống như learning rate $\eta$, có thể chọn theo kinh nghiệm, tuy nhiên về mặt lý thuyết để đảm bảo dãy hội tụ thì cần có hai điều kiện: 
$$
\begin{align}
\sum_{n=1}^{\infty} \alpha_n(a) &= \infty, \space(1)\\
\sum_{n=1}^{\infty}\alpha^2_n(a) &< \infty, \space(2)
\end{align}
$$  
$(1)$ đảm bảo step size đủ lớn để vượt qua bất kì điều kiện ban đầu nào, $(2)$ đảm bảo rằng step size đủ nhỏ để hội tụ, $\frac{1}{n}$ đáp ứng cả hai điều kiện trên. Tuy nhiên trong thực tế ít ai làm vậy lắm, người tune $\alpha$ như tune learning rate vậy.

---
## Finite Markov Decision Process

$$
p(s', r \mid s, a) \doteq \Pr\{ S_t = s', R_t = r \mid S_{t-1} = s, A_{t-1} = a \},
$$
Đây gọi là *dynamics* của MDP

Return theo episode được định nghĩa là:
$$
G_t \doteq R_{t+1} + R_{t+2} + R_{t+3} \dots + R_T
$$
Nếu thêm discounting $\gamma$ thì return lúc này là:
$$
G_t \doteq R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots = \sum_{k=0}^{\infty}\gamma^k R_{t+k+1}
$$
Không khó để thấy:
$$
G_t = R_{t+1} + \gamma G_{t+1}
$$

==Value function==: Ta định nghĩa value function của một trạng thái $s$ dưới một policy $\pi$, kí hiệu là $v_{\pi}$:
$$
\begin{align}
v_{\pi}(s) \doteq \mathbb{E}_{\pi}[G_t \mid S_t=s] = \mathbb{E}_{\pi} \left[ \sum_{k=0}^{\infty} \gamma^kR_{t+k+1} \mid S_t = s\right], \space \text{for all}\ s  \in  \mathcal{S}
\end{align}
$$ ==Action value function==: 
$$
q_{\pi}(s,a) \doteq \mathbb{E}[G_t | S_t = s, A_t = a] = \mathbb{E}_{\pi}\left[ \sum_{k=0}^{\infty} \gamma^kR_{t+k+1} \mid S_t = s, A_t = a \right]
$$

#### Iterative form

Biến đổi công thức một chút, ta được dạng update phụ thuộc lẫn nhau, tức là $v_{\pi}(s)$ có thể được tính dựa trên $v_{\pi}(s')$ như sau:
$$
\begin{align*}
v_\pi(s) &\doteq \mathbb{E}_\pi \left[ G_t \mid S_t = s \right] \\
         &= \mathbb{E}_\pi \left[ R_{t+1} + \gamma G_{t+1} \mid S_t = s \right] \\
         &= \sum_a \pi(a \mid s) \sum_{s'} \sum_r p(s', r \mid s, a) \left[ r + \gamma \mathbb{E}_\pi \left[ G_{t+1} \mid S_{t+1} = s' \right] \right] \\
         &= \sum_a \pi(a \mid s) \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma v_\pi(s') \right], \quad \text{for all } s \in \mathcal{S}.
\end{align*}
$$
Nhớ lại, dynamics của MDP như sau:
$$
p(s', r \mid s, a)
$$
Do đó, để tính kì vọng của $R_{t+1}$, ta phải xét đến $a$ được chọn là gì ($\pi(a \mid s$)), toàn bộ $r$ có khả năng xảy ra ($\sum_r$) cũng như xác suất biên của $r$ đó ($\sum_s'$). Đó là lí do cho cục công thức dài ngoằng ở trên =))))

> Điều này thì có ý nghĩa gì?
> 
Có chứ! Công thức này để ta tính $v_{\pi}(s)$ theo kiểu update dần dần dựa trên các $v_{\pi}(s')$.

#### Optimal policy

Một policy $\pi$ được gọi là tốt hơn $\pi'$ khi và chỉ khi $v_{\pi}(s) \geq v_{\pi'}(s), \ \forall s$, ==và sẽ luôn tồn tại một ít nhất policy luôn tốt hơn mọi policy khác==. Mặc dù có thể có cùng lúc nhiều optimal policy, ta kí hiệu nó chung là $\pi_*$. Chúng có cùng optimal value function và action value function:
$$
\begin{align}
v_*(s) &\doteq \max_{\pi}v_{\pi}(s), \\
q_*(s,a) &\doteq \max_{\pi}q_{\pi}(s,a)
\end{align}
$$
Ta có:
$$
\begin{align*}
v_*(s) &= \max_{a \in \mathcal{A}(s)} q_*(s, a) \\
       &= \max_a \mathbb{E}_{\pi_*} \left[ G_t \mid S_t = s, A_t = a \right] \\
       &= \max_a \mathbb{E}_{\pi_*} \left[ R_{t+1} + \gamma G_{t+1} \mid S_t = s, A_t = a \right] \\
       &= \max_a \mathbb{E} \left[ R_{t+1} + \gamma v_*(S_{t+1}) \mid S_t = s, A_t = a \right] \\
       &= \max_a \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma v_*(s') \right].
\end{align*}
$$
Tương tự cho action function value:
$$
\begin{align*}
q_*(s, a) 
&= \mathbb{E} \left[ R_{t+1} + \gamma \max_{a'} q_*(S_{t+1}, a') \,\middle|\, S_t = s, A_t = a \right] \\
&= \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma \max_{a'} q_*(s', a') \right].
\end{align*}
$$

==Lưu ý==: Công thức này chỉ khác với công thức trước đó (Bellman equation) ở chỗ là nó tìm max thay vì là $\mathbb{E}$. Có thể hiểu thế này:
- Bellman equation: để một $v_{\pi}(s)$ là một value function, nó phải đáp ứng phương trình này.
- Bellman optimal policy: để một $v_{\pi}(s)$ là một optimal policy, nó phải đáp ứng phương trình này, và tất nhiên cũng đáp ứng luôn Bellman equation.

==Take away==:
$$
\begin{align*}
v_*(s) &= \max_a \mathbb{E} \left[ R_{t+1} + \gamma v_*(S_{t+1}) \mid S_t = s, A_t = a \right] \\
       &= \max_a \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma v_*(s') \right], \quad \text{or} \\
q_*(s, a) &= \mathbb{E} \left[ R_{t+1} + \gamma \max_{a'} q_*(S_{t+1}, a') \mid S_t = s, A_t = a \right] \\
         &= \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma \max_{a'} q_*(s', a') \right].
\end{align*}
$$

#### Policy Evaluation

Ý tưởng là đầu ta có một policy $\pi$, tuy nhiên ta chưa có $v$ tương ứng và ta muốn tính nó. Bắt đầu từ một $v_0$ tùy ý, ta update nó dần dần để $v_k$ trở nên chính xác.

$$
\begin{align*}
v_{k+1}(s) &\doteq \mathbb{E}_\pi \left[ R_{t+1} + \gamma v_k(S_{t+1}) \mid S_t = s \right] \\
          &= \sum_a \pi(a \mid s) \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma v_k(s') \right],
\end{align*}
$$
Tức là ở đây sẽ có hai vòng for, một for ngoài sẽ xét điều kiện sai số đủ để hội tụ, for trong sẽ for toàn bộ $s$ để update.

#### Policy Improvement

Nhớ lại nhiệm vụ của ta là tìm ra policy tối ưu. Vậy làm cách nào? 
Đơn giản, cũng bắt đầu từ một policy ngẫu nhiên $\pi_0$, ta policy evaluation để tính $v_{\pi_0}$, sau đó ta update policy của mình như sau:
$$
\begin{align*}
\pi'(s) &\doteq \arg\max_a q_\pi(s, a) \\
       &= \arg\max_a \mathbb{E} \left[ R_{t+1} + \gamma v_\pi(S_{t+1}) \mid S_t = s, A_t = a \right] \\
       &= \arg\max_a \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma v_\pi(s') \right],
\end{align*}
$$
Hay nói cách khác, policy mới là greedy. 

#### Policy iteration

Kết hợp Value evaluation và policy iteration, ta được một thuật toán lặp để từ một policy $\pi_0$ tùy ý, ta tìm được optimal policy $\pi_*$. 

$$
\pi_0 
\xrightarrow{\text{E}} v_{\pi_0} 
\xrightarrow{\text{I}} \pi_1 
\xrightarrow{\text{E}} v_{\pi_1} 
\xrightarrow{\text{I}} \pi_2 
\xrightarrow{\text{E}} \cdots 
\xrightarrow{\text{I}} \pi_* 
\xrightarrow{\text{E}} v_*,
$$
==Giả mã==:
$$
\begin{align*}
&\textbf{1. Initialization} \\
&\quad V(s) \in \mathbb{R} \text{ and } \pi(s) \in \mathcal{A}(s) \text{ arbitrarily for all } s \in \mathcal{S} \\[1ex]

&\textbf{2. Policy Evaluation} \\
&\textbf{Loop:} \\
&\quad \Delta \leftarrow 0 \\
&\quad \text{Loop for each } s \in \mathcal{S}: \\
&\quad\quad v \leftarrow V(s) \\
&\quad\quad V(s) \leftarrow \sum_{s', r} p(s', r \mid s, \pi(s)) \left[ r + \gamma V(s') \right] \\
&\quad\quad \Delta \leftarrow \max(\Delta, |v - V(s)|) \\
&\quad \text{until } \Delta < \theta\ \text{(a small positive threshold)} \\[1ex]

&\textbf{3. Policy Improvement} \\
&\quad policy\text{-}stable \leftarrow \text{true} \\
&\quad \text{For each } s \in \mathcal{S}: \\
&\quad\quad old\text{-}action \leftarrow \pi(s) \\
&\quad\quad \pi(s) \leftarrow \arg\max_a \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma V(s') \right] \\
&\quad\quad \text{If } old\text{-}action \ne \pi(s) \text{ then } policy\text{-}stable \leftarrow \text{false} \\[1ex]

&\text{If } policy\text{-}stable, \text{ then stop and return } V \approx v_* \text{ and } \pi \approx \pi_*; \text{ else go to 2}
\end{align*}

$$
#### Value iteration

Ý tưởng là ở bước Policy Evaluation, ta không cần chạy nhiều bước cho $v$ hội tụ mà thậm chí chỉ cần chạy 1 vòng lặp là được. Tức là ở policy iteration, bước policy evaluation được chạy $n$ lần để hội tụ, còn ở value iteration, policy evaluation có thể chạy một lần là được.

==Giả mã==:
$$
\begin{align*}
&\textbf{Algorithm parameter:} \ \theta > 0 \ \text{(small threshold for accuracy)} \\
&\textbf{Initialize: } V(s), \text{ for all } s \in \mathcal{S}^+, \text{ arbitrarily except } V(\text{terminal}) = 0 \\[1ex]

&\textbf{Loop:} \\
&\quad \Delta \leftarrow 0 \\
&\quad \text{Loop for each } s \in \mathcal{S}: \\
&\quad\quad v \leftarrow V(s) \\
&\quad\quad V(s) \leftarrow \max_a \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma V(s') \right] \\
&\quad\quad \Delta \leftarrow \max(\Delta, |v - V(s)|) \\
&\text{until } \Delta < \theta \\[1ex]

&\textbf{Output a deterministic policy } \pi \approx \pi_* \text{ such that:} \\
&\quad \pi(s) = \arg\max_a \sum_{s', r} p(s', r \mid s, a) \left[ r + \gamma V(s') \right]
\end{align*}
$$

==Take away==: Nếu so sánh giã mã trên với Policy Evaluation, chắc bạn thắc mắc bước Policy Improvement đâu rồi. Thực ra nó đã được tích hợp vào công thức cập nhật $V(s)$ bên trên rồi đó.
#### Asynchronous 

Cũng giống value iteration nhưng ở mức cực đoan hơn, tức là ở bước policy evaluation, đáng lẽ phải for qua toàn bộ $s \in \mathcal{S}$ mới được xem là một lặp, asynchronous chỉ cần update một một vài $s$, thậm chí có thể chỉ là một $s$ duy nhất.

>To converge correctly, however, an asynchronous algorithm must continue to update the values of all the states: it can’t ignore any state after some point in the computation. 

--- 

## Monte Carlo Methods

Ở phần trước, để tìm được $\pi_*$, ta phải biết dynamics của môi trường $p(s', r \mid s, a)$, đây là điều thường rất rất hiếm khi khả thi trong thực tế. Monte Carlo Methods cho ta một cách tiếp khác khi lúc này agent thực sự học từ kinh nghiệm thực tế, tức là thử và sai - tương tác với môi trường.

Có thể hình dung Monte Carlo Methods như kiểu bạn lấy trung bình các số liệu trong thực tế trong một state $s$ cụ thể nào đó, theo luật số lớn, càng nhiều sample thì mean của ước lượng càng chính xác.

==Lưu ý==: Do ta không có $p(s', r \mid s, a)$ nên việc ước lượng $q(s, a)$ sẽ có ích hơn là $v(s)$. 

Lý do cho việc này rất đơn giản: nếu ta đang đứng ở $s$, làm sao ta biết được *nên làm gì* để đạt được return lớn nhất? Bạn có thể nghĩ rằng chỉ cần chọn $s'=\arg \max_{s'}v(s')$ là được, nhưng ôi bạn tôi ơi, chúng ta còn xác suất chuyển trạng thái nữa, tức là cái dynamics mà ta đang không biết. Lúc này $q(s, a)$ đưa cho ta một gợi ý cực kì đơn giản chọn $a = \arg \max_s q(s,a)$. 

$$
\begin{align*}
&\textbf{Initialize:} \\
&\quad \pi(s) \in \mathcal{A}(s)\ \text{(arbitrarily), for all } s \in \mathcal{S} \\
&\quad Q(s, a) \in \mathbb{R}\ \text{(arbitrarily), for all } s \in \mathcal{S},\ a \in \mathcal{A}(s) \\
&\quad Returns(s, a) \leftarrow \text{empty list, for all } s \in \mathcal{S},\ a \in \mathcal{A}(s) \\[1ex]

&\textbf{Loop forever (for each episode):} \\
&\quad \text{Choose } S_0 \in \mathcal{S},\ A_0 \in \mathcal{A}(S_0)\ \text{randomly such that all pairs have probability } > 0 \\
&\quad \text{Generate an episode from } S_0, A_0,\ \text{following } \pi: \\
&\quad\quad S_0, A_0, R_1, \dots, S_{T-1}, A_{T-1}, R_T \\
&\quad G \leftarrow 0 \\

&\quad \textbf{Loop for each step of episode},\ t = T{-}1, T{-}2, \dots, 0: \\
&\quad\quad G \leftarrow \gamma G + R_{t+1} \\
&\quad\quad \textbf{Unless the pair } (S_t, A_t) \text{ appears in } (S_0, A_0), \dots, (S_{t-1}, A_{t-1}): \\
&\quad\quad\quad Returns(S_t, A_t) \leftarrow \text{append } G \\
&\quad\quad\quad Q(S_t, A_t) \leftarrow \text{average}(Returns(S_t, A_t)) \\
&\quad\quad\quad \pi(S_t) \leftarrow \arg\max_a Q(S_t, a)
\end{align*}
$$

==Take away==: Có thể thấy giả mã trên là một dạng asynchronous, tức là tương tự như Value Iteration. Tuy nhiên ở Value Iteration, một vòng lặp cho mọi $s \in \mathcal{S}$ được thực hiện, còn ở đây chỉ cập nhật cho các $s$ xuất hiện trong episode.

--- 

Một vấn đề với Monte Carlo là tồn tại khả năng có một số $s$ không được ghé thăm, có thể là do policy hoặc dynamics hiện tại. Điều này đòi hỏi phải có các phương pháp đảm bảo xác suất ghét thăm các $s$ phải luôn $\ge 0$. Chúng ta sẽ tìm hiểu hai cách làm như vậy trong hai tình huống là on-policy method và off policy method.
#### On-policy method

Người ta thường dùng $\epsilon$-soft policy, tức là với xác xuất $\frac{\epsilon}{|\mathcal{A}(s)|}$, chọn action không greedy. $\epsilon$-soft greedy policy vẫn đảm bảo lớn hơn mọi $\epsilon$-soft policy khác như ở policy evaluation.  Tất nhiên là $\epsilon$-soft optimal policy không đảm bảo là policy tối ưu nhất, nó chỉ tối ưu trong các $\epsilon$-soft policy thôi.

#### Off-policy method
###### Important sampling

$$
\mathbb{E}_{p(x)}[f(x)] = \int f(x)\frac{p(x)}{q(x)}q(x) dx = \mathbb{E}_{q(x)}\left[f(x)\frac{p(x)}{q(x)}\right]
$$
Trong trường hợp này, công thức trên nói rằng: Nếu lấy data theo phân phối hiện tại là quá khó hoặc vì lí do gì đó mà muốn tránh, chỉ cần lấy theo phân phối khác, ta sẽ chuyển kết quả về lại phân phối gốc bằng important sampling.

==Lưu ý==: Tùy vào trường hợp mà important sampling có variance cao, đặc biệt là khi hai phân phối quá khác nhau, ví dụ $p(x) = 0$ ở những nơi $q(x) \neq 0$  
Xét một trajectory $A_t, S_t, A_{t +1}, S_{t+1} \dots S_T$, ta có:

$$
\begin{align*}
\Pr\{ A_t, S_{t+1}, A_{t+1}, \ldots, S_T \mid S_t, A_{t:T-1} \sim \pi \}
&= \pi(A_t \mid S_t) p(S_{t+1} \mid S_t, A_t) \pi(A_{t+1} \mid S_{t+1}) \cdots p(S_T \mid S_{T-1}, A_{T-1}) \\
&= \prod_{k=t}^{T-1} \pi(A_k \mid S_k) p(S_{k+1} \mid S_k, A_k),
\end{align*}
$$

importance sampling ratio là:
$$
\rho_{t:T-1} \doteq \frac{
    \prod_{k=t}^{T-1} \pi(A_k \mid S_k) p(S_{k+1} \mid S_k, A_k)
}{
    \prod_{k=t}^{T-1} b(A_k \mid S_k) p(S_{k+1} \mid S_k, A_k)
}
= \prod_{k=t}^{T-1} \frac{\pi(A_k \mid S_k)}{b(A_k \mid S_k)}.
$$
==Lưu ý==: Mặc dù ban đầu có xuất hiện dynamics, về sau chúng triệt tiêu lẫn nhau, chỉ còn lại hai policy.

Nhắc lại, mục đích ban đầu của chúng ta là tính $v(s)$ cho $\pi$, tuy nhiên ta cần một policy khác lấy sample để đảm bao rằng các trạng thái $s$ được bao phủ, do đó behavior policy ở đây là $b$. Sau khi có $G_t$, ta chỉ cần áp dụng importance sampling là được: 

$$
\mathbb{E}[\rho_{t:T-1} G_t \mid S_t = s] = v_\pi(s).
$$

Monte Carlo là thuật toán lấy trung bình để ước lượng, do đó công thức có thể được viết lại là:
$$
\begin{align}
V(s) &\doteq \frac{ \sum_{t \in \mathcal{T}(s)} \rho_{t:T(t)-1} G_t }{ \lvert \mathcal{T}(s) \rvert }.\\
V(s) &\doteq \frac{ \sum_{t \in \mathcal{T}(s)} \rho_{t:T(t)-1} G_t }{ \sum_{t \in \mathcal{T}(s)} \rho_{t:T(t)-1} }, \space \text{weighted version}

\end{align}
$$
#### Incremental Implementation

Chúng ta cần tính:
$$
V_n \doteq \frac{ \sum_{k=1}^{n-1} W_k G_k }{ \sum_{k=1}^{n-1} W_k }, \quad n \geq 2,
$$
Với $W_i$ là bất kì weight nào, $W_i = 1$ thì sẽ trở về là tính trung bình cộng, $W_i = \rho_{t:T(t_i)-1}$ thì trở thành weighted với importance sampling.

==Từ đây ta có thể suy ra công thức==:

$$
\begin{align}
V_{n+1}  &\doteq V_n + \frac{W_n}{C_n} \left[ G_n - V_n \right], \quad n \geq 1 \\
C_{n+1} &\doteq C_n + W_{n+1}
\end{align}
$$
Chứng minh:

$$
\begin{align}
V_{n+1} &= \frac{\sum_{k=1}^{n} W_k G_k}{\sum_{k=1}^{n}W_k}\\
&=V_n + \frac{\sum_{k=1}^{n} W_k G_k}{\sum_{k=1}^{n}W_k} - \frac{\sum_{k=1}^{n-1} W_k G_k}{\sum_{k=1}^{n-1}W_k}\\
&=V_n + \frac{\sum_{k=1}^{n} W_k G_k \sum_{k=1}^{n-1} W_k - \sum_{k=1}^{n-1} W_k G_k \sum_{k=1}^{n} W_k}{\sum_{k=1}^{n} W_k\sum_{k=1}^{n-1} W_k}
\end{align}
$$

Để vậy thì hơi khó thấy, đặt:
$$
\begin{align}
a &= \sum_{k=1}^{n} W_k G_k\\
b &= \sum_{k=1}^{n-1} W_k
\end{align}
$$
Phương trình to ở trên trở thành:
$$
\begin{align}
V_{n+1} &= V_n + \frac{ab - (a - W_n G_n)(b + W_n)}{\sum_{k=1}^{n} W_k \sum_{k=1}^{n-1} W_k}\\
&= V_n + \frac{ab - ab -aW_n +W_n G_n b + W_n^2 G_n}{\sum_{k=1}^{n} W_k \sum_{k=1}^{n-1} W_k}\\
&= V_n + \frac{W_n G_n \sum_{k=1}^{n-1} W_k}{\sum_{k=1}^{n} W_k \sum_{k=1}^{n-1} W_k} - \frac{W_n(\sum_{k=1}^{n} W_k G_k - W_nG_n)}{\sum_{k=1}^{n} W_k \sum_{k=1}^{n-1} W_k}\\
&= V_n + \frac{W_n G_n}{\sum_{k=1}^{n} W_k} - \frac{W_n}{\sum_{k=1}^{n} W_k}V_n \\
&= V_n + \frac{W_n}{\sum_{k=1}^{n} W_k}(G_n - V_n) 
\end{align}
$$

Vl format mỏi lưng quá, thôi xem tạm đi, có vài chỗ hơi tắt xíu=))))))

$$
\begin{align}
&\textbf{Initialize:} \nonumber \\
&\quad Q(s,a) \in \mathbb{R} \quad \text{(arbitrarily)}, \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s) \\
&\quad C(s,a) \leftarrow 0 \\
&\quad \pi(s) \leftarrow \arg\max_a Q(s,a) \quad \text{(with ties broken consistently)} \\
\nonumber \\
&\textbf{Loop forever (for each episode):} \nonumber \\
&\quad b \leftarrow \text{any soft policy} \\
&\quad \text{Generate an episode using } b: \quad S_0, A_0, R_1, \ldots, S_{T-1}, A_{T-1}, R_T \\
&\quad G \leftarrow 0 \\
&\quad W \leftarrow 1 \\
&\quad \textbf{For } t = T{-}1, T{-}2, \ldots, 0: \nonumber \\
&\qquad G \leftarrow \gamma G + R_{t+1} \\
&\qquad C(S_t, A_t) \leftarrow C(S_t, A_t) + W \\
&\qquad Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \frac{W}{C(S_t, A_t)} \left[ G - Q(S_t, A_t) \right] \\
&\qquad \pi(S_t) \leftarrow \arg\max_a Q(S_t, a) \quad \text{(with ties broken consistently)} \\
&\qquad \text{If } A_t \ne \pi(S_t) \text{ then break (proceed to next episode)} \\
&\qquad W \leftarrow W\frac{1}{b(A_t \mid S_t)}
\end{align}
$$

Tuy nhiên có một điểm rất cần lưu ý là trong thuật toán trên, điều kiện if dừng lại khi $A_t$ của behavior policy khác với policy hiện tại. Điều này là bởi sự ảnh hưởng của importance sampling khi nếu điều kiện if xảy ra, importance sampling ratio sẽ ngay lập tức $=0$ trong các vòng lặp tiếp đó. Điều này khiến cho thuật toán có khả năng lãng phí rất nhiều data của một episode.

---
## Giảm variance cho Importance Sampling

Mình chưa bao giờ thực nghiệm cái này, tuy nhiên có một điểm đáng lưu ý trong sách là tác giả luôn trình bày những cách để làm giảm phương sai cho Importance sampling. Tuy nhiên ở lần đầu mình đọc sách thì không có cảm giác rằng điều này là quan trọng, ở lần thứ hai đọc qua thì mình mới để ý kĩ nó hơn. 

==Lưu ý==: phương pháp này chỉ dùng cho

#### Discounting-aware Importance Sampling

Trong phần này tác giả biến đổi công thức rất ảo ma để có thể giảm được variance. Tác giả giải thích cái này trong sách rất khó hiểu, mình không nắm được ở lần đầu đọc, tuy nhiên lần thứ hai đọc qua thì có nắm được một vài ý chính. Theo cảm nhận của mình thì cứ đừng giải thích gì cả, cứ bảo rằng chúng ta đang biến đổi toán học để có thể tách được Importance sampling ratio ra là dễ hiểu nhất cmnr=))))


Nhớ lại return được định nghĩa là:
$$
G_t \doteq R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots \gamma^{T- t-1}R_{T}
$$
Và trong công thức Importance Sampling:
$$
V(s) \doteq \frac{ \sum_{t \in \mathcal{T}(s)} \rho_{t:T-1} G_t }{ \lvert \mathcal{T}(s) \rvert }.
$$
Ý tưởng là ở đây $\rho_{t:T(t)-1}$ và $G_t$ đang được nhân lại với nhau, và $G_t$ thì lại có thể phân tích ra thành các $R_i$, có khả năng giảm variance được. 

Tiếp đến ta biến đổi $G_t$ thành:

$$
\begin{align}
G_t \doteq\;& R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots + \gamma^{T - t - 1} R_T \\
=\;& (1 - \gamma) R_{t+1} \nonumber \\
&+ (1 - \gamma) \gamma (R_{t+1} + R_{t+2}) \nonumber \\
&+ (1 - \gamma) \gamma^2 (R_{t+1} + R_{t+2} + R_{t+3}) \nonumber \\
&\quad \vdots \nonumber \\
&+ (1 - \gamma) \gamma^{T - t - 2} (R_{t+1} + R_{t+2} + \cdots + R_{T-1}) \nonumber \\
&+ \gamma^{T - t - 1} (R_{t+1} + R_{t+2} + \cdots + R_T) \\
=\;& (1 - \gamma) \sum_{h = t+1}^{T - 1} \gamma^{h - t - 1} \bar{G}_{t:h} + \gamma^{T - t - 1} \bar{G}_{t:T}.
\end{align}
$$
Sao nghĩ ra được cái này ý hả, bố ai mà biết dc=))))))))

Tiếp đến, ta nhận thấy rằng ở mỗi term $\bar{G}_{t:h}$ chỉ phụ thuộc vào $R_{t:h}$ thôi nên hệ số importance sampling ở đằng sau có thể được lược bỏ, dẫn đến công thức bên dưới đây: 
$$
\begin{align}
V(s) \doteq \frac{
    \sum_{t \in \mathcal{T}(s)} \left(
        (1 - \gamma) \sum_{h = t+1}^{T(t)-1} \gamma^{h - t - 1} \rho_{t:h-1} \, \bar{G}_{t:h}
        + \gamma^{T(t) - t - 1} \rho_{t:T(t)-1} \, \bar{G}_{t:T(t)}
    \right)
}{
    \lvert \mathcal{T}(s) \rvert
}, \tag{5.9}
\end{align}
$$

hoặc phiên bản weighted:

$$
\begin{align}
V(s) \doteq 
\frac{
    \sum_{t \in \mathcal{T}(s)} \left(
        (1 - \gamma) \sum_{h = t+1}^{T(t)-1} \gamma^{h - t - 1} \rho_{t:h-1} \, \bar{G}_{t:h}
        + \gamma^{T(t) - t - 1} \rho_{t:T(t)-1} \, \bar{G}_{t:T(t)}
    \right)
}{
    \sum_{t \in \mathcal{T}(s)} \left(
        (1 - \gamma) \sum_{h = t+1}^{T(t)-1} \gamma^{h - t - 1} \rho_{t:h-1}
        + \gamma^{T(t) - t - 1} \rho_{t:T(t)-1}
    \right)
}. \tag{5.10}
\end{align}
$$
Nhìn thì kinh dị nhưng thực ra nó vẫn giống công thức Importance Sampling ban đầu thôi, khác ở chỗ hệ số Importance Sampling bị lược bỏ đi (lược bỏ xong dài hơn ban đầu :D). 

Chứng minh cái cục trên giảm được bao nhiêu variance so với công thức gốc thì mình chịu, toán không phải là thứ mình được sinh ra để làm=)))))

#### Per-decision Importance Sampling

Cái này mình thấy đọc dễ hiểu hơn này, nó có insight dễ chịu hơn. 
Đầu tiên ta xem xét công thức cho $G_t$
$$
\begin{align}
\rho_{t:T-1} G_t 
&= \rho_{t:T-1} \left( R_{t+1} + \gamma R_{t+2} + \cdots + \gamma^{T - t - 1} R_T \right) \\
&= \rho_{t:T-1} R_{t+1} + \gamma \rho_{t:T-1} R_{t+2} + \cdots + \gamma^{T - t - 1} \rho_{t:T-1} R_T.
\end{align}
$$
Cũng giống như phần trước, có thể bạn đã để ý, $\rho_{t:T-1}$ bị "dư"  một khúc ở phía sau so với $R_{t+1}$, do đó công thức có thể được viết lại là:

$$
\begin{align}
\widetilde{G}_t =\;& \rho_{t:t} R_{t+1} 
+ \gamma \rho_{t:t+1} R_{t+2} 
+ \gamma^2 \rho_{t:t+2} R_{t+3} 
+ \cdots 
+ \gamma^{T - t - 1} \rho_{t:T-1} R_T.
\end{align}
$$
Và công thức tính $V(s)$:
$$
V(s) \doteq \frac{ \sum_{t \in \mathcal{T}(s)} \widetilde{G}_t }{ \lvert \mathcal{T}(s) \rvert }
$$

---

## Temporal-Difference Learning

Nhớ lại công thức cập nhật theo style incremental đã được chứng minh từ đâu:
$$
\begin{align}
V(S_t) \leftarrow V(S_t) + \alpha \left[ G_t - V(S_t) \right],
\end{align}
$$
==Ta chỉ cần biến đổi Gt bên trong==:
$$
\begin{align}
V(S_t) \leftarrow V(S_t) + \alpha \left[ R_{t+1} + \gamma V(S_{t+1}) - V(S_t) \right]
\end{align}
$$
Ta có được điều này là do:
$$
\begin{align}
v_\pi(s) &\doteq \mathbb{E}_\pi \left[ G_t \mid S_t = s \right] \\
        &= \mathbb{E}_\pi \left[ R_{t+1} + \gamma G_{t+1} \mid S_t = s \right] \\
        &= \mathbb{E}_\pi \left[ R_{t+1} + \gamma v_\pi(S_{t+1}) \mid S_t = s \right].
\end{align}
$$

> Việc biến đổi này có nghĩa gì?

Có chứ, công thức ban đầu là Monte Carlo method, ta chỉ có $G_t$ sau khi toàn bộ episode. Tức là mọi giá trị ($v$ hay $q$) ta dùng trong suốt quá trình lấy mẫu đều là giá trị cũ. Còn công thức biến đổi, mỗi khi bạn take action, ta có ngay $R_{t+1}$ cũng như $V(S_{t+1})$ - cái mà thực chất chỉ là giá trị cũ, tức là ta có một thuật toán online learning.

==Giả mã==:
$$
\begin{align}
&\textbf{Input:} \text{ the policy } \pi \text{ to be evaluated} \\
&\text{Algorithm parameter: step size } \alpha \in (0,1] \\
&\text{Initialize } V(s), \; \forall s \in \mathcal{S}^+, \text{ arbitrarily except that } V(\text{terminal}) = 0 \\
\nonumber \\
&\textbf{Loop for each episode:} \nonumber \\
&\quad \text{Initialize } S \\
&\quad \textbf{Loop for each step of episode:} \nonumber \\
&\qquad A \leftarrow \pi(S) \\
&\qquad \text{Take action } A, \text{ observe } R, S' \\
&\qquad V(S) \leftarrow V(S) + \alpha \left[ R + \gamma V(S') - V(S) \right] \\
&\qquad S \leftarrow S' \\
&\quad \text{until } S \text{ is terminal}
\end{align}
$$


Có một định lượng khá quan trọng (ít nhất là mình thấy vậy) là TD-error:
$$
\begin{align}
\delta_t \doteq R_{t+1} + \gamma V(S_{t+1}) - V(S_t).
\end{align}
$$Và công thức Monte Carlo cũng có thể được viết lại dưới dạng tổng của các TD-errors:
$$
\begin{align}
G_t - V(S_t) 
&= R_{t+1} + \gamma G_{t+1} - V(S_t) + \gamma V(S_{t+1}) - \gamma V(S_{t+1})  \\
&= \delta_t + \gamma (G_{t+1} - V(S_{t+1})) \\
&= \delta_t + \gamma \delta_{t+1} + \gamma^2 (G_{t+2} - V(S_{t+2})) \\
&= \delta_t + \gamma \delta_{t+1} + \gamma^2 \delta_{t+2} + \cdots + \gamma^{T - t - 1} \delta_{T-1} + \gamma^{T - t} (G_T - V(S_T)) \\
&= \delta_t + \gamma \delta_{t+1} + \gamma^2 \delta_{t+2} + \cdots + \gamma^{T - t - 1} \delta_{T-1} + \gamma^{T - t} (0 - 0) \\
&= \sum_{k = t}^{T - 1} \gamma^{k - t} \delta_k. \tag{6.6}
\end{align}
$$

==TD maximum-likelihood trong khi Monte Carlo minimize mean square error==

---

## SARSA

$$
\begin{align}
Q(S_t, A_t) \leftarrow Q(S_t, A_t) + \alpha \left[ R_{t+1} + \gamma Q(S_{t+1}, A_{t+1}) - Q(S_t, A_t) \right].
\end{align}
$$
Lí do nó tên là SARSA là để tính được $Q(S_t, A_t)$, ta cần một bộ:
$$
S_t, A_t, R_{t+1}, S_{t+1}, A_{t+1}
$$

==Giả mã==:
$$
\begin{align}
&\textbf{Algorithm parameters:} \quad \alpha \in (0, 1], \quad \varepsilon > 0 \\
&\text{Initialize } Q(s,a), \quad \forall s \in \mathcal{S}^+,\, a \in \mathcal{A}(s), \text{ arbitrarily except } Q(\text{terminal}, \cdot) = 0 \\
\nonumber \\
&\textbf{Loop for each episode:} \nonumber \\
&\quad \text{Initialize } S \\
&\quad \text{Choose } A \text{ from } S \text{ using policy derived from } Q \text{ (e.g., } \varepsilon\text{-greedy)} \\
&\quad \textbf{Loop for each step of episode:} \nonumber \\
&\qquad \text{Take action } A, \text{ observe } R, S' \\
&\qquad \text{Choose } A' \text{ from } S' \text{ using policy derived from } Q \text{ (e.g., } \varepsilon\text{-greedy)} \\
&\qquad Q(S, A) \leftarrow Q(S, A) + \alpha \left[ R + \gamma Q(S', A') - Q(S, A) \right] \\
&\qquad S \leftarrow S', \quad A \leftarrow A' \\
&\quad \text{until } S \text{ is terminal}
\end{align}
$$


---

## Q-Learning

Q-learning giống như SARSA nhưng là off-policy:
$$
\begin{align}
Q(S_t, A_t) \leftarrow Q(S_t, A_t) 
+ \alpha \left[ R_{t+1} + \gamma \max_a Q(S_{t+1}, a) - Q(S_t, A_t) \right].
\end{align}
$$
==Giả mã==:
$$
\begin{align}
&\textbf{Algorithm parameters:} \quad \alpha \in (0, 1], \quad \varepsilon > 0 \\
&\text{Initialize } Q(s,a), \quad \forall s \in \mathcal{S}^+,\, a \in \mathcal{A}(s), \text{ arbitrarily except } Q(\text{terminal}, \cdot) = 0 \\
\nonumber \\
&\textbf{Loop for each episode:} \nonumber \\
&\quad \text{Initialize } S \\
&\quad \textbf{Loop for each step of episode:} \nonumber \\
&\qquad \text{Choose } A \text{ from } S \text{ using policy derived from } Q \text{ (e.g., } \varepsilon\text{-greedy)} \\
&\qquad \text{Take action } A, \text{ observe } R, S' \\
&\qquad Q(S, A) \leftarrow Q(S, A) + \alpha \left[ R + \gamma \max_{a} Q(S', a) - Q(S, A) \right] \\
&\qquad S \leftarrow S' \\
&\quad \text{until } S \text{ is terminal}
\end{align}
$$

---

## Expected SARSA

Mình không hiểu tại sao nhưng mọi người cứ bảo cái này giống  Q-learning hơn, mình thấy giống cả hai, chủ yếu là tên gọi cho dễ nhớ thôi.
$$
\begin{align}
Q(S_t, A_t) 
&\leftarrow Q(S_t, A_t) + \alpha \left[ R_{t+1} + \gamma \, \mathbb{E}_\pi \left[ Q(S_{t+1}, A_{t+1}) \mid S_{t+1} \right] - Q(S_t, A_t) \right] \\
&\leftarrow Q(S_t, A_t) + \alpha \left[ R_{t+1} + \gamma \sum_{a} \pi(a \mid S_{t+1}) Q(S_{t+1}, a) - Q(S_t, A_t) \right].
\end{align}
$$

---

## Double Learning

Giống như trong Q-learning, tuy nhiên việc dùng cùng một $Q(s, a)$ cho việc ước lượng cũng như chọn hành động để update có thể mang bias cao, thay vào đó có thể chia làm hai hàm $Q_1(s, a)$ và $Q_2(s, a)$ khác nhau. 

==Giả mã==: Ước tính $Q_1 \approx Q_2 \approx q_*$  
$$
\begin{align}
&\textbf{Algorithm parameters:} \quad \alpha \in (0, 1], \quad \varepsilon > 0 \\
&\text{Initialize } Q_1(s,a) \text{ and } Q_2(s,a), \quad \forall s \in \mathcal{S}^+,\, a \in \mathcal{A}(s), \text{ such that } Q(\text{terminal}, \cdot) = 0 \\
\nonumber \\
&\textbf{Loop for each episode:} \nonumber \\
&\quad \text{Initialize } S \\
&\quad \textbf{Loop for each step of episode:} \nonumber \\
&\qquad \text{Choose } A \text{ from } S \text{ using } \varepsilon\text{-greedy policy derived from } Q_1 + Q_2 \\
&\qquad \text{Take action } A, \text{ observe } R, S' \\
&\qquad \text{With probability } 0.5: \nonumber \\
&\qquad\quad Q_1(S, A) \leftarrow Q_1(S, A) + \alpha \left[ R + \gamma Q_2\left(S', \arg\max_a Q_1(S', a) \right) - Q_1(S, A) \right] \\
&\qquad \text{else:} \nonumber \\
&\qquad\quad Q_2(S, A) \leftarrow Q_2(S, A) + \alpha \left[ R + \gamma Q_1\left(S', \arg\max_a Q_2(S', a) \right) - Q_2(S, A) \right] \\
&\qquad S \leftarrow S' \\
&\quad \text{until } S \text{ is terminal}
\end{align}
$$

--- 

## n-step Bootstrapping

TD chỉ cần một step, Monte Carlo đợi đến khi hết một episode. Xét return của n-step:
$$
\begin{align}
G_{t:t+n} &\doteq R_{t+1} + \gamma R_{t+2} + \cdots + \gamma^{n-1} R_{t+n} + \gamma^n V_{t+n-1}(S_{t+n}) \\
V_{t+n}(S_t) &\doteq V_{t+n-1}(S_t) + \alpha \left[ G_{t:t+n} - V_{t+n-1}(S_t) \right],
\end{align}
$$

==Giả mã==: n-step TD ước lượng cho $V \approx v_{\pi}$
$$
\begin{align}
&\textbf{Input:} \text{ a policy } \pi \\
&\text{Algorithm parameters: step size } \alpha \in (0, 1], \text{ a positive integer } n \\
&\text{Initialize } V(s) \text{ arbitrarily, for all } s \in \mathcal{S} \\
&\text{All store/access operations use index mod } n+1 \\
\nonumber \\
&\textbf{Loop for each episode:} \nonumber \\
&\quad \text{Initialize and store } S_0 \ne \text{terminal} \\
&\quad T \leftarrow \infty \\
&\quad \textbf{Loop for } t = 0, 1, 2, \ldots: \nonumber \\
&\qquad \text{If } t < T \text{ then:} \nonumber \\
&\qquad\quad \text{Take action } A_t \sim \pi(\cdot \mid S_t) \\
&\qquad\quad \text{Observe and store reward } R_{t+1} \text{ and next state } S_{t+1} \\
&\qquad\quad \text{If } S_{t+1} \text{ is terminal, then } T \leftarrow t+1 \\
&\qquad \tau \leftarrow t - n + 1 \quad \text{(time whose estimate is being updated)} \\
&\qquad \text{If } \tau \ge 0 \text{ then:} \nonumber \\
&\qquad\quad G \leftarrow \sum_{i = \tau+1}^{\min(\tau+n, T)} \gamma^{i - \tau - 1} R_i \\
&\qquad\quad \text{If } \tau + n < T \text{ then:} \nonumber \\
&\qquad\qquad G \leftarrow G + \gamma^n V(S_{\tau+n}) \\
&\qquad\quad V(S_\tau) \leftarrow V(S_\tau) + \alpha \left[ G - V(S_\tau) \right] \\
&\quad \text{until } \tau = T - 1
\end{align}
$$

---

## n-step SARSA

$$
\begin{align}
G_{t:t+n} \doteq R_{t+1} + \gamma R_{t+2} + \cdots + \gamma^{n-1} R_{t+n} 
+ \gamma^n Q_{t+n-1}(S_{t+n}, A_{t+n}),
\quad n \ge 1,\; 0 \le t < T - n. 
\end{align}
$$

$$
\begin{align}
Q_{t+n}(S_t, A_t) \doteq Q_{t+n-1}(S_t, A_t) 
+ \alpha \left[ G_{t:t+n} - Q_{t+n-1}(S_t, A_t) \right], 
\quad 0 \le t < T,
\end{align}
$$

==Giả mã==:
$$
\begin{align}
&\text{Initialize } Q(s,a) \text{ arbitrarily, for all } s \in \mathcal{S}, a \in \mathcal{A} \\
&\text{Initialize } \pi \text{ to be } \varepsilon\text{-greedy w.r.t. } Q \text{ or to a fixed policy} \\
&\text{Algorithm parameters: } \alpha \in (0,1], \; \varepsilon > 0, \; n \in \mathbb{Z}^+ \\
&\text{All access operations use indices mod } n+1 \\
\nonumber \\
&\textbf{Loop for each episode:} \nonumber \\
&\quad \text{Initialize and store } S_0 \ne \text{terminal} \\
&\quad \text{Select and store action } A_0 \sim \pi(\cdot \mid S_0) \\
&\quad T \leftarrow \infty \\
&\quad \textbf{Loop for } t = 0,1,2,\ldots: \nonumber \\
&\qquad \text{If } t < T: \nonumber \\
&\qquad\quad \text{Take action } A_t \\
&\qquad\quad \text{Observe and store reward } R_{t+1} \text{ and state } S_{t+1} \\
&\qquad\quad \text{If } S_{t+1} \text{ is terminal: } T \leftarrow t + 1 \\
&\qquad\quad \text{else: select and store } A_{t+1} \sim \pi(\cdot \mid S_{t+1}) \\
&\qquad \tau \leftarrow t - n + 1 \\
&\qquad \text{If } \tau \ge 0: \nonumber \\
&\qquad\quad G \leftarrow \sum_{i = \tau+1}^{\min(\tau+n, T)} \gamma^{i - \tau - 1} R_i \\
&\qquad\quad \text{If } \tau + n < T: \nonumber \\
&\qquad\qquad G \leftarrow G + \gamma^n Q(S_{\tau+n}, A_{\tau+n}) \\
&\qquad\quad Q(S_\tau, A_\tau) \leftarrow Q(S_\tau, A_\tau) + \alpha \left[ G - Q(S_\tau, A_\tau) \right] \\
&\qquad\quad \text{If } \pi \text{ is being learned, ensure } \pi(\cdot \mid S_\tau) \text{ is } \varepsilon\text{-greedy w.r.t. } Q \\
&\quad \text{Until } \tau = T - 1
\end{align}
$$

---

## n-step Off-policy Learning

$$
\begin{align}
V_{t+n}(S_t) &\doteq V_{t+n-1}(S_t) + \alpha \rho_{t:t+n-1} \left[ G_{t:t+n} - V_{t+n-1}(S_t) \right], \quad 0 \leq t < T\\
Q_{t+n}(S_t, A_t) &\doteq Q_{t+n-1}(S_t, A_t) + \alpha \rho_{t+1:t+n} \left[ G_{t:t+n} - Q_{t+n-1}(S_t, A_t) \right]

\end{align}
$$

==Giả mã==:
$$
\begin{align*}
&\textbf{Input: } \text{behavior policy } b \text{ s.t. } b(a|s) > 0,\ \forall s \in \mathcal{S},\ a \in \mathcal{A} \\
&\text{Initialize } Q(s,a)\ \text{arbitrarily},\ \forall s \in \mathcal{S},\ a \in \mathcal{A} \\
&\text{Initialize } \pi \text{ to be greedy w.r.t. } Q \text{ or fixed} \\
&\text{Set step size } \alpha \in (0,1],\ \text{positive integer } n \\
&\text{Use cyclic buffer of size } n+1 \text{ for } S_t,\ A_t,\ R_t \\[1ex]

&\textbf{Loop for each episode:} \\
&\quad \text{Initialize and store } S_0 \ne \text{terminal} \\
&\quad \text{Select and store } A_0 \sim b(\cdot|S_0) \\
&\quad T \leftarrow \infty \\

&\quad \textbf{Loop for } t = 0,1,2,\dots: \\
&\quad\quad \textbf{If } t < T: \\
&\quad\quad\quad \text{Take action } A_t \\
&\quad\quad\quad \text{Observe and store } R_{t+1},\ S_{t+1} \\
&\quad\quad\quad \textbf{If } S_{t+1} \text{ is terminal:} \\
&\quad\quad\quad\quad T \leftarrow t + 1 \\
&\quad\quad\quad \textbf{else:} \\
&\quad\quad\quad\quad \text{Select and store } A_{t+1} \sim b(\cdot | S_{t+1}) \\

&\quad\quad \tau \leftarrow t - n + 1 \quad \text{(time to update)} \\[1ex]

&\quad\quad \textbf{If } \tau \ge 0: \\
&\quad\quad\quad \rho \leftarrow \prod_{i=\tau+1}^{\min(\tau+n-1,T-1)} \frac{\pi(A_i|S_i)}{b(A_i|S_i)} \\
&\quad\quad\quad G \leftarrow \sum_{i=\tau+1}^{\min(\tau+n,T)} \gamma^{i-\tau-1} R_i \\
&\quad\quad\quad \textbf{If } \tau+n < T: \\
&\quad\quad\quad\quad G \leftarrow G + \gamma^n Q(S_{\tau+n}, A_{\tau+n}) \\
&\quad\quad\quad Q(S_\tau, A_\tau) \leftarrow Q(S_\tau, A_\tau) + \alpha \rho \left( G - Q(S_\tau, A_\tau) \right) \\

&\quad\quad\quad \textbf{If } \pi \text{ is being learned:} \\
&\quad\quad\quad\quad \pi(\cdot | S_\tau) \leftarrow \text{greedy w.r.t. } Q \\

&\textbf{Until } \tau = T - 1
\end{align*}
$$

---

## Per-decision Methods with Control Variates

Tương tự như Monte Carlo method, ta cũng có một số cách để giảm variance.
Đầu tiên phải nhắc lại công thức tính $V$ và $Q$:

$$
\begin{align}
V_\pi(s) &= \mathbb{E}_\pi \left[ G_t \mid S_t = s \right] \\
Q_\pi(s, a) &= \mathbb{E}_\pi \left[ G_t \mid S_t = s, A_t = a \right] 
\end{align}
$$
Tức là dù tính kiểu gì, công thức nào thì cũng là biến đổi của công thức trên, tức là tính dựa vào return $G_t$.  Trước tiên ta viết lại $G_t$ theo công thức đệ quy:

$$
\begin{align}
G_{t:h} = R_{t+1} + \gamma G_{t+1:h}, \qquad t < h < T,
\end{align}
$$

Tiếp đến ta có công thức biến đổi để giảm variance như sau:
$$
G_{t:h} \doteq \rho_t \left( R_{t+1} + \gamma G_{t+1:h} \right) + (1 - \rho_t) V_{h-1}(S_t), \qquad t < h < T,
$$
Ở đây có thêm term $(1 - \rho_t) V_{h-1}(S_t)$. Hãy nghĩ đến trường hợp $\rho_t = 0$ (tức là $\pi(A_t \mid S_t) = 0$ trong khi $b(A_t \mid S_t) \neq 0$)  thì công thức ban đầu sẽ trả về $0$. Bằng cách thêm term kia vào, chúng ta vẫn có thể trả về được kết quả ước tính trước đó bằng $V$. Điều này còn thêm một khả năng nữa là khi $\rho = 0$, ta vẫn có thể tiếp tục trả về kết quả mà ko cần ngắt thuật toán ngay (mình ko chắc nhưng có suy ra được cái này).

Quay trở lại với mục đích ban đầu của chúng ta là giảm variance cho việc tính $V$, nhắc lại công thức là:
$$
V_{t+n}(S_t) \doteq V_{t+n-1}(S_t) + \alpha \left[ G_{t:t+n} - V_{t+n-1}(S_t) \right], \qquad 0 \leq t < T,
$$
Rồi giờ cắm cái $G_{t:t+n}$ bên trên vào công thức là được, ok rồi đó.


Tiếp đến là công thức giảm variance cho việc tính $Q$. Do action đầu tiên trong $Q(S_t, A_t)$ được cố định, ta cần một công thức hơi khác một xíu:
$$
\begin{align*}
G_{t:h} &\doteq R_{t+1} + \gamma \left( \rho_{t+1} G_{t+1:h} + \bar{V}_{h-1}(S_{t+1}) - \rho_{t+1} Q_{h-1}(S_{t+1}, A_{t+1}) \right), \\
&= R_{t+1} + \gamma \rho_{t+1} \left( G_{t+1:h} - Q_{h-1}(S_{t+1}, A_{t+1}) \right) + \gamma \bar{V}_{h-1}(S_{t+1}), \qquad t < h \leq T.
\end{align*}
$$

Chứng minh công thức này bằng cách chứng mình rằng:
$$
\begin{align}
\mathbb{E}\left[\gamma \left( \ \bar{V}_{h-1}(S_{t+1}) - \rho_{t+1} Q_{h-1}(S_{t+1}, A_{t+1}) \right)\right] = 0
\end{align}
$$
Cái này ko khó lắm nhưng mà do kí hiệu $\mathbb{E}$ được trình bày hơi vắn tắt, để ý tí là được.

---

## Off-policy Learning Without Importance Sampling

Có thể dùng off-policy mà không cần importance sampling không? Q-learning và Expected-Sarsa cho chúng ta câu trả lời là có, nhưng ở trường hợp 1 step. Vậy nếu n-step thì có làm được không? 

Có chứ, mà cách này hơi tốn tài nguyên.
![[Pasted image 20250712011040.png]]
Biểu đồ trên là một ví dụ để cách tính này dễ hình dung hơn. Ta vẫn sẽ tính $Q$ ở mỗi level, tuy nhiên những action không được chọn mới được đóng góp vào tổng cuối cùng, action được chọn thì đem hệ số $\pi(A_t \mid S_t)$ nhân xuống lần gọi đệ quy tiếp theo. Ở level cuối, mọi action đều được đóng góp vào tổng (kể cả action được chọn). Công thức bên dưới đây ví dụ cho trường hợp có 2 level (lưu ý hình trên là 3 level):

$$
\begin{align*}
G_{t:t+2} &\doteq R_{t+1} + \gamma \sum_{a \neq A_{t+1}} \pi(a \mid S_{t+1}) Q_{t+1}(S_{t+1}, a) \\
&\quad + \gamma \pi(A_{t+1} \mid S_{t+1}) \left( R_{t+2} + \gamma \sum_{a} \pi(a \mid S_{t+2}) Q_{t+1}(S_{t+2}, a) \right) \\
&= R_{t+1} + \gamma \sum_{a \neq A_{t+1}} \pi(a \mid S_{t+1}) Q_{t+1}(S_{t+1}, a) 
+ \gamma \pi(A_{t+1} \mid S_{t+1}) G_{t+1:t+2},
\end{align*}
$$

==Một vài điều lưu ý==:
- Phương pháp này vẫn là off policy, tức là $A_i$ được lấy từ behavior policy $b$, tuy nhiên phần xác suất thì lấy từ $\pi$ thế nên mới không cần importance sampling.
- Lý do cho việc current action node của mỗi level (trừ level cuối) không được tính vào tổng là để tránh việc cộng trùng lặp.

---

## A Unifying Algorithm: n-step Q($\sigma$)

![[Pasted image 20250712012259.png]]
Chúng ta có thể tổng quát hóa Q learning bằng $\sigma$, tức là liệu có hay không ta muốn sample các action khác. Với ý tưởng này, $\sigma \in \{  0, 1 \}$   , tuy nhiên ta cũng có thể tổng quát thêm nữa bằng cách đưa ra công thức với $\sigma$ có thể nằm trong khoảng $[0, 1]$.

Đầu tiên là
$$
\begin{align*}
G_{t:h} &= R_{t+1} + \gamma \sum_{a \ne A_{t+1}} \pi(a \mid S_{t+1}) Q_{h-1}(S_{t+1}, a) 
+ \gamma \pi(A_{t+1} \mid S_{t+1}) G_{t+1:h} \\
&= R_{t+1} + \gamma \bar{V}_{h-1}(S_{t+1}) 
- \gamma \pi(A_{t+1} \mid S_{t+1}) Q_{h-1}(S_{t+1}, A_{t+1}) 
+ \gamma \pi(A_{t+1} \mid S_{t+1}) G_{t+1:h} \\
&= R_{t+1} + \gamma \pi(A_{t+1} \mid S_{t+1}) \left( G_{t+1:h} - Q_{h-1}(S_{t+1}, A_{t+1}) \right) 
+ \gamma \bar{V}_{h-1}(S_{t+1}),
\end{align*}
$$

Thêm phần switch bằng $\sigma$ vào là ta có công thức:

$$
\begin{equation}
G_{t:h} \doteq R_{t+1} + \gamma \left( \sigma_{t+1} \rho_{t+1} + (1 - \sigma_{t+1}) \pi(A_{t+1} \mid S_{t+1}) \right)
\left( G_{t+1:h} - Q_{h-1}(S_{t+1}, A_{t+1}) \right)
+ \gamma \bar{V}_{h-1}(S_{t+1}),
\tag{7.17}
\end{equation}
$$
 
---

## Planning and learning: Dyna Q

Cho tới giờ các phương pháp cho tới giờ mà chúng ta tìm hiểu có thể phân ra làm hai loại, một là yêu cầu dynamics của environment như dynamic programming , hai là không cần dùng đến dynamics như Monte Carlo hoặc TD.

- Model: bất kể thứ gì mà mô hình dùng để dự đoán cách mà môi trường phản ứng.
- Planning: dùng mô phỏng nội bộ
- Learning: tương tác thực tế với môi trường
![[Pasted image 20250712031837.png]]

![[Pasted image 20250712032425.png]]

Ngạc nhiên là với planning, agent học nhanh hơn nhiều. Mình từng nghĩ điều này là không thể vì khi ước tính model, một nguồn sai số nữa đã được thêm vào và cho dù tận dụng được nhiều dữ liệu (giả lập dữ liệu môi trường bằng model) tới đâu thì kết quả ra vẫn là sai. Tuy nhiên các thực nghiệm nghiệm trong thực tế (ít nhất là trong sách) cho ra một kết quả rất tích cực.

---

## Prioritized sweeping

Có thể bạn đã nhận ra, thứ tự cập nhật state cũng quan trọng trong việc tối ưu thời gian học. Ví dụ ở episode đầu, chỉ có state kế cuối là có value function được update khác đi so với giá trị ban đầu, do đó (ông nào học cp mà không biết cái này thì vừa tội) thì update ngược là một ý tưởng hay. Tuy nhiên, tác giả không muốn ý tưởng sweeping trên bị gán cố định với một state cụ thể nào cả, vậy nên bất kì state nào bị thay đổi so quá một giá trị $\theta$ nào đó, ta sẽ đưa nó vào hàng đợi.
$$
\begin{align*}
&\text{Initialize } Q(s, a), \text{Model}(s, a) \text{ for all } s, a, \text{ and } PQueue \text{ to empty} \\
&\textbf{Loop forever:} \\
&\quad (a) \quad S \leftarrow \text{current (nonterminal) state} \\
&\quad (b) \quad A \leftarrow \text{policy}(S, Q) \\
&\quad (c) \quad \text{Take action } A; \text{ observe reward } R \text{ and state } S' \\
&\quad (d) \quad \text{Model}(S, A) \leftarrow R, S' \\
&\quad (e) \quad P \leftarrow \left| R + \gamma \max_a Q(S', a) - Q(S, A) \right| \\
&\quad (f) \quad \text{if } P > \theta, \text{ insert } (S, A) \text{ into } PQueue \text{ with priority } P \\
&\quad (g) \quad \text{Loop repeat } n \text{ times, while } PQueue \text{ not empty:} \\
&\qquad S, A \leftarrow \text{first}(PQueue) \\
&\qquad R, S' \leftarrow \text{Model}(S, A) \\
&\qquad Q(S, A) \leftarrow Q(S, A) + \alpha \left[ R + \gamma \max_a Q(S', a) - Q(S, A) \right] \\
&\qquad \text{Loop for all } \bar{S}, \bar{A} \text{ predicted to lead to } S: \\
&\qquad\quad \bar{R} \leftarrow \text{predicted reward for } \bar{S}, \bar{A}, S \\
&\qquad\quad P \leftarrow \left| \bar{R} + \gamma \max_a Q(S, a) - Q(\bar{S}, \bar{A}) \right| \\
&\qquad\quad \text{if } P > \theta \text{ then insert } (\bar{S}, \bar{A}) \text{ into } PQueue \text{ with priority } P
\end{align*}
$$

==Note==:
- Update $Q$ và $V$ dựa trên policy ($\pi$ hay $b$) đều được gọi là trajectory sampling và nó update dựa trên trajectory hiện tại.

--- 

## Miscellaneous

- Real-time DP khác DP chỗ DP quét qua toàn bộ trạng thái để update trong khi real-time DP chỉ update dựa trên trajectory hiện tại (có dùng model dynamics)
- Planning có hai kiểu, một là background, hai là decision time. Decision time dùng khi chỉ có value function là tính được, sau đó action sẽ được chọn dựa vào việc so sánh giữa chúng dựa trên model dynamics.
