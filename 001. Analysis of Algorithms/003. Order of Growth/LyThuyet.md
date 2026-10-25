# Order of Growth (Bậc tăng trưởng) — Lý thuyết chi tiết + Ví dụ

## 1. Order of Growth là gì?

**Order of growth** (bậc tăng trưởng) của một hàm chi phí $T(n)$ là **tốc độ tăng của $T(n)$ khi $n \to \infty$**, sau khi đã:

- Bỏ **hằng số nhân** (constant factor)
- Bỏ **số hạng bậc thấp** (lower-order terms)

Nói cách khác, order of growth mô tả **hình dạng đường cong tăng trưởng**, không phải giá trị cụ thể.

### Ví dụ trực giác

Xét ba hàm:

$$
T_1(n) = 3n + 100, \quad T_2(n) = n^2/100, \quad T_3(n) = 2^n
$$

| $n$    | $T_1(n)$ | $T_2(n)$  | $T_3(n)$              |
| ------ | -------- | --------- | --------------------- |
| 10     | 130      | 1         | 1,024                 |
| 100    | 400      | 100       | $\approx 10^{30}$     |
| 1,000  | 3,100    | 10,000    | vô cùng lớn           |
| 10,000 | 30,100   | 1,000,000 | không tưởng tượng nổi |

- $T_1$ tăng **tuyến tính** — order $n$.
- $T_2$ tăng **bậc hai** — order $n^2$.
- $T_3$ tăng **hàm mũ** — order $2^n$.

Dù $T_2(10) = 1 < T_1(10) = 130$, khi $n$ lớn, $T_2$ vượt xa $T_1$. **Order of growth quyết định ai thắng khi $n$ lớn.**

> **Lý thuyết bên cạnh:** Order of growth **không quan tâm** đến hằng số hay giá trị nhỏ. Nó trả lời câu hỏi: "Khi $n$ tăng gấp đôi, chi phí tăng bao nhiêu lần?" — đây là **bản chất** của thuật toán, độc lập với máy.

---

## 2. Tại sao phải dùng order of growth?

### 2.1. Vấn đề với thời gian chính xác

Nếu đo thời gian thực:

- Máy A (nhanh gấp 10 lần máy B) → kết quả khác.
- Cùng máy, chạy lúc rảnh vs lúc bận → kết quả khác.
- Ngôn ngữ, compiler, cache → kết quả khác.

Order of growth **loại bỏ tất cả các yếu tố này**, chỉ giữ lại **bản chất tăng trưởng**.

### 2.2. Dự đoán tương lai

Với order of growth, ta trả lời được:

- "Nếu dữ liệu tăng gấp 10, chương trình chậm đi bao nhiêu?"
  - $O(n)$: chậm gấp 10.
  - $O(n^2)$: chậm gấp 100.
  - $O(2^n)$: chậm gấp $2^9 \approx 512$ lần **chỉ khi $n$ tăng thêm 9** — không thể chấp nhận.

### 2.3. So sánh thuật toán độc lập máy

Hai thuật toán cùng bài toán:

- Thuật toán A: $3n^2 + 100n$ → order $n^2$.
- Thuật toán B: $n\log n + 10^6$ → order $n\log n$.

B thắng khi $n$ đủ lớn, bất kể hằng số. Đây là quyết định **thiết kế**, không phải **tối ưu code**.

> **Lý thuyết bên cạnh:** Trong thực tế, đôi khi hằng số quan trọng với $n$ nhỏ. Nhưng order of growth là **tiêu chí đầu tiên** khi chọn thuật toán cho dữ liệu lớn. Ví dụ: Python dùng Timsort ($O(n\log n)$) cho `sorted()` thay vì insertion sort ($O(n^2)$) dù hằng số của insertion sort nhỏ hơn.

---

## 3. Định nghĩa hình thức qua ký hiệu tiệm cận

Order of growth của $T(n)$ chính là **lớp hàm** $\Theta(g(n))$ mà $T(n)$ thuộc vào.

