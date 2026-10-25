# Phương pháp cây đệ quy (Recursion Tree Method) — Lý thuyết + Ví dụ

## 1. Cây đệ quy là gì?

**Cây đệ quy** (recursion tree) là một **công cụ trực quan** để giải hệ thức truy hồi. Ý tưởng:

- Mỗi **nút** biểu diễn một lời gọi đệ quy với kích thước cụ thể.
- **Gốc** là bài toán ban đầu kích thước $n$.
- **Con** là các bài toán con được sinh ra từ lời gọi đệ quy.
- **Lá** là base case (không gọi đệ quy nữa).
- Mỗi nút có **chi phí riêng** (chi phí chia/trộn ngoài đệ quy).

**Tổng chi phí = tổng chi phí tất cả các nút trong cây.**

### Tại sao dùng cây đệ quy?

| Ưu điểm                                     | Nhược điểm                       |
| ------------------------------------------- | -------------------------------- |
| **Trực quan** — "thấy" được chi phí phân bố | **Không chặt chẽ** — chỉ để đoán |
| **Linh hoạt** — áp dụng cho mọi hệ thức     | Không thay thế được chứng minh   |
| **Phát hiện pattern** — nhận ra cấu trúc    | Với cây lớn, khó vẽ chính xác    |
| **Kết hợp tốt** với phương pháp thế         | Cần kiểm tra lại bằng quy nạp    |

**Vai trò thực tế:** Cây đệ quy dùng để **đoán nghiệm** — sau đó chứng minh bằng **phương pháp thế**. Hoặc dùng khi **Master Theorem không áp dụng**.

> **Lý thuyết bên cạnh:** Cây đệ quy không phải là **chứng minh** — nó là **kỹ thuật khám phá**. Kết quả từ cây đệ quy cần được xác nhận bằng quy nạp (phương pháp thế) để đảm bảo tính chặt chẽ. Tuy nhiên, trong thực hành, cây đệ quy thường cho kết quả **đúng** — vì nó phản ánh chính xác cấu trúc tính toán.

---

## 2. Quy trình 5 bước

### Bước 1 — Viết hệ thức truy hồi

Từ code, xác định:

- **Base case:** $T(1) = \Theta(1)$ (hoặc tương tự).
- **Recursive case:** $T(n) = aT(n/b) + f(n)$ (hoặc dạng khác).

### Bước 2 — Vẽ cây

- **Gốc:** ghi $n$ (kích thước) và $f(n)$ (chi phí nút).
- **Mỗi nút:** vẽ $a$ con, mỗi con kích thước $n/b$.
- **Tiếp tục** cho đến khi đạt base case (kích thước 1 hoặc hằng số).

### Bước 3 — Tính chi phí từng tầng

Với mỗi tầng $k$:

- **Số nút:** $a^k$.
- **Kích thước mỗi nút:** $n/b^k$.
- **Chi phí mỗi nút:** $f(n/b^k)$.
- **Tổng chi phí tầng:** $a^k \cdot f(n/b^k)$.

### Bước 4 — Xác định số tầng

Cây dừng khi kích thước = 1:

$$
\frac{n}{b^k} = 1 \Rightarrow k = \log_b n
$$

Vậy có $\log_b n + 1$ tầng (từ $k=0$ đến $k=\log_b n$).

### Bước 5 — Cộng tất cả các tầng

$$
T(n) = \sum_{k=0}^{\log_b n} a^k \cdot f\!\left(\frac{n}{b^k}\right)
$$

Đơn giản bằng $\Theta$ — xác định tầng nào **áp đảo** (dominant).

> **Lý thuyết bên cạnh:** Bước 5 là **khó nhất** — phải xác định tầng nào chi phối. Ba khả năng:
> - **Tầng lá áp đảo** (chi phí tăng theo $k$) → tổng = chi phí lá.
> - **Tầng gốc áp đảo** (chi phí giảm theo $k$) → tổng = chi phí gốc.
> - **Mọi tầng bằng nhau** (chi phí không đổi) → tổng = chi phí mỗi tầng × số tầng.
>
> Đây chính là cơ sở của **Master Theorem** — ba case tương ứng với ba khả năng này.

---

## 3. Ví dụ cơ bản — Merge Sort

### 3.1. Hệ thức

$$
T(n) = 2T(n/2) + n, \quad T(1) = 1
$$

### 3.2. Vẽ cây

```
                    n                    ← tầng 0: chi phí n
                  /   \
                n/2   n/2                ← tầng 1: 2 nút, mỗi nút n/2
               / \    / \
             n/4 n/4 n/4 n/4             ← tầng 2: 4 nút, mỗi nút n/4
             / \  / \  / \  / \
            ...  ...  ...  ...           ← ...
             1 1 1 1 ... 1 1             ← tầng log₂n: n nút, mỗi nút 1
```

### 3.3. Chi phí từng tầng

| Tầng $k$   | Số nút   | Kích thước mỗi nút | Chi phí mỗi nút | Tổng tầng             |
| ---------- | -------- | ------------------ | --------------- | --------------------- |
| 0          | $1$      | $n$                | $n$             | $n$                   |
| 1          | $2$      | $n/2$              | $n/2$           | $2 \cdot n/2 = n$     |
| 2          | $4$      | $n/4$              | $n/4$           | $4 \cdot n/4 = n$     |
| $\cdots$   | $\cdots$ | $\cdots$           | $\cdots$        | $\cdots$              |
| $k$        | $2^k$    | $n/2^k$            | $n/2^k$         | $2^k \cdot n/2^k = n$ |
| $\log_2 n$ | $n$      | $1$                | $1$             | $n \cdot 1 = n$       |

