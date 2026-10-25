# Phân tích đệ quy (Analysis of Recursion) — Nhập môn

## 1. Đệ quy là gì và tại sao phải phân tích?

**Đệ quy** là kỹ thuật một hàm **gọi lại chính nó** với đầu vào nhỏ hơn, cho đến khi đạt **trường hợp cơ sở** (base case) — nơi không cần gọi đệ quy nữa.

### Ví dụ mở đầu — tính giai thừa

```text
Factorial(n):
    if n <= 1: return 1        // base case
    return n * Factorial(n-1)  // recursive case
```

### Tại sao cần phân tích?

Đệ quy có thể **rất hiệu quả** (Merge Sort: $\Theta(n\log n)$) hoặc **thảm họa** (Fibonacci naive: $\Theta(\varphi^n)$). Sự khác biệt không nằm ở "đệ quy" — mà ở **cấu trúc gọi đệ quy**. Phân tích đệ quy giúp:

1. **Đoán trước** hiệu năng trước khi chạy.
2. **So sánh** các cách đệ quy khác nhau cho cùng bài toán.
3. **Tránh bẫy** đệ quy naive (như Fibonacci).
4. **Chứng minh** thuật toán là tối ưu.

### Khó khăn đặc thù

Với vòng lặp, ta đếm trực tiếp số lần lặp. Với đệ quy, **không có vòng lặp** — ta phải mô tả $T(n)$ theo $T$ của đầu vào nhỏ hơn. Đây là **hệ thức truy hồi** (recurrence relation).

> **Lý thuyết bên cạnh:** Đệ quy và vòng lặp **tương đương** về sức mạnh tính toán — mọi đệ quy đều viết được bằng vòng lặp (dùng stack) và ngược lại. Nhưng **phân tích** chúng cần công cụ khác nhau: vòng lặp → đếm/tổng; đệ quy → hệ thức truy hồi.

---

## 2. Hệ thức truy hồi (Recurrence Relation)

### 2.1. Định nghĩa

**Hệ thức truy hồi** biểu diễn $T(n)$ — chi phí cho đầu vào kích thước $n$ — theo $T$ của các đầu vào **nhỏ hơn**:

$$
T(n) = \text{(chi phí chia/trộn)} + \sum_{\text{bài toán con}} T(\text{kích thước con})
$$

Kèm theo **điều kiện cơ sở** (base case) — nơi $T(n)$ là hằng số.

### 2.2. Cấu trúc tổng quát

Hệ thức truy hồi luôn có hai phần:

| Phần               | Ý nghĩa              | Ví dụ                |
| ------------------ | -------------------- | -------------------- |
| **Base case**      | Điều kiện dừng       | $T(1) = 1$           |
| **Recursive case** | Chi phí + gọi đệ quy | $T(n) = 2T(n/2) + n$ |

**Thiếu base case → hệ thức vô nghĩa** (không xác định được giá trị cụ thể).

### 2.3. Ví dụ 1 — Factorial

```text
Factorial(n):
    if n <= 1: return 1
    return n * Factorial(n-1)
```

**Phân tích:**

- **Base case:** $n \le 1$ — không gọi đệ quy ⇒ $T(1) = \Theta(1)$.
- **Recursive case:** một phép nhân $\Theta(1)$ + một lời gọi $T(n-1)$.

$$
\boxed{T(n) = T(n-1) + \Theta(1), \quad T(1) = \Theta(1)}
$$

### 2.4. Ví dụ 2 — Merge Sort

```text
MergeSort(A, low, high):
    if low >= high: return
    mid = (low + high) / 2
    MergeSort(A, low, mid)          // T(n/2)
    MergeSort(A, mid+1, high)       // T(n/2)
    Merge(A, low, mid, high)        // Θ(n)
```

**Phân tích:**

- **Base case:** $n \le 1$ ⇒ $T(1) = \Theta(1)$.
- **Recursive case:** hai lời gọi $T(n/2)$ + bước trộn $\Theta(n)$.

$$
\boxed{T(n) = 2T(n/2) + \Theta(n), \quad T(1) = \Theta(1)}
$$

### 2.5. Ví dụ 3 — Binary Search

```text
BinarySearch(A, low, high, x):
    if low > high: return -1
    mid = (low + high) / 2
    if A[mid] == x: return mid
    else if A[mid] < x:
        return BinarySearch(A, mid+1, high, x)   // T(n/2)
    else:
        return BinarySearch(A, low, mid-1, x)    // T(n/2)
```

**Phân tích:** Mỗi lần chỉ gọi **một** nửa:

$$
\boxed{T(n) = T(n/2) + \Theta(1), \quad T(1) = \Theta(1)}
$$

### 2.6. Ví dụ 4 — Fibonacci naive

```text
Fib(n):
    if n <= 1: return n
    return Fib(n-1) + Fib(n-2)
```

**Phân tích:** Hai lời gọi với kích thước khác nhau:

$$
\boxed{T(n) = T(n-1) + T(n-2) + \Theta(1), \quad T(0) = T(1) = \Theta(1)}
$$

**Đây không thuộc dạng chuẩn** (vì $n-1$ và $n-2$ khác nhau) — cần công cụ đặc biệt.

### 2.7. Ví dụ 5 — Tìm max đệ quy

```text
FindMax(A, low, high):
    if low == high: return A[low]
    mid = (low + high) / 2
    left = FindMax(A, low, mid)          // T(n/2)
    right = FindMax(A, mid+1, high)      // T(n/2)
    return max(left, right)              // Θ(1)
```

$$
\boxed{T(n) = 2T(n/2) + \Theta(1), \quad T(1) = \Theta(1)}
$$