### 3.1. Nhắc lại định nghĩa

$$
T(n) = \Theta(g(n)) \iff \exists\, c_1, c_2 > 0, n_0 : \forall n \ge n_0,\ 0 \le c_1 g(n) \le T(n) \le c_2 g(n)
$$

Khi đó ta nói: **$T(n)$ có order of growth là $g(n)$**.

### 3.2. Ví dụ

$T(n) = 5n^2 + 3n + 7$ có order of growth là $n^2$ vì:

$$
3n^2 \le 5n^2 + 3n + 7 \le 15n^2, \quad \forall n \ge 1
$$

(chọn $c_1 = 3$, $c_2 = 15$, $n_0 = 1$).

### 3.3. Quan hệ với $O, \Omega, \Theta$

| Ký hiệu               | Ý nghĩa order of growth                                |
| --------------------- | ------------------------------------------------------ |
| $T(n) = O(g(n))$      | $T$ tăng **không nhanh hơn** $g$                       |
| $T(n) = \Omega(g(n))$ | $T$ tăng **không chậm hơn** $g$                        |
| $T(n) = \Theta(g(n))$ | $T$ tăng **đúng bằng** $g$ (order of growth chính xác) |

Khi nói "order of growth của thuật toán là $g(n)$", ta thường ngầm hiểu là $\Theta(g(n))$ — **đúng bậc**, không chỉ cận trên.

> **Lý thuyết bên cạnh:** Trong tài liệu, người ta đôi khi dùng $O$ lỏng lẻo để nói order of growth. Nhưng chính xác thì order of growth = $\Theta$. Ví dụ: "$n^2 + n$ có order of growth là $n^2$" — nghĩa là $\Theta(n^2)$.

---

## 4. Thứ bậc tăng trưởng (Growth Hierarchy)

Đây là **bảng xếp hạng** các order of growth thường gặp, từ **chậm nhất** (tốt nhất) đến **nhanh nhất** (tệ nhất):

$$
1 \ \prec\ \log\log n \ \prec\ \log n \ \prec\ \sqrt{n} \ \prec\ n \ \prec\ n\log n \ \prec\ n^2 \ \prec\ n^3 \ \prec\ \cdots \ \prec\ 2^n \ \prec\ n! \ \prec\ n^n
$$

Ký hiệu $\prec$ nghĩa là "tăng chậm hơn thực sự" (tức $f = o(g)$).

### 4.1. Bảng chi tiết với tên gọi

| Order         | Tên            | Ví dụ thuật toán                               |
| ------------- | -------------- | ---------------------------------------------- |
| $O(1)$        | Hằng số        | Truy cập mảng `A[i]`                           |
| $O(\log n)$   | Logarit        | Binary search                                  |
| $O(\sqrt{n})$ | Căn bậc hai    | Kiểm tra số nguyên tố (thử đến $\sqrt{n}$)     |
| $O(n)$        | Tuyến tính     | Duyệt mảng, linear search                      |
| $O(n\log n)$  | Tuyến tính-log | Merge sort, heap sort                          |
| $O(n^2)$      | Bậc hai        | Bubble sort, insertion sort worst              |
| $O(n^3)$      | Bậc ba         | Nhân ma trận naive                             |
| $O(2^n)$      | Hàm mũ         | Fibonacci đệ quy naive, subset sum brute force |
| $O(n!)$       | Giai thừa      | Liệt kê hoán vị, TSP brute force               |
| $O(n^n)$      | Siêu giai thừa | Một số bài toán tổ hợp cực khó                 |

### 4.2. Trực giác về tốc độ

Giả sử máy chạy $10^9$ phép toán/giây. Với các order khác nhau, thời gian chạy với $n = 100$:

| Order       | Số phép toán       | Thời gian                 |
| ----------- | ------------------ | ------------------------- |
| $O(\log n)$ | $\approx 7$        | tức thời                  |
| $O(n)$      | $100$              | tức thời                  |
| $O(n^2)$    | $10{,}000$         | tức thời                  |
| $O(n^3)$    | $10^6$             | 1 ms                      |
| $O(2^n)$    | $\approx 10^{30}$  | **lâu hơn tuổi vũ trụ**   |
| $O(n!)$     | $\approx 10^{158}$ | **không thể tưởng tượng** |

**Nhận xét:** Sự khác biệt giữa $O(n^2)$ và $O(2^n)$ không phải "vài lần" — mà là **hàng tỷ tỷ lần**. Đây là lý do order of growth quan trọng hơn hằng số.

> **Lý thuyết bên cạnh:** Với $n = 100$, $2^{100} \approx 10^{30}$ — lớn hơn số nguyên tử trong vũ trụ quan sát được ($\approx 10^{80}$) chỉ vài bậc. Với $n!$ khi $n = 100$, con số lớn đến mức không thể biểu diễn bằng bất kỳ ý nghĩa vật lý nào. Đây là lý do các thuật toán $O(2^n)$ và $O(n!)$ chỉ dùng được cho $n$ rất nhỏ.

---

## 5. Cách xác định order of growth

### 5.1. Quy tắc thực hành

**Quy tắc 1 — Bỏ hằng số nhân:**

$$
T(n) = 5n^2 + 3n \Rightarrow \text{order } n^2
$$

**Quy tắc 2 — Bỏ số hạng bậc thấp:**

$$
T(n) = n^3 + n^2 + n \Rightarrow \text{order } n^3
$$

**Quy tắc 3 — Chỉ giữ số hạng tăng nhanh nhất:**

$$
T(n) = 2^n + n^{100} \Rightarrow \text{order } 2^n
$$

**Quy tắc 4 — Với log, bỏ cơ số:**

$$
T(n) = \log_2 n + \log_{10} n \Rightarrow \text{order } \log n
$$

**Quy tắc 5 — Với tổng có số hạng tử cố định, lấy max:**

$$
T(n) = O(f) + O(g) \Rightarrow \text{order } \max(f, g)
$$

**Quy tắc 6 — Với tích, nhân các order:**

$$
O(f) \cdot O(g) \Rightarrow \text{order } f \cdot g
$$

### 5.2. Phương pháp giới hạn (limit method)

Xác định order of growth bằng cách so sánh với hàm chuẩn $g(n)$:

$$
L = \lim_{n \to \infty} \frac{T(n)}{g(n)}
$$

| $L$           | Kết luận                                         |
| ------------- | ------------------------------------------------ |
| $0$           | $T$ có order **nhỏ hơn** $g$ ($T = o(g)$)        |
| hằng số $> 0$ | $T$ có order **đúng bằng** $g$ ($T = \Theta(g)$) |
| $\infty$      | $T$ có order **lớn hơn** $g$ ($T = \omega(g)$)   |

### 5.3. Ví dụ — xác định order của $T(n) = 4n^2 + 100n + 1000$

**So sánh với $g(n) = n^2$:**

$$
L = \lim_{n \to \infty} \frac{4n^2 + 100n + 1000}{n^2} = 4
$$

Hằng số khác 0 ⇒ $T(n) = \Theta(n^2)$. Order of growth là $n^2$.

**So sánh với $g(n) = n$:**

$$
L = \lim_{n \to \infty} \frac{4n^2 + 100n + 1000}{n} = \infty
$$

⇒ $T = \omega(n)$ — tăng nhanh hơn $n$.

**So sánh với $g(n) = n^3$:**

$$
L = \lim_{n \to \infty} \frac{4n^2 + 100n + 1000}{n^3} = 0
$$

⇒ $T = o(n^3)$ — tăng chậm hơn $n^3$.

---

## 6. Ví dụ chi tiết kèm giải thích

### Ví dụ 1 — Vòng lặp đơn

```text
for i = 1 to n:
    doing O(1) work
```

**Đếm:** $n$ lần lặp, mỗi lần $O(1)$.

