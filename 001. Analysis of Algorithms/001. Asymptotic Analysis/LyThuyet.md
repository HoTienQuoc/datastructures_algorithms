# Phân tích tiệm cận (Asymptotic Analysis) — lý thuyết chi tiết + ví dụ kèm giải thích

## 1. Động cơ: tại sao cần phân tích tiệm cận?

Khi đánh giá giải thuật, ta không thể nói "giải thuật này chạy 2.3 giây" vì con số đó phụ thuộc vào:

- **Phần cứng** (CPU, RAM, cache)
- **Ngôn ngữ lập trình** và trình biên dịch
- **Hệ điều hành**, tải máy
- **Cài đặt cụ thể** (dùng mảng hay danh sách liên kết…)

Ta cần một thước đo **độc lập với máy**, chỉ phụ thuộc vào **cách tăng của thời gian chạy khi kích thước đầu vào $n$ tăng**.

> **Phân tích tiệm cận** (asymptotic analysis) là phương pháp mô tả **tốc độ tăng** (growth rate) của hàm chi phí $T(n)$ khi $n \to \infty$, bỏ qua hằng số và số hạng bậc thấp.

### Ví dụ động cơ

Giả sử:
- Giải thuật A: $T_A(n) = 100n + 5000$
- Giải thuật B: $T_B(n) = 2n^2 + 10$

Với $n$ nhỏ, A chậm hơn B. Nhưng khi $n$ lớn:

| $n$  | $T_A(n)$ | $T_B(n)$  |
| ------ | ---------- | ----------- |
| 10     | 6,000      | 210         |
| 100    | 15,000     | 20,010      |
| 1,000  | 105,000    | 2,000,010   |
| 10,000 | 1,005,000  | 200,000,010 |

A tăng **tuyến tính**, B tăng **bậc hai**. Phân tích tiệm cận nói: $T_A = \Theta(n)$, $T_B = \Theta(n^2)$, và với $n$ đủ lớn, A luôn thắng.

> **Lý thuyết bên cạnh:** Phân tích tiệm cận không nói gì về $n$ nhỏ. Với $n$ nhỏ, hằng số có thể áp đảo. Đây là lý do trong thực tế đôi khi dùng insertion sort (đơn giản, hằng số nhỏ) cho mảng nhỏ, dù nó là $O(n^2)$.

---

## 2. Ý tưởng cốt lõi: "đủ lớn" và "bỏ hằng số"

Phân tích tiệm cận dựa trên hai trực giác:

1. **Chỉ quan tâm $n$ lớn:** Khi $n \to \infty$, số hạng bậc cao nhất chi phối.
   $$
   n^2 + 1000n + 10^6 \approx n^2 \quad \text{khi } n \text{ rất lớn}
   $$

2. **Bỏ hằng số nhân:** $3n^2$ và $100n^2$ cùng "bậc" — chỉ khác hằng số, không đổi bản chất tăng trưởng.

Hình thức hóa hai trực giác này chính là **ký hiệu tiệm cận** $O, \Omega, \Theta, o, \omega$.

> **Lý thuyết bên cạnh:** Có hai "kiểu" phân tích tiệm cận: **asymptotic upper/lower bounds** (dùng $O/\Omega$) và **asymptotically tight bounds** (dùng $\Theta$). Trong thực tế người ta thường dùng $O$ một cách lỏng lẻo, nhưng về lý thuyết, $\Theta$ mới là "đúng bậc".

---

## 3. Định nghĩa hình thức của các ký hiệu

Cho $f(n), g(n)$ là các hàm xác định trên số nguyên dương, **không âm** với $n$ đủ lớn.

### 3.1. Ký hiệu $O$ (big-O) — cận trên tiệm cận

$$
f(n) = O(g(n)) \iff \exists\, c > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le f(n) \le c\,g(n)
$$

**Ý nghĩa:** với $n$ đủ lớn, $f$ **không vượt quá** $g$ sai khác một hằng số.

