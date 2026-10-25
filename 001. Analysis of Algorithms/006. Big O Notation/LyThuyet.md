# Big O Notation — Lý thuyết chi tiết + Ví dụ

## 1. Big O là gì?

**Big O notation** (ký hiệu $O$ lớn) là công cụ mô tả **cận trên tiệm cận** của một hàm — nói cách khác, nó trả lời câu hỏi:

> "Hàm $f(n)$ **tăng không nhanh hơn** hàm $g(n)$ (sai khác một hằng số) khi $n \to \infty$ không?"

Big O là ký hiệu **phổ biến nhất** trong 5 ký hiệu tiệm cận ($O, \Omega, \Theta, o, \omega$), vì:

- Nó cho **bảo đảm** — thuật toán "không bao giờ chậm hơn" một ngưỡng.
- Nó **không cần giả định** về phân bố input (khác average case).
- Trong thực tế, người ta dùng $O$ lỏng lẻo để nói "order of growth" — dù chính xác hơn phải là $\Theta$.

### Trực giác

$f(n) = O(g(n))$ nghĩa là: **"$f$ bị chặn trên bởi $g$ (sai khác hằng số) khi $n$ đủ lớn"**.

$$
f(n) = O(g(n)) \quad \Longleftrightarrow \quad f \text{ tăng chậm hơn hoặc bằng } g \text{ về bậc}
$$

**Loại suy số thực:** $O$ giống như dấu $\le$ trong so sánh số.

> **Lý thuyết bên cạnh:** Big O **không** nói "chính xác bằng" — nó chỉ nói "không vượt quá". Vì vậy $n = O(n^2)$ là **đúng** (dù lỏng), còn $n = O(n)$ là **chặt** hơn. Trong thực hành, ta luôn cố gắng tìm Big O **chặt nhất** có thể chứng minh.

---

## 2. Định nghĩa hình thức

Cho $f(n)$ và $g(n)$ là các hàm xác định trên số nguyên dương, **không âm** với $n$ đủ lớn.

$$
\boxed{f(n) = O(g(n)) \iff \exists\, c > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le f(n) \le c\,g(n)}
$$

**Đọc là:** "$f(n)$ là big-O của $g(n)$" hoặc "$f(n)$ thuộc $O(g(n))$".

### Phân tích các thành phần

| Thành phần               | Ý nghĩa                            |
| ------------------------ | ---------------------------------- |
| $\exists c > 0$          | Tồn tại **một** hằng số nhân dương |
| $\exists n_0 > 0$        | Tồn tại **một** ngưỡng kích thước  |
| $\forall n \ge n_0$      | Với **mọi** $n$ từ ngưỡng trở lên  |
| $0 \le f(n) \le c\,g(n)$ | $f$ bị chặn trên bởi $c\,g$        |

### Điểm mấu chốt cần hiểu

1. **$c$ và $n_0$ không phụ thuộc $n$** — chúng là hằng số **cố định** sau khi chọn.
2. **$n_0$ cho phép bỏ qua hành vi với $n$ nhỏ** — nơi hằng số và số hạng bậc thấp có thể áp đảo.
3. **$c$ cho phép bỏ qua hằng số nhân** — $3n^2$ và $100n^2$ cùng $O(n^2)$.
4. **Chỉ cần tồn tại** một bộ $(c, n_0)$ — không cần tối ưu.

### Ký hiệu tập hợp (chính xác hơn)

$O(g(n))$ là một **tập hợp các hàm**:

$$
O(g(n)) = \{f(n) : \exists c > 0, n_0 > 0, \forall n \ge n_0,\ 0 \le f(n) \le c\,g(n)\}
$$

Vì vậy chính xác phải viết $f(n) \in O(g(n))$, không phải $f(n) = O(g(n))$.

**Hệ quả:** Không thể "trừ hai vế" hay "chuyển vế" như đẳng thức thực sự.

> **Lý thuyết bên cạnh:** Dấu $=$ trong $f(n) = O(g(n))$ là **lạm dụng ký hiệu** (abuse of notation) — nhưng là quy ước phổ quát trong khoa học máy tính. Khi đọc, hãy hiểu là "$\in$". Khi viết, cứ dùng $=$ cho tiện.

### Phủ định (để chứng minh $f \ne O(g)$)

$$
f(n) \ne O(g(n)) \iff \forall c > 0,\ \forall n_0 > 0,\ \exists n \ge n_0 : f(n) > c\,g(n)
$$

**Ý nghĩa:** Dù chọn $c$ lớn đến đâu, dù chọn $n_0$ lớn đến đâu, luôn tìm được $n$ đủ lớn để $f$ vượt $c\,g$.

---

## 3. Cách đọc và viết Big O

### 3.1. Các cách viết tương đương

Tất cả các cách sau đều đúng và tương đương:

$$
f(n) = O(g(n)), \quad f(n) \in O(g(n)), \quad f \text{ là } O(g), \quad f \preceq g
$$

### 3.2. Cách đọc trong thực tế

| Viết                | Đọc                        |
| ------------------- | -------------------------- |
| $T(n) = O(n^2)$     | "T là ô của n bình phương" |
| $T(n) = O(n\log n)$ | "T là ô của n log n"       |
| $T(n) = O(\log n)$  | "T là ô của log n"         |
| $T(n) = O(1)$       | "T là ô của một" (hằng số) |

### 3.3. Big O **không** phải là hàm chính xác

**Quan trọng:** $T(n) = O(n^2)$ **không** có nghĩa $T(n) = n^2$. Nó chỉ nói $T$ **bị chặn trên** bởi $n^2$ (sai khác hằng số).

Ví dụ, tất cả các hàm sau đều là $O(n^2)$:

$$
n^2, \quad 3n^2, \quad n^2 + 100n, \quad 5n^{1.99}, \quad 1000n, \quad n\log n, \quad \log n, \quad 1
$$

Nhưng chỉ $n^2, 3n^2, n^2 + 100n$ là **chặt** (tức $\Theta(n^2)$).

### 3.4. Big O chặt vs lỏng

| Tuyên bố             | Đúng? | Chặt?    |
| -------------------- | ----- | -------- |
| $3n^2 + 5n = O(n^2)$ | ✅     | ✅ (chặt) |
| $3n^2 + 5n = O(n^3)$ | ✅     | ❌ (lỏng) |
| $3n^2 + 5n = O(n)$   | ❌     | —        |

**Nguyên tắc thực hành:** Luôn cố gắng tìm Big O **chặt nhất** có thể chứng minh được. Nhưng khi không chắc, Big O lỏng vẫn hữu ích (vẫn là bảo đảm).

> **Lý thuyết bên cạnh:** Trong bài báo khoa học, người ta thường viết "thuật toán có độ phức tạp $O(n^2)$" với ngụ ý **chặt** — dù về lý thuyết phải là $\Theta(n^2)$. Đây là quy ước thực tế. Khi đọc, cần phân biệt ngữ cảnh: "worst case $O(f)$" thường là cận trên; "thuật toán là $\Theta(f)$" là đúng bậc.

---

## 4. Tại sao Big O phổ biến nhất trong 5 ký hiệu?

### 4.1. Lý do thực tiễn

1. **Bảo đảm:** Khi nói "thuật toán $O(n^2)$", ta cam kết nó **không bao giờ** chậm hơn $n^2$ (sai khác hằng số). Đây là điều quan trọng nhất khi chọn thuật toán.

2. **Không cần giả định:** Big O áp dụng cho **mọi input** — không cần giả định phân bố xác suất như average case.

3. **Đơn giản:** Chỉ cần chứng minh **một** bất đẳng thức (cận trên). Với $\Theta$, phải chứng minh **hai** (cả trên lẫn dưới).

4. **An toàn:** Nếu không biết chính xác, Big O vẫn đúng (dù có thể lỏng).

### 4.2. Khi nào Big O **không đủ**?

- **Cần cận dưới:** Để chứng minh thuật toán **tối ưu** (ví dụ: comparison sort cần $\Omega(n\log n)$) — phải dùng $\Omega$.
- **Cần đúng bậc:** Để so sánh hai thuật toán cùng upper bound — phải dùng $\Theta$.
- **Cần phân biệt hằng số:** Khi $n$ nhỏ — Big O bỏ qua hằng số.

### 4.3. Bảng so sánh 5 ký hiệu (nhắc lại)

| Ký hiệu  | Ý nghĩa       | Tần suất dùng         |
| -------- | ------------- | --------------------- |
| $O$      | Cận trên      | ⭐⭐⭐⭐⭐ (phổ biến nhất) |
| $\Theta$ | Cận chặt      | ⭐⭐⭐⭐                  |
| $\Omega$ | Cận dưới      | ⭐⭐⭐                   |
| $o$      | Cận trên chặt | ⭐⭐                    |
| $\omega$ | Cận dưới chặt | ⭐                     |

> **Lý thuyết bên cạnh:** Big O phổ biến vì nó **đủ dùng** trong hầu hết tình huống thiết kế thuật toán. Khi cần phân tích chính xác hơn (chứng minh tối ưu, so sánh thuật toán cùng bậc), mới cần $\Theta$ và $\Omega$.

---

## 5. Các lớp Big O thường gặp

### 5.1. Bảng chi tiết

| Big O         | Tên gọi        | Ý nghĩa             | Ví dụ code            |
| ------------- | -------------- | ------------------- | --------------------- |
| $O(1)$        | Hằng số        | Không phụ thuộc $n$ | `A[i]`                |
| $O(\log n)$   | Logarit        | Chia đôi mỗi bước   | Binary search         |
| $O(\sqrt{n})$ | Căn            | Thử đến $\sqrt{n}$  | Kiểm tra số nguyên tố |
| $O(n)$        | Tuyến tính     | Duyệt một lần       | Linear search         |
| $O(n\log n)$  | Tuyến tính-log | Chia để trị         | Merge sort            |
| $O(n^2)$      | Bậc hai        | Hai vòng lồng       | Bubble sort           |
| $O(n^3)$      | Bậc ba         | Ba vòng lồng        | Nhân ma trận naive    |
| $O(n^k)$      | Đa thức        | $k$ vòng lồng       | —                     |
| $O(2^n)$      | Hàm mũ         | Hai nhánh đệ quy    | Fibonacci naive       |
| $O(n!)$       | Giai thừa      | Hoán vị             | TSP brute force       |
| $O(n^n)$      | Siêu giai thừa | —                   | —                     |

### 5.2. Ví dụ code cho từng lớp

**$O(1)$ — hằng số:**
```text
x = A[i]        // truy cập mảng
x = A[i] + A[j] // hai truy cập
```
Không phụ thuộc $n$ — luôn một số phép toán cố định.

**$O(\log n)$ — logarit:**
```text
i = 1
while i < n:
    i = i * 2
```
$i$ nhận $1, 2, 4, \ldots, 2^k < n$. Số lần lặp: $\lfloor \log_2 n \rfloor + 1 = O(\log n)$.

**$O(\sqrt{n})$ — căn:**
```text
for i = 1 to sqrt(n):
    if n % i == 0: return "not prime"
return "prime"
```
Vòng lặp chạy $\lfloor\sqrt{n}\rfloor$ lần.

**$O(n)$ — tuyến tính:**
```text
for i = 1 to n:
    doing O(1) work
```
$n$ lần lặp.

**$O(n\log n)$ — tuyến tính-log:**
```text
for i = 1 to n:
    j = 1
    while j < n:
        doing O(1) work
        j = j * 2
```
Vòng ngoài $n$, vòng trong $\log n$ ⇒ $O(n\log n)$.

**$O(n^2)$ — bậc hai:**
```text
for i = 1 to n:
    for j = 1 to n:
        doing O(1) work
```
$n \cdot n = n^2$.

**$O(n^3)$ — bậc ba:**
```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n:
            doing O(1) work
```
$n^3$.

**$O(2^n)$ — hàm mũ:**
```text
Fib(n):
    if n <= 1: return n
    return Fib(n-1) + Fib(n-2)
```
Cây đệ quy có $\approx 2^n$ nút (chính xác hơn: $\Theta(\varphi^n)$ với $\varphi \approx 1.618$).

**$O(n!)$ — giai thừa:**
```text
Permute(A, k, n):
    if k == n: print A
    else:
        for i = k to n:
            swap(A[k], A[i])
            Permute(A, k+1, n)
            swap(A[k], A[i])
```
Số hoán vị: $n!$.

### 5.3. Trực quan về tốc độ

Giả sử máy chạy $10^9$ phép toán/giây:

| Big O        | $n = 10$         | $n = 100$  | $n = 1000$      | $n = 10^6$          |
| ------------ | ---------------- | ---------- | --------------- | ------------------- |
| $O(\log n)$  | ~3               | ~7         | ~10             | ~20                 |
| $O(n)$       | 10               | 100        | $10^3$          | $10^6$              |
| $O(n\log n)$ | ~33              | ~664       | ~10⁴            | $\sim 2 \cdot 10^7$ |
| $O(n^2)$     | 100              | $10^4$     | $10^6$          | $10^{12}$           |
| $O(n^3)$     | $10^3$           | $10^6$     | $10^9$          | $10^{18}$           |
| $O(2^n)$     | $10^3$           | $10^{30}$  | **không tưởng** | —                   |
| $O(n!)$      | $3.6 \cdot 10^6$ | $10^{158}$ | —               | —                   |

**Kết luận:** Sự khác biệt giữa $O(n^2)$ và $O(2^n)$ không phải "vài lần" — mà là **hàng tỷ tỷ lần**. Big O quyết định **tính khả thi**.

---

## 6. Quy tắc xác định Big O từ code

### 6.1. Quy tắc cơ bản

**Quy tắc 1 — Tuần tự (sequential):**

$$
O(f) + O(g) = O(\max(f, g))
$$

Đoạn code nối tiếp: lấy Big O **lớn hơn**.

```text
for i = 1 to n: doing O(1)     // O(n)
for i = 1 to n:                // O(n^2)
    for j = 1 to n: doing O(1)
```
Tổng: $O(n) + O(n^2) = O(n^2)$.

**Quy tắc 2 — Lồng nhau (nested):**

$$
O(f) \cdot O(g) = O(f \cdot g)
$$

Vòng lồng nhau: **nhân** Big O.

```text
for i = 1 to n:         // O(n)
    for j = 1 to n:     // O(n)
        doing O(1)
```
Tổng: $O(n) \cdot O(n) = O(n^2)$.

**Quy tắc 3 — Điều kiện (conditional):**

$$
\max(\text{nhánh if}, \text{nhánh else})
$$

Lấy Big O **lớn hơn** của các nhánh.

```text
if condition:           // O(n)
    for i = 1 to n: ...
else:                   // O(n^2)
    for i = 1 to n:
        for j = 1 to n: ...
```
Tổng: $O(n^2)$ (worst case).

**Quy tắc 4 — Bỏ hằng số:**

$$
c \cdot O(f) = O(f)
$$

$O(3n) = O(n)$, $O(n/2) = O(n)$.

**Quy tắc 5 — Bỏ số hạng bậc thấp:**

$$
O(n^2 + n) = O(n^2)
$$

**Quy tắc 6 — Log: bỏ cơ số:**

$$
O(\log_2 n) = O(\log_{10} n) = O(\ln n) = O(\log n)
$$

### 6.2. Ví dụ tổng hợp

```text
Function F(n):
    sum = 0                          // O(1)
    for i = 1 to n:                  // O(n)
        sum += A[i]
    for i = 1 to n:                  // O(n^2)
        for j = 1 to n:
            sum += A[i] * A[j]
    if sum > 0:                      // O(n)
        for i = 1 to n:
            sum -= 1
    return sum
```

**Phân tích:**

- Đoạn 1: $O(1)$
- Đoạn 2: $O(n)$
- Đoạn 3: $O(n^2)$
- Đoạn 4: $O(n)$
- Đoạn 5: $O(1)$

**Tổng:** $O(1) + O(n) + O(n^2) + O(n) + O(1) = O(n^2)$.

### 6.3. Bẫy: vòng lặp có số lần lặp phụ thuộc $n$

```text
for i = 1 to n:
    for j = 1 to i:
        doing O(1) work
```

**Số lần thực hiện:**

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2} = O(n^2)
$$

**Quan trọng:** Vòng trong chạy $i$ lần (không phải $n$ lần), nhưng tổng vẫn là $O(n^2)$.

```text
for i = 1 to n:
    j = 1
    while j < n:
        doing O(1)
        j = j * 3
```

Vòng trong chạy $\log_3 n$ lần. Tổng: $O(n \log n)$.

### 6.4. Bẫy: vòng lặp có bước nhảy thay đổi

```text
i = 1
while i < n:
    doing O(1)
    i = i * i    // bình phương mỗi bước
```

$i$ nhận $1, 2, 4, 16, 256, \ldots$. Số bước: $\log\log n$ (vì sau $k$ bước, $i = 2^{2^{k-1}}$). Vậy $O(\log\log n)$.

---

## 7. Chứng minh Big O bằng định nghĩa

### 7.1. Phương pháp "nới lỏng" (loosening)

**Ý tưởng:** Thay các số hạng khó bằng phiên bản **lớn hơn** (với $O$) sao cho dễ xử lý, rồi tìm hằng số.

**Mẹo:** Không cần $c$ tối ưu — chỉ cần **tồn tại** một $c$ hợp lệ.

### 7.2. Ví dụ 1 — $3n^2 + 5n + 7 = O(n^2)$

**Cần tìm:** $c > 0, n_0$ sao cho $3n^2 + 5n + 7 \le c\,n^2$ với $n \ge n_0$.

**Nới lỏng** (với $n \ge 1$):

$$
3n^2 + 5n + 7 \le 3n^2 + 5n^2 + 7n^2 = 15n^2
$$

(Vì $5n \le 5n^2$ và $7 \le 7n^2$ khi $n \ge 1$.)

**Chọn:** $c = 15$, $n_0 = 1$. $\blacksquare$

**Kiểm tra:** $n=1$: $3+5+7 = 15 \le 15 \cdot 1$. Đúng. $n=2$: $12+10+7 = 29 \le 15 \cdot 4 = 60$. Đúng. ✓

### 7.3. Ví dụ 2 — $100n + 5 = O(n)$

**Cần tìm:** $c > 0, n_0$ sao cho $100n + 5 \le c\,n$.

**Phân tích:** Hằng số 5 là "bẫy" — cần $n_0$ đủ lớn để $5 \le n$.

**Nới lỏng** (với $n \ge 5$): $5 \le n$, nên:

$$
100n + 5 \le 100n + n = 101n
$$

**Chọn:** $c = 101$, $n_0 = 5$. $\blacksquare$

**Kiểm tra:** $n=5$: $500+5 = 505 \le 101 \cdot 5 = 505$. Đúng (bằng). ✓

### 7.4. Ví dụ 3 — $n^2 + n\log n + n = O(n^2)$

**Cần tìm:** $c, n_0$ sao cho $n^2 + n\log n + n \le c\,n^2$ với $n \ge n_0$.

**Nới lỏng** (với $n \ge 2$):

- $\log n \le n$ ⇒ $n\log n \le n^2$
- $n \le n^2$

Vậy:

$$
n^2 + n\log n + n \le n^2 + n^2 + n^2 = 3n^2
$$

**Chọn:** $c = 3$, $n_0 = 2$. $\blacksquare$

### 7.5. Ví dụ 4 — $6n^3 \ne O(n^2)$ (phản chứng)

**Giả sử** $6n^3 = O(n^2)$. Khi đó tồn tại $c > 0, n_0$ sao cho:

$$
6n^3 \le c\,n^2, \quad \forall n \ge n_0
$$

Chia hai vế cho $n^2 > 0$:

$$
6n \le c, \quad \forall n \ge n_0
$$

Nhưng với $n = \max(n_0, \lceil c/6 \rceil + 1)$, ta có $6n > c$ — **mâu thuẫn**. Vậy $6n^3 \ne O(n^2)$. $\blacksquare$

### 7.6. Ví dụ 5 — $n = O(2^n)$

**Cần tìm:** $c, n_0$ sao cho $n \le c \cdot 2^n$ với $n \ge n_0$.

**Nhận xét:** Với mọi $n \ge 1$, $2^n \ge n$ (chứng minh bằng quy nạp, hoặc biết rằng hàm mũ tăng nhanh hơn tuyến tính).

**Chọn:** $c = 1$, $n_0 = 1$. $\blacksquare$

### 7.7. Ví dụ 6 — $\log(n!) = O(n\log n)$

**Cần tìm:** $c, n_0$ sao cho $\log(n!) \le c \cdot n\log n$.

$$
\log(n!) = \sum_{i=1}^{n} \log i \le \sum_{i=1}^{n} \log n = n\log n
$$

**Chọn:** $c = 1$, $n_0 = 1$. $\blacksquare$

> **Lý thuyết bên cạnh:** Kỹ thuật "chặn từng số hạng bằng max" rất hữu ích. Ở đây, thay mỗi $\log i$ bằng $\log n$ (là max của dãy). Đây là cách chuẩn để chặn trên tổng.

### 7.8. Ví dụ 7 — $n^k = O(c^n)$ với mọi $k > 0, c > 1$

**Chứng minh (ý tưởng):** Dùng quy nạp hoặc L'Hôpital. Với $c = 2$:

$$
\lim_{n\to\infty} \frac{n^k}{2^n} = 0 \quad \text{(áp dụng L'Hôpital } k \text{ lần)}
$$

Vậy $n^k = O(2^n)$. $\blacksquare$

**Ý nghĩa:** Đa thức bậc bất kỳ luôn bị chặn trên bởi hàm mũ cơ số bất kỳ $> 1$.

---

## 8. Chứng minh Big O bằng giới hạn

### 8.1. Công thức

Khi $g(n) > 0$ với $n$ đủ lớn, xét:

$$
L = \lim_{n \to \infty} \frac{f(n)}{g(n)}
$$

| $L$              | Kết luận                                  |
| ---------------- | ----------------------------------------- |
| $L = 0$          | $f = O(g)$ (thực ra $f = o(g)$, mạnh hơn) |
| $0 < L < \infty$ | $f = \Theta(g)$ ⇒ $f = O(g)$              |
| $L = \infty$     | $f \ne O(g)$ (thực ra $f = \omega(g)$)    |
| không tồn tại    | Cần xét $\limsup$                         |

**Lưu ý:** Phương pháp giới hạn **mạnh hơn** định nghĩa — cho kết quả chặt hơn (phân biệt $\Theta$ vs $o$).

### 8.2. Ví dụ 8 — $5n^2 + 3n = O(n^2)$

$$
L = \lim_{n\to\infty} \frac{5n^2 + 3n}{n^2} = 5
$$

$0 < 5 < \infty$ ⇒ $5n^2 + 3n = \Theta(n^2)$ ⇒ $= O(n^2)$. $\blacksquare$

### 8.3. Ví dụ 9 — $n^3 = O(2^n)$

$$
L = \lim_{n\to\infty} \frac{n^3}{2^n} = 0 \quad \text{(L'Hôpital 3 lần)}
$$

$L = 0$ ⇒ $n^3 = o(2^n)$ ⇒ $= O(2^n)$. $\blacksquare$

### 8.4. Ví dụ 10 — $n^2 \ne O(n)$

$$
L = \lim_{n\to\infty} \frac{n^2}{n} = \infty
$$

$L = \infty$ ⇒ $n^2 \ne O(n)$. $\blacksquare$

### 8.5. Ví dụ 11 — $\log_2 n = O(\ln n)$

$$
L = \lim_{n\to\infty} \frac{\log_2 n}{\ln n} = \frac{1}{\ln 2} \approx 1.44
$$

$0 < L < \infty$ ⇒ $\log_2 n = \Theta(\ln n)$ ⇒ $= O(\ln n)$. $\blacksquare$

---

## 9. Tính chất của Big O

### 9.1. Tính phản xạ (reflexivity)

$$
f(n) = O(f(n))
$$

**Chứng minh:** Chọn $c = 1$, $n_0 = 1$. $\blacksquare$

### 9.2. Tính bắc cầu (transitivity)

Nếu $f = O(g)$ và $g = O(h)$ thì $f = O(h)$.

**Chứng minh:** Có $c_1, n_1$ với $f \le c_1 g$ ($n \ge n_1$) và $c_2, n_2$ với $g \le c_2 h$ ($n \ge n_2$). Chọn $c = c_1 c_2$, $n_0 = \max(n_1, n_2)$:

$$
f(n) \le c_1 g(n) \le c_1 c_2 h(n) = c\,h(n), \quad \forall n \ge n_0
$$

$\blacksquare$

### 9.3. Tính phản đối xứng (transpose symmetry)

$$
f = O(g) \iff g = \Omega(f)
$$

Đây là **cầu nối** giữa $O$ và $\Omega$.

### 9.4. Quy tắc cộng (cho số hạng tử cố định)

$$
O(f) + O(g) = O(\max(f, g))
$$

**Chứng minh:** $f \le c_1 \max(f, g)$ và $g \le c_2 \max(f, g)$ với $n$ đủ lớn. Vậy $f + g \le (c_1 + c_2)\max(f, g)$. $\blacksquare$

### 9.5. Quy tắc nhân

$$
O(f) \cdot O(g) = O(f \cdot g)
$$

**Chứng minh:** $f \cdot g \le c_1 g_1 \cdot c_2 g_2 = (c_1 c_2)(f_1 g_1)$ với $n$ đủ lớn. $\blacksquare$

### 9.6. Quy tắc hằng số

$$
c \cdot O(f) = O(f), \quad \forall c > 0
$$

### 9.7. Bảng tóm tắt tính chất

| Tính chất     | Công thức                                 |
| ------------- | ----------------------------------------- |
| Phản xạ       | $f = O(f)$                                |
| Bắc cầu       | $f = O(g), g = O(h) \Rightarrow f = O(h)$ |
| Phản đối xứng | $f = O(g) \Leftrightarrow g = \Omega(f)$  |
| Cộng          | $O(f) + O(g) = O(\max(f, g))$             |
| Nhân          | $O(f) \cdot O(g) = O(fg)$                 |
| Hằng số       | $c \cdot O(f) = O(f)$                     |

---

## 10. Big O cho đệ quy

### 10.1. Phương pháp

Với hàm đệ quy, viết **hệ thức truy hồi** rồi giải:

$$
T(n) = a\,T(n/b) + f(n)
$$

**Ba công cụ:**

1. **Phương pháp thế:** đoán nghiệm, chứng minh bằng quy nạp.
2. **Cây đệ quy:** vẽ cây, cộng chi phí từng tầng.
3. **Master Theorem:** công thức đóng cho trường hợp chia đều.

### 10.2. Master Theorem (nhắc nhanh)

Với $T(n) = aT(n/b) + f(n)$, đặt $x = n^{\log_b a}$:

1. $f(n) = O(x^{1-\varepsilon})$ ⇒ $T(n) = \Theta(x)$.
2. $f(n) = \Theta(x)$ ⇒ $T(n) = \Theta(x \log n)$.
3. $f(n) = \Omega(x^{1+\varepsilon})$ (và điều kiện chính quy) ⇒ $T(n) = \Theta(f(n))$.

### 10.3. Ví dụ 12 — Binary Search

$$
T(n) = T(n/2) + O(1)
$$

$a = 1$, $b = 2$, $f(n) = 1$, $x = n^{\log_2 1} = 1$. $f(n) = \Theta(x)$ ⇒ Case 2:

$$
T(n) = O(\log n)
$$

### 10.4. Ví dụ 13 — Merge Sort

$$
T(n) = 2T(n/2) + O(n)
$$

$a = 2$, $b = 2$, $f(n) = n$, $x = n^{\log_2 2} = n$. $f(n) = \Theta(x)$ ⇒ Case 2:

$$
T(n) = O(n\log n)
$$

### 10.5. Ví dụ 14 — Fibonacci naive

$$
T(n) = T(n-1) + T(n-2) + O(1)
$$

**Không áp dụng Master Theorem** (vì trừ, không chia).

**Cây đệ quy:** Số nút $\approx F_n = \Theta(\varphi^n)$ với $\varphi \approx 1.618$.

$$
T(n) = O(2^n) \quad \text{(cận trên lỏng)}
$$

Cận chặt hơn: $\Theta(\varphi^n)$. Nhưng trong thực tế, viết $O(2^n)$ là đủ.

### 10.6. Ví dụ 15 — Tìm max đệ quy

```text
FindMax(A, low, high):
    if low == high: return A[low]
    mid = (low + high) / 2
    left_max = FindMax(A, low, mid)
    right_max = FindMax(A, mid+1, high)
    return max(left_max, right_max)
```

$$
T(n) = 2T(n/2) + O(1)
$$

$a = 2$, $b = 2$, $f(n) = 1$, $x = n^{\log_2 2} = n$. $f(n) = O(n^{1-\varepsilon})$ ⇒ Case 1:

$$
T(n) = O(n)
$$

**Nhận xét:** Đệ quy **không** làm thuật toán nhanh hơn ở đây — vẫn $O(n)$, giống vòng lặp. Đây là ví dụ cho thấy đệ quy không tự động tốt hơn.

> **Lý thuyết bên cạnh:** Đệ quy **không** luôn tốt hơn vòng lặp về Big O. Nó chỉ hữu ích khi bài toán có **cấu trúc chia để trị** thực sự (như Merge Sort, Binary Search). Với bài toán tuyến tính (như tìm max), đệ quy chỉ tốn thêm bộ nhớ stack — không cải thiện Big O.

---

## 11. Big O cho cấu trúc dữ liệu

### 11.1. Bảng tra nhanh

| CTDL                   | Truy cập         | Tìm         | Chèn        | Xóa         |
| ---------------------- | ---------------- | ----------- | ----------- | ----------- |
| **Array**              | $O(1)$           | $O(n)$      | $O(n)$      | $O(n)$      |
| **Linked List**        | $O(n)$           | $O(n)$      | $O(1)$*     | $O(1)$*     |
| **Stack**              | $O(n)$           | $O(n)$      | $O(1)$      | $O(1)$      |
| **Queue**              | $O(n)$           | $O(n)$      | $O(1)$      | $O(1)$      |
| **BST (avg)**          | $O(\log n)$      | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ |
| **BST (worst)**        | $O(n)$           | $O(n)$      | $O(n)$      | $O(n)$      |
| **AVL / Red-Black**    | $O(\log n)$      | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ |
| **Heap**               | $O(1)$ (min/max) | $O(n)$      | $O(\log n)$ | $O(\log n)$ |
| **Hash Table (avg)**   | —                | $O(1)$      | $O(1)$      | $O(1)$      |
| **Hash Table (worst)** | —                | $O(n)$      | $O(n)$      | $O(n)$      |
| **Trie**               | —                | $O(L)$**    | $O(L)$**    | $O(L)$**    |

*Với linked list, chèn/xóa $O(1)$ **nếu đã có con trỏ** đến vị trí. Nếu phải tìm vị trí trước, là $O(n)$.

**$L$ = độ dài chuỗi (không phải số phần tử).

### 11.2. Nhận xét quan trọng

- **Array** truy cập $O(1)$ nhưng chèn/xóa $O(n)$ — trade-off.
- **Linked List** ngược lại: truy cập $O(n)$ nhưng chèn/xóa $O(1)$.
- **BST** trung bình $O(\log n)$ nhưng **worst $O(n)$** — cần cây cân bằng để đảm bảo.
- **Hash Table** trung bình $O(1)$ nhưng worst $O(n)$ — cần xử lý collision tốt.
- **Heap** tìm min/max $O(1)$ nhưng tìm phần tử bất kỳ $O(n)$.

> **Lý thuyết bên cạnh:** Bảng trên là **average case** trừ khi ghi rõ worst case. Khi thiết kế hệ thống, cần biết **cả hai** để tránh bất ngờ với adversarial input (ví dụ HashDoS tấn công hash table).

---

## 12. Big O cho thuật toán phổ biến

### 12.1. Sắp xếp

| Thuật toán     | Best         | Average      | Worst        | Bộ nhớ      |
| -------------- | ------------ | ------------ | ------------ | ----------- |
| Bubble Sort    | $O(n)$*      | $O(n^2)$     | $O(n^2)$     | $O(1)$      |
| Insertion Sort | $O(n)$       | $O(n^2)$     | $O(n^2)$     | $O(1)$      |
| Selection Sort | $O(n^2)$     | $O(n^2)$     | $O(n^2)$     | $O(1)$      |
| Merge Sort     | $O(n\log n)$ | $O(n\log n)$ | $O(n\log n)$ | $O(n)$      |
| Quick Sort     | $O(n\log n)$ | $O(n\log n)$ | $O(n^2)$     | $O(\log n)$ |
| Heap Sort      | $O(n\log n)$ | $O(n\log n)$ | $O(n\log n)$ | $O(1)$      |
| Counting Sort  | $O(n+k)$     | $O(n+k)$     | $O(n+k)$     | $O(k)$      |
| Radix Sort     | $O(d(n+k))$  | $O(d(n+k))$  | $O(d(n+k))$  | $O(n+k)$    |

*Bubble Sort có best $O(n)$ nếu có optimization "dừng sớm khi không swap".

### 12.2. Tìm kiếm

| Thuật toán    | Best   | Average     | Worst       |
| ------------- | ------ | ----------- | ----------- |
| Linear Search | $O(1)$ | $O(n)$      | $O(n)$      |
| Binary Search | $O(1)$ | $O(\log n)$ | $O(\log n)$ |
| Hash Search   | $O(1)$ | $O(1)$      | $O(n)$      |
| BST Search    | $O(1)$ | $O(\log n)$ | $O(n)$      |

### 12.3. Đồ thị

| Thuật toán       | Big O            | Ghi chú           |
| ---------------- | ---------------- | ----------------- |
| BFS              | $O(V+E)$         | Dùng queue        |
| DFS              | $O(V+E)$         | Dùng stack/đệ quy |
| Dijkstra (heap)  | $O((V+E)\log V)$ | Không âm          |
| Bellman-Ford     | $O(VE)$          | Cho phép cạnh âm  |
| Floyd-Warshall   | $O(V^3)$         | All-pairs         |
| Kruskal          | $O(E\log E)$     | Sắp xếp cạnh      |
| Prim (heap)      | $O((V+E)\log V)$ | Giống Dijkstra    |
| Topological Sort | $O(V+E)$         | DAG               |

### 12.4. Quy hoạch động

| Bài toán                         | Big O        | Bộ nhớ          |
| -------------------------------- | ------------ | --------------- |
| Fibonacci                        | $O(n)$       | $O(1)$ (tối ưu) |
| LCS (Longest Common Subsequence) | $O(nm)$      | $O(nm)$         |
| Knapsack 0/1                     | $O(nW)$      | $O(nW)$         |
| Matrix Chain                     | $O(n^3)$     | $O(n^2)$        |
| Edit Distance                    | $O(nm)$      | $O(nm)$         |
| TSP (Held-Karp)                  | $O(n^2 2^n)$ | $O(n 2^n)$      |

---

## 13. Big O với nhiều biến

### 13.1. Khi nào có nhiều biến?

Khi kích thước đầu vào có **nhiều chiều**:

- **Đồ thị:** $V$ đỉnh, $E$ cạnh ⇒ $O(V + E)$.
- **Ma trận:** $n \times m$ ⇒ $O(nm)$.
- **Chuỗi:** độ dài $n$ và $m$ ⇒ $O(nm)$ (edit distance).
- **Knapsack:** $n$ vật, capacity $W$ ⇒ $O(nW)$.

### 13.2. Ví dụ — duyệt ma trận $n \times m$

```text
for i = 1 to n:
    for j = 1 to m:
        doing O(1) work
```

$$
T(n, m) = O(nm)
$$

**Không** phải $O(n^2)$ trừ khi $m = n$.

### 13.3. Ví dụ — BFS trên đồ thị

```text
BFS(G, s):
    for each vertex v: visited[v] = false    // O(V)
    queue = [s]; visited[s] = true
    while queue not empty:                   // O(V) lần
        u = queue.dequeue()
        for each neighbor v of u:            // O(deg(u)) lần
            if not visited[v]:
                visited[v] = true
                queue.enqueue(v)
```

**Phân tích:**

- Khởi tạo: $O(V)$.
- Mỗi đỉnh vào/ra queue một lần: $O(V)$.
- Tổng số lần duyệt cạnh: $\sum_{u} \deg(u) = 2E = O(E)$.

**Tổng:** $O(V + E)$.

**Không** phải $O(V \cdot E)$ — vì mỗi cạnh chỉ duyệt hằng số lần.

### 13.4. Big O biểu diễn theo biến nào?

**Nguyên tắc:** Biểu diễn theo **tất cả** biến ảnh hưởng đến kích thước input.

- Đồ thị thưa ($E = O(V)$): $O(V+E) = O(V)$.
- Đồ thị dày ($E = O(V^2)$): $O(V+E) = O(V^2)$.

**Không** được tự ý bỏ biến nếu không có giả định.

> **Lý thuyết bên cạnh:** Với đồ thị, luôn viết $O(V+E)$ thay vì $O(V^2)$ — vì $O(V^2)$ chỉ đúng cho đồ thị dày. Đây là lỗi phổ biến khi phân tích BFS/DFS.

---

## 14. Những hiểu nhầm phổ biến về Big O

### Hiểu nhầm 1 — "$O(n)$ nghĩa là đúng $n$ phép toán"

**Sai.** $O(n)$ nghĩa là **bị chặn trên** bởi $c\,n$ với $n$ đủ lớn. Hàm $100n$, $5n + 1000$, $n/2$, thậm chí $\log n$ đều là $O(n)$.

### Hiểu nhầm 2 — "Big O luôn là worst case"

**Sai về định nghĩa.** Big O là **quan hệ giữa hai hàm**, không phải case. Tuy nhiên, **quy ước thực tế**: khi nói "thuật toán $O(n^2)$" không nói case, ngầm hiểu là worst case.

**Đúng:** Có thể nói "best case $O(n)$", "average case $O(n^2)$", "worst case $O(n^2)$" — Big O áp dụng cho **mọi case**.

### Hiểu nhầm 3 — "Big O nhỏ hơn luôn nhanh hơn"

**Sai với $n$ nhỏ.** $O(n^2)$ với hằng số 0.01 có thể nhanh hơn $O(n\log n)$ với hằng số 1000 khi $n$ nhỏ.

**Ví dụ thực tế:** Insertion Sort ($O(n^2)$) nhanh hơn Merge Sort ($O(n\log n)$) với $n < 50$ trong nhiều cài đặt. Đây là lý do Timsort dùng Insertion Sort cho mảng nhỏ.

### Hiểu nhầm 4 — "Big O bỏ qua hằng số nên không quan trọng"

**Sai trong thực tế.** Big O bỏ qua hằng số **về mặt tiệm cận**, nhưng hằng số quan trọng khi $n$ vừa phải. $10^9 n$ và $n$ cùng $O(n)$ nhưng thực tế chênh $10^9$ lần.

### Hiểu nhầm 5 — "$f = O(g)$ và $g = O(h)$ suy ra $f = \Theta(h)$"

**Sai.** Ví dụ: $n = O(n^2)$ và $n^2 = O(n^3)$, nhưng $n \ne \Theta(n^3)$.

**Đúng:** Nếu $f = O(g)$ **và** $g = O(f)$ thì $f = \Theta(g)$.

### Hiểu nhầm 6 — "$O(f) + O(g) = O(f + g)$"

**Đúng nhưng chưa chặt.** Đúng hơn là $O(f) + O(g) = O(\max(f, g))$.

**Ví dụ:** $O(n) + O(n^2) = O(n^2)$, không phải $O(n + n^2)$ (dù cùng nghĩa nhưng $O(n^2)$ chặt hơn).

### Hiểu nhầm 7 — "Vòng lặp lồng nhau luôn là $O(n^2)$"

**Sai.** Phụ thuộc số lần lặp **thực tế**.

```text
for i = 1 to n:
    for j = 1 to i:
        doing O(1)
```
Là $O(n^2)$ (vì $\sum i = n^2/2$).

```text
for i = 1 to n:
    for j = 1 to 10:
        doing O(1)
```
Là $O(n)$ (vì vòng trong cố định 10 lần).

### Hiểu nhầm 8 — "Đệ quy luôn chậm hơn vòng lặp"

**Sai.** Đệ quy và vòng lặp có thể **cùng Big O**. Ví dụ: tìm max đệ quy và tìm max vòng lặp đều $O(n)$. Đệ quy chỉ tốn thêm bộ nhớ stack.

**Đúng:** Đệ quy hữu ích khi bài toán có **cấu trúc chia để trị** thực sự (Merge Sort, Binary Search). Với bài toán tuyến tính, đệ quy không cải thiện Big O.

### Hiểu nhầm 9 — "$O(2^n)$ và $O(n!)$ gần giống nhau"

**Sai hoàn toàn.** $n!$ tăng **nhanh hơn** $2^n$ rất nhiều:

$$
2^n = o(n!)
$$

Với $n = 20$: $2^{20} \approx 10^6$, còn $20! \approx 2.4 \cdot 10^{18}$ — chênh $10^{12}$ lần.

### Hiểu nhầm 10 — "Big O chỉ áp dụng cho thời gian"

**Sai.** Big O áp dụng cho **mọi tài nguyên**: thời gian, bộ nhớ, số phép so sánh, số lần truy cập đĩa, băng thông mạng, v.v.

**Ví dụ:** Merge Sort có thời gian $O(n\log n)$ nhưng bộ nhớ $O(n)$. Cả hai đều biểu diễn bằng Big O.

> **Lý thuyết bên cạnh:** Hầu hết hiểu nhầm đến từ việc **quên rằng Big O là quan hệ giữa các hàm**. Khi đọc $f = O(g)$, hãy đọc là "$f$ bị chặn trên bởi $g$ (sai khác hằng số, với $n$ đủ lớn)". Điều này giúp tránh nhiều lỗi.

---

## 15. Big O trong thực tế — khi nào hằng số quan trọng?

### 15.1. Big O là " asymptotic" — chỉ đúng khi $n$ lớn

Với $n$ nhỏ, hằng số có thể áp đảo. **Ngưỡng "đủ lớn"** phụ thuộc vào hằng số.

**Ví dụ:** So sánh hai thuật toán:

- A: $T_A(n) = 1000n$ (Big O: $O(n)$)
- B: $T_B(n) = n^2$ (Big O: $O(n^2)$)

**A thắng khi nào?**

$$
1000n < n^2 \iff n > 1000
$$

Với $n \le 1000$, **B nhanh hơn A** dù Big O của B tệ hơn.

### 15.2. Chiến lược thực tế

1. **$n$ lớn:** Dùng Big O để chọn thuật toán.
2. **$n$ nhỏ:** Đo thực nghiệm, có thể dùng thuật toán "Big O tệ hơn nhưng hằng số nhỏ".
3. **Hybrid:** Kết hợp — ví dụ Timsort dùng Insertion Sort cho mảng nhỏ, Merge Sort cho mảng lớn.

### 15.3. Ví dụ thực tế — sắp xếp trong thư viện chuẩn

- **C++ `std::sort`:** Introsort — Quick Sort + Heap Sort + Insertion Sort.
- **Python `sorted`:** Timsort — Merge Sort + Insertion Sort.
- **Java `Arrays.sort`:** Dual-pivot Quick Sort (cho primitive) + Timsort (cho object).

**Lý do:** Big O thuần túy không đủ — cần kết hợp nhiều thuật toán để tối ưu **cả** worst case **và** thực tế.

> **Lý thuyết bên cạnh:** Big O là **công cụ lý thuyết** — nó cho biết xu hướng dài hạn, không phải hiệu năng thực tế với $n$ cụ thể. Trong kỹ thuật, cần kết hợp Big O với **đo thực nghiệm** và **hiểu biết về hằng số**.

---

## 16. So sánh Big O với các ký hiệu khác

### 16.1. Big O vs Theta

|                | $O$                  | $\Theta$                |
| -------------- | -------------------- | ----------------------- |
| Ý nghĩa        | Cận trên             | Cận chặt                |
| Bất đẳng thức  | $f \le c\,g$         | $c_1 g \le f \le c_2 g$ |
| Số hằng số cần | 1 ($c$)              | 2 ($c_1, c_2$)          |
| Độ mạnh        | Yếu hơn              | Mạnh hơn                |
| Khi nào dùng   | Không biết chính xác | Biết đúng bậc           |

**Ví dụ:** $n^2 + n = O(n^2)$ (đúng) và $= \Theta(n^2)$ (đúng, chặt hơn).

### 16.2. Big O vs Omega

|               | $O$                                      | $\Omega$     |
| ------------- | ---------------------------------------- | ------------ |
| Ý nghĩa       | Cận trên                                 | Cận dưới     |
| Bất đẳng thức | $f \le c\,g$                             | $f \ge c\,g$ |
| Loại suy      | $\le$                                    | $\ge$        |
| Quan hệ       | $f = O(g) \Leftrightarrow g = \Omega(f)$ |              |

### 16.3. Big O vs little-o

|                    | $O$                    | $o$            |
| ------------------ | ---------------------- | -------------- |
| Ý nghĩa            | $\le$ (bậc)            | $<$ (bậc)      |
| Cho phép cùng bậc? | Có                     | Không          |
| Giới hạn           | $\limsup f/g < \infty$ | $\lim f/g = 0$ |
| Ví dụ              | $n = O(n)$ ✅           | $n = o(n)$ ❌   |
|                    | $n = O(n^2)$ ✅         | $n = o(n^2)$ ✅ |

### 16.4. Bảng tổng hợp

| Ký hiệu  | Loại suy | Ý nghĩa         | Khi nào dùng        |
| -------- | -------- | --------------- | ------------------- |
| $O$      | $\le$    | Cận trên        | Bảo đảm, worst case |
| $\Omega$ | $\ge$    | Cận dưới        | Lower bound         |
| $\Theta$ | $=$      | Cận chặt        | Đúng bậc            |
| $o$      | $<$      | Nhỏ hơn thực sự | So sánh nghiêm ngặt |
| $\omega$ | $>$      | Lớn hơn thực sự | So sánh nghiêm ngặt |

---

## 17. Bài tập tự luyện

**Bài 1:** Chứng minh $n^2 + 3n + 2 = O(n^2)$ bằng định nghĩa.

*Hướng:* Chọn $c = 6$, $n_0 = 1$: $n^2 + 3n + 2 \le n^2 + 3n^2 + 2n^2 = 6n^2$ với $n \ge 1$.

**Bài 2:** Chứng minh $f(n) = 5n^3 + 2n^2 + n \ne O(n^2)$.

*Hướng:* Phản chứng. Giả sử $5n^3 + 2n^2 + n \le c\,n^2$ với $n \ge n_0$. Chia $n^2$: $5n + 2 + 1/n \le c$. Với $n$ đủ lớn, vế trái $\to \infty$ — mâu thuẫn.

**Bài 3:** Xác định Big O của:

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to 10:
            doing O(1)
```

*Hướng:* $n \cdot n \cdot 10 = O(n^2)$ (hằng số 10 bị bỏ).

**Bài 4:** Giải $T(n) = 4T(n/2) + n$.

*Hướng:* Master Theorem: $a = 4$, $b = 2$, $f(n) = n$, $x = n^{\log_2 4} = n^2$. $f(n) = O(n^{2-\varepsilon})$ ⇒ Case 1 ⇒ $T(n) = O(n^2)$.

**Bài 5:** Xác định Big O của BFS trên đồ thị có $V$ đỉnh, $E$ cạnh.

*Hướng:* $O(V + E)$.

**Bài 6:** So sánh $n^{0.1}$ và $\log^{10} n$.

*Hướng:* $\log^{10} n = o(n^{0.1})$ ⇒ $n^{0.1}$ tăng nhanh hơn. Vậy $\log^{10} n = O(n^{0.1})$.

**Bài 7:** Chứng minh $2^{n+1} = O(2^n)$.

*Hướng:* $2^{n+1} = 2 \cdot 2^n \le 2 \cdot 2^n$ với $n \ge 1$ ⇒ chọn $c = 2$, $n_0 = 1$.

**Bài 8:** Tìm lỗi trong lập luận: "Vòng lặp sau là $O(n^2)$ vì có hai vòng lồng nhau":

```text
for i = 1 to n:
    for j = 1 to 10:
        doing O(1)
```

*Hướng:* Vòng trong **cố định 10 lần**, không phụ thuộc $n$. Tổng: $n \cdot 10 = O(n)$. Lập luận sai vì "hai vòng lồng" không đủ để kết luận $O(n^2)$ — phải đếm **số lần lặp thực tế**.

**Bài 9:** Xác định Big O của hàm đệ quy:

```text
F(n):
    if n <= 1: return 1
    return F(n/2) + F(n/2) + n
```

*Hướng:* $T(n) = 2T(n/2) + n = O(n\log n)$ (giống Merge Sort).

**Bài 10:** Chứng minh $\log(n!) = O(n\log n)$.

*Hướng:* $\log(n!) = \sum_{i=1}^n \log i \le n\log n$ (mỗi số hạng $\le \log n$).

---

## 18. Tóm tắt một trang

### Định nghĩa

$$
f(n) = O(g(n)) \iff \exists c > 0, n_0 > 0 : \forall n \ge n_0,\ 0 \le f(n) \le c\,g(n)
$$

### Ý nghĩa

$f$ **bị chặn trên** bởi $g$ (sai khác hằng số) khi $n$ đủ lớn.

### Cách chứng minh

1. **Định nghĩa:** tìm $c$, $n_0$ bằng "nới lỏng".
2. **Giới hạn:** tính $\lim f/g$ (L'Hôpital, Stirling).
3. **Phản chứng:** giả sử sai → mâu thuẫn.

### Quy tắc từ code

| Cấu trúc      | Big O                     |
| ------------- | ------------------------- |
| Tuần tự       | $\max$                    |
| Lồng nhau     | tích                      |
| Điều kiện     | $\max$ các nhánh          |
| Bỏ hằng số    | $c \cdot O(f) = O(f)$     |
| Bỏ bậc thấp   | $O(n^2 + n) = O(n^2)$     |
| Log: bỏ cơ số | $O(\log_2 n) = O(\log n)$ |

### Thứ bậc

$$
O(1) \subset O(\log n) \subset O(\sqrt{n}) \subset O(n) \subset O(n\log n) \subset O(n^2) \subset O(n^3) \subset O(2^n) \subset O(n!)
$$

### Quan hệ "vàng"

- $\log^k n = O(n^\varepsilon)$ với mọi $k, \varepsilon > 0$.
- $n^k = O(c^n)$ với mọi $k > 0, c > 1$.
- $c^n = O(n!)$ với mọi $c > 1$.

### Lưu ý

- $f = O(g)$ **không** có nghĩa $f$ chính xác bằng $g$.
- $O$ là **cận trên**, không phải worst case.
- Big O bỏ hằng số — quan trọng khi $n$ nhỏ.
- Vòng lặp lồng **không luôn** $O(n^2)$ — phải đếm số lần lặp thực tế.
- Đệ quy **không luôn** chậm hơn vòng lặp — có thể cùng Big O.

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Chứng minh chi tiết** Master Theorem và các biến thể.
- **Phân tích Big O** cho một thuật toán cụ thể (Quick Sort, Dijkstra, DP) từng bước.
- **Bài tập nâng cao** về so sánh Big O phức tạp (ví dụ $n^{\log\log n}$ vs $2^{\sqrt{n}}$).
- **Big O trong thực tế** — cách đo và tối ưu code dựa trên Big O.