### 3.4. Số tầng

$$
\frac{n}{2^k} = 1 \Rightarrow k = \log_2 n
$$

Vậy có $\log_2 n + 1$ tầng.

### 3.5. Tổng

Mỗi tầng chi phí $n$, có $\log_2 n + 1$ tầng:

$$
T(n) = n(\log_2 n + 1) = \Theta(n\log n)
$$

### 3.6. Nhận xét

**Pattern:** Chi phí mỗi tầng **không đổi** ($n$) — đây là **Case 2** của Master Theorem.

> **Lý thuyết bên cạnh:** Khi chi phí mỗi tầng **không đổi**, tổng = (chi phí mỗi tầng) × (số tầng). Đây là trường hợp phổ biến nhất — tương ứng với nhiều thuật toán chia để trị (Merge Sort, Heapify, FFT). Số tầng là $\log_b n$ — hệ số log quyết định bậc.

---

## 4. Ví dụ — Binary Search

### 4.1. Hệ thức

$$
T(n) = T(n/2) + 1, \quad T(1) = 1
$$

### 4.2. Vẽ cây

```
                   1                ← tầng 0: chi phí 1
                   |
                   1                ← tầng 1: chi phí 1
                   |
                   1                ← tầng 2: chi phí 1
                   |
                  ...
                   |
                   1                ← tầng log₂n: chi phí 1
```

**Chỉ có một nhánh** — mỗi tầng một nút.

### 4.3. Chi phí từng tầng

| Tầng $k$   | Số nút   | Chi phí mỗi nút | Tổng tầng |
| ---------- | -------- | --------------- | --------- |
| 0          | $1$      | $1$             | $1$       |
| 1          | $1$      | $1$             | $1$       |
| 2          | $1$      | $1$             | $1$       |
| $\cdots$   | $\cdots$ | $\cdots$        | $\cdots$  |
| $\log_2 n$ | $1$      | $1$             | $1$       |

### 4.4. Tổng

$$
T(n) = \sum_{k=0}^{\log_2 n} 1 = \log_2 n + 1 = \Theta(\log n)
$$

**Pattern:** Mỗi tầng chi phí **không đổi** ($1$), có $\log_2 n$ tầng.

---

## 5. Ví dụ — Tìm max đệ quy

### 5.1. Hệ thức

$$
T(n) = 2T(n/2) + 1, \quad T(1) = 1
$$

### 5.2. Vẽ cây

```
                   1                ← tầng 0: 1 nút, tổng 1
                  / \
                 1   1              ← tầng 1: 2 nút, tổng 2
                / \ / \
               1  1 1  1            ← tầng 2: 4 nút, tổng 4
               ...
              1 1 1 ... 1           ← tầng log₂n: n nút, tổng n
```

### 5.3. Chi phí từng tầng

| Tầng $k$   | Số nút   | Chi phí mỗi nút | Tổng tầng |
| ---------- | -------- | --------------- | --------- |
| 0          | $1$      | $1$             | $1$       |
| 1          | $2$      | $1$             | $2$       |
| 2          | $4$      | $1$             | $4$       |
| $\cdots$   | $\cdots$ | $\cdots$        | $\cdots$  |
| $k$        | $2^k$    | $1$             | $2^k$     |
| $\log_2 n$ | $n$      | $1$             | $n$       |

### 5.4. Tổng

Cấp số nhân:

$$
T(n) = \sum_{k=0}^{\log_2 n} 2^k = 2^{\log_2 n + 1} - 1 = 2n - 1 = \Theta(n)
$$

**Pattern:** Chi phí **tăng gấp đôi** mỗi tầng — tầng lá áp đảo. Đây là **Case 1** của Master Theorem.

### 5.5. Nhận xét quan trọng

Dù đệ quy có **hai nhánh**, tổng chi phí chỉ là $\Theta(n)$ — **không phải** $\Theta(n\log n)$. Lý do: chi phí mỗi nút **giảm** khi xuống sâu (từ $1$ xuống $1$), nhưng số nút **tăng** — tổng là cấp số nhân, áp đảo bởi tầng lá.

**So sánh với Merge Sort:** Cùng $a=2$, $b=2$, nhưng $f(n)$ khác:

- Merge Sort: $f(n) = n$ ⇒ chi phí mỗi tầng **không đổi** ⇒ $\Theta(n\log n)$.
- FindMax: $f(n) = 1$ ⇒ chi phí mỗi tầng **tăng** ⇒ $\Theta(n)$.

**Chỉ khác nhau ở $f(n)$ — kết quả khác bậc hoàn toàn.**

---

## 6. Ví dụ — Strassen's Matrix Multiplication

### 6.1. Hệ thức

$$
T(n) = 7T(n/2) + n^2, \quad T(1) = 1
$$

### 6.2. Vẽ cây

```
                    n²                  ← tầng 0: n²
                 /  |  |  |  |  |  \
               (n/2)² × 7              ← tầng 1: 7 nút, mỗi nút (n/2)²
              /  |  ...  |  \
            ...                      ← tầng 2: 49 nút, mỗi nút (n/4)²
            ...
            1 1 1 ... 1              ← tầng log₂n: 7^log₂n = n^log₂7 nút
```