$$
T(n) = n \cdot 1 = n \Rightarrow \text{order } n
$$

**Giải thích:** Khi $n$ tăng gấp đôi, số phép toán tăng gấp đôi. Đây là **tuyến tính**.

### Ví dụ 2 — Vòng lặp lồng nhau

```text
for i = 1 to n:
    for j = 1 to n:
        doing O(1) work
```

**Đếm:** $n \cdot n = n^2$ lần.

$$
T(n) = n^2 \Rightarrow \text{order } n^2
$$

**Giải thích:** Khi $n$ tăng gấp đôi, số phép toán tăng gấp **bốn**. Đây là **bậc hai**.

### Ví dụ 3 — Vòng lặp với điều kiện $j \le i$

```text
for i = 1 to n:
    for j = 1 to i:
        doing O(1) work
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \frac{n^2 + n}{2}
$$

**Order:** Bỏ hằng số và số hạng bậc thấp ⇒ $\Theta(n^2)$.

**Giải thích:** Dù tổng thực tế chỉ khoảng $n^2/2$ (nửa của $n^2$), order vẫn là $n^2$. Hằng số $1/2$ **không ảnh hưởng** đến order.

### Ví dụ 4 — Vòng lặp chia đôi

```text
i = 1
while i < n:
    doing O(1) work
    i = i * 2
```

**Đếm:** $i$ nhận $1, 2, 4, 8, \ldots, 2^k < n$. Số lần lặp:

$$
k = \lfloor \log_2 n \rfloor + 1
$$

**Order:** $\Theta(\log n)$.

**Giải thích:** Khi $n$ tăng gấp đôi, số lần lặp **chỉ tăng thêm 1**. Đây là lý do $O(\log n)$ được coi là "cực nhanh" — $n = 10^9$ chỉ cần $\approx 30$ bước.

### Ví dụ 5 — Vòng lặp lồng với bước nhảy logarit

```text
for i = 1 to n:
    j = 1
    while j < n:
        doing O(1) work
        j = j * 2
