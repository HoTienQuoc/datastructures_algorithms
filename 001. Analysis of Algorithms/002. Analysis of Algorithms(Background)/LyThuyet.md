# Nền tảng cần thiết cho Phân tích Giải thuật (Background)

Dưới đây là **toàn bộ nền tảng kiến thức** bạn cần có trước khi học phân tích giải thuật một cách nghiêm túc. Mình dùng delimiter `$...$` / `$$...$$` để tránh lỗi định dạng như bạn đã gặp.

---

## 1. Vì sao cần background?

Phân tích giải thuật là môn **giao thoa** giữa:

- **Toán rời rạc** (đếm, tổng, log, giai thừa)
- **Kỹ thuật chứng minh** (quy nạp, phản chứng)
- **Cấu trúc dữ liệu** (mảng, cây, đồ thị)
- **Xác suất** (cho average-case và randomized algorithms)
- **Mô hình tính toán** (RAM model)

Thiếu nền tảng nào, bạn sẽ bị "khựng" ở phần tương ứng. Dưới đây là từng mảng.

---

## 2. Toán rời rạc cơ bản

### 2.1. Logarit — các đẳng thức **phải thuộc lòng**

Với $a, b > 0$, $a \ne 1$, $b \ne 1$, $x, y > 0$:

$$
\log_a(xy) = \log_a x + \log_a y
$$

$$
\log_a\!\left(\frac{x}{y}\right) = \log_a x - \log_a y
$$

$$
\log_a(x^k) = k \log_a x
$$

$$
\log_a x = \frac{\log_b x}{\log_b a} \quad \text{(đổi cơ số)}
$$

$$
a^{\log_b x} = x^{\log_b a}
$$

$$
\log_a b = \frac{1}{\log_b a}
$$

**Hệ quả quan trọng cho phân tích giải thuật:**

- Đổi cơ số chỉ **nhân thêm hằng số** $\Rightarrow$ **mọi log cùng bậc**: $\log_2 n = \Theta(\log_{10} n) = \Theta(\ln n)$.
- Vì vậy khi viết $O(\log n)$, **không cần ghi cơ số**.
- $2^{\log_2 n} = n$ — dùng rất nhiều khi giải hệ thức truy hồi.

### 2.2. Lũy thừa — các đẳng thức cơ bản

$$
a^{m+n} = a^m \cdot a^n, \quad a^{mn} = (a^m)^n, \quad a^{-n} = \frac{1}{a^n}
$$

$$
(a^m)^{1/n} = a^{m/n}, \quad \sqrt[n]{a} = a^{1/n}
$$

**Tăng trưởng so sánh:**

Với mọi hằng số $k > 0$ và $\varepsilon > 0$:

$$
\log^k n = o(n^{\varepsilon})
$$

Nghĩa là **log (dù lũy thừa bao nhiêu) luôn chậm hơn đa thức bậc dương bất kỳ**.

Với mọi $c > 1$ và $k > 0$:

$$
n^k = o(c^n)
$$

Nghĩa là **đa thức luôn chậm hơn hàm mũ**.

Với mọi $c > 1$ và $k > 0$:

$$
c^n = o(n!)
$$

Nghĩa là **hàm mũ luôn chậm hơn giai thừa**.

> **Lý thuyết bên cạnh:** Ba quan hệ trên là "xương sống" của thứ bậc tăng trưởng. Chúng giải thích vì sao $O(\log n)$ được coi là "gần như hằng số" trong thực tế, và vì sao thuật toán $O(2^n)$ chỉ dùng được cho $n$ rất nhỏ.

### 2.3. Giai thừa và xấp xỉ Stirling

Định nghĩa:

$$
n! = 1 \cdot 2 \cdot 3 \cdots n, \quad 0! = 1
$$

**Xấp xỉ Stirling** (công cụ chuẩn để so sánh hàm chứa $n!$):

$$
n! \approx \sqrt{2\pi n} \left(\frac{n}{e}\right)^n
$$

Suy ra:

$$
\log(n!) = \Theta(n \log n)
$$

**Ứng dụng:** Đây là lý do **cận dưới** của mọi thuật toán sắp xếp dựa trên so sánh là $\Omega(n \log n)$ — vì có $n!$ hoán vị cần phân biệt, và cây quyết định có ít nhất $n!$ lá, chiều cao $\ge \log(n!) = \Omega(n\log n)$.

