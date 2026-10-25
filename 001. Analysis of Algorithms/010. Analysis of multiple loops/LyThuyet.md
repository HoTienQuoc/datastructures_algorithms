# Phân tích nhiều vòng lặp (Analysis of Multiple Loops) — Lý thuyết + Ví dụ

## 1. Đặt vấn đề

Trong thực tế, code hiếm khi chỉ có **một** vòng lặp. Một hàm thường có:

- **Nhiều vòng tuần tự** (sequential loops)
- **Nhiều vòng lồng nhau** (nested loops) — có thể nhiều tầng
- **Hỗn hợp**: vòng lặp trong điều kiện, trong đệ quy, gọi hàm có vòng lặp
- **Vòng lặp có chi phí biến đổi** — cần amortized analysis

**Mục tiêu:** Xác định độ phức tạp tổng của **toàn bộ** code, không chỉ từng vòng riêng lẻ.

### Ba quy tắc nền tảng

| Cấu trúc                | Quy tắc            | Công thức                                |
| ----------------------- | ------------------ | ---------------------------------------- |
| **Tuần tự**             | **Cộng** (lấy max) | $T_1 + T_2 = \Theta(\max(T_1, T_2))$     |
| **Lồng nhau độc lập**   | **Nhân**           | $T_1 \times T_2 = \Theta(T_1 \cdot T_2)$ |
| **Lồng nhau phụ thuộc** | **Viết tổng**      | $T = \sum_{i} T_{\text{inner}}(i)$       |

**Điểm mấu chốt:** Quy tắc cộng chỉ đúng khi **số đoạn là hằng số**. Khi số đoạn phụ thuộc $n$, phải **nhân**.

> **Lý thuyết bên cạnh:** Phân tích nhiều vòng lặp thực chất là **kết hợp** ba quy tắc trên theo cấu trúc code. Khó khăn không nằm ở quy tắc — mà ở việc **nhận diện cấu trúc** và **xử lý trường hợp phụ thuộc**.

---

## 2. Nhiều vòng tuần tự (Sequential Loops)

### 2.1. Quy tắc cộng — khi số đoạn cố định

Khi có $k$ đoạn code nối tiếp ($k$ là **hằng số**, không phụ thuộc $n$):

$$
T(n) = T_1(n) + T_2(n) + \cdots + T_k(n) = \Theta(\max_i T_i(n))
$$

**Lý do:** Số hạng lớn nhất áp đảo khi $n \to \infty$.

### 2.2. Ví dụ 1 — ba đoạn tuần tự

```text
Function F(n):
    for i = 1 to n:              // Đoạn 1: Θ(n)
        doing O(1)
    for i = 1 to n:              // Đoạn 2: Θ(n²)
        for j = 1 to n:
            doing O(1)
    for i = 1 to n:              // Đoạn 3: Θ(n)
        doing O(1)
```

**Phân tích:**

$$
T(n) = \Theta(n) + \Theta(n^2) + \Theta(n) = \Theta(n^2)
$$

**Giải thích:** Đoạn 2 (bậc hai) áp đảo hai đoạn còn lại (tuyến tính) khi $n$ lớn.

### 2.3. Ví dụ 2 — đoạn có logarit

```text
Function G(n):
    i = 1
    while i < n:                 // Đoạn 1: Θ(log n)
        doing O(1)
        i = i * 2
    for i = 1 to n:              // Đoạn 2: Θ(n)
        doing O(1)
```

**Phân tích:**

$$
T(n) = \Theta(\log n) + \Theta(n) = \Theta(n)
$$

**Giải thích:** Tuyến tính áp đảo logarit.

### 2.4. Ví dụ 3 — nhiều đoạn cùng bậc

```text
Function H(n):
    for i = 1 to n:              // Θ(n)
        doing O(1)
    for i = 1 to n:              // Θ(n)
        doing O(1)
    for i = 1 to n:              // Θ(n)
        doing O(1)
```

**Phân tích:**

$$
T(n) = 3\Theta(n) = \Theta(n)
$$

**Giải thích:** Hằng số 3 bị bỏ. Ba đoạn cùng bậc $\Theta(n)$ vẫn là $\Theta(n)$ — **không** thành $\Theta(3n)$ riêng biệt.

### 2.5. Ví dụ 4 — bậc khác nhau rõ rệt

```text
Function K(n):
    for i = 1 to n:              // Θ(n)
        doing O(1)
    for i = 1 to n:              // Θ(n²)
        for j = 1 to n:
            doing O(1)
    for i = 1 to n:              // Θ(n³)
        for j = 1 to n:
            for k = 1 to n:
                doing O(1)
```

**Phân tích:**

$$
T(n) = \Theta(n) + \Theta(n^2) + \Theta(n^3) = \Theta(n^3)
$$

**Giải thích:** Bậc cao nhất áp đảo hoàn toàn.

### 2.6. Bẫy — số đoạn phụ thuộc $n$

```text
for k = 1 to n:                  // n đoạn — KHÔNG phải hằng số!
    for i = 1 to n:
        doing O(1)
```

**Sai nếu nói** $\Theta(n)$ (lấy max của $n$ đoạn $\Theta(n)$).

**Đúng:** Có $n$ đoạn, mỗi đoạn $\Theta(n)$:

$$
T(n) = \sum_{k=1}^{n} \Theta(n) = n \cdot \Theta(n) = \Theta(n^2)
$$

**Đây thực chất là vòng lồng nhau** — không phải tuần tự. Quy tắc cộng **chỉ áp dụng khi số đoạn cố định**.

### 2.7. Bảng tóm tắt — sequential loops

