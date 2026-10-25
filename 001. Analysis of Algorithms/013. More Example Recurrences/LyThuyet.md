# Các ví dụ hệ thức truy hồi nâng cao (More Example Recurrences)

Tài liệu này tập hợp **nhiều dạng hệ thức truy hồi** khác nhau, phân loại theo cấu trúc, mỗi ví dụ có **lời giải chi tiết bằng nhiều phương pháp**. Mục tiêu: xây dựng **phản xạ** nhận diện dạng và chọn công cụ phù hợp.

---

## 1. Bảng phân loại hệ thức truy hồi

| Dạng                 | Đặc điểm                                     | Công cụ chính                     |
| -------------------- | -------------------------------------------- | --------------------------------- |
| **Chia đều**         | $T(n) = aT(n/b) + f(n)$, $a,b$ hằng          | Master Theorem                    |
| **Chia không đều**   | $T(n) = T(\alpha n) + T((1-\alpha)n) + f(n)$ | Cây đệ quy, Akra–Bazzi            |
| **Trừ đều**          | $T(n) = aT(n-c) + f(n)$                      | Unrolling, phương trình đặc trưng |
| **Trừ không đều**    | $T(n) = T(n-1) + T(n-2) + \ldots$            | Phương trình đặc trưng            |
| **Căn thức**         | $T(n) = aT(\sqrt{n}) + f(n)$                 | Đổi biến $m = \log n$             |
| **Đa tham số**       | $T(n,m) = \ldots$                            | Phân tích hai chiều               |
| **Không thuần nhất** | $f(n)$ có $\log$, $\sqrt{}$, dao động        | Cây đệ quy + thế                  |

---

## 2. Nhóm 1 — Chia đều chuẩn (Master Theorem)

### Ví dụ 1 — $T(n) = 2T(n/2) + n^2$

**Phân tích:** $a = 2$, $b = 2$, $f(n) = n^2$, $x = n^{\log_2 2} = n$.

$f(n) = n^2 = \Omega(n^{1+\varepsilon})$ với $\varepsilon = 1$. Kiểm tra điều kiện chính quy:

$$
a\,f(n/b) = 2 \cdot (n/2)^2 = n^2/2 \le c \cdot n^2 \quad \text{với } c = 1/2 < 1
$$

⇒ **Case 3:**

$$
T(n) = \Theta(n^2)
$$

**Trực giác cây:** Tầng 0 chi phí $n^2$, tầng 1 chi phí $n^2/2$, tầng 2 chi phí $n^2/4$ — giảm dần, tầng gốc áp đảo.

---

### Ví dụ 2 — $T(n) = 4T(n/2) + n$

**Phân tích:** $a = 4$, $b = 2$, $f(n) = n$, $x = n^{\log_2 4} = n^2$.

$f(n) = n = O(n^{2-\varepsilon})$ với $\varepsilon = 1$ ⇒ **Case 1:**

$$
T(n) = \Theta(n^2)
$$

**Trực giác cây:** Tầng $k$ có $4^k$ nút, mỗi nút chi phí $n/2^k$, tổng $n \cdot 2^k$ — tăng theo $2^k$, tầng lá áp đảo. Số lá: $4^{\log_2 n} = n^2$.

---

### Ví dụ 3 — $T(n) = 3T(n/2) + n^2$

**Phân tích:** $a = 3$, $b = 2$, $x = n^{\log_2 3} \approx n^{1.585}$, $f(n) = n^2$.

$f(n) = n^2 = \Omega(n^{1.585+\varepsilon})$ với $\varepsilon \approx 0.415$. Kiểm tra chính quy: $3(n/2)^2 = 0.75 n^2 \le c n^2$ với $c = 0.75 < 1$ ✓.

⇒ **Case 3:**

$$
T(n) = \Theta(n^2)
$$

---

### Ví dụ 4 — $T(n) = 9T(n/3) + n^2$

**Phân tích:** $a = 9$, $b = 3$, $x = n^{\log_3 9} = n^2$, $f(n) = n^2$.

$f(n) = \Theta(x)$ ⇒ **Case 2:**

$$
T(n) = \Theta(n^2\log n)
$$

**Trực giác:** Mỗi tầng chi phí $n^2$, số tầng $\log_3 n$ ⇒ $n^2\log_3 n = \Theta(n^2\log n)$.

---

### Ví dụ 5 — $T(n) = 8T(n/4) + n^{1.5}$

**Phân tích:** $a = 8$, $b = 4$, $x = n^{\log_4 8} = n^{1.5}$, $f(n) = n^{1.5}$.

$f(n) = \Theta(x)$ ⇒ **Case 2:**

$$
T(n) = \Theta(n^{1.5}\log n)
$$

**Lưu ý tinh tế:** $\log_4 8 = 3/2$ — đúng bằng số mũ của $f$. Đây là "biên" giữa Case 1 và Case 3 — nên rơi vào Case 2.

---

## 3. Nhóm 2 — Chia không đều (Uneven split)

### Ví dụ 6 — $T(n) = T(n/3) + T(2n/3) + n$

**Đây là Quick Sort worst case** với pivot chia 1:2.

**Cây đệ quy:** Mỗi tầng, tổng kích thước các nút = $n/3 + 2n/3 = n$ ⇒ tổng chi phí mỗi tầng $n$.

**Số tầng:**

- Nhánh ngắn nhất: $n/3$ — độ sâu $\log_3 n$.
- Nhánh dài nhất: $2n/3$ — độ sâu $\log_{3/2} n$.

**Chặn:**

