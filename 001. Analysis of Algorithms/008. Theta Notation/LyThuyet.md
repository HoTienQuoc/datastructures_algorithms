# Theta Notation ($\Theta$) — Lý thuyết chi tiết + Ví dụ

## 1. Theta là gì?

**Theta notation** (ký hiệu $\Theta$ lớn) là công cụ mô tả **cận chặt tiệm cận** của một hàm — trả lời câu hỏi:

> "Hàm $f(n)$ có **cùng bậc tăng** với hàm $g(n)$ khi $n \to \infty$ không?"

Nói cách khác, $\Theta$ cho biết $f$ **bị chặn cả trên lẫn dưới** bởi $g$ (sai khác hằng số) — tức là $f$ và $g$ **tăng cùng tốc độ** về mặt tiệm cận.

### Trực giác

$$
f(n) = \Theta(g(n)) \quad \Longleftrightarrow \quad f \text{ và } g \text{ cùng bậc tăng}
$$

**Loại suy số thực:** $\Theta$ giống như dấu $=$ trong so sánh số — nhưng là "bằng **về bậc**", không phải bằng chính xác.

**Ví dụ trực giác:** $3n^2 + 5n + 7$ và $n^2$ **cùng bậc** vì cả hai đều tăng theo bình phương khi $n$ lớn. Hằng số 3 và các số hạng thấp hơn không ảnh hưởng đến **tốc độ tăng**.

> **Lý thuyết bên cạnh:** $\Theta$ là ký hiệu **mạnh nhất** trong bộ ba $O, \Omega, \Theta$ — vì nó cho biết **đúng bậc**, không chỉ cận trên hay cận dưới. Khi nói "thuật toán có độ phức tạp $\Theta(f)$", ta đang nói **chính xác** về tốc độ tăng (sai khác hằng số). Đây là tuyên bố mạnh và hữu ích nhất — nhưng cũng **khó chứng minh nhất** (cần cả hai chiều).

---

## 2. Định nghĩa hình thức

Cho $f(n)$ và $g(n)$ là các hàm xác định trên số nguyên dương, **không âm** với $n$ đủ lớn.

$$
\boxed{f(n) = \Theta(g(n)) \iff \exists\, c_1, c_2 > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le c_1\,g(n) \le f(n) \le c_2\,g(n)}
$$

**Đọc là:** "$f(n)$ là theta của $g(n)$" hoặc "$f(n)$ thuộc $\Theta(g(n))$".

### Phân tích các thành phần

| Thành phần                         | Ý nghĩa                                |
| ---------------------------------- | -------------------------------------- |
| $\exists c_1, c_2 > 0$             | Tồn tại **hai** hằng số nhân dương     |
| $\exists n_0 > 0$                  | Tồn tại **một** ngưỡng kích thước      |
| $\forall n \ge n_0$                | Với **mọi** $n$ từ ngưỡng trở lên      |
| $c_1\,g(n) \le f(n) \le c_2\,g(n)$ | $f$ bị **kẹp** giữa hai bội số của $g$ |

### Điểm mấu chốt

1. **Hai hằng số $c_1, c_2$** — khác với $O$ (một hằng số) và $\Omega$ (một hằng số).
2. **Kẹp cả hai chiều** — $f$ vừa bị chặn trên, vừa bị chặn dưới bởi $g$.
3. **$c_1$ và $c_2$ không phụ thuộc $n$** — cố định sau khi chọn.
4. **$n_0$ cho phép bỏ qua $n$ nhỏ** — nơi hằng số và số hạng thấp áp đảo.

### Định nghĩa tương đương qua $O$ và $\Omega$

$$
\boxed{f(n) = \Theta(g(n)) \iff f(n) = O(g(n)) \text{ và } f(n) = \Omega(g(n))}
$$

**Đây là định nghĩa thực dụng nhất** — chứng minh $\Theta$ bằng cách chứng minh **hai** bất đẳng thức riêng biệt.

### Ký hiệu tập hợp (chính xác hơn)

$$
\Theta(g(n)) = O(g(n)) \cap \Omega(g(n))
$$

Chính xác phải viết $f(n) \in \Theta(g(n))$, nhưng quy ước phổ biến dùng $=$.

### Phủ định (để chứng minh $f \ne \Theta(g)$)

$$
f(n) \ne \Theta(g(n)) \iff f(n) \ne O(g(n)) \text{ hoặc } f(n) \ne \Omega(g(n))
$$

**Ý nghĩa:** Chỉ cần **một** trong hai chiều sai là đủ.

---

## 3. So sánh Theta với Big O và Omega

### 3.1. Bảng so sánh trực tiếp

|                   | $O$                  | $\Omega$          | $\Theta$                  |
| ----------------- | -------------------- | ----------------- | ------------------------- |
| Bất đẳng thức     | $f \le c_2\,g$       | $f \ge c_1\,g$    | $c_1\,g \le f \le c_2\,g$ |
| Số hằng số        | **1**                | **1**             | **2**                     |
| Ý nghĩa           | Cận trên             | Cận dưới          | Cận **chặt**              |
| Loại suy          | $\le$                | $\ge$             | $=$ (bậc)                 |
| Khi nào dùng      | Không biết chính xác | Chỉ cần tối thiểu | **Biết đúng bậc**         |
| Độ mạnh           | Yếu                  | Yếu               | **Mạnh nhất**             |
| Độ khó chứng minh | Dễ                   | Dễ                | **Khó** (cần 2 chiều)     |

### 3.2. Quan hệ tập hợp

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

**Minh họa Venn:**

```
        O(g)
    ┌───────────────┐
    │               │
    │   ┌───────┐   │
    │   │ Θ(g)  │   │
    │   └───────┘   │
    │               │
    └───────────────┘
        Ω(g)
```

### 3.3. Cùng một hàm, ba cách nói

Xét $f(n) = 5n^2 + 3n + 7$:

| Tuyên bố          | Đúng? | Chặt?           |
| ----------------- | ----- | --------------- |
| $f = O(n^2)$      | ✅     | ✅               |
| $f = O(n^3)$      | ✅     | ❌ (lỏng)        |
| $f = \Omega(n^2)$ | ✅     | ✅               |
| $f = \Omega(n)$   | ✅     | ❌ (lỏng)        |
| $f = \Theta(n^2)$ | ✅     | ✅ **chặt nhất** |

**Nhận xét:** $\Theta(n^2)$ là tuyên bố **mạnh nhất** — nó bao hàm cả $O(n^2)$ và $\Omega(n^2)$.

### 3.4. Tại sao Theta quan trọng?

**$\Theta$ trả lời câu hỏi chính xác nhất:** "Thuật toán này chạy **đúng** bao lâu?"

- $O$: "không chậm hơn" — có thể nhanh hơn nhiều.
- $\Omega$: "không nhanh hơn" — có thể chậm hơn nhiều.
- $\Theta$: "**đúng** bậc" — cả hai chiều đều chặt.

**Trong thực tế:**

- Khi **thiết kế** thuật toán: dùng $O$ (cận trên) làm mục tiêu.
- Khi **chứng minh lower bound**: dùng $\Omega$ (cận dưới).
- Khi **so sánh** thuật toán: dùng $\Theta$ (đúng bậc) để biết chúng có tương đương không.