**Ví dụ áp dụng — so sánh $2^n$ và $n!$:**

$$
\frac{2^n}{n!} \approx \frac{2^n}{\sqrt{2\pi n} (n/e)^n} = \frac{(2e)^n}{\sqrt{2\pi n} \cdot n^n}
$$

Vì $n^n$ tăng nhanh hơn $(2e)^n$ khi $n \to \infty$, tỉ số $\to 0$. Vậy $2^n = o(n!)$.

### 2.4. Tổng (summations) — các công thức **phải thuộc**

**Cấp số cộng:**

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \Theta(n^2)
$$

$$
\sum_{i=1}^{n} (2i - 1) = n^2
$$

**Cấp số nhân:**

$$
\sum_{i=0}^{n} r^i = \frac{r^{n+1} - 1}{r - 1} \quad (r \ne 1)
$$

**Trường hợp $r = 2$ (cực hay gặp):**

$$
\sum_{i=0}^{n} 2^i = 2^{n+1} - 1 = \Theta(2^n)
$$

**Trường hợp $r = 1/2$ (chuỗi hội tụ):**

$$
\sum_{i=0}^{\infty} \left(\frac{1}{2}\right)^i = 2
$$

Đây là lý do **amortized cost** của dynamic array là $O(1)$: tổng chi phí copy $1 + 2 + 4 + \cdots + 2^k < 2n$.

**Tổng bình phương và lập phương:**

$$
\sum_{i=1}^{n} i^2 = \frac{n(n+1)(2n+1)}{6} = \Theta(n^3)
$$

$$
\sum_{i=1}^{n} i^3 = \left(\frac{n(n+1)}{2}\right)^2 = \Theta(n^4)
$$

**Tổng điều hòa (harmonic):**

$$
H_n = \sum_{i=1}^{n} \frac{1}{i} = \ln n + O(1) = \Theta(\log n)
$$

**Ứng dụng:** Quick Sort average case có $T(n) = \Theta(n \log n)$ vì chi phí trung bình liên quan đến $H_n$.

**Tổng log:**

$$
\sum_{i=1}^{n} \log i = \log(n!) = \Theta(n \log n)
$$

**Ứng dụng:** Merge Sort — mỗi tầng có tổng chi phí $n$, có $\log n$ tầng, tổng là $\Theta(n\log n)$.

**Công thức quan trọng — tổng có số hạng phụ thuộc $n$:**

$$
\sum_{i=1}^{n} \frac{1}{i} - \ln n \to \gamma \approx 0.5772 \quad \text{(hằng số Euler–Mascheroni)}
$$

### 2.5. Ví dụ đầy đủ — đếm phép toán bằng tổng

**Ví dụ:** Phân tích vòng lặp

```text
for i = 1 to n:
    for j = 1 to i:
        doing O(1) work
```

Số lần thực hiện phép toán cơ bản:

$$
T(n) = \sum_{i=1}^{n} \sum_{j=1}^{i} 1 = \sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \Theta(n^2)
$$

**Ví dụ khác — vòng lặp với bước nhảy:**

```text
for i = 1 to n:
    j = 1
    while j < n:
        doing O(1) work
        j = j * 2
```

Với mỗi $i$, $j$ nhận các giá trị $1, 2, 4, \ldots, 2^k$ với $2^k < n$. Số lần lặp là $\lfloor \log_2 n \rfloor + 1 = \Theta(\log n)$. Có $n$ vòng ngoài:

$$
T(n) = n \cdot \Theta(\log n) = \Theta(n \log n)
$$

> **Lý thuyết bên cạnh:** Khi thấy vòng lặp với biến nhân/chia, nghĩ ngay đến **log**. Khi thấy vòng lặp cộng/trừ, nghĩ đến **tuyến tính**. Đây là phản xạ cần luyện.

---

## 3. Kỹ thuật chứng minh

### 3.1. Quy nạp toán học (Mathematical Induction)

**Cấu trúc:**

1. **Base case:** Chứng minh mệnh đề đúng với $n = n_0$ (thường $n_0 = 1$ hoặc $0$).
2. **Inductive step:** Giả sử mệnh đề đúng với $n = k$ (giả thiết quy nạp). Chứng minh nó đúng với $n = k+1$.
3. **Kết luận:** Mệnh đề đúng với mọi $n \ge n_0$.