> **Lý thuyết bên cạnh:** Hệ thức truy hồi là **bản dịch toán học** của code đệ quy. Bước đầu tiên luôn là **viết đúng hệ thức** — nếu sai hệ thức, mọi bước sau vô nghĩa. Ba câu hỏi khi viết: (1) Base case là gì? (2) Có bao nhiêu lời gọi đệ quy? (3) Kích thước mỗi bài toán con? (4) Chi phí chia/trộn ngoài đệ quy?

---

## 3. Ba phương pháp giải hệ thức truy hồi

Có ba phương pháp chính, từ **trực quan** đến **chặt chẽ**:

| Phương pháp                        | Ưu điểm                   | Nhược điểm                        |
| ---------------------------------- | ------------------------- | --------------------------------- |
| **Cây đệ quy** (recursion tree)    | Trực quan, dễ hiểu        | Không chặt chẽ — chỉ để đoán      |
| **Phương pháp thế** (substitution) | Chặt chẽ, chứng minh được | Cần đoán trước nghiệm             |
| **Master Theorem**                 | Công thức đóng, nhanh     | Chỉ áp dụng dạng $aT(n/b) + f(n)$ |

**Chiến lược thực tế:**

1. **Đoán nghiệm** bằng cây đệ quy.
2. **Chứng minh** bằng phương pháp thế (quy nạp).
3. **Hoặc** dùng Master Theorem nếu đúng dạng.

> **Lý thuyết bên cạnh:** Trong thực hành, Master Theorem được dùng nhiều nhất — vì hầu hết thuật toán chia để trị đều có dạng $T(n) = aT(n/b) + f(n)$. Cây đệ quy và phương pháp thế dùng khi không thuộc dạng chuẩn (ví dụ $T(n) = T(n-1) + T(n-2)$).

---

## 4. Phương pháp cây đệ quy (Recursion Tree)

### 4.1. Ý tưởng

Vẽ **cây** biểu diễn các lời gọi đệ quy:

- **Gốc:** bài toán kích thước $n$, chi phí $f(n)$.
- **Con:** các bài toán con, mỗi con có chi phí riêng.
- **Lá:** base case.

**Tổng chi phí = tổng chi phí tất cả các nút.**

### 4.2. Ví dụ 6 — $T(n) = 2T(n/2) + n$

**Vẽ cây:**

```
                    n                    ← tầng 0: chi phí n
                  /   \
                n/2   n/2                ← tầng 1: tổng n
               / \    / \
             n/4 n/4 n/4 n/4             ← tầng 2: tổng n
             / \  / \  / \  / \
            ...  ...  ...  ...           ← ...
             1 1 1 1 ... 1 1             ← tầng log n: tổng n
```

**Phân tích từng tầng:**

| Tầng     | Số nút   | Kích thước mỗi nút | Tổng chi phí tầng |
| -------- | -------- | ------------------ | ----------------- |
| 0        | $1$      | $n$                | $n$               |
| 1        | $2$      | $n/2$              | $n$               |
| 2        | $4$      | $n/4$              | $n$               |
| $\cdots$ | $\cdots$ | $\cdots$           | $\cdots$          |
| $k$      | $2^k$    | $n/2^k$            | $n$               |

**Số tầng:** Cây dừng khi kích thước = 1, tức $n/2^k = 1$ ⇒ $k = \log_2 n$. Vậy có $\log_2 n + 1$ tầng.

**Tổng:**

$$
T(n) = n \cdot (\log_2 n + 1) = \Theta(n\log n)
$$

**Đây là Merge Sort.**

### 4.3. Ví dụ 7 — $T(n) = T(n/2) + 1$

**Vẽ cây:**

```
                  1                ← tầng 0: chi phí 1
                  |
                  1                ← tầng 1: chi phí 1
                  |
                  1                ← tầng 2: chi phí 1
                  |
                 ...
                  |
                  1                ← tầng log n: chi phí 1
```

**Chỉ có một nhánh** — mỗi tầng một nút, chi phí 1. Số tầng: $\log_2 n + 1$.

**Tổng:**

$$
T(n) = \Theta(\log n)
$$

**Đây là Binary Search.**

### 4.4. Ví dụ 8 — $T(n) = 2T(n/2) + 1$

**Vẽ cây:**

```
                   1                ← tầng 0: 1 nút, tổng 1
                  / \
                 1   1              ← tầng 1: 2 nút, tổng 2
                / \ / \
               1  1 1  1            ← tầng 2: 4 nút, tổng 4
               ...
              1 1 ... 1             ← tầng log n: n nút, tổng n
```

**Tổng các tầng:**

$$
T(n) = 1 + 2 + 4 + \cdots + n = 2n - 1 = \Theta(n)
$$

**Đây là FindMax đệ quy.** Đệ quy **không** cải thiện độ phức tạp — vẫn $\Theta(n)$.

### 4.5. Ví dụ 9 — $T(n) = 3T(n/2) + n$

**Vẽ cây:**

```
                    n                ← tầng 0: n
                 /  |  \
               n/2 n/2 n/2            ← tầng 1: 3n/2
              /|\  /|\  /|\
            ...                    ← tầng 2: 9n/4
```

**Tầng $k$:** $3^k$ nút, mỗi nút kích thước $n/2^k$, tổng chi phí $3^k \cdot n/2^k = n \cdot (3/2)^k$.

**Số tầng:** $\log_2 n$.

**Tổng:**

$$
T(n) = \sum_{k=0}^{\log_2 n} n \left(\frac{3}{2}\right)^k = n \cdot \frac{(3/2)^{\log_2 n + 1} - 1}{3/2 - 1} = n \cdot \frac{3 \cdot n^{\log_2 3 / \log_2 2} - 1}{1}
$$

