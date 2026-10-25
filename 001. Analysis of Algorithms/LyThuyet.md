# Phân tích giải thuật (Analysis of Algorithms) — lý thuyết chi tiết + ví dụ kèm giải thích

## 1. Phân tích giải thuật là gì?

**Phân tích giải thuật** là việc đánh giá một giải thuật theo hai mặt chính:

1. **Tính đúng đắn (correctness):** giải thuật có luôn cho kết quả đúng với mọi đầu vào hợp lệ không?
2. **Hiệu quả tài nguyên (resource efficiency):**
   - **Thời gian chạy:** số phép toán cơ bản cần thực hiện, biểu diễn theo kích thước đầu vào $n$.
   - **Bộ nhớ:** lượng bộ nhớ phụ cần dùng, cũng biểu diễn theo $n$.

Ta thường không đo bằng “giây” vì phụ thuộc máy, ngôn ngữ, trình biên dịch. Thay vào đó, ta xem thời gian chạy là một **hàm của kích thước đầu vào**:

$$
T(n) = \text{số phép toán cơ bản khi đầu vào kích thước } n
$$

### Các trường hợp phân tích

- **Best case:** đầu vào thuận lợi nhất.
- **Worst case:** đầu vào bất lợi nhất — thường được dùng nhất vì cho “bảo đảm” trên.
- **Average case:** trung bình trên một phân bố đầu vào nào đó — cần giả định phân bố xác suất.

> **Lý thuyết bên cạnh:** Worst-case thường được ưu tiên vì nó cho cận trên đáng tin cậy. Average-case sát thực tế hơn nhưng phải nêu rõ giả định phân bố. Best-case ít dùng một mình vì không phản ánh hành vi điển hình.

---

## 2. Ký hiệu tiệm cận: $O, \Omega, \Theta$

### 2.1. Định nghĩa hình thức

Cho $f(n), g(n) \ge 0$.

- **$f(n) = O(g(n))$**: tồn tại $c>0, n_0$ sao cho
  $$
  0 \le f(n) \le c g(n), \quad \forall n \ge n_0
  $$
  Nghĩa là $g(n)$ là **cận trên** của $f(n)$ khi $n$ lớn.

- **$f(n) = \Omega(g(n))$**: tồn tại $c>0, n_0$ sao cho
  $$
  0 \le c g(n) \le f(n), \quad \forall n \ge n_0
  $$
  Nghĩa là $g(n)$ là **cận dưới** của $f(n)$.

- **$f(n) = \Theta(g(n))$**: $f(n)=O(g(n))$ và $f(n)=\Omega(g(n))$.
  Nghĩa là $f(n)$ và $g(n)$ **cùng bậc tăng**.

- **$f(n) = o(g(n))$**: $f(n)/g(n) \to 0$.
- **$f(n) = \omega(g(n))$**: $f(n)/g(n) \to \infty$.

### 2.2. Quy tắc thực hành

Khi phân tích tiệm cận:

1. **Bỏ hằng số:** $3n^2 = \Theta(n^2)$.
2. **Bỏ số hạng bậc thấp:** $n^2 + 5n + 7 = \Theta(n^2)$.
3. **Cơ số log không quan trọng:** $\log_2 n = \Theta(\log_{10} n)$.
4. Chỉ giữ **số hạng tăng nhanh nhất**.

### Bảng độ phức tạp thường gặp

| Độ phức tạp | Tên | Ví dụ |
|---|---|---|
| $O(1)$ | hằng số | truy cập mảng |
| $O(\log n)$ | logarithm | binary search |
| $O(n)$ | tuyến tính | duyệt mảng |
| $O(n\log n)$ | gần tuyến tính | merge sort |
| $O(n^2)$ | bậc hai | insertion sort worst |
| $O(2^n)$ | mũ | Fibonacci đệ quy naive |
| $O(n!)$ | giai thừa | liệt kê hoán vị |

> **Lý thuyết bên cạnh:** $O$ là “không lớn hơn về bậc”; $\Omega$ là “không nhỏ hơn về bậc”; $\Theta$ là “đúng bậc”. Trong tài liệu thực tế, người ta đôi khi dùng $O$ một cách lỏng lẻo để chỉ “độ phức tạp là…”.

---

## 3. Quy trình phân tích một giải thuật