> **Lý thuyết bên cạnh:** Trong các bài báo khoa học, người ta thường viết $O$ một cách lỏng lẻo khi thực chất là $\Theta$. Ví dụ: "Merge Sort có độ phức tạp $O(n\log n)$" — thực ra là $\Theta(n\log n)$ (vì nó cũng là $\Omega(n\log n)$). Khi đọc, cần phân biệt ngữ cảnh. Khi **viết** phân tích chính xác, nên dùng $\Theta$ nếu biết đúng bậc.

---

## 4. Ý nghĩa thực tiễn của Theta

### 4.1. Theta = "đúng bậc"

Khi nói $T(n) = \Theta(n^2)$, ta đang nói:

- $T(n)$ **không bao giờ** vượt quá $c_2 n^2$ (với $n$ đủ lớn).
- $T(n)$ **không bao giờ** thấp hơn $c_1 n^2$ (với $n$ đủ lớn).
- Tức là $T$ và $n^2$ **tăng cùng tốc độ** — chỉ khác hằng số.

### 4.2. Tại sao cần cả hai chiều?

**Ví dụ:** Xét hai thuật toán:

- A: $T_A(n) = n^2$ — thực sự bậc hai.
- B: $T_B(n) = n$ — thực sự tuyến tính.

Nếu chỉ dùng $O$:

- A là $O(n^2)$ ✅.
- B cũng là $O(n^2)$ ✅ (vì $n \le n^2$).

**$O$ không phân biệt được A và B** — cả hai đều $O(n^2)$. Nhưng thực tế A chậm hơn B rất nhiều.

Nếu dùng $\Theta$:

- A là $\Theta(n^2)$ ✅.
- B là $\Theta(n)$ — **không** phải $\Theta(n^2)$ ❌.

**$\Theta$ phân biệt được** — đây là lý do $\Theta$ mạnh hơn.

### 4.3. Theta = "tối ưu đến hằng số"

Nếu $T(n) = \Theta(f(n))$ và $f$ là lower bound của bài toán, thì thuật toán là **tối ưu về tiệm cận** — không thể cải thiện bậc.

**Ví dụ:** Merge Sort là $\Theta(n\log n)$, và comparison sort có lower bound $\Omega(n\log n)$. Vậy Merge Sort là **tối ưu** — không thuật toán so sánh nào có thể nhanh hơn về bậc.

> **Lý thuyết bên cạnh:** Trong thực tế, "tối ưu đến hằng số" chưa đủ — hằng số quan trọng với $n$ vừa. Nhưng về mặt **lý thuyết tiệm cận**, $\Theta$ + lower bound = tối ưu. Đây là kết luận mạnh nhất có thể đạt được với asymptotic analysis.

---

## 5. Các lớp Theta thường gặp

### 5.1. Bảng chi tiết

| Theta               | Tên            | Ví dụ thuật toán                           |
| ------------------- | -------------- | ------------------------------------------ |
| $\Theta(1)$         | Hằng số        | Truy cập mảng `A[i]`                       |
| $\Theta(\log n)$    | Logarit        | Binary Search (worst)                      |
| $\Theta(\sqrt{n})$  | Căn            | Kiểm tra số nguyên tố (thử đến $\sqrt{n}$) |
| $\Theta(n)$         | Tuyến tính     | Linear Search (worst), Find Max            |
| $\Theta(n\log n)$   | Tuyến tính-log | Merge Sort, Heap Sort                      |
| $\Theta(n^2)$       | Bậc hai        | Bubble Sort, Selection Sort                |
| $\Theta(n^3)$       | Bậc ba         | Nhân ma trận naive                         |
| $\Theta(2^n)$       | Hàm mũ         | Subset Sum brute force                     |
| $\Theta(n!)$        | Giai thừa      | TSP brute force                            |
| $\Theta(\varphi^n)$ | Tỉ lệ vàng     | Fibonacci naive ($\varphi \approx 1.618$)  |

### 5.2. Ví dụ code cho từng lớp

**$\Theta(1)$:**
```text
x = A[i] + A[j]   // luôn 2 phép, bất kể n
```
Đúng: $T(n) = 2 = \Theta(1)$.

**$\Theta(\log n)$:**
```text
i = 1
while i < n:
    i = i * 2
```
Số lần lặp: $\lfloor \log_2 n \rfloor + 1$. Chính xác là $\Theta(\log n)$.

**$\Theta(n)$:**
```text
for i = 1 to n:
    doing O(1)
```
Chính xác $n$ lần ⇒ $\Theta(n)$.

**$\Theta(n\log n)$:**
```text
for i = 1 to n:
    j = 1
    while j < n:
        doing O(1)
        j = j * 2
```
Vòng ngoài $n$, vòng trong $\log n$ ⇒ $\Theta(n\log n)$.

**$\Theta(n^2)$:**
```text
for i = 1 to n:
    for j = 1 to n:
        doing O(1)
```
Chính xác $n^2$ lần ⇒ $\Theta(n^2)$.

**$\Theta(n^3)$:**
```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n:
            doing O(1)
```
$n^3$ ⇒ $\Theta(n^3)$.

### 5.3. Bảng trực quan với $n = 10^6$

| $\Theta$          | Số phép             | Thời gian (~$10^9$ phép/s) |
| ----------------- | ------------------- | -------------------------- |
| $\Theta(1)$       | 1                   | tức thời                   |
| $\Theta(\log n)$  | ~20                 | tức thời                   |
| $\Theta(n)$       | $10^6$              | ~1 ms                      |
| $\Theta(n\log n)$ | $\sim 2 \cdot 10^7$ | ~20 ms                     |
| $\Theta(n^2)$     | $10^{12}$           | ~17 phút                   |
| $\Theta(n^3)$     | $10^{18}$           | ~31,000 năm                |
| $\Theta(2^n)$     | không tưởng         | không thể                  |

---

## 6. Cách chứng minh Theta

### 6.1. Ba phương pháp chính

**Phương pháp 1 — Chứng minh cả hai chiều (qua định nghĩa):**

1. Chứng minh $f = O(g)$: tìm $c_2, n_2$.
2. Chứng minh $f = \Omega(g)$: tìm $c_1, n_1$.
3. Kết luận $f = \Theta(g)$ với $n_0 = \max(n_1, n_2)$.

**Phương pháp 2 — Giới hạn:**

$$
L = \lim_{n\to\infty} \frac{f(n)}{g(n)}
$$

Nếu $0 < L < \infty$ ⇒ $f = \Theta(g)$.

**Phương pháp 3 — Kẹp giữa (sandwich):**

Tìm $h(n) \le f(n) \le k(n)$ với $h = \Theta(g)$ và $k = \Theta(g)$. Khi đó $f = \Theta(g)$.

### 6.2. Ví dụ 1 — $3n^2 + 5n + 7 = \Theta(n^2)$ (qua định nghĩa)

**Chiều $O$** (đã biết từ Big O): Với $n \ge 1$:

$$
3n^2 + 5n + 7 \le 3n^2 + 5n^2 + 7n^2 = 15n^2
$$

Chọn $c_2 = 15$, $n_2 = 1$.

**Chiều $\Omega$** (đã biết từ Omega): Với $n \ge 1$:

$$
3n^2 + 5n + 7 \ge 3n^2
$$

Chọn $c_1 = 3$, $n_1 = 1$.

**Kết luận:** Chọn $c_1 = 3$, $c_2 = 15$, $n_0 = 1$:

$$
3n^2 \le 3n^2 + 5n + 7 \le 15n^2, \quad \forall n \ge 1
$$

Vậy $3n^2 + 5n + 7 = \Theta(n^2)$. $\blacksquare$

### 6.3. Ví dụ 2 — $5n^3 + 2n^2 + n = \Theta(n^3)$

**Chiều $O$:** Với $n \ge 1$:

$$
5n^3 + 2n^2 + n \le 5n^3 + 2n^3 + n^3 = 8n^3
$$

Chọn $c_2 = 8$.

**Chiều $\Omega$:** Với $n \ge 1$:

$$
5n^3 + 2n^2 + n \ge 5n^3
$$

Chọn $c_1 = 5$.

**Kết luận:** $c_1 = 5$, $c_2 = 8$, $n_0 = 1$:

$$
5n^3 \le 5n^3 + 2n^2 + n \le 8n^3, \quad \forall n \ge 1
$$

Vậy $= \Theta(n^3)$. $\blacksquare$

### 6.4. Ví dụ 3 — $100n + 5 = \Theta(n)$

**Chiều $O$:** Với $n \ge 5$: $5 \le n$ ⇒ $100n + 5 \le 101n$. Chọn $c_2 = 101$, $n_2 = 5$.

**Chiều $\Omega$:** Với $n \ge 1$: $100n + 5 \ge 100n$. Chọn $c_1 = 100$, $n_1 = 1$.

**Kết luận:** $c_1 = 100$, $c_2 = 101$, $n_0 = 5$:

$$
100n \le 100n + 5 \le 101n, \quad \forall n \ge 5
$$

Vậy $= \Theta(n)$. $\blacksquare$

**Nhận xét:** $n_0 = 5$ — cần $n$ đủ lớn để hằng số 5 "không đáng kể" so với $100n$.

### 6.5. Ví dụ 4 — $n^2 + n\log n + n = \Theta(n^2)$

**Chiều $O$:** Với $n \ge 2$: $\log n \le n$ ⇒ $n\log n \le n^2$; $n \le n^2$:

$$
n^2 + n\log n + n \le n^2 + n^2 + n^2 = 3n^2
$$

Chọn $c_2 = 3$, $n_2 = 2$.

**Chiều $\Omega$:** $n^2 + n\log n + n \ge n^2$ (vì các số hạng khác dương). Chọn $c_1 = 1$, $n_1 = 1$.

**Kết luận:** $c_1 = 1$, $c_2 = 3$, $n_0 = 2$:

$$
n^2 \le n^2 + n\log n + n \le 3n^2, \quad \forall n \ge 2
$$

Vậy $= \Theta(n^2)$. $\blacksquare$

### 6.6. Ví dụ 5 — $f(n) = 2n^2 + 3n$ không phải $\Theta(n^3)$

**Phản chứng.** Giả sử $f = \Theta(n^3)$. Khi đó $f = \Omega(n^3)$: tồn tại $c > 0, n_0$:

$$
2n^2 + 3n \ge c\,n^3, \quad \forall n \ge n_0
$$

Chia $n^3$: $2/n + 3/n^2 \ge c$. Khi $n \to \infty$, vế trái $\to 0$ — không thể $\ge c > 0$. **Mâu thuẫn.**

Vậy $f \ne \Theta(n^3)$. $\blacksquare$

### 6.7. Ví dụ 6 — $n \ne \Theta(n^2)$

**Phản chứng.** Giả sử $n = \Theta(n^2)$, tức $n = \Omega(n^2)$: tồn tại $c > 0, n_0$:

$$
n \ge c\,n^2, \quad \forall n \ge n_0
$$

Chia $n$: $1 \ge c\,n$. Với $n > 1/c$, mâu thuẫn. Vậy $n \ne \Theta(n^2)$. $\blacksquare$

### 6.8. Ví dụ 7 — $n^2 + n$ không phải $\Theta(n^2 - n^3)$

**Lưu ý:** $n^2 - n^3 < 0$ với $n \ge 2$ — không phải hàm không âm. **Không áp dụng được** asymptotic notation (định nghĩa yêu cầu $f, g \ge 0$ với $n$ đủ lớn).

**Bài học:** Kiểm tra điều kiện không âm trước khi áp dụng.

> **Lý thuyết bên cạnh:** Định nghĩa $\Theta$ yêu cầu $f(n) \ge 0$ với $n \ge n_0$. Nếu hàm có thể âm (như $n^2 - n^3$ với $n \ge 2$), asymptotic notation **không áp dụng trực tiếp**. Trong phân tích thuật toán, điều này hiếm gặp vì chi phí luôn không âm — nhưng cần lưu ý khi làm việc với hàm tùy ý.

---

## 7. Chứng minh Theta bằng giới hạn

### 7.1. Công thức

Khi $g(n) > 0$ với $n$ đủ lớn, xét:

$$
L = \lim_{n \to \infty} \frac{f(n)}{g(n)}
$$

| $L$              | Kết luận                                             |
| ---------------- | ---------------------------------------------------- |
| $0 < L < \infty$ | $f = \Theta(g)$                                      |
| $L = 0$          | $f = o(g)$ ⇒ $f = O(g)$ nhưng $f \ne \Omega(g)$      |
| $L = \infty$     | $f = \omega(g)$ ⇒ $f = \Omega(g)$ nhưng $f \ne O(g)$ |
| không tồn tại    | Không kết luận được — cần xét $\limsup, \liminf$     |

**Chỉ khi $L$ là hằng số dương hữu hạn** thì mới $\Theta$.

### 7.2. Ví dụ 8 — $5n^2 + 3n = \Theta(n^2)$

$$
L = \lim_{n\to\infty} \frac{5n^2 + 3n}{n^2} = \lim_{n\to\infty}\left(5 + \frac{3}{n}\right) = 5
$$

$0 < 5 < \infty$ ⇒ $= \Theta(n^2)$. $\blacksquare$

### 7.3. Ví dụ 9 — $n^3 - 2n^2 = \Theta(n^3)$

$$
L = \lim_{n\to\infty} \frac{n^3 - 2n^2}{n^3} = \lim_{n\to\infty}\left(1 - \frac{2}{n}\right) = 1
$$

$0 < 1 < \infty$ ⇒ $= \Theta(n^3)$. $\blacksquare$

### 7.4. Ví dụ 10 — $\log_2 n = \Theta(\ln n)$

$$
L = \lim_{n\to\infty} \frac{\log_2 n}{\ln n} = \frac{1}{\ln 2} \approx 1.4427
$$

$0 < L < \infty$ ⇒ $= \Theta(\ln n)$. $\blacksquare$

**Ứng dụng:** Đây là lý do "cơ số log không quan trọng" trong phân tích tiệm cận.

### 7.5. Ví dụ 11 — $2n^2 + 3n \ne \Theta(n^3)$

$$
L = \lim_{n\to\infty} \frac{2n^2 + 3n}{n^3} = 0
$$

$L = 0$ ⇒ $2n^2 + 3n = o(n^3)$, **không phải** $\Theta(n^3)$. $\blacksquare$

### 7.6. Ví dụ 12 — $n^2 = \Theta(n^2 + n)$

$$
L = \lim_{n\to\infty} \frac{n^2}{n^2 + n} = \lim_{n\to\infty}\frac{1}{1 + 1/n} = 1
$$

$0 < 1 < \infty$ ⇒ $= \Theta(n^2 + n)$. $\blacksquare$