Rút gọn (vì $(3/2)^{\log_2 n} = n^{\log_2 3 - 1}$):

$$
T(n) = \Theta(n^{\log_2 3}) \approx \Theta(n^{1.585})
$$

**Đây là Strassen's Matrix Multiplication** — nhanh hơn nhân ma trận naive $\Theta(n^3)$.

> **Lý thuyết bên cạnh:** Cây đệ quy cho **trực giác** rất mạnh — bạn "thấy" được chi phí phân bố thế nào qua các tầng. Nhưng nó **không chặt chẽ** — cần phương pháp thế để chứng minh. Trong thực hành, vẽ cây để **đoán nghiệm**, rồi chứng minh bằng quy nạp.

---

## 5. Phương pháp thế (Substitution Method)

### 5.1. Ý tưởng

1. **Đoán** nghiệm (từ cây đệ quy hoặc kinh nghiệm).
2. **Chứng minh** bằng quy nạp toán học.
3. **Tìm hằng số** $c$ và $n_0$ phù hợp.

### 5.2. Quy trình

**Bước 1:** Đoán $T(n) = O(g(n))$ (hoặc $\Theta(g(n))$).

**Bước 2:** Giả thiết quy nạp: $T(k) \le c\,g(k)$ với mọi $k < n$.

**Bước 3:** Chứng minh $T(n) \le c\,g(n)$:

- Thay hệ thức truy hồi.
- Áp dụng giả thiết quy nạp cho các $T$ con.
- Biến đổi để được $\le c\,g(n)$.

**Bước 4:** Tìm $c$ và $n_0$ sao cho bất đẳng thức đúng ở **base case** và **bước quy nạp**.

### 5.3. Ví dụ 10 — $T(n) = 2T(n/2) + n$

**Đoán:** $T(n) = O(n\log n)$.

**Giả thiết quy nạp:** $T(k) \le c\,k\log k$ với mọi $k < n$.

**Chứng minh với $n$:**

$$
T(n) = 2T(n/2) + n \le 2 \cdot c(n/2)\log(n/2) + n
$$

$$
= cn\log(n/2) + n = cn(\log n - 1) + n = cn\log n - cn + n
$$

Để $\le cn\log n$, cần:

$$
-cn + n \le 0 \iff n(1 - c) \le 0 \iff c \ge 1
$$

**Chọn $c = 1$:** $T(n) \le n\log n$ với $n$ đủ lớn.

**Kiểm tra base case:** $T(1) = 1 \le c \cdot 1 \cdot \log 1 = 0$? — **Sai!** Vì $\log 1 = 0$.

**Khắc phục:** Dùng base case $n = 2$: $T(2) = 2T(1) + 2 = 2 + 2 = 4$. Kiểm tra: $4 \le c \cdot 2 \cdot \log 2 = 2c$. Chọn $c \ge 2$.

**Kết luận:** Với $c = 2$, $n_0 = 2$: $T(n) \le 2n\log n$ với $n \ge 2$. Vậy $T(n) = O(n\log n)$. $\blacksquare$

**Nhận xét quan trọng:** Base case có thể **không khớp** với giả thiết quy nạp — cần **điều chỉnh** $n_0$ hoặc $c$. Đây là bẫy phổ biến.

### 5.4. Ví dụ 11 — $T(n) = T(n/2) + 1$

**Đoán:** $T(n) = O(\log n)$.

**Giả thiết:** $T(k) \le c\log k$ với $k < n$.

**Chứng minh:**

$$
T(n) = T(n/2) + 1 \le c\log(n/2) + 1 = c(\log n - 1) + 1 = c\log n - c + 1
$$

Để $\le c\log n$, cần $-c + 1 \le 0$ ⇒ $c \ge 1$. Chọn $c = 2$.

**Base case:** $T(2) = T(1) + 1 = 2$. Kiểm tra: $2 \le 2\log 2 = 2$. Đúng.

**Kết luận:** $T(n) = O(\log n)$. $\blacksquare$

### 5.5. Ví dụ 12 — $T(n) = 2T(n/2) + 1$

**Đoán:** $T(n) = O(n)$.

**Giả thiết:** $T(k) \le ck$ với $k < n$.

**Chứng minh:**

$$
T(n) = 2T(n/2) + 1 \le 2 \cdot c(n/2) + 1 = cn + 1
$$

Để $\le cn$, cần $1 \le 0$ — **không thể** với mọi $c$. Nhưng ta có thể **nới lỏng** giả thiết: $T(k) \le ck - d$ (trừ thêm hằng số).

**Giả thiết mới:** $T(k) \le ck - d$ với $k < n$, $d > 0$.

$$
T(n) = 2T(n/2) + 1 \le 2(c(n/2) - d) + 1 = cn - 2d + 1
$$

Để $\le cn - d$, cần $-2d + 1 \le -d$ ⇒ $d \ge 1$. Chọn $d = 1$, $c = 2$.

**Kết luận:** $T(n) \le 2n - 1$ ⇒ $T(n) = O(n)$. $\blacksquare$

**Kỹ thuật:** Khi giả thiết đơn giản không đủ, **trừ thêm hằng số** — đây là mẹo chuẩn trong phương pháp thế.

### 5.6. Ví dụ 13 — $T(n) = 2T(n/2) + n$ (chặt hơn)

**Đoán:** $T(n) = \Theta(n\log n)$ — cần chứng minh cả hai chiều.

**Chiều $O$:** Đã chứng minh ở Ví dụ 10.

**Chiều $\Omega$:** Đoán $T(n) \ge c\,n\log n$. Giả thiết $T(k) \ge c\,k\log k$:

