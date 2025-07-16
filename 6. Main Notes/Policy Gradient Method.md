
Policy Gradient Method là những phương pháp học dựa trên
$$
\begin{align}
\boldsymbol{\theta}_{t+1} = \boldsymbol{\theta}_t + \alpha \widehat{\nabla J}(\boldsymbol{\theta}_t),
\end{align}
$$
Ngoài ra, những phương pháp có học thêm value function thì được gọi là Actor-Critic method.

---
## The Policy Gradient Theorem

==Lưu ý==: Trường hợp episodic và continuing cần hai cách xử lí khác nhau, bên dưới đây là cho episodic task.

Giả sử ta có $\theta$ là tham số cho policy, để đánh giá policy này, ta định nghĩa hàm $J(\theta)$ như sau:
$$
\begin{align}
J(\boldsymbol{\theta}) \doteq v_{\pi_{\boldsymbol{\theta}}}(s_0),
\end{align}
$$
Lưu ý $s_0$ ở đây là cố định. Lúc đầu mình đọc qua mình cũng chưa hiểu tại sao lại cố định một state như vậy. Cơ mà ngồi suy nghĩ lại thì cũng hợp lí, lúc thực hiện hai phép đo, bạn sẽ muốn chúng có state ban đầu giống nhau tại mọi phép thử. Hơn nữa, nếu bạn muốn tổng các $v$ trên các state khác thì cũng được thôi, định lí policy gradient cũng có thể dễ dàng mở rộng ra cho trường hợp đó.

Để ý khi muốn lấy đạo hàm của $J$, ta cần dynamics, thức mà không dễ dàng, nếu không muốn nói là không thể có được trong thực tế. Policy gradient theorem cho ta cách tính đạo hàm của $J$ mà không cần dựa vào dynamics.