### 7.7. Ví dụ 13 — $n! = \Theta(n^n)$? **Không.**

Dùng xấp xỉ Stirling:

$$
n! \approx \sqrt{2\pi n}\left(\frac{n}{e}\right)^n
$$

$$
\frac{n!}{n^n} \approx \frac{\sqrt{2\pi n}}{e^n} \to 0
$$

$L = 0$ ⇒ $n! = o(n^n)$ ⇒ **không phải** $\Theta(n^n)$. $\blacksquare$

### 7.8. Ví dụ 14 — $n! = \Theta(2^n)$? **Không.**

$$
\frac{n!}{2^n} \to \infty
$$

$L = \infty$ ⇒ $n! = \omega(2^n)$ ⇒ **không phải** $\Theta(2^n)$. $\blacksquare$

> **Lý thuyết bên cạnh:** Phương pháp giới hạn **nhanh nhất** khi hàm "đẹp". Nhưng nếu giới hạn không tồn tại (hàm dao động), phải quay lại định nghĩa. Ví dụ hàm $f(n) = n(1 + \sin n)$ — giới hạn của $f/n$ không tồn tại. Ta cần dùng $\liminf$ (kết luận $\Omega$) và $\limsup$ (kết luận $O$).

---

## 8. Phương pháp "kẹp giữa" (Sandwich)

### 8.1. Ý tưởng

Để chứng minh $f = \Theta(g)$:

1. Tìm $h(n) \le f(n) \le k(n)$ với $n$ đủ lớn.
2. Chứng minh $h = \Theta(g)$ và $k = \Theta(g)$.
3. Kết luận $f = \Theta(g)$.

### 8.2. Ví dụ 15 — $\log(n!) = \Theta(n\log n)$

**Chặn trên** (với $i \le n$):

$$
\log(n!) = \sum_{i=1}^{n} \log i \le \sum_{i=1}^{n} \log n = n\log n
$$

**Chặn dưới** (chỉ lấy nửa sau):

$$
\log(n!) = \sum_{i=1}^{n} \log i \ge \sum_{i=\lceil n/2 \rceil}^{n} \log i \ge \sum_{i=\lceil n/2 \rceil}^{n} \log(n/2) = \frac{n}{2}\log(n/2)
$$

Với $n \ge 4$: $\log(n/2) = \log n - 1 \ge \frac{1}{2}\log n$. Vậy:

$$
\log(n!) \ge \frac{n}{2} \cdot \frac{1}{2}\log n = \frac{n\log n}{4}
$$

**Kẹp:** Với $n \ge 4$:

$$
\frac{n\log n}{4} \le \log(n!) \le n\log n
$$

**Kết luận:** $\log(n!) = \Theta(n\log n)$ với $c_1 = 1/4$, $c_2 = 1$, $n_0 = 4$. $\blacksquare$

### 8.3. Ví dụ 16 — $\sum_{i=1}^{n} i^2 = \Theta(n^3)$

**Chặn trên:** Mỗi số hạng $\le n^2$:

$$
\sum_{i=1}^{n} i^2 \le \sum_{i=1}^{n} n^2 = n^3
$$

**Chặn dưới:** Chỉ lấy nửa sau ($i \ge n/2$), mỗi số hạng $\ge (n/2)^2 = n^2/4$:

$$
\sum_{i=1}^{n} i^2 \ge \sum_{i=\lceil n/2 \rceil}^{n} i^2 \ge \frac{n}{2} \cdot \frac{n^2}{4} = \frac{n^3}{8}
$$

**Kẹp:** Với $n \ge 1$:

$$
\frac{n^3}{8} \le \sum_{i=1}^{n} i^2 \le n^3
$$

**Kết luận:** $\sum i^2 = \Theta(n^3)$. $\blacksquare$

**Hoặc dùng công thức chính xác:**

$$
\sum_{i=1}^{n} i^2 = \frac{n(n+1)(2n+1)}{6} = \frac{2n^3 + 3n^2 + n}{6} = \Theta(n^3)
$$

> **Lý thuyết bên cạnh:** Kỹ thuật "chặn bằng nửa sau" cực hữu ích khi tổng có số hạng tăng. Nó tránh phải tính chính xác — chỉ cần **một** cận dưới đủ tốt. Đây là cách chuẩn để chứng minh $\Omega$ cho tổng.

---

## 9. Tính chất của Theta

### 9.1. Tính phản xạ (reflexivity)

$$
f(n) = \Theta(f(n))
$$

**Chứng minh:** Chọn $c_1 = c_2 = 1$, $n_0 = 1$. $\blacksquare$

### 9.2. Tính đối xứng (symmetry)

$$
f = \Theta(g) \iff g = \Theta(f)
$$

**Chứng minh:** Nếu $c_1 g \le f \le c_2 g$ thì $(1/c_2) f \le g \le (1/c_1) f$. $\blacksquare$

**Đây là tính chất đặc biệt** — chỉ $\Theta$ có. $O$ và $\Omega$ không có (chúng có transpose, không phải symmetry).

### 9.3. Tính bắc cầu (transitivity)

Nếu $f = \Theta(g)$ và $g = \Theta(h)$ thì $f = \Theta(h)$.

**Chứng minh:** $c_1 c_1' h \le c_1 g \le f \le c_2 g \le c_2 c_2' h$. $\blacksquare$

### 9.4. Quy tắc cộng

$$
\Theta(f) + \Theta(g) = \Theta(\max(f, g))
$$

**Chứng minh:** Giống $O$ — lấy max vì $\Theta$ bao gồm cả $O$.

**Ví dụ:** $\Theta(n) + \Theta(n^2) = \Theta(n^2)$.

**Cảnh báo:** Chỉ đúng khi **số hạng tử cố định**. Nếu tổng có $n$ số hạng, mỗi $\Theta(n)$, tổng là $n \cdot \Theta(n) = \Theta(n^2)$.

### 9.5. Quy tắc nhân

$$
\Theta(f) \cdot \Theta(g) = \Theta(f \cdot g)
$$

**Ví dụ:** Vòng ngoài $\Theta(n)$, vòng trong $\Theta(n)$ ⇒ $\Theta(n^2)$.

### 9.6. Quy tắc hằng số

$$
c \cdot \Theta(f) = \Theta(f), \quad \forall c > 0
$$

### 9.7. Quan hệ với $O$ và $\Omega$

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

$$
f = \Theta(g) \Rightarrow f = O(g) \text{ và } f = \Omega(g)
$$

**Nhưng:** $f = O(g)$ **và** $f = \Omega(g)$ chỉ suy ra được $f = \Theta(g)$ khi cả hai đúng với **cùng** $g$.

### 9.8. Bảng tóm tắt

| Tính chất    | Công thức                                                |
| ------------ | -------------------------------------------------------- |
| Phản xạ      | $f = \Theta(f)$                                          |
| **Đối xứng** | $f = \Theta(g) \Leftrightarrow g = \Theta(f)$            |
| Bắc cầu      | $f = \Theta(g), g = \Theta(h) \Rightarrow f = \Theta(h)$ |
| Cộng         | $\Theta(f) + \Theta(g) = \Theta(\max(f, g))$             |
| Nhân         | $\Theta(f) \cdot \Theta(g) = \Theta(fg)$                 |
| Hằng số      | $c \cdot \Theta(f) = \Theta(f)$                          |
| Giao         | $\Theta = O \cap \Omega$                                 |