$$
T(n) = 2T(n/2) + n \ge 2c(n/2)\log(n/2) + n = cn\log(n/2) + n = cn\log n - cn + n
$$

Để $\ge cn\log n$, cần $-cn + n \ge 0$ ⇒ $c \le 1$. Chọn $c = 1/2$ (hoặc nhỏ hơn).

**Kết luận:** $T(n) = \Theta(n\log n)$. $\blacksquare$

> **Lý thuyết bên cạnh:** Phương pháp thế là **công cụ chứng minh** mạnh nhất — nhưng cần **đoán đúng**. Nếu đoán sai (quá chặt), sẽ không chứng minh được (dù kết quả đúng). Khi đó, cần **nới lỏng** giả thiết (trừ hằng số, thêm số hạng bậc thấp) hoặc **đoán lại**.

---

## 6. Master Theorem — công cụ mạnh nhất

### 6.1. Dạng áp dụng

Master Theorem áp dụng cho hệ thức dạng:

$$
\boxed{T(n) = a\,T(n/b) + f(n)}
$$

với:

- $a \ge 1$ — số bài toán con.
- $b > 1$ — hệ số chia kích thước.
- $f(n)$ — chi phí chia/trộn (không đệ quy).

### 6.2. Phát biểu

Đặt:

$$
x = n^{\log_b a}
$$

**So sánh $f(n)$ với $x$:**

| Trường hợp | Điều kiện                                                                 | Kết quả                  |
| ---------- | ------------------------------------------------------------------------- | ------------------------ |
| **Case 1** | $f(n) = O(x^{1-\varepsilon})$ với $\varepsilon > 0$                       | $T(n) = \Theta(x)$       |
| **Case 2** | $f(n) = \Theta(x)$                                                        | $T(n) = \Theta(x\log n)$ |
| **Case 3** | $f(n) = \Omega(x^{1+\varepsilon})$ và $a\,f(n/b) \le c\,f(n)$ với $c < 1$ | $T(n) = \Theta(f(n))$    |

**Trực giác:**

- **Case 1:** Chi phí **bài toán con** áp đảo ⇒ tổng = chi phí lá.
- **Case 2:** Chi phí **cân bằng** ⇒ thêm hệ số $\log n$ (số tầng).
- **Case 3:** Chi phí **chia/trộn** áp đảo ⇒ tổng = chi phí gốc.

### 6.3. Ví dụ 14 — $T(n) = 2T(n/2) + n$ (Merge Sort)

$a = 2$, $b = 2$, $f(n) = n$.

$$
x = n^{\log_2 2} = n
$$

$f(n) = n = \Theta(x)$ ⇒ **Case 2**:

$$
T(n) = \Theta(n\log n)
$$

### 6.4. Ví dụ 15 — $T(n) = T(n/2) + 1$ (Binary Search)

$a = 1$, $b = 2$, $f(n) = 1$.

$$
x = n^{\log_2 1} = n^0 = 1
$$

$f(n) = 1 = \Theta(x)$ ⇒ **Case 2**:

$$
T(n) = \Theta(\log n)
$$

### 6.5. Ví dụ 16 — $T(n) = 2T(n/2) + 1$ (FindMax)

$a = 2$, $b = 2$, $f(n) = 1$.

$$
x = n^{\log_2 2} = n
$$

$f(n) = 1 = O(n^{1-\varepsilon})$ với $\varepsilon = 1$ ⇒ **Case 1**:

$$
T(n) = \Theta(n)
$$

### 6.6. Ví dụ 17 — $T(n) = 4T(n/2) + n$ (Case 1)

$a = 4$, $b = 2$, $f(n) = n$.

$$
x = n^{\log_2 4} = n^2
$$

$f(n) = n = O(n^{2-\varepsilon})$ với $\varepsilon = 1$ ⇒ **Case 1**:

$$
T(n) = \Theta(n^2)
$$

### 6.7. Ví dụ 18 — $T(n) = 4T(n/2) + n^2$ (Case 2)

$a = 4$, $b = 2$, $f(n) = n^2$.

$$
x = n^{\log_2 4} = n^2
$$

$f(n) = n^2 = \Theta(x)$ ⇒ **Case 2**:

$$
T(n) = \Theta(n^2\log n)
$$

### 6.8. Ví dụ 19 — $T(n) = 4T(n/2) + n^3$ (Case 3)

$a = 4$, $b = 2$, $f(n) = n^3$.

$$
x = n^{\log_2 4} = n^2
$$

$f(n) = n^3 = \Omega(n^{2+\varepsilon})$ với $\varepsilon = 1$ ⇒ **Case 3** (kiểm tra điều kiện chính quy: $a f(n/b) = 4(n/2)^3 = n^3/2 \le (1/2) n^3$ — đúng với $c = 1/2$):

$$
T(n) = \Theta(n^3)
$$

### 6.9. Ví dụ 20 — $T(n) = 7T(n/2) + n^2$ (Strassen)

$a = 7$, $b = 2$, $f(n) = n^2$.

$$
x = n^{\log_2 7} \approx n^{2.807}
$$

$f(n) = n^2 = O(n^{2.807-\varepsilon})$ ⇒ **Case 1**:

$$
T(n) = \Theta(n^{\log_2 7}) \approx \Theta(n^{2.807})
$$

### 6.10. Bảng tổng hợp Master Theorem