$$
n\log_3 n \le T(n) \le n\log_{3/2} n
$$

⇒ $T(n) = \Theta(n\log n)$.

**Nhận xét:** Dù chia **không đều**, kết quả vẫn $\Theta(n\log n)$ — chỉ khác hằng số. Đây là **định lý tổng quát**: nếu $T(n) = T(\alpha n) + T((1-\alpha)n) + n$ với $0 < \alpha \le 1/2$ cố định, thì $T(n) = \Theta(n\log n)$ bất kể $\alpha$.

---

### Ví dụ 7 — $T(n) = T(n/10) + T(9n/10) + n$

**Cây đệ quy:** Mỗi tầng tổng chi phí $n$ (vì $n/10 + 9n/10 = n$).

**Số tầng:** từ $\log_{10} n$ (nhánh ngắn) đến $\log_{10/9} n$ (nhánh dài).

Vì $\log_{10/9} n \approx 21.85 \log_2 n$ — chỉ khác hằng số so với $\log n$.

**Kết quả:**

$$
T(n) = \Theta(n\log n)
$$

**Ý nghĩa:** Pivot chia 1:9 vẫn cho $\Theta(n\log n)$ — đây là lý do Quick Sort với pivot **không quá tệ** vẫn nhanh.

---

### Ví dụ 8 — $T(n) = T(n/2) + T(n/4) + n$

**Cây đệ quy:** Mỗi tầng tổng kích thước **không cố định**:

- Tầng 0: $n$
- Tầng 1: $n/2 + n/4 = 3n/4$
- Tầng 2: $n/4 + n/8 + n/8 + n/16 = 5n/8$? — cần tính chính xác.

**Quan sát:** Mỗi tầng tổng **giảm** — tầng gốc áp đảo:

$$
T(n) \le n + \frac{3n}{4} + \left(\frac{3}{4}\right)^2 n + \cdots = n \cdot \frac{1}{1 - 3/4} = 4n
$$

**Chặn dưới:** $T(n) \ge n$.

**Kết quả:**

$$
T(n) = \Theta(n)
$$

**Lưu ý:** Đây là dạng **Akra–Bazzi** — không thuộc Master Theorem chuẩn vì hai bài toán con có kích thước khác nhau.

---

## 4. Nhóm 3 — Trừ đều (Subtract-and-conquer)

### Ví dụ 9 — $T(n) = T(n-1) + 1$

**Phương pháp unrolling:**

$$
T(n) = T(n-1) + 1 = T(n-2) + 2 = \cdots = T(1) + (n-1)
$$

$$
T(n) = \Theta(n)
$$

**Ứng dụng:** Factorial, in dãy ngược, vòng lặp đệ quy tuyến tính.

---

### Ví dụ 10 — $T(n) = T(n-1) + n$

**Unrolling:**

$$
T(n) = \sum_{k=1}^{n} k = \frac{n(n+1)}{2} = \Theta(n^2)
$$

**Ứng dụng:** Selection Sort, Insertion Sort đệ quy.

---

### Ví dụ 11 — $T(n) = 2T(n-1) + 1$

**Unrolling:**

$$
T(n) = 2T(n-1) + 1 = 4T(n-2) + 3 = 8T(n-3) + 7 = \cdots = 2^k T(n-k) + (2^k - 1)
$$

Với $k = n-1$: $T(n) = 2^{n-1} T(1) + 2^{n-1} - 1 = \Theta(2^n)$.

**Ứng dụng:** Tháp Hà Nội, liệt kê tất cả tập con.

---

### Ví dụ 12 — $T(n) = 3T(n-1) + 2^n$

**Unrolling:**

$$
T(n) = 3T(n-1) + 2^n = 3^2 T(n-2) + 3 \cdot 2^{n-1} + 2^n = \cdots
$$

$$
= 3^n T(0) + \sum_{k=0}^{n-1} 3^k \cdot 2^{n-k} = 3^n T(0) + 2^n \sum_{k=0}^{n-1} \left(\frac{3}{2}\right)^k
$$

Tổng cấp số nhân:

$$
2^n \cdot \frac{(3/2)^n - 1}{3/2 - 1} = 2 \cdot 3^n - 2 \cdot 2^n
$$

**Kết quả:**

$$
T(n) = \Theta(3^n)
$$

**Quan sát:** Hàm mũ $3^n$ áp đảo $2^n$ — vì $3 > 2$.

---

### Ví dụ 13 — $T(n) = 2T(n-1) + n$

**Unrolling:**

$$
T(n) = 2^n T(0) + \sum_{k=0}^{n-1} 2^k (n-k)
$$

Xét tổng $\sum_{k=0}^{n-1} 2^k (n-k)$. Có thể tính chính xác:

$$
= n \sum 2^k - \sum k 2^k = n(2^n - 1) - \left((n-2)2^n + 2\right) = 2^{n+1} - n - 2
$$

**Kết quả:**

$$
T(n) = \Theta(2^n)
$$

**Nhận xét:** $2^n$ (từ số nhánh) áp đảo $n$ (từ $f(n)$). Kết quả là hàm mũ.

---

### Ví dụ 14 — $T(n) = T(n-2) + n$

**Unrolling** (chú ý $n$ giảm 2):

$$
T(n) = n + (n-2) + (n-4) + \cdots + 2 = 2 \sum_{k=1}^{n/2} k = 2 \cdot \frac{(n/2)(n/2+1)}{2} = \Theta(n^2)
$$

**Kết quả:**

$$
T(n) = \Theta(n^2)
$$

**Bước nhảy $c$ không đổi bậc** — chỉ khác hằng số.