> **Lý thuyết bên cạnh:** $\Theta$ là **quan hệ tương đương** (equivalence relation) trên tập các hàm — vì nó có cả ba tính chất: phản xạ, đối xứng, bắc cầu. Điều này có nghĩa: $\Theta$ **phân hoạch** tập hàm thành các **lớp tương đương** — mỗi lớp gồm các hàm "cùng bậc". Ví dụ: $3n^2$, $5n^2 + n$, $100n^2 - \log n$ cùng thuộc lớp $\Theta(n^2)$.

---

## 10. Theta cho đệ quy

### 10.1. Master Theorem — cho kết quả $\Theta$

Với $T(n) = aT(n/b) + f(n)$, đặt $x = n^{\log_b a}$:

1. $f(n) = O(x^{1-\varepsilon})$ ⇒ $T(n) = \Theta(x)$.
2. $f(n) = \Theta(x)$ ⇒ $T(n) = \Theta(x \log n)$.
3. $f(n) = \Omega(x^{1+\varepsilon})$ (và điều kiện chính quy) ⇒ $T(n) = \Theta(f(n))$.

**Ưu điểm lớn:** Master Theorem cho **$\Theta$ trực tiếp** — không cần chứng minh hai chiều. Đây là công cụ mạnh nhất để phân tích đệ quy chia để trị.

### 10.2. Ví dụ 17 — Binary Search

$$
T(n) = T(n/2) + \Theta(1)
$$

$a = 1$, $b = 2$, $x = n^{\log_2 1} = 1$, $f(n) = \Theta(1) = \Theta(x)$ ⇒ Case 2:

$$
T(n) = \Theta(\log n)
$$

### 10.3. Ví dụ 18 — Merge Sort

$$
T(n) = 2T(n/2) + \Theta(n)
$$

$a = 2$, $b = 2$, $x = n^{\log_2 2} = n$, $f(n) = \Theta(n) = \Theta(x)$ ⇒ Case 2:

$$
T(n) = \Theta(n\log n)
$$

### 10.4. Ví dụ 19 — Nhân ma trận Strassen

$$
T(n) = 7T(n/2) + \Theta(n^2)
$$

$a = 7$, $b = 2$, $x = n^{\log_2 7} \approx n^{2.807}$. $f(n) = \Theta(n^2) = O(n^{2.807-\varepsilon})$ ⇒ Case 1:

$$
T(n) = \Theta(n^{\log_2 7}) \approx \Theta(n^{2.807})
$$

### 10.5. Ví dụ 20 — Tìm max đệ quy

$$
T(n) = 2T(n/2) + \Theta(1)
$$

$a = 2$, $b = 2$, $x = n$, $f(n) = 1 = O(n^{1-\varepsilon})$ ⇒ Case 1:

$$
T(n) = \Theta(n)
$$

**Nhận xét:** Đệ quy không cải thiện Big O so với vòng lặp — vẫn $\Theta(n)$. Đệ quy chỉ hữu ích khi có cấu trúc chia để trị thực sự (Merge Sort, Binary Search).

### 10.6. Ví dụ 21 — Fibonacci naive

$$
T(n) = T(n-1) + T(n-2) + \Theta(1)
$$

**Không áp dụng Master Theorem** (trừ, không chia). Dùng cây đệ quy:

- Số nút xấp xỉ $F_n = \Theta(\varphi^n)$ với $\varphi = (1+\sqrt{5})/2 \approx 1.618$.

$$
T(n) = \Theta(\varphi^n)
$$

**Không** phải $\Theta(2^n)$ — dù nhiều tài liệu viết $O(2^n)$ (cận trên lỏng). Cận chặt là $\varphi^n$.

> **Lý thuyết bên cạnh:** Master Theorem chỉ áp dụng cho hệ thức **chia đều** (dạng $T(n) = aT(n/b) + f(n)$). Với hệ thức **trừ** (như $T(n) = T(n-1) + \ldots$), phải dùng cây đệ quy hoặc phương pháp thế. Với hệ thức **không đều** (như $T(n) = T(n/3) + T(2n/3) + n$), dùng cây đệ quy hoặc định lý Akra–Bazzi.

---

## 11. Theta cho cấu trúc dữ liệu

### 11.1. Bảng tra nhanh (khi biết đúng bậc)

| CTDL                   | Truy cập              | Tìm              | Chèn             | Xóa              |
| ---------------------- | --------------------- | ---------------- | ---------------- | ---------------- |
| **Array**              | $\Theta(1)$           | $\Theta(n)$      | $\Theta(n)$      | $\Theta(n)$      |
| **Linked List**        | $\Theta(n)$           | $\Theta(n)$      | $\Theta(1)$*     | $\Theta(1)$*     |
| **Stack**              | $\Theta(n)$           | $\Theta(n)$      | $\Theta(1)$      | $\Theta(1)$      |
| **Queue**              | $\Theta(n)$           | $\Theta(n)$      | $\Theta(1)$      | $\Theta(1)$      |
| **BST (avg)**          | $\Theta(\log n)$      | $\Theta(\log n)$ | $\Theta(\log n)$ | $\Theta(\log n)$ |
| **BST (worst)**        | $\Theta(n)$           | $\Theta(n)$      | $\Theta(n)$      | $\Theta(n)$      |
| **AVL / Red-Black**    | $\Theta(\log n)$      | $\Theta(\log n)$ | $\Theta(\log n)$ | $\Theta(\log n)$ |
| **Heap**               | $\Theta(1)$ (min/max) | $\Theta(n)$      | $\Theta(\log n)$ | $\Theta(\log n)$ |
| **Hash Table (avg)**   | —                     | $\Theta(1)$      | $\Theta(1)$      | $\Theta(1)$      |
| **Hash Table (worst)** | —                     | $\Theta(n)$      | $\Theta(n)$      | $\Theta(n)$      |

*Chỉ khi đã có con trỏ đến vị trí.

### 11.2. Nhận xét

Hầu hết các CTDL đều có $\Theta$ chính xác (không chỉ $O$) — vì ta biết cả cận trên lẫn cận dưới:

- **Array truy cập:** $\Theta(1)$ — đúng một phép.
- **Array tìm:** $\Theta(n)$ — worst case phải duyệt hết.
- **BST cân bằng:** $\Theta(\log n)$ — cả upper lẫn lower đều $\log n$.
- **Hash table (avg):** $\Theta(1)$ — cả hai chiều đều hằng số.

**Ngoại lệ cần lưu ý:** Khi nói "BST $\Theta(\log n)$", phải nói rõ **average** hay **balanced**. BST thường (không cân bằng) có worst case $\Theta(n)$ — không phải $\Theta(\log n)$.

> **Lý thuyết bên cạnh:** Với CTDL, "$\Theta$" thường ngầm hiểu là **average case** — vì worst case có thể khác bậc (như BST). Khi cần chính xác, phải ghi rõ "worst case $\Theta(n)$" hay "average case $\Theta(\log n)$".

---

## 12. Theta cho thuật toán phổ biến

### 12.1. Sắp xếp

