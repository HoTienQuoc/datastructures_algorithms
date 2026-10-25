# Phân tích các vòng lặp thường gặp (Analysis of Common Loops) — Lý thuyết + Ví dụ

## 1. Tại sao phải phân tích vòng lặp?

Trong hầu hết thuật toán, **thời gian chạy chủ yếu đến từ vòng lặp**. Vì vậy, phân tích độ phức tạp = **đếm số lần thực hiện phép toán cơ bản** bên trong vòng lặp, biểu diễn theo $n$.

**Quy trình tổng quát:**

1. Xác định **phép toán cơ bản** (phép so sánh, gán, cộng…).
2. Đếm **số lần lặp** của mỗi vòng.
3. **Nhân** các vòng lồng nhau, **cộng** các vòng tuần tự.
4. Đơn giản bằng ký hiệu tiệm cận ($\Theta$, $O$).

**Bốn mẫu vòng lặp cơ bản cần thuộc lòng:**

| Mẫu                       | Big-$\Theta$            | Khi nào          |
| ------------------------- | ----------------------- | ---------------- |
| Vòng đơn, bước $+1$       | $\Theta(n)$             | `for i = 1 to n` |
| Vòng đơn, bước $\times c$ | $\Theta(\log n)$        | `i = i * 2`      |
| Vòng lồng đều             | $\Theta(n^k)$           | $k$ vòng $n$ lần |
| Vòng lồng phụ thuộc       | $\Theta(n^2)$ hoặc khác | `for j = 1 to i` |

Phần còn lại của bài là **chi tiết + biến thể + bẫy**.

> **Lý thuyết bên cạnh:** Phân tích vòng lặp là **kỹ năng nền tảng** — hầu hết bài tập phân tích thuật toán đều quy về đây. Khi thành thạo, bạn có thể "đọc" code và **đoán ngay** độ phức tạp mà không cần đếm từng bước.

---

## 2. Vòng lặp đơn — bước cộng/trừ (linear)

### 2.1. Mẫu cơ bản

```text
for i = 1 to n:
    doing O(1) work
```

**Đếm:** $i$ nhận $1, 2, \ldots, n$ — **$n$ lần**.

$$
T(n) = n \cdot \Theta(1) = \Theta(n)
$$

### 2.2. Biến thể — khoảng chạy khác

```text
for i = 0 to n-1:       // n lần
for i = 1 to n-1:       // n-1 lần = Θ(n)
for i = 0 to n:         // n+1 lần = Θ(n)
for i = 5 to n:         // n-4 lần = Θ(n)
```

**Quy tắc:** Số lần lặp = $\text{high} - \text{low} + 1$. Hằng số dịch chuyển **không ảnh hưởng** đến $\Theta$.

### 2.3. Biến thể — bước nhảy $> 1$

```text
for i = 1 to n step 2:   // i = 1, 3, 5, ...
    doing O(1)
```

**Đếm:** $i$ nhận $\lceil n/2 \rceil$ giá trị.

$$
T(n) = \Theta(n/2) = \Theta(n)
$$

**Tổng quát — bước nhảy $c$:**

```text
for i = 1 to n step c:
```

Số lần lặp: $\lceil n/c \rceil = \Theta(n)$ — **bước nhảy hằng số không đổi bậc**.

### 2.4. Biến thể — đi ngược

```text
for i = n downto 1:
    doing O(1)
```

Vẫn $n$ lần ⇒ $\Theta(n)$.

### 2.5. Ví dụ đầy đủ — tính tổng

```text
sum = 0
for i = 1 to n:
    sum += i
```

**Phân tích:**

- Phép toán cơ bản: `sum += i` (gán + cộng).
- Số lần: $n$.
- Tổng: $\Theta(n)$.

**Kiểm chứng:** Tổng thực tế là $\frac{n(n+1)}{2}$ — nhưng **số phép toán** là $n$, không phải $n^2/2$. Đây là điểm quan trọng: **giá trị tính được** khác với **số phép tính**.

> **Lý thuyết bên cạnh:** Đừng nhầm "kết quả của hàm" với "số phép toán để tính kết quả". Ví dụ trên tính tổng $\Theta(n^2)$ nhưng chỉ dùng $\Theta(n)$ phép. Đây là lý do dùng công thức đóng (closed form) nhanh hơn vòng lặp.

---

## 3. Vòng lặp đơn — bước nhân/chia (logarithmic)

### 3.1. Mẫu cơ bản — nhân đôi

```text
i = 1
while i < n:
    doing O(1)
    i = i * 2
```

**Đếm:** $i$ nhận $1, 2, 4, 8, \ldots, 2^k$ với $2^k < n$.

Giá trị cuối: $2^k \ge n$ ⇒ $k = \lceil \log_2 n \rceil$.

$$
T(n) = \Theta(\log n)
$$

**Trực giác:** Khi $n$ tăng gấp đôi, số lần lặp **chỉ tăng thêm 1**. Đây là lý do $\Theta(\log n)$ cực nhanh.

### 3.2. Biến thể — nhân $c$

```text
i = 1
while i < n:
    doing O(1)
    i = i * c        // c > 1
```

Số lần: $\lceil \log_c n \rceil = \Theta(\log n)$.

**Cơ số không quan trọng** vì $\log_c n = \frac{\log_2 n}{\log_2 c}$ — chỉ khác hằng số.

### 3.3. Biến thể — chia đôi

```text
i = n
while i > 1:
    doing O(1)
    i = i / 2
```