**Trực giác:** "$f$ lớn chậm hơn hoặc bằng $g$ về bậc."

### 3.2. Ký hiệu $\Omega$ (big-Omega) — cận dưới tiệm cận

$$
f(n) = \Omega(g(n)) \iff \exists\, c > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le c\,g(n) \le f(n)
$$

**Ý nghĩa:** với $n$ đủ lớn, $f$ **không nhỏ hơn** $g$ sai khác hằng số.

### 3.3. Ký hiệu $\Theta$ (big-Theta) — cận chặt

$$
f(n) = \Theta(g(n)) \iff f(n) = O(g(n)) \text{ và } f(n) = \Omega(g(n))
$$

Tương đương:
$$
\exists\, c_1, c_2 > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le c_1 g(n) \le f(n) \le c_2 g(n)
$$

**Ý nghĩa:** $f$ và $g$ **cùng bậc tăng**.

### 3.4. Ký hiệu $o$ (little-o) — cận trên chặt

$$
f(n) = o(g(n)) \iff \forall c>0,\ \exists n_0 : \forall n \ge n_0,\ 0 \le f(n) < c\,g(n)
$$

Tương đương:
$$
\lim_{n\to\infty} \frac{f(n)}{g(n)} = 0
$$

**Ý nghĩa:** $f$ tăng **chậm hơn thực sự** so với $g$. Ví dụ: $n = o(n^2)$.

### 3.5. Ký hiệu $\omega$ (little-omega) — cận dưới chặt

$$
f(n) = \omega(g(n)) \iff \lim_{n\to\infty} \frac{f(n)}{g(n)} = \infty
$$

**Ý nghĩa:** $f$ tăng **nhanh hơn thực sự** so với $g$. Ví dụ: $n^2 = \omega(n)$.

### Bảng so sánh nhanh

| Ký hiệu    | Quan hệ       | Trực giác                   |
| ---------- | ------------- | --------------------------- |
| $O$      | $\le$ (bậc) | $f$ không lớn hơn $g$   |
| $\Omega$ | $\ge$ (bậc) | $f$ không nhỏ hơn $g$   |
| $\Theta$ | $=$ (bậc)   | $f$ đúng bậc $g$        |
| $o$      | $<$ (bậc)   | $f$ nhỏ hơn $g$ thật sự |
| $\omega$ | $>$ (bậc)   | $f$ lớn hơn $g$ thật sự |

> **Lý thuyết bên cạnh:** Dấu "=" trong $f(n) = O(g(n))$ là **ký hiệu quy ước**, không phải đẳng thức thực sự. Chính xác hơn phải viết $f(n) \in O(g(n))$, vì $O(g)$ là một **tập hợp hàm**. Vì vậy **không** suy ra $O(g) = f$ được, và không "trừ hai vế" được.

---

## 4. Phương pháp chứng minh bằng định nghĩa (tìm $c$ và $n_0$)

### Ví dụ 1: Chứng minh $3n^2 + 5n + 7 = O(n^2)$

**Cần tìm:** $c>0, n_0$ sao cho $3n^2 + 5n + 7 \le c\,n^2$ với mọi $n \ge n_0$.

**Cách làm — "nới lỏng" các số hạng:**

Với $n \ge 1$:
- $5n \le 5n^2$
- $7 \le 7n^2$

Do đó:
$$
3n^2 + 5n + 7 \le 3n^2 + 5n^2 + 7n^2 = 15n^2
$$

Chọn $c = 15$, $n_0 = 1$. Kết luận: $3n^2+5n+7 = O(n^2)$. $\blacksquare$

> **Lý thuyết bên cạnh:** Kỹ thuật "nới lỏng" (loosening) là cách chuẩn để tìm hằng số. Ta không cần hằng số **tối ưu** — chỉ cần **tồn tại** một bộ $(c, n_0)$ là đủ.

### Ví dụ 2: Chứng minh $3n^2 + 5n + 7 = \Omega(n^2)$

**Cần tìm:** $c>0, n_0$ sao cho $3n^2 + 5n + 7 \ge c\,n^2$.