| Thuật toán     | Best              | Average           | Worst             |
| -------------- | ----------------- | ----------------- | ----------------- |
| Bubble Sort    | $\Theta(n)$*      | $\Theta(n^2)$     | $\Theta(n^2)$     |
| Insertion Sort | $\Theta(n)$       | $\Theta(n^2)$     | $\Theta(n^2)$     |
| Selection Sort | $\Theta(n^2)$     | $\Theta(n^2)$     | $\Theta(n^2)$     |
| **Merge Sort** | $\Theta(n\log n)$ | $\Theta(n\log n)$ | $\Theta(n\log n)$ |
| Quick Sort     | $\Theta(n\log n)$ | $\Theta(n\log n)$ | $\Theta(n^2)$     |
| **Heap Sort**  | $\Theta(n\log n)$ | $\Theta(n\log n)$ | $\Theta(n\log n)$ |
| Counting Sort  | $\Theta(n+k)$     | $\Theta(n+k)$     | $\Theta(n+k)$     |

*Bubble Sort có best $\Theta(n)$ nếu có optimization "dừng sớm".

**Quan sát:**

- **Selection Sort, Merge Sort, Heap Sort:** cả ba case **cùng bậc** ⇒ $\Theta$ duy nhất.
- **Insertion Sort, Bubble Sort:** best khác bậc ⇒ không có $\Theta$ duy nhất.
- **Quick Sort:** worst khác bậc ⇒ không có $\Theta$ duy nhất.

### 12.2. Tìm kiếm

| Thuật toán        | Best        | Average          | Worst            |
| ----------------- | ----------- | ---------------- | ---------------- |
| Linear Search     | $\Theta(1)$ | $\Theta(n)$      | $\Theta(n)$      |
| **Binary Search** | $\Theta(1)$ | $\Theta(\log n)$ | $\Theta(\log n)$ |
| Hash Search       | $\Theta(1)$ | $\Theta(1)$      | $\Theta(n)$      |

**Nhận xét:** Binary Search có average = worst = $\Theta(\log n)$ — nhưng best khác bậc ($\Theta(1)$). Vậy **không có $\Theta$ duy nhất cho mọi case**.

### 12.3. Đồ thị

| Thuật toán      | $\Theta$ (worst)      |
| --------------- | --------------------- |
| BFS             | $\Theta(V + E)$       |
| DFS             | $\Theta(V + E)$       |
| Dijkstra (heap) | $\Theta((V+E)\log V)$ |
| Bellman-Ford    | $\Theta(VE)$          |
| Floyd-Warshall  | $\Theta(V^3)$         |
| Kruskal         | $\Theta(E\log E)$     |

**Lưu ý:** Với hầu hết thuật toán đồ thị, lower bound gặp upper bound ⇒ $\Theta$ chính xác. Ngoại lệ: **nhân ma trận** (lower bound $\Omega(n^2)$, upper bound $O(n^{2.37})$) — chưa biết $\Theta$.

### 12.4. Quy hoạch động

| Bài toán           | $\Theta$          | Bộ nhớ          |
| ------------------ | ----------------- | --------------- |
| Fibonacci (tối ưu) | $\Theta(n)$       | $\Theta(1)$     |
| LCS                | $\Theta(nm)$      | $\Theta(nm)$    |
| Knapsack 0/1       | $\Theta(nW)$      | $\Theta(nW)$    |
| Matrix Chain       | $\Theta(n^3)$     | $\Theta(n^2)$   |
| Edit Distance      | $\Theta(nm)$      | $\Theta(nm)$    |
| TSP (Held-Karp)    | $\Theta(n^2 2^n)$ | $\Theta(n 2^n)$ |

---

## 13. Theta với nhiều biến

### 13.1. Khi nào có nhiều biến?

Giống $O$ và $\Omega$: đồ thị ($V, E$), ma trận ($n \times m$), chuỗi ($n, m$), knapsack ($n, W$).

### 13.2. Ví dụ — duyệt ma trận $n \times m$

```text
for i = 1 to n:
    for j = 1 to m:
        doing O(1)
```

$$
T(n, m) = \Theta(nm)
$$

**Không** phải $\Theta(n^2)$ trừ khi $m = n$.

### 13.3. Ví dụ — BFS trên đồ thị

$$
T(V, E) = \Theta(V + E)
$$

**Chứng minh:**

- **Upper:** Mỗi đỉnh vào/ra queue một lần ($O(V)$); mỗi cạnh duyệt một lần ($O(E)$).
- **Lower:** Phải thăm mọi đỉnh liên thông ($\Omega(V)$); phải duyệt mọi cạnh ($\Omega(E)$).

Vậy $\Theta(V+E)$. $\blacksquare$

### 13.4. Quy tắc biến

**Luôn viết $\Theta$ theo tất cả biến ảnh hưởng:**

- Đồ thị thưa ($E = O(V)$): $\Theta(V+E) = \Theta(V)$.
- Đồ thị dày ($E = O(V^2)$): $\Theta(V+E) = \Theta(V^2)$.

**Không** được tự ý bỏ biến nếu không có giả định.

---

## 14. Những hiểu nhầm phổ biến về Theta

### Hiểu nhầm 1 — "$\Theta$ là trung bình cộng của $O$ và $\Omega$"

**Sai.** $\Theta$ không phải trung bình — nó là **giao** của $O$ và $\Omega$:

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

Nếu $f = \Theta(g)$, thì $f = O(g)$ **và** $f = \Omega(g)$ — cùng một $g$.

### Hiểu nhầm 2 — "$\Theta$ mạnh hơn $O$ nên luôn dùng $\Theta$"

**Sai trong thực tế.** $\Theta$ chỉ tồn tại khi biết **đúng bậc**. Nếu chỉ biết cận trên (ví dụ worst case của Quick Sort có thể tệ hơn $n\log n$), không thể nói $\Theta$.

**Khi nào dùng $\Theta$:** Khi chắc chắn cả hai chiều. **Khi nào dùng $O$:** Khi chỉ cần bảo đảm cận trên.

### Hiểu nhầm 3 — "$f = \Theta(g)$ nghĩa là $f = g$"

**Sai.** $\Theta$ bỏ qua hằng số và số hạng bậc thấp:

$$
3n^2 + 5n + 7 = \Theta(n^2) \quad \text{nhưng} \quad 3n^2 + 5n + 7 \ne n^2
$$

$\Theta$ chỉ nói "cùng bậc", không nói "bằng chính xác".

### Hiểu nhầm 4 — "$\Theta$ chỉ dùng cho average case"

**Sai.** $\Theta$ áp dụng cho **mọi case** — best, average, worst — miễn là biết đúng bậc.

**Ví dụ:**

- Binary Search **worst case** $\Theta(\log n)$.
- Binary Search **best case** $\Theta(1)$.
- Merge Sort **worst case** $\Theta(n\log n)$.

### Hiểu nhầm 5 — "Nếu $f = O(g)$ và $f = \Omega(g)$ thì $f = \Theta(g)$"

**Đúng** — đây là **định nghĩa**. Nhưng cần **cùng một $g$**. Nếu $f = O(n^2)$ và $f = \Omega(n)$, không suy ra $\Theta$.

### Hiểu nhầm 6 — "$\Theta(f) + \Theta(g) = \Theta(f + g)$"

**Đúng nhưng chưa chặt.** Đúng hơn:

$$
\Theta(f) + \Theta(g) = \Theta(\max(f, g))
$$

**Ví dụ:** $\Theta(n) + \Theta(n^2) = \Theta(n^2)$, không phải $\Theta(n + n^2)$ (dù cùng nghĩa, $\Theta(n^2)$ chặt hơn).