---

### Ví dụ 15 — $T(n) = T(n-1) + \log n$

**Unrolling:**

$$
T(n) = \sum_{k=1}^{n} \log k = \log(n!) = \Theta(n\log n)
$$

**Ứng dụng:** Một số biến thể của Insertion Sort.

---

## 5. Nhóm 4 — Trừ không đều (Fibonacci và tương tự)

### Ví dụ 16 — $T(n) = T(n-1) + T(n-2) + 1$

**Phương trình đặc trưng:** Bỏ qua hạng tử hằng số:

$$
c^2 = c + 1 \Rightarrow c = \frac{1+\sqrt{5}}{2} = \varphi \approx 1.618
$$

**Kết quả:**

$$
T(n) = \Theta(\varphi^n) = \Theta(1.618^n)
$$

**Không phải $2^n$** — dù nhiều tài liệu viết $O(2^n)$ (cận trên lỏng).

---

### Ví dụ 17 — $T(n) = T(n-1) + T(n-3)$

**Phương trình đặc trưng:**

$$
c^3 = c^2 + 1 \Rightarrow c^3 - c^2 - 1 = 0
$$

Nghiệm thực duy nhất xấp xỉ $c \approx 1.4656$ (hằng số **plastic number** / tỉ lệ bạc).

**Kết quả:**

$$
T(n) = \Theta(1.4656^n)
$$

**Phương pháp:** Nghiệm của đa thức đặc trưng cho biết cơ số mũ. Đây là dạng tổng quát của Fibonacci.

---

### Ví dụ 18 — $T(n) = 2T(n-2) + 1$

**Đổi biến:** Đặt $m = n/2$ và $S(m) = T(2m)$:

$$
S(m) = 2S(m-1) + 1
$$

Đây là dạng quen thuộc: $S(m) = \Theta(2^m)$.

**Quay lại:**

$$
T(n) = S(n/2) = \Theta(2^{n/2}) = \Theta(\sqrt{2}^n) \approx \Theta(1.414^n)
$$

---

### Ví dụ 19 — $T(n) = 4T(n/2) + n!$ (một trường hợp kỳ lạ)

**Phân tích:** Đây là dạng chia đều nhưng $f(n) = n!$ **tăng quá nhanh** so với $n^{\log_2 4} = n^2$.

$f(n) = n! = \Omega(n^{2+\varepsilon})$ với mọi $\varepsilon$ — thậm chí $\omega(n^k)$ với mọi $k$.

**Case 3** áp dụng (với điều kiện chính quy: $4(n/2)! \le c \cdot n!$ — vì $(n/2)! \ll n!$ khi $n$ lớn, điều kiện thỏa với $c = 1/2$ hoặc nhỏ hơn):

$$
T(n) = \Theta(n!)
$$

**Nhận xét:** Khi $f(n)$ tăng cực nhanh (như $n!$), nó **áp đảo hoàn toàn** phần đệ quy — tầng gốc chi phối.

---

## 6. Nhóm 5 — Căn thức và đổi biến

### Ví dụ 20 — $T(n) = 2T(\sqrt{n}) + 1$

**Đổi biến:** $m = \log_2 n$ ⇒ $n = 2^m$. Đặt $S(m) = T(2^m)$:

$$
S(m) = 2S(m/2) + 1
$$

Master Theorem: $a = 2$, $b = 2$, $x = m$, $f(m) = 1 = O(m^{1-\varepsilon})$ ⇒ **Case 1**:

$$
S(m) = \Theta(m)
$$

**Quay lại:**

$$
T(n) = S(\log n) = \Theta(\log n)
$$

---

### Ví dụ 21 — $T(n) = T(\sqrt{n}) + 1$

**Đổi biến:** $S(m) = T(2^m)$ ⇒ $S(m) = S(m/2) + 1$.

Master Theorem Case 2: $S(m) = \Theta(\log m)$.

**Quay lại:**

$$
T(n) = \Theta(\log\log n)
$$

**Ứng dụng:** Một số thuật toán số học (tính gcd nhanh, kiểm tra số nguyên tố xác suất).

---

### Ví dụ 22 — $T(n) = 3T(\sqrt{n}) + \log n$

**Đổi biến:** $S(m) = T(2^m)$ ⇒ $S(m) = 3S(m/2) + m$.

Master Theorem: $a = 3$, $b = 2$, $x = m^{\log_2 3} \approx m^{1.585}$, $f(m) = m = O(m^{1.585-\varepsilon})$ ⇒ **Case 1**:

$$
S(m) = \Theta(m^{\log_2 3})
$$

**Quay lại:**

$$
T(n) = \Theta((\log n)^{\log_2 3})
$$

---

### Ví dụ 23 — $T(n) = T(\sqrt{n}) + T(n - \sqrt{n}) + n$ (dạng hiếm)

**Phân tích:** Một bài toán con nhỏ ($\sqrt{n}$), một bài toán gần $n$. Dạng này **không thuộc** cả hai loại chuẩn.

**Chặn dưới:** $T(n) \ge T(n - \sqrt{n}) + n$ — lặp lại khoảng $\sqrt{n}$ lần để giảm $n$ đi $n - n^{1/2}$, rồi $n^{1/2} - n^{1/4}$, ... — tổng chi phí $\approx n\sqrt{n} = n^{1.5}$.

**Kết quả (chấp nhận):**

$$
T(n) = \Theta(n^{1.5})
$$

**Lưu ý:** Đây là trường hợp cần **Akra–Bazzi** hoặc phân tích trực tiếp.

---