1. Xác định **kích thước đầu vào** $n$.
2. Chọn **phép toán cơ bản** cần đếm: so sánh, gán, cộng, truy cập mảng…
3. Đếm số lần phép toán đó theo $n$.
4. Đơn giản bằng ký hiệu tiệm cận.
5. Xét **best/worst/average** nếu cần.
6. Phân tích thêm **bộ nhớ phụ**.

> **Lý thuyết bên cạnh:** Phân tích tiệm cận tập trung vào $n$ lớn. Với $n$ nhỏ, hằng số và chi phí thực tế có thể quan trọng, nhưng phân tích tiệm cận giúp so sánh giải thuật một cách độc lập máy.

---

# 4. Ví dụ chi tiết kèm lý thuyết

## Ví dụ 1 — Linear Search (tìm kiếm tuần tự)

### Mã giả

```text
LinearSearch(A, n, x):
    for i = 1 to n:
        if A[i] == x:
            return i
    return -1
```

### Phân tích

Phép toán cơ bản: so sánh `A[i] == x`.

- **Best case:** $x$ nằm ở vị trí đầu tiên.
  $$
  T_{\text{best}}(n) = 1 = \Theta(1)
  $$
- **Worst case:** $x$ nằm cuối hoặc không có trong mảng.
  $$
  T_{\text{worst}}(n) = n = \Theta(n)
  $$
- **Average case:** nếu $x$ có mặt và mỗi vị trí đồng khả năng:
  $$
  \frac{1+2+\cdots+n}{n} = \frac{n+1}{2} = \Theta(n)
  $$
  Nếu $x$ không có mặt thì luôn $n$ phép so sánh.

**Bộ nhớ phụ:** $O(1)$.

> **Lý thuyết bên cạnh:** Cùng một giải thuật có thể có best-case rất nhanh nhưng worst-case và average-case vẫn tuyến tính. Vì vậy không thể chỉ nói “linear search là $O(1)$” dựa trên best-case.

---

## Ví dụ 2 — Insertion Sort

### Mã giả

```text
InsertionSort(A, n):
    for i = 2 to n:
        key = A[i]
        j = i - 1
        while j > 0 and A[j] > key:
            A[j+1] = A[j]
            j = j - 1
        A[j+1] = key
```

### Phân tích worst case

Worst case xảy ra khi mảng **giảm dần**. Mỗi phần tử $A[i]$ phải so sánh và dịch với tất cả $i-1$ phần tử trước đó.

Số phép so sánh/dịch xấp xỉ:

$$
\sum_{i=2}^{n} (i-1)
= 1+2+\cdots+(n-1)
= \frac{n(n-1)}{2}
= \Theta(n^2)
$$

### Best case

Mảng đã **tăng dần**. Mỗi vòng `while` không chạy. Chỉ có $n-1$ lần so sánh ngoài:

$$
T_{\text{best}}(n) = \Theta(n)
$$

### Average case

Trung bình khoảng một nửa số phần tử trước đó bị dịch:

$$
\Theta(n^2)
$$

**Bộ nhớ phụ:** $O(1)$ — sắp xếp tại chỗ.
**Tính chất:** ổn định (stable).

> **Lý thuyết bên cạnh:** Với giải thuật có vòng lặp lồng nhau, không phải lúc nào cũng $n^2$. Ở insertion sort, số lần lặp trong `while` **phụ thuộc dữ liệu**, nên best-case và worst-case khác nhau rõ rệt. Một cách nhìn khác: thời gian insertion sort tỉ lệ với số **nghịch thế** (inversions) của mảng.

---

## Ví dụ 3 — Hai vòng lặp lồng nhau

### Trường hợp A

```text
for i = 1 to n:
    for j = 1 to i:
        doing O(1) work
```

Số lần thực hiện:

$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \Theta(n^2)
$$

### Trường hợp B

```text
for i = 1 to n:
    j = 1
    while j < n:
        doing O(1) work
        j = j * 2
```

Với mỗi $i$, biến $j$ tăng gấp đôi: $1,2,4,\dots$. Số lần lặp là $\lceil \log_2 n \rceil$. Có $n$ vòng ngoài nên:

$$
T(n) = n \cdot \Theta(\log n) = \Theta(n\log n)
$$

> **Lý thuyết bên cạnh:** Phân tích vòng lặp phải đếm **số lần lặp thực tế**. Vòng lặp trong không nhất thiết chạy $n$ lần; nếu biến tăng theo cấp số nhân, số lần là logarit.

---

## Ví dụ 4 — Binary Search (tìm kiếm nhị phân)

### Mã giả