```

**Đếm:** Vòng ngoài $n$ lần, vòng trong $\log n$ lần.

$$
T(n) = n \cdot \log n \Rightarrow \text{order } n\log n
$$

**Giải thích:** Khi $n$ tăng gấp đôi, $n$ tăng gấp đôi **và** $\log n$ tăng thêm 1 ⇒ tổng tăng hơn gấp đôi một chút. Đây là order của Merge Sort, Heap Sort.

### Ví dụ 6 — Đệ quy Fibonacci naive

$$
T(n) = T(n-1) + T(n-2) + O(1)
$$

**Phân tích:** Cây đệ quy có số nút xấp xỉ $F_n$ (số Fibonacci thứ $n$). Ta có:

$$
F_n = \Theta(\varphi^n), \quad \varphi = \frac{1+\sqrt{5}}{2} \approx 1.618
$$

**Order:** $\Theta(\varphi^n)$ — **hàm mũ**.

**Giải thích:** Cùng một bài toán con được tính đi tính lại rất nhiều lần. Order hàm mũ khiến thuật toán chỉ dùng được cho $n$ rất nhỏ (khoảng $n \le 40$).

### Ví dụ 7 — Merge Sort

$$
T(n) = 2T(n/2) + \Theta(n)
$$

**Phân tích bằng cây đệ quy:**

- Tầng 0: $n$
- Tầng 1: $2 \cdot n/2 = n$
- Tầng 2: $4 \cdot n/4 = n$
- ...
- Tầng $\log_2 n$: tổng $n$

Có $\log_2 n + 1$ tầng, mỗi tầng $n$:

$$
T(n) = n(\log_2 n + 1) \Rightarrow \text{order } n\log n
$$

**Giải thích:** Chia để trị với chi phí trộn tuyến tính ⇒ order $n\log n$. Đây là **cận dưới** cho mọi thuật toán sắp xếp dựa trên so sánh.

### Ví dụ 8 — Binary Search

$$
T(n) = T(n/2) + O(1)
$$

**Phân tích:** Mỗi bước giảm một nửa kích thước. Số bước:

$$
\log_2 n \Rightarrow \text{order } \log n
$$

### Ví dụ 9 — Đa thức tổng quát

$$
T(n) = 7n^4 + 3n^3 + 100n^2 + 5000
$$

**Order:** $n^4$ — chỉ giữ số hạng bậc cao nhất, bỏ hằng số.

**Giải thích:** Với $n$ lớn, $n^4$ áp đảo tất cả các số hạng khác. Cụ thể khi $n = 1000$: $7n^4 = 7 \cdot 10^{12}$, còn $3n^3 = 3 \cdot 10^9$ — nhỏ hơn $2000$ lần.

### Ví dụ 10 — Tổng có số hạng phụ thuộc $n$

$$
T(n) = \sum_{i=1}^{n} i^2
$$

**Tính chính xác:**

$$
T(n) = \frac{n(n+1)(2n+1)}{6} = \frac{2n^3 + 3n^2 + n}{6}
$$

**Order:** $\Theta(n^3)$.

**Giải thích:** Tổng bình phương tăng theo lập phương. Khi $n$ tăng gấp đôi, tổng tăng gấp **tám**.

### Ví dụ 11 — Tổng harmonic

$$
T(n) = \sum_{i=1}^{n} \frac{1}{i}
$$

**Công thức:** $H_n = \ln n + O(1)$.

**Order:** $\Theta(\log n)$.

**Giải thích:** Mặc dù mỗi số hạng giảm, tổng vẫn tăng — nhưng rất chậm, chỉ theo logarit. Đây là lý do Quick Sort average case là $\Theta(n\log n)$.

### Ví dụ 12 — Hàm có nhiều đoạn

```text
Function F(n):
    for i = 1 to n:           // O(n)
        doing O(1) work
    for i = 1 to n:           // O(n^2)
        for j = 1 to n:
            doing O(1) work
    for i = 1 to n:           // O(n)
        doing O(1) work