**Ví dụ:** Chứng minh $\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$ với mọi $n \ge 1$.

- **Base:** $n = 1$: $\sum_{i=1}^{1} i = 1 = \frac{1 \cdot 2}{2}$. Đúng.
- **Step:** Giả sử đúng với $k$: $\sum_{i=1}^{k} i = \frac{k(k+1)}{2}$. Xét $n = k+1$:

$$
\sum_{i=1}^{k+1} i = \sum_{i=1}^{k} i + (k+1) = \frac{k(k+1)}{2} + (k+1) = \frac{(k+1)(k+2)}{2}
$$

Đúng với $k+1$. $\blacksquare$

**Ứng dụng trong phân tích giải thuật:** Chứng minh nghiệm của hệ thức truy hồi (ví dụ chứng minh $T(n) = O(n\log n)$ bằng quy nạp).

### 3.2. Quy nạp mạnh (Strong Induction)

Khác biệt: giả thiết đúng với **mọi** $k \le n$, không chỉ $n$ đơn lẻ.

**Dùng khi:** Hệ thức truy hồi phụ thuộc nhiều giá trị trước (ví dụ $T(n) = T(n-1) + T(n-2)$ cho Fibonacci).

### 3.3. Phản chứng (Proof by Contradiction)

**Cấu trúc:** Giả sử mệnh đề **sai**, dẫn đến mâu thuẫn logic. Vậy mệnh đề đúng.

**Ví dụ:** Chứng minh $\sqrt{2}$ vô tỉ (kinh điển) — hoặc trong phân tích giải thuật: chứng minh $n^2 \ne O(n)$.

**Phản chứng:** Giả sử $n^2 = O(n)$. Khi đó tồn tại $c > 0, n_0$ sao cho $n^2 \le cn$ với mọi $n \ge n_0$. Chia hai vế cho $n > 0$: $n \le c$ với mọi $n \ge n_0$ — vô lý vì $n$ có thể lớn tùy ý. $\blacksquare$

### 3.4. Chứng minh trực tiếp

Đi thẳng từ giả thiết đến kết luận bằng biến đổi đại số. Hay dùng khi tìm $c, n_0$ cho ký hiệu tiệm cận.

### 3.5. Phản ví dụ (Counterexample)

Để chứng minh một mệnh đề **sai**, chỉ cần **một** phản ví dụ.

**Ví dụ:** Mệnh đề "$f = O(g)$ và $g = O(h)$ suy ra $f = \Theta(h)$" — **sai**. Phản ví dụ: $f(n) = n$, $g(n) = n^2$, $h(n) = n^3$. Ta có $n = O(n^2)$ và $n^2 = O(n^3)$, nhưng $n \ne \Theta(n^3)$.

> **Lý thuyết bên cạnh:** Trong phân tích giải thuật, ta dùng **quy nạp** để chứng minh nghiệm của hệ thức truy hồi, **phản chứng** để chứng minh cận dưới (lower bounds), và **phản ví dụ** để bác bỏ các tuyên bố "luôn đúng".

---

## 4. Cấu trúc dữ liệu cơ bản (cần biết trước)

Phân tích giải thuật giả định bạn đã quen với chi phí các thao tác trên CTDL cơ bản.

### 4.1. Mảng (Array)

| Thao tác                       | Chi phí                  |
| ------------------------------ | ------------------------ |
| Truy cập `A[i]`                | $O(1)$                   |
| Tìm kiếm tuần tự               | $O(n)$                   |
| Chèn/xóa đầu/cuối (mảng tĩnh)  | $O(n)$ (do dịch phần tử) |
| Tìm kiếm nhị phân (đã sắp xếp) | $O(\log n)$              |

### 4.2. Danh sách liên kết (Linked List)

| Thao tác                   | Chi phí |
| -------------------------- | ------- |
| Truy cập theo chỉ số       | $O(n)$  |
| Chèn/xóa khi đã có con trỏ | $O(1)$  |
| Tìm kiếm                   | $O(n)$  |

### 4.3. Stack / Queue

- Push, pop, enqueue, dequeue: $O(1)$ amortized.
- **Stack:** LIFO. **Queue:** FIFO.