## 7. Nhóm 6 — $f(n)$ không thuộc dạng chuẩn

### Ví dụ 24 — $T(n) = 2T(n/2) + n\log n$

**Master Theorem không áp dụng** ($n\log n$ nằm giữa $n$ và $n^{1+\varepsilon}$).

**Cây đệ quy:** Tầng $k$ chi phí $n(\log n - k)$. Số tầng $\log n$.

$$
T(n) = n\sum_{j=0}^{\log n} j = n \cdot \frac{\log n(\log n + 1)}{2} = \Theta(n\log^2 n)
$$

**Quy tắc ngón tay:** Nếu $f(n) = n^{\log_b a}\log^k n$, thì $T(n) = \Theta(n^{\log_b a}\log^{k+1} n)$.

Ở đây: $n^{\log_2 2} \log^1 n$ ⇒ $T(n) = \Theta(n\log^2 n)$. ✓

---

### Ví dụ 25 — $T(n) = 2T(n/2) + n/\log n$

**Master Theorem không áp dụng** ($n/\log n$ nhỏ hơn $n$ nhưng lớn hơn $n^{1-\varepsilon}$).

**Cây đệ quy:** Tầng $k$ chi phí $\frac{n/2^k}{\log(n/2^k)} \cdot 2^k = \frac{n}{\log n - k}$.

$$
T(n) = \sum_{k=0}^{\log n} \frac{n}{\log n - k} = n \sum_{j=1}^{\log n} \frac{1}{j} = n H_{\log n} = \Theta(n\log\log n)
$$

**Kết quả:**

$$
T(n) = \Theta(n\log\log n)
$$

**Lưu ý:** Đây là trường hợp **rất hiếm** — kết quả có $\log\log$.

---

### Ví dụ 26 — $T(n) = \sqrt{2}\,T(n/2) + \sqrt{n}$

**Phân tích:** Hệ số $a = \sqrt{2}$ không nguyên — nhưng Master Theorem vẫn áp dụng (với $a > 0$ thực).

$x = n^{\log_2 \sqrt{2}} = n^{0.5} = \sqrt{n}$, $f(n) = \sqrt{n} = \Theta(x)$ ⇒ **Case 2**:

$$
T(n) = \Theta(\sqrt{n}\log n)
$$

---

### Ví dụ 27 — $T(n) = T(n/2) + n/\log n$

**Master Theorem không áp dụng** ($f(n) = n/\log n$ giữa $1$ và $n$).

**Cây đệ quy:** Tầng $k$ chi phí $\frac{n/2^k}{\log(n/2^k)}$.

$$
T(n) = \sum_{k=0}^{\log n} \frac{n/2^k}{\log n - k}
$$

Đặt $j = \log n - k$:

$$
= n \sum_{j=1}^{\log n} \frac{1}{2^{\log n - j} \cdot j} = \frac{n}{n} \sum_{j=1}^{\log n} \frac{2^j}{j} = \sum_{j=1}^{\log n} \frac{2^j}{j}
$$

Số hạng cuối áp đảo: $\frac{2^{\log n}}{\log n} = \frac{n}{\log n}$.

$$
T(n) = \Theta\left(\frac{n}{\log n}\right)
$$

**Nhận xét:** Kết quả **nhỏ hơn** $n$ — bất ngờ! Vì $f(n)$ giảm nhanh khi chia đôi.

---

## 8. Nhóm 7 — Hệ thức nhiều tham số

### Ví dụ 28 — $T(n, m) = T(n-1, m) + T(n, m-1)$

**Đây là hệ thức của tổ hợp $\binom{n+m}{n}$.**

**Base case:** $T(n, 0) = T(0, m) = 1$.

**Nghiệm:** $T(n, m) = \binom{n+m}{n}$.

**Kết quả tiệm cận:** Với $m = n$:

$$
T(n, n) = \binom{2n}{n} = \Theta\left(\frac{4^n}{\sqrt{n}}\right)
$$

(Dùng xấp xỉ Stirling.)

---

### Ví dụ 29 — $T(n, m) = T(n-1, m) + m$

**Unrolling theo $n$:**

$$
T(n, m) = T(0, m) + nm = \Theta(nm)
$$

**Ứng dụng:** Duyệt ma trận $n \times m$ theo cột hoặc hàng.

---

### Ví dụ 30 — $T(n, m) = 2T(n/2, m) + m$

**Coi $m$ là hằng số:** Master Theorem với $a=2, b=2, x=n$:

$$
T(n, m) = \Theta(nm)
$$

**Ứng dụng:** Merge Sort trên $n$ phần tử, mỗi phần tử có kích thước $m$ — chi phí so sánh/trộn là $m$ mỗi lần.

---

### Ví dụ 31 — $T(n, m) = T(n-1, m) + T(n-1, m-1) + 1$

**Đây là hệ thức của tam giác Pascal.**

**Nghiệm:** $T(n, m) = \binom{n}{m}$.

**Với $m = n/2$:** $\binom{n}{n/2} = \Theta(2^n / \sqrt{n})$.

**Ứng dụng:** Một số bài toán quy hoạch động (đếm tổ hợp con).

---

## 9. Nhóm 8 — Hệ thức từ thuật toán thực tế

### Ví dụ 32 — Karatsuba Multiplication

$$
T(n) = 3T(n/2) + \Theta(n)
$$

**Master Theorem:** $a=3$, $b=2$, $x=n^{\log_2 3} \approx n^{1.585}$, $f(n)=n = O(n^{1.585-\varepsilon})$ ⇒ **Case 1**:

$$
T(n) = \Theta(n^{\log_2 3}) \approx \Theta(n^{1.585})
$$

**So sánh:** Nhân số nguyên naive là $\Theta(n^2)$ — Karatsuba nhanh hơn đáng kể.

---

### Ví dụ 33 — FFT (Fast Fourier Transform)

$$
T(n) = 2T(n/2) + \Theta(n)
$$

**Case 2:** $T(n) = \Theta(n\log n)$.

**Ứng dụng:** Nhân đa thức, xử lý tín hiệu, nén ảnh.

---

### Ví dụ 34 — Closest Pair of Points (1D)

$$
T(n) = 2T(n/2) + \Theta(n\log n)
$$

**Đây là trường hợp $f(n) = n\log n$** — Master Theorem không áp dụng trực tiếp.

**Quy tắc ngón tay** (với $f(n) = n\log^k n$): $T(n) = \Theta(n\log^{k+1} n)$.

Ở đây $k=1$ ⇒ $T(n) = \Theta(n\log^2 n)$.

**Thực tế:** Nếu bước chia/trộn là $\Theta(n\log n)$ (sắp xếp trước khi chia), tổng là $n\log^2 n$. Nhưng tối ưu hơn (sắp xếp một lần) cho $O(n\log n)$.

---

### Ví dụ 35 — Tìm phần tử lớn thứ hai (tournament)

$$
T(n) = T(n-1) + 1 \quad \text{(đơn giản: } \Theta(n)\text{)}
$$

Nhưng **tối ưu** là dùng tournament:

- Tìm max: $n-1$ phép so sánh.
- Tìm second max: cần so sánh với max — max đã thắng $\lceil \log_2 n \rceil$ lần.
- Tổng: $n - 1 + \lceil \log_2 n \rceil - 1$.

$$
T(n) = n + \lceil \log_2 n \rceil - 2 = \Theta(n)
$$

**Đây không phải hệ thức đệ quy** — nhưng là ví dụ về **cải thiện hằng số** so với $2n - 3$.

---

### Ví dụ 36 — Binary Search trên cây cân bằng

$$
T(h) = T(h-1) + \Theta(1)
$$

Với $h$ là chiều cao cây, $n = 2^{h+1} - 1$ (cây đầy đủ).

**Unrolling:** $T(h) = \Theta(h) = \Theta(\log n)$.

**Tương tự Binary Search** — nhưng hệ thức theo **chiều cao**, không theo $n$.

---

### Ví dụ 37 — Duyệt cây nhị phân

```text
Traverse(node):
    if node == null: return
    Traverse(node.left)
    Traverse(node.right)
    visit(node)
```

Với $n$ nút:

$$
T(n) = T(k) + T(n - k - 1) + \Theta(1)
$$

với $k$ là số nút cây con trái.

**Worst case** (cây suy biến, $k = n-1$):

$$
T(n) = T(n-1) + \Theta(1) \Rightarrow T(n) = \Theta(n)
$$

**Best case** (cây cân bằng, $k \approx n/2$):

$$
T(n) = 2T(n/2) + \Theta(1) \Rightarrow T(n) = \Theta(n)
$$

**Kết quả:** Duyệt cây luôn $\Theta(n)$ bất kể hình dạng — vì mỗi nút thăm đúng một lần.

---

## 10. Nhóm 9 — Hệ thức đặc biệt

### Ví dụ 38 — $T(n) = T(n/2) + T(n/4) + T(n/8) + n$

**Cây đệ quy:** Mỗi tầng tổng kích thước $n/2 + n/4 + n/8 = 7n/8$ — giảm dần.

$$
T(n) \le n \sum_{k=0}^{\infty} \left(\frac{7}{8}\right)^k = n \cdot \frac{1}{1 - 7/8} = 8n
$$

**Kết quả:**

$$
T(n) = \Theta(n)
$$

**Nhận xét:** Dù có 3 bài toán con, tổng kích thước giảm ($7/8 n$) ⇒ tầng gốc áp đảo.

---

### Ví dụ 39 — $T(n) = T(n/2) + T(n/4) + n$ (đã xét ở Ví dụ 8)

Tương tự, $T(n) = \Theta(n)$.

---

### Ví dụ 40 — $T(n) = T(n/2) + 2T(n/4) + n$

Tầng 0: $n$. Tầng 1: $n/2 + 2(n/4) = n/2 + n/2 = n$. Tầng 2: tương tự...

**Quan sát:** Mỗi tầng chi phí **không đổi** = $n$ (có thể chứng minh bằng quy nạp).

Số tầng: $\log_4 n$ đến $\log_2 n$ — khoảng $\Theta(\log n)$ tầng.

**Kết quả:**

$$
T(n) = \Theta(n\log n)
$$

---

### Ví dụ 41 — $T(n) = \sqrt{n}\,T(\sqrt{n}) + n$

**Đổi biến:** $S(m) = T(2^m)/2^m$ (chia cả hai vế cho $n$):

Đặt $n = 2^m$:

$$
T(2^m) = 2^{m/2} T(2^{m/2}) + 2^m
$$

$$
S(m) = \frac{T(2^m)}{2^m} = \frac{2^{m/2} T(2^{m/2})}{2^m} + 1 = \frac{T(2^{m/2})}{2^{m/2}} + 1 = S(m/2) + 1
$$

**Giải:** $S(m) = \Theta(\log m)$.

**Quay lại:** $T(n) = n \cdot S(\log n) = n\log\log n$.

$$
T(n) = \Theta(n\log\log n)
$$

---

### Ví dụ 42 — $T(n) = T(n-1) + \frac{1}{n}$