==Policy Gradient Theorem==:
$$
\begin{align*}
\nabla v_{\pi}(s) &= \nabla \left[ \sum_a \pi(a \mid s) q_{\pi}(s, a) \right] && \text{for all } s \in \mathcal{S} \quad \text{(Exercise 3.18)} \\
&= \sum_a \left[ \nabla \pi(a \mid s) q_{\pi}(s, a) + \pi(a \mid s) \nabla q_{\pi}(s, a) \right] && \text{(product rule)} \\
&= \sum_a \left[ \nabla \pi(a \mid s) q_{\pi}(s, a) + \pi(a \mid s) \nabla \sum_{s', r} p(s', r \mid s, a) (r + v_{\pi}(s')) \right] \\
&= \sum_a \left[ \nabla \pi(a \mid s) q_{\pi}(s, a) + \pi(a \mid s) \sum_{s'} p(s' \mid s, a) \nabla v_{\pi}(s') \right] && \text{(Eq. 3.4)} \\
&= \sum_a \nabla \pi(a \mid s) q_{\pi}(s, a) + \sum_{s'} \left[ \sum_a \pi(a \mid s) p(s' \mid s, a) \right] \nabla v_{\pi}(s') \\
&= \sum_a \nabla \pi(a \mid s) q_{\pi}(s, a) + \sum_{s'} P(s' \mid s, \pi) \nabla v_{\pi}(s') \\
&= \sum_{x \in \mathcal{S}} \sum_{k=0}^{\infty} \Pr(s \rightarrow x, k, \pi) \sum_a \nabla \pi(a \mid x) q_{\pi}(x, a)
\end{align*}
$$
Lặp lại quá trình unrolling, ta có tổng xác suất chuyển từ $s_0$ sang $s$ sau $k$ bước. Cũng là $\eta(s)$.
$$
\begin{align*}
\nabla J(\boldsymbol{\theta}) = \nabla v_{\pi}(s_0)
&= \sum_s \left( \sum_{k=0}^{\infty} \Pr(s_0 \rightarrow s, k, \pi) \right) \sum_a \nabla \pi(a \mid s) q_{\pi}(s, a) \\
&= \sum_s \eta(s) \sum_a \nabla \pi(a \mid s) q_{\pi}(s, a) && \text{(box page 199)} \\
&= \sum_{s'} \eta(s') \sum_s \frac{\eta(s)}{\sum_{s'} \eta(s')} \sum_a \nabla \pi(a \mid s) q_{\pi}(s, a) \\
&= \sum_{s'} \eta(s') \sum_s \mu(s) \sum_a \nabla \pi(a \mid s) q_{\pi}(s, a) \\
&\propto \sum_s \mu(s) \sum_a \nabla \pi(a \mid s) q_{\pi}(s, a) && \text{(Q.E.D.)}
\end{align*}
$$
Và đây là kết quả cuối cùng của ta:
$$
\begin{align}
\nabla J(\boldsymbol{\theta}) \propto \sum_s \mu(s) \sum_a q_{\pi}(s, a) \nabla \pi(a \mid s, \boldsymbol{\theta}),
\end{align}
$$

---

## REINFORCE: Monte Carlo Policy Gradient

Policy gradient cho ta một công thức cập nhật stochastic gradient-ascent như sau:
$$
\begin{align}
\nabla J(\boldsymbol{\theta}) 
&\propto \sum_s \mu(s) \sum_a q_{\pi}(s, a) \nabla \pi(a \mid s, \boldsymbol{\theta}) \\
&= \mathbb{E}_{\pi} \left[ \sum_a q_{\pi}(S_t, a) \nabla \pi(a \mid S_t, \boldsymbol{\theta}) \right].
\end{align}
$$
$$
\begin{align}
\boldsymbol{\theta}_{t+1} \doteq \boldsymbol{\theta}_t + \alpha \sum_a \hat{q}(S_t, a, \mathbf{w}) \nabla \pi(a \mid S_t, \boldsymbol{\theta}),
\end{align}
$$

Quá tuyệt vời. Tuy nhiên ta cần biến đổi thêm vài bước nữa vì ở trên có xuất hiện tổng các action trên toàn bộ không gian action, điều này khá tốn tài nguyên:

$$
\begin{align}
\nabla J(\boldsymbol{\theta}) 
&= \mathbb{E}_{\pi} \left[ \sum_a \pi(a \mid S_t, \boldsymbol{\theta}) q_{\pi}(S_t, a) \frac{\nabla \pi(a \mid S_t, \boldsymbol{\theta})}{\pi(a \mid S_t, \boldsymbol{\theta})} \right] \\
&= \mathbb{E}_{\pi} \left[ q_{\pi}(S_t, A_t) \frac{\nabla \pi(A_t \mid S_t, \boldsymbol{\theta})}{\pi(A_t \mid S_t, \boldsymbol{\theta})} \right] 
\quad \text{(replacing } a \text{ by the sample } A_t \sim \pi) \\
&= \mathbb{E}_{\pi} \left[ G_t \frac{\nabla \pi(A_t \mid S_t, \boldsymbol{\theta})}{\pi(A_t \mid S_t, \boldsymbol{\theta})} \right]
\quad \text{(because } \mathbb{E}_{\pi}[G_t \mid S_t, A_t] = q_{\pi}(S_t, A_t))
\end{align}
$$
Đầu tiên là ta nhân $\frac{\pi(a \mid S_t, \theta)}{\pi(a \mid S_t, \theta)}$ vào, cái mà không làm đổi kết quả. Lí do ta cần làm vậy là để $a$ trong dòng 1 thành $A_t$ trong dòng 2. Tức là ta lấy $\mathbb{E}$ thêm một lần nữa, trong công thức trên chỉ để $\mathbb{E}_{\pi}$ nên dễ nhầm lẫn, tuy nhiên nên nhớ là ở dòng 2 ta đã lấy $\mathbb{E}$ thêm một lần nữa.

Ta có công thức cập nhật sau:

$$
\begin{align}
\boldsymbol{\theta}_{t+1} \doteq \boldsymbol{\theta}_t + \alpha G_t \frac{\nabla \pi(A_t \mid S_t, \boldsymbol{\theta}_t)}{\pi(A_t \mid S_t, \boldsymbol{\theta}_t)}.
\end{align}
$$

==Giả mã==:
$$
\begin{align}
&\textbf{Input: } \text{a differentiable policy parameterization } \pi(a \mid s, \theta) \\
&\text{Algorithm parameter: step size } \alpha > 0 \\
&\text{Initialize policy parameter } \theta \in \mathbb{R}^{d'} \quad (\text{e.g., to } 0) \\
&\textbf{Loop forever (for each episode):} \nonumber \\
&\quad \text{Generate an episode } S_0, A_0, R_1, \ldots, S_{T-1}, A_{T-1}, R_T \text{ following } \pi(\cdot \mid \theta) \\
&\quad \textbf{Loop for each step of the episode } t = 0, 1, \ldots, T-1: \nonumber \\
&\qquad G \leftarrow \sum_{k=t+1}^{T} \gamma^{k - t - 1} R_k \\
&\qquad \theta \leftarrow \theta + \alpha \gamma^{t} G \nabla \ln \pi(A_t \mid S_t, \theta)
\end{align}
$$
Công thức cập nhật ở dòng cuối không đúng với ở trên lắm, tuy nhiên nó là tương đương:
$$
\nabla ln[f(x)] =  \frac{\nabla \pi(A_t \mid S_t, \theta_t)}{\pi(A_t \mid S_t, \theta_t)}
$$

Với lại công thức cập nhật có thêm $\gamma ^t$, này là do ta bao gồm thêm discounting, chứng minh khá khó với mình nên mình chịu.

---

## REINFORCE with Baseline

Chúng ta có thể thêm baseline để tăng tốc độ học:
$$
\begin{align}
\nabla J(\theta) \propto \sum_{s} \mu(s) \sum_{a} \left( q_{\pi}(s, a) - b(s) \right) \nabla \pi(a \mid s, \theta)
\end{align}
$$

Điều này đúng là do:
$$
\begin{align}
\sum_{a} b(s) \nabla \pi(a \mid s, \theta)
= b(s) \nabla \sum_{a} \pi(a \mid s, \theta)
= b(s) \nabla 1
= 0
\end{align}
$$

Do đó, ta có công thức cập nhật sau
$$
\begin{align}
\theta_{t+1} \doteq \theta_t + \alpha \left( G_t - b(S_t) \right)
\frac{\nabla \pi(A_t \mid S_t, \theta_t)}{\pi(A_t \mid S_t, \theta_t)}
\end{align}
$$
Ta hoàn toàn có thể học xấp xỉ hàm $b$ như cách ta học $v$. Vì REINFORCE dùng Monte Carlo method để học $\theta$, ta cũng có thể dùng Monte Carlo để học $v$ như một cách tự nhiên:

$$
\begin{align}
&\textbf{Input: } \text{a differentiable policy parameterization } \pi(a \mid s, \theta) \\
&\textbf{Input: } \text{a differentiable state-value function parameterization } \hat{v}(s, \mathbf{w}) \\
&\text{Algorithm parameters: step sizes } \alpha^\theta > 0, \; \alpha^w > 0 \\
&\text{Initialize policy parameter } \theta \in \mathbb{R}^{d'} \text{ and state-value weights } \mathbf{w} \in \mathbb{R}^d \quad (\text{e.g., to } 0) \\
&\textbf{Loop forever (for each episode):} \nonumber \\
&\quad \text{Generate an episode } S_0, A_0, R_1, \ldots, S_{T-1}, A_{T-1}, R_T \text{ following } \pi(\cdot \mid \theta) \\
&\quad \textbf{Loop for each step of the episode } t = 0, 1, \ldots, T-1: \nonumber \\
&\qquad G \leftarrow \sum_{k = t+1}^{T} \gamma^{k - t - 1} R_k \\
&\qquad \delta \leftarrow G - \hat{v}(S_t, \mathbf{w}) \\
&\qquad \mathbf{w} \leftarrow \mathbf{w} + \alpha^w \delta \nabla \hat{v}(S_t, \mathbf{w}) \\
&\qquad \theta \leftarrow \theta + \alpha^\theta \gamma^t \delta \nabla \ln \pi(A_t \mid S_t, \theta)
\end{align}
$$

---

## Actor–Critic Methods

Mặc dù REINFORCE-with-baseline học đồng thời policy và value function, ta không phân loại nó như actor-critic bởi vì nó không dùng bootstrapping và do đó buộc phải đợi toàn bộ episode thực hiện xong mới cập nhật.

Ta có công thức one step actor critic như sau:

$$
\begin{align}
\theta_{t+1} &\doteq \theta_t + \alpha \left( G_{t:t+1} - \hat{v}(S_t, \mathbf{w}) \right)
\frac{\nabla \pi(A_t \mid S_t, \theta_t)}{\pi(A_t \mid S_t, \theta_t)} \\
&= \theta_t + \alpha \left( R_{t+1} + \gamma \hat{v}(S_{t+1}, \mathbf{w}) - \hat{v}(S_t, \mathbf{w}) \right)
\frac{\nabla \pi(A_t \mid S_t, \theta_t)}{\pi(A_t \mid S_t, \theta_t)} \\
&= \theta_t + \alpha \delta_t \frac{\nabla \pi(A_t \mid S_t, \theta_t)}{\pi(A_t \mid S_t, \theta_t)}
\end{align}
$$
==Giả mã==:
$$
\begin{align}
&\textbf{Input: } \text{a differentiable policy parameterization } \pi(a \mid s, \theta) \\
&\textbf{Input: } \text{a differentiable state-value function parameterization } \hat{v}(s, \mathbf{w}) \\
&\text{Parameters: step sizes } \alpha^\theta > 0, \; \alpha^w > 0 \\
&\text{Initialize policy parameter } \theta \in \mathbb{R}^{d'} \text{ and state-value weights } \mathbf{w} \in \mathbb{R}^d \quad (\text{e.g., to } 0) \\
&\textbf{Loop forever (for each episode):} \nonumber \\
&\quad \text{Initialize } S \text{ (first state of episode)} \\
&\quad I \leftarrow 1 \\
&\quad \textbf{Loop while } S \text{ is not terminal (for each time step):} \nonumber \\
&\qquad A \sim \pi(\cdot \mid S, \theta) \\
&\qquad \text{Take action } A, \text{ observe } S', R \\
&\qquad \delta \leftarrow R + \gamma \hat{v}(S', \mathbf{w}) - \hat{v}(S, \mathbf{w}) \quad \text{(if } S' \text{ is terminal, then } \hat{v}(S', \mathbf{w}) \doteq 0) \\
&\qquad \mathbf{w} \leftarrow \mathbf{w} + \alpha^w \delta \nabla \hat{v}(S, \mathbf{w}) \\
&\qquad \theta \leftarrow \theta + \alpha^\theta I \delta \nabla \ln \pi(A \mid S, \theta) \\
&\qquad I \leftarrow \gamma I \\
&\qquad S \leftarrow S'
\end{align}
$$

==Giả mã== : Actor-Critic with Eligibility Traces
$$
\begin{align}
&\textbf{Input: } \text{a differentiable policy parameterization } \pi(a \mid s, \theta) \\
&\textbf{Input: } \text{a differentiable state-value function parameterization } \hat{v}(s, \mathbf{w}) \\
&\text{Parameters: trace-decay rates } \lambda^\theta \in [0,1], \; \lambda^w \in [0,1]; \text{ step sizes } \alpha^\theta > 0, \; \alpha^w > 0 \\
&\text{Initialize policy parameter } \theta \in \mathbb{R}^{d'} \text{ and state-value weights } \mathbf{w} \in \mathbb{R}^d \; (\text{e.g., to } 0) \\
&\textbf{Loop forever (for each episode):} \nonumber \\
&\quad \text{Initialize } S \text{ (first state of episode)} \\
&\quad \mathbf{z}^\theta \leftarrow \mathbf{0} \quad \text{(eligibility trace vector for } \theta \text{)} \\
&\quad \mathbf{z}^w \leftarrow \mathbf{0} \quad \text{(eligibility trace vector for } \mathbf{w} \text{)} \\
&\quad I \leftarrow 1 \\
&\quad \textbf{Loop while } S \text{ is not terminal:} \nonumber \\
&\qquad A \sim \pi(\cdot \mid S, \theta) \\
&\qquad \text{Take action } A, \text{ observe } S', R \\
&\qquad \delta \leftarrow R + \gamma \hat{v}(S', \mathbf{w}) - \hat{v}(S, \mathbf{w}) \quad \text{(if } S' \text{ is terminal, then } \hat{v}(S', \mathbf{w}) \doteq 0) \\
&\qquad \mathbf{z}^w \leftarrow \gamma \lambda^w \mathbf{z}^w + \nabla \hat{v}(S, \mathbf{w}) \\
&\qquad \mathbf{z}^\theta \leftarrow \gamma \lambda^\theta \mathbf{z}^\theta + I \nabla \ln \pi(A \mid S, \theta) \\
&\qquad \mathbf{w} \leftarrow \mathbf{w} + \alpha^w \delta \mathbf{z}^w \\
&\qquad \theta \leftarrow \theta + \alpha^\theta \delta \mathbf{z}^\theta \\
&\qquad I \leftarrow \gamma I \\
&\qquad S \leftarrow S'
\end{align}
$$
---

## Policy Gradient for Continuing Problems

Ta có thang đo đánh giá cho trường hợp continuing:
$$
\begin{align}
J(\theta) \doteq r(\pi) &\doteq \lim_{h \to \infty} \frac{1}{h} \sum_{t=1}^{h} \mathbb{E}[R_t \mid S_0, A_{0:t-1} \sim \pi] \\
&= \lim_{t \to \infty} \mathbb{E}[R_t \mid S_0, A_{0:t-1} \sim \pi] \\
&= \sum_{s} \mu(s) \sum_{a} \pi(a \mid s) \sum_{s', r} p(s', r \mid s, a) r
\end{align}
$$
Nhắc lại $\mu(s)$ là steady-state distribution:
$$
\begin{align}
\sum_{s} \mu(s) \sum_{a} \pi(a \mid s, \theta) \, p(s' \mid s, a) = \mu(s'), \quad \text{for all } s' \in \mathcal{S}.
\end{align}
$$
==Giả mã==:
$$
\begin{align}
&\textbf{Input: } \text{a differentiable policy parameterization } \pi(a \mid s, \theta) \\
&\textbf{Input: } \text{a differentiable state-value function parameterization } \hat{v}(s, \mathbf{w}) \\
&\text{Algorithm parameters: } \lambda^w \in [0,1],\; \lambda^\theta \in [0,1],\; \alpha^w > 0,\; \alpha^\theta > 0,\; \alpha_{\bar{R}} > 0 \\
&\text{Initialize } \bar{R} \in \mathbb{R} \quad (\text{e.g., to } 0) \\
&\text{Initialize state-value weights } \mathbf{w} \in \mathbb{R}^d \text{ and policy parameter } \theta \in \mathbb{R}^{d'} \quad (\text{e.g., to } 0) \\
&\text{Initialize } S \in \mathcal{S} \quad (\text{e.g., to } s_0) \\
&\mathbf{z}^w \leftarrow \mathbf{0} \\
&\mathbf{z}^\theta \leftarrow \mathbf{0} \\
&\textbf{Loop forever (for each time step):} \nonumber \\
&\quad A \sim \pi(\cdot \mid S, \theta) \\
&\quad \text{Take action } A, \text{ observe } S', R \\
&\quad \delta \leftarrow R - \bar{R} + \hat{v}(S', \mathbf{w}) - \hat{v}(S, \mathbf{w}) \\
&\quad \bar{R} \leftarrow \bar{R} + \alpha_{\bar{R}} \delta \\
&\quad \mathbf{z}^w \leftarrow \lambda^w \mathbf{z}^w + \nabla \hat{v}(S, \mathbf{w}) \\
&\quad \mathbf{z}^\theta \leftarrow \lambda^\theta \mathbf{z}^\theta + \nabla \ln \pi(A \mid S, \theta) \\
&\quad \mathbf{w} \leftarrow \mathbf{w} + \alpha^w \delta \mathbf{z}^w \\
&\quad \theta \leftarrow \theta + \alpha^\theta \delta \mathbf{z}^\theta \\
&\quad S \leftarrow S'
\end{align}
$$
Trước giờ mình cứ nghĩ Critic đóng vai trò là một baseline để criticize cho hậu quả của action chọn bởi policy. Tuy nhiên nhìn vào công thức ở trên, TD error bản thân nó cũng đóng vai trò như là một criticizer rồi, vì vậy cũng không hẳn là bắt buộc Critic phải là baseline lắm, chỉ cần ước lượng $v$ sử dụng bootstrapping là được.

==Note==: Còn phần chứng minh policy gradient cho trường hợp continuing nữa, tìm trong sách á, kết quả cũng y rang thôi.