### 6.3. Chi phí từng tầng

| Tầng $k$ | Số nút   | Kích thước mỗi nút | Chi phí mỗi nút   | Tổng tầng                                            |
| -------- | -------- | ------------------ | ----------------- | ---------------------------------------------------- |
| 0        | $1$      | $n$                | $n^2$             | $n^2$                                                |
| 1        | $7$      | $n/2$              | $(n/2)^2 = n^2/4$ | $7 \cdot n^2/4 = \frac{7}{4}n^2$                     |
| 2        | $49$     | $n/4$              | $n^2/16$          | $49 \cdot n^2/16 = \left(\frac{7}{4}\right)^2 n^2$   |
| $\cdots$ | $\cdots$ | $\cdots$           | $\cdots$          | $\cdots$                                             |
| $k$      | $7^k$    | $n/2^k$            | $n^2/4^k$         | $7^k \cdot n^2/4^k = \left(\frac{7}{4}\right)^k n^2$ |

### 6.4. Số tầng

$$
\frac{n}{2^k} = 1 \Rightarrow k = \log_2 n
$$

### 6.5. Tổng

$$
T(n) = \sum_{k=0}^{\log_2 n} \left(\frac{7}{4}\right)^k n^2 = n^2 \cdot \frac{(7/4)^{\log_2 n + 1} - 1}{7/4 - 1}
$$

Rút gọn (dùng $(7/4)^{\log_2 n} = n^{\log_2 7 - 2}$):

$$
T(n) = \Theta(n^{\log_2 7}) \approx \Theta(n^{2.807})
$$

**Pattern:** Chi phí **tăng** theo $(7/4)^k$ — tầng lá áp đảo. Đây là **Case 1** của Master Theorem.

### 6.6. So sánh với nhân ma trận naive

- Naive: $T(n) = 8T(n/2) + n^2$ ⇒ $\Theta(n^{\log_2 8}) = \Theta(n^3)$.
- Strassen: $T(n) = 7T(n/2) + n^2$ ⇒ $\Theta(n^{\log_2 7}) \approx \Theta(n^{2.807})$.

**Chỉ giảm $a$ từ 8 xuống 7** — nhưng tiết kiệm được một bậc lớn!

> **Lý thuyết bên cạnh:** Strassen là ví dụ kinh điển cho thấy **hằng số $a$ quan trọng** — giảm $a$ từ 8 xuống 7 làm giảm số mũ từ 3 xuống 2.807. Đây là động lực của nghiên cứu nhân ma trận: tìm cách giảm $a$ hơn nữa. Hiện tại, thuật toán tốt nhất (Williams 2012) là $O(n^{2.372})$.

---

## 7. Ví dụ — Trường hợp chi phí giảm (Case 3)

### 7.1. Hệ thức

$$
T(n) = 2T(n/2) + n^2, \quad T(1) = 1
$$

### 7.2. Vẽ cây

```
                    n²                  ← tầng 0: n²
                  /   \
               (n/2)² (n/2)²            ← tầng 1: 2 · (n/2)² = n²/2
               / \    / \
             (n/4)² × 4                ← tầng 2: 4 · (n/4)² = n²/4
             ...
             1 1 ... 1                 ← tầng log₂n: n · 1 = n
```

### 7.3. Chi phí từng tầng

| Tầng $k$ | Số nút   | Chi phí mỗi nút | Tổng tầng |
| -------- | -------- | --------------- | --------- |
| 0        | $1$      | $n^2$           | $n^2$     |
| 1        | $2$      | $n^2/4$         | $n^2/2$   |
| 2        | $4$      | $n^2/16$        | $n^2/4$   |
| $\cdots$ | $\cdots$ | $\cdots$        | $\cdots$  |
| $k$      | $2^k$    | $n^2/4^k$       | $n^2/2^k$ |

### 7.4. Tổng

Chi phí **giảm** theo $n^2/2^k$ — tầng gốc áp đảo:

$$
T(n) = n^2 \sum_{k=0}^{\log_2 n} \frac{1}{2^k} < n^2 \cdot 2 = \Theta(n^2)
$$

**Pattern:** Chi phí **giảm** — tầng gốc áp đảo. Đây là **Case 3** của Master Theorem.

### 7.5. Kiểm tra bằng Master Theorem

$a = 2$, $b = 2$, $x = n^{\log_2 2} = n$, $f(n) = n^2 = \Omega(n^{1+\varepsilon})$ với $\varepsilon = 1$ ⇒ **Case 3**:

$$
T(n) = \Theta(n^2)
$$

✓ Khớp với cây đệ quy.

---

## 8. Ví dụ — Hệ thức không đều (Fibonacci)

### 8.1. Hệ thức

$$
T(n) = T(n-1) + T(n-2) + 1, \quad T(0) = T(1) = 1
$$

### 8.2. Vẽ cây (cho $n = 5$)

```
                          T(5)
                        /      \
                    T(4)        T(3)
                   /   \        /   \
                T(3)  T(2)   T(2)  T(1)
               /  \   /  \   /  \
            T(2) T(1) T(1) T(0) T(1) T(0)
            / \
          T(1) T(0)
```

### 8.3. Nhận xét

