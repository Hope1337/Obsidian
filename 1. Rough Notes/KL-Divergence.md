
---
#### Hàm tự thông tin

$$
I(x) = -\log _bP(x)
$$
Với đơn vị là bits nếu $b=2$, nats nếu $b = e$, hoặc hartleys nếu $b = 10$

---

#### Entropy

Entropy, được kí hiệu là $H(x)$, đo lượng thông tin trung bình, hay còn gọi là **độ bất định** của một biến ngẫu nhiên
$$
H(X) = \mathbb{E}[I(x)] = -\sum_{x \in \mathcal{X}}P(x)\log _b P(x)
$$
---

#### Cross Entropy

Xét hai phân phối xác suất rời rạc $P$ và $Q$ và vector xác suất tương ứng $p = (p_1, p_2 \dots p_n)$ và $q = (q_1, q_2 \dots q_n)$. Cross Entropy được định nghĩa như sau:
$$
H(p, q) = -\sum_{i=1}^{n}p_i \log_b q_i
$$
- Cross Entropy không có tính đối xứng, tức là $H(p, q) \neq H(q, p)$. Trong các bài toán thì $H(p, q)$ được dùng với $p$ là phân bố cần dự đoán, $q$ là phân bố hiện tại mô hình đang dự đoán.
- Cross Entropy phạt rất nặng khi $p_i$ lớn nhưng $q_i$ lại nhỏ. Lý do là term $-\log_b(x)$ tăng rất nhanh khi $x$ tiến về $0$.

---

#### Kullback-Leiber divergence

$$
\begin{align}
D_{KL}(p||q) = H(p, q) - H(p) \\
\end{align}
$$
Tức là:
$$
D_{KL}(p||q) = - \sum_{i=1}^n p_i \log_b \frac{q_i}{p_i} = \sum_{i=1}^n p_i \log_b \frac{p_i}{q_i}
$$
Có một số tính chất:
- Kullback-Leiber divergence luôn luôn là một số không âm $D_{KL}(p||q) \geq 0$.
- Cực tiểu hóa Kullback-Leiber tương đương cực tiểu hóa Cross Entropy
- Kullback-Leiber không phải metric, tức là không có tính đối xứng $D_{KL}(p||q) \neq D_{KL}(q||p)$