Vì $5n + 7 \ge 0$ với $n\ge 1$, ta có ngay:
$$
3n^2 + 5n + 7 \ge 3n^2
$$

Chọn $c = 3$, $n_0 = 1$. Vậy $3n^2+5n+7 = \Omega(n^2)$. $\blacksquare$

### Ví dụ 3: Kết hợp — $\Theta$

Từ hai ví dụ trên:
$$
3 \cdot n^2 \le 3n^2+5n+7 \le 15 \cdot n^2, \quad \forall n \ge 1
$$
Suy ra:
$$
3n^2 + 5n + 7 = \Theta(n^2)
$$
với $c_1 = 3$, $c_2 = 15$, $n_0 = 1$.

### Ví dụ 4: Chứng minh $n^2 \ne O(n)$

**Phản chứng.** Giả sử $n^2 = O(n)$. Khi đó tồn tại $c>0, n_0$ sao cho:
$$
n^2 \le c\,n, \quad \forall n \ge n_0
$$
Chia hai vế cho $n > 0$:
$$
n \le c, \quad \forall n \ge n_0
$$
Nhưng với $n = \max(n_0, \lceil c \rceil + 1)$, ta có $n > c$ — **mâu thuẫn**. Vậy $n^2 \ne O(n)$. $\blacksquare$

> **Lý thuyết bên cạnh:** Để chứng minh **không** phải $O$, dùng phản chứng: giả sử có $c$ và $n_0$, rồi chọn $n$ đủ lớn để phá bất đẳng thức. Đây là mẫu chứng minh rất phổ biến.

### Ví dụ 5: Chứng minh $n = o(n^2)$

Theo định nghĩa $o$:
$$
\lim_{n\to\infty} \frac{n}{n^2} = \lim_{n\to\infty} \frac{1}{n} = 0
$$
Vậy $n = o(n^2)$. $\blacksquare$

---

## 5. Phương pháp giới hạn (limit method)

Khi $g(n) > 0$ với $n$ đủ lớn, xét:
$$
L = \lim_{n \to \infty} \frac{f(n)}{g(n)}
$$

| Giá trị $L$      | Kết luận                                                                                      |
| ------------------ | --------------------------------------------------------------------------------------------- |
| $L = 0$          | $f = o(g)$, tức là $f$ nhỏ hơn $g$ thực sự. Suy ra $f = O(g)$, $f \ne \Omega(g)$.   |
| $0 < L < \infty$ | $f = \Theta(g)$.                                                                            |
| $L = \infty$     | $f = \omega(g)$, tức $f$ lớn hơn $g$ thực sự. Suy ra $f = \Omega(g)$, $f \ne O(g)$. |

Đôi khi cần dùng **quy tắc L'Hôpital** hoặc **Stirling** để tính giới hạn.

### Ví dụ 6: So sánh $n^3$ và $2^n$

$$
L = \lim_{n\to\infty} \frac{n^3}{2^n}
$$

Áp dụng L'Hôpital 3 lần (hoặc biết rằng hàm mũ tăng nhanh hơn đa thức):
$$
L = \lim_{n\to\infty} \frac{6}{(\ln 2)^3 \cdot 2^n} = 0
$$

Vậy $n^3 = o(2^n)$.

### Ví dụ 7: So sánh $n\log n$ và $n^{1.1}$

$$
\frac{n\log n}{n^{1.1}} = \frac{\log n}{n^{0.1}} \to 0
$$
Vậy $n\log n = o(n^{1.1})$.

### Ví dụ 8: So sánh $\log_2 n$ và $\ln n$

$$
\frac{\log_2 n}{\ln n} = \frac{\ln n / \ln 2}{\ln n} = \frac{1}{\ln 2} \approx 1.4427
$$
Giới hạn là hằng số khác 0, nên $\log_2 n = \Theta(\ln n)$.