- Cây **không đều** — các nhánh có độ sâu khác nhau.
- Số nút xấp xỉ $F_n$ (số Fibonacci thứ $n$).
- Công thức Binet: $F_n = \Theta(\varphi^n)$ với $\varphi = \frac{1+\sqrt{5}}{2} \approx 1.618$.

### 8.4. Chứng minh bằng phương trình đặc trưng

Giả sử $T(n) = c^n$. Thay vào:

$$
c^n = c^{n-1} + c^{n-2} \Rightarrow c^2 = c + 1 \Rightarrow c = \frac{1 \pm \sqrt{5}}{2}
$$

Nghiệm dương lớn nhất: $c = \varphi$. Vậy:

$$
T(n) = \Theta(\varphi^n) \approx \Theta(1.618^n)
$$

### 8.5. So sánh với $2^n$

$\varphi^n$ **nhỏ hơn** $2^n$ rất nhiều:

| $n$ | $\varphi^n$ | $2^n$          |
| --- | ----------- | -------------- |
| 10  | ~123        | 1024           |
| 20  | ~15,127     | ~1,048,576     |
| 30  | ~1,860,498  | ~1,073,741,824 |

**Kết luận:** Fibonacci naive là $\Theta(\varphi^n)$ — **không phải** $\Theta(2^n)$. Viết $O(2^n)$ là cận trên **lỏng**, đúng nhưng không chặt.

> **Lý thuyết bên cạnh:** Với hệ thức **không đều** (các nhánh khác kích thước), cây đệ quy vẫn vẽ được nhưng **khó tính tổng chính xác**. Cách tốt hơn: dùng **phương trình đặc trưng** (cho hệ thức tuyến tính thuần nhất) hoặc **chặn trên/dưới** (như $T(n) \le 2T(n-1) + 1$ ⇒ $O(2^n)$).

---

## 9. Ví dụ — Hệ thức với $f(n) = n\log n$

### 9.1. Hệ thức

$$
T(n) = 2T(n/2) + n\log n, \quad T(1) = 1
$$

**Đây là trường hợp Master Theorem không áp dụng** (vì $n\log n$ không so sánh được với $n^{1\pm\varepsilon}$).

### 9.2. Vẽ cây

| Tầng $k$ | Số nút   | Kích thước mỗi nút | Chi phí mỗi nút      | Tổng tầng      |
| -------- | -------- | ------------------ | -------------------- | -------------- |
| 0        | $1$      | $n$                | $n\log n$            | $n\log n$      |
| 1        | $2$      | $n/2$              | $(n/2)\log(n/2)$     | $n\log(n/2)$   |
| 2        | $4$      | $n/4$              | $(n/4)\log(n/4)$     | $n\log(n/4)$   |
| $\cdots$ | $\cdots$ | $\cdots$           | $\cdots$             | $\cdots$       |
| $k$      | $2^k$    | $n/2^k$            | $(n/2^k)\log(n/2^k)$ | $n\log(n/2^k)$ |

### 9.3. Đơn giản hóa

$$
\log(n/2^k) = \log n - k
$$

Vậy tổng tầng $k$:

$$
n(\log n - k)
$$

### 9.4. Tổng tất cả các tầng

$$
T(n) = \sum_{k=0}^{\log n} n(\log n - k) = n \sum_{j=0}^{\log n} j \quad \text{(đặt } j = \log n - k\text{)}
$$

$$
= n \cdot \frac{\log n(\log n + 1)}{2} = \Theta(n\log^2 n)
$$

### 9.5. Kiểm tra bằng phương pháp thế

**Đoán:** $T(n) = O(n\log^2 n)$.

**Giả thiết:** $T(k) \le c\,k\log^2 k$ với $k < n$.

$$
T(n) = 2T(n/2) + n\log n \le 2 \cdot c(n/2)\log^2(n/2) + n\log n
$$

$$
= cn(\log n - 1)^2 + n\log n = cn(\log^2 n - 2\log n + 1) + n\log n
$$

$$
= cn\log^2 n - 2cn\log n + cn + n\log n
$$

Để $\le cn\log^2 n$, cần:

$$
-2cn\log n + cn + n\log n \le 0 \iff n\log n(1 - 2c) + cn \le 0
$$

Với $c \ge 1$: $1 - 2c \le -1$, và $cn > 0$ — bất đẳng thức đúng khi $n$ đủ lớn.

**Kết luận:** $T(n) = O(n\log^2 n)$. Tương tự chứng minh $\Omega$ ⇒ $\Theta(n\log^2 n)$. $\blacksquare$

### 9.6. Nhận xét

Dù $f(n) = n\log n$ "gần" $n$, kết quả là $\Theta(n\log^2 n)$ — **không phải** $\Theta(n\log n)$. Đây là trường hợp **giữa Case 2 và Case 3** — Master Theorem không áp dụng, phải dùng cây đệ quy.

> **Lý thuyết bên cạnh:** Khi $f(n)$ nằm **giữa** hai case của Master Theorem, kết quả thường có thêm hệ số $\log$. Quy tắc ngón tay: nếu $f(n) = n^{\log_b a} \cdot \log^k n$, thì $T(n) = \Theta(n^{\log_b a} \cdot \log^{k+1} n)$. Đây là **Case 2 mở rộng** — không có trong Master Theorem chuẩn nhưng rất phổ biến.

---