| Hệ thức             | $a$ | $b$ | $f(n)$    | $x$         | Case | $T(n)$              |
| ------------------- | --- | --- | --------- | ----------- | ---- | ------------------- |
| $2T(n/2) + n$       | 2   | 2   | $n$       | $n$         | 2    | $\Theta(n\log n)$   |
| $T(n/2) + 1$        | 1   | 2   | $1$       | $1$         | 2    | $\Theta(\log n)$    |
| $2T(n/2) + 1$       | 2   | 2   | $1$       | $n$         | 1    | $\Theta(n)$         |
| $4T(n/2) + n$       | 4   | 2   | $n$       | $n^2$       | 1    | $\Theta(n^2)$       |
| $4T(n/2) + n^2$     | 4   | 2   | $n^2$     | $n^2$       | 2    | $\Theta(n^2\log n)$ |
| $4T(n/2) + n^3$     | 4   | 2   | $n^3$     | $n^2$       | 3    | $\Theta(n^3)$       |
| $7T(n/2) + n^2$     | 7   | 2   | $n^2$     | $n^{2.807}$ | 1    | $\Theta(n^{2.807})$ |
| $2T(n/4) + n$       | 2   | 4   | $n$       | $n^{0.5}$   | 3    | $\Theta(n)$         |
| $2T(n/2) + n\log n$ | 2   | 2   | $n\log n$ | $n$         | —    | Không áp dụng       |

> **Lý thuyết bên cạnh:** Master Theorem **không áp dụng** khi:
> - $f(n)$ không so sánh được với $n^{\log_b a}$ (ví dụ $f(n) = n\log n$ với $a=b=2$).
> - $a$ hoặc $b$ phụ thuộc $n$.
> - Hệ thức dạng **trừ** ($T(n) = T(n-1) + \ldots$).
> - Điều kiện chính quy của Case 3 không thỏa.
>
> Khi đó, phải dùng cây đệ quy hoặc phương pháp thế.

---

## 7. Hệ thức "trừ" — không thuộc Master Theorem

### 7.1. Dạng

$$
T(n) = T(n-1) + f(n)
$$

hoặc tổng quát hơn:

$$
T(n) = T(n-1) + T(n-2) + \ldots
$$

**Đây không phải chia để trị** — kích thước giảm **tuyến tính**, không chia.

### 7.2. Giải bằng cách mở rộng (unrolling)

**Ví dụ 21 — $T(n) = T(n-1) + 1$ (Factorial)**

Mở rộng:

$$
T(n) = T(n-1) + 1 = T(n-2) + 2 = T(n-3) + 3 = \cdots = T(1) + (n-1)
$$

Với $T(1) = 1$:

$$
T(n) = n = \Theta(n)
$$

**Ví dụ 22 — $T(n) = T(n-1) + n$ (Selection Sort đệ quy)**

$$
T(n) = T(n-1) + n = T(n-2) + (n-1) + n = \cdots = \sum_{k=1}^{n} k = \frac{n(n+1)}{2} = \Theta(n^2)
$$

**Ví dụ 23 — $T(n) = T(n-1) + n^2$**

$$
T(n) = \sum_{k=1}^{n} k^2 = \Theta(n^3)
$$

**Ví dụ 24 — $T(n) = 2T(n-1) + 1$ (Hanoi Tower)**

Mở rộng:

$$
T(n) = 2T(n-1) + 1 = 4T(n-2) + 3 = 8T(n-3) + 7 = \cdots = 2^{n-1} T(1) + (2^{n-1} - 1)
$$

Với $T(1) = 1$:

$$
T(n) = 2^n - 1 = \Theta(2^n)
$$

**Đây là bài toán Tháp Hà Nội** — độ phức tạp hàm mũ.

### 7.3. Bảng tóm tắt hệ thức trừ

| Hệ thức               | $T(n)$        |
| --------------------- | ------------- |
| $T(n) = T(n-1) + 1$   | $\Theta(n)$   |
| $T(n) = T(n-1) + n$   | $\Theta(n^2)$ |
| $T(n) = T(n-1) + n^2$ | $\Theta(n^3)$ |
| $T(n) = 2T(n-1) + 1$  | $\Theta(2^n)$ |
| $T(n) = 3T(n-1) + 1$  | $\Theta(3^n)$ |

**Quy tắc:** Nếu $T(n) = aT(n-1) + f(n)$ với $a \ge 1$:

- **$a = 1$:** $T(n) = \Theta\left(\sum f(k)\right)$ — tổng $f$.
- **$a \ge 2$:** $T(n) = \Theta(a^n)$ — hàm mũ (trừ khi $f$ tăng nhanh hơn).

### 7.4. Fibonacci — trường hợp đặc biệt

**Ví dụ 25 — $T(n) = T(n-1) + T(n-2) + \Theta(1)$**

**Không thuộc** cả hai dạng trên. Dùng **cây đệ quy**:

- Cây có cấu trúc giống dãy Fibonacci.
- Số nút xấp xỉ $F_n$ (số Fibonacci thứ $n$).
- Công thức Binet: $F_n = \Theta(\varphi^n)$ với $\varphi = (1+\sqrt{5})/2 \approx 1.618$.

$$
T(n) = \Theta(\varphi^n) \approx \Theta(1.618^n)
$$

**Không phải $2^n$** — dù nhiều tài liệu viết $O(2^n)$ (cận trên lỏng). Cận chặt là $\varphi^n$.

**Chứng minh (ý tưởng):** Giả sử $T(n) = c^n$. Thay vào:

$$
c^n = c^{n-1} + c^{n-2} \Rightarrow c^2 = c + 1 \Rightarrow c = \frac{1 \pm \sqrt{5}}{2}
$$

Nghiệm dương lớn nhất: $c = \varphi$. Vậy $T(n) = \Theta(\varphi^n)$.

> **Lý thuyết bên cạnh:** Phương trình đặc trưng là công cụ chuẩn cho hệ thức **tuyến tính thuần nhất** (không có $f(n)$). Với Fibonacci: $T(n) = T(n-1) + T(n-2)$ — phương trình đặc trưng $c^2 = c + 1$. Nghiệm cho biết bậc tăng. Đây là kiến thức cơ bản của phương trình sai phân — nhưng trong phân tích thuật toán, thường chỉ cần biết Fibonacci là $\Theta(\varphi^n)$.

---

## 8. So sánh ba phương pháp

| Tiêu chí         | Cây đệ quy  | Phương pháp thế | Master Theorem       |
| ---------------- | ----------- | --------------- | -------------------- |
| **Mục đích**     | Đoán nghiệm | Chứng minh      | Công thức đóng       |
| **Chặt chẽ**     | ❌           | ✅               | ✅                    |
| **Độ khó**       | Dễ          | Trung bình      | Dễ (nếu đúng dạng)   |
| **Áp dụng**      | Mọi hệ thức | Mọi hệ thức     | Chỉ $aT(n/b) + f(n)$ |
| **Khi nào dùng** | Bước đầu    | Sau khi đoán    | Khi đúng dạng        |

**Chiến lược thực tế:**

1. **Kiểm tra Master Theorem trước** — nếu đúng dạng, dùng luôn.
2. **Nếu không:** vẽ cây đệ quy để đoán nghiệm.
3. **Chứng minh** bằng phương pháp thế.

---

## 9. Những bẫy phổ biến

### Bẫy 1 — "Master Theorem áp dụng cho mọi hệ thức"

**Sai.** Master Theorem chỉ áp dụng dạng $aT(n/b) + f(n)$ với $a, b$ hằng số. Không áp dụng cho:

- Hệ thức trừ: $T(n) = T(n-1) + \ldots$
- $a$ hoặc $b$ phụ thuộc $n$: $T(n) = nT(n/2) + \ldots$
- $f(n)$ không so sánh được: $T(n) = 2T(n/2) + n\log n$ (nằm giữa Case 2 và Case 3).

### Bẫy 2 — "Fibonacci naive là $O(2^n)$ nên cận chặt là $2^n$"

**Sai.** $O(2^n)$ là cận trên **lỏng**. Cận chặt là $\Theta(\varphi^n) \approx \Theta(1.618^n)$ — nhỏ hơn $2^n$ rất nhiều.

### Bẫy 3 — "Base case không quan trọng"

**Sai.** Base case sai ⇒ hệ thức vô nghĩa. Và khi chứng minh bằng phương pháp thế, base case có thể **không khớp** với giả thiết quy nạp — cần điều chỉnh $c$ hoặc $n_0$.

### Bẫy 4 — "Số tầng cây là $\log_2 n$ luôn"

**Sai.** Số tầng phụ thuộc **hệ số chia** $b$:

- Chia đôi ($b=2$): $\log_2 n$ tầng.
- Chia ba ($b=3$): $\log_3 n$ tầng.

Nhưng **bậc** vẫn là $\Theta(\log n)$ — chỉ khác hằng số.

### Bẫy 5 — "Đệ quy luôn chậm hơn vòng lặp"

**Sai.** Tìm max đệ quy là $\Theta(n)$ — giống vòng lặp. Merge Sort đệ quy là $\Theta(n\log n)$ — tối ưu cho comparison sort.

### Bẫy 6 — "Hai lời gọi đệ quy luôn nhân đôi chi phí"

**Sai.** $T(n) = 2T(n/2) + n$ là $\Theta(n\log n)$, không phải $\Theta(2^n)$. Vì **kích thước giảm**, không phải giữ nguyên.

### Bẫy 7 — "Master Theorem cho kết quả chính xác"

**Sai.** Master Theorem cho **$\Theta$** (bậc) — không cho hằng số. Nếu cần hằng số (ví dụ so sánh hai thuật toán cùng bậc), phải dùng phương pháp thế.

### Bẫy 8 — "Điều kiện chính quy Case 3 luôn thỏa"

**Sai.** Case 3 yêu cầu $a\,f(n/b) \le c\,f(n)$ với $c < 1$. Điều kiện này thường thỏa với $f$ đa thức, nhưng **không** thỏa với $f$ dao động hoặc $f$ tăng quá nhanh.

### Bẫy 9 — "Hệ thức trừ luôn là $\Theta(n)$"

**Sai.** $T(n) = T(n-1) + n$ là $\Theta(n^2)$; $T(n) = 2T(n-1) + 1$ là $\Theta(2^n)$. Phụ thuộc $f(n)$ và hệ số $a$.

### Bẫy 10 — "Đoán nghiệm bằng cây đệ quy là đủ"

**Sai.** Cây đệ quy chỉ để **đoán**. Muốn **chứng minh**, cần phương pháp thế (quy nạp). Trong bài thi, nếu chỉ vẽ cây mà không chứng minh, có thể mất điểm.

> **Lý thuyết bên cạnh:** Hầu hết bẫy đến từ việc **áp dụng công cụ sai chỗ**. Master Theorem rất mạnh nhưng **hẹp** — chỉ đúng dạng chuẩn. Cây đệ quy trực quan nhưng **không chặt**. Phương pháp thế chặt nhưng **cần đoán**. Kết hợp cả ba là chiến lược tốt nhất.

---

## 10. Ví dụ tổng hợp — phân tích từng bước

### Ví dụ 26 — Phân tích đầy đủ một hàm đệ quy

```text
Function Mystery(A, n):
    if n <= 1: return A[1]
    mid = n / 2
    left = Mystery(A, mid)              // T(n/2)
    right = Mystery(A + mid, n - mid)   // T(n/2)
    return left + right                  // Θ(1)
```

**Bước 1 — Viết hệ thức:**

$$
T(n) = 2T(n/2) + \Theta(1), \quad T(1) = \Theta(1)
$$

**Bước 2 — Áp dụng Master Theorem:**

$a = 2$, $b = 2$, $f(n) = 1$, $x = n^{\log_2 2} = n$.

$f(n) = 1 = O(n^{1-\varepsilon})$ ⇒ **Case 1** ⇒ $T(n) = \Theta(n)$.

**Bước 3 — Kiểm tra bằng cây đệ quy:**

Tầng 0: 1 nút, chi phí 1.
Tầng 1: 2 nút, chi phí 2.
Tầng $k$: $2^k$ nút, chi phí $2^k$.
Tầng $\log n$: $n$ nút, chi phí $n$.

Tổng: $1 + 2 + 4 + \cdots + n = 2n - 1 = \Theta(n)$. ✓

**Kết luận:** $T(n) = \Theta(n)$. Đây là **tính tổng mảng** đệ quy — không cải thiện so với vòng lặp.

### Ví dụ 27 — Hệ thức không chuẩn

```text
Function Weird(n):
    if n <= 1: return 1
    for i = 1 to n:
        doing O(1)                      // Θ(n)
    Weird(n/2)
    Weird(n/2)
    Weird(n/2)
```

**Hệ thức:**

$$
T(n) = 3T(n/2) + \Theta(n)
$$

**Master Theorem:** $a = 3$, $b = 2$, $x = n^{\log_2 3} \approx n^{1.585}$, $f(n) = n = O(n^{1.585-\varepsilon})$ ⇒ **Case 1**:

$$
T(n) = \Theta(n^{\log_2 3}) \approx \Theta(n^{1.585})
$$

### Ví dụ 28 — Hệ thức có $f(n) = n\log n$

```text
Function Tricky(n):
    if n <= 1: return 1
    Tricky(n/2)                         // T(n/2)
    Tricky(n/2)                         // T(n/2)
    sort(A, n)                          // Θ(n log n)
```

**Hệ thức:**

$$
T(n) = 2T(n/2) + \Theta(n\log n)
$$

**Master Theorem:** $a = 2$, $b = 2$, $x = n$. $f(n) = n\log n$ — **không** so sánh được với $n$:

- $f(n) = \Omega(n^{1+\varepsilon})$? — Không (vì $\log n$ chậm hơn $n^\varepsilon$).
- $f(n) = O(n^{1-\varepsilon})$? — Không.
- $f(n) = \Theta(n)$? — Không (vì có $\log n$).

**Không áp dụng Master Theorem.** Dùng cây đệ quy:

- Tầng 0: $n\log n$
- Tầng 1: $2 \cdot (n/2)\log(n/2) = n\log(n/2) = n\log n - n$
- Tầng 2: $4 \cdot (n/4)\log(n/4) = n\log(n/4) = n\log n - 2n$
- ...
- Tầng $k$: $n\log n - kn$

Tổng:

$$
T(n) = \sum_{k=0}^{\log n} (n\log n - kn) = (\log n + 1) n\log n - n \cdot \frac{\log n(\log n + 1)}{2} = \Theta(n\log^2 n)
$$

**Đây là trường hợp đặc biệt** — Master Theorem không áp dụng, nhưng kết quả vẫn là $\Theta(n\log^2 n)$.

> **Lý thuyết bên cạnh:** Khi $f(n)$ nằm **giữa** hai case của Master Theorem (như $n\log n$ giữa $n$ và $n^{1+\varepsilon}$), cần dùng cây đệ quy hoặc **phương pháp thế mở rộng**. Kết quả thường có thêm hệ số $\log$ — như $\Theta(n\log^2 n)$.

---

## 11. Chiến lược phân tích đệ quy

### 11.1. Quy trình 5 bước

1. **Viết hệ thức truy hồi:**
   - Base case là gì?
   - Bao nhiêu lời gọi đệ quy?
   - Kích thước mỗi bài toán con?
   - Chi phí chia/trộn ngoài đệ quy?
2. **Kiểm tra dạng:**
   - $aT(n/b) + f(n)$? → Master Theorem.
   - $T(n-1) + f(n)$? → Mở rộng.
   - $T(n-1) + T(n-2) + \ldots$? → Phương trình đặc trưng.
3. **Áp dụng công cụ phù hợp.**
4. **Kiểm tra bằng cây đệ quy** (nếu cần).
5. **Chứng minh** bằng phương pháp thế (nếu cần chặt chẽ).

### 11.2. Bảng quyết định

| Dạng hệ thức      | Công cụ                | Ví dụ       |
| ----------------- | ---------------------- | ----------- |
| $aT(n/b) + f(n)$  | Master Theorem         | Merge Sort  |
| $T(n-1) + f(n)$   | Mở rộng                | Factorial   |
| $aT(n-1) + f(n)$  | Mở rộng                | Hanoi Tower |
| $T(n-1) + T(n-2)$ | Phương trình đặc trưng | Fibonacci   |
| Hệ thức phức tạp  | Cây đệ quy + thế       | Tricky      |

### 11.3. Mẹo kiểm tra

- **Thử $n$ nhỏ:** $n = 1, 2, 4$ — xem công thức có khớp không.
- **Kiểm tra bậc:** Nếu $T(n) = \Theta(n\log n)$, thử $n \to 2n$ — thời gian tăng $\approx 2 \times (1 + 1/\log n)$ lần.
- **So sánh với trực giác:** Đệ quy chia đôi thường $\log n$ hoặc $n\log n$; đệ quy trừ 1 thường $n$ hoặc $n^2$; đệ quy nhân đôi thường $2^n$.

---

## 12. Bài tập tự luyện

**Bài 1:** Giải $T(n) = 2T(n/4) + 1$, $T(1) = 1$.

*Hướng:* Master Theorem: $a=2$, $b=4$, $x = n^{\log_4 2} = n^{0.5}$, $f(n)=1=O(n^{0.5-\varepsilon})$ ⇒ Case 1 ⇒ $\Theta(\sqrt{n})$.

**Bài 2:** Giải $T(n) = 3T(n/3) + n$.

*Hướng:* $a=3$, $b=3$, $x=n$, $f(n)=n=\Theta(x)$ ⇒ Case 2 ⇒ $\Theta(n\log n)$.

**Bài 3:** Giải $T(n) = T(n-1) + n^2$.

*Hướng:* Mở rộng: $T(n) = \sum_{k=1}^{n} k^2 = \Theta(n^3)$.

**Bài 4:** Giải $T(n) = 2T(n-1) + n$.

*Hướng:* Mở rộng: $T(n) = \Theta(2^n)$ — hàm mũ áp đảo.

**Bài 5:** Giải $T(n) = 8T(n/2) + n^2$.

*Hướng:* $a=8$, $b=2$, $x=n^3$, $f(n)=n^2=O(n^{3-\varepsilon})$ ⇒ Case 1 ⇒ $\Theta(n^3)$.

**Bài 6:** Giải $T(n) = T(n/2) + n$.

*Hướng:* $a=1$, $b=2$, $x=1$, $f(n)=n=\Omega(n^{0+\varepsilon})$ ⇒ Case 3 (kiểm tra điều kiện: $f(n/2) = n/2 \le cn$ với $c=1$) ⇒ $\Theta(n)$.

**Bài 7:** Chứng minh $T(n) = 2T(n/2) + n$ là $\Theta(n\log n)$ bằng phương pháp thế.

*Hướng:* Đã làm ở Ví dụ 10 và 13.

**Bài 8:** Giải $T(n) = T(n-1) + T(n-2) + 1$.

*Hướng:* Phương trình đặc trưng $c^2 = c + 1$ ⇒ $c = \varphi$ ⇒ $\Theta(\varphi^n)$.

**Bài 9:** Tại sao không áp dụng Master Theorem cho $T(n) = T(n-1) + n$?

*Hướng:* Vì kích thước giảm **tuyến tính** ($n-1$), không phải **chia** ($n/b$). Master Theorem yêu cầu $b > 1$.

**Bài 10:** Giải $T(n) = 2T(n/2) + n\log n$.

*Hướng:* Master Theorem không áp dụng (như Ví dụ 28). Cây đệ quy: $\Theta(n\log^2 n)$.

---

## 13. Tóm tắt một trang

### Ba công cụ

| Công cụ             | Khi nào dùng     | Kết quả               |
| ------------------- | ---------------- | --------------------- |
| **Cây đệ quy**      | Đoán nghiệm      | Trực quan, không chặt |
| **Phương pháp thế** | Chứng minh       | Chặt chẽ, cần đoán    |
| **Master Theorem**  | $aT(n/b) + f(n)$ | Công thức đóng        |

### Master Theorem — bảng tra

$$
T(n) = aT(n/b) + f(n), \quad x = n^{\log_b a}
$$

| Case | Điều kiện                          | $T(n)$            |
| ---- | ---------------------------------- | ----------------- |
| 1    | $f(n) = O(x^{1-\varepsilon})$      | $\Theta(x)$       |
| 2    | $f(n) = \Theta(x)$                 | $\Theta(x\log n)$ |
| 3    | $f(n) = \Omega(x^{1+\varepsilon})$ | $\Theta(f(n))$    |

### Hệ thức trừ

| Hệ thức                  | $T(n)$              |
| ------------------------ | ------------------- |
| $T(n) = T(n-1) + 1$      | $\Theta(n)$         |
| $T(n) = T(n-1) + n$      | $\Theta(n^2)$       |
| $T(n) = 2T(n-1) + 1$     | $\Theta(2^n)$       |
| $T(n) = T(n-1) + T(n-2)$ | $\Theta(\varphi^n)$ |

### Chiến lược

1. **Viết hệ thức** từ code.
2. **Kiểm tra dạng** — Master Theorem? Trừ? Fibonacci?
3. **Áp dụng công cụ** phù hợp.
4. **Kiểm tra** bằng cây đệ quy / thử $n$ nhỏ.
5. **Chứng minh** bằng phương pháp thế (nếu cần).

### Bẫy cần tránh

1. Master Theorem **không** áp dụng cho hệ thức trừ.
2. Fibonacci là $\Theta(\varphi^n)$, **không** phải $\Theta(2^n)$.
3. Base case có thể **không khớp** giả thiết quy nạp — điều chỉnh $c$, $n_0$.
4. $f(n) = n\log n$ **không** thuộc case nào của Master Theorem — dùng cây đệ quy.
5. Đệ quy **không** luôn chậm hơn vòng lặp.
6. Hai lời gọi đệ quy **không** luôn nhân đôi — phụ thuộc kích thước con.

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Chứng minh đầy đủ** Master Theorem (cả ba case) bằng cây đệ quy.
- **Phương pháp thế nâng cao** — kỹ thuật "trừ hằng số", "đoán qua biến đổi".
- **Phân tích amortized cho đệ quy** (ví dụ Union-Find, balanced BST).
- **Bài tập nâng cao** với hệ thức phức tạp (ví dụ $T(n) = T(n/3) + T(2n/3) + n$, $T(n) = \sqrt{n}\,T(\sqrt{n}) + n$).
- **Akra–Bazzi** — mở rộng Master Theorem cho hệ thức không đều.