```text
BinarySearch(A, n, x):
    low = 1
    high = n
    while low <= high:
        mid = (low + high) / 2
        if A[mid] == x:
            return mid
        else if A[mid] < x:
            low = mid + 1
        else:
            high = mid - 1
    return -1
```

Điều kiện: mảng **đã sắp xếp**.

### Phân tích

Mỗi vòng lặp giảm kích thước bài toán còn một nửa:

$$
T(n) = T(n/2) + \Theta(1)
$$

Áp dụng **Master Theorem** với $a=1, b=2, f(n)=\Theta(1)$:

$$
n^{\log_b a} = n^{\log_2 1} = n^0 = 1
$$

Vì $f(n)=\Theta(1)=\Theta(n^{\log_b a})$, rơi vào **Case 2**:

$$
T(n) = \Theta(\log n)
$$

Cụ thể số vòng lặp tối đa là $\lfloor \log_2 n \rfloor + 1$.

- Best case: $x$ nằm ngay giữa — $O(1)$.
- Worst case: $O(\log n)$.
- Bộ nhớ phụ: $O(1)$ nếu viết lặp; $O(\log n)$ nếu viết đệ quy do ngăn xếp gọi.

> **Lý thuyết bên cạnh:** Binary search là ví dụ điển hình của **chia để trị / giảm kích thước một nửa**. Nó chỉ đúng khi dữ liệu đã sắp xếp. Nếu phải sắp xếp trước, chi phí sắp xếp có thể lớn hơn nhiều so với $\log n$.

---

## Ví dụ 5 — Merge Sort

### Ý tưởng

- Chia mảng làm hai nửa.
- Đệ quy sắp xếp từng nửa.
- Trộn hai nửa đã sắp xếp trong thời gian tuyến tính.

### Hệ thức truy hồi

$$
T(n) = 2T(n/2) + \Theta(n)
$$

### Giải bằng Master Theorem

$$
a=2,\quad b=2,\quad f(n)=\Theta(n)
$$

$$
n^{\log_b a} = n^{\log_2 2} = n
$$

Vì $f(n)=\Theta(n)=\Theta(n^{\log_b a})$, rơi vào **Case 2**:

$$
T(n) = \Theta(n\log n)
$$

### Giải bằng cây đệ quy

- Mỗi tầng có tổng chi phí trộn là $n$.
- Có khoảng $\log_2 n$ tầng.
- Tổng:
  $$
  n + n + \cdots + n \quad (\log n \text{ lần})
  = \Theta(n\log n)
  $$

**Bộ nhớ phụ:** $O(n)$ cho mảng trộn.
**Tính chất:** ổn định nếu cài đặt đúng.

> **Lý thuyết bên cạnh:** Với chia để trị, hệ thức dạng $T(n)=aT(n/b)+f(n)$ rất phổ biến. Master Theorem so sánh chi phí “chia và trị” $f(n)$ với $n^{\log_b a}$ — là chi phí của các bài toán con nếu chúng chiếm ưu thế.

---

## Ví dụ 6 — Fibonacci: đệ quy naive vs quy hoạch động

### Cách 1: Đệ quy naive

```text
Fib(n):
    if n <= 1: return n
    return Fib(n-1) + Fib(n-2)
```

Hệ thức:

$$
T(n) = T(n-1) + T(n-2) + O(1)
$$

Cây gọi đệ quy có số nút xấp xỉ $F_n$, với $F_n$ là số Fibonacci thứ $n$. Ta có:

$$
F_n = \Theta(\varphi^n), \quad
\varphi = \frac{1+\sqrt{5}}{2} \approx 1.618
$$

Do đó:

$$
T(n) = \Theta(\varphi^n) \quad \text{(tăng theo hàm mũ)}
$$

Rất nhiều bài toán con bị tính lặp lại.

### Cách 2: Memoization / quy hoạch động

Mỗi giá trị $Fib(0), Fib(1), \dots, Fib(n)$ chỉ tính một lần.

$$
T(n) = O(n)
$$
$$
S(n) = O(n) \quad \text{(mảng lưu hoặc đệ quy có memo)}
$$

### Cách 3: Lặp với hai biến

```text
FibIter(n):
    a = 0; b = 1
    for i = 1 to n:
        c = a + b
        a = b
        b = c
    return a
```

$$
T(n)=O(n), \quad S(n)=O(1)
$$

> **Lý thuyết bên cạnh:** Đây là ví dụ về **bài toán con chồng lấp** (overlapping subproblems). Đệ quy naive không sai nhưng cực kém hiệu quả. Memoization giảm từ hàm mũ xuống tuyến tính bằng cách lưu kết quả đã tính.