## 10. Ví dụ — Hệ thức không đều (uneven split)

### 10.1. Hệ thức

$$
T(n) = T(n/3) + T(2n/3) + n, \quad T(1) = 1
$$

**Đây là Quick Sort worst-case** (pivot chia 1:2) — hoặc Merge Sort với chia không đều.

### 10.2. Vẽ cây

```
                        n                    ← tầng 0: n
                      /   \
                   n/3    2n/3               ← tầng 1: n/3 + 2n/3 = n
                  / \     /   \
               n/9 2n/9 2n/9 4n/9            ← tầng 2: tổng = n
               ...
```

### 10.3. Chi phí từng tầng

**Quan sát quan trọng:** Mỗi tầng, tổng kích thước các nút = $n$ (vì $n/3 + 2n/3 = n$, và tương tự cho các tầng sau).

| Tầng $k$ | Tổng kích thước                | Chi phí tầng |
| -------- | ------------------------------ | ------------ |
| 0        | $n$                            | $n$          |
| 1        | $n/3 + 2n/3 = n$               | $n$          |
| 2        | $n/9 + 2n/9 + 2n/9 + 4n/9 = n$ | $n$          |
| $\cdots$ | $\cdots$                       | $\cdots$     |

**Mỗi tầng chi phí $n$** — giống Merge Sort!

### 10.4. Số tầng

**Nhánh ngắn nhất:** $n/3$ — độ sâu $\log_3 n$.
**Nhánh dài nhất:** $2n/3$ — độ sâu $\log_{3/2} n$.

Cây **không đều** — các lá ở độ sâu khác nhau. Nhưng:

- Tầng từ $0$ đến $\log_3 n$: **đầy đủ**, mỗi tầng $n$.
- Tầng từ $\log_3 n$ đến $\log_{3/2} n$: **không đầy đủ**, chi phí $\le n$.

### 10.5. Chặn trên và chặn dưới

**Chặn dưới:** Ít nhất $\log_3 n$ tầng đầy đủ:

$$
T(n) \ge n \log_3 n = \Omega(n\log n)
$$

**Chặn trên:** Nhiều nhất $\log_{3/2} n$ tầng, mỗi tầng $\le n$:

$$
T(n) \le n \log_{3/2} n = O(n\log n)
$$

**Kết luận:**

$$
T(n) = \Theta(n\log n)
$$

### 10.6. Nhận xét

**Bất ngờ:** Dù chia **không đều** (1:2 thay vì 1:1), kết quả vẫn $\Theta(n\log n)$ — **chỉ khác hằng số** (cơ số log). Đây là lý do Quick Sort average case vẫn $\Theta(n\log n)$ dù pivot không hoàn hảo.

**Tổng quát:** Nếu $T(n) = T(\alpha n) + T((1-\alpha)n) + n$ với $0 < \alpha \le 1/2$, thì $T(n) = \Theta(n\log n)$ — **bất kể $\alpha$**. Chỉ khi $\alpha \to 0$ (chia cực không đều) mới tệ hơn.

> **Lý thuyết bên cạnh:** Đây là **định lý quan trọng** — giải thích tại sao Quick Sort với pivot ngẫu nhiên vẫn nhanh: xác suất pivot rơi vào giữa (chia đều) đủ cao để average case là $\Theta(n\log n)$. Chỉ worst case (pivot luôn ở biên) mới $\Theta(n^2)$.

---

## 11. Ví dụ — Hệ thức với $\sqrt{n}$

### 11.1. Hệ thức

$$
T(n) = 2T(\sqrt{n}) + \log n
$$

**Đây là dạng đặc biệt** — kích thước giảm theo căn bậc hai, không phải chia hằng số.

### 11.2. Đổi biến

Đặt $m = \log n$ ⇒ $n = 2^m$. Thay vào:

$$
T(2^m) = 2T(2^{m/2}) + m
$$

Đặt $S(m) = T(2^m)$:

$$
S(m) = 2S(m/2) + m
$$

**Đây là dạng chuẩn!** Master Theorem: $a=2$, $b=2$, $f(m)=m$, $x = m^{\log_2 2} = m$, $f(m) = \Theta(x)$ ⇒ **Case 2**:

$$
S(m) = \Theta(m\log m)
$$

Quay lại:

$$
T(n) = S(\log n) = \Theta(\log n \cdot \log\log n)
$$

### 11.3. Nhận xét

**Kỹ thuật đổi biến** rất hữu ích khi kích thước giảm **không theo hằng số**. Đây là trường hợp phổ biến trong các thuật toán số học (FFT, tính gcd nhanh).

> **Lý thuyết bên cạnh:** Khi hệ thức có dạng $T(n) = aT(n^c) + f(n)$ với $0 < c < 1$, đổi biến $m = \log n$ đưa về dạng chuẩn $S(m) = aS(cm) + f(2^m)$. Sau đó áp dụng Master Theorem cho $S$. Đây là **kỹ thuật chuẩn** cho hệ thức "siêu hình học".

---

## 12. Bảng tổng hợp các pattern

### 12.1. Ba pattern chính

| Pattern       | Chi phí mỗi tầng | Tầng áp đảo | Kết quả                                     |
| ------------- | ---------------- | ----------- | ------------------------------------------- |
| **Tăng**      | Tăng theo $k$    | Tầng lá     | $\Theta(\text{chi phí lá})$                 |
| **Không đổi** | Không đổi        | Mọi tầng    | $\Theta(\text{chi phí tầng} \times \log n)$ |
| **Giảm**      | Giảm theo $k$    | Tầng gốc    | $\Theta(\text{chi phí gốc})$                |