| Số đoạn         | Bậc mỗi đoạn                                            | Tổng          |
| --------------- | ------------------------------------------------------- | ------------- |
| 3 (cố định)     | $\Theta(n), \Theta(n^2), \Theta(n)$                     | $\Theta(n^2)$ |
| 5 (cố định)     | $\Theta(n), \Theta(n), \Theta(n), \Theta(n), \Theta(n)$ | $\Theta(n)$   |
| $n$ (phụ thuộc) | $\Theta(n)$ mỗi đoạn                                    | $\Theta(n^2)$ |
| $n$ (phụ thuộc) | $\Theta(1)$ mỗi đoạn                                    | $\Theta(n)$   |

> **Lý thuyết bên cạnh:** Khi đọc code, **luôn hỏi**: "Số đoạn có phụ thuộc $n$ không?" Nếu có, đó thực chất là vòng lồng (dù viết tuần tự). Đây là bẫy phổ biến nhất trong phân tích multiple loops.

---

## 3. Nhiều vòng lồng nhau — độc lập (Independent Nested)

### 3.1. Quy tắc nhân

Khi các vòng lồng **độc lập** (vòng trong không phụ thuộc biến vòng ngoài):

$$
T(n) = T_1(n) \times T_2(n) \times \cdots \times T_k(n)
$$

### 3.2. Ví dụ 5 — hai vòng độc lập

```text
for i = 1 to n:                  // n lần
    for j = 1 to n:              // n lần — độc lập với i
        doing O(1)
```

$$
T(n) = n \cdot n = \Theta(n^2)
$$

### 3.3. Ví dụ 6 — ba vòng độc lập với giới hạn khác nhau

```text
for i = 1 to n:                  // n lần
    for j = 1 to m:              // m lần — độc lập
        for k = 1 to p:          // p lần — độc lập
            doing O(1)
```

$$
T(n, m, p) = n \cdot m \cdot p = \Theta(nmp)
$$

**Lưu ý:** Với nhiều biến, giữ **tất cả** biến — không gộp thành $n^3$ trừ khi $n = m = p$.

### 3.4. Ví dụ 7 — vòng trong logarit

```text
for i = 1 to n:                  // n lần
    j = 1
    while j < n:                 // log n lần
        doing O(1)
        j = j * 2
```

$$
T(n) = n \cdot \log n = \Theta(n\log n)
$$

### 3.5. Ví dụ 8 — vòng ngoài logarit, vòng trong tuyến tính

```text
i = 1
while i < n:                     // log n lần
    for j = 1 to n:              // n lần
        doing O(1)
    i = i * 2
```

$$
T(n) = \log n \cdot n = \Theta(n\log n)
$$

**Quan trọng:** Thứ tự vòng (log ngoài, n trong) hay (n ngoài, log trong) đều cho $\Theta(n\log n)$ — vì phép nhân có tính giao hoán.

### 3.6. Ví dụ 9 — hai vòng logarit lồng nhau

```text
i = 1
while i < n:                     // log n lần
    j = 1
    while j < n:                 // log n lần
        doing O(1)
        j = j * 2
    i = i * 2
```

$$
T(n) = \log n \cdot \log n = \Theta(\log^2 n)
$$

### 3.7. Bảng tóm tắt — independent nested

| Vòng ngoài       | Vòng trong         | Tổng               |
| ---------------- | ------------------ | ------------------ |
| $\Theta(n)$      | $\Theta(n)$        | $\Theta(n^2)$      |
| $\Theta(n)$      | $\Theta(\log n)$   | $\Theta(n\log n)$  |
| $\Theta(\log n)$ | $\Theta(n)$        | $\Theta(n\log n)$  |
| $\Theta(\log n)$ | $\Theta(\log n)$   | $\Theta(\log^2 n)$ |
| $\Theta(n)$      | $\Theta(\sqrt{n})$ | $\Theta(n^{3/2})$  |
| $\Theta(n)$      | $\Theta(1)$        | $\Theta(n)$        |

> **Lý thuyết bên cạnh:** Vòng lồng độc lập **dễ nhất** — chỉ cần nhân. Phức tạp chỉ xuất hiện khi vòng trong **phụ thuộc** vòng ngoài (mục 4). Khi đọc code, kiểm tra: "Biến vòng trong có dùng biến vòng ngoài không?" Nếu không → nhân. Nếu có → viết tổng.

---

## 4. Nhiều vòng lồng nhau — phụ thuộc (Dependent Nested)

### 4.1. Quy tắc — viết tổng

Khi vòng trong phụ thuộc vòng ngoài:

$$
T(n) = \sum_{i} T_{\text{inner}}(i)
$$

Không thể nhân trực tiếp — phải **tính tổng**.

### 4.2. Ví dụ 10 — tam giác cơ bản

```text
for i = 1 to n:                  // n lần
    for j = 1 to i:              // i lần — phụ thuộc i
        doing O(1)
```

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \Theta(n^2)
$$

**So sánh với nhân trực tiếp:** $n \cdot n = n^2$ — cùng bậc nhưng **hằng số khác** ($1/2$ vs $1$). Nếu chỉ cần $\Theta$, kết quả giống nhau. Nếu cần chính xác, phải tính tổng.

### 4.3. Ví dụ 11 — vòng trong từ $i$ đến $n$

```text
for i = 1 to n:
    for j = i to n:              // n - i + 1 lần
        doing O(1)
```

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} (n - i + 1) = n + (n-1) + \cdots + 1 = \frac{n(n+1)}{2} = \Theta(n^2)
$$

### 4.4. Ví dụ 12 — vòng trong chạy $i^2$ lần

```text
for i = 1 to n:
    for j = 1 to i*i:            // i² lần
        doing O(1)
```

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} i^2 = \frac{n(n+1)(2n+1)}{6} = \Theta(n^3)
$$

**Khác biệt:** Vòng trong chạy $i^2$ (không phải $i$) — tổng thành bậc ba, không phải bậc hai.

### 4.5. Ví dụ 13 — vòng trong chạy $\sqrt{i}$ lần