### Hiểu nhầm 7 — "Mọi thuật toán đều có $\Theta$ duy nhất"

**Sai.** Nhiều thuật toán có best/worst khác bậc:

- Quick Sort: best $\Theta(n\log n)$, worst $\Theta(n^2)$.
- Insertion Sort: best $\Theta(n)$, worst $\Theta(n^2)$.
- Linear Search: best $\Theta(1)$, worst $\Theta(n)$.

**Khi đó không có $\Theta$ duy nhất** — phải nói rõ case.

### Hiểu nhầm 8 — "$\Theta$ yêu cầu hằng số $c_1 = 1/c_2$"

**Sai.** $c_1$ và $c_2$ **độc lập**. Chỉ cần $0 < c_1 \le c_2 < \infty$. Ví dụ $c_1 = 3$, $c_2 = 15$ — hợp lệ.

### Hiểu nhầm 9 — "$\Theta(n^2)$ mạnh hơn $\Theta(n)$"

**Sai về mặt so sánh.** Không có "mạnh hơn" — chúng là **hai lớp khác nhau**. Tùy thuật toán:

- Nếu thuật toán thực sự bậc hai: $\Theta(n^2)$ đúng.
- Nếu thuật toán thực sự tuyến tính: $\Theta(n)$ đúng.

**$\Theta$ đúng hay sai phụ thuộc vào thuật toán**, không phải "mạnh hơn" hay "yếu hơn".

### Hiểu nhầm 10 — "$\Theta$ luôn tồn tại nếu biết $O$ và $\Omega$"

**Sai.** $O$ và $\Omega$ có thể **không gặp nhau**:

- Thuật toán A: $O(n^2)$, $\Omega(n\log n)$ — chưa biết $\Theta$ (khoảng trống).
- Nhân ma trận: $O(n^{2.37})$, $\Omega(n^2)$ — chưa biết $\Theta$.

**Chỉ khi $O$ gặp $\Omega$ (cùng bậc)** mới có $\Theta$.

> **Lý thuyết bên cạnh:** Trong thực tế, hầu hết thuật toán cụ thể đều có $\Theta$ chính xác — vì ta phân tích được cả hai chiều. Ngoại lệ là các bài toán mở (nhân ma trận, so khớp chuỗi tối ưu) — nơi lower bound và upper bound chưa gặp nhau.

---

## 15. Theta trong thực tế — khi nào dùng?

### 15.1. Khi nào dùng $\Theta$?

**Dùng $\Theta$ khi:**

1. **Biết đúng bậc** — cả upper lẫn lower.
2. **So sánh thuật toán** — $\Theta$ cho biết chúng có tương đương không.
3. **Chứng minh tối ưu** — khi $\Theta$ thuật toán = lower bound bài toán.
4. **Phân tích chính xác** — khi muốn tuyên bố mạnh nhất.

### 15.2. Khi nào **không** dùng $\Theta$?

**Không dùng $\Theta$ khi:**

1. **Chỉ biết upper bound** — dùng $O$.
2. **Chỉ biết lower bound** — dùng $\Omega$.
3. **Best/worst khác bậc** — không có $\Theta$ duy nhất (phải nói rõ case).
4. **Chưa chứng minh được chiều còn lại** — dùng $O$ hoặc $\Omega$ tạm.

### 15.3. Quy trình phân tích đầy đủ với $\Theta$

1. **Xác định phép toán cơ bản.**
2. **Tính $T(n)$ chính xác hoặc chặn cả hai chiều.**
3. **Tìm $O$** (chặn trên).
4. **Tìm $\Omega$** (chặn dưới).
5. **Nếu $O = \Omega$ (cùng bậc)** ⇒ kết luận $\Theta$.
6. **Nếu khác bậc** ⇒ không có $\Theta$ duy nhất — phải nói rõ case.

### 15.4. Ví dụ thực tế — so sánh hai thuật toán

**Merge Sort** là $\Theta(n\log n)$ (mọi case).

**Quick Sort** là:

- Best: $\Theta(n\log n)$.
- Average: $\Theta(n\log n)$.
- Worst: $\Theta(n^2)$.

**So sánh:**

- Về **average**, cả hai cùng $\Theta(n\log n)$ — tương đương về bậc.
- Về **worst**, Merge Sort tốt hơn ($\Theta(n\log n)$ vs $\Theta(n^2)$).
- Nhưng Quick Sort có hằng số nhỏ hơn — thực tế nhanh hơn với dữ liệu ngẫu nhiên.

**Kết luận:** Không thể chỉ dựa vào $\Theta$ để chọn thuật toán — cần xét **case** và **hằng số**.

> **Lý thuyết bên cạnh:** $\Theta$ là tuyên bố **mạnh nhất** về tiệm cận — nhưng **không phải tất cả**. Trong thực tế, cần kết hợp $\Theta$ với:

- **Case** (best/average/worst).
- **Hằng số** (với $n$ nhỏ).
- **Bộ nhớ phụ**.
- **Tính ổn định** (stable sort).
- **Khả năng song song hóa**.

Big-O/$\Theta$/$\Omega$ chỉ là **một phần** của bức tranh.

---

## 16. So sánh Theta với các ký hiệu khác

### 16.1. Bảng tổng hợp 5 ký hiệu

| Ký hiệu  | Loại suy | Định nghĩa              | Số hằng số | Khi nào dùng    |
| -------- | -------- | ----------------------- | ---------- | --------------- |
| $O$      | $\le$    | $f \le c_2 g$           | 1          | Upper bound     |
| $\Omega$ | $\ge$    | $f \ge c_1 g$           | 1          | Lower bound     |
| $\Theta$ | $=$      | $c_1 g \le f \le c_2 g$ | 2          | **Đúng bậc**    |
| $o$      | $<$      | $f/g \to 0$             | —          | Nhỏ hơn thực sự |
| $\omega$ | $>$      | $f/g \to \infty$        | —          | Lớn hơn thực sự |

### 16.2. Quan hệ then chốt

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

$$
f = \Theta(g) \iff f = O(g) \text{ và } f = \Omega(g)
$$

$$
f = O(g) \iff g = \Omega(f) \quad \text{(transpose)}
$$

$$
f = \Theta(g) \iff g = \Theta(f) \quad \text{(symmetry — chỉ }\Theta\text{ có)}
$$

### 16.3. Sơ đồ quan hệ

```
                  O(g)
              ┌───────────────┐
              │               │
              │   ┌───────┐   │
              │   │ Θ(g)  │   │
              │   └───────┘   │
              │               │
              └───────────────┘
                  Ω(g)

         o(g) ⊂ O(g)
         ω(g) ⊂ Ω(g)
         Θ(g) = O(g) ∩ Ω(g)
```

### 16.4. Khi nào dùng ký hiệu nào?

| Mục đích                  | Ký hiệu                               |
| ------------------------- | ------------------------------------- |
| "Không chậm hơn"          | $O$                                   |
| "Không nhanh hơn"         | $\Omega$                              |
| "Đúng bậc"                | $\Theta$                              |
| "Chậm hơn thực sự"        | $o$                                   |
| "Nhanh hơn thực sự"       | $\omega$                              |
| Chứng minh tối ưu         | $\Theta$ + lower bound $\Omega$       |
| Worst case của thuật toán | $O$ (thường) hoặc $\Theta$ (nếu biết) |