### 12.2. Ví dụ cho từng pattern

| Hệ thức         | Pattern   | $T(n)$                 |
| --------------- | --------- | ---------------------- |
| $2T(n/2) + 1$   | Tăng      | $\Theta(n)$            |
| $2T(n/2) + n$   | Không đổi | $\Theta(n\log n)$      |
| $2T(n/2) + n^2$ | Giảm      | $\Theta(n^2)$          |
| $T(n/2) + 1$    | Không đổi | $\Theta(\log n)$       |
| $3T(n/2) + n$   | Tăng      | $\Theta(n^{\log_2 3})$ |
| $4T(n/2) + n^2$ | Không đổi | $\Theta(n^2\log n)$    |
| $4T(n/2) + n^3$ | Giảm      | $\Theta(n^3)$          |

### 12.3. Liên hệ với Master Theorem

| Pattern cây                        | Master Theorem                                          |
| ---------------------------------- | ------------------------------------------------------- |
| Chi phí **tăng** (tầng lá áp đảo)  | **Case 1:** $f(n) = O(n^{\log_b a - \varepsilon})$      |
| Chi phí **không đổi**              | **Case 2:** $f(n) = \Theta(n^{\log_b a})$               |
| Chi phí **giảm** (tầng gốc áp đảo) | **Case 3:** $f(n) = \Omega(n^{\log_b a + \varepsilon})$ |

**Đây là cơ sở trực quan của Master Theorem** — ba case tương ứng với ba pattern của cây đệ quy.

> **Lý thuyết bên cạnh:** Master Theorem có thể được **chứng minh** hoàn toàn bằng cây đệ quy. Mỗi case tương ứng với việc tầng nào áp đảo trong tổng. Đây là cách hiểu sâu nhất — không chỉ nhớ công thức mà hiểu **tại sao** nó đúng.

---

## 13. Kỹ thuật nâng cao — "Unrolling" và "Telescoping"

### 13.1. Unrolling (mở rộng)

Khi cây đệ quy **một nhánh** (như $T(n) = T(n-1) + f(n)$), có thể "mở rộng" trực tiếp:

$$
T(n) = f(n) + f(n-1) + f(n-2) + \cdots + f(1)
$$

**Ví dụ:** $T(n) = T(n-1) + n$

$$
T(n) = n + (n-1) + (n-2) + \cdots + 1 = \frac{n(n+1)}{2} = \Theta(n^2)
$$

### 13.2. Telescoping (thu gọn)

Khi hệ thức có dạng đặc biệt, có thể "thu gọn" bằng cách trừ hai phương trình.

**Ví dụ:** $T(n) = 2T(n/2) + n$

Chia hai vế cho $n$:

$$
\frac{T(n)}{n} = \frac{2T(n/2)}{n} + 1 = \frac{T(n/2)}{n/2} + 1
$$

Đặt $S(n) = T(n)/n$:

$$
S(n) = S(n/2) + 1 \Rightarrow S(n) = \Theta(\log n) \Rightarrow T(n) = \Theta(n\log n)
$$

**Kỹ thuật này gọn hơn cây đệ quy** — nhưng chỉ áp dụng khi hệ thức có cấu trúc phù hợp.

> **Lý thuyết bên cạnh:** Telescoping là **phương pháp đại số** — biến đổi hệ thức thành dạng dễ giải. Nó thường dùng khi cây đệ quy quá phức tạp hoặc khi cần chứng minh chặt chẽ. Kết hợp với đổi biến, đây là công cụ mạnh cho hệ thức phức tạp.

---

## 14. Những bẫy phổ biến

### Bẫy 1 — "Cây đệ quy là chứng minh"

**Sai.** Cây đệ quy chỉ để **đoán**. Cần chứng minh bằng phương pháp thế (quy nạp). Trong bài thi, nếu chỉ vẽ cây mà không chứng minh, có thể mất điểm.

### Bẫy 2 — "Mọi tầng đều có chi phí $n$"

**Sai.** Chỉ đúng khi $f(n) = n$ và $a/b = 1$ (như Merge Sort). Với $f(n) = 1$, chi phí mỗi tầng **tăng**; với $f(n) = n^2$, chi phí **giảm**.

### Bẫy 3 — "Số tầng luôn là $\log_2 n$"

**Sai.** Phụ thuộc **hệ số chia** $b$:

- Chia đôi: $\log_2 n$.
- Chia ba: $\log_3 n$.
- Chia không đều: $\log_{1/\alpha} n$ (với $\alpha$ là tỉ lệ nhỏ nhất).

### Bẫy 4 — "Cây không đều không tính được"

**Sai.** Có thể **chặn trên và chặn dưới**:

- Chặn dưới: dùng nhánh **ngắn nhất** (số tầng ít nhất).
- Chặn trên: dùng nhánh **dài nhất** (số tầng nhiều nhất).

Nếu hai chặn **cùng bậc** ⇒ kết quả chính xác.

### Bẫy 5 — "Fibonacci là $O(2^n)$ nên cận chặt là $2^n$"