### 4.4. Cây tìm kiếm nhị phân (BST)

- Trung bình: tìm/chèn/xóa $O(\log n)$.
- Worst case (cây suy biến): $O(n)$.
- Cây cân bằng (AVL, Red-Black): **đảm bảo** $O(\log n)$.

### 4.5. Heap (đống)

- Tìm min/max: $O(1)$.
- Chèn: $O(\log n)$.
- Xóa min/max: $O(\log n)$.
- Build heap: $O(n)$ (không phải $O(n\log n)$ — kỹ thuật quan trọng).

### 4.6. Bảng băm (Hash Table)

- Tìm/chèn/xóa: $O(1)$ **average**, $O(n)$ **worst**.

### 4.7. Đồ thị (Graph)

- Biểu diễn: **ma trận kề** $O(V^2)$ bộ nhớ; **danh sách kề** $O(V + E)$ bộ nhớ.
- Duyệt: DFS, BFS đều $O(V + E)$.

> **Lý thuyết bên cạnh:** Khi phân tích giải thuật dùng CTDL, ta **cộng chi phí thao tác CTDL vào tổng**. Ví dụ BFS dùng queue: mỗi đỉnh vào/ra queue một lần → $O(V)$; duyệt cạnh → $O(E)$; tổng $O(V+E)$.

---

## 5. Xác suất cơ bản (cho Average-case và Randomized Algorithms)

### 5.1. Biến ngẫu nhiên và kỳ vọng

- **Biến ngẫu nhiên** $X$: hàm gán giá trị số cho mỗi kết quả.
- **Kỳ vọng:** $E[X] = \sum_{x} x \cdot P(X = x)$.

### 5.2. Biến chỉ báo (Indicator Random Variable)

Với biến cố $A$:

$$
I_A = \begin{cases} 1 & \text{nếu } A \text{ xảy ra} \\ 0 & \text{ngược lại} \end{cases}
$$

Tính chất vàng:

$$
E[I_A] = P(A)
$$

### 5.3. Linearity of Expectation

Với mọi biến ngẫu nhiên $X_1, X_2, \ldots, X_n$ (kể cả **không độc lập**):

$$
E\left[\sum_{i=1}^{n} X_i\right] = \sum_{i=1}^{n} E[X_i]
$$

Đây là **công cụ mạnh nhất** để phân tích average-case.

### 5.4. Ví dụ kinh điển — Số nghịch thế trong Insertion Sort

**Bài toán:** Mảng $A[1..n]$ hoán vị ngẫu nhiên đều. Số nghịch thế $I$ = số cặp $(i, j)$ với $i < j$ và $A[i] > A[j]$. Chứng minh $E[I] = \frac{n(n-1)}{4}$.

**Giải:** Với mỗi cặp $(i, j)$ với $i < j$, đặt:

$$
X_{ij} = \begin{cases} 1 & \text{nếu } A[i] > A[j] \\ 0 & \text{ngược lại} \end{cases}
$$

Vì hoán vị ngẫu nhiên, $P(A[i] > A[j]) = \frac{1}{2}$, nên $E[X_{ij}] = \frac{1}{2}$.

Tổng số nghịch thế:

$$
I = \sum_{i < j} X_{ij}
$$

Theo linearity of expectation:

$$
E[I] = \sum_{i<j} E[X_{ij}] = \binom{n}{2} \cdot \frac{1}{2} = \frac{n(n-1)}{4} = \Theta(n^2)
$$

**Kết luận:** Insertion Sort average case là $\Theta(n^2)$ — đúng như đã biết, nhưng giờ có **chứng minh chặt chẽ**.

> **Lý thuyết bên cạnh:** Linearity of expectation hoạt động **không cần độc lập**. Đây là điểm mạnh nhất của nó. Trong phân tích Quick Sort, ta cũng dùng đúng kỹ thuật này: đặt $X_{ij}$ = 1 nếu $A[i]$ và $A[j]$ được so sánh, tính $E[X_{ij}]$ từ xác suất pivot rơi vào khoảng $[i, j]$.

---

## 6. Hệ thức truy hồi — giới thiệu nhanh

### 6.1. Định nghĩa

**Hệ thức truy hồi** biểu diễn $T(n)$ theo $T$ của đầu vào nhỏ hơn.

**Ví dụ:**