$i$ nhận $n, n/2, n/4, \ldots, 1$. Số lần: $\lfloor \log_2 n \rfloor = \Theta(\log n)$.

### 3.4. Biến thể — chia $c$

```text
i = n
while i >= 1:
    doing O(1)
    i = i / c        // c > 1
```

Số lần: $\lceil \log_c n \rceil = \Theta(\log n)$.

### 3.5. Bẫy — bước nhân nhưng khởi đầu khác

```text
i = 2
while i <= n:
    doing O(1)
    i = i * 3
```

$i$ nhận $2, 6, 18, \ldots$ — tức $2 \cdot 3^k \le n$ ⇒ $k \le \log_3(n/2)$.

Số lần: $\Theta(\log n)$ — **hằng số khởi đầu không ảnh hưởng bậc**.

### 3.6. Bẫy — vòng lặp logarit lồng nhau

```text
for i = 1 to n:              // n lần
    j = 1
    while j < n:             // log n lần
        doing O(1)
        j = j * 2
```

Vòng ngoài $n$, vòng trong $\log n$:

$$
T(n) = n \cdot \Theta(\log n) = \Theta(n\log n)
$$

**Đây là mẫu của Merge Sort, Heap Sort.**

### 3.7. Ví dụ — vòng lặp logarit kép (log log)

```text
i = 2
while i < n:
    doing O(1)
    i = i * i        // bình phương mỗi bước
```

$i$ nhận $2, 4, 16, 256, \ldots$ — tức $i_k = 2^{2^k}$. Điều kiện $2^{2^k} < n$ ⇒ $2^k < \log_2 n$ ⇒ $k < \log_2\log_2 n$.

$$
T(n) = \Theta(\log\log n)
$$

**Cực nhanh** — nhưng hiếm gặp trong thực tế. Ví dụ: một số thuật toán số học (tính gcd nhanh, FFT đa cấp).

> **Lý thuyết bên cạnh:** Ba mẫu vòng đơn cần phân biệt:
> - **Cộng/trừ** (bước $+1$ hoặc $+c$): $\Theta(n)$.
> - **Nhân/chia** (bước $\times c$ hoặc $/c$): $\Theta(\log n)$.
> - **Nhân/chia lặp** (bước $i \cdot i$ hoặc $\sqrt{i}$): $\Theta(\log\log n)$.
>
> Khi đọc code, **nhìn bước cập nhật biến** để đoán ngay.

---

## 4. Vòng lặp lồng nhau — độc lập (independent nested)

### 4.1. Mẫu cơ bản — hai vòng đều $n$

```text
for i = 1 to n:
    for j = 1 to n:
        doing O(1)
```

**Đếm:** Vòng ngoài $n$ lần, mỗi lần vòng trong $n$ lần ⇒ $n \cdot n = n^2$.

$$
T(n) = \Theta(n^2)
$$

### 4.2. Ba vòng độc lập

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n:
            doing O(1)
```

$$
T(n) = n \cdot n \cdot n = \Theta(n^3)
$$

### 4.3. Tổng quát — $k$ vòng độc lập

$k$ vòng lồng, mỗi vòng $n$ lần ⇒ $\Theta(n^k)$.

**Ví dụ thực tế:**

- $k=2$: Bubble Sort, Selection Sort, nhân ma trận naive ($\Theta(n^3)$ thực ra $k=3$).
- $k=3$: Floyd-Warshall, Matrix Chain DP.

### 4.4. Biến thể — vòng trong có giới hạn khác

```text
for i = 1 to n:              // n lần
    for j = 1 to m:          // m lần
        doing O(1)
```

$$
T(n, m) = \Theta(nm)
$$

**Không** phải $\Theta(n^2)$ trừ khi $m = n$.

### 4.5. Biến thể — vòng trong bước nhảy

```text
for i = 1 to n:              // n lần
    for j = 1 to n step 3:   // n/3 lần
        doing O(1)
```

$$
T(n) = n \cdot \Theta(n/3) = \Theta(n^2)
$$

Bước nhảy hằng số **không đổi bậc**.

### 4.6. Biến thể — vòng trong logarit

```text
for i = 1 to n:              // n lần
    j = 1
    while j < n:             // log n lần
        doing O(1)
        j = j * 2
```

$$
T(n) = \Theta(n\log n)
$$

### 4.7. Biến thể — vòng ngoài logarit, vòng trong tuyến tính

```text
i = 1
while i < n:                 // log n lần
    for j = 1 to n:          // n lần
        doing O(1)
    i = i * 2
```

$$
T(n) = \Theta(\log n) \cdot \Theta(n) = \Theta(n\log n)
$$

> **Lý thuyết bên cạnh:** Với vòng lồng **độc lập**, quy tắc rất đơn giản: **nhân số lần lặp**. Chỉ cần phân tích từng vòng riêng, rồi nhân. Phức tạp chỉ xuất hiện khi vòng trong **phụ thuộc** vòng ngoài (mục 5).

---

## 5. Vòng lặp lồng nhau — phụ thuộc (dependent nested)

### 5.1. Mẫu tam giác — vòng trong chạy đến $i$

```text
for i = 1 to n:
    for j = 1 to i:
        doing O(1)
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \Theta(n^2)
$$

**Trực giác:** Vòng trong chạy $1, 2, 3, \ldots, n$ lần — tổng là $\frac{n^2}{2}$.

**Hằng số $\frac{1}{2}$ không ảnh hưởng** — vẫn $\Theta(n^2)$.

### 5.2. Mẫu tam giác ngược — vòng trong từ $i$ đến $n$