```

**Tổng:** $O(n) + O(n^2) + O(n) = O(n^2)$.

**Order:** $n^2$ — đoạn lớn nhất quyết định.

> **Lý thuyết bên cạnh:** Quy tắc "lấy max" chỉ đúng khi **số đoạn là cố định**. Nếu có $n$ đoạn, mỗi đoạn $O(n)$, tổng là $O(n^2)$ — không phải $O(n)$.

---

## 7. So sánh các order of growth — kỹ thuật nâng cao

### 7.1. Quy tắc "log chậm hơn đa thức"

Với mọi $k > 0$ và $\varepsilon > 0$:

$$
\log^k n = o(n^{\varepsilon})
$$

**Ví dụ:** $\log^{100} n = o(n^{0.001})$ — dù log mũ 100, vẫn chậm hơn $n^{0.001}$ khi $n$ đủ lớn.

**Ứng dụng:** Nếu thuật toán A là $O(\log^{10} n)$ và B là $O(n^{0.1})$, A thắng khi $n$ đủ lớn (dù hằng số mũ log lớn đến đâu).

### 7.2. Quy tắc "đa thức chậm hơn hàm mũ"

Với mọi $k > 0$ và $c > 1$:

$$
n^k = o(c^n)
$$

**Ví dụ:** $n^{1000} = o(1.001^n)$ — dù đa thức bậc 1000, vẫn chậm hơn hàm mũ cơ số 1.001 khi $n$ đủ lớn.

### 7.3. Quy tắc "hàm mũ chậm hơn giai thừa"

Với mọi $c > 1$:

$$
c^n = o(n!)
$$

**Chứng minh (dùng Stirling):**

$$
\frac{c^n}{n!} \approx \frac{c^n}{\sqrt{2\pi n} (n/e)^n} = \frac{(ce)^n}{\sqrt{2\pi n} \cdot n^n} \to 0
$$

### 7.4. So sánh cùng họ — hằng số mũ

Trong cùng họ đa thức: $n^a$ vs $n^b$ với $a < b$ ⇒ $n^a = o(n^b)$.

Trong cùng họ mũ: $a^n$ vs $b^n$ với $a < b$ ⇒ $a^n = o(b^n)$.

Trong cùng họ log: $\log^a n$ vs $\log^b n$ với $a < b$ ⇒ $\log^a n = o(\log^b n)$.

### 7.5. Ví dụ so sánh phức tạp

**So sánh $n\log n$ và $n^{1.1}$:**

$$
\frac{n\log n}{n^{1.1}} = \frac{\log n}{n^{0.1}} \to 0
$$

Vậy $n\log n = o(n^{1.1})$.

**So sánh $2^n$ và $n^{100}$:**

$$
\frac{2^n}{n^{100}} \to \infty
$$

Vậy $n^{100} = o(2^n)$.

**So sánh $n!$ và $2^{n\log n}$:**

$$
2^{n\log n} = (2^{\log n})^n = n^n
$$

So sánh $n!$ với $n^n$: dùng Stirling, $n! \approx (n/e)^n \sqrt{2\pi n}$, nhỏ hơn $n^n$ một yếu tố $(e)^n$. Vậy $n! = o(n^n)$ nhưng $n! = \omega(2^n)$.

> **Lý thuyết bên cạnh:** Khi so sánh hai order, hãy đưa về **cùng dạng** (cùng cơ số mũ hoặc cùng log) rồi so sánh. Ví dụ $2^{n\log n} = n^n$ — chuyển về cùng cơ số giúp so sánh dễ hơn.

---

## 8. Order of growth trong hệ thức truy hồi

### 8.1. Phương pháp đoán — substitution

**Bài toán:** $T(n) = 2T(n/2) + n$, $T(1) = 1$. Tìm order.

**Đoán:** $T(n) = O(n\log n)$.

**Chứng minh bằng quy nạp:**

Giả sử $T(k) \le ck\log k$ với mọi $k < n$. Xét $n$:

$$
T(n) = 2T(n/2) + n \le 2c(n/2)\log(n/2) + n = cn(\log n - 1) + n = cn\log n - cn + n
$$

Để $\le cn\log n$, cần $-cn + n \le 0$, tức $c \ge 1$. Chọn $c = 1$ (hoặc lớn hơn), bất đẳng thức đúng. $\blacksquare$

### 8.2. Phương pháp cây đệ quy

Vẽ cây, mỗi tầng cộng chi phí, rồi cộng tất cả các tầng. (Đã minh họa ở Ví dụ 7.)

### 8.3. Master Theorem

Với $T(n) = aT(n/b) + f(n)$:

Đặt $x = n^{\log_b a}$.

1. Nếu $f(n) = O(x^{1-\varepsilon})$ ⇒ $T(n) = \Theta(x)$.
2. Nếu $f(n) = \Theta(x)$ ⇒ $T(n) = \Theta(x \log n)$.
3. Nếu $f(n) = \Omega(x^{1+\varepsilon})$ (và điều kiện chính quy) ⇒ $T(n) = \Theta(f(n))$.

**Ví dụ áp dụng:**

| Hệ thức                | $a$ | $b$ | $f(n)$ | $x = n^{\log_b a}$ | Case          | Order               |
| ---------------------- | --- | --- | ------ | ------------------ | ------------- | ------------------- |
| $T(n) = T(n/2) + 1$    | 1   | 2   | $1$    | $1$                | 2             | $\Theta(\log n)$    |
| $T(n) = 2T(n/2) + n$   | 2   | 2   | $n$    | $n$                | 2             | $\Theta(n\log n)$   |
| $T(n) = 4T(n/2) + n$   | 4   | 2   | $n$    | $n^2$              | 1             | $\Theta(n^2)$       |
| $T(n) = 4T(n/2) + n^2$ | 4   | 2   | $n^2$  | $n^2$              | 2             | $\Theta(n^2\log n)$ |
| $T(n) = 4T(n/2) + n^3$ | 4   | 2   | $n^3$  | $n^2$              | 3             | $\Theta(n^3)$       |
| $T(n) = 2T(n-1) + 1$   | —   | —   | —      | —                  | không áp dụng | $\Theta(2^n)$       |

> **Lý thuyết bên cạnh:** Master Theorem **không áp dụng** cho hệ thức trừ (như $T(n) = T(n-1) + \ldots$). Với hệ thức trừ, phải dùng phương pháp thế hoặc cây đệ quy. Ví dụ $T(n) = 2T(n-1) + 1$ cho $T(n) = \Theta(2^n)$.

---

## 9. Những hiểu nhầm phổ biến về order of growth

### Hiểu nhầm 1 — "Order of growth là số phép toán chính xác"

**Sai.** Order of growth là **bậc**, không phải giá trị. $T(n) = n^2$ và $T(n) = 1000n^2$ có **cùng order** $n^2$.

### Hiểu nhầm 2 — "Hằng số không quan trọng"

**Sai một nửa.** Hằng số không quan trọng **về mặt tiệm cận**, nhưng quan trọng **trong thực tế với $n$ nhỏ**. Ví dụ: $10^9 n$ và $n$ cùng $O(n)$ nhưng thực tế rất khác.

### Hiểu nhầm 3 — "Order nhỏ hơn luôn nhanh hơn"

**Đúng về xu hướng khi $n$ lớn.** Nhưng với $n$ nhỏ, order lớn hơn với hằng số nhỏ có thể nhanh hơn. Ví dụ: Insertion Sort ($O(n^2)$) nhanh hơn Merge Sort ($O(n\log n)$) với $n < 50$ trong nhiều cài đặt.

### Hiểu nhầm 4 — "Cùng order nghĩa là cùng tốc độ"

**Sai.** Cùng order, khác hằng số ⇒ khác tốc độ thực tế. $n\log n$ với hằng số 1 và $n\log n$ với hằng số 100 cùng order nhưng chênh 100 lần.

### Hiểu nhầm 5 — "Order of growth chỉ áp dụng cho thời gian"

**Sai.** Order of growth áp dụng cho **mọi tài nguyên**: thời gian, bộ nhớ, số phép so sánh, số lần truy cập đĩa, băng thông mạng…

### Hiểu nhầm 6 — "$O(2n) = O(n)$ nghĩa là mọi hằng số đều bỏ được"

**Đúng về lý thuyết tiệm cận**, nhưng khi phân tích **chi tiết** (ví dụ so sánh hai thuật toán cùng order), phải giữ hằng số.

---

## 10. Ứng dụng thực tế của order of growth

### 10.1. Chọn thuật toán

Với dữ liệu lớn, order of growth là **tiêu chí đầu tiên**:

- Sắp xếp $10^6$ phần tử: $O(n\log n)$ ($\approx 2 \cdot 10^7$ phép) thay vì $O(n^2)$ ($\approx 10^{12}$ phép).
- Tìm kiếm trong $10^9$ phần tử: $O(\log n)$ ($\approx 30$ bước) thay vì $O(n)$ ($10^9$ bước).

### 10.2. Dự đoán khả năng mở rộng

Nếu thuật toán là $O(n^2)$ và hiện tại chạy 1 giây với $n = 1000$:

- $n = 2000$ ⇒ 4 giây.
- $n = 10{,}000$ ⇒ 100 giây.
- $n = 100{,}000$ ⇒ $\approx 2.8$ giờ.

Với $O(n\log n)$:

- $n = 100{,}000$ ⇒ $\approx 1.7$ giây.

### 10.3. Thiết kế hệ thống

Order of growth ảnh hưởng đến:

- **Kích thước dữ liệu tối đa** có thể xử lý.
- **Số máy chủ** cần thiết.
- **Chi phí điện toán** (cloud billing).

> **Lý thuyết bên cạnh:** Trong thực tế, hằng số có thể quan trọng với dữ liệu vừa. Nhưng khi thiết kế hệ thống cho dữ liệu **lớn và tăng trưởng**, order of growth là yếu tố **quyết định** khả năng mở rộng.

---

## 11. Tóm tắt — bảng tra nhanh

### 11.1. Quy tắc xác định order

| Tình huống                           | Order     |
| ------------------------------------ | --------- |
| Vòng lặp đơn $n$ lần                 | $n$       |
| Vòng lặp lồng $k$ tầng, mỗi tầng $n$ | $n^k$     |
| Vòng lặp nhân/chia đôi               | $\log n$  |
| Vòng ngoài $n$, vòng trong $\log n$  | $n\log n$ |
| Đệ quy $T(n) = T(n/2) + O(1)$        | $\log n$  |
| Đệ quy $T(n) = 2T(n/2) + O(n)$       | $n\log n$ |
| Đệ quy $T(n) = T(n-1) + O(1)$        | $n$       |
| Đệ quy $T(n) = 2T(n-1) + O(1)$       | $2^n$     |
| Tổng $\sum_{i=1}^{n} i$              | $n^2$     |
| Tổng $\sum_{i=1}^{n} i^2$            | $n^3$     |
| Tổng $\sum_{i=1}^{n} 1/i$            | $\log n$  |

### 11.2. Thứ bậc tăng trưởng

$$
1 \prec \log\log n \prec \log n \prec \sqrt{n} \prec n \prec n\log n \prec n^2 \prec n^3 \prec 2^n \prec n! \prec n^n
$$

### 11.3. Quy tắc vàng

1. **Bỏ hằng số nhân.**
2. **Bỏ số hạng bậc thấp.**
3. **Log: bỏ cơ số.**
4. **Tổng cố định: lấy max.**
5. **Tích: nhân các order.**
6. **Khi nghi ngờ: dùng giới hạn.**

---

## 12. Bài tập tự luyện (kèm hướng giải)

**Bài 1:** Xác định order của $T(n) = 3n^3 + 100n^2 + 5$.

*Hướng:* Bỏ hằng số và bậc thấp ⇒ $\Theta(n^3)$.

**Bài 2:** Xác định order của $T(n) = 2^n + n^{100} + n!$.

*Hướng:* So sánh $2^n$ vs $n!$: $n!$ lớn hơn. Vậy $\Theta(n!)$.

**Bài 3:** Xác định order của vòng lặp:

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n:
            doing O(1) work
```