> **Lý thuyết bên cạnh:** Cơ số logarit **không ảnh hưởng** đến bậc tiệm cận, vì đổi cơ số chỉ nhân với một hằng số: $\log_a n = \frac{\log_b n}{\log_b a}$. Vì vậy khi viết $O(\log n)$, cơ số không cần nêu.

---

## 6. Thứ bậc tăng trưởng (growth hierarchy)

Các hàm thường gặp, từ **chậm đến nhanh**:

$$
1 \ < \ \log\log n \ < \ \log n \ < \ \sqrt{n} \ < \ n \ < \ n\log n \ < \ n^2 \ < \ n^3 \ < \ \dots \ < \ 2^n \ < \ n! \ < \ n^n
$$

### Cách nhớ

| Nhóm            | Ví dụ               | Nhận xét                                           |
| --------------- | ------------------- | -------------------------------------------------- |
| **Hằng số**     | $O(1)$            | Không phụ thuộc $n$                              |
| **Logarit**     | $O(\log n)$       | Rất chậm; $\log n$ nhỏ hơn mọi $n^\varepsilon$ |
| **Đa thức**     | $O(n^k)$          | Chia theo bậc                                      |
| **Gần đa thức** | $O(n\log n)$      | Nhanh hơn $n$, chậm hơn $n^{1+\varepsilon}$    |
| **Mũ**          | $O(c^n)$, $c>1$ | Tăng cực nhanh                                     |
| **Giai thừa**   | $O(n!)$           | Tăng nhanh hơn mọi mũ cố định                      |

### Sự thật quan trọng

Với **mọi** hằng số $k>0$ và $\varepsilon>0$:
$$
\log^k n = o(n^\varepsilon)
$$
Nghĩa là logarit (dù lũy thừa bao nhiêu) luôn **chậm hơn** đa thức bậc dương bất kỳ.

Với mọi $c>1$ và $k>0$:
$$
n^k = o(c^n)
$$
Đa thức luôn **chậm hơn** hàm mũ.

> **Lý thuyết bên cạnh:** Vì $\log n = o(n^{0.001})$, người ta hay nói "logarit gần như hằng số" trong thực tế. Đây là lý do binary search $O(\log n)$ được coi là cực nhanh: với $n = 10^9$, $\log_2 n \approx 30$ — chỉ 30 bước.

---

## 7. Tính chất của các ký hiệu tiệm cận

### 7.1. Tính phản xạ (reflexivity)

$$
f(n) = O(f(n)), \quad f(n) = \Omega(f(n)), \quad f(n) = \Theta(f(n))
$$

### 7.2. Tính bắc cầu (transitivity)

Nếu $f = O(g)$ và $g = O(h)$ thì $f = O(h)$. Tương tự cho $\Omega, \Theta, o, \omega$.

### 7.3. Tính đối xứng (symmetry) — chỉ với $\Theta$

$$
f = \Theta(g) \iff g = \Theta(f)
$$

### 7.4. Tính phản đối xứng (transpose symmetry)

$$
f = O(g) \iff g = \Omega(f)
$$
$$
f = o(g) \iff g = \omega(f)
$$

### 7.5. Quy tắc cộng và nhân

- **Cộng:** $O(f) + O(g) = O(\max(f,g))$.
  Ví dụ: $O(n) + O(n^2) = O(n^2)$.

- **Nhân:** $O(f) \cdot O(g) = O(fg)$.
  Ví dụ: vòng ngoài $O(n)$, vòng trong $O(n)$ → tổng $O(n^2)$.

> **Lý thuyết bên cạnh:** Quy tắc cộng chỉ đúng với **số lượng hạng tử cố định**. Nếu có $n$ hạng tử, mỗi hạng $O(n)$, tổng là $O(n^2)$ chứ không phải $O(n)$. Đây là bẫy phổ biến.

---

## 8. Các ví dụ phân tích đầy đủ

### Ví dụ 9: Hàm đa thức tổng quát

Cho:
$$
f(n) = 4n^3 + 100n^2 + 5000n + 10^6
$$