```text
for i = 1 to n:
    for j = i to n:
        doing O(1)
```

**Đếm:** Vòng trong chạy $n - i + 1$ lần.

$$
T(n) = \sum_{i=1}^{n} (n - i + 1) = n + (n-1) + \cdots + 1 = \frac{n(n+1)}{2} = \Theta(n^2)
$$

**Kết quả giống hệt** mẫu trên — chỉ khác thứ tự.

### 5.3. Mẫu "tam giác" ba tầng

```text
for i = 1 to n:
    for j = 1 to i:
        for k = 1 to j:
            doing O(1)
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{n} \sum_{j=1}^{i} j = \sum_{i=1}^{n} \frac{i(i+1)}{2} = \frac{1}{2}\sum_{i=1}^{n} (i^2 + i) = \Theta(n^3)
$$

**Vì** $\sum i^2 = \Theta(n^3)$ — số hạng bậc cao nhất quyết định.

### 5.4. Mẫu "tam giác" — vòng trong bắt đầu từ $i$

```text
for i = 1 to n:
    for j = i to n:
        for k = i to j:
            doing O(1)
```

**Đếm:** Với mỗi $i$, vòng giữa chạy $n - i + 1$ lần, vòng trong chạy $j - i + 1$ lần.

$$
T(n) = \sum_{i=1}^{n} \sum_{j=i}^{n} (j - i + 1)
$$

Đặt $m = n - i + 1$. Khi $j$ từ $i$ đến $n$, $j - i + 1$ từ $1$ đến $m$:

$$
\sum_{j=i}^{n} (j - i + 1) = \frac{m(m+1)}{2}
$$

Vậy:

$$
T(n) = \sum_{i=1}^{n} \frac{(n-i+1)(n-i+2)}{2} = \sum_{m=1}^{n} \frac{m(m+1)}{2} = \Theta(n^3)
$$

### 5.5. Mẫu với $j$ bắt đầu từ $i^2$ hoặc hàm khác

```text
for i = 1 to n:
    for j = i to i*i:       // j chạy i^2 - i + 1 lần
        doing O(1)
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{n} (i^2 - i + 1) = \Theta(n^3)
$$

**Vì** $\sum i^2 = \Theta(n^3)$.

### 5.6. Mẫu với vòng trong có điều kiện phức tạp

```text
for i = 1 to n:
    for j = 1 to n:
        if i + j == k:    // hiếm khi đúng
            doing O(1)
```

**Quan trọng:** Số lần **kiểm tra điều kiện** là $n^2$, dù thân `if` hiếm khi chạy.

$$
T(n) = \Theta(n^2)
$$

**Bài học:** Phân tích phải đếm **mọi phép toán**, kể cả kiểm tra điều kiện — không chỉ thân `if`.

> **Lý thuyết bên cạnh:** Với vòng lồng phụ thuộc, **luôn viết tổng** rồi tính. Đừng đoán. Tổng thường là cấp số cộng ($\Theta(n^2)$), cấp số nhân ($\Theta(2^n)$), hoặc tổng bình phương ($\Theta(n^3)$). Khi thành thạo, bạn nhận ra mẫu ngay — nhưng viết tổng giúp **chắc chắn** và **tránh sai**.

---

## 6. Vòng lặp lồng nhau — cận phụ thuộc phức tạp

### 6.1. Vòng trong chạy $\log i$ lần

```text
for i = 1 to n:
    j = 1
    while j < i:
        doing O(1)
        j = j * 2
```

**Đếm:** Với mỗi $i$, vòng trong chạy $\lceil \log_2 i \rceil$ lần.

$$
T(n) = \sum_{i=1}^{n} \log i = \log(n!) = \Theta(n\log n)
$$

**Dùng xấp xỉ Stirling:** $\log(n!) = \Theta(n\log n)$.

### 6.2. Vòng trong chạy $\sqrt{i}$ lần

```text
for i = 1 to n:
    for j = 1 to sqrt(i):
        doing O(1)
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{n} \sqrt{i} \approx \int_0^n \sqrt{x}\,dx = \frac{2}{3}n^{3/2} = \Theta(n^{3/2})
$$

**Dùng tích phân** để xấp xỉ tổng — kỹ thuật chuẩn khi tổng không có công thức đóng.

### 6.3. Vòng trong chạy $n/i$ lần

```text
for i = 1 to n:
    for j = 1 to n/i:
        doing O(1)
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{n} \frac{n}{i} = n \sum_{i=1}^{n} \frac{1}{i} = n H_n = \Theta(n\log n)
$$

**Với** $H_n = \sum 1/i = \Theta(\log n)$ là số harmonic.

### 6.4. Vòng trong chạy $i^2$ lần

```text
for i = 1 to n:
    for j = 1 to i*i:
        doing O(1)
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{n} i^2 = \Theta(n^3)
$$

### 6.5. Vòng lặp với $i$ tăng theo cấp số nhân (nhưng vẫn tuyến tính tổng)

```text
i = 1
while i <= n:
    for j = 1 to i:
        doing O(1)
    i = i * 2
```

**Đếm:** $i$ nhận $1, 2, 4, \ldots, 2^k$ với $2^k \le n$. Số lần vòng trong: $1 + 2 + 4 + \cdots + 2^k = 2^{k+1} - 1$.

$$
T(n) = \Theta(n)
$$

**Bất ngờ:** Dù có vòng lồng, tổng vẫn **tuyến tính** — vì cấp số nhân cộng dồn $\approx 2 \times$ số hạng cuối.

**Bài học:** Vòng lồng **không luôn** là $\Theta(n^2)$ — phải tính tổng.

### 6.6. Ví dụ phức tạp — vòng trong chạy $n - i^2$ lần

```text
for i = 1 to sqrt(n):
    for j = 1 to n - i*i:
        doing O(1)