```text
for i = 1 to n:
    for j = 1 to sqrt(i):        // √i lần
        doing O(1)
```

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} \sqrt{i} \approx \int_0^n \sqrt{x}\,dx = \frac{2}{3}n^{3/2} = \Theta(n^{3/2})
$$

**Kỹ thuật tích phân:** Khi tổng không có công thức đóng, dùng tích phân để xấp xỉ.

### 4.6. Ví dụ 14 — vòng trong chạy $n/i$ lần

```text
for i = 1 to n:
    for j = 1 to n/i:            // n/i lần
        doing O(1)
```

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} \frac{n}{i} = n \sum_{i=1}^{n} \frac{1}{i} = n H_n = \Theta(n\log n)
$$

**Với** $H_n = \Theta(\log n)$ là số harmonic.

**Ứng dụng:** Đây là mẫu của **sàng Eratosthenes** (sàng số nguyên tố) — tổng $n/2 + n/3 + n/5 + \cdots$ xấp xỉ $n\log\log n$.

### 4.7. Ví dụ 15 — vòng trong chạy $\log i$ lần

```text
for i = 1 to n:
    j = 1
    while j < i:                 // log i lần
        doing O(1)
        j = j * 2
```

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} \log i = \log(n!) = \Theta(n\log n)
$$

**Dùng xấp xỉ Stirling.**

### 4.8. Ví dụ 16 — ba vòng phụ thuộc

```text
for i = 1 to n:
    for j = 1 to i:
        for k = 1 to j:
            doing O(1)
```

**Tổng:**

$$
T(n) = \sum_{i=1}^{n} \sum_{j=1}^{i} j = \sum_{i=1}^{n} \frac{i(i+1)}{2} = \Theta(n^3)
$$

**Giải thích:** Cả ba tầng đều phụ thuộc — tổng cuối là bậc ba.

### 4.9. Ví dụ 17 — vòng trong phụ thuộc cả hai biến

```text
for i = 1 to n:
    for j = i to n:
        for k = i to j:          // phụ thuộc cả i và j
            doing O(1)
```

**Tổng:** Với mỗi $i$, đặt $m = n - i + 1$. Khi $j$ từ $i$ đến $n$, $k$ chạy $j - i + 1$ lần:

$$
\sum_{j=i}^{n} (j - i + 1) = \frac{m(m+1)}{2}
$$

Vậy:

$$
T(n) = \sum_{i=1}^{n} \frac{(n-i+1)(n-i+2)}{2} = \sum_{m=1}^{n} \frac{m(m+1)}{2} = \Theta(n^3)
$$

### 4.10. Bảng tóm tắt — dependent nested

| Vòng trong chạy | Tổng            | Big-$\Theta$      |
| --------------- | --------------- | ----------------- |
| $1..i$          | $\sum i$        | $\Theta(n^2)$     |
| $i..n$          | $\sum (n-i+1)$  | $\Theta(n^2)$     |
| $1..i^2$        | $\sum i^2$      | $\Theta(n^3)$     |
| $1..\sqrt{i}$   | $\sum \sqrt{i}$ | $\Theta(n^{3/2})$ |
| $1..n/i$        | $nH_n$          | $\Theta(n\log n)$ |
| $1..\log i$     | $\log(n!)$      | $\Theta(n\log n)$ |
| $1..2^i$        | $\sum 2^i$      | $\Theta(2^n)$     |

> **Lý thuyết bên cạnh:** Vòng lồng phụ thuộc là **phần khó nhất** của phân tích multiple loops. Chiến lược: (1) viết tổng chính xác, (2) dùng công thức có sẵn hoặc tích phân, (3) đơn giản bằng $\Theta$. Đừng đoán — **luôn viết tổng** để tránh sai.

---

## 5. Hỗn hợp — vòng lặp trong điều kiện

### 5.1. Vòng lặp trong `if-else`

```text
Function F(n):
    if n > 100:
        for i = 1 to n:
            for j = 1 to n:
                doing O(1)       // Θ(n²) nếu nhánh này chạy
    else:
        for i = 1 to n:
            doing O(1)           // Θ(n) nếu nhánh này chạy
```

**Phân tích:**

- **Worst case** ($n > 100$): $\Theta(n^2)$.
- **Best case** ($n \le 100$): $\Theta(n)$ — nhưng với $n \le 100$, $\Theta(n)$ bị chặn bởi hằng số ⇒ $\Theta(1)$.

**Kết luận:** $T(n) = O(n^2)$ và $T(n) = \Omega(n)$ (nếu $n$ có thể lớn tùy ý).

**Chiến lược:** Phân tích **từng nhánh**, rồi lấy **max** (worst) hoặc **min** (best).

### 5.2. Ví dụ 18 — nhánh phụ thuộc tham số

```text
Function G(n, flag):
    if flag:
        for i = 1 to n:
            for j = 1 to n:
                doing O(1)       // Θ(n²)
    else:
        for i = 1 to n:
            doing O(1)           // Θ(n)
```

**Phân tích:**

- Nếu `flag = true`: $\Theta(n^2)$.
- Nếu `flag = false`: $\Theta(n)$.

**Kết luận:** Không có $\Theta$ duy nhất cho mọi input — phụ thuộc `flag`. Phải nói rõ điều kiện.

### 5.3. Ví dụ 19 — điều kiện bên trong vòng lặp

```text
for i = 1 to n:
    if A[i] > 0:
        for j = 1 to n:
            doing O(1)           // Θ(n) — chỉ chạy khi điều kiện đúng
```

**Phân tích:**

- **Best case** (mọi $A[i] \le 0$): chỉ vòng ngoài $\Theta(n)$.
- **Worst case** (mọi $A[i] > 0$): $\Theta(n^2)$.
- **Average case:** Nếu $P(A[i] > 0) = p$, kỳ vọng số lần vòng trong chạy: $p \cdot n$. Tổng: $n \cdot (1 + p \cdot n) = \Theta(n + pn^2)$.