**Phân tích:** Số hạng bậc cao nhất là $4n^3$. Với $n\ge 1$:
- $100n^2 \le 100n^3$
- $5000n \le 5000n^3$
- $10^6 \le 10^6 n^3$

Nên:
$$
f(n) \le (4 + 100 + 5000 + 10^6) n^3 = 1{,}005{,}104\, n^3
$$
Và $f(n) \ge 4n^3$. Vậy:
$$
f(n) = \Theta(n^3)
$$

**Quy tắc tổng quát:** Đa thức bậc $k$ luôn là $\Theta(n^k)$ — chỉ phụ thuộc bậc cao nhất.

### Ví dụ 10: Tổng có số hạng phụ thuộc $n$

Cho:
$$
S(n) = \sum_{i=1}^{n} i^2
$$

Ta biết công thức:
$$
S(n) = \frac{n(n+1)(2n+1)}{6} = \frac{2n^3 + 3n^2 + n}{6} = \Theta(n^3)
$$

Nhưng nếu **không** biết công thức, ta có thể ước lượng:
$$
S(n) \le \sum_{i=1}^n n^2 = n \cdot n^2 = n^3
$$
Và:
$$
S(n) \ge \sum_{i=n/2}^n (n/2)^2 = (n/2) \cdot (n^2/4) = n^3/8
$$
Vậy $S(n) = \Theta(n^3)$. $\blacksquare$

> **Lý thuyết bên cạnh:** Kỹ thuật "chặn trên bằng max, chặn dưới bằng nửa sau" rất hữu ích khi tổng không có công thức đóng. Đây là cách tiếp cận chuẩn trong phân tích thuật toán.

### Ví dụ 11: Logarit lồng nhau

$$
f(n) = \log(\log n), \quad g(n) = \log n
$$

Xét:
$$
\frac{\log(\log n)}{\log n} \to 0
$$
Vậy $\log\log n = o(\log n)$.

Nhưng $\log\log n = \omega(1)$: nó vẫn tăng, chỉ rất chậm.

### Ví dụ 12: Hàm mũ so với giai thừa

So sánh $2^n$ và $n!$. Dùng **xấp xỉ Stirling**:
$$
n! \approx \sqrt{2\pi n} \left(\frac{n}{e}\right)^n
$$

Xét tỉ số:
$$
\frac{2^n}{n!} \approx \frac{2^n}{\sqrt{2\pi n}\, (n/e)^n} = \frac{(2e)^n}{\sqrt{2\pi n}\, n^n}
$$

Vì $2e \approx 5.44$ nhưng $n^n$ tăng nhanh hơn $(2e)^n$ khi $n$ lớn, tỉ số $\to 0$. Vậy:
$$
2^n = o(n!)
$$

> **Lý thuyết bên cạnh:** Xấp xỉ Stirling là công cụ chuẩn để so sánh các hàm liên quan giai thừa hoặc log của giai thừa: $\log(n!) = \Theta(n\log n)$. Đây là lý do nhiều thuật toán sắp xếp dựa trên so sánh có cận dưới $\Omega(n\log n)$.

---

## 9. Asymptotic analysis vs. Empirical analysis

| Tiêu chí              | Phân tích tiệm cận            | Đo thực nghiệm         |
| --------------------- | ----------------------------- | ---------------------- |
| **Đối tượng**         | Mô hình toán học của $T(n)$ | Chạy chương trình thật |
| **Phụ thuộc máy**     | Không                         | Có                     |
| **Dự đoán $n$ lớn** | Tốt (chính xác về bậc)        | Khó, tốn thời gian     |
| **Hằng số**           | Bỏ qua                        | Phản ánh đầy đủ        |
| **Kết quả**           | $O, \Omega, \Theta$         | Đồ thị thời gian thực  |