```

**Đếm:**

$$
T(n) = \sum_{i=1}^{\sqrt{n}} (n - i^2) = n\sqrt{n} - \sum_{i=1}^{\sqrt{n}} i^2
$$

$\sum_{i=1}^{\sqrt{n}} i^2 = \Theta((\sqrt{n})^3) = \Theta(n^{3/2})$.

Vậy:
$$
T(n) = n^{3/2} - \Theta(n^{3/2}) = \Theta(n^{3/2})
$$

> **Lý thuyết bên cạnh:** Khi tổng không có công thức đóng, dùng **tích phân** để xấp xỉ:
> $$\sum_{i=1}^{n} f(i) \approx \int_0^n f(x)\,dx$$
> với điều kiện $f$ đơn điệu. Đây là công cụ chuẩn cho các tổng phức tạp như $\sum \sqrt{i}$, $\sum i\log i$, $\sum n/i$.

---

## 7. Nhiều vòng lặp tuần tự (sequential loops)

### 7.1. Quy tắc cộng

Khi có nhiều vòng **nối tiếp** (không lồng), **cộng** số lần lặp:

$$
T(n) = T_1(n) + T_2(n) + \cdots + T_k(n) = \Theta(\max_i T_i(n))
$$

Lấy **max** vì số hạng lớn nhất áp đảo.

### 7.2. Ví dụ cơ bản

```text
for i = 1 to n:              // Θ(n)
    doing O(1)
for i = 1 to n:              // Θ(n^2)
    for j = 1 to n:
        doing O(1)
for i = 1 to n:              // Θ(n)
    doing O(1)
```

$$
T(n) = \Theta(n) + \Theta(n^2) + \Theta(n) = \Theta(n^2)
$$

### 7.3. Ví dụ với logarit

```text
for i = 1 to n:              // Θ(n)
    doing O(1)
i = 1
while i < n:                 // Θ(log n)
    doing O(1)
    i = i * 2
```

$$
T(n) = \Theta(n) + \Theta(\log n) = \Theta(n)
$$

### 7.4. Bẫy — số đoạn phụ thuộc $n$

```text
for k = 1 to n:              // n đoạn
    for i = 1 to n:          // mỗi đoạn Θ(n)
        doing O(1)
```

**Sai nếu nói** $\Theta(n)$ (lấy max của $n$ đoạn $\Theta(n)$).

**Đúng:** Có $n$ đoạn, mỗi đoạn $\Theta(n)$, tổng $n \cdot \Theta(n) = \Theta(n^2)$.

**Quy tắc cộng chỉ đúng khi số đoạn là hằng số.** Với $n$ đoạn, phải **nhân**.

### 7.5. Ví dụ — đoạn có điều kiện

```text
if n > 100:
    for i = 1 to n:
        for j = 1 to n:
            doing O(1)       // Θ(n^2) nếu nhánh này chạy
else:
    for i = 1 to n:
        doing O(1)           // Θ(n)
```

**Phân tích:**

- **Worst case** ($n > 100$): $\Theta(n^2)$.
- **Best case** ($n \le 100$): $\Theta(n)$ — nhưng thực ra với $n$ nhỏ, $\Theta(n)$ bị chặn bởi hằng số ⇒ $\Theta(1)$.

**Kết luận:** $T(n) = O(n^2)$ và $T(n) = \Omega(n)$ (hoặc $\Omega(1)$ nếu $n$ cố định nhỏ).

> **Lý thuyết bên cạnh:** Khi phân tích code có nhiều nhánh, **phân tích từng nhánh** rồi lấy **max** (worst case) hoặc **min** (best case). Đừng gộp chung — vì mỗi nhánh có thể có bậc khác nhau.

---

## 8. Các mẫu vòng lặp đặc biệt

### 8.1. Vòng lặp có điều kiện dừng phức tạp

```text
i = 1
while i * i <= n:            // i <= sqrt(n)
    doing O(1)
    i = i + 1
```

$i$ từ $1$ đến $\lfloor\sqrt{n}\rfloor$ ⇒ $\Theta(\sqrt{n})$.

**Ứng dụng:** Kiểm tra số nguyên tố — thử chia đến $\sqrt{n}$.

### 8.2. Vòng lặp với bước thay đổi (biến thiên)

```text
i = 1
while i <= n:
    doing O(1)
    i = i + i/2              // i tăng 1.5 lần
```

$i$ tăng theo cấp số nhân (tỉ lệ $1.5$): $i_k = 1.5^k$.

$1.5^k \le n$ ⇒ $k \le \log_{1.5} n = \Theta(\log n)$.

### 8.3. Vòng lặp với hai biến

```text
i = 1
j = n
while i < j:
    doing O(1)
    i = i + 1
    j = j - 1
```

Mỗi bước $i$ tăng 1, $j$ giảm 1 — khoảng cách giảm 2. Số bước: $\lfloor n/2 \rfloor = \Theta(n)$.

### 8.4. Vòng lặp với điều kiện AND/OR

```text
i = 1
while i < n and A[i] != x:   // dừng khi tìm thấy hoặc hết mảng
    i = i + 1