**Unrolling:**

$$
T(n) = T(1) + \sum_{k=2}^{n} \frac{1}{k} = \Theta(\log n)
$$

**Ứng dụng:** Một số phân tích amortized — chi phí biến đổi nhưng tổng harmonic.

---

### Ví dụ 43 — $T(n) = T(n-1) + \frac{1}{2^n}$

**Unrolling:**

$$
T(n) = \sum_{k=1}^{n} \frac{1}{2^k} < \sum_{k=1}^{\infty} \frac{1}{2^k} = 1
$$

**Kết quả:**

$$
T(n) = \Theta(1)
$$

**Nhận xét:** Tổng **hội tụ** — dù có $n$ số hạng, tổng bị chặn bởi hằng số. Đây là dạng chuỗi hội tụ.

---

## 11. Nhóm 10 — Bẫy Master Theorem

### Ví dụ 44 — $T(n) = 2T(n/2) + n/\sqrt{\log n}$

**Master Theorem không áp dụng** — vì $n/\sqrt{\log n}$ không so sánh được với $n$ qua $\varepsilon$ (nó lớn hơn $n^{1-\varepsilon}$ với mọi $\varepsilon > 0$ nhưng nhỏ hơn $n$).

**Quy tắc ngón tay** (dạng mở rộng): Khi $f(n) = n \cdot g(n)$ với $g$ giảm chậm, kết quả thường là $\Theta(n \cdot h(n))$ với $h$ liên quan đến tích phân của $g$.

Với $g(n) = 1/\sqrt{\log n}$, có thể chứng minh:

$$
T(n) = \Theta(n\sqrt{\log n})
$$

---

### Ví dụ 45 — $T(n) = 2T(n/2) + n \cdot 2^{\sqrt{\log n}}$

**Master Theorem không áp dụng** — $f(n)$ không có dạng $n^k$.

$f(n) = n \cdot 2^{\sqrt{\log n}} = \omega(n \log^k n)$ với mọi $k$ nhưng $o(n^{1+\varepsilon})$ với mọi $\varepsilon > 0$.

**Kết quả (chấp nhận):**

$$
T(n) = \Theta(f(n)) = \Theta(n \cdot 2^{\sqrt{\log n}})
$$

Đây là trường hợp **Case 3 mở rộng** — nhưng không thuộc Master Theorem chuẩn.

---

### Ví dụ 46 — $T(n) = T(n/2) + n^2 / \log n$

**Master Theorem không áp dụng** — $f(n) = n^2/\log n$ không phải $\Omega(n^{1+\varepsilon})$ (vì $\log$ chia).

**Kết quả (chấp nhận):**

$$
T(n) = \Theta(n^2)
$$

Vì $f(n)$ vẫn là bậc hai (chỉ nhỏ hơn bởi hằng số log).

---

## 12. Nhóm 11 — Hệ thức với điều kiện base case đặc biệt

### Ví dụ 47 — $T(n) = T(n/2) + 1$, $T(2) = 1$

**Unrolling:**

$$
T(n) = 1 + \log_2(n/2) = \log_2 n - 1 = \Theta(\log n)
$$

**Lưu ý:** Base case $T(2) = 1$ (không phải $T(1) = 1$) — số tầng là $\log_2 n - 1$ (thay vì $\log_2 n$). Bậc **không đổi**.

---

### Ví dụ 48 — $T(n) = 2T(n/2) + n$, $T(1) = 1$ nhưng $n$ không phải lũy thừa của 2

**Xử lý:** Đặt $n' = 2^{\lceil \log_2 n \rceil}$ (làm tròn lên lũy thừa 2 gần nhất).