> **Lý thuyết bên cạnh:** Hai phương pháp **bổ sung** cho nhau. Phân tích tiệm cận cho biết xu hướng dài hạn; đo thực nghiệm cho biết hiệu năng thực tế. Một giải thuật $O(n^2)$ với hằng số cực nhỏ có thể nhanh hơn giải thuật $O(n\log n)$ với hằng số lớn khi $n$ vừa phải. Ví dụ thực tế: insertion sort vs. quicksort cho mảng nhỏ.

---

## 10. Những hiểu nhầm phổ biến

1. **"$O$ nghĩa là worst case, $\Omega$ nghĩa là best case."**
   **Sai.** $O/\Omega/\Theta$ mô tả **quan hệ giữa hai hàm**, không mô tả best/worst case. Một giải thuật có thể có worst case là $\Theta(n^2)$ và best case là $\Theta(n)$. Ta có thể nói worst case $= O(n^2)$, best case $= \Omega(n)$, nhưng bản thân ký hiệu không mang nghĩa "worst/best".

2. **"$f = O(g)$ nghĩa là $f$ luôn nhỏ hơn $g$."**
   **Sai.** Nó chỉ nói $f \le cg$ với $n$ đủ lớn. Với $n$ nhỏ, $f$ có thể lớn hơn. Ví dụ $n = O(n^2)$ nhưng $n \le n^2$ chỉ đúng khi $n \ge 1$.

3. **"$O(2n) = O(n)$ nên mọi hằng số đều bỏ được."**
   **Đúng về tiệm cận**, nhưng **cẩn thận khi so sánh cùng bậc.** $10^9 n$ và $n$ cùng $O(n)$, nhưng thực tế rất khác. Trong phân tích tiệm cận ta bỏ hằng số; trong tối ưu thực tế, hằng số quan trọng.

4. **"$O(\log n)$ luôn nhanh hơn $O(n)$."**
   **Đúng về xu hướng khi $n$ lớn.** Nhưng nếu $\log n$ là $\log_2 n$ với $n=10^9$ thì $\log n \approx 30$, còn $n = 10^9$ — chênh lệch khổng lồ. Tuy nhiên với $n$ cực nhỏ, hằng số có thể đảo ngược.

5. **"$f = O(g)$ và $f = O(h)$ thì $g = \Theta(h)$."**
   **Sai.** $n = O(n^2)$ và $n = O(n^3)$, nhưng $n^2 \ne \Theta(n^3)$. $O$ chỉ là cận trên, không phải "đúng bậc".

6. **"Tổng của $O(n)$ lặp $n$ lần vẫn là $O(n)$."**
   **Sai.** Đó là $n \times O(n) = O(n^2)$. Quy tắc cộng chỉ áp dụng cho số hạng tử cố định.

7. **"$\Theta$ và $O$ giống nhau."**
   **Sai.** $O$ là cận trên; $\Theta$ là cận chặt. $n = O(n^2)$ nhưng $n \ne \Theta(n^2)$.

> **Lý thuyết bên cạnh:** Ký hiệu tiệm cận thường bị dùng lỏng lẻo trong tài liệu kỹ thuật. Khi đọc, cần phân biệt: tác giả nói "worst case $O(n^2)$" nghĩa là cận trên; còn "thuật toán là $\Theta(n\log n)$" nghĩa là đúng bậc với mọi đầu vào đủ lớn.

---

## 11. Áp dụng vào phân tích thuật toán — ví dụ tích hợp

### Ví dụ 13: Phân tích hàm có nhiều đoạn

```text
Function F(n):
    sum = 0
    for i = 1 to n:
        sum += i
    for i = 1 to n:
        for j = 1 to n:
            sum += i * j
    return sum
```

**Phân tích từng đoạn:**
- Vòng 1: $n$ phép → $O(n)$
- Vòng 2: $n \times n = n^2$ phép → $O(n^2)$

**Tổng:** $O(n) + O(n^2) = O(n^2)$ theo quy tắc cộng (lấy max bậc).

Vì vòng 2 luôn chạy đúng $n^2$ lần, ta có thể kết luận chặt hơn: $T(n) = \Theta(n^2)$.

### Ví dụ 14: Vòng lặp với điều kiện phụ thuộc dữ liệu

```text
Function G(A, n):
    count = 0
    for i = 1 to n:
        if A[i] > 0:
            count += 1
    return count
```

Số lần thực hiện phép `count += 1` phụ thuộc dữ liệu:
- **Best case:** mảng toàn số âm → 0 lần tăng, nhưng vẫn duyệt $n$ lần → $T(n) = \Theta(n)$.
- **Worst case:** mảng toàn số dương → $n$ lần tăng, vẫn $\Theta(n)$.
- **Average case:** nửa số phần tử dương → $\Theta(n)$.

Vậy $T(n) = \Theta(n)$ cho mọi trường hợp — phần **thân if** không ảnh hưởng đến bậc tổng.

> **Lý thuyết bên cạnh:** Khi phân tích, ta cộng chi phí của **mọi** thao tác, kể cả thao tác không chạy. Ở đây, vòng `for` luôn chạy $n$ lần bất kể dữ liệu — nên $T(n) = \Theta(n)$ dù nhánh trong có chạy hay không. Đây là ví dụ cho thấy tầm quan trọng của việc xác định **cấu trúc điều khiển**, không chỉ nội dung thân vòng lặp.

---

## 12. Tóm tắt quy trình phân tích tiệm cận

1. **Xác định biến kích thước $n$** — thường là số phần tử, số bit, kích thước ma trận…
2. **Chọn phép toán cơ bản** — phép so sánh, phép gán, phép cộng…
3. **Đếm số lần phép toán đó** theo $n$ → hàm $T(n)$.
4. **Xác định trường hợp** (best/worst/average) nếu phép đếm phụ thuộc dữ liệu.
5. **Đơn giản hóa** bằng:
   - Bỏ hằng số
   - Bỏ số hạng bậc thấp
   - Quy tắc cộng và nhân cho vòng lặp lồng nhau
6. **Chứng minh chặt chẽ** bằng:
   - Định nghĩa (tìm $c$, $n_0$)
   - Giới hạn
   - Hoặc Master Theorem cho hệ thức truy hồi
7. **Kết luận** bằng $\Theta$ (chặt nhất) nếu có thể; ngược lại dùng $O$ hoặc $\Omega$.

---

## 13. Bảng tổng hợp các quy tắc thực hành

| Tình huống                                    | Kết quả                                        |
| --------------------------------------------- | ---------------------------------------------- |
| $c \cdot f(n)$ với $c>0$                  | cùng bậc với $f(n)$                          |
| $f(n) + g(n)$                               | bậc của hàm lớn hơn                            |
| $f(n) \cdot g(n)$                           | tích hai bậc                                   |
| Vòng lặp lồng nhau $k$ tầng, mỗi tầng $n$ | $O(n^k)$                                     |
| Vòng lặp chia đôi mỗi bước                    | $O(\log n)$                                  |
| Đệ quy $T(n) = T(n/2) + O(1)$               | $O(\log n)$                                  |
| Đệ quy $T(n) = 2T(n/2) + O(n)$              | $O(n\log n)$                                 |
| Đệ quy $T(n) = T(n-1) + O(1)$               | $O(n)$                                       |
| Đệ quy $T(n) = 2T(n-1) + O(1)$              | $O(2^n)$                                     |
| $\log_a n$                                  | cùng bậc với $\log_b n$                      |
| $n^k$ với $k>0$                           | $o(c^n)$ với mọi $c>1$                     |
| $\log^k n$ với $k>0$                      | $o(n^\varepsilon)$ với mọi $\varepsilon>0$ |

---

Nếu bạn muốn, mình có thể đi sâu tiếp vào:
- **Chứng minh Master Theorem** và các biến thể (Akra–Bazzi)
- **Phân tích amortized** chi tiết (potential method, accounting method)
- **Phân tích average-case** với mô hình xác suất cụ thể (ví dụ Quick Sort)
- **Bài tập có lời giải** về tìm $c, n_0$, so sánh hàm, chứng minh tiệm cận