```

**Phân tích:**

- **Best case** ($A[1] = x$): $\Theta(1)$.
- **Worst case** ($x$ không có): $\Theta(n)$.
- **Average case:** $\Theta(n)$ (nếu $x$ phân bố đều).

### 8.5. Vòng lặp lồng với điều kiện phức tạp

```text
for i = 1 to n:
    for j = 1 to n:
        if A[i] + A[j] == target:
            doing O(1)
```

**Quan trọng:** Số lần **kiểm tra** là $n^2$ — dù thân `if` có thể chạy ít hơn. Vậy $\Theta(n^2)$.

**Đây là bài toán 2-SUM** — brute force $\Theta(n^2)$. Có thể tối ưu xuống $\Theta(n)$ bằng hash table (nhưng phải phân tích riêng).

### 8.6. Vòng lặp đệ quy lồng vòng lặp

```text
F(n):
    if n <= 1: return
    for i = 1 to n:
        doing O(1)           // Θ(n) mỗi lần gọi
    F(n-1)
```

**Hệ thức:**

$$
T(n) = T(n-1) + \Theta(n)
$$

**Giải:**

$$
T(n) = \sum_{k=1}^{n} \Theta(k) = \Theta(n^2)
$$

Đây là **Selection Sort** — $\Theta(n^2)$.

> **Lý thuyết bên cạnh:** Vòng lặp và đệ quy **tương đương** về sức mạnh. Mọi vòng lặp đều viết được bằng đệ quy và ngược lại. Khi phân tích, dùng công cụ phù hợp: vòng lặp → tổng; đệ quy → hệ thức truy hồi.

---

## 9. Bảng tổng hợp các mẫu vòng lặp

### 9.1. Vòng đơn

| Mẫu                      | Số lần       | Big-$\Theta$         |
| ------------------------ | ------------ | -------------------- |
| `for i = 1 to n`         | $n$          | $\Theta(n)$          |
| `for i = 1 to n step c`  | $n/c$        | $\Theta(n)$          |
| `for i = n downto 1`     | $n$          | $\Theta(n)$          |
| `while i < n: i *= 2`    | $\log_2 n$   | $\Theta(\log n)$     |
| `while i < n: i *= c`    | $\log_c n$   | $\Theta(\log n)$     |
| `while i < n: i = i*i`   | $\log\log n$ | $\Theta(\log\log n)$ |
| `while i*i <= n: i += 1` | $\sqrt{n}$   | $\Theta(\sqrt{n})$   |

### 9.2. Vòng lồng

| Mẫu                                        | Tổng                                      | Big-$\Theta$      |
| ------------------------------------------ | ----------------------------------------- | ----------------- |
| `for i = 1..n, for j = 1..n`               | $n^2$                                     | $\Theta(n^2)$     |
| `for i = 1..n, for j = 1..i`               | $n(n+1)/2$                                | $\Theta(n^2)$     |
| `for i = 1..n, for j = i..n`               | $n(n+1)/2$                                | $\Theta(n^2)$     |
| `for i = 1..n, for j = 1..i, for k = 1..j` | $n(n+1)(n+2)/6$                           | $\Theta(n^3)$     |
| `for i = 1..n, while j < n: j *= 2`        | $n\log n$                                 | $\Theta(n\log n)$ |
| `for i = 1..n, for j = 1..i*i`             | $\sum i^2 = n^3/3$                        | $\Theta(n^3)$     |
| `for i = 1..n, for j = 1..sqrt(i)`         | $\sum\sqrt{i} \approx \frac{2}{3}n^{3/2}$ | $\Theta(n^{3/2})$ |
| `for i = 1..n, for j = 1..n/i`             | $nH_n$                                    | $\Theta(n\log n)$ |
| `while i <= n, for j = 1..i: i *= 2`       | $1+2+4+\cdots+n \approx 2n$               | $\Theta(n)$       |

### 9.3. Quy tắc nhanh

| Cấu trúc                  | Quy tắc              |
| ------------------------- | -------------------- |
| Tuần tự                   | **Cộng** (lấy max)   |
| Lồng nhau                 | **Nhân**             |
| Bước $+1$ hoặc $+c$       | $\Theta(n)$          |
| Bước $\times c$ hoặc $/c$ | $\Theta(\log n)$     |
| Bước $\times$ chính nó    | $\Theta(\log\log n)$ |
| Điều kiện $i^2 \le n$     | $\Theta(\sqrt{n})$   |
| Vòng trong phụ thuộc $i$  | **Viết tổng** $\sum$ |

---

## 10. Kỹ thuật tính tổng

### 10.1. Công thức có sẵn

**Cấp số cộng:**

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \Theta(n^2)
$$

$$
\sum_{i=1}^{n} (a + (i-1)d) = \frac{n(2a + (n-1)d)}{2} = \Theta(n^2)
$$

**Cấp số nhân:**

$$
\sum_{i=0}^{n} r^i = \frac{r^{n+1} - 1}{r - 1} = \Theta(r^n) \quad (r > 1)
$$

$$
\sum_{i=0}^{\infty} r^i = \frac{1}{1-r} = \Theta(1) \quad (|r| < 1)
$$

**Tổng bình phương, lập phương:**

$$
\sum_{i=1}^{n} i^2 = \frac{n(n+1)(2n+1)}{6} = \Theta(n^3)
$$

$$
\sum_{i=1}^{n} i^3 = \left(\frac{n(n+1)}{2}\right)^2 = \Theta(n^4)
$$

**Harmonic:**

$$
H_n = \sum_{i=1}^{n} \frac{1}{i} = \ln n + O(1) = \Theta(\log n)
$$

**Log:**

$$
\sum_{i=1}^{n} \log i = \log(n!) = \Theta(n\log n)
$$

### 10.2. Kỹ thuật tích phân

Khi $f$ đơn điệu tăng:

$$
\int_0^n f(x)\,dx \le \sum_{i=1}^{n} f(i) \le \int_1^{n+1} f(x)\,dx
$$

**Suy ra** $\sum f(i) = \Theta\left(\int f\right)$.

**Ví dụ:**

- $\sum_{i=1}^{n} \sqrt{i} = \Theta(n^{3/2})$ (vì $\int \sqrt{x}\,dx = \frac{2}{3}x^{3/2}$).
- $\sum_{i=1}^{n} \frac{1}{i} = \Theta(\log n)$ (vì $\int 1/x\,dx = \ln x$).
- $\sum_{i=1}^{n} i\log i = \Theta(n^2\log n)$ (vì $\int x\log x\,dx = \frac{x^2}{2}\log x - \frac{x^2}{4}$).

### 10.3. Kỹ thuật "chặn bằng nửa sau"

Khi tổng có số hạng tăng, chỉ lấy nửa sau:

**Ví dụ:** $\sum_{i=1}^{n} i$

$$
\sum_{i=1}^{n} i \ge \sum_{i=\lceil n/2 \rceil}^{n} i \ge \frac{n}{2} \cdot \frac{n}{2} = \frac{n^2}{4}
$$

Vậy $\sum i = \Omega(n^2)$; kết hợp với $\le n \cdot n = O(n^2)$ ⇒ $\Theta(n^2)$.

### 10.4. Kỹ thuật "chặn bằng max"

Chặn trên mỗi số hạng bằng max:

**Ví dụ:** $\sum_{i=1}^{n} \log i$

$$
\sum_{i=1}^{n} \log i \le \sum_{i=1}^{n} \log n = n\log n
$$

### 10.5. Kỹ thuật tách tổng

Tách tổng thành các phần dễ tính:

**Ví dụ:** $\sum_{i=1}^{n} (i^2 + 3i + 2)$

$$
= \sum i^2 + 3\sum i + 2\sum 1 = \Theta(n^3) + \Theta(n^2) + \Theta(n) = \Theta(n^3)
$$

> **Lý thuyết bên cạnh:** Khi tính tổng, **không cần công thức chính xác** — chỉ cần **bậc**. Vì vậy có thể dùng chặn (bounding) thay vì tính đúng. Đây là mẹo giúp phân tích nhanh hơn nhiều.

---

## 11. Các bẫy phổ biến

### Bẫy 1 — "Vòng lồng luôn là $n^2$"

**Sai.** Phải đếm số lần lặp thực tế:

```text
for i = 1 to n:           // n lần
    for j = 1 to 10:      // 10 lần — KHÔNG phụ thuộc n
        doing O(1)