---

## 17. Bài tập tự luyện

**Bài 1:** Chứng minh $n^2 + 3n + 2 = \Theta(n^2)$ bằng định nghĩa.

*Hướng:* Chọn $c_1 = 1$, $c_2 = 6$, $n_0 = 1$: $n^2 \le n^2 + 3n + 2 \le 6n^2$ với $n \ge 1$.

**Bài 2:** Chứng minh $5n^3 + 2n^2 + n = \Theta(n^3)$.

*Hướng:* $c_1 = 5$, $c_2 = 8$, $n_0 = 1$.

**Bài 3:** Chứng minh $n^2 = \Theta(n^2 + n)$ qua giới hạn.

*Hướng:* $\lim n^2/(n^2+n) = 1$ ⇒ $\Theta$.

**Bài 4:** Chứng minh $2^n \ne \Theta(n^3)$.

*Hướng:* $\lim 2^n/n^3 = \infty$ ⇒ $2^n = \omega(n^3)$ ⇒ không phải $\Theta$.

**Bài 5:** Chứng minh $n! = \Theta(n^n)$? **Không** — dùng Stirling.

*Hướng:* $\lim n!/n^n = 0$ ⇒ $n! = o(n^n)$.

**Bài 6:** Chứng minh $\log(n!) = \Theta(n\log n)$ bằng kẹp giữa.

*Hướng:* $\frac{n\log n}{4} \le \log(n!) \le n\log n$ với $n \ge 4$.

**Bài 7:** Giải $T(n) = 4T(n/2) + n$.

*Hướng:* Master Theorem: $a=4$, $b=2$, $x=n^2$, $f(n)=n=O(n^{2-\varepsilon})$ ⇒ Case 1 ⇒ $\Theta(n^2)$.

**Bài 8:** Giải $T(n) = 2T(n/2) + n^2$.

*Hướng:* $a=2$, $b=2$, $x=n$, $f(n)=n^2=\Omega(n^{1+\varepsilon})$ ⇒ Case 3 ⇒ $\Theta(n^2)$.

**Bài 9:** Tại sao Quick Sort **không có** $\Theta$ duy nhất?

*Hướng:* Best/average $\Theta(n\log n)$, worst $\Theta(n^2)$ — khác bậc. Không thể dùng một $\Theta$.

**Bài 10:** Cho thuật toán có $T(n) = O(n^2)$ và $T(n) = \Omega(n\log n)$. Có thể kết luận $\Theta$ không?

*Hướng:* **Không** — có khoảng trống giữa $\Omega(n\log n)$ và $O(n^2)$. Chưa biết $\Theta$.

---

## 18. Tóm tắt một trang

### Định nghĩa

$$
f(n) = \Theta(g(n)) \iff \exists c_1, c_2 > 0, n_0 : \forall n \ge n_0,\ 0 \le c_1 g(n) \le f(n) \le c_2 g(n)
$$

Tương đương:

$$
f = \Theta(g) \iff f = O(g) \text{ và } f = \Omega(g)
$$

### Ý nghĩa

$f$ và $g$ **cùng bậc tăng** — $f$ bị kẹp giữa hai bội số của $g$ (sai khác hằng số) khi $n$ đủ lớn.

### Vai trò

- **Tuyên bố mạnh nhất** về độ phức tạp — cho biết đúng bậc.
- **Chứng minh tối ưu** — khi $\Theta$ thuật toán = lower bound bài toán.
- **So sánh thuật toán** — biết chúng có tương đương về bậc không.

### Cách chứng minh

1. **Qua định nghĩa:** chứng minh cả $O$ và $\Omega$.
2. **Qua giới hạn:** $0 < \lim f/g < \infty$.
3. **Kẹp giữa:** tìm $h \le f \le k$ với $h, k = \Theta(g)$.
4. **Master Theorem:** cho đệ quy chia để trị — kết quả $\Theta$ trực tiếp.

### Tính chất

| Tính chất    | Công thức                                                |
| ------------ | -------------------------------------------------------- |
| Phản xạ      | $f = \Theta(f)$                                          |
| **Đối xứng** | $f = \Theta(g) \Leftrightarrow g = \Theta(f)$            |
| Bắc cầu      | $f = \Theta(g), g = \Theta(h) \Rightarrow f = \Theta(h)$ |
| Cộng         | $\Theta(f) + \Theta(g) = \Theta(\max(f, g))$             |
| Nhân         | $\Theta(f) \cdot \Theta(g) = \Theta(fg)$                 |
| Hằng số      | $c \cdot \Theta(f) = \Theta(f)$                          |
| Giao         | $\Theta = O \cap \Omega$                                 |

### Lưu ý

- $\Theta$ **không phải** trung bình của $O$ và $\Omega$ — mà là **giao**.
- $\Theta$ chỉ tồn tại khi biết **đúng bậc** — cả hai chiều.
- Nhiều thuật toán **không có** $\Theta$ duy nhất (best/worst khác bậc) — phải nói rõ case.
- $\Theta$ là **quan hệ tương đương** — phân hoạch tập hàm thành các lớp.

---

## 19. Bộ ba hoàn chỉnh — $O$, $\Omega$, $\Theta$

### 19.1. Bảng so sánh cuối

|                  | $O$            | $\Omega$          | $\Theta$                |
| ---------------- | -------------- | ----------------- | ----------------------- |
| **Loại suy**     | $\le$          | $\ge$             | $=$                     |
| **Định nghĩa**   | $f \le c_2 g$  | $f \ge c_1 g$     | $c_1 g \le f \le c_2 g$ |
| **Số hằng số**   | 1              | 1                 | **2**                   |
| **Ý nghĩa**      | Cận trên       | Cận dưới          | **Cận chặt**            |
| **Độ mạnh**      | Yếu            | Yếu               | **Mạnh nhất**           |
| **Độ khó CM**    | Dễ             | Dễ                | **Khó** (2 chiều)       |
| **Khi nào dùng** | Upper bound    | Lower bound       | **Đúng bậc**            |
| **Ví dụ**        | $n^2 = O(n^2)$ | $n^2 = \Omega(n)$ | $n^2 = \Theta(n^2)$     |

### 19.2. Quan hệ then chốt

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

$$
f = \Theta(g) \iff f = O(g) \text{ và } f = \Omega(g)
$$

### 19.3. Chiến lược phân tích

1. **Bắt đầu bằng $O$** — dễ nhất, cho bảo đảm cận trên.
2. **Thêm $\Omega$** nếu cần — để biết lower bound.
3. **Kết luận $\Theta$** nếu cả hai gặp nhau — tuyên bố mạnh nhất.
4. **Xét case** — best/average/worst có thể khác bậc.
5. **Bổ sung hằng số** — khi so sánh thuật toán cùng $\Theta$.

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Chứng minh đầy đủ** Master Theorem và các biến thể (Akra–Bazzi).
- **Phân tích $\Theta$** cho một thuật toán cụ thể (Quick Sort average, Dijkstra, DP) từng bước.
- **So sánh $\Theta$** của các thuật toán cùng bài toán (sorting, shortest path, matrix multiplication).
- **Bài tập nâng cao** về so sánh $\Theta$ phức tạp (ví dụ $n^{\log\log n}$ vs $2^{\sqrt{n}}$).
- **Ứng dụng $\Theta$** trong chứng minh tối ưu (lower bound + upper bound gặp nhau).