**Sai.** $O(2^n)$ là cận trên **lỏng**. Cận chặt là $\Theta(\varphi^n) \approx \Theta(1.618^n)$.

### Bẫy 6 — "Mỗi tầng có cùng số nút"

**Sai.** Số nút tăng theo $a^k$ (nếu $a > 1$) hoặc giảm (nếu $a < 1$). Với $a = 1$, số nút không đổi.

### Bẫy 7 — "Cây đệ quy không áp dụng cho hệ thức trừ"

**Sai.** Vẫn vẽ được — nhưng cây **không cân bằng** (nhánh dài $n$ tầng). Kết quả thường là hàm mũ hoặc đa thức.

**Ví dụ:** $T(n) = 2T(n-1) + 1$ — cây có $2^n$ nút ⇒ $\Theta(2^n)$.

### Bẫy 8 — "Bỏ qua chi phí ở lá"

**Sai.** Lá có chi phí $T(1) = \Theta(1)$ — nhưng **số lá** có thể lớn ($a^{\log_b n} = n^{\log_b a}$). Tổng chi phí lá = $n^{\log_b a} \cdot \Theta(1)$ — có thể áp đảo.

### Bẫy 9 — "Chỉ cần tính tầng cuối"

**Sai.** Phải cộng **tất cả** các tầng. Chỉ khi biết tầng nào áp đảo mới có thể bỏ qua các tầng khác (nhưng phải chứng minh).

### Bẫy 10 — "Cây đệ quy cho kết quả chính xác về hằng số"

**Sai.** Cây đệ quy cho **bậc** ($\Theta$) — không cho hằng số. Nếu cần hằng số (so sánh hai thuật toán cùng bậc), phải dùng phương pháp thế với hằng số cụ thể.

> **Lý thuyết bên cạnh:** Hầu hết bẫy đến từ việc **quên rằng cây đệ quy là công cụ đoán**, không phải chứng minh. Kết quả từ cây đệ quy **luôn** cần được xác nhận bằng quy nạp — trừ khi chỉ cần đoán để dùng Master Theorem.

---

## 15. So sánh với các phương pháp khác

### 15.1. Bảng so sánh

| Tiêu chí         | Cây đệ quy  | Phương pháp thế | Master Theorem       |
| ---------------- | ----------- | --------------- | -------------------- |
| **Mục đích**     | Đoán nghiệm | Chứng minh      | Công thức đóng       |
| **Chặt chẽ**     | ❌           | ✅               | ✅                    |
| **Độ khó**       | Dễ          | Trung bình      | Dễ (nếu đúng dạng)   |
| **Áp dụng**      | Mọi hệ thức | Mọi hệ thức     | Chỉ $aT(n/b) + f(n)$ |
| **Trực quan**    | ✅✅✅         | ❌               | ✅                    |
| **Khi nào dùng** | Bước đầu    | Sau khi đoán    | Khi đúng dạng        |

### 15.2. Chiến lược kết hợp

1. **Kiểm tra Master Theorem trước** — nếu đúng dạng, dùng luôn.
2. **Nếu không:** vẽ cây đệ quy để đoán nghiệm.
3. **Chứng minh** bằng phương pháp thế.
4. **Kiểm tra** bằng cách thử $n$ nhỏ.

### 15.3. Ví dụ — cùng một bài toán, ba cách tiếp cận

**Hệ thức:** $T(n) = 2T(n/2) + n$.

**Cách 1 — Master Theorem:** $a=2$, $b=2$, $x=n$, $f(n)=\Theta(x)$ ⇒ Case 2 ⇒ $\Theta(n\log n)$.

**Cách 2 — Cây đệ quy:** Mỗi tầng $n$, có $\log n$ tầng ⇒ $\Theta(n\log n)$.

**Cách 3 — Phương pháp thế:** Đoán $T(n) = O(n\log n)$, chứng minh bằng quy nạp (đã làm ở Ví dụ 10).

**Cả ba đều cho cùng kết quả** — nhưng mỗi cách có ưu điểm riêng.

---

## 16. Bài tập tự luyện

**Bài 1:** Vẽ cây đệ quy và giải $T(n) = 3T(n/2) + n$.

*Hướng:* Cây có $3^k$ nút tầng $k$, chi phí mỗi nút $n/2^k$, tổng tầng $n(3/2)^k$. Tăng — tầng lá áp đảo. Kết quả: $\Theta(n^{\log_2 3}) \approx \Theta(n^{1.585})$.

**Bài 2:** Vẽ cây đệ quy và giải $T(n) = T(n/3) + T(2n/3) + n$.

*Hướng:* Mỗi tầng chi phí $n$, số tầng từ $\log_3 n$ đến $\log_{3/2} n$. Kết quả: $\Theta(n\log n)$.

**Bài 3:** Vẽ cây đệ quy và giải $T(n) = 4T(n/2) + n^2$.

*Hướng:* Tầng $k$: $4^k$ nút, chi phí $n^2/4^k$, tổng $n^2$. Không đổi. Kết quả: $\Theta(n^2\log n)$.

**Bài 4:** Vẽ cây đệ quy và giải $T(n) = T(n-1) + n$.

*Hướng:* Cây một nhánh, tổng $1 + 2 + \cdots + n = \Theta(n^2)$.

**Bài 5:** Vẽ cây đệ quy và giải $T(n) = 2T(n-1) + 1$.

*Hướng:* Cây có $2^n$ nút, mỗi nút chi phí 1. Kết quả: $\Theta(2^n)$.

**Bài 6:** Giải $T(n) = T(n/2) + n$ bằng cây đệ quy.

*Hướng:* Một nhánh, chi phí giảm: $n + n/2 + n/4 + \cdots < 2n$. Kết quả: $\Theta(n)$.

**Bài 7:** Giải $T(n) = 2T(\sqrt{n}) + \log n$ bằng đổi biến.

*Hướng:* Đặt $m = \log n$, $S(m) = T(2^m)$ ⇒ $S(m) = 2S(m/2) + m$ ⇒ $S(m) = \Theta(m\log m)$ ⇒ $T(n) = \Theta(\log n \cdot \log\log n)$.

**Bài 8:** Giải $T(n) = 2T(n/2) + n\log n$ bằng cây đệ quy.

*Hướng:* Tầng $k$: $n(\log n - k)$, tổng $\Theta(n\log^2 n)$.

**Bài 9:** Tại sao cây đệ quy không đủ để chứng minh?

*Hướng:* Vì nó chỉ đoán nghiệm, không xét mọi trường hợp. Cần quy nạp để chứng minh với mọi $n$.

**Bài 10:** Vẽ cây đệ quy cho $T(n) = 3T(n/3) + n^2$ và giải.

*Hướng:* Tầng $k$: $3^k$ nút, chi phí $n^2/9^k$, tổng $n^2(1/3)^k$. Giảm — tầng gốc áp đảo. Kết quả: $\Theta(n^2)$.

---

## 17. Tóm tắt một trang

### Định nghĩa

**Cây đệ quy** là đồ thị biểu diễn các lời gọi đệ quy — mỗi nút là một lời gọi, chi phí nút là chi phí chia/trộn.

### Quy trình 5 bước

1. **Viết hệ thức** từ code.
2. **Vẽ cây** — gốc $n$, con $n/b$, chi phí $f(n/b^k)$.
3. **Tính chi phí từng tầng** — $a^k \cdot f(n/b^k)$.
4. **Xác định số tầng** — $\log_b n$.
5. **Cộng tất cả các tầng** — tìm tầng áp đảo.

### Ba pattern chính

| Pattern       | Chi phí tầng        | Tầng áp đảo | Kết quả              | Master Theorem |
| ------------- | ------------------- | ----------- | -------------------- | -------------- |
| **Tăng**      | $a^k f(n/b^k)$ tăng | Lá          | $\Theta(\text{lá})$  | Case 1         |
| **Không đổi** | $= f(n)$            | Mọi tầng    | $\Theta(f(n)\log n)$ | Case 2         |
| **Giảm**      | giảm                | Gốc         | $\Theta(f(n))$       | Case 3         |

### Công thức cây đệ quy

$$
T(n) = \sum_{k=0}^{\log_b n} a^k \cdot f\!\left(\frac{n}{b^k}\right)
$$

### Các pattern phổ biến

| Hệ thức                | $T(n)$                 |
| ---------------------- | ---------------------- |
| $2T(n/2) + n$          | $\Theta(n\log n)$      |
| $2T(n/2) + 1$          | $\Theta(n)$            |
| $2T(n/2) + n^2$        | $\Theta(n^2)$          |
| $T(n/2) + 1$           | $\Theta(\log n)$       |
| $3T(n/2) + n$          | $\Theta(n^{\log_2 3})$ |
| $7T(n/2) + n^2$        | $\Theta(n^{\log_2 7})$ |
| $T(n/3) + T(2n/3) + n$ | $\Theta(n\log n)$      |
| $T(n-1) + n$           | $\Theta(n^2)$          |
| $2T(n-1) + 1$          | $\Theta(2^n)$          |
| $T(n-1) + T(n-2)$      | $\Theta(\varphi^n)$    |
| $2T(n/2) + n\log n$    | $\Theta(n\log^2 n)$    |

### Kỹ thuật đặc biệt

- **Đổi biến:** $T(n) = aT(n^c) + f(n)$ → đặt $m = \log n$.
- **Unrolling:** $T(n) = T(n-1) + f(n)$ → tổng $f$.
- **Telescoping:** chia hai vế cho $n$ hoặc hàm phù hợp.

### Bẫy cần tránh

1. Cây đệ quy **không phải** chứng minh — cần quy nạp.
2. Mỗi tầng **không luôn** chi phí $n$.
3. Số tầng phụ thuộc $b$ — **không luôn** $\log_2 n$.
4. Fibonacci là $\Theta(\varphi^n)$ — **không phải** $\Theta(2^n)$.
5. Cây không đều vẫn giải được — dùng chặn trên/dưới.
6. **Không bỏ qua** chi phí lá.

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Chứng minh Master Theorem** hoàn toàn bằng cây đệ quy (cả ba case).
- **Phương pháp thế** chi tiết với các kỹ thuật nâng cao (trừ hằng số, đổi biến).
- **Akra–Bazzi** — mở rộng cho hệ thức không đều.
- **Bài tập nâng cao** với hệ thức phức tạp (ví dụ $T(n) = T(n/2) + T(n/3) + T(n/6) + n$, $T(n) = T(\sqrt{n}) + 1$).
- **Ứng dụng** cây đệ quy trong phân tích thuật toán thực tế (FFT, Strassen, Karatsuba).