### 5.4. Ví dụ 20 — vòng lặp có `break`

```text
for i = 1 to n:
    for j = 1 to n:
        if A[i][j] == x:
            break                // thoát vòng trong
```

**Phân tích:**

- **Best case:** $A[1][1] = x$ ⇒ $\Theta(1)$.
- **Worst case:** $x$ không có ⇒ $\Theta(n^2)$.
- **Average case:** Nếu $x$ ngẫu nhiên, trung bình duyệt $n/2$ mỗi vòng ⇒ $\Theta(n^2/4) = \Theta(n^2)$.

### 5.5. Ví dụ 21 — `break` thoát cả hai vòng

```text
found = false
for i = 1 to n:
    for j = 1 to n:
        if A[i][j] == x:
            found = true
            break
    if found: break
```

**Phân tích:**

- **Best case:** $A[1][1] = x$ ⇒ $\Theta(1)$.
- **Worst case:** $x$ không có ⇒ $\Theta(n^2)$.
- **Average case:** Nếu $x$ ngẫu nhiên và có mặt, vị trí trung bình là $n^2/2$ ⇒ $\Theta(n^2)$.

> **Lý thuyết bên cạnh:** `break` làm phức tạp phân tích — vì số lần lặp **phụ thuộc dữ liệu**. Chiến lược: phân tích **best case** (break sớm nhất) và **worst case** (break muộn nhất hoặc không break). Average case cần giả định phân bố.

---

## 6. Hỗn hợp — vòng lặp gọi hàm

### 6.1. Vòng lặp gọi hàm có chi phí $O(1)$

```text
Function helper(x):
    return x * x                 // Θ(1)

Function F(n):
    for i = 1 to n:
        doing helper(i)          // Θ(1) mỗi lần
```

$$
T(n) = n \cdot \Theta(1) = \Theta(n)
$$

### 6.2. Vòng lặp gọi hàm có chi phí $\Theta(n)$

```text
Function helper(x):
    for j = 1 to x:
        doing O(1)               // Θ(x)
    return

Function F(n):
    for i = 1 to n:
        doing helper(i)          // Θ(i) mỗi lần
```

**Phân tích:** Đây thực chất là vòng lồng phụ thuộc:

$$
T(n) = \sum_{i=1}^{n} \Theta(i) = \Theta(n^2)
$$

**Bài học:** Hàm được gọi **ẩn** vòng lặp bên trong. Phải tính chi phí **mỗi lần gọi**, rồi cộng/nhân.

### 6.3. Ví dụ 22 — hàm có chi phí $\log n$

```text
Function helper(x):
    j = 1
    while j < x:
        doing O(1)
        j = j * 2                // Θ(log x)

Function F(n):
    for i = 1 to n:
        doing helper(i)
```

**Phân tích:**

$$
T(n) = \sum_{i=1}^{n} \log i = \log(n!) = \Theta(n\log n)
$$

### 6.4. Ví dụ 23 — vòng lặp gọi hàm đệ quy

```text
Function fib(k):
    if k <= 1: return k
    return fib(k-1) + fib(k-2)   // Θ(φ^k)

Function F(n):
    for i = 1 to n:
        doing fib(i)
```

**Phân tích:**

$$
T(n) = \sum_{i=1}^{n} \Theta(\varphi^i) = \Theta(\varphi^n)
$$

Vì tổng cấp số nhân bị chi phối bởi số hạng cuối.

**Bài học:** Khi vòng lặp gọi hàm đệ quy, phải biết chi phí của hàm đó trước, rồi áp dụng quy tắc.

### 6.5. Ví dụ 24 — hàm lồng nhiều tầng

```text
Function inner(n):
    for i = 1 to n:
        doing O(1)               // Θ(n)

Function middle(n):
    for i = 1 to n:
        doing inner(n)           // Θ(n) mỗi lần

Function outer(n):
    for i = 1 to n:
        doing middle(n)          // Θ(n²) mỗi lần
```

**Phân tích từ trong ra:**

- `inner`: $\Theta(n)$.
- `middle`: $n \cdot \Theta(n) = \Theta(n^2)$.
- `outer`: $n \cdot \Theta(n^2) = \Theta(n^3)$.

**Bài học:** Phân tích **từ trong ra ngoài** — mỗi hàm biết chi phí của hàm con.

> **Lý thuyết bên cạnh:** Khi code gọi hàm, phải **mở rộng** hàm đó ra (inline) hoặc **dùng chi phí đã biết**. Không được bỏ qua chi phí ẩn bên trong hàm — đây là lỗi phổ biến khi phân tích code thực tế (ví dụ gọi `sort()` trong vòng lặp).

---

## 7. Hỗn hợp — vòng lặp trong đệ quy

### 7.1. Mẫu cơ bản — đệ quy có vòng lặp

```text
Function F(n):
    if n <= 1: return
    for i = 1 to n:
        doing O(1)               // Θ(n) mỗi lần gọi
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

**Đây là Selection Sort / Bubble Sort đệ quy.**

### 7.2. Ví dụ 25 — đệ quy chia đôi có vòng lặp tuyến tính

```text
Function F(n):
    if n <= 1: return
    for i = 1 to n:
        doing O(1)               // Θ(n)
    F(n/2)
    F(n/2)
```

**Hệ thức:**

$$
T(n) = 2T(n/2) + \Theta(n)
$$

**Master Theorem Case 2:** $T(n) = \Theta(n\log n)$.

**Đây là Merge Sort (nếu thêm bước trộn).**

### 7.3. Ví dụ 26 — đệ quy có vòng lặp logarit

```text
Function F(n):
    if n <= 1: return
    i = 1
    while i < n:                 // Θ(log n)
        doing O(1)
        i = i * 2
    F(n/2)
