# Omega Notation ($\Omega$) — Lý thuyết chi tiết + Ví dụ

## 1. Omega là gì?

**Omega notation** (ký hiệu $\Omega$ lớn) là công cụ mô tả **cận dưới tiệm cận** của một hàm — trả lời câu hỏi:

> "Hàm $f(n)$ **tăng không chậm hơn** hàm $g(n)$ (sai khác một hằng số) khi $n \to \infty$ không?"

Nói cách khác, $\Omega$ cho biết **ngưỡng tối thiểu** — hàm $f$ **phải** tăng ít nhất nhanh bằng $g$.

### Trực giác

$$
f(n) = \Omega(g(n)) \quad \Longleftrightarrow \quad f \text{ tăng nhanh hơn hoặc bằng } g \text{ về bậc}
$$

**Loại suy số thực:** $\Omega$ giống như dấu $\ge$ trong so sánh số.

**Ví dụ trực giác:** Nếu thuật toán là $\Omega(n\log n)$, thì dù cài đặt tốt đến đâu, nó **không thể** chạy nhanh hơn $n\log n$ (sai khác hằng số). Đây là **giới hạn dưới** — không thể vượt qua.

> **Lý thuyết bên cạnh:** $\Omega$ là **đối ngẫu** của $O$ qua phép transpose: $f = O(g) \Leftrightarrow g = \Omega(f)$. Vì vậy mọi kiến thức về $O$ đều "dịch" được sang $\Omega$ bằng cách đảo chiều bất đẳng thức. Nhưng $\Omega$ có **vai trò riêng** — nó dùng để chứng minh **lower bound** (cận dưới), điều mà $O$ không làm được.

---

## 2. Định nghĩa hình thức

Cho $f(n)$ và $g(n)$ là các hàm xác định trên số nguyên dương, **không âm** với $n$ đủ lớn.

$$
\boxed{f(n) = \Omega(g(n)) \iff \exists\, c > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le c\,g(n) \le f(n)}
$$

**Đọc là:** "$f(n)$ là omega của $g(n)$" hoặc "$f(n)$ thuộc $\Omega(g(n))$".

### Phân tích các thành phần

| Thành phần | Ý nghĩa |
|---|---|
| $\exists c > 0$ | Tồn tại **một** hằng số nhân dương |
| $\exists n_0 > 0$ | Tồn tại **một** ngưỡng kích thước |
| $\forall n \ge n_0$ | Với **mọi** $n$ từ ngưỡng trở lên |
| $0 \le c\,g(n) \le f(n)$ | $f$ **bị chặn dưới** bởi $c\,g$ |

### So sánh trực tiếp với $O$

| | $O$ | $\Omega$ |
|---|---|---|
| Bất đẳng thức | $f(n) \le c\,g(n)$ | $f(n) \ge c\,g(n)$ |
| Ý nghĩa | $f$ bị chặn **trên** | $f$ bị chặn **dưới** |
| Loại suy | $\le$ | $\ge$ |
| Transpose | $f = O(g) \Leftrightarrow g = \Omega(f)$ | |

**Chỉ khác nhau ở chiều bất đẳng thức.** Mọi kỹ thuật chứng minh $O$ đều dùng được cho $\Omega$ — chỉ cần đảo chiều.

### Ký hiệu tập hợp (chính xác hơn)

$$
\Omega(g(n)) = \{f(n) : \exists c > 0, n_0 > 0, \forall n \ge n_0,\ 0 \le c\,g(n) \le f(n)\}
$$

Chính xác phải viết $f(n) \in \Omega(g(n))$, không phải $=$. Nhưng quy ước phổ biến dùng $=$.

### Phủ định (để chứng minh $f \ne \Omega(g)$)

$$
f(n) \ne \Omega(g(n)) \iff \forall c > 0,\ \forall n_0 > 0,\ \exists n \ge n_0 : f(n) < c\,g(n)
$$

**Ý nghĩa:** Dù chọn $c$ nhỏ đến đâu, dù chọn $n_0$ lớn đến đâu, luôn tìm được $n$ đủ lớn để $f$ **thấp hơn** $c\,g$.

---

## 3. Cách đọc và viết Omega

### 3.1. Các cách viết tương đương

$$
f(n) = \Omega(g(n)), \quad f(n) \in \Omega(g(n)), \quad f \text{ là } \Omega(g), \quad f \succeq g
$$

### 3.2. Cách đọc trong thực tế

| Viết | Đọc |
|---|---|
| $T(n) = \Omega(n^2)$ | "T là omega của n bình phương" |
| $T(n) = \Omega(n\log n)$ | "T là omega của n log n" |
| $T(n) = \Omega(\log n)$ | "T là omega của log n" |
| $T(n) = \Omega(1)$ | "T là omega của một" (bị chặn dưới bởi hằng số) |

### 3.3. Omega **không** phải hàm chính xác

**Quan trọng:** $T(n) = \Omega(n^2)$ **không** có nghĩa $T(n) = n^2$. Nó chỉ nói $T$ **bị chặn dưới** bởi $n^2$ (sai khác hằng số).

Ví dụ, tất cả các hàm sau đều là $\Omega(n^2)$:

$$
n^2, \quad 3n^2, \quad n^2 + 100n, \quad n^3, \quad 2^n, \quad n!
$$

Nhưng chỉ $n^2, 3n^2, n^2 + 100n$ là **chặt** (tức $\Theta(n^2)$).

### 3.4. Omega chặt vs lỏng

| Tuyên bố | Đúng? | Chặt? |
|---|---|---|
| $3n^2 + 5n = \Omega(n^2)$ | ✅ | ✅ (chặt) |
| $3n^2 + 5n = \Omega(n)$ | ✅ | ❌ (lỏng) |
| $3n^2 + 5n = \Omega(n^3)$ | ❌ | — |

**Nguyên tắc:** Luôn cố gắng tìm Omega **chặt nhất** (tức $\Theta$ nếu có thể). Nhưng khi chỉ cần **bảo đảm tối thiểu**, Omega lỏng vẫn hữu ích.

---

## 4. Tại sao Omega quan trọng? — Vai trò của lower bound

### 4.1. Omega trả lời câu hỏi ngược với Big O

| Câu hỏi | Ký hiệu |
|---|---|
| "Thuật toán này **nhanh nhất** có thể là bao nhiêu?" | $O$ (cận trên) |
| "Bài toán này **không thể** giải nhanh hơn bao nhiêu?" | $\Omega$ (cận dưới) |

**Big O** mô tả **thuật toán** (upper bound).
**Omega** mô tả **bài toán** (lower bound) — giới hạn của mọi thuật toán có thể.

### 4.2. Ví dụ kinh điển — Comparison Sort

**Bài toán:** Sắp xếp $n$ phần tử bất kỳ, chỉ dùng phép **so sánh**.

**Định lý:** Mọi thuật toán sắp xếp dựa trên so sánh đều có worst case $\Omega(n\log n)$.

**Chứng minh (ý tưởng — cây quyết định):**

- Có $n!$ hoán vị có thể của input.
- Mỗi lá của cây quyết định tương ứng với một hoán vị.
- Cây nhị phân có $n!$ lá phải có chiều cao ít nhất $\log_2(n!)$.
- Theo xấp xỉ Stirling: $\log_2(n!) = \Theta(n\log n)$.

**Kết luận:** Không thuật toán so sánh nào có thể nhanh hơn $\Omega(n\log n)$. Merge Sort và Heap Sort đạt $O(n\log n)$ — **tối ưu** (vì upper bound gặp lower bound).

> **Lý thuyết bên cạnh:** Đây là **ứng dụng quan trọng nhất** của Omega — chứng minh **tối ưu**. Khi upper bound ($O$) của một thuật toán **gặp** lower bound ($\Omega$) của bài toán, ta biết thuật toán đó **không thể cải thiện** về mặt tiệm cận. Đây là kết luận mạnh — không phải "chưa ai tìm được thuật toán tốt hơn", mà là "**không ai có thể** tìm được".

### 4.3. Các lower bound nổi tiếng

| Bài toán | Lower bound | Thuật toán đạt |
|---|---|---|
| Comparison sort | $\Omega(n\log n)$ | Merge Sort, Heap Sort |
| Tìm max trong mảng | $\Omega(n)$ | Duyệt tuyến tính |
| Tìm phần tử trong mảng chưa sắp xếp | $\Omega(n)$ | Linear Search |
| Tìm kiếm trong mảng đã sắp xếp | $\Omega(\log n)$ | Binary Search |
| Nhân ma trận | $\Omega(n^2)$ (hiện biết) | Strassen: $O(n^{2.81})$ |
| Phát hiện phần tử trùng | $\Omega(n\log n)$ (comparison) | Sort rồi duyệt |

**Lưu ý:** Lower bound có thể **lỏng** — ví dụ nhân ma trận, lower bound hiện biết là $\Omega(n^2)$ nhưng thuật toán tốt nhất là $O(n^{2.81})$ — có khoảng trống. Đây là **bài toán mở** nổi tiếng.

### 4.4. Omega cho thuật toán cụ thể

Ngoài lower bound bài toán, Omega còn dùng để mô tả **best case** của thuật toán:

- Insertion Sort: best case $\Omega(n)$ (mảng đã sắp xếp).
- Linear Search: best case $\Omega(1)$ (phần tử ở đầu).
- Quick Sort: best case $\Omega(n\log n)$.

---

## 5. Các lớp Omega thường gặp

### 5.1. Bảng chi tiết

| Omega | Tên | Ý nghĩa | Ví dụ |
|---|---|---|---|
| $\Omega(1)$ | Hằng số | Bị chặn dưới bởi hằng số | Bất kỳ thuật toán nào |
| $\Omega(\log n)$ | Logarit | Ít nhất logarit | Binary Search (worst) |
| $\Omega(n)$ | Tuyến tính | Ít nhất tuyến tính | Duyệt mảng |
| $\Omega(n\log n)$ | Tuyến tính-log | Ít nhất $n\log n$ | Comparison sort |
| $\Omega(n^2)$ | Bậc hai | Ít nhất bậc hai | Bubble sort, nhân ma trận naive |
| $\Omega(n^3)$ | Bậc ba | Ít nhất bậc ba | Matrix chain DP |
| $\Omega(2^n)$ | Hàm mũ | Ít nhất hàm mũ | Fibonacci naive (worst) |
| $\Omega(n!)$ | Giai thừa | Ít nhất giai thừa | TSP brute force |

### 5.2. Ví dụ code

**$\Omega(1)$ — mọi thuật toán đều ít nhất hằng số:**
```text
x = A[1]    // dù n lớn đến đâu, vẫn 1 phép
```
Mọi thuật toán đều là $\Omega(1)$ (vì luôn có ít nhất một phép toán).

**$\Omega(\log n)$ — Binary Search worst case:**
```text
while low <= high:
    mid = (low + high) / 2
    ...
```
Với $x$ không có trong mảng, số bước là $\lceil \log_2 n \rceil$ — **đúng** $\Theta(\log n)$, tức cả $O$ lẫn $\Omega$.

**$\Omega(n)$ — tìm max:**
```text
max = A[1]
for i = 2 to n:
    if A[i] > max: max = A[i]
```
**Mọi** phần tử phải được xem ít nhất một lần ⇒ $\Omega(n)$.

**$\Omega(n\log n)$ — comparison sort:**
Mọi thuật toán sắp xếp dựa trên so sánh đều phải thực hiện $\Omega(n\log n)$ phép so sánh trong worst case (chứng minh ở §4.2).

**$\Omega(n^2)$ — nhân ma trận naive:**
```text
for i = 1 to n:
    for j = 1 to n:
        C[i][j] = sum(A[i][k] * B[k][j] for k = 1..n)
```
Có $n^2$ phần tử kết quả, mỗi phần tử là tổng $n$ số hạng ⇒ ít nhất $n^2$ phép ghi và $\Omega(n^2)$ phép đọc.

**$\Omega(2^n)$ — Fibonacci naive:**
```text
Fib(n):
    if n <= 1: return n
    return Fib(n-1) + Fib(n-2)
```
Cây đệ quy có $\approx \varphi^n$ nút với $\varphi \approx 1.618$ ⇒ $\Omega(\varphi^n)$ ⇒ $\Omega(2^n)$ (lỏng hơn).

---

## 6. Quy tắc xác định Omega từ code

### 6.1. Quy tắc cơ bản (đối ngẫu với $O$)

**Quy tắc 1 — Tuần tự:**

$$
\Omega(f) + \Omega(g) = \Omega(\min(f, g))
$$

**Chú ý:** Khác với $O$! Với $\Omega$, khi nối tiếp, ta lấy **min** (cận dưới yếu nhất trong các đoạn).

**Ví dụ:**
```text
for i = 1 to n: doing O(1)     // Ω(n) — vì phải duyệt n lần
for i = 1 to 1: doing O(1)     // Ω(1)
```
Tổng: $\Omega(n) + \Omega(1) = \Omega(1)$ (min). **Nhưng** thực tế tổng là $n + 1$ phép — vẫn $\Omega(n)$ (vì $\Omega(1) \subset \Omega(n)$, nên $\Omega(n)$ cũng đúng). **Cẩn thận:** dùng $\Omega(n)$ vẫn đúng vì $\Omega(1)$ là cận dưới yếu hơn.

**Đúng hơn:** nếu biết chính xác hơn, dùng $\Theta$ cho từng đoạn rồi cộng: $\Theta(n) + \Theta(1) = \Theta(n)$.

**Quy tắc 2 — Lồng nhau:**

$$
\Omega(f) \cdot \Omega(g) = \Omega(f \cdot g)
$$

Vòng lồng: **nhân** Omega. (Giống $O$.)

**Ví dụ:**
```text
for i = 1 to n:         // Ω(n)
    for j = 1 to n:     // Ω(n)
        doing O(1)
```
Tổng: $\Omega(n^2)$.

**Quy tắc 3 — Điều kiện:**

$$
\min(\text{nhánh if}, \text{nhánh else})
$$

**Chú ý:** Với $\Omega$, lấy **min** — vì ta cần cận dưới **bảo đảm**, không phụ thuộc nhánh.

**Ví dụ:**
```text
if condition:           // Ω(n)
    for i = 1 to n: ...
else:                   // Ω(1)
    doing O(1)
```
Tổng: $\Omega(1)$ — vì nhánh else có thể xảy ra.

**Quy tắc 4 — Bỏ hằng số, bỏ bậc thấp:**

$$
c \cdot \Omega(f) = \Omega(f), \quad \Omega(n^2 + n) = \Omega(n^2)
$$

**Quy tắc 5 — Log: bỏ cơ số:** $\Omega(\log_2 n) = \Omega(\ln n)$.

### 6.2. Ví dụ tổng hợp — phân tích cả $O$ và $\Omega$

```text
Function F(n):
    sum = 0                          // Ω(1), O(1)
    for i = 1 to n:                  // Ω(n), O(n)
        sum += A[i]
    for i = 1 to n:                  // Ω(n^2), O(n^2)
        for j = 1 to n:
            sum += A[i] * A[j]
    if sum > 0:                      // nhánh phụ thuộc
        for i = 1 to n:              // Ω(n), O(n)
            sum -= 1
    return sum                       // Ω(1), O(1)
```

**Phân tích:**

- **Best case** (sum ≤ 0): chỉ chạy hai đoạn đầu → $\Omega(n)$.
- **Worst case** (sum > 0): chạy cả ba → $O(n^2)$.
- **Bậc chính** do đoạn 3 (lồng nhau) quyết định: $\Theta(n^2)$ khi sum > 0, $\Theta(n)$ khi sum ≤ 0.

**Cận dưới tổng thể:** $\Omega(n)$ (best case).
**Cận trên tổng thể:** $O(n^2)$ (worst case).

**Kết luận:** $T(n) = \Omega(n)$ và $T(n) = O(n^2)$. **Không** thể nói $\Theta$ cho toàn hàm vì best/worst khác bậc.

### 6.3. Bẫy: Omega của vòng lặp có điều kiện

```text
for i = 1 to n:
    if A[i] > 0:
        doing O(1) work
```

**Số phép `doing O(1)` phụ thuộc dữ liệu:**

- **Best case** (mảng toàn số âm): 0 lần → $\Omega(1)$ cho phần thân.
- **Worst case** (mảng toàn dương): $n$ lần → $O(n)$.

**Nhưng** vòng `for` **luôn** chạy $n$ lần ⇒ tổng thời gian luôn $\Omega(n)$ (vì vòng lặp phải duyệt $n$ phần tử, ít nhất để kiểm tra điều kiện).

**Đây là điểm quan trọng:** Omega của **cấu trúc điều khiển** (vòng lặp) khác Omega của **thân** (điều kiện).

### 6.4. Bẫy: giảm kích thước mỗi bước

```text
i = n
while i > 1:
    doing O(1)
    i = i / 2
```

$i$ nhận $n, n/2, n/4, \ldots, 1$. Số bước: $\log_2 n$.

Cả $O$ lẫn $\Omega$ đều là $\Theta(\log n)$ — vì số bước **chính xác** là $\lfloor \log_2 n \rfloor$.

---

## 7. Chứng minh Omega bằng định nghĩa

### 7.1. Phương pháp "nới lỏng" (đảo chiều)

**Ý tưởng:** Thay các số hạng khó bằng phiên bản **nhỏ hơn** (với $\Omega$) sao cho dễ xử lý, rồi tìm hằng số.

**Mẹo:** Không cần $c$ tối ưu — chỉ cần **tồn tại** một $c$ hợp lệ. Không cần chứng minh chặt — chỉ cần một cận dưới đúng.

### 7.2. Ví dụ 1 — $3n^2 + 5n + 7 = \Omega(n^2)$

**Cần tìm:** $c > 0, n_0$ sao cho $3n^2 + 5n + 7 \ge c\,n^2$ với $n \ge n_0$.

**Nới lỏng** (với $n \ge 1$, các số hạng dương):

$$
3n^2 + 5n + 7 \ge 3n^2
$$

**Chọn:** $c = 3$, $n_0 = 1$. $\blacksquare$

**Kiểm tra:** $n=1$: $3+5+7 = 15 \ge 3 \cdot 1 = 3$. Đúng. ✓

### 7.3. Ví dụ 2 — $100n + 5 = \Omega(n)$

**Cần tìm:** $c > 0, n_0$ sao cho $100n + 5 \ge c\,n$.

**Nới lỏng:** $100n + 5 \ge 100n$ (vì $5 \ge 0$).

**Chọn:** $c = 100$, $n_0 = 1$. $\blacksquare$

**Lưu ý:** Không cần "bẫy hằng số 5" như với $O$ — vì $5$ đẩy hàm **lên**, không kéo xuống.

### 7.4. Ví dụ 3 — $n^3 - 5n = \Omega(n^3)$?

**Phân tích:** Cần $n^3 - 5n \ge c\,n^3$ với $n \ge n_0$.

Chia $n^3$: $1 - 5/n^2 \ge c$.

Khi $n \to \infty$, vế trái $\to 1$. Vậy với $n \ge 4$: $1 - 5/16 = 11/16$.

**Chọn:** $c = 1/2$, $n_0 = 4$:

$$
n^3 - 5n = n^3(1 - 5/n^2) \ge n^3(1 - 5/16) = \frac{11}{16} n^3 \ge \frac{1}{2} n^3
$$

$\blacksquare$

### 7.5. Ví dụ 4 — $n \ne \Omega(n^2)$ (phản chứng)

**Giả sử** $n = \Omega(n^2)$. Khi đó tồn tại $c > 0, n_0$ sao cho:

$$
n \ge c\,n^2, \quad \forall n \ge n_0
$$

Chia hai vế cho $n > 0$:

$$
1 \ge c\,n, \quad \forall n \ge n_0
$$

Nhưng với $n = \max(n_0, \lceil 1/c \rceil + 1)$, ta có $c\,n > 1$ — **mâu thuẫn**. Vậy $n \ne \Omega(n^2)$. $\blacksquare$

### 7.6. Ví dụ 5 — $n^3 = \Omega(n^2)$

**Cần tìm:** $c > 0, n_0$ sao cho $n^3 \ge c\,n^2$.

Chia $n^2$: $n \ge c$.

**Chọn:** $c = 1$, $n_0 = 1$. $\blacksquare$

**Tổng quát:** $n^a = \Omega(n^b)$ khi $a \ge b$.

### 7.7. Ví dụ 6 — $2^n = \Omega(n^{100})$

**Cần tìm:** $c > 0, n_0$ sao cho $2^n \ge c \cdot n^{100}$.

**Ý tưởng:** Hàm mũ tăng nhanh hơn đa thức. Có thể chứng minh bằng quy nạp hoặc L'Hôpital:

$$
\lim_{n \to \infty} \frac{2^n}{n^{100}} = \infty
$$

Vậy tồn tại $n_0$ sao cho với $n \ge n_0$, $2^n / n^{100} \ge 1$. Chọn $c = 1$. $\blacksquare$

### 7.8. Ví dụ 7 — $\log(n!) = \Omega(n\log n)$

**Cần tìm:** $c > 0, n_0$ sao cho $\log(n!) \ge c \cdot n\log n$.

**Chặn dưới:** Chỉ lấy nửa sau của tổng:

$$
\log(n!) = \sum_{i=1}^{n} \log i \ge \sum_{i=\lceil n/2 \rceil}^{n} \log i \ge \sum_{i=\lceil n/2 \rceil}^{n} \log(n/2)
$$

Số số hạng: $\lfloor n/2 \rfloor + 1$. Vậy:

$$
\log(n!) \ge \frac{n}{2} \log(n/2) = \frac{n}{2}(\log n - 1)
$$

Với $n \ge 4$: $\log n - 1 \ge \frac{1}{2}\log n$. Vậy:

$$
\log(n!) \ge \frac{n}{2} \cdot \frac{1}{2}\log n = \frac{1}{4} n\log n
$$

**Chọn:** $c = 1/4$, $n_0 = 4$. $\blacksquare$

> **Lý thuyết bên cạnh:** Kỹ thuật "chặn dưới bằng nửa sau" rất hiệu quả với tổng có số hạng tăng. Nó tránh được việc phải tính chính xác tổng — chỉ cần chặn dưới bằng một phần của tổng.

---

## 8. Chứng minh Omega bằng giới hạn

### 8.1. Công thức

Khi $g(n) > 0$ với $n$ đủ lớn, xét:

$$
L = \lim_{n \to \infty} \frac{f(n)}{g(n)}
$$

| $L$ | Kết luận |
|---|---|
| $L = \infty$ | $f = \omega(g)$ ⇒ $f = \Omega(g)$ (mạnh hơn $\Omega$) |
| $0 < L < \infty$ | $f = \Theta(g)$ ⇒ $f = \Omega(g)$ và $f = O(g)$ |
| $L = 0$ | $f \ne \Omega(g)$ (thực ra $f = o(g)$) |
| không tồn tại | Cần xét $\liminf$ |

**Lưu ý:** Với $\Omega$, ta cần **$L > 0$** hoặc $L = \infty$. Nếu $L = 0$, $f$ nhỏ hơn $g$ về bậc ⇒ không phải $\Omega$.

### 8.2. Ví dụ 8 — $5n^2 + 3n = \Omega(n^2)$

$$
L = \lim_{n\to\infty} \frac{5n^2 + 3n}{n^2} = 5
$$

$0 < 5 < \infty$ ⇒ $5n^2 + 3n = \Theta(n^2)$ ⇒ $= \Omega(n^2)$. $\blacksquare$

### 8.3. Ví dụ 9 — $n^2 = \Omega(n)$

$$
L = \lim_{n\to\infty} \frac{n^2}{n} = \infty
$$

$L = \infty$ ⇒ $n^2 = \omega(n)$ ⇒ $= \Omega(n)$. $\blacksquare$

### 8.4. Ví dụ 10 — $n \ne \Omega(n^2)$

$$
L = \lim_{n\to\infty} \frac{n}{n^2} = 0
$$

$L = 0$ ⇒ $n \ne \Omega(n^2)$ (thực ra $n = o(n^2)$). $\blacksquare$

### 8.5. Ví dụ 11 — $2^n = \Omega(n^3)$

$$
L = \lim_{n\to\infty} \frac{2^n}{n^3} = \infty \quad \text{(L'Hôpital 3 lần)}
$$

$L = \infty$ ⇒ $2^n = \omega(n^3)$ ⇒ $= \Omega(n^3)$. $\blacksquare$

### 8.6. Ví dụ 12 — $n\log n = \Omega(n)$

$$
L = \lim_{n\to\infty} \frac{n\log n}{n} = \lim_{n\to\infty} \log n = \infty
$$

$L = \infty$ ⇒ $n\log n = \omega(n)$ ⇒ $= \Omega(n)$. $\blacksquare$

---

## 9. Tính chất của Omega

### 9.1. Tính phản xạ (reflexivity)

$$
f(n) = \Omega(f(n))
$$

**Chứng minh:** Chọn $c = 1$, $n_0 = 1$. $\blacksquare$

### 9.2. Tính bắc cầu (transitivity)

Nếu $f = \Omega(g)$ và $g = \Omega(h)$ thì $f = \Omega(h)$.

**Chứng minh:** Có $c_1, n_1$: $f \ge c_1 g$ ($n \ge n_1$); $c_2, n_2$: $g \ge c_2 h$ ($n \ge n_2$). Chọn $c = c_1 c_2$, $n_0 = \max(n_1, n_2)$:

$$
f(n) \ge c_1 g(n) \ge c_1 c_2 h(n) = c\,h(n), \quad \forall n \ge n_0
$$

$\blacksquare$

### 9.3. Transpose symmetry — cầu nối với $O$

$$
f = O(g) \iff g = \Omega(f)
$$

Đây là **tính chất quan trọng nhất** liên kết $O$ và $\Omega$. Mọi chứng minh $O$ đều "dịch" được sang $\Omega$ bằng cách đảo hai hàm.

### 9.4. Quan hệ với $\Theta$

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

### 9.5. Quy tắc cộng

$$
\Omega(f) + \Omega(g) = \Omega(\min(f, g))
$$

**Chú ý:** Khác $O$! Với $\Omega$, lấy **min** — vì ta cần cận dưới **bảo đảm** cho **mọi** đoạn.

### 9.6. Quy tắc nhân

$$
\Omega(f) \cdot \Omega(g) = \Omega(f \cdot g)
$$

### 9.7. Quy tắc hằng số

$$
c \cdot \Omega(f) = \Omega(f), \quad \forall c > 0
$$

### 9.8. Bảng tóm tắt tính chất

| Tính chất | Công thức |
|---|---|
| Phản xạ | $f = \Omega(f)$ |
| Bắc cầu | $f = \Omega(g), g = \Omega(h) \Rightarrow f = \Omega(h)$ |
| Transpose | $f = O(g) \Leftrightarrow g = \Omega(f)$ |
| Cộng | $\Omega(f) + \Omega(g) = \Omega(\min(f, g))$ |
| Nhân | $\Omega(f) \cdot \Omega(g) = \Omega(fg)$ |
| Hằng số | $c \cdot \Omega(f) = \Omega(f)$ |
| Giao với $O$ | $\Theta = O \cap \Omega$ |

---

## 10. Omega cho đệ quy

### 10.1. Hệ thức truy hồi và Master Theorem

Với $T(n) = aT(n/b) + f(n)$, đặt $x = n^{\log_b a}$:

1. $f(n) = O(x^{1-\varepsilon})$ ⇒ $T(n) = \Theta(x)$.
2. $f(n) = \Theta(x)$ ⇒ $T(n) = \Theta(x \log n)$.
3. $f(n) = \Omega(x^{1+\varepsilon})$ (và điều kiện chính quy) ⇒ $T(n) = \Theta(f(n))$.

**Kết quả Master Theorem luôn là $\Theta$** — nên vừa cho $O$ vừa cho $\Omega$.

### 10.2. Ví dụ 13 — Merge Sort

$$
T(n) = 2T(n/2) + n
$$

Master Theorem Case 2: $T(n) = \Theta(n\log n)$ ⇒ $T(n) = \Omega(n\log n)$.

### 10.3. Ví dụ 14 — Binary Search

$$
T(n) = T(n/2) + 1
$$

Master Theorem Case 2: $T(n) = \Theta(\log n)$ ⇒ $T(n) = \Omega(\log n)$.

### 10.4. Ví dụ 15 — Fibonacci naive (worst case)

$$
T(n) = T(n-1) + T(n-2) + 1
$$

**Không áp dụng Master Theorem.** Dùng cây đệ quy:

- Số nút $\ge$ số nút trên nhánh $T(n-1)$ (nếu chỉ xét nhánh dài nhất, ta được $n$).
- Chặt hơn: cây đệ quy có $\approx \varphi^n$ nút với $\varphi \approx 1.618$.

Vậy:
$$
T(n) = \Omega(\varphi^n) \quad \text{và} \quad T(n) = O(\varphi^n)
$$

Tức $T(n) = \Theta(\varphi^n)$. Vì $\varphi > 1$, cũng có $T(n) = \Omega(2^{n/2})$ (cận dưới yếu hơn).

### 10.5. Ví dụ 16 — Tìm max đệ quy

$$
T(n) = 2T(n/2) + 1
$$

Master Theorem Case 1 ($x = n$, $f(n) = 1 = O(n^{1-\varepsilon})$): $T(n) = \Theta(n)$ ⇒ $\Omega(n)$.

> **Lý thuyết bên cạnh:** Master Theorem cho kết quả $\Theta$ — mạnh nhất trong 5 ký hiệu. Vì vậy **luôn cố gắng áp dụng Master Theorem** khi hệ thức có dạng $aT(n/b) + f(n)$. Khi không áp dụng được (ví dụ $T(n) = T(n-1) + \ldots$), dùng cây đệ quy hoặc phương pháp thế.

---

## 11. Omega cho cấu trúc dữ liệu

### 11.1. Bảng tra nhanh (lower bound)

| CTDL | Truy cập | Tìm | Chèn | Xóa |
|---|---|---|---|---|
| **Array** | $\Omega(1)$ | $\Omega(n)$ | $\Omega(n)$ | $\Omega(n)$ |
| **Linked List** | $\Omega(n)$ | $\Omega(n)$ | $\Omega(1)$* | $\Omega(1)$* |
| **Stack** | $\Omega(n)$ | $\Omega(n)$ | $\Omega(1)$ | $\Omega(1)$ |
| **Queue** | $\Omega(n)$ | $\Omega(n)$ | $\Omega(1)$ | $\Omega(1)$ |
| **BST** | $\Omega(\log n)$ (avg), $\Omega(1)$ (best) | $\Omega(\log n)$ avg | $\Omega(\log n)$ avg | $\Omega(\log n)$ avg |
| **Hash Table** | — | $\Omega(1)$ | $\Omega(1)$ | $\Omega(1)$ |
| **Heap** | $\Omega(1)$ (min/max) | $\Omega(n)$ | $\Omega(\log n)$ | $\Omega(\log n)$ |

*Chèn/xóa linked list là $\Omega(1)$ chỉ khi đã có con trỏ đến vị trí. Nếu phải tìm, là $\Omega(n)$.

### 11.2. Nhận xét

Bảng trên là **lower bound** — tức là **tối thiểu** thuật toán phải thực hiện. Ví dụ:

- Array truy cập ít nhất $\Omega(1)$ — đúng, vì cần ít nhất một phép đọc.
- Array tìm kiếm ít nhất $\Omega(n)$ — đúng, vì có thể phải duyệt toàn bộ.
- Hash table tìm kiếm ít nhất $\Omega(1)$ — đúng, vì cần ít nhất một phép băm và một phép so sánh.

**Chú ý:** Lower bound của CTDL thường **lỏng hơn** lower bound của bài toán trừu tượng. Ví dụ: tìm max trong mảng bất kỳ là $\Omega(n)$ — nhưng nếu có CTDL đặc biệt (như max-heap), tìm max có thể là $\Omega(1)$.

### 11.3. Ví dụ — Heap

- **Tìm max** (max-heap): $\Omega(1)$ (chỉ cần đọc gốc) và $O(1)$ ⇒ $\Theta(1)$.
- **Chèn:** $\Omega(\log n)$ (phải duy trì tính chất heap) và $O(\log n)$ ⇒ $\Theta(\log n)$.
- **Xóa max:** $\Omega(\log n)$ (tương tự) và $O(\log n)$ ⇒ $\Theta(\log n)$.
- **Tìm phần tử bất kỳ:** $\Omega(n)$ (phải duyệt) — **không** tối ưu.

---

## 12. Omega cho thuật toán phổ biến

### 12.1. Sắp xếp

| Thuật toán | Best | Average | Worst |
|---|---|---|---|
| Bubble Sort | $\Omega(n)$ | $\Omega(n^2)$ | $\Omega(n^2)$ |
| Insertion Sort | $\Omega(n)$ | $\Omega(n^2)$ | $\Omega(n^2)$ |
| Selection Sort | $\Omega(n^2)$ | $\Omega(n^2)$ | $\Omega(n^2)$ |
| Merge Sort | $\Omega(n\log n)$ | $\Omega(n\log n)$ | $\Omega(n\log n)$ |
| Quick Sort | $\Omega(n\log n)$ | $\Omega(n\log n)$ | $\Omega(n^2)$ |
| Heap Sort | $\Omega(n\log n)$ | $\Omega(n\log n)$ | $\Omega(n\log n)$ |
| Counting Sort | $\Omega(n+k)$ | $\Omega(n+k)$ | $\Omega(n+k)$ |

**Quan sát:**

- **Selection Sort, Merge Sort, Heap Sort:** cả ba case **giống nhau** — không phụ thuộc dữ liệu.
- **Insertion Sort, Bubble Sort:** best $\Omega(n)$ — adaptive.
- **Quick Sort:** best và average $\Omega(n\log n)$, worst $\Omega(n^2)$.

### 12.2. Tìm kiếm

| Thuật toán | Best | Average | Worst |
|---|---|---|---|
| Linear Search | $\Omega(1)$ | $\Omega(n)$ | $\Omega(n)$ |
| Binary Search | $\Omega(1)$ | $\Omega(\log n)$ | $\Omega(\log n)$ |
| Hash Search | $\Omega(1)$ | $\Omega(1)$ | $\Omega(n)$ |

### 12.3. Đồ thị

| Thuật toán | Lower bound (worst) | Upper bound |
|---|---|---|
| BFS | $\Omega(V + E)$ | $O(V + E)$ |
| DFS | $\Omega(V + E)$ | $O(V + E)$ |
| Dijkstra (heap) | $\Omega((V+E)\log V)$ | $O((V+E)\log V)$ |
| Bellman-Ford | $\Omega(VE)$ | $O(VE)$ |
| Floyd-Warshall | $\Omega(V^3)$ | $O(V^3)$ |

**Lưu ý:** Với hầu hết thuật toán đồ thị, lower bound **gặp** upper bound (tức $\Theta$) — vì ta biết thuật toán là tối ưu hoặc gần tối ưu. Trường hợp ngoại lệ: **nhân ma trận** (lower bound hiện biết $\Omega(n^2)$, upper bound tốt nhất $O(n^{2.81})$ — có khoảng trống).

### 12.4. Quy hoạch động

| Bài toán | Lower bound | Upper bound |
|---|---|---|
| Fibonacci (tối ưu) | $\Omega(n)$ | $O(n)$ |
| LCS | $\Omega(nm)$ | $O(nm)$ |
| Knapsack 0/1 | $\Omega(nW)$ | $O(nW)$ |
| Matrix Chain | $\Omega(n^3)$ | $O(n^3)$ |
| TSP (Held-Karp) | $\Omega(n^2 2^n)$ | $O(n^2 2^n)$ |

---

## 13. Omega với nhiều biến

### 13.1. Khi nào có nhiều biến?

Giống như $O$: đồ thị ($V, E$), ma trận ($n \times m$), chuỗi ($n, m$), knapsack ($n, W$).

### 13.2. Ví dụ — duyệt ma trận $n \times m$

```text
for i = 1 to n:
    for j = 1 to m:
        doing O(1)
```

$$
T(n, m) = \Omega(nm)
$$

Không phải $\Omega(n^2)$ trừ khi $m = n$.

### 13.3. Ví dụ — BFS trên đồ thị

$$
T(V, E) = \Omega(V + E)
$$

**Lý do:**

- Phải thăm **mọi** đỉnh ít nhất một lần ⇒ $\Omega(V)$.
- Phải duyệt **mọi** cạnh ít nhất một lần (với đồ thị liên thông) ⇒ $\Omega(E)$.
- Tổng: $\Omega(V + E)$.

**Cả upper và lower đều là $\Theta(V+E)$** — BFS là tối ưu.

### 13.4. Quy tắc biến

**Luôn viết $\Omega$ theo tất cả biến ảnh hưởng:**

- Đồ thị thưa: $\Omega(V+E) = \Omega(V)$ (vì $E = O(V)$).
- Đồ thị dày: $\Omega(V+E) = \Omega(V^2)$ (vì $E = O(V^2)$).

**Không** được tự ý bỏ biến nếu không có giả định.

---

## 14. Những hiểu nhầm phổ biến về Omega

### Hiểu nhầm 1 — "$\Omega$ là best case"

**Sai.** $\Omega$ là **cận dưới** — quan hệ giữa hai hàm. Nó **không** mô tả case.

**Đúng:** Có thể nói "best case $\Omega(n)$", "worst case $\Omega(n^2)$" — $\Omega$ áp dụng cho **mọi case**. Khi nói "thuật toán là $\Omega(n\log n)$", ta đang nói **lower bound của thuật toán** (trên mọi input, hoặc worst case — tùy ngữ cảnh).

### Hiểu nhầm 2 — "$\Omega$ là $O$ đảo ngược"

**Đúng một nửa.** $\Omega$ là $O$ đảo ngược qua **transpose**: $f = O(g) \Leftrightarrow g = \Omega(f)$. Nhưng **không** phải "đảo hai vế của cùng một bất đẳng thức":

- $f = O(g)$ nghĩa là $f \le c\,g$.
- $f = \Omega(g)$ nghĩa là $f \ge c\,g$.
- **Không** suy ra $f = O(g) \Leftrightarrow f = \Omega(g)$ (chỉ đúng khi $f = \Theta(g)$).

### Hiểu nhầm 3 — "$f = \Omega(g)$ nghĩa là $f$ luôn lớn hơn $g$"

**Sai.** $\Omega$ chỉ nói $f \ge c\,g$ với $n \ge n_0$. Với $n$ nhỏ, $f$ có thể nhỏ hơn $g$. Ví dụ: $n^2 = \Omega(n)$, nhưng với $n = 0.5$ thì $n^2 < n$.

### Hiểu nhầm 4 — "$f = \Omega(g)$ và $g = \Omega(h)$ suy ra $f = \Theta(h)$"

**Sai.** Ví dụ: $n^2 = \Omega(n)$ và $n = \Omega(\log n)$, nhưng $n^2 \ne \Theta(\log n)$ — chênh hai bậc.

**Đúng:** Nếu $f = \Omega(g)$ **và** $g = \Omega(f)$ thì $f = \Theta(g)$.

### Hiểu nhầm 5 — "$\Omega$ chỉ dùng cho lower bound của bài toán"

**Sai.** $\Omega$ dùng cho **mọi** cận dưới, bao gồm:

- Lower bound của bài toán (mạnh nhất — không thuật toán nào vượt qua).
- Best case của thuật toán (yếu hơn — chỉ nói thuật toán **có thể** nhanh).
- Worst case của thuật toán khi ta biết chính xác (thường là $\Theta$).

### Hiểu nhầm 6 — "$\Omega(f) + \Omega(g) = \Omega(\max(f,g))$"

**Sai.** Với $\Omega$, quy tắc cộng là **min**:

$$
\Omega(f) + \Omega(g) = \Omega(\min(f, g))
$$

**Lý do:** Cận dưới phải đúng cho **mọi** đoạn. Nếu đoạn 1 là $\Omega(n^2)$ và đoạn 2 là $\Omega(1)$, tổng **ít nhất** là $\Omega(1)$ — vì đoạn 2 có thể chỉ chạy hằng số bước.

**Nhưng:** Thực tế nếu biết chính xác hơn (dùng $\Theta$), ta có thể cộng: $\Theta(n^2) + \Theta(1) = \Theta(n^2)$.

### Hiểu nhầm 7 — "Mọi thuật toán đều là $\Omega(1)$"

**Đúng** — mọi thuật toán đều thực hiện ít nhất một phép toán. Nhưng cận dưới này **quá lỏng** — không hữu ích. Khi nói "thuật toán là $\Omega(f)$", ta ngụ ý $f$ là **cận dưới chặt nhất** có thể chứng minh.

### Hiểu nhầm 8 — "$\Omega(n^2)$ mạnh hơn $\Omega(n)$"

**Đúng về mặt cận dưới**, nhưng **không phải lúc nào cũng tốt hơn**. $\Omega(n^2)$ nói "ít nhất bậc hai", $\Omega(n)$ nói "ít nhất tuyến tính". Cái nào **chặt hơn** phụ thuộc vào bài toán:

- Nếu thuật toán thực sự là $\Theta(n^2)$: $\Omega(n^2)$ là **chặt**.
- Nếu thuật toán thực sự là $\Theta(n)$: $\Omega(n^2)$ là **sai** (thuật toán không thể $\ge n^2$).

**Khi nói lower bound, phải chọn cận dưới đúng và chặt nhất.**

### Hiểu nhầm 9 — "$\Omega(2^n)$ và $\Omega(n!)$ giống nhau"

**Sai.** $n! = \omega(2^n)$ — tức $n!$ tăng **nhanh hơn** $2^n$. Vậy $\Omega(n!)$ mạnh hơn $\Omega(2^n)$.

Ví dụ: TSP brute force là $\Omega(n!)$, mạnh hơn $\Omega(2^n)$ (mặc dù cả hai đều là hàm mũ/giai thừa).

### Hiểu nhầm 10 — "Lower bound luôn đạt được"

**Sai.** Lower bound có thể **không đạt được** bởi bất kỳ thuật toán nào biết:

- Nhân ma trận: lower bound hiện biết $\Omega(n^2)$, upper bound tốt nhất $O(n^{2.81})$ — có khoảng trống.
- So khớp chuỗi: lower bound $\Omega(n+m)$, upper bound tốt nhất $O(n+m)$ (KMP) — **đạt**.
- Sắp xếp: lower bound $\Omega(n\log n)$, upper bound tốt nhất $O(n\log n)$ (Merge Sort) — **đạt**.

**Khi lower bound gặp upper bound, ta biết thuật toán là tối ưu.**

> **Lý thuyết bên cạnh:** Khoảng trống giữa lower bound và upper bound là **động lực nghiên cứu** trong khoa học máy tính lý thuyết. Nhân ma trận là ví dụ nổi tiếng: từ $O(n^3)$ (naive) xuống $O(n^{2.81})$ (Strassen) xuống $O(n^{2.37})$ (Williams 2012) — nhưng lower bound vẫn là $\Omega(n^2)$. Câu hỏi "nhân ma trận có thể nhanh hơn $O(n^{2+\varepsilon})$ không?" vẫn mở.

---

## 15. Omega trong thực tế — ứng dụng lower bound

### 15.1. Chứng minh thuật toán là tối ưu

**Quy trình:**

1. Chứng minh upper bound: thuật toán của bạn là $O(f(n))$.
2. Chứng minh lower bound: bài toán là $\Omega(g(n))$.
3. Nếu $f = g$: thuật toán là **tối ưu** (về tiệm cận).

**Ví dụ — Merge Sort:**

- Upper: Merge Sort là $O(n\log n)$.
- Lower: Comparison sort là $\Omega(n\log n)$ (cây quyết định).
- Kết luận: Merge Sort là **tối ưu** trong lớp thuật toán so sánh.

**Ví dụ — Tìm max:**

- Upper: duyệt tuyến tính là $O(n)$.
- Lower: phải xem mọi phần tử ⇒ $\Omega(n)$.
- Kết luận: thuật toán duyệt là **tối ưu**.

### 15.2. Lower bound của bài toán cụ thể

| Bài toán | Lower bound | Thuật toán tối ưu |
|---|---|---|
| Comparison sort | $\Omega(n\log n)$ | Merge Sort, Heap Sort |
| Tìm max | $\Omega(n)$ | Duyệt tuyến tính |
| Tìm cả max và min | $\Omega(\lceil 3n/2 \rceil - 2)$ | Tournament |
| Tìm phần tử trong mảng chưa sắp xếp | $\Omega(n)$ | Linear Search |
| Tìm trong mảng đã sắp xếp | $\Omega(\log n)$ | Binary Search |
| Nhân ma trận | $\Omega(n^2)$ (hiện biết) | Strassen $O(n^{2.81})$ (chưa tối ưu) |
| Phát hiện phần tử trùng (comparison) | $\Omega(n\log n)$ | Sort rồi duyệt |

### 15.3. Khi lower bound không đạt được

**Ví dụ nhân ma trận:**

- Lower bound: $\Omega(n^2)$ (vì output có $n^2$ phần tử, mỗi phần tử cần ít nhất một phép ghi).
- Upper bound tốt nhất hiện biết: $O(n^{2.81})$ (Strassen), $O(n^{2.37})$ (Williams).
- **Khoảng trống:** $\Omega(n^2) \ne O(n^{2.37})$.

**Ý nghĩa:** Có thể còn thuật toán tốt hơn — hoặc có thể không. Đây là **bài toán mở**.

### 15.4. Adversary argument

Kỹ thuật mạnh để chứng minh lower bound.

**Ý tưởng:** Xây dựng một **"kẻ thù"** trả lời theo cách **làm chậm thuật toán nhất**.

**Ví dụ — Tìm max:**

- Adversary: luôn trả lời "phần tử mới lớn hơn" khi bị hỏi so sánh.
- Mỗi lần so sánh, thuật toán chỉ loại được **một** phần tử khỏi ứng viên max.
- Cần ít nhất $n-1$ phép so sánh để loại $n-1$ phần tử.
- Vậy lower bound $\Omega(n)$.

**Ví dụ — Sắp xếp (comparison):**

- Adversary: luôn trả lời sao cho mọi hoán vị vẫn có thể.
- Mỗi phép so sánh chia không gian hoán vị thành hai nửa.
- Cần ít nhất $\log_2(n!)$ phép so sánh để phân biệt $n!$ hoán vị.
- Vậy lower bound $\Omega(n\log n)$.

> **Lý thuyết bên cạnh:** Adversary argument là **chứng minh lower bound** mạnh nhất — nó không phụ thuộc vào thuật toán cụ thể, mà chỉ dựa trên **thông tin cần thiết** để giải bài toán. Đây là kỹ thuật cốt lõi trong lý thuyết độ phức tạp.

---

## 16. So sánh Omega với các ký hiệu khác

### 16.1. Omega vs Big O

| | $O$ | $\Omega$ |
|---|---|---|
| Ý nghĩa | Cận trên | Cận dưới |
| Bất đẳng thức | $f \le c\,g$ | $f \ge c\,g$ |
| Loại suy | $\le$ | $\ge$ |
| Ứng dụng chính | Worst case, upper bound | Lower bound, best case |
| Transpose | $f = O(g) \Leftrightarrow g = \Omega(f)$ | |

### 16.2. Omega vs Theta

| | $\Omega$ | $\Theta$ |
|---|---|---|
| Ý nghĩa | Cận dưới | Cận chặt |
| Bất đẳng thức | $f \ge c\,g$ | $c_1 g \le f \le c_2 g$ |
| Số hằng số | 1 | 2 |
| Độ mạnh | Yếu hơn | Mạnh hơn |
| Khi nào dùng | Không biết chính xác (hoặc chỉ cần cận dưới) | Biết đúng bậc |

**Ví dụ:** $n^2 + n = \Omega(n^2)$ (đúng) và $= \Theta(n^2)$ (đúng, chặt hơn).

### 16.3. Omega vs little-omega

| | $\Omega$ | $\omega$ |
|---|---|---|
| Ý nghĩa | $\ge$ (bậc) | $>$ (bậc) |
| Cho phép cùng bậc? | Có | Không |
| Giới hạn | $\liminf f/g > 0$ | $\lim f/g = \infty$ |
| Ví dụ | $n = \Omega(n)$ ✅ | $n = \omega(n)$ ❌ |
| | $n^2 = \Omega(n)$ ✅ | $n^2 = \omega(n)$ ✅ |

### 16.4. Bảng tổng hợp 5 ký hiệu

| Ký hiệu | Loại suy | Ý nghĩa | Ứng dụng chính |
|---|---|---|---|
| $O$ | $\le$ | Cận trên | Upper bound, worst case |
| $\Omega$ | $\ge$ | Cận dưới | Lower bound, best case |
| $\Theta$ | $=$ | Cận chặt | Đúng bậc |
| $o$ | $<$ | Nhỏ hơn thực sự | So sánh nghiêm ngặt |
| $\omega$ | $>$ | Lớn hơn thực sự | So sánh nghiêm ngặt |

**Sơ đồ quan hệ:**

$$
\Theta(g) = O(g) \cap \Omega(g)
$$
$$
o(g) \subset O(g), \quad \omega(g) \subset \Omega(g)
$$
$$
f = O(g) \iff g = \Omega(f), \quad f = o(g) \iff g = \omega(f)
$$

---

## 17. Bài tập tự luyện

**Bài 1:** Chứng minh $n^2 + 3n + 2 = \Omega(n^2)$ bằng định nghĩa.

*Hướng:* Chọn $c = 1$, $n_0 = 1$: $n^2 + 3n + 2 \ge n^2$ với mọi $n \ge 1$.

**Bài 2:** Chứng minh $5n^3 + 2n^2 + n = \Omega(n^3)$.

*Hướng:* Chọn $c = 5$, $n_0 = 1$: $5n^3 + 2n^2 + n \ge 5n^3$.

**Bài 3:** Chứng minh $n \ne \Omega(n^2)$.

*Hướng:* Phản chứng. Giả sử $n \ge c\,n^2$ với $n \ge n_0$. Chia $n$: $1 \ge c\,n$. Với $n > 1/c$, mâu thuẫn.

**Bài 4:** Chứng minh $\log(n!) = \Omega(n\log n)$.

*Hướng:* Chỉ lấy nửa sau: $\sum_{i=n/2}^n \log i \ge \frac{n}{2}\log(n/2) \ge \frac{n}{4}\log n$ với $n \ge 4$.

**Bài 5:** Xác định $\Omega$ cho:

```text
for i = 1 to n:
    for j = 1 to n:
        doing O(1)
```

*Hướng:* $\Omega(n^2)$ — cả hai vòng đều chạy đủ $n$ lần.

**Bài 6:** Giải $T(n) = 3T(n/3) + n$.

*Hướng:* Master Theorem: $a=3, b=3, x=n^{\log_3 3}=n$, $f(n)=\Theta(x)$ ⇒ Case 2 ⇒ $T(n) = \Theta(n\log n)$ ⇒ $\Omega(n\log n)$.

**Bài 7:** Chứng minh mọi thuật toán so sánh có lower bound $\Omega(n\log n)$.

*Hướng:* Cây quyết định có $n!$ lá ⇒ chiều cao $\ge \log_2(n!) = \Theta(n\log n)$.

**Bài 8:** Tại sao $\Omega(f) + \Omega(g) = \Omega(\min(f, g))$ mà không phải $\max$?

*Hướng:* Vì cận dưới phải đúng cho **mọi** đoạn. Đoạn nhỏ nhất có thể chỉ $\Omega(\min)$. Nếu dùng $\max$, sẽ sai khi đoạn nhỏ chạy.

**Bài 9:** Chứng minh BFS là $\Theta(V+E)$ (cả $O$ lẫn $\Omega$).

*Hướng:* $O$: mỗi đỉnh vào/ra queue một lần ($O(V)$), mỗi cạnh duyệt một lần ($O(E)$). $\Omega$: phải thăm mọi đỉnh ($\Omega(V)$), phải duyệt mọi cạnh liên thông ($\Omega(E)$).

**Bài 10:** Cho thuật toán có worst case $O(n^2)$ và lower bound $\Omega(n\log n)$. Có thể kết luận gì?

*Hướng:* Có khoảng trống giữa $\Omega(n\log n)$ và $O(n^2)$. Thuật toán **chưa tối ưu** — có thể còn cải thiện (thuật toán tốt hơn nằm trong khoảng này).

---

## 18. Tóm tắt một trang

### Định nghĩa

$$
f(n) = \Omega(g(n)) \iff \exists c > 0, n_0 > 0 : \forall n \ge n_0,\ 0 \le c\,g(n) \le f(n)
$$

### Ý nghĩa

$f$ **bị chặn dưới** bởi $g$ (sai khác hằng số) khi $n$ đủ lớn. Tức $f$ **tăng ít nhất nhanh bằng** $g$.

### Vai trò chính

- **Lower bound** của bài toán (giới hạn dưới — không thuật toán nào vượt qua).
- **Best case** của thuật toán.
- **Chứng minh tối ưu:** khi upper bound $O$ gặp lower bound $\Omega$.

### Cách chứng minh

1. **Định nghĩa:** tìm $c$, $n_0$ bằng "nới lỏng xuống" (thay số hạng khó bằng số nhỏ hơn).
2. **Giới hạn:** $\lim f/g > 0$ hoặc $= \infty$.
3. **Phản chứng:** giả sử sai → mâu thuẫn.

### Quy tắc từ code

| Cấu trúc | Omega |
|---|---|
| Tuần tự | $\min$ (khác $O$!) |
| Lồng nhau | tích |
| Điều kiện | $\min$ các nhánh |
| Bỏ hằng số | $c \cdot \Omega(f) = \Omega(f)$ |
| Bỏ bậc thấp | $\Omega(n^2 + n) = \Omega(n^2)$ |
| Log: bỏ cơ số | $\Omega(\log_2 n) = \Omega(\log n)$ |

### Tính chất

- Phản xạ: $f = \Omega(f)$.
- Bắc cầu: $f = \Omega(g), g = \Omega(h) \Rightarrow f = \Omega(h)$.
- **Transpose:** $f = O(g) \Leftrightarrow g = \Omega(f)$.
- Giao: $\Theta = O \cap \Omega$.

### Lower bound nổi tiếng

- Comparison sort: $\Omega(n\log n)$.
- Tìm max: $\Omega(n)$.
- Tìm kiếm mảng đã sắp: $\Omega(\log n)$.
- Nhân ma trận: $\Omega(n^2)$ (lower bound hiện biết).

### Lưu ý

- $\Omega$ **không phải** best case — nó là **cận dưới** (quan hệ giữa hàm).
- $\Omega(f) + \Omega(g) = \Omega(\min(f, g))$ — **khác** quy tắc của $O$.
- Lower bound có thể **không đạt được** (nhân ma trận).
- Khi lower bound gặp upper bound ⇒ thuật toán **tối ưu**.

---

## 19. Liên kết với Big O và Theta

### 19.1. Bộ ba hoàn chỉnh

| | $O$ | $\Omega$ | $\Theta$ |
|---|---|---|---|
| Loại suy | $\le$ | $\ge$ | $=$ |
| Số hằng số | 1 | 1 | 2 |
| Ý nghĩa | Cận trên | Cận dưới | Cận chặt |
| Khi nào dùng | Upper bound | Lower bound | Đúng bậc |
| Ví dụ | $n^2 = O(n^2)$ | $n^2 = \Omega(n)$ | $n^2 = \Theta(n^2)$ |

### 19.2. Quan hệ then chốt

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

$$
f = \Theta(g) \iff f = O(g) \text{ và } f = \Omega(g)
$$

### 19.3. Quy trình phân tích đầy đủ một thuật toán

1. **Xác định phép toán cơ bản.**
2. **Tính $T(n)$ chính xác hoặc chặn.**
3. **Tìm $O$** (cận trên) — thường dễ, chỉ cần chặn trên.
4. **Tìm $\Omega$** (cận dưới) — khó hơn, cần chứng minh tối thiểu.
5. **Kết luận $\Theta$** nếu $O$ và $\Omega$ cùng bậc.
6. **Xét best/average/worst** nếu cần.

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Chứng minh đầy đủ** lower bound $\Omega(n\log n)$ của comparison sort bằng cây quyết định.
- **Adversary argument** chi tiết cho các bài toán (tìm max, tìm max+min, sắp xếp).
- **Phân tích lower bound của các bài toán đồ thị** (liên thông, shortest path, MST).
- **So sánh** lower bound và upper bound cho các bài toán mở (nhân ma trận, so khớp chuỗi, SAT