*Hướng:* Ba tầng lồng ⇒ $\Theta(n^3)$.

**Bài 4:** Giải $T(n) = 3T(n/3) + n$.

*Hướng:* Master Theorem: $a = 3, b = 3, f(n) = n$, $x = n^{\log_3 3} = n$. $f(n) = \Theta(n) = \Theta(x)$ ⇒ Case 2 ⇒ $\Theta(n\log n)$.

**Bài 5:** So sánh $n^{0.1}$ và $\log^{10} n$.

*Hướng:* $\log^{10} n = o(n^{0.1})$ ⇒ $n^{0.1}$ tăng nhanh hơn.

**Bài 6:** Chứng minh $2^{n+1} = \Theta(2^n)$.

*Hướng:* $2^{n+1} = 2 \cdot 2^n$ ⇒ cùng order (hằng số 2 bị bỏ).

---

Nếu bạn muốn, mình có thể đi sâu tiếp vào:
- **Chứng minh hình thức** các quy tắc so sánh order (dùng giới hạn).
- **Phân tích average-case** của Quick Sort với order of growth chi tiết.
- **Bài tập nâng cao** về so sánh các order phức tạp (ví dụ $n^{\log n}$ vs $2^{\sqrt{n}}$).
- **Ứng dụng order of growth** trong thiết kế hệ thống thực tế.