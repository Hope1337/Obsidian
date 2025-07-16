Đây là [bản được đang trên Nature](https://www.nature.com/articles/nature14236) của paper rất nổi tiếng: [Playing Atari with Deep Reinforcement Learning](https://arxiv.org/abs/1312.5602). 

## Abstract

Bối cảnh lúc đó các thuật toán RL vẫn đang dùng hand crafted features. Nhóm tác giả đề xuất mạng deep Q-learning, học trực tiếp từ "high-dimensional sensory input" (nói tóm lại là hình ảnh đó=)))) ).  

## Gist

> Reinforcement learning is known to be unstable or even to diverge when a nonlinear function approximator such as a neural network is used to represent the action-value (also known as Q) function

Cái này liên quan đến **deadly triad** mà mình có đọc trong cuốn RL của bác Barto and Sutton, mà quên cmnr=)))):
- Function Approximation
- Bootstrapping
- Off-policy Learning
Khi ba yếu tố này gặp nhau thì các sai số trong quá trình ước lượng $Q$ bị khuếch đại lên rất nhiều, gây ra mất ổn định.

==Key ideas==: 
- ==experience replay==
- ==periodically update== (méo hiểu lắm)

-> Các nghiên cứu trước như *neural fitted Q-iteration*

#### Experience replay

Lưu trữ các experience $(s_t, a_t,r_t,s_{t+1})$ tại mỗi time step $t$ vào một data set $D$. Mỗi lần cập nhật $Q$ lấy một mini batch các experience ra train, công thức cập nhật là:
$$
L_i(\theta_i) = \mathbb{E}_{(s,a,r,s') \sim U(D)} \left[ \left( r + \gamma \max_{a'} Q(s', a'; \theta_i^-) - Q(s,a; \theta_i) \right)^2 \right]
$$
Việc lấy mẫu này là ý tưởng của ==experience replay==, loại bỏ đi sự tương quan của các điểm dữ liệu với nhau.

Lưu ý trong công thức trên có hai mạng riêng biệt là $\theta ^-$ và $\theta$, ở đây $\theta^-$  là version cũ của $\theta$, chỉ được update theo chu kì nhất định. Mục đích là để target ổn định hơn, không bị chạy linh tinh nữa. Đây là ý tưởng của ==periodically update==.

---
==Quote 1==:
>  ...illustrated by the temporal evolution of two indices of learning (the agent’s average score-per-episode and average predicted Q-values..

Khoan, average predicted Q-values được dùng để đánh giá á?

Có thể, dùng để xem xem Q-value function có đang hội tụ hay không hay đang dao động liên tục.

==Quote 2==:
> In the future, it will be important to explore the potential use of biasing the content of experience replay towards salient events

==Quote 3==:
>First, to encode a single frame we take the maximum value for each pixel colour value over the frame being encoded and the previous frame. This was necessary to remove flickering that is present in games where some objects appear only in even frames while other objects appear only in odd frames, an artefact caused by the limited number of sprites Atari 2600 can display at once.

Do giới hạn phần cứng, số lượng object hiển thị cùng lúc là không nhiều, do đó các dev thời điểm đó thực hiện trick là display object luân phiên nhau, tức là thay vì cả 10 object trong 1 frame, 5 object đầu được hiển thị trong frame 1 và 5 object sau được hiển thị trong frame 2. Giờ mình mới hiểu lí do tại sao xem lại mấy video chơi game cũ nó cứ bị nhá hình.

==Quote 4==:
> The exact architecture, shown schematically in Fig. 1, is as follows. The input to the neural network consists of an 84 3 84 3 4 image produced by the preprocessing map w. The first hidden layer convolves 32 filters of 8 3 8 with stride 4 with the input image and applies a rectifier nonlinearity31,32. The second hidden layer convolves 64 filters of 4 3 4 with stride 2, again followed by a rectifier nonlinearity. This is followed by a third convolutional layer that convolves 64 filters of 3 3 3 with stride 1 followed by a rectifier. The final hidden layer is fully-connected and consists of 512 rectifier units. The output layer is a fully-connected linear layer with a single output for each valid action. The number of valid actions varied between 4 and 18 on the games we considered.

-> mạng đơn giản đến bất ngờ

==Quote 5==: 
> We also found it helpful to clip the error term from the update $r+\gamma \max_{a'}Q(s',a';\theta_i^-) - Q(s, a;\theta_i)$ to be between $-1$ and $1$.

-> Clip loss trong các trường hợp mà variance của loss cao, có khả năng làm mất ổn định quá trình huấn luyện