$$
T(n) = 2T(n/2) + n, \quad T(1) = 1
$$

### 6.2. Ba phương pháp giải

1. **Phương pháp thế (substitution):** đoán nghiệm, chứng minh bằng quy nạp.
2. **Cây đệ quy (recursion tree):** vẽ cây, cộng chi phí từng tầng.
3. **Master Theorem:** cho hệ thức dạng $T(n) = aT(n/b) + f(n)$.

### 6.3. Ví dụ — giải bằng cây đệ quy

$$
T(n) = 2T(n/2) + n
$$

- **Tầng 0:** $n$
- **Tầng 1:** $2 \cdot (n/2) = n$
- **Tầng 2:** $4 \cdot (n/4) = n$
- ...
- **Tầng $\log_2 n$:** $n \cdot 1 = n$ (mỗi bài toán kích thước 1)

Có $\log_2 n + 1$ tầng, mỗi tầng tổng $n$:

$$
T(n) = n(\log_2 n + 1) = \Theta(n \log n)
$$

> **Lý thuyết bên cạnh:** Cây đệ quy cho **trực giác** rất mạnh. Master Theorem là **công thức đóng** cho trường hợp chia đều. Với hệ thức không đều (ví dụ $T(n) = T(n/3) + T(2n/3) + n$), phải dùng cây đệ quy hoặc Akra–Bazzi.

---

## 7. Mô hình tính toán (Model of Computation)

### 7.1. RAM Model (Random Access Machine)

Mô hình chuẩn trong phân tích giải thuật:

- Bộ nhớ là mảng các ô, mỗi ô chứa một số/word.
- Truy cập bất kỳ ô nào: $O(1)$.
- Phép toán số học cơ bản (cộng, trừ, nhân, chia, so sánh): $O(1)$.
- **Giả định ngầm:** mỗi word đủ lớn để chứa $n$ (hoặc $\log n$ bit). Đây là giả định **word RAM** — thường được dùng.

### 7.2. Tại sao mô hình quan trọng?

- Trên máy thật, phép nhân số lớn **không** $O(1)$ — nhưng trên RAM model với word cố định, ta giả định $O(1)$.
- Với thuật toán số lớn (big integer), phải dùng **bit complexity** thay vì word complexity.
- Thuật toán lượng tử, thuật toán song song có mô hình riêng.

> **Lý thuyết bên cạnh:** Hầu hết tài liệu phân tích giải thuật cổ điển (CLRS, Kleinberg–Tardos) dùng **word RAM model**. Khi đọc các bài báo về thuật toán số (FFT, phép nhân số lớn), cần chú ý họ dùng **bit model** — chi phí khác hoàn toàn.

---

## 8. Ký hiệu và quy ước

### 8.1. Ký hiệu

| Ký hiệu             | Ý nghĩa                                               |
| ------------------- | ----------------------------------------------------- |
| $\lfloor x \rfloor$ | phần nguyên dưới (floor)                              |
| $\lceil x \rceil$   | phần nguyên trên (ceiling)                            |
| $\binom{n}{k}$      | tổ hợp chập $k$ của $n$                               |
| $\sum, \prod$       | tổng, tích                                            |
| $\log n$            | logarit cơ số 2 (mặc định trong phân tích giải thuật) |
| $\ln n$             | logarit tự nhiên (cơ số $e$)                          |
| $\lg n$             | đôi khi dùng cho $\log_2 n$ (trong CLRS)              |

### 8.2. Quy ước về $n$

- $n$ thường là **kích thước đầu vào** (số phần tử, số bit, số đỉnh…).
- Đôi khi có nhiều tham số: $T(n, m)$, $T(V, E)$ — phân tích đa biến.
- Với đồ thị: $n = |V|$, $m = |E|$; độ phức tạp thường $O(V + E)$.

### 8.3. Tính chất của tổ hợp

$$
\binom{n}{k} = \frac{n!}{k!(n-k)!}
$$

$$
\binom{n}{k} = \binom{n}{n-k}
$$

$$
\binom{n}{0} = \binom{n}{n} = 1
$$

$$
\binom{n}{1} = n, \quad \binom{n}{2} = \frac{n(n-1)}{2}
$$

**Dùng khi:** đếm cặp, đếm hoán vị, phân tích average-case.

---

## 9. Ví dụ tổng hợp — áp dụng toàn bộ background

### Bài toán: Đếm số cặp nghịch thế bằng vòng lặp đôi

```text
CountInversions(A, n):
    count = 0
    for i = 1 to n-1:
        for j = i+1 to n:
            if A[i] > A[j]:
                count = count + 1
    return count
```

**Phân tích:**

Số lần so sánh:

$$
T(n) = \sum_{i=1}^{n-1} \sum_{j=i+1}^{n} 1 = \sum_{i=1}^{n-1} (n - i) = \sum_{k=1}^{n-1} k = \frac{n(n-1)}{2} = \Theta(n^2)
$$

**Nhận xét:** Thuật toán này **luôn** $\Theta(n^2)$ bất kể dữ liệu — vì vòng lặp không phụ thuộc giá trị. So sánh với Insertion Sort (cũng $\Theta(n^2)$ worst case nhưng $\Theta(n)$ best case).

**Cải tiến:** Dùng Merge Sort để đếm nghịch thế trong $\Theta(n \log n)$ — đây là ứng dụng kinh điển của chia để trị.

---

## 10. Lộ trình học background (thứ tự khuyến nghị)

1. **Toán rời rạc cơ bản** — log, lũy thừa, tổng, giai thừa (§2).
2. **Kỹ thuật chứng minh** — quy nạp, phản chứng (§3).
3. **Cấu trúc dữ liệu** — mảng, danh sách, cây, heap, hash, đồ thị (§4).
4. **Xác suất cơ bản** — biến ngẫu nhiên, kỳ vọng, indicator, linearity (§5).
5. **Hệ thức truy hồi** — substitution, recursion tree, Master Theorem (§6).
6. **Mô hình tính toán** — RAM model (§7).

Sau đó mới vào **phân tích tiệm cận** ($O, \Omega, \Theta$) và các **thuật toán cụ thể** (sort, search, graph, DP).

---

## 11. Tài liệu tham khảo kinh điển

| Sách                                                    | Nội dung                            | Mức độ         |
| ------------------------------------------------------- | ----------------------------------- | -------------- |
| **CLRS** — *Introduction to Algorithms*                 | Toàn diện nhất, có cả nền tảng toán | Trung bình–khó |
| **Kleinberg & Tardos** — *Algorithm Design*             | Trực quan, nhiều ví dụ thiết kế     | Trung bình     |
| **Sedgewick & Wayne** — *Algorithms*                    | Thực hành, code cụ thể              | Dễ–trung bình  |
| **Dasgupta, Papadimitriou, Vazirani** — *Algorithms*    | Ngắn gọn, sâu sắc                   | Khó            |
| **Rosen** — *Discrete Mathematics and Its Applications* | Nền tảng toán rời rạc               | Dễ–trung bình  |
| **Mitzenmacher & Upfal** — *Probability and Computing*  | Xác suất cho thuật toán             | Trung bình–khó |

---

## Tóm tắt 1 dòng cho mỗi phần

- **Log:** đổi cơ số = nhân hằng số → mọi log cùng bậc.
- **Lũy thừa:** $\log^k n = o(n^\varepsilon) = o(c^n) = o(n!)$.
- **Tổng:** cấp số cộng $\Theta(n^2)$, cấp số nhân $\Theta(r^n)$, harmonic $\Theta(\log n)$.
- **Quy nạp:** base + step → kết luận.
- **Phản chứng:** giả sử sai → mâu thuẫn.
- **CTDL:** chi phí thao tác cộng vào tổng phân tích.
- **Xác suất:** indicator + linearity of expectation = vũ khí average-case.
- **Hệ thức truy hồi:** substitution / recursion tree / Master Theorem.
- **RAM model:** mọi phép toán cơ bản $O(1)$ trên word.

---

Nếu bạn muốn, mình có thể đi sâu vào bất kỳ mục nào — ví dụ:
- **Chứng minh đầy đủ** cận dưới $\Omega(n\log n)$ của comparison sort (dùng cây quyết định + Stirling).
- **Phân tích Quick Sort** average-case chi tiết (dùng indicator + linearity).
- **Giải hệ thức truy hồi** bằng phương pháp thế với ví dụ đầy đủ.
- **Bài tập nền tảng** (tính tổng, chứng minh tiệm cận, chứng minh quy nạp) kèm lời giải.