```

**Hệ thức:**

$$
T(n) = T(n/2) + \Theta(\log n)
$$

**Giải:** Mở rộng:

$$
T(n) = \Theta(\log n) + \Theta(\log(n/2)) + \Theta(\log(n/4)) + \cdots
$$

$$
= \Theta(\log n + (\log n - 1) + (\log n - 2) + \cdots + 1) = \Theta(\log^2 n)
$$

### 7.4. Ví dụ 27 — đệ quy có hai vòng lặp tuần tự

```text
Function F(n):
    if n <= 1: return
    for i = 1 to n:
        doing O(1)               // Θ(n)
    for i = 1 to n:
        for j = 1 to n:
            doing O(1)           // Θ(n²)
    F(n/2)
    F(n/2)
```

**Hệ thức:**

$$
T(n) = 2T(n/2) + \Theta(n) + \Theta(n^2) = 2T(n/2) + \Theta(n^2)
$$

**Master Theorem Case 3:** $a=2$, $b=2$, $x=n^{\log_2 2}=n$, $f(n)=n^2=\Omega(n^{1+\varepsilon})$:

$$
T(n) = \Theta(n^2)
$$

### 7.5. Ví dụ 28 — đệ quy có vòng lặp lồng phụ thuộc

```text
Function F(n):
    if n <= 1: return
    for i = 1 to n:
        for j = 1 to i:
            doing O(1)           // Θ(n²)
    F(n-1)
```

**Hệ thức:**

$$
T(n) = T(n-1) + \Theta(n^2)
$$

**Giải:**

$$
T(n) = \sum_{k=1}^{n} \Theta(k^2) = \Theta(n^3)
$$

### 7.6. Bảng tóm tắt — recursion + loops

| Hệ thức                          | Big-$\Theta$       | Ví dụ              |
| -------------------------------- | ------------------ | ------------------ |
| $T(n) = T(n-1) + \Theta(n)$      | $\Theta(n^2)$      | Selection Sort     |
| $T(n) = T(n-1) + \Theta(n^2)$    | $\Theta(n^3)$      | Đệ quy + vòng lồng |
| $T(n) = 2T(n/2) + \Theta(n)$     | $\Theta(n\log n)$  | Merge Sort         |
| $T(n) = 2T(n/2) + \Theta(n^2)$   | $\Theta(n^2)$      | Đệ quy + vòng lồng |
| $T(n) = T(n/2) + \Theta(\log n)$ | $\Theta(\log^2 n)$ | Đệ quy + vòng log  |
| $T(n) = T(n-1) + \Theta(1)$      | $\Theta(n)$        | Đệ quy tuyến tính  |

> **Lý thuyết bên cạnh:** Khi đệ quy **chứa vòng lặp**, chi phí mỗi lần gọi là chi phí vòng lặp. Hệ thức truy hồi có dạng $T(n) = aT(n/b) + f(n)$ với $f(n)$ = chi phí vòng lặp. Áp dụng Master Theorem (nếu chia đều) hoặc cây đệ quy (nếu trừ).

---

## 8. Amortized analysis cho nhiều vòng lặp

### 8.1. Khi nào cần amortized?

Khi một số thao tác **đắt** nhưng **hiếm**, và trung bình trên chuỗi thao tác lại **rẻ**.

### 8.2. Ví dụ 29 — Dynamic Array (vector)

```text
Function push(x):
    if size == capacity:
        new_capacity = 2 * capacity
        new_array = allocate(new_capacity)
        copy all elements                  // Θ(size) — đắt
        capacity = new_capacity
    array[size] = x
    size = size + 1
```

**Phân tích từng thao tác:**

- Thông thường: $\Theta(1)$.
- Khi đầy: $\Theta(n)$ (copy toàn bộ).

**Amortized:** Xét $n$ lần push:

- Các lần copy xảy ra khi capacity là $1, 2, 4, 8, \ldots$
- Tổng chi phí copy: $1 + 2 + 4 + \cdots + 2^k < 2n$
- Tổng chi phí $n$ lần push: $\Theta(n)$

**Amortized cost mỗi push:** $\Theta(n)/n = \Theta(1)$.

### 8.3. Ví dụ 30 — vòng lặp với chi phí biến đổi

```text
i = 1
total = 0
while i <= n:
    j = 1
    while j <= i:
        doing O(1)              // i lần
        j = j + 1
    i = i + 1
```

**Phân tích:**

$$
T(n) = \sum_{i=1}^{n} i = \Theta(n^2)
$$

Đây không phải amortized — chỉ là vòng lồng phụ thuộc thông thường.

### 8.4. Ví dụ 31 — Union-Find với path compression

```text
Function find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])   // path compression
    return parent[x]
```

**Phân tích:** Mỗi lần `find` có thể đắt ($O(\log n)$ worst), nhưng **amortized** trên chuỗi thao tác là $O(\alpha(n))$ — gần như hằng số.

**Đây là ví dụ kinh điển của amortized analysis** — kết quả không thể suy ra từ phân tích từng thao tác riêng lẻ.

### 8.5. Bảng tóm tắt — amortized

| Cấu trúc                 | Worst case 1 thao tác | Amortized           |
| ------------------------ | --------------------- | ------------------- |
| Dynamic array push       | $\Theta(n)$           | $\Theta(1)$         |
| Hash table insert        | $\Theta(n)$           | $\Theta(1)$         |
| Union-Find find          | $\Theta(\log n)$      | $\Theta(\alpha(n))$ |
| Binary counter increment | $\Theta(\log n)$      | $\Theta(1)$         |

> **Lý thuyết bên cạnh:** Amortized analysis khác **average case** — nó **không cần giả định xác suất**. Nó nói: "Trên **mọi** chuỗi $n$ thao tác, tổng chi phí là $O(n)$" — đây là **bảo đảm worst case** cho chuỗi. Ba phương pháp: aggregate, accounting, potential.

---

## 9. Ví dụ tổng hợp — phân tích từng bước

### Ví dụ 32 — code phức tạp

```text
Function Complex(A, n):
    // Đoạn 1: sắp xếp (giả sử dùng Merge Sort)
    sort(A)                                    // Θ(n log n)

    // Đoạn 2: vòng lặp đơn
    for i = 1 to n:
        doing O(1)                             // Θ(n)

    // Đoạn 3: vòng lồng phụ thuộc
    for i = 1 to n:
        for j = 1 to i:
            doing O(1)                         // Θ(n²)

    // Đoạn 4: vòng logarit
    i = 1
    while i < n:
        doing O(1)
        i = i * 3                              // Θ(log n)

    // Đoạn 5: điều kiện
    if A[1] > 0:
        for i = 1 to n:
            for j = 1 to n:
                for k = 1 to n:
                    doing O(1)                 // Θ(n³) — worst case
```

**Phân tích từng đoạn:**

- Đoạn 1: $\Theta(n\log n)$
- Đoạn 2: $\Theta(n)$
- Đoạn 3: $\Theta(n^2)$
- Đoạn 4: $\Theta(\log n)$
- Đoạn 5: $\Theta(n^3)$ worst, $\Theta(1)$ best

**Tổng (worst case):**

$$
T(n) = \Theta(n\log n) + \Theta(n) + \Theta(n^2) + \Theta(\log n) + \Theta(n^3) = \Theta(n^3)
$$

**Tổng (best case):**

$$
T(n) = \Theta(n\log n) + \Theta(n) + \Theta(n^2) + \Theta(\log n) + \Theta(1) = \Theta(n^2)
$$

**Kết luận:** $T(n) = O(n^3)$ và $T(n) = \Omega(n^2)$.

### Ví dụ 33 — vòng lặp lồng với hàm

```text
Function Process(A, n):
    for i = 1 to n:
        for j = 1 to n:
            doing binarySearch(A, n, A[i] + A[j])
```

**Phân tích:**

- Binary Search: $\Theta(\log n)$.
- Vòng $i, j$: $n^2$ cặp.
- Tổng: $n^2 \cdot \Theta(\log n) = \Theta(n^2\log n)$.

### Ví dụ 34 — vòng lặp với điều kiện phức tạp

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n:
            if i + j + k == n:
                for l = 1 to n:
                    doing O(1)
```

**Phân tích:**

- Số lần kiểm tra `if`: $n^3$.
- Số lần thân `if` chạy: số bộ $(i,j,k)$ với $i+j+k = n$. Với mỗi $i, j$, có **đúng một** $k$ (nếu $n - i - j$ trong $[1, n]$). Số bộ: $\Theta(n^2)$.
- Mỗi lần thân `if` chạy: $\Theta(n)$.

**Tổng:**

$$
T(n) = \Theta(n^3) + \Theta(n^2) \cdot \Theta(n) = \Theta(n^3) + \Theta(n^3) = \Theta(n^3)
$$

### Ví dụ 35 — vòng lặp trong vòng lặp gọi đệ quy

```text
Function F(n):
    if n <= 1: return 1
    result = 0
    for i = 1 to n:
        result += F(n/2)         // gọi đệ quy n lần
    return result
```

**Hệ thức:**

$$
T(n) = n \cdot T(n/2) + \Theta(n)
$$

**Giải:** Đây không phải Master Theorem dạng chuẩn (vì $n$ phụ thuộc $n$). Mở rộng:

$$
T(n) = n \cdot T(n/2) + n = n\left(\frac{n}{2} T(n/4) + \frac{n}{2}\right) + n = \frac{n^2}{2} T(n/4) + \frac{n^2}{2} + n
$$

Tiếp tục:

$$
T(n) = \frac{n^2}{2} \cdot \frac{n}{4} T(n/8) + \cdots = \frac{n^k}{2^{1+2+\cdots+(k-1)}} T(n/2^k) + \cdots
$$

Với $k = \log_2 n$: $T(1) = 1$, và hệ số $\frac{n^{\log n}}{2^{\log^2 n / 2}}$ — dẫn đến:

$$
T(n) = \Theta(n^{\log n})
$$

**Đây là trường hợp cực hiếm** — không thuộc Master Theorem chuẩn.

> **Lý thuyết bên cạnh:** Hệ thức $T(n) = n T(n/2) + f(n)$ không thuộc Master Theorem chuẩn (vì $a = n$ phụ thuộc $n$). Cần dùng phương pháp thế hoặc cây đệ quy. Kết quả thường là siêu đa thức — rất chậm.

---

## 10. Những bẫy phổ biến

### Bẫy 1 — "Cộng các đoạn mà quên số đoạn phụ thuộc $n$"

```text
for k = 1 to n:                  // n đoạn
    for i = 1 to n:
        doing O(1)
```

**Sai:** $\Theta(n)$ (lấy max).
**Đúng:** $n \cdot \Theta(n) = \Theta(n^2)$.

### Bẫy 2 — "Nhân vòng lồng mà vòng trong phụ thuộc"

```text
for i = 1 to n:
    for j = 1 to i:              // không phải n lần!
        doing O(1)
```

**Không chính xác:** $n \cdot n = n^2$.
**Chính xác:** $n(n+1)/2$ — cùng $\Theta$ nhưng hằng số $1/2$.

### Bẫy 3 — "Bỏ qua chi phí hàm được gọi"

```text
for i = 1 to n:
    sort(A)                      // Θ(n log n) mỗi lần!
```

**Sai:** $\Theta(n)$ (chỉ đếm vòng lặp).
**Đúng:** $n \cdot \Theta(n\log n) = \Theta(n^2\log n)$.

### Bẫy 4 — "Bỏ qua chi phí kiểm tra điều kiện"

```text
for i = 1 to n:
    for j = 1 to n:
        if hiếm_khi_đúng:
            doing O(1)
```

**Sai:** $\Theta(1)$ (chỉ đếm thân `if`).
**Đúng:** $\Theta(n^2)$ (kiểm tra $n^2$ lần).

### Bẫy 5 — "`break` không ảnh hưởng"

**Sai.** `break` làm giảm số lần lặp — phải phân tích best/worst case.

### Bẫy 6 — "Đệ quy có vòng lặp là nhân"

```text
Function F(n):
    for i = 1 to n: doing O(1)
    F(n-1)
```

**Sai:** $n \cdot T(n-1)$.
**Đúng:** $T(n) = T(n-1) + \Theta(n)$ ⇒ $\Theta(n^2)$.

**Lý do:** Đệ quy **không** gọi song song — nó gọi **tuần tự** (sau vòng lặp). Vậy cộng, không nhân.

### Bẫy 7 — "Nhân khi đệ quy có hai nhánh"

```text
Function F(n):
    for i = 1 to n: doing O(1)
    F(n/2)
    F(n/2)
```

**Đúng:** $T(n) = 2T(n/2) + \Theta(n)$ ⇒ $\Theta(n\log n)$.

**Lý do:** Hai nhánh đệ quy **cộng** (không nhân), nhưng mỗi nhánh có $T(n/2)$ ⇒ $2T(n/2)$.

### Bẫy 8 — "Vòng lặp lồng luôn $n^k$"

**Sai khi vòng trong phụ thuộc:**

```text
for i = 1 to n:
    for j = 1 to n/i:
        doing O(1)
```

**Không phải** $n^2$ — mà là $nH_n = \Theta(n\log n)$.

### Bẫy 9 — "Ba vòng lồng luôn $n^3$"

**Sai.** Phụ thuộc giới hạn:

```text
for i = 1 to n:
    for j = 1 to i:
        for k = 1 to j:
            doing O(1)
```

**Đúng:** $\Theta(n^3)$ — nhưng nếu vòng trong là $\log j$ thì $\Theta(n^2\log n)$.

### Bẫy 10 — "Bỏ qua chi phí khởi tạo"

```text
for i = 1 to n:
    A = new Array(i)             // Θ(i) để cấp phát
    for j = 1 to i:
        A[j] = 0
```

**Đúng:** $\sum_{i=1}^{n} (\Theta(i) + \Theta(i)) = \Theta(n^2)$ — cả cấp phát và khởi tạo đều tính.

> **Lý thuyết bên cạnh:** Hầu hết bẫy đến từ việc **áp dụng quy tắc máy móc** mà không đọc kỹ code. Luôn tự hỏi: "Vòng trong có phụ thuộc vòng ngoài không?", "Có hàm nào được gọi không?", "Số đoạn có phụ thuộc $n$ không?", "Có `break`/`return` không?".

---

## 11. Chiến lược phân tích tổng quát

### 11.1. Quy trình 6 bước

1. **Xác định phép toán cơ bản** (so sánh, gán, gọi hàm…).
2. **Phân tích từ trong ra ngoài** — bắt đầu từ vòng lặp sâu nhất.
3. **Với mỗi vòng lặp:**
   - Xác định số lần lặp.
   - Nếu phụ thuộc biến ngoài → viết tổng.
4. **Với nhiều đoạn tuần tự:** cộng (lấy max) — nếu số đoạn cố định.
5. **Với vòng lồng:** nhân (nếu độc lập) hoặc tổng (nếu phụ thuộc).
6. **Đơn giản bằng $\Theta$** — bỏ hằng số và số hạng bậc thấp.

### 11.2. Bảng quyết định

| Cấu trúc                                  | Quy tắc          | Công thức                    |
| ----------------------------------------- | ---------------- | ---------------------------- |
| Nhiều đoạn tuần tự, số đoạn cố định       | Cộng, lấy max    | $\Theta(\max T_i)$           |
| Nhiều đoạn tuần tự, số đoạn phụ thuộc $n$ | Nhân             | $n \cdot \Theta(f)$          |
| Vòng lồng độc lập                         | Nhân             | $\Theta(f \cdot g)$          |
| Vòng lồng phụ thuộc                       | Viết tổng        | $\sum T_{\text{inner}}$      |
| Vòng lặp gọi hàm                          | Nhân chi phí hàm | $n \cdot \Theta(f)$          |
| Đệ quy + vòng lặp                         | Hệ thức truy hồi | $T(n) = T(n-1) + \Theta(f)$  |
| Đệ quy hai nhánh + vòng lặp               | Hệ thức          | $T(n) = 2T(n/2) + \Theta(f)$ |
| Chi phí biến đổi                          | Amortized        | Phân tích chuỗi              |

### 11.3. Mẹo kiểm tra

- **Thử $n$ nhỏ:** $n=1, 2, 3$ — xem công thức có khớp không.
- **Kiểm tra bậc:** Nếu kết quả là $\Theta(n^2)$, thử $n \to 2n$ — thời gian có tăng $\approx 4$ lần không?
- **So sánh với trực giác:** Vòng lồng 2 tầng thường $n^2$; 3 tầng thường $n^3$ — nếu khác, kiểm tra lại.

---

## 12. Bài tập tự luyện

**Bài 1:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to n:
        doing O(1)
for i = 1 to n:
    doing O(1)
```

*Hướng:* $\Theta(n^2) + \Theta(n) = \Theta(n^2)$.

**Bài 2:** Phân tích:

```text
for k = 1 to n:
    for i = 1 to k:
        doing O(1)
```

*Hướng:* $\sum_{k=1}^{n} k = \Theta(n^2)$.

**Bài 3:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to n:
        for k = 1 to n/i:
            doing O(1)
```

*Hướng:* $\sum_{i=1}^{n} n \cdot (n/i) = n^2 H_n = \Theta(n^2\log n)$.

**Bài 4:** Phân tích:

```text
for i = 1 to n:
    doing helper(i)     // helper(x) chạy Θ(x²)
```

*Hướng:* $\sum_{i=1}^{n} i^2 = \Theta(n^3)$.

**Bài 5:** Phân tích:

```text
Function F(n):
    if n <= 1: return
    for i = 1 to n:
        for j = 1 to n:
            doing O(1)
    F(n/2)
    F(n/2)
```

*Hướng:* $T(n) = 2T(n/2) + \Theta(n^2)$ ⇒ Master Theorem Case 3 ⇒ $\Theta(n^2)$.

**Bài 6:** Phân tích:

```text
i = 1
while i <= n:
    for j = 1 to i:
        doing O(1)
    i = i * 2
```

*Hướng:* $\sum_{k=0}^{\log n} 2^k = \Theta(n)$.

**Bài 7:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to n:
        if (i + j) % 2 == 0:
            for k = 1 to n:
                doing O(1)
```

*Hướng:* Số cặp $(i,j)$ có $i+j$ chẵn: $\approx n^2/2$. Mỗi cặp: $\Theta(n)$. Tổng: $\Theta(n^3)$.

**Bài 8:** Phân tích:

```text
for i = 1 to n:
    for j = 1 to n:
        binarySearch(A, n, target)
```

*Hướng:* $n^2 \cdot \Theta(\log n) = \Theta(n^2\log n)$.

**Bài 9:** Phân tích:

```text
for i = 1 to n:
    for j = i to n:
        for k = i to j:
            doing O(1)
```

*Hướng:* $\sum_{i=1}^{n} \sum_{j=i}^{n} (j-i+1) = \sum_{i=1}^{n} \frac{(n-i+1)(n-i+2)}{2} = \Theta(n^3)$.

**Bài 10:** Phân tích:

```text
Function G(n):
    if n <= 1: return
    for i = 1 to n:
        doing O(1)
    G(n/2)
    G(n/2)
    G(n/2)
```

*Hướng:* $T(n) = 3T(n/2) + \Theta(n)$ ⇒ Master Theorem Case 1: $a=3, b=2, x=n^{\log_2 3} \approx n^{1.585}$, $f(n) = n = O(n^{1.585-\varepsilon})$ ⇒ $T(n) = \Theta(n^{\log_2 3})$.

---

## 13. Tóm tắt một trang

### Ba quy tắc nền tảng

| Cấu trúc                  | Quy tắc       | Công thức               |
| ------------------------- | ------------- | ----------------------- |
| Tuần tự (số đoạn cố định) | Cộng, lấy max | $\Theta(\max T_i)$      |
| Tuần tự (số đoạn $= n$)   | Nhân          | $n \cdot \Theta(f)$     |
| Lồng độc lập              | Nhân          | $\Theta(f \cdot g)$     |
| Lồng phụ thuộc            | Viết tổng     | $\sum T_{\text{inner}}$ |

### Bảng tổng hợp các mẫu

| Mẫu                                      | Big-$\Theta$       |
| ---------------------------------------- | ------------------ |
| $k$ vòng độc lập, mỗi vòng $n$           | $\Theta(n^k)$      |
| Vòng ngoài $n$, vòng trong $i$           | $\Theta(n^2)$      |
| Vòng ngoài $n$, vòng trong $i^2$         | $\Theta(n^3)$      |
| Vòng ngoài $n$, vòng trong $\sqrt{i}$    | $\Theta(n^{3/2})$  |
| Vòng ngoài $n$, vòng trong $n/i$         | $\Theta(n\log n)$  |
| Vòng ngoài $n$, vòng trong $\log i$      | $\Theta(n\log n)$  |
| Vòng ngoài $\log n$, vòng trong $n$      | $\Theta(n\log n)$  |
| Vòng ngoài $\log n$, vòng trong $\log n$ | $\Theta(\log^2 n)$ |
| $i$ tăng gấp đôi, vòng trong $i$         | $\Theta(n)$        |

### Đệ quy + vòng lặp

| Hệ thức                          | Big-$\Theta$       |
| -------------------------------- | ------------------ |
| $T(n) = T(n-1) + \Theta(n)$      | $\Theta(n^2)$      |
| $T(n) = T(n-1) + \Theta(n^2)$    | $\Theta(n^3)$      |
| $T(n) = 2T(n/2) + \Theta(n)$     | $\Theta(n\log n)$  |
| $T(n) = 2T(n/2) + \Theta(n^2)$   | $\Theta(n^2)$      |
| $T(n) = T(n/2) + \Theta(\log n)$ | $\Theta(\log^2 n)$ |

### Bẫy cần tránh

1. **Số đoạn phụ thuộc $n$** → nhân, không cộng.
2. **Vòng trong phụ thuộc $i$** → viết tổng, không nhân.
3. **Chi phí hàm được gọi** → nhân vào.
4. **Kiểm tra điều kiện** → tính là phép toán.
5. **`break`** → phân tích best/worst case.
6. **Đệ quy + vòng lặp** → hệ thức truy hồi, không nhân.
7. **Hai nhánh đệ quy** → cộng $T(n/2) + T(n/2) = 2T(n/2)$.

### Chiến lược

1. **Từ trong ra ngoài** — phân tích vòng sâu nhất trước.
2. **Viết tổng** cho vòng phụ thuộc.
3. **Mở rộng hàm** được gọi — không bỏ qua chi phí ẩn.
4. **Đơn giản bằng $\Theta$** — bỏ hằng số và bậc thấp.
5. **Kiểm tra** bằng $n$ nhỏ và so sánh trực giác.

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Phân tích chi tiết** một thuật toán thực tế có nhiều vòng lặp (Dijkstra, KMP, sàng Eratosthenes, Union-Find).
- **Bài tập nâng cao** với tổng phức tạp (ví dụ $\sum_{i=1}^{n} \lfloor n/i \rfloor$, $\sum_{i=1}^{n} i\log i$).
- **Amortized analysis** chi tiết (aggregate, accounting, potential method).
- **Phân tích average case** cho vòng lặp có điều kiện dừng ngẫu nhiên.