---

## Ví dụ 7 — Amortized analysis: mảng động (dynamic array)

Giả sử mảng động có sức chứa $capacity$. Khi đầy, ta cấp phát mảng mới gấp đôi và copy toàn bộ phần tử.

### Phân tích từng thao tác

- Thông thường `push` là $O(1)$.
- Khi mảng đầy, `push` là $O(n)$ do phải copy $n$ phần tử.

### Phân tích amortized

Xét $n$ lần `push`:

- Các lần copy xảy ra khi dung lượng là $1,2,4,8,\dots$
- Tổng chi phí copy:
  $$
  1+2+4+\cdots+2^k < 2n
  $$
- Tổng chi phí cho $n$ lần push là $O(n)$.

Vậy **chi phí amortized** mỗi lần push là:

$$
\frac{O(n)}{n} = O(1)
$$

> **Lý thuyết bên cạnh:** Amortized cost không phải worst-case của một thao tác đơn lẻ. Một lần push có thể là $O(n)$, nhưng trung bình trên một chuỗi thao tác là $O(1)$. Đây là lý thuyết quan trọng cho `vector`, `ArrayList`, `dynamic array`.

---

# 5. Hệ thức truy hồi và Master Theorem

Nhiều giải thuật đệ quy có dạng:

$$
T(n) = aT(n/b) + f(n)
$$

với $a \ge 1, b > 1$. Đặt:

$$
x = n^{\log_b a}
$$

### Master Theorem

1. Nếu $f(n) = O(n^{\log_b a - \varepsilon})$ với $\varepsilon>0$:
   $$
   T(n) = \Theta(n^{\log_b a})
   $$
2. Nếu $f(n) = \Theta(n^{\log_b a})$:
   $$
   T(n) = \Theta(n^{\log_b a}\log n)
   $$
3. Nếu $f(n) = \Omega(n^{\log_b a + \varepsilon})$ và có điều kiện chính quy $a f(n/b) \le c f(n)$ với $c<1$:
   $$
   T(n) = \Theta(f(n))
   $$

### Áp dụng nhanh

- Binary search: $T(n)=T(n/2)+\Theta(1)$
  $$
  x=1,\ f(n)=\Theta(1) \Rightarrow \Theta(\log n)
  $$

- Merge sort: $T(n)=2T(n/2)+\Theta(n)$
  $$
  x=n,\ f(n)=\Theta(n) \Rightarrow \Theta(n\log n)
  $$

> **Lý thuyết bên cạnh:** Master Theorem chỉ áp dụng cho một lớp hệ thức chia đều. Với hệ thức không đều, có thể dùng cây đệ quy, phương pháp thế, hoặc định lý Akra–Bazzi.

---

# 6. Một số lỗi hiểu nhầm thường gặp

1. **“$O(1)$ nghĩa là một phép toán”** — Sai. $O(1)$ nghĩa là số phép toán bị chặn bởi hằng số, không phụ thuộc $n$.
2. **“$O(n)$ luôn nhanh hơn $O(n^2)$ trong mọi trường hợp”** — Đúng về xu hướng khi $n$ lớn; với $n$ nhỏ, hằng số có thể khiến kết quả thực tế khác.
3. **Bỏ qua bộ nhớ** — Đệ quy có thể tốn $O(n)$ hoặc $O(\log n)$ stack.
4. **Nhầm amortized với worst-case** — Amortized là trung bình trên chuỗi thao tác; một thao tác đơn lẻ vẫn có thể đắt.
5. **Average case không có giả định** — Phải nói rõ phân bố đầu vào.

---

# 7. Tóm tắt checklist phân tích giải thuật

- Kích thước đầu vào là gì?
- Phép toán cơ bản là gì?
- Có vòng lặp/đệ quy không? Đếm số lần.
- Best/worst/average ra sao?
- Viết hệ thức truy hồi nếu đệ quy.
- Giải bằng Master Theorem / cây đệ quy / phương pháp thế.
- Độ phức tạp thời gian là gì?
- Bộ nhớ phụ là gì?
- Có tính chất đặc biệt: in-place, stable, amortized…?

Nếu bạn muốn, mình có thể tiếp tục với **bài tập có lời giải** hoặc **phân tích một giải thuật cụ thể** như Quick Sort, Heap Sort, Dijkstra, DFS/BFS, Dynamic Programming…