$$
T(n) \le T(n') \Rightarrow T(n) = O(n'\log n') = O(2n \log(2n)) = O(n\log n)
$$

**Tương tự** chặn dưới với $n'' = 2^{\lfloor \log_2 n \rfloor}$ ⇒ $\Omega(n\log n)$.

**Kết quả:** $\Theta(n\log n)$ — **bậc không đổi**.

**Bài học:** Trong thực tế, làm tròn $n$ lên/xuống lũy thừa gần nhất **không ảnh hưởng** đến $\Theta$ — vì chỉ thay đổi hằng số.

---

## 13. Bảng tổng hợp — 48 ví dụ

| #   | Hệ thức                               | $T(n)$                              | Phương pháp         |
| --- | ------------------------------------- | ----------------------------------- | ------------------- |
| 1   | $2T(n/2) + n^2$                       | $\Theta(n^2)$                       | MT Case 3           |
| 2   | $4T(n/2) + n$                         | $\Theta(n^2)$                       | MT Case 1           |
| 3   | $3T(n/2) + n^2$                       | $\Theta(n^2)$                       | MT Case 3           |
| 4   | $9T(n/3) + n^2$                       | $\Theta(n^2\log n)$                 | MT Case 2           |
| 5   | $8T(n/4) + n^{1.5}$                   | $\Theta(n^{1.5}\log n)$             | MT Case 2           |
| 6   | $T(n/3) + T(2n/3) + n$                | $\Theta(n\log n)$                   | Cây đệ quy          |
| 7   | $T(n/10) + T(9n/10) + n$              | $\Theta(n\log n)$                   | Cây đệ quy          |
| 8   | $T(n/2) + T(n/4) + n$                 | $\Theta(n)$                         | Cây đệ quy          |
| 9   | $T(n-1) + 1$                          | $\Theta(n)$                         | Unrolling           |
| 10  | $T(n-1) + n$                          | $\Theta(n^2)$                       | Unrolling           |
| 11  | $2T(n-1) + 1$                         | $\Theta(2^n)$                       | Unrolling           |
| 12  | $3T(n-1) + 2^n$                       | $\Theta(3^n)$                       | Unrolling           |
| 13  | $2T(n-1) + n$                         | $\Theta(2^n)$                       | Unrolling           |
| 14  | $T(n-2) + n$                          | $\Theta(n^2)$                       | Unrolling           |
| 15  | $T(n-1) + \log n$                     | $\Theta(n\log n)$                   | Unrolling           |
| 16  | $T(n-1) + T(n-2) + 1$                 | $\Theta(\varphi^n)$                 | Đặc trưng           |
| 17  | $T(n-1) + T(n-3)$                     | $\Theta(1.4656^n)$                  | Đặc trưng           |
| 18  | $2T(n-2) + 1$                         | $\Theta(\sqrt{2}^n)$                | Đổi biến            |
| 19  | $4T(n/2) + n!$                        | $\Theta(n!)$                        | MT Case 3           |
| 20  | $2T(\sqrt{n}) + 1$                    | $\Theta(\log n)$                    | Đổi biến + MT       |
| 21  | $T(\sqrt{n}) + 1$                     | $\Theta(\log\log n)$                | Đổi biến + MT       |
| 22  | $3T(\sqrt{n}) + \log n$               | $\Theta((\log n)^{\log_2 3})$       | Đổi biến + MT       |
| 23  | $T(\sqrt{n}) + T(n-\sqrt{n}) + n$     | $\Theta(n^{1.5})$                   | Phân tích trực tiếp |
| 24  | $2T(n/2) + n\log n$                   | $\Theta(n\log^2 n)$                 | Cây đệ quy          |
| 25  | $2T(n/2) + n/\log n$                  | $\Theta(n\log\log n)$               | Cây đệ quy          |
| 26  | $\sqrt{2}T(n/2) + \sqrt{n}$           | $\Theta(\sqrt{n}\log n)$            | MT Case 2           |
| 27  | $T(n/2) + n/\log n$                   | $\Theta(n/\log n)$                  | Cây đệ quy          |
| 28  | $T(n-1,m) + T(n,m-1)$                 | $\Theta\binom{n+m}{n}$              | Tổ hợp              |
| 29  | $T(n-1,m) + m$                        | $\Theta(nm)$                        | Unrolling           |
| 30  | $2T(n/2,m) + m$                       | $\Theta(nm)$                        | MT                  |
| 31  | $T(n-1,m) + T(n-1,m-1) + 1$           | $\binom{n}{m}$                      | Pascal              |
| 32  | $3T(n/2) + n$                         | $\Theta(n^{\log_2 3})$              | Karatsuba           |
| 33  | $2T(n/2) + n$                         | $\Theta(n\log n)$                   | FFT                 |
| 34  | $2T(n/2) + n\log n$                   | $\Theta(n\log^2 n)$                 | Closest Pair        |
| 35  | $T(h-1) + 1$                          | $\Theta(\log n)$                    | Cây                 |
| 36  | $T(k) + T(n-k-1) + 1$                 | $\Theta(n)$                         | Duyệt cây           |
| 37  | $T(n/2) + T(n/4) + T(n/8) + n$        | $\Theta(n)$                         | Cây đệ quy          |
| 38  | $T(n/2) + 2T(n/4) + n$                | $\Theta(n\log n)$                   | Cây đệ quy          |
| 39  | $\sqrt{n}T(\sqrt{n}) + n$             | $\Theta(n\log\log n)$               | Đổi biến            |
| 40  | $T(n-1) + 1/n$                        | $\Theta(\log n)$                    | Harmonic            |
| 41  | $T(n-1) + 1/2^n$                      | $\Theta(1)$                         | Chuỗi hội tụ        |
| 42  | $2T(n/2) + n/\sqrt{\log n}$           | $\Theta(n\sqrt{\log n})$            | Mở rộng             |
| 43  | $2T(n/2) + n \cdot 2^{\sqrt{\log n}}$ | $\Theta(n \cdot 2^{\sqrt{\log n}})$ | Case 3 mở rộng      |
| 44  | $T(n/2) + n^2/\log n$                 | $\Theta(n^2)$                       | Chặn trực tiếp      |
| 45  | $T(n/2) + 1$, $T(2)=1$                | $\Theta(\log n)$                    | Unrolling           |
| 46  | $2T(n/2)+n$, $n$ không lũy thừa 2     | $\Theta(n\log n)$                   | Làm tròn            |

---

## 14. Quy tắc nhận diện nhanh

### 14.1. Theo dạng hệ thức

| Thấy dạng                             | Dùng                   |
| ------------------------------------- | ---------------------- |
| $aT(n/b) + f(n)$                      | Master Theorem         |
| $T(n-1) + f(n)$                       | Unrolling              |
| $T(n-1) + T(n-2)$                     | Phương trình đặc trưng |
| $aT(n-1) + f(n)$                      | Unrolling              |
| $T(\sqrt{n}) + f(n)$                  | Đổi biến $m = \log n$  |
| $T(\alpha n) + T((1-\alpha)n) + f(n)$ | Cây đệ quy             |
| $T(n,m) = \ldots$                     | Phân tích hai chiều    |

### 14.2. Theo $f(n)$

| $f(n)$     | Nhận xét                                                        |
| ---------- | --------------------------------------------------------------- |
| $1$        | Thường $\Theta(\text{số lá})$                                   |
| $n$        | Thường $\Theta(n\log n)$ nếu $a=b$, hoặc $\Theta(n^{\log_b a})$ |
| $n^2$      | Thường $\Theta(n^2)$ hoặc $\Theta(n^2\log n)$                   |
| $n\log n$  | $\Theta(n\log^2 n)$ (nếu $a=b$)                                 |
| $n/\log n$ | Thường $\Theta(n\log\log n)$                                    |
| $n!$       | Áp đảo hoàn toàn                                                |

### 14.3. Ba quy tắc ngón tay

**Quy tắc 1 — $f(n) = n^{\log_b a}\log^k n$:**

$$
T(n) = \Theta(n^{\log_b a}\log^{k+1} n)
$$

**Quy tắc 2 — $f(n)$ tăng cực nhanh (như $n!$):**

$$
T(n) = \Theta(f(n))
$$

**Quy tắc 3 — $f(n)$ giảm (như $1/2^n$):**

$$
T(n) = \Theta(f(n)) \text{ hoặc } \Theta(1)
$$

---

## 15. Bài tập tự luyện (có đáp án)

**Bài 1:** $T(n) = 5T(n/2) + n^2$

*Đáp:* $a=5$, $b=2$, $x=n^{\log_2 5} \approx n^{2.322}$, $f(n)=n^2 = O(n^{2.322-\varepsilon})$ ⇒ Case 1 ⇒ $\Theta(n^{\log_2 5})$.

**Bài 2:** $T(n) = 2T(n/2) + \sqrt{n}$

*Đáp:* $x = n$, $f(n) = n^{0.5} = O(n^{1-\varepsilon})$ ⇒ Case 1 ⇒ $\Theta(n)$.

**Bài 3:** $T(n) = T(n/2) + n^2$

*Đáp:* $x = 1$, $f(n) = n^2 = \Omega(n^{0+\varepsilon})$ ⇒ Case 3 ⇒ $\Theta(n^2)$.

**Bài 4:** $T(n) = 4T(n/4) + n$

*Đáp:* $a=4$, $b=4$, $x = n^1 = n$, $f(n) = n = \Theta(x)$ ⇒ Case 2 ⇒ $\Theta(n\log n)$.

**Bài 5:** $T(n) = T(n-1) + n^3$

*Đáp:* Unrolling ⇒ $\sum k^3 = \Theta(n^4)$.

**Bài 6:** $T(n) = 3T(n-2) + 1$

*Đáp:* Đổi biến $m=n/2$: $S(m) = 3S(m-1) + 1$ ⇒ $S(m) = \Theta(3^m)$ ⇒ $T(n) = \Theta(3^{n/2}) = \Theta(\sqrt{3}^n)$.

**Bài 7:** $T(n) = 2T(\sqrt{n}) + \log\log n$

*Đáp:* Đổi biến $m = \log n$: $S(m) = 2S(m/2) + \log m$ ⇒ Case 2 mở rộng ⇒ $S(m) = \Theta(m\log m)$ ⇒ $T(n) = \Theta(\log n \cdot \log\log n)$.

**Bài 8:** $T(n) = T(n/2) + T(n/4) + T(n/8) + T(n/16) + n$

*Đáp:* Tầng 0: $n$. Tầng 1: $n(1/2+1/4+1/8+1/16) = 15n/16$. Tổng $\le 16n$ ⇒ $\Theta(n)$.

**Bài 9:** $T(n) = T(\lfloor n/2 \rfloor) + T(\lceil n/2 \rceil) + n$

*Đáp:* Đây là Merge Sort với làm tròn — vẫn $\Theta(n\log n)$.

**Bài 10:** $T(n) = 2T(n/3) + T(2n/3) + n$

*Đáp:* Mỗi tầng: $n/3 + n/3 + 2n/3 = 4n/3 > n$ — **tăng**. Số tầng $\log_{3/2} n$ (nhánh dài). Kết quả: $\Theta(n\log n)$ (tăng nhưng chậm — vẫn $\log$).

---

## 16. Tóm tắt — 10 quy tắc vàng

1. **$aT(n/b) + f(n)$** → Master Theorem (kiểm tra 3 case).
2. **$T(n-1) + f(n)$** → Unrolling (tổng $f$).
3. **$T(n-1) + T(n-2)$** → Phương trình đặc trưng (nghiệm $\varphi$).
4. **$T(\sqrt{n}) + f(n)$** → Đổi biến $m = \log n$.
5. **$T(\alpha n) + T((1-\alpha)n) + n$** → Cây đệ quy ($\Theta(n\log n)$ nếu $\alpha$ cố định).
6. **$f(n) = n^{\log_b a}\log^k n$** → $\Theta(n^{\log_b a}\log^{k+1} n)$.
7. **$f(n) = n!$** → $\Theta(f(n))$ (áp đảo hoàn toàn).
8. **$f(n) = 1/2^n$** → $\Theta(1)$ (chuỗi hội tụ).
9. **Hệ thức nhiều tham số** → phân tích hai chiều.
10. **Không chắc** → vẽ cây đệ quy, rồi chứng minh bằng phương pháp thế.

---

Nếu bạn muốn, mình có thể đi sâu:
- **Akra–Bazzi** — công thức tổng quát hơn Master Theorem cho hệ thức không đều.
- **Phương trình đặc trưng** — lý thuyết đầy đủ cho hệ thức tuyến tính thuần nhất.
- **Bài tập khó** — hệ thức với $f(n)$ dao động, hệ thức không thuần nhất bậc cao.
- **Ứng dụng** — phân tích các thuật toán cụ thể (Strassen, FFT, Karatsuba, closest pair).