# Cận trên bằng phương pháp cây đệ quy (Upper Bounds Using Recursion Tree Method)

## 1. Mục tiêu: chứng minh $T(n) = O(g(n))$

Khác với việc **giải chính xác** hệ thức (tìm $\Theta$), khi chỉ cần **cận trên** ta có một lợi thế lớn:

> **Được phép "phóng đại" (overestimate) chi phí ở mỗi bước** — miễn là không vi phạm tính đúng đắn của bất đẳng thức.

Cụ thể, nếu ta chứng minh được:

$$
T(n) \le \text{(tổng chi phí cây phóng đại)} \le c\,g(n)
$$

thì kết luận $T(n) = O(g(n))$ là hợp lệ — dù cây phóng đại có thể **lỏng hơn** cây thật.

### Lợi ích của việc phóng đại

| Kỹ thuật phóng đại | Tác dụng |
|---|---|
| Thay $\lceil n/2 \rceil$ hoặc $\lfloor n/2 \rfloor$ bằng $n/2$ | Bỏ được floor/ceil |
| Thay $n/3$ bằng $n$ (trong nhánh nhỏ) | Cây trở nên **đều** hơn |
| Kéo dài cây đến độ sâu lớn hơn | Bỏ qua điều kiện dừng phức tạp |
| Chặn $f(n)$ bằng hàm đơn giản hơn | Dễ tính tổng hơn |

**Nguyên tắc vàng:** Phóng đại **phải có kiểm soát** — để tổng vẫn là $O(g(n))$. Nếu phóng đại quá tay (ví dụ thay mọi $f(n/b)$ bằng $f(n)$), cây có thể cho cận trên **sai** (quá lỏng, thậm chí vô hạn).

> **Lý thuyết bên cạnh:** Phương pháp cây đệ quy cho **cận trên** thường dễ hơn tìm **cận chặt** ($\Theta$), vì ta không cần lo lắng về hằng số tối ưu. Tuy nhiên, ta **vẫn phải chứng minh** kết quả bằng quy nạp (phương pháp thế) — cây chỉ cho **dự đoán**. Điểm khác biệt so với chứng minh $\Theta$: khi chứng minh $O$, ta có thể **nới lỏng** giả thiết quy nạp (ví dụ $T(k) \le c\,k\log k$ thay vì đúng bằng).

---

## 2. Quy trình 5 bước (dành riêng cho cận trên)

### Bước 1 — Viết hệ thức

$$
T(n) = a\,T(n/b) + f(n), \quad T(1) = \Theta(1)
$$

### Bước 2 — Vẽ cây phóng đại

- **Gốc:** kích thước $n$, chi phí $f(n)$.
- **Mỗi nút:** vẽ $a$ con, kích thước **xấp xỉ** $n/b$ (bỏ floor/ceil).
- **Nếu nhánh không đều** (ví dụ $T(n/3) + T(2n/3)$): **phóng đại** nhánh nhỏ lên bằng nhánh lớn — cây trở nên đều.
- **Tiếp tục** đến khi kích thước = 1 (hoặc dừng sớm nếu đủ).

### Bước 3 — Tính chi phí từng tầng

Tầng $k$: $a^k$ nút, mỗi nút chi phí $\le f(n/b^k)$.

$$
\text{Chi phí tầng } k \le a^k \cdot f\!\left(\frac{n}{b^k}\right)
$$

**Chú ý dấu $\le$** — đây là chỗ phóng đại phát huy tác dụng.

### Bước 4 — Xác định tầng áp đảo

Có ba khả năng:

| Pattern | Chi phí tầng | Tầng áp đảo | Dự đoán |
|---|---|---|---|
| **Tăng** | $a^k f(n/b^k)$ tăng theo $k$ | Lá | $O(n^{\log_b a})$ |
| **Không đổi** | $\approx f(n)$ | Mọi tầng | $O(f(n)\log_b n)$ |
| **Giảm** | Giảm theo $k$ | Gốc | $O(f(n))$ |

### Bước 5 — Chứng minh bằng quy nạp

Đây là bước **bắt buộc** để biến dự đoán thành chứng minh:

1. **Giả thiết quy nạp:** $T(k) \le c\,g(k)$ với mọi $k < n$.
2. **Chứng minh:** $T(n) \le c\,g(n)$.
3. **Tìm $c$ và $n_0$** — có thể dùng kỹ thuật "trừ hằng số" nếu cần.

> **Lý thuyết bên cạnh:** Trong nhiều tài liệu, bước 5 (chứng minh quy nạp) được lược bớt — chỉ trình bày cây. Nhưng về mặt **toán học**, cây đệ quy chỉ là **công cụ khám phá** (heuristic). Muốn có chứng minh **chặt chẽ**, phải quy nạp. Trong bài thi, nếu không có bước 5, có thể bị trừ điểm.

---

## 3. Kỹ thuật phóng đại cốt lõi

### 3.1. Bỏ floor/ceil

Nếu $T(n) = 2T(\lceil n/2 \rceil) + n$, ta có:

$$
T(n) \le 2T(\lceil n/2 \rceil) + n \le 2T(n/2 + 1) + n
$$

Để đơn giản, phóng đại thêm: $n/2 + 1 \le n$ (khi $n \ge 2$). Nhưng cách này **quá lỏng** — cây có thể không thu nhỏ.

**Cách đúng:** Đặt $n' = 2^{\lceil \log_2 n \rceil}$ (làm tròn lên lũy thừa 2 gần nhất). Khi đó $n' \le 2n$, và:

$$
T(n) \le T(n') = O(n'\log n') = O(n\log n)
$$

### 3.2. Phóng đại nhánh nhỏ (cho cây không đều)

Với $T(n) = T(n/3) + T(2n/3) + n$:

- **Cây thật:** nhánh trái $n/3$ (sâu $\log_3 n$), nhánh phải $2n/3$ (sâu $\log_{3/2} n$).
- **Phóng đại:** thay $n/3$ bằng $2n/3$ — cây trở thành **đều** với mỗi nút có hai con $2n/3$.

Khi đó:

$$
T(n) \le 2T(2n/3) + n
$$

Cây mới có chi phí mỗi tầng $\le n$ (phóng đại), số tầng $\log_{3/2} n$. Vậy:

$$
T(n) \le n \log_{3/2} n = O(n\log n)
$$

**Chú ý:** Cây phóng đại cho **cận trên đúng bậc** — dù cây thật không đều. Đây là sức mạnh của phóng đại có kiểm soát.

### 3.3. Kéo dài cây (dừng muộn hơn)

Nếu cây thật dừng khi kích thước = 1, ta có thể **kéo dài** đến khi kích thước = 0 (hoặc âm) — miễn là số tầng chỉ tăng thêm hằng số.

**Ví dụ:** $T(n) = T(n/2) + 1$ — cây thật dừng ở $T(1) = 1$. Ta phóng đại: cây dừng ở $T(0) = 1$ (thêm 1 tầng). Số tầng tăng từ $\log_2 n$ lên $\log_2 n + 1$ — không đổi bậc.

### 3.4. Chặn $f(n)$ bằng hàm đơn giản hơn

Nếu $f(n) = n\log n + n$, ta chặn $f(n) \le 2n\log n$ (với $n \ge 2$). Khi đó cây dùng $f(n) \le 2n\log n$ — đơn giản hơn, vẫn đúng bậc.

> **Lý thuyết bên cạnh:** Ba kỹ thuật phóng đại trên tương ứng với ba "nút thắt" trong phân tích: (1) floor/ceil gây khó khăn cho việc xác định độ sâu; (2) nhánh không đều gây khó khăn cho việc tính tổng; (3) $f(n)$ phức tạp gây khó khăn cho việc tính chi phí mỗi nút. Phóng đại giải quyết cả ba — với điều kiện không làm mất bậc.

---

## 4. Ví dụ 1 — $T(n) = 3T(n/4) + \Theta(n^2)$

### 4.1. Vẽ cây

```
                        n²                  ← tầng 0: n²
                    /   |   \
              (n/4)² (n/4)² (n/4)²         ← tầng 1: 3·(n/4)² = 3n²/16
              /|\    /|\    /|\
             ...                        ← tầng 2: 9·(n/16)² = 9n²/256
             ...
             1 1 ... 1                  ← tầng log₄n: 3^log₄n = n^log₄3 nút
```

### 4.2. Chi phí từng tầng

| Tầng $k$ | Số nút | Chi phí mỗi nút | Tổng tầng |
|---|---|---|---|
| 0 | $1$ | $n^2$ | $n^2$ |
| 1 | $3$ | $n^2/16$ | $\frac{3}{16}n^2$ |
| 2 | $9$ | $n^2/256$ | $\left(\frac{3}{16}\right)^2 n^2$ |
| $\cdots$ | $\cdots$ | $\cdots$ | $\cdots$ |
| $k$ | $3^k$ | $n^2/16^k$ | $\left(\frac{3}{16}\right)^k n^2$ |

### 4.3. Tổng

Vì $3/16 < 1$, tổng là **chuỗi hình học hội tụ**:

$$
T(n) \le n^2 \sum_{k=0}^{\infty} \left(\frac{3}{16}\right)^k = n^2 \cdot \frac{1}{1 - 3/16} = \frac{16}{13}n^2
$$

### 4.4. Kết luận

$$
T(n) = O(n^2)
$$

### 4.5. Chứng minh bằng quy nạp

**Giả thiết:** $T(k) \le c\,k^2$ với mọi $k < n$.

**Chứng minh:**

$$
T(n) = 3T(n/4) + n^2 \le 3c(n/4)^2 + n^2 = \frac{3c}{16}n^2 + n^2
$$

Để $\le c\,n^2$, cần:

$$
\frac{3c}{16} + 1 \le c \iff 1 \le c\left(1 - \frac{3}{16}\right) = \frac{13c}{16} \iff c \ge \frac{16}{13}
$$

**Chọn $c = 2$** (thoải mái). Base case: $T(1) \le 2 \cdot 1^2 = 2$ — chọn $T(1) = 1$ thì thỏa.

**Kết luận:** $T(n) \le 2n^2$ với mọi $n \ge 1$ ⇒ $T(n) = O(n^2)$. $\blacksquare$

**Nhận xét:** Hằng số $c = 2$ **khớp** với cận từ cây ($16/13 \approx 1.23$). Đây là điểm hay — cây cho **dự đoán sát**.

---

## 5. Ví dụ 2 — $T(n) = T(n/3) + T(2n/3) + O(n)$ (cây không đều)

### 5.1. Phân tích cây thật

```
                        n                    ← tầng 0: n
                      /   \
                   n/3    2n/3               ← tầng 1: n/3 + 2n/3 = n
                  / \     /   \
               n/9 2n/9 2n/9 4n/9            ← tầng 2: tổng = n
               ...
```

**Quan sát:** Mỗi tầng, **tổng kích thước** các nút = $n$. Nhưng cây **không đều**:

- Nhánh trái sâu: $\log_3 n$.
- Nhánh phải sâu: $\log_{3/2} n$.

### 5.2. Phóng đại — cây đều

Thay nhánh $n/3$ bằng $2n/3$ (phóng đại nhánh nhỏ):

$$
T(n) \le 2T(2n/3) + n
$$

Cây mới **đều**: mỗi nút có hai con kích thước $2n/3$.

**Chi phí mỗi tầng:**

- Tầng $k$: $2^k$ nút, mỗi nút kích thước $n(2/3)^k$.
- Chi phí mỗi nút: $n(2/3)^k$ (vì $f(n) = n$).
- Tổng tầng: $2^k \cdot n(2/3)^k = n \cdot (4/3)^k$.

**Chú ý:** Tổng tầng **tăng** theo $(4/3)^k$ — tầng lá áp đảo!

**Số tầng:** $n(2/3)^k = 1$ ⇒ $k = \log_{3/2} n$.

**Tổng (cây phóng đại):**

$$
T(n) \le \sum_{k=0}^{\log_{3/2} n} n\left(\frac{4}{3}\right)^k = n \cdot \frac{(4/3)^{\log_{3/2} n + 1} - 1}{4/3 - 1}
$$

Rút gọn: $(4/3)^{\log_{3/2} n} = n^{\log_{3/2}(4/3)}$. Tính $\log_{3/2}(4/3) = \frac{\ln(4/3)}{\ln(3/2)} \approx \frac{0.2877}{0.4055} \approx 0.7095$.

Vậy:
$$
T(n) \le n \cdot n^{0.7095} \cdot \frac{1}{1/3} \cdot 3 = O(n^{1.7095})
$$

**Đây là cận trên LỎNG!** Vì ta đã phóng đại nhánh nhỏ từ $n/3$ lên $2n/3$ — làm cây "nặng" hơn nhiều.

### 5.3. Cách đúng — phóng đại có kiểm soát

**Cách 1 — Chặn theo từng tầng (không phóng đại nhánh):**

Quay lại cây thật. Mỗi tầng tổng $= n$. Số tầng $\le \log_{3/2} n$ (nhánh dài nhất). Vậy:

$$
T(n) \le n \cdot \log_{3/2} n = O(n\log n)
$$

**Cách 2 — Phóng đại nhánh nhỏ lên bằng nhánh lớn (đúng):**

Thay $n/3$ bằng $2n/3$ — nhưng **giữ nguyên $f(n) = n$**. Khi đó:

$$
T(n) \le 2T(2n/3) + n
$$

Cây đều với chi phí mỗi tầng... nhưng chờ đã — chi phí mỗi tầng là $2^k \cdot n(2/3)^k = n(4/3)^k$ — **tăng**!

Đây là **nghịch lý của phóng đại**: phóng đại nhánh nhỏ lên bằng nhánh lớn **không** cho cận trên đúng — vì ta thêm nhiều "khối lượng" không có thật.

### 5.4. Cách đúng nhất — chặn theo từng tầng

**Nhận xét quan trọng:** Mỗi tầng của cây thật có **tổng kích thước các nút = n** (vì $n/3 + 2n/3 = n$, và tổng này bảo toàn qua các tầng).

**Chứng minh:** Bằng quy nạp theo tầng. Tầng 0: $n$ ✓. Tầng $k+1$: mỗi nút $m$ sinh ra $m/3 + 2m/3 = m$. Vậy tổng kích thước **không đổi** = $n$. $\blacksquare$

**Số tầng:** Nhánh dài nhất có kích thước $n(2/3)^k = 1$ ⇒ $k = \log_{3/2} n$.

Vậy:
$$
T(n) \le n \cdot \log_{3/2} n = O(n\log n)
$$

### 5.5. Chứng minh bằng quy nạp

**Giả thiết:** $T(k) \le c\,k\log k$ với $k < n$.

**Chứng minh:**

$$
T(n) = T(n/3) + T(2n/3) + n \le c\frac{n}{3}\log\frac{n}{3} + c\frac{2n}{3}\log\frac{2n}{3} + n
$$

$$
= c\frac{n}{3}(\log n - \log 3) + c\frac{2n}{3}(\log n - \log(3/2)) + n
$$

$$
= cn\log n - c\frac{n}{3}\log 3 - c\frac{2n}{3}\log(3/2) + n
$$

Để $\le cn\log n$, cần:

$$
-c\frac{n}{3}\log 3 - c\frac{2n}{3}\log(3/2) + n \le 0
$$

$$
n \le c\frac{n}{3}\left(\log 3 + 2\log(3/2)\right) = c\frac{n}{3}\log(3 \cdot (3/2)^2) = c\frac{n}{3}\log(27/4)
$$

Vì $\log_2(27/4) \approx 2.755$, cần $c \ge 3/2.755 \approx 1.09$. Chọn $c = 2$.

**Kết luận:** $T(n) = O(n\log n)$. $\blacksquare$

> **Lý thuyết bên cạnh:** Ví dụ này minh họa **bẫy phóng đại**: phóng đại nhánh nhỏ lên bằng nhánh lớn **làm hỏng bảo toàn tổng kích thước** — dẫn đến cận trên **lỏng hơn** (thậm chí sai bậc nếu phóng đại quá tay). Cách đúng: **chặn theo từng tầng** dựa trên bất biến của cây (tổng kích thước mỗi tầng = $n$). Đây là kỹ thuật quan trọng — nhớ kỹ!

---

## 6. Ví dụ 3 — $T(n) = 2T(\lfloor n/2 \rfloor) + n$

### 6.1. Bỏ floor

**Phóng đại:** $\lfloor n/2 \rfloor \le n/2$ ⇒ $T(\lfloor n/2 \rfloor) \le T(n/2)$ (vì $T$ đơn điệu tăng — giả định hợp lý với chi phí thuật toán).

Vậy:
$$
T(n) \le 2T(n/2) + n
$$

### 6.2. Vẽ cây

```
                    n                    ← tầng 0: n
                  /   \
                n/2   n/2                ← tầng 1: n
               / \    / \
             n/4 n/4 n/4 n/4             ← tầng 2: n
             ...
             1 1 ... 1                  ← tầng log₂n: n
```

### 6.3. Tổng

Mỗi tầng chi phí $n$, số tầng $\log_2 n + 1$:

$$
T(n) \le n(\log_2 n + 1) = O(n\log n)
$$

### 6.4. Chứng minh quy nạp

**Giả thiết:** $T(k) \le c\,k\log k$ với $k < n$.

$$
T(n) \le 2T(n/2) + n \le 2c(n/2)\log(n/2) + n = cn\log(n/2) + n
$$

$$
= cn(\log n - 1) + n = cn\log n - cn + n
$$

Để $\le cn\log n$, cần $c \ge 1$. Chọn $c = 1$ (hoặc $c = 2$ để an toàn với base case).

**Base case:** $T(2) = 2T(1) + 2 = 2 + 2 = 4$. Kiểm tra: $4 \le c \cdot 2 \cdot \log 2 = 2c$ ⇒ $c \ge 2$.

**Kết luận:** $T(n) \le 2n\log n$ với $n \ge 2$ ⇒ $T(n) = O(n\log n)$. $\blacksquare$

---

## 7. Ví dụ 4 — $T(n) = T(n/2) + T(n/4) + T(n/8) + n$

### 7.1. Vẽ cây

```
                        n                    ← tầng 0: n
                    /   |   \
                  n/2  n/4  n/8              ← tầng 1: n/2 + n/4 + n/8 = 7n/8
                /|\   /|\   /|\
               ...                        ← tầng 2: (7/8)² · n
               ...
```

### 7.2. Chi phí từng tầng

Tầng 1: $n/2 + n/4 + n/8 = \frac{7}{8}n$.

Giả sử (có thể chứng minh bằng quy nạp) tầng $k$ có tổng $\le (7/8)^k n$:

| Tầng $k$ | Tổng chi phí $\le$ |
|---|---|
| 0 | $n$ |
| 1 | $(7/8)n$ |
| 2 | $(7/8)^2 n$ |
| $\cdots$ | $\cdots$ |

### 7.3. Tổng — chuỗi hình học hội tụ

$$
T(n) \le n\sum_{k=0}^{\infty} \left(\frac{7}{8}\right)^k = n \cdot \frac{1}{1 - 7/8} = 8n
$$

### 7.4. Kết luận

$$
T(n) = O(n)
$$

### 7.5. Chứng minh quy nạp

**Giả thiết:** $T(k) \le c\,k$ với $k < n$.

$$
T(n) = T(n/2) + T(n/4) + T(n/8) + n \le c\frac{n}{2} + c\frac{n}{4} + c\frac{n}{8} + n = cn\left(\frac{1}{2} + \frac{1}{4} + \frac{1}{8}\right) + n
$$

$$
= \frac{7c}{8}n + n
$$

Để $\le cn$, cần $\frac{7c}{8} + 1 \le c$ ⇒ $c \ge 8$. Chọn $c = 8$.

**Kết luận:** $T(n) \le 8n$ ⇒ $O(n)$. $\blacksquare$

**Nhận xét:** Dù có **3 nhánh đệ quy**, tổng vẫn là $O(n)$ — vì tổng kích thước các nhánh **giảm** ($7/8 < 1$). Đây là hiện tượng **"nhiều nhánh nhưng tổng giảm"** — tầng gốc áp đảo.

---

## 8. Ví dụ 5 — $T(n) = 2T(\sqrt{n}) + \log n$ (đổi biến)

### 8.1. Đổi biến

Đặt $m = \log_2 n$ ⇒ $n = 2^m$. Đặt $S(m) = T(2^m)$:

$$
S(m) = 2S(m/2) + m
$$

### 8.2. Vẽ cây cho $S(m)$

```
                    m                    ← tầng 0: m
                  /   \
                m/2   m/2                ← tầng 1: m
               / \    / \
             m/4 m/4 m/4 m/4             ← tầng 2: m
             ...
             1 1 ... 1                  ← tầng log₂m: m
```

### 8.3. Tổng

$$
S(m) = O(m\log m)
$$

### 8.4. Quay lại

$$
T(n) = S(\log n) = O(\log n \cdot \log\log n)
$$

### 8.5. Chứng minh quy nạp (cho $S$)

**Giả thiết:** $S(k) \le c\,k\log k$ với $k < m$.

$$
S(m) = 2S(m/2) + m \le 2c(m/2)\log(m/2) + m = cm\log(m/2) + m
$$

$$
= cm(\log m - 1) + m = cm\log m - cm + m
$$

Để $\le cm\log m$, cần $c \ge 1$. Chọn $c = 2$.

**Kết luận:** $S(m) = O(m\log m)$ ⇒ $T(n) = O(\log n \cdot \log\log n)$. $\blacksquare$

---

## 9. Ví dụ 6 — $T(n) = 4T(n/2) + n^2\sqrt{n}$

### 9.1. Phân tích sơ bộ

$a = 4$, $b = 2$, $x = n^{\log_2 4} = n^2$. $f(n) = n^{2.5}$.

$f(n) = \Omega(n^{2+\varepsilon})$ với $\varepsilon = 0.5$ ⇒ nghi ngờ **Case 3**: $T(n) = O(n^{2.5})$.

### 9.2. Vẽ cây

| Tầng $k$ | Số nút | Chi phí mỗi nút | Tổng tầng |
|---|---|---|---|
| 0 | $1$ | $n^{2.5}$ | $n^{2.5}$ |
| 1 | $4$ | $(n/2)^{2.5} = n^{2.5}/2^{2.5}$ | $4n^{2.5}/2^{2.5} = n^{2.5} \cdot 4/2^{2.5}$ |
| 2 | $16$ | $n^{2.5}/4^{2.5}$ | $n^{2.5} \cdot (4/2^{2.5})^2$ |

Tỉ số: $\frac{4}{2^{2.5}} = \frac{4}{5.657} \approx 0.707 < 1$.

### 9.3. Tổng — chuỗi hội tụ

$$
T(n) \le n^{2.5} \sum_{k=0}^{\infty} \left(\frac{4}{2^{2.5}}\right)^k = n^{2.5} \cdot \frac{1}{1 - 0.707} \approx 3.41\, n^{2.5}
$$

### 9.4. Kết luận

$$
T(n) = O(n^{2.5})
$$

### 9.5. Chứng minh quy nạp

**Giả thiết:** $T(k) \le c\,k^{2.5}$ với $k < n$.

$$
T(n) = 4T(n/2) + n^{2.5} \le 4c(n/2)^{2.5} + n^{2.5} = \frac{4c}{2^{2.5}} n^{2.5} + n^{2.5}
$$

Hệ số $\frac{4}{2^{2.5}} = \frac{4}{4\sqrt{2}} = \frac{1}{\sqrt{2}} \approx 0.707$.

$$
= \frac{c}{\sqrt{2}} n^{2.5} + n^{2.5}
$$

Để $\le c\,n^{2.5}$: $\frac{1}{\sqrt{2}} + \frac{1}{c} \le 1$ ⇒ $\frac{1}{c} \le 1 - 0.707 = 0.293$ ⇒ $c \ge 3.41$.

**Chọn $c = 4$**. $\blacksquare$

---

## 10. Ví dụ 7 — $T(n) = T(n/2) + n^2$ (Case 3 — chi phí giảm)

### 10.1. Vẽ cây

```
                    n²                  ← tầng 0: n²
                    |
                  (n/2)²                ← tầng 1: n²/4
                    |
                  (n/4)²                ← tầng 2: n²/16
                    |
                  ...
                    |
                    1                   ← tầng log₂n
```

**Chỉ một nhánh** (vì $a = 1$).

### 10.2. Tổng — chuỗi hội tụ

$$
T(n) \le n^2 \left(1 + \frac{1}{4} + \frac{1}{16} + \cdots\right) = n^2 \cdot \frac{1}{1 - 1/4} = \frac{4}{3}n^2
$$

### 10.3. Kết luận

$$
T(n) = O(n^2)
$$

### 10.4. Chứng minh quy nạp

**Giả thiết:** $T(k) \le c\,k^2$ với $k < n$.

$$
T(n) = T(n/2) + n^2 \le c(n/2)^2 + n^2 = \frac{c}{4}n^2 + n^2
$$

Để $\le cn^2$: $\frac{1}{4} + \frac{1}{c} \le 1$ ⇒ $c \ge 4/3$. Chọn $c = 2$.

$\blacksquare$

---

## 11. Ví dụ 8 — Trường hợp chi phí tăng (Case 1)

### 11.1. Hệ thức

$$
T(n) = 3T(n/2) + n, \quad T(1) = 1
$$

### 11.2. Vẽ cây

| Tầng $k$ | Số nút | Chi phí mỗi nút | Tổng tầng |
|---|---|---|---|
| 0 | $1$ | $n$ | $n$ |
| 1 | $3$ | $n/2$ | $3n/2$ |
| 2 | $9$ | $n/4$ | $9n/4$ |
| $\cdots$ | $\cdots$ | $\cdots$ | $\cdots$ |
| $k$ | $3^k$ | $n/2^k$ | $n(3/2)^k$ |

Tầng $k$ tăng theo $(3/2)^k$ — tầng lá áp đảo.

### 11.3. Số tầng và tổng

Số tầng: $\log_2 n$. Tầng lá: $3^{\log_2 n} = n^{\log_2 3}$ nút, mỗi nút chi phí 1.

$$
T(n) \le \sum_{k=0}^{\log_2 n} n\left(\frac{3}{2}\right)^k = n \cdot \frac{(3/2)^{\log_2 n + 1} - 1}{1/2} = O\left(n \cdot n^{\log_2(3/2)}\right)
$$

Tính: $\log_2(3/2) = \log_2 3 - 1 \approx 0.585$.

$$
T(n) = O(n \cdot n^{0.585}) = O(n^{1.585}) = O(n^{\log_2 3})
$$

### 11.4. Chứng minh quy nạp

**Giả thiết:** $T(k) \le c\,k^{\log_2 3}$ với $k < n$. Đặt $\alpha = \log_2 3 \approx 1.585$:

$$
T(n) = 3T(n/2) + n \le 3c(n/2)^\alpha + n = \frac{3c}{2^\alpha} n^\alpha + n
$$

Vì $2^\alpha = 2^{\log_2 3} = 3$:

$$
= \frac{3c}{3} n^\alpha + n = c\,n^\alpha + n
$$

Cần $\le c\,n^\alpha$ ⇒ $n \le 0$ — **không thể**! Cần kỹ thuật "trừ hằng số":

**Giả thiết mạnh hơn:** $T(k) \le c\,k^\alpha - d\,k$ với $d > 0$:

$$
T(n) \le 3(c(n/2)^\alpha - d(n/2)) + n = \frac{3c}{3}n^\alpha - \frac{3d}{2}n + n = c\,n^\alpha - \left(\frac{3d}{2} - 1\right)n
$$

Để $\le c\,n^\alpha - dn$: cần $\frac{3d}{2} - 1 \ge d$ ⇒ $\frac{d}{2} \ge 1$ ⇒ $d \ge 2$. Chọn $d = 2$.

**Kết luận:** $T(n) = O(n^{\log_2 3})$. $\blacksquare$

> **Lý thuyết bên cạnh:** Kỹ thuật "trừ hằng số" (subtract a lower-order term) cực kỳ hữu ích khi chứng minh $O$ cho trường hợp **chi phí tăng** — vì giả thiết đơn giản $T(k) \le c\,k^\alpha$ **không đủ mạnh** để chứng minh $T(n) \le c\,n^\alpha$. Phải trừ thêm $d\,n$ (hoặc $d\,k$) để "hấp thụ" số hạng dư. Đây là bẫy phổ biến trong phương pháp thế.

---

## 12. Bảng tổng hợp các pattern cây

| Pattern | Chi phí tầng $k$ | Tầng áp đảo | Cận trên |
|---|---|---|---|
| $a/b > 1$ (tăng) | $\propto (a/b)^k$ | Lá | $O(n^{\log_b a})$ |
| $a/b = 1$ (không đổi) | $\propto n^{\log_b a}$ | Mọi tầng | $O(n^{\log_b a}\log n)$ |
| $a/b < 1$ (giảm) | $\propto (a/b)^k$ | Gốc | $O(n^{\log_b a})$ |
| $f(n)$ giảm nhanh | $\propto f(n/b^k)$ | Gốc | $O(f(n))$ |
| $f(n)$ tăng nhanh | $\propto a^k f(n/b^k)$ | Lá | $O(\text{chi phí lá})$ |

**Công thức chung:** Khi $f(n) = n^c$:

$$
T(n) = \sum_{k=0}^{\log_b n} a^k (n/b^k)^c = n^c \sum_{k=0}^{\log_b n} (a/b^c)^k
$$

- $a/b^c < 1$ ⇒ chuỗi hội tụ ⇒ $T(n) = O(n^c) = O(f(n))$.
- $a/b^c = 1$ ⇒ mỗi tầng đóng góp $n^c$ ⇒ $T(n) = O(n^c\log n)$.
- $a/b^c > 1$ ⇒ chuỗi hình học tăng ⇒ $T(n) = O(a^{\log_b n}) = O(n^{\log_b a})$.

**Đây chính là Master Theorem — nhưng hiểu qua lăng kính cây đệ quy.**

---

## 13. Kỹ thuật "trừ hằng số" — khi giả thiết đơn giản không đủ

### 13.1. Khi nào cần?

Khi chứng minh $T(n) \le c\,g(n)$ mà bước quy nạp cho ra **số hạng dư dương** không triệt tiêu được.

**Ví dụ:** $T(n) = T(n/2) + 1$, muốn chứng minh $T(n) = O(1)$? — Sai! Thực ra $T(n) = O(\log n)$. Nhưng thử chứng minh $T(n) = O(n)$ thì được.

**Trường hợp điển hình cần trừ hằng số:** $T(n) = T(n/2) + n$ — muốn chứng minh $O(n)$.

### 13.2. Ví dụ — $T(n) = T(n/2) + n$, chứng minh $O(n)$

**Giả thiết đơn giản:** $T(k) \le c\,k$ với $k < n$.

$$
T(n) = T(n/2) + n \le c(n/2) + n = \frac{c}{2}n + n
$$

Để $\le cn$: $\frac{c}{2} + 1 \le c$ ⇒ $c \ge 2$. Chọn $c = 2$.

**Chứng minh thành công** — không cần trừ hằng số trong trường hợp này.

### 13.3. Ví dụ cần trừ hằng số — $T(n) = 2T(n/2) + 1$

**Giả thiết đơn giản:** $T(k) \le c\,k$ với $k < n$.

$$
T(n) = 2T(n/2) + 1 \le 2 \cdot c(n/2) + 1 = cn + 1
$$

Để $\le cn$: $1 \le 0$ — **không thể**!

**Giả thiết mạnh hơn:** $T(k) \le c\,k - d$ với $d > 0$:

$$
T(n) \le 2(c(n/2) - d) + 1 = cn - 2d + 1
$$

Để $\le cn - d$: $-2d + 1 \le -d$ ⇒ $d \ge 1$. Chọn $d = 1$, $c = 2$.

**Kết luận:** $T(n) \le 2n - 1$ ⇒ $O(n)$. $\blacksquare$

**Nhận xét:** Cây đệ quy cho $T(n) = 2n - 1$ — khớp với chứng minh quy nạp!

> **Lý thuyết bên cạnh:** Kỹ thuật "trừ hằng số" thường cần khi $f(n)$ **không đủ lớn** để "hấp thụ" chi phí đệ quy. Cụ thể, khi $T(n) = aT(n/b) + f(n)$ với $f(n)$ nhỏ (như hằng số), giả thiết $T(k) \le c\,g(k)$ thường **không đủ** — phải trừ thêm số hạng bậc thấp. Quy tắc: nếu bước quy nạp cho ra $+O(1)$ dư, trừ đi $d$ (hằng số); nếu cho ra $+O(k)$ dư, trừ đi $d\,k$.

---

## 14. Ví dụ tổng hợp — áp dụng đầy đủ 5 bước

### Bài toán: $T(n) = T(n/2) + T(n/4) + T(n/8) + T(n/16) + n$, $T(1) = 1$

#### Bước 1 — Hệ thức

Đã cho.

#### Bước 2 — Vẽ cây

```
                        n                    ← tầng 0: n
              /     /       \      \
            n/2   n/4      n/8    n/16    ← tầng 1: (15/16)n
            /|\   /|\      /|\     /|\
           ...                           ← tầng 2: (15/16)² n
           ...
```

#### Bước 3 — Chi phí từng tầng

Tầng 1: $n/2 + n/4 + n/8 + n/16 = \frac{8+4+2+1}{16}n = \frac{15}{16}n$.

Bằng quy nạp (theo tầng), tầng $k$ có tổng $\le (15/16)^k n$.

#### Bước 4 — Tổng

Vì $15/16 < 1$, chuỗi hội tụ:

$$
T(n) \le n\sum_{k=0}^{\infty} \left(\frac{15}{16}\right)^k = 16n
$$

**Dự đoán:** $T(n) = O(n)$.

#### Bước 5 — Chứng minh quy nạp

**Giả thiết:** $T(k) \le c\,k$ với $k < n$.

$$
T(n) \le c\left(\frac{n}{2} + \frac{n}{4} + \frac{n}{8} + \frac{n}{16}\right) + n = c \cdot \frac{15}{16}n + n
$$

Để $\le cn$: $\frac{15}{16} + \frac{1}{c} \le 1$ ⇒ $\frac{1}{c} \le \frac{1}{16}$ ⇒ $c \ge 16$.

**Chọn $c = 16$.** Base case: $T(1) = 1 \le 16 \cdot 1 = 16$ ✓.

**Kết luận:** $T(n) \le 16n$ với mọi $n \ge 1$ ⇒ $T(n) = O(n)$. $\blacksquare$

**Đối chiếu:** Cây cho hằng số $16$ — chứng minh quy nạp cũng cho $c = 16$. Khớp hoàn hảo.

---

## 15. Những bẫy phổ biến khi chứng minh cận trên bằng cây đệ quy

### Bẫy 1 — "Phóng đại quá tay"

**Sai:** Thay $n/3$ bằng $n$ (thay vì $2n/3$) — cây không thu nhỏ, tổng **phân kỳ**.

**Ví dụ:** $T(n) = T(n/3) + n$. Nếu phóng đại $T(n/3) \le T(n)$, được $T(n) \le T(n) + n$ — vô nghĩa.

**Đúng:** Chỉ phóng đại nhánh nhỏ lên **bằng nhánh lớn** (không vượt quá $n$).

### Bẫy 2 — "Bỏ qua tổng kích thước mỗi tầng"

Với cây không đều, mỗi tầng có thể có **tổng kích thước khác nhau**. Không được giả định "mỗi tầng chi phí $n$" mà không chứng minh.

**Cách đúng:** Chứng minh bất biến "tổng kích thước tầng $k$ = $n$" (hoặc $\le n$) bằng quy nạp.

### Bẫy 3 — "Không chứng minh quy nạp"

**Sai:** Chỉ vẽ cây và kết luận $O(g(n))$.

**Đúng:** Cây chỉ cho **dự đoán**. Phải chứng minh bằng quy nạp (phương pháp thế). Trong bài thi, thiếu bước này có thể mất điểm.

### Bẫy 4 — "Chuỗi hình học không hội tụ"

Khi tỉ số $r = a/b^c \ge 1$, chuỗi **phân kỳ** — không thể dùng công thức $\sum r^k = 1/(1-r)$.

**Phải xử lý:**

- $r = 1$: mỗi tầng đóng góp như nhau ⇒ $T(n) = O(f(n) \cdot \log n)$.
- $r > 1$: tầng lá áp đảo ⇒ $T(n) = O(a^{\log_b n}) = O(n^{\log_b a})$.

### Bẫy 5 — "Base case không khớp"

Khi chứng minh quy nạp, base case có thể **không thỏa** với $c$ tìm được (như $T(1) = 1 \le c \cdot 1 \cdot \log 1 = 0$ — sai).

**Khắc phục:** Điều chỉnh $n_0$ (dùng $n_0 = 2$ thay vì $1$), hoặc tăng $c$.

### Bẫy 6 — "Giả thiết quy nạp quá yếu"

Khi $f(n)$ nhỏ (như hằng số), giả thiết $T(k) \le c\,g(k)$ **không đủ** — cần trừ hằng số: $T(k) \le c\,g(k) - d$.

**Ví dụ:** $T(n) = 2T(n/2) + 1$, chứng minh $O(n)$ — cần $T(k) \le ck - d$.

### Bẫy 7 — "Đổi biến không đúng"

Với $T(n) = 2T(\sqrt{n}) + 1$, phải đặt $m = \log n$ (không phải $m = \sqrt{n}$). Khi đó $T(2^m) = 2T(2^{m/2}) + 1$ ⇒ $S(m) = 2S(m/2) + 1$.

### Bẫy 8 — "Bỏ qua chi phí lá"

Khi tính tổng, phải cộng **cả tầng lá**. Với cây có $a^{\log_b n} = n^{\log_b a}$ lá, mỗi lá chi phí $\Theta(1)$ — tổng là $O(n^{\log_b a})$. Bỏ qua có thể **thiếu bậc**.

### Bẫy 9 — "Nhầm cận trên với cận chặt"

Cây đệ quy cho **cận trên** — nhưng nếu dùng phóng đại, cận trên có thể **lỏng** (như Ví dụ 2 với phóng đại sai). Phải kiểm tra xem cận trên có **chặt** không (bằng cách so sánh với chặn dưới).

### Bẫy 10 — "Không kiểm tra điều kiện áp dụng"

Cây đệ quy **luôn** vẽ được — nhưng với hệ thức **không chuẩn** (như $T(n) = T(n-1) + T(n-2)$), cây **không đều**, khó tính chính xác. Khi đó, dùng công cụ khác (phương trình đặc trưng).

> **Lý thuyết bên cạnh:** Hầu hết bẫy đến từ việc **quên rằng cây đệ quy là công cụ đoán** — cần chứng minh quy nạp để hoàn thiện. Và khi phóng đại, phải **kiểm soát** — không được làm mất bậc. Hai nguyên tắc: (1) phóng đại **có bảo toàn bậc**; (2) **luôn chứng minh** bằng quy nạp.

---

## 16. So sánh cây đệ quy cho cận trên vs cận chặt

| Tiêu chí | Cận trên ($O$) | Cận chặt ($\Theta$) |
|---|---|---|
| **Mục đích** | Chứng minh $T(n) \le c\,g(n)$ | Chứng minh $c_1 g \le T \le c_2 g$ |
| **Phóng đại** | ✅ Được phép | ❌ Không (phải chính xác) |
| **Bỏ floor/ceil** | ✅ Được | Cần xử lý cẩn thận |
| **Cây không đều** | Chặn từng tầng | Cần chặn cả hai chiều |
| **Chứng minh quy nạp** | Một chiều ($\le$) | Hai chiều ($\le$ và $\ge$) |
| **Độ khó** | Dễ hơn | Khó hơn |
| **Kết quả** | $O(g(n))$ — có thể lỏng | $\Theta(g(n))$ — chặt |

**Chiến lược thực tế:**

1. **Chứng minh $O$ trước** (dễ hơn) — để có bảo đảm.
2. **Chứng minh $\Omega$ sau** (nếu cần) — để có cận chặt.
3. **Kết hợp** ⇒ $\Theta$.

---

## 17. Bài tập tự luyện

**Bài 1:** $T(n) = 2T(n/3) + n$, chứng minh $O(n)$.

*Hướng:* Cây có $2^k$ nút tầng $k$, mỗi nút $n/3^k$. Tổng tầng: $n(2/3)^k$ — giảm. Tổng $\le 3n$ ⇒ $O(n)$. Quy nạp: $T(k) \le c\,k$ ⇒ $T(n) \le 2c(n/3) + n = (2c/3 + 1)n$ — cần $c \ge 3$.

**Bài 2:** $T(n) = 3T(n/3) + n$, chứng minh $O(n\log n)$.

*Hướng:* Mỗi tầng $n$, số tầng $\log_3 n$ ⇒ $n\log_3 n$. Quy nạp: $T(n) \le 3c(n/3)\log(n/3) + n = cn\log(n/3) + n = cn\log n - cn\log 3 + n$ — cần $c \ge 1/\log 3$.

**Bài 3:** $T(n) = T(2n/3) + 1$, chứng minh $O(\log n)$.

*Hướng:* Cây một nhánh, kích thước $n(2/3)^k$, dừng khi = 1 ⇒ $k = \log_{3/2} n$ ⇒ $O(\log n)$.

**Bài 4:** $T(n) = 2T(n/4) + \sqrt{n}$, chứng minh $O(\sqrt{n}\log n)$.

*Hướng:* $a=2$, $b=4$, $f(n)=n^{0.5}$, $x = n^{\log_4 2} = n^{0.5}$ — Case 2 ⇒ $O(\sqrt{n}\log n)$.

**Bài 5:** $T(n) = 4T(n/2) + n$, chứng minh $O(n^2)$.

*Hướng:* $a=4$, $b=2$, $x=n^2$, $f(n)=n=O(n^{2-\varepsilon})$ — Case 1 ⇒ $O(n^2)$.

**Bài 6:** $T(n) = T(n-1) + n^2$, chứng minh $O(n^3)$.

*Hướng:* Cây một nhánh, tổng $\sum_{k=1}^{n} k^2 = \Theta(n^3)$ ⇒ $O(n^3)$.

**Bài 7:** $T(n) = 2T(n/2) + n\log n$, chứng minh $O(n\log^2 n)$.

*Hướng:* Tầng $k$ chi phí $n(\log n - k)$ — tổng $n\sum_{j=1}^{\log n} j = O(n\log^2 n)$.

**Bài 8:** $T(n) = T(n/2) + T(n/4) + n$, chứng minh $O(n)$.

*Hướng:* Tầng 1: $n/2 + n/4 = 3n/4$. Tổng $\le n\sum (3/4)^k = 4n$ ⇒ $O(n)$.

**Bài 9:** $T(n) = 3T(n/2) + n^2$, chứng minh $O(n^2)$.

*Hướng:* $a=3$, $b=2$, $x=n^{1.585}$, $f(n)=n^2 = \Omega(n^{1.585+\varepsilon})$ — Case 3 ⇒ $O(n^2)$.

**Bài 10:** $T(n) = 2T(\sqrt{n}) + \log n$, chứng minh $O(\log n\log\log n)$.

*Hướng:* Đổi biến $m = \log n$ ⇒ $S(m) = 2S(m/2) + m = O(m\log m)$.

---

## 18. Tóm tắt một trang

### Quy trình 5 bước

1. **Viết hệ thức.**
2. **Vẽ cây phóng đại** — bỏ floor/ceil, phóng đại có kiểm soát.
3. **Tính chi phí từng tầng** — $a^k f(n/b^k)$.
4. **Xác định tầng áp đảo** — tăng/không đổi/giảm.
5. **Chứng minh quy nạp** — bắt buộc.

### Ba pattern

| Pattern | Cận trên |
|---|---|
| Tầng lá áp đảo (tăng) | $O(n^{\log_b a})$ |
| Mọi tầng bằng nhau | $O(f(n)\log n)$ |
| Tầng gốc áp đảo (giảm) | $O(f(n))$ |

### Công thức cây

$$
T(n) \le \sum_{k=0}^{\log_b n} a^k f\!\left(\frac{n}{b^k}\right)
$$

Khi $f(n) = n^c$:

$$
T(n) \le n^c \sum_{k=0}^{\log_b n} \left(\frac{a}{b^c}\right)^k
$$

- $a/b^c < 1$ ⇒ hội tụ ⇒ $O(n^c)$.
- $a/b^c = 1$ ⇒ $O(n^c\log n)$.
- $a/b^c > 1$ ⇒ $O(n^{\log_b a})$.

### Kỹ thuật phóng đại

- Bỏ floor/ceil (dùng $n' = 2^{\lceil \log n \rceil}$).
- Phóng đại nhánh nhỏ **có kiểm soát** (không vượt quá nhánh lớn).
- Chặn $f(n)$ bằng hàm đơn giản hơn.
- Chứng minh bất biến "tổng kích thước mỗi tầng" bằng quy nạp.

### Kỹ thuật "trừ hằng số"

Khi giả thiết $T(k) \le c\,g(k)$ không đủ:

$$
T(k) \le c\,g(k) - d \quad \text{hoặc} \quad c\,g(k) - d\,k
$$

### Bẫy cần tránh

1. **Phóng đại quá tay** — cây không thu nhỏ.
2. **Bỏ qua tổng kích thước mỗi tầng** — phải chứng minh bất biến.
3. **Không chứng minh quy nạp** — cây chỉ là dự đoán.
4. **Chuỗi hình học phân kỳ** — xử lý riêng $r \ge 1$.
5. **Base case không khớp** — điều chỉnh $n_0$.
6. **Giả thiết quá yếu** — trừ hằng số.

---

Nếu bạn muốn, mình có thể đi sâu:
- **Lower bounds** bằng cây đệ quy (chứng minh $\Omega$ — cần **đánh giá thấp**, không phóng đại).
- **Chứng minh Master Theorem** hoàn toàn bằng cây đệ quy (cả ba case).
- **Akra–Bazzi** — mở rộng cho hệ thức không đều.
- **Bài tập khó** — hệ thức với $f(n)$ dao động, $f(n)$ không đơn điệu.
- **Ứng dụng** — phân tích thuật toán cụ thể (FFT, Strassen, closest pair).