```

Tổng: $10n = \Theta(n)$ — **không phải** $\Theta(n^2)$.

### Bẫy 2 — "Vòng trong chạy $i$ lần nên tổng là $n \cdot n$"

**Sai.** Vòng trong chạy $i$ lần, tổng là $\sum i = n^2/2$ — **không phải** $n^2$. Dù cùng $\Theta(n^2)$, nhưng nếu so sánh **hằng số**, cần tính chính xác.

### Bẫy 3 — "Đệ quy luôn chậm hơn vòng lặp"

**Sai.** Đệ quy và vòng lặp có thể cùng bậc. Ví dụ tìm max đệ quy $\Theta(n)$ — giống vòng lặp.

### Bẫy 4 — "Vòng lặp có `break` vẫn là $O(n)$"

**Phụ thuộc vị trí `break`:**

```text
for i = 1 to n:
    if A[i] == x: break
```

- **Best case:** $\Theta(1)$ ($A[1] = x$).
- **Worst case:** $\Theta(n)$ (không có $x$).

**Không** thể nói $\Theta(n)$ — phải nói case.

### Bẫy 5 — "Bỏ qua chi phí kiểm tra điều kiện"

```text
for i = 1 to n:
    for j = 1 to n:
        if điều_kiện_hiếm:
            doing O(1)
```

Số lần **kiểm tra** là $n^2$ — dù thân `if` hiếm chạy. Vậy $\Theta(n^2)$.

### Bẫy 6 — "Hằng số không quan trọng với $n$ nhỏ"

Đúng về tiệm cận, **sai trong thực tế**. $\Theta(n^2)$ với hằng số $0.001$ có thể nhanh hơn $\Theta(n\log n)$ với hằng số $1000$ khi $n < 10^6$.

### Bẫy 7 — "Vòng lặp logarit luôn $\log_2 n$"

**Sai.** Cơ số phụ thuộc bước nhân:

```text
i = 1
while i < n:
    i = i * 3      // log_3 n, không phải log_2 n
```

Nhưng **bậc** vẫn $\Theta(\log n)$ — chỉ khác hằng số.

### Bẫy 8 — "Cộng các đoạn nối tiếp luôn đúng"

**Sai khi số đoạn phụ thuộc $n$:**

```text
for k = 1 to n:          // n đoạn
    for i = 1 to n:
        doing O(1)
```

**Không phải** $\Theta(n)$ — mà là $n \cdot \Theta(n) = \Theta(n^2)$.

### Bẫy 9 — "Nhân các vòng lồng luôn đúng"

**Sai khi vòng trong phụ thuộc vòng ngoài** — phải viết tổng:

```text
for i = 1 to n:
    for j = 1 to i:      // không phải n lần!
        doing O(1)
```

**Không phải** $n \cdot n = n^2$ theo nghĩa chính xác — mà là $n(n+1)/2$. Cùng bậc nhưng khác hằng số.

### Bẫy 10 — "Vòng lặp với biến nhân là $\Theta(\log n)$"

**Phụ thuộc giá trị khởi đầu và điều kiện:**

```text
i = 1
while i < n:
    doing O(1)
    i = i * i        // Θ(log log n), KHÔNG phải Θ(log n)
```

**Nhân đôi** → $\Theta(\log n)$. **Nhân chính nó** → $\Theta(\log\log n)$. Phải phân biệt.

> **Lý thuyết bên cạnh:** Hầu hết bẫy đến từ việc **áp dụng quy tắc máy móc** mà không đọc kỹ điều kiện. Quy tắc "vòng lồng = nhân" chỉ đúng khi vòng trong **độc lập** với vòng ngoài. Khi phụ thuộc, **luôn viết tổng** rồi tính.

---

## 12. Ví dụ tổng hợp — phân tích từng bước

### Ví dụ 1

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to i:
            doing O(1)
```

**Phân tích:**

- Vòng $i$: $n$ lần.
- Vòng $j$: $n$ lần (độc lập với $i$).
- Vòng $k$: $i$ lần (phụ thuộc $i$).

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} \sum_{j=1}^{n} i = \sum_{i=1}^{n} n \cdot i = n \sum_{i=1}^{n} i = n \cdot \frac{n(n+1)}{2} = \Theta(n^3)
$$

### Ví dụ 2

```text
for i = 1 to n:
    j = n
    while j > 0:
        doing O(1)
        j = j / 2
```

**Phân tích:**

- Vòng $i$: $n$ lần.
- Vòng trong: $j$ giảm một nửa mỗi bước ⇒ $\log_2 n$ lần.

$$
T(n) = n \cdot \log n = \Theta(n\log n)
$$

### Ví dụ 3

```text
i = n
while i >= 1:
    for j = 1 to n:
        doing O(1)
    i = i / 3
```

**Phân tích:**

- Vòng ngoài: $i = n, n/3, n/9, \ldots, 1$ ⇒ $\log_3 n$ lần.
- Vòng trong: $n$ lần mỗi bước.

$$
T(n) = \log_3 n \cdot n = \Theta(n\log n)
$$

### Ví dụ 4

```text
for i = 1 to n:
    for j = 1 to n:
        if i == j:
            for k = 1 to n:
                doing O(1)
```

**Phân tích:**

- Vòng $i, j$: $n^2$ cặp.
- Thân `if` chạy $n$ lần — chỉ khi $i = j$ (có $n$ cặp như vậy).
- Trong mỗi lần, vòng $k$: $n$ lần.

**Tổng:**

$$
T(n) = n^2 \cdot \Theta(1) + n \cdot n = \Theta(n^2) + \Theta(n^2) = \Theta(n^2)
$$

### Ví dụ 5

```text
for i = 1 to n:
    j = i
    while j < n:
        doing O(1)
        j = j * 2
```

**Phân tích:**

Với mỗi $i$, vòng trong chạy từ $j = i$ đến $j = n$ với bước nhân đôi. Số lần: $\lceil \log_2(n/i) \rceil + 1$.

$$
T(n) = \sum_{i=1}^{n} \log_2\!\left(\frac{n}{i}\right) = \sum_{i=1}^{n} (\log n - \log i) = n\log n - \log(n!) = n\log n - \Theta(n\log n)
$$

**Kết quả:** $\Theta(n\log n)$.

**Lưu ý tinh tế:** Dù trông phức tạp, kết quả vẫn $\Theta(n\log n)$ — vì cả hai số hạng đều $\Theta(n\log n)$ và không triệt tiêu về bậc.

### Ví dụ 6

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n:
            if i + j + k == n:
                doing O(1)
```

**Phân tích:**

Số lần **kiểm tra** điều kiện là $n^3$ — dù thân `if` chạy ít hơn.

**Tổng:** $\Theta(n^3)$.

**Bài học:** Phân tích đếm **phép toán cơ bản** — kiểm tra điều kiện là một phép toán.

### Ví dụ 7

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to j:
            doing O(1)
```

**Phân tích:**

- Vòng $i$: $n$ lần.
- Vòng $j$: $n$ lần.
- Vòng $k$: $j$ lần — phụ thuộc $j$.

$$
T(n) = \sum_{i=1}^{n} \sum_{j=1}^{n} j = \sum_{i=1}^{n} \frac{n(n+1)}{2} = n \cdot \frac{n(n+1)}{2} = \Theta(n^3)
$$

### Ví dụ 8

```text
sum = 0
for i = 1 to n:
    for j = 1 to i:
        sum += i * j
```

**Phân tích:**

- Vòng $i$: $n$ lần.
- Vòng $j$: $i$ lần — phụ thuộc $i$.
- Mỗi phép: $\Theta(1)$.

$$
T(n) = \sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \Theta(n^2)
$$

**Lưu ý:** Dù giá trị tính được là $\Theta(n^4)$ (vì $\sum_{i,j} i \cdot j$ có bậc 4), **số phép toán** chỉ là $\Theta(n^2)$. Đây là điểm khác biệt quan trọng giữa **kết quả** và **chi phí tính**.

---

## 13. Bài tập tự luyện

**Bài 1:** Phân tích:

```text
for i = 1 to n:
    for j = i to n:
        doing O(1)
```

*Hướng:* $\sum_{i=1}^{n} (n-i+1) = n^2/2 + n/2 = \Theta(n^2)$.

**Bài 2:** Phân tích:

```text
i = n
while i >= 1:
    doing O(1)
    i = i / 2
```

*Hướng:* $\Theta(\log n)$ — chia đôi mỗi bước.

**Bài 3:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to i:
        for k = 1 to j:
            doing O(1)
```

*Hướng:* $\sum_{i=1}^{n} \sum_{j=1}^{i} j = \sum_{i=1}^{n} i(i+1)/2 = \Theta(n^3)$.

**Bài 4:** Phân tích:

```text
for i = 1 to n:
    j = 1
    while j < i:
        doing O(1)
        j = j * 3
```

*Hướng:* $\sum_{i=1}^{n} \log_3 i = \Theta(n\log n)$.

**Bài 5:** Phân tích:

```text
i = 1
while i * i * i <= n:
    doing O(1)
    i = i + 1
```

*Hướng:* $i \le n^{1/3}$ ⇒ $\Theta(n^{1/3})$.

**Bài 6:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to n/i:
        doing O(1)
```

*Hướng:* $\sum n/i = nH_n = \Theta(n\log n)$.

**Bài 7:** Phân tích:

```text
for i = 1 to n:
    j = n
    while j >= 1:
        doing O(1)
        j = j - i        // trừ i mỗi bước
```

*Hướng:* Với mỗi $i$, vòng trong chạy $\lceil n/i \rceil$ lần. Tổng: $\sum n/i = nH_n = \Theta(n\log n)$.

**Bài 8:** Phân tích:

```text
i = 1
while i <= n:
    for j = 1 to i:
        doing O(1)
    i = i * 2
```

*Hướng:* Tổng $1 + 2 + 4 + \cdots + 2^k = 2^{k+1} - 1 \approx 2n$ ⇒ $\Theta(n)$.

**Bài 9:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to n:
        if (i + j) % 2 == 0:
            doing O(1)
```

*Hướng:* Số lần kiểm tra là $n^2$; thân `if` chạy $\approx n^2/2$ lần. Tổng: $\Theta(n^2)$.

**Bài 10:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to sqrt(i):
        for k = 1 to j:
            doing O(1)
```

*Hướng:* $\sum_{i=1}^{n} \sum_{j=1}^{\sqrt{i}} j = \sum_{i=1}^{n} \frac{\sqrt{i}(\sqrt{i}+1)}{2} \approx \sum i/2 = \Theta(n^2)$.

---

## 14. Tóm tắt một trang

### Bốn mẫu vàng

| Mẫu                      | Big-$\Theta$     |
| ------------------------ | ---------------- |
| `for i = 1 to n`         | $\Theta(n)$      |
| `while i < n: i *= c`    | $\Theta(\log n)$ |
| Hai vòng độc lập $n$ lần | $\Theta(n^2)$    |
| `for i=1..n, for j=1..i` | $\Theta(n^2)$    |

### Quy tắc

1. **Tuần tự** → **cộng** (lấy max).
2. **Lồng nhau độc lập** → **nhân**.
3. **Lồng nhau phụ thuộc** → **viết tổng** $\sum$, rồi tính.
4. Bước $+c$ → $\Theta(n)$.
5. Bước $\times c$ → $\Theta(\log n)$.
6. Bước $\times$ chính nó → $\Theta(\log\log n)$.
7. Điều kiện $i^2 \le n$ → $\Theta(\sqrt{n})$.

### Công thức tổng

| Tổng                      | Big-$\Theta$      |
| ------------------------- | ----------------- |
| $\sum_{i=1}^{n} 1$        | $\Theta(n)$       |
| $\sum_{i=1}^{n} i$        | $\Theta(n^2)$     |
| $\sum_{i=1}^{n} i^2$      | $\Theta(n^3)$     |
| $\sum_{i=1}^{n} 1/i$      | $\Theta(\log n)$  |
| $\sum_{i=1}^{n} \log i$   | $\Theta(n\log n)$ |
| $\sum_{i=1}^{n} \sqrt{i}$ | $\Theta(n^{3/2})$ |
| $\sum_{i=0}^{n} 2^i$      | $\Theta(2^n)$     |

### Bẫy cần tránh

1. Vòng lồng **không luôn** $n^2$ — phải đếm.
2. Vòng trong **phụ thuộc** $i$ — phải viết tổng.
3. **Kiểm tra điều kiện** cũng là phép toán — phải đếm.
4. Số đoạn **phụ thuộc $n$** — phải nhân, không cộng.
5. **Giá trị tính được** khác **số phép toán**.
6. **$n$ nhỏ** — hằng số có thể quan trọng.
7. **Nhân chính nó** → $\log\log$, không phải $\log$.

### Chiến lược

1. Xác định **phép toán cơ bản**.
2. Viết **tổng** cho vòng lồng phụ thuộc.
3. Dùng **công thức có sẵn** hoặc **tích phân** cho tổng phức tạp.
4. Đơn giản bằng **$\Theta$** (chặt nhất có thể).
5. **Kiểm tra lại** bằng cách thử $n$ nhỏ ($n=1, 2, 3$).

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Phân tích chi tiết** một thuật toán cụ thể từng vòng lặp (Merge Sort, Quick Sort, Dijkstra).
- **Bài tập nâng cao** với tổng phức tạp (ví dụ $\sum_{i=1}^{n} i\log i$, $\sum_{i=1}^{n} \lfloor n/i \rfloor$).
- **Phân tích amortized** cho vòng lặp có chi phí biến đổi (dynamic array, union-find).
- **Phân tích average case** cho vòng lặp có điều kiện dừng ngẫu nhiên.