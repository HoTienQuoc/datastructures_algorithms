# Best, Average và Worst Cases — Lý thuyết chi tiết + Ví dụ

## 1. Đặt vấn đề: Tại sao cần phân biệt các trường hợp?

Cùng một thuật toán, với **cùng kích thước đầu vào** $n$, thời gian chạy có thể **rất khác nhau** tùy vào **giá trị cụ thể** của dữ liệu.

### Ví dụ mở đầu — Linear Search

```text
LinearSearch(A, n, x):
    for i = 1 to n:
        if A[i] == x:
            return i
    return -1
```

Xét mảng $A = [5, 3, 8, 1, 9, 2]$ ($n = 6$):

| Tìm $x$ | Vị trí   | Số phép so sánh       |
| ------- | -------- | --------------------- |
| $x = 5$ | 1        | **1** (tìm thấy ngay) |
| $x = 3$ | 2        | 2                     |
| $x = 8$ | 3        | 3                     |
| $x = 9$ | 5        | 5                     |
| $x = 7$ | không có | **6** (duyệt hết)     |

**Cùng thuật toán, cùng $n = 6$, nhưng số phép so sánh từ 1 đến 6.**

Vậy khi nói "Linear Search là $O(n)$" — đó là nói về **worst case** (trường hợp xấu nhất). Nhưng để hiểu đầy đủ, ta cần phân tích **cả ba trường hợp**: best, average, worst.

> **Lý thuyết bên cạnh:** Trong phân tích thuật toán, "case" (trường hợp) đề cập đến **tập hợp các đầu vào** có cùng đặc điểm ảnh hưởng đến hiệu năng. Không phải "input cụ thể" mà là **lớp input** với tính chất tương tự.

---

## 2. Định nghĩa ba trường hợp

### 2.1. Best Case (trường hợp tốt nhất)

**Định nghĩa:** Thời gian chạy **nhỏ nhất** trên tất cả các đầu vào kích thước $n$.

$$
T_{\text{best}}(n) = \min_{\text{input } I,\ |I| = n} T(I)
$$

**Ý nghĩa:** Cận dưới của thời gian chạy. Là "lạc quan nhất" — thường **ít dùng** để đánh giá thuật toán, vì không phản ánh hành vi điển hình.

**Khi nào hữu ích:**
- Chứng minh thuật toán **có thể** nhanh (không phải lúc nào cũng chậm).
- Phân tích thuật toán **adaptive** (tự điều chỉnh theo dữ liệu) — ví dụ Insertion Sort nhanh trên mảng gần sắp xếp.

### 2.2. Worst Case (trường hợp xấu nhất)

**Định nghĩa:** Thời gian chạy **lớn nhất** trên tất cả các đầu vào kích thước $n$.

$$
T_{\text{worst}}(n) = \max_{\text{input } I,\ |I| = n} T(I)
$$

**Ý nghĩa:** Cận trên của thời gian chạy. Là "bảo đảm" — thuật toán **không bao giờ** chậm hơn con số này.

**Khi nào hữu ích:**
- **Hầu hết các phân tích thuật toán** dùng worst case.
- Hệ thống **real-time** cần đảm bảo thời gian phản hồi.
- Hệ thống **an toàn** (security) — kẻ tấn công có thể cố tình tạo worst case (ví dụ hash collision attack).

### 2.3. Average Case (trường hợp trung bình)

**Định nghĩa:** Thời gian chạy **kỳ vọng** trên một **phân bố xác suất** của đầu vào.

$$
T_{\text{avg}}(n) = E[T(I)] = \sum_{I} P(I) \cdot T(I)
$$

với $P(I)$ là xác suất đầu vào $I$ xuất hiện.

**Ý nghĩa:** Phản ánh hành vi **điển hình** trong thực tế. Nhưng **phụ thuộc vào giả định phân bố** — nếu giả định sai, kết quả vô nghĩa.

**Khi nào hữu ích:**
- Khi biết (hoặc giả định hợp lý) phân bố đầu vào.
- Khi thuật toán có worst case quá tệ nhưng average case tốt (ví dụ Quick Sort).
- Khi phân tích randomized algorithms.

### 2.4. Bảng so sánh

| Tiêu chí             | Best                | Average            | Worst               |
| -------------------- | ------------------- | ------------------ | ------------------- |
| **Công thức**        | $\min T(I)$         | $E[T(I)]$          | $\max T(I)$         |
| **Ý nghĩa**          | Lạc quan            | Điển hình          | Bảo đảm             |
| **Cần giả định?**    | Không               | **Có** (phân bố)   | Không               |
| **Độ khó phân tích** | Dễ                  | **Khó**            | Trung bình          |
| **Tần suất dùng**    | Ít                  | Trung bình         | **Nhiều nhất**      |
| **Ứng dụng**         | Adaptive algorithms | Real-world systems | Real-time, security |

> **Lý thuyết bên cạnh:** Worst case được ưu tiên vì nó **không cần giả định** và cho **bảo đảm tuyệt đối**. Average case sát thực tế hơn nhưng cần giả định phân bố — nếu dữ liệu thực tế khác giả định, kết quả average case có thể sai lệch hoàn toàn. Best case ít dùng một mình vì không phản ánh hành vi điển hình.

---

## 3. Ví dụ chi tiết — Linear Search

### 3.1. Phân tích

Phép toán cơ bản: **so sánh** `A[i] == x`.

**Best case:** $x$ nằm ở vị trí đầu tiên ($A[1] = x$).

$$
T_{\text{best}}(n) = 1 = \Theta(1)
$$

**Worst case:** $x$ nằm ở cuối mảng hoặc không có trong mảng.

$$
T_{\text{worst}}(n) = n = \Theta(n)
$$

**Average case:** Giả định $x$ **có mặt** trong mảng và **mỗi vị trí đồng khả năng** ($P = 1/n$).

Số phép so sánh khi $x$ ở vị trí $i$ là $i$. Vậy:

$$
T_{\text{avg}}(n) = \sum_{i=1}^{n} \frac{1}{n} \cdot i = \frac{1}{n} \cdot \frac{n(n+1)}{2} = \frac{n+1}{2} = \Theta(n)
$$

**Nếu $x$ không có mặt** (xác suất $q$):

$$
T_{\text{avg}}(n) = (1-q) \cdot \frac{n+1}{2} + q \cdot n = \Theta(n)
$$

### 3.2. Nhận xét quan trọng

- **Best $\ne$ Average $\ne$ Worst** về hằng số, nhưng **Average và Worst cùng bậc** $O(n)$.
- **Best case $O(1)$ không có nghĩa là thuật toán nhanh** — chỉ là "có thể nhanh trong trường hợp đặc biệt".
- Khi nói "Linear Search là $O(n)$", ta đang nói **worst case**.

> **Lý thuyết bên cạnh:** Với Linear Search, average case và worst case **cùng bậc** — nhưng điều này **không phải lúc nào cũng đúng**. Quick Sort có worst $\Theta(n^2)$ nhưng average $\Theta(n\log n)$ — khác bậc hoàn toàn.

---

## 4. Ví dụ chi tiết — Insertion Sort

### 4.1. Mã giả

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

Phép toán cơ bản: **so sánh** `A[j] > key` và **dịch** `A[j+1] = A[j]`.

### 4.2. Best case — mảng đã sắp xếp tăng dần

Mỗi phần tử $A[i]$ đã đúng vị trí, `while` **không chạy** (điều kiện `A[j] > key` sai ngay).

Số phép so sánh: $n - 1$ lần (mỗi vòng ngoài một lần).

$$
T_{\text{best}}(n) = \Theta(n)
$$

**Ý nghĩa:** Insertion Sort **cực nhanh** trên mảng gần sắp xếp. Đây là lý do nó được dùng trong **Timsort** (Python, Java) cho các mảng nhỏ.

### 4.3. Worst case — mảng sắp xếp giảm dần

Mỗi phần tử $A[i]$ phải dịch qua **tất cả** $i - 1$ phần tử trước.

Số phép so sánh/dịch:

$$
T_{\text{worst}}(n) = \sum_{i=2}^{n} (i - 1) = \frac{n(n-1)}{2} = \Theta(n^2)
$$

### 4.4. Average case — hoán vị ngẫu nhiên đều

**Giả định:** Mảng là hoán vị ngẫu nhiên đều của $n$ phần tử phân biệt.

**Công cụ:** Số **nghịch thế** (inversions). Nghịch thế là cặp $(i, j)$ với $i < j$ nhưng $A[i] > A[j]$.

**Quan sát:** Số phép dịch trong Insertion Sort = số nghịch thế của mảng.

**Chứng minh:** Mỗi lần `while` chạy tương ứng với một nghịch thế được "sửa" (phần tử $A[i]$ dịch qua phần tử lớn hơn nó). Vì Insertion Sort sửa **tất cả** nghịch thế, tổng số phép dịch = tổng số nghịch thế ban đầu.

**Tính kỳ vọng:** Với mỗi cặp $(i, j)$ với $i < j$, đặt biến chỉ báo:

$$
X_{ij} = \begin{cases} 1 & \text{nếu } A[i] > A[j] \\ 0 & \text{ngược lại} \end{cases}
$$

Vì hoán vị đều, $P(A[i] > A[j]) = 1/2$, nên $E[X_{ij}] = 1/2$.

Tổng số nghịch thế $I = \sum_{i<j} X_{ij}$. Theo **linearity of expectation**:

$$
E[I] = \sum_{i<j} E[X_{ij}] = \binom{n}{2} \cdot \frac{1}{2} = \frac{n(n-1)}{4} = \Theta(n^2)
$$

Vậy:

$$
T_{\text{avg}}(n) = \Theta(n^2)
$$

### 4.5. Bảng tổng hợp Insertion Sort

| Case    | Công thức       | Order         |
| ------- | --------------- | ------------- |
| Best    | $n - 1$         | $\Theta(n)$   |
| Average | $\approx n^2/4$ | $\Theta(n^2)$ |
| Worst   | $n(n-1)/2$      | $\Theta(n^2)$ |

**Nhận xét:** Best $\Theta(n)$ **khác bậc** với Average/Worst $\Theta(n^2)$. Đây là thuật toán **adaptive** — hiệu năng phụ thuộc mạnh vào "độ sắp xếp" của dữ liệu.

> **Lý thuyết bên cạnh:** Số nghịch thế là **thước đo tự nhiên** cho "độ xa sắp xếp" của mảng. Insertion Sort có thời gian $\Theta(n + I)$ với $I$ là số nghịch thế. Đây là **phân tích chính xác hơn** so với chỉ nói "$\Theta(n^2)$ worst case".

---

## 5. Ví dụ chi tiết — Quick Sort

### 5.1. Mã giả

```text
QuickSort(A, low, high):
    if low < high:
        p = Partition(A, low, high)   // chọn pivot, chia mảng
        QuickSort(A, low, p-1)
        QuickSort(A, p+1, high)

Partition(A, low, high):
    pivot = A[high]
    i = low - 1
    for j = low to high-1:
        if A[j] <= pivot:
            i = i + 1
            swap(A[i], A[j])
    swap(A[i+1], A[high])
    return i + 1
```

### 5.2. Best case — pivot luôn chia đôi mảng

Mỗi lần Partition chia mảng thành **hai nửa đều nhau**:

$$
T_{\text{best}}(n) = 2T(n/2) + \Theta(n) = \Theta(n\log n)
$$

**Ý nghĩa:** Đây cũng là **cận dưới** cho mọi thuật toán sắp xếp dựa trên so sánh.

### 5.3. Worst case — pivot luôn là min hoặc max

Mảng đã sắp xếp (tăng hoặc giảm) và chọn pivot là phần tử cuối/đầu. Mỗi lần chia mảng thành **một phần rỗng và một phần $n-1$ phần tử**:

$$
T_{\text{worst}}(n) = T(n-1) + \Theta(n) = \Theta(n^2)
$$

**Ý nghĩa:** Quick Sort **cực tệ** trên mảng đã sắp xếp (với pivot chọn kém). Đây là lý do cần chọn pivot ngẫu nhiên hoặc median-of-three.

### 5.4. Average case — pivot rơi ngẫu nhiên

**Giả định:** Mỗi lần Partition, pivot rơi vào vị trí ngẫu nhiên đều trong mảng.

**Công cụ:** Phân tích bằng **biến chỉ báo** và **linearity of expectation**.

**Kết quả quan trọng:** Có thể chứng minh:

$$
T_{\text{avg}}(n) = \Theta(n\log n)
$$

**Ý tưởng chứng minh (rút gọn):** Với mỗi cặp $(i, j)$ với $i < j$ trong mảng đã sắp xếp, đặt:

$$
X_{ij} = \begin{cases} 1 & \text{nếu } A[i] \text{ và } A[j] \text{ được so sánh} \\ 0 & \text{ngược lại} \end{cases}
$$

Có thể chứng minh:

$$
P(A[i] \text{ và } A[j] \text{ được so sánh}) = \frac{2}{j - i + 1}
$$

Tổng số phép so sánh:

$$
E[\text{so sánh}] = \sum_{i<j} \frac{2}{j - i + 1} = 2n \sum_{k=1}^{n} \frac{1}{k} - O(n) = 2n H_n - O(n) = \Theta(n\log n)
$$

với $H_n$ là số harmonic: $H_n = \Theta(\log n)$.

### 5.5. Bảng tổng hợp Quick Sort

| Case    | Order             | Khi nào xảy ra      |
| ------- | ----------------- | ------------------- |
| Best    | $\Theta(n\log n)$ | Pivot chia đôi mảng |
| Average | $\Theta(n\log n)$ | Pivot ngẫu nhiên    |
| Worst   | $\Theta(n^2)$     | Pivot luôn min/max  |

**Nhận xét:** Best và Average **cùng bậc** $\Theta(n\log n)$, nhưng Worst **khác bậc** $\Theta(n^2)$. Đây là lý do:
- Trong thực tế, Quick Sort rất nhanh (average case tốt).
- Trong lý thuyết, cần **randomized quicksort** để tránh worst case bị kẻ tấn công khai thác (adversarial input).

> **Lý thuyết bên cạnh:** Với **randomized quicksort** (chọn pivot ngẫu nhiên), worst case **vẫn** là $\Theta(n^2)$ về mặt lý thuyết, nhưng **xác suất xảy ra cực nhỏ** — có thể chứng minh với xác suất cao, thời gian là $\Theta(n\log n)$. Đây là khái niệm **expected running time** — khác với average case (average case giả định phân bố input, expected running time lấy kỳ vọng trên **tính ngẫu nhiên của thuật toán**).

---

## 6. Ví dụ chi tiết — Binary Search

### 6.1. Phân tích

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

**Best case:** $x$ nằm ngay giữa mảng ở lần đầu.

$$
T_{\text{best}}(n) = 1 = \Theta(1)
$$

**Worst case:** $x$ không có trong mảng, hoặc ở vị trí sâu nhất trong cây tìm kiếm.

$$
T_{\text{worst}}(n) = \lfloor \log_2 n \rfloor + 1 = \Theta(\log n)
$$

**Average case:** $x$ có mặt, mỗi vị trí đồng khả năng.

Với cây tìm kiếm nhị phân hoàn chỉnh, độ sâu trung bình $\approx \log_2 n$. Có thể chứng minh:

$$
T_{\text{avg}}(n) = \Theta(\log n)
$$

### 6.2. Bảng tổng hợp

| Case    | Order            |
| ------- | ---------------- |
| Best    | $\Theta(1)$      |
| Average | $\Theta(\log n)$ |
| Worst   | $\Theta(\log n)$ |

**Nhận xét:** Binary Search có best $\Theta(1)$ nhưng average/worst **cùng bậc** $\Theta(\log n)$. Đây là thuật toán **không adaptive** — hiệu năng ổn định trên mọi input.

---

## 7. Ví dụ chi tiết — Hash Table (Bảng băm)

### 7.1. Phân tích

Hash table với **separate chaining** (mỗi ô là danh sách liên kết).

**Giả định:** Hàm băm phân bố đều (uniform hashing) — mỗi key đồng khả năng rơi vào bất kỳ ô nào trong $m$ ô.

**Hệ số tải:** $\alpha = n/m$ (số phần tử / số ô).

**Best case:** Không có collision (không có hai key cùng ô).

$$
T_{\text{best}} = O(1)
$$

**Worst case:** **Tất cả** key rơi vào cùng một ô (hash collision hoàn toàn).

$$
T_{\text{worst}} = O(n)
$$

**Average case:** Với uniform hashing, độ dài trung bình mỗi danh sách là $\alpha = n/m$. Tìm kiếm không thành công duyệt hết danh sách: $\Theta(1 + \alpha)$.

$$
T_{\text{avg}} = \Theta(1 + \alpha)
$$

Nếu $m = \Theta(n)$ (hệ số tải hằng số), thì $T_{\text{avg}} = \Theta(1)$.

### 7.2. Bảng tổng hợp

| Case    | Order           | Khi nào          |
| ------- | --------------- | ---------------- |
| Best    | $O(1)$          | Không collision  |
| Average | $O(1 + \alpha)$ | Uniform hashing  |
| Worst   | $O(n)$          | Tất cả collision |

**Ứng dụng thực tế:** Worst case $O(n)$ của hash table là **lỗ hổng bảo mật** nổi tiếng (hash collision attack — HashDoS). Kẻ tấn công có thể tạo input gây collision để làm chậm server. Đây là lý do nhiều ngôn ngữ dùng **randomized hashing** (thêm salt ngẫu nhiên vào hàm băm).

> **Lý thuyết bên cạnh:** Hash table là ví dụ điển hình cho việc **worst case và average case khác bậc hoàn toàn** — $O(n)$ vs $O(1)$. Trong thực tế, average case là điều quan trọng, nhưng worst case cần được xử lý (ví dụ bằng randomized hashing hoặc balanced BST thay thế).

---

## 8. Ví dụ chi tiết — Tìm max/min trong mảng

### 8.1. Phân tích

```text
FindMax(A, n):
    max = A[1]
    for i = 2 to n:
        if A[i] > max:
            max = A[i]
    return max
```

**Quan sát quan trọng:** Số lần **so sánh** `A[i] > max` là **luôn** $n - 1$, bất kể dữ liệu.

**Best = Average = Worst:**

$$
T(n) = n - 1 = \Theta(n) \quad \text{cho mọi trường hợp}
$$

### 8.2. Nhận xét

**Không phải thuật toán nào cũng có ba trường hợp khác nhau.** Khi số phép toán **không phụ thuộc dữ liệu** (chỉ phụ thuộc $n$), cả ba trường hợp giống nhau.

**Ví dụ khác:** Merge Sort — số phép so sánh có thể khác nhau, nhưng **số phép trộn** luôn là $\Theta(n\log n)$ bất kể dữ liệu. Vậy Merge Sort có Best = Average = Worst = $\Theta(n\log n)$ về mặt **số phép toán cơ bản**.

> **Lý thuyết bên cạnh:** Khi phân tích, cần xác định **phép toán cơ bản** có phụ thuộc dữ liệu không. Nếu có (như so sánh trong Insertion Sort), ba trường hợp khác nhau. Nếu không (như số phép trộn trong Merge Sort), ba trường hợp giống nhau.

---

## 9. Mối quan hệ với ký hiệu tiệm cận

### 9.1. Cách viết chuẩn

Khi nói về các trường hợp, ta thường dùng ký hiệu tiệm cận **kèm theo case**:

- **Worst case:** $T_{\text{worst}}(n) = O(g(n))$ — cận trên của trường hợp xấu nhất.
- **Best case:** $T_{\text{best}}(n) = \Omega(g(n))$ — cận dưới của trường hợp tốt nhất.
- **Average case:** $T_{\text{avg}}(n) = \Theta(g(n))$ — bậc trung bình.

### 9.2. Nhầm lẫn phổ biến — $O$ vs worst case

**Sai:** "$O$ nghĩa là worst case, $\Omega$ nghĩa là best case."

**Đúng:** $O$ và $\Omega$ mô tả **quan hệ giữa hai hàm**, **không** mô tả case. Tuy nhiên, có một **quy ước thực tế**:

- Khi nói "thuật toán là $O(n^2)$" mà không nói case, thường ngầm hiểu là **worst case**.
- Khi nói "best case là $\Omega(n)$", ta đang nói **cận dưới** của best case.

### 9.3. Bảng phân biệt

| Cách nói                         | Ý nghĩa chính xác                               |
| -------------------------------- | ----------------------------------------------- |
| "$T(n) = O(n^2)$"                | $T$ bị chặn trên bởi $n^2$ (không nói case nào) |
| "Worst case $O(n^2)$"            | Trường hợp xấu nhất bị chặn trên bởi $n^2$      |
| "Best case $\Omega(n)$"          | Trường hợp tốt nhất bị chặn dưới bởi $n$        |
| "Average case $\Theta(n\log n)$" | Trung bình đúng bậc $n\log n$                   |

> **Lý thuyết bên cạnh:** Trong tài liệu, "worst case $O(f)$" là cách viết **dư thừa** nhưng phổ biến — vì $O$ đã là cận trên, thêm "worst case" để nhấn mạnh. Chính xác hơn có thể viết "worst case là $\Theta(f)$" nếu biết đúng bậc.

---

## 10. Average case — phân tích chuyên sâu

### 10.1. Tại sao average case khó?

Average case cần **ba yếu tố**:

1. **Mô hình phân bố xác suất** của input — thường là giả định, không phải sự thật.
2. **Không gian mẫu** — tất cả input có thể.
3. **Kỹ thuật tính kỳ vọng** — thường dùng linearity of expectation.

**Vấn đề:** Nếu giả định phân bố sai, kết quả average case **vô nghĩa**.

### 10.2. Ví dụ — hai giả định khác nhau cho cùng thuật toán

Xét Linear Search với $x$ **có mặt** trong mảng:

**Giả định A — phân bố đều:** Mỗi vị trí đồng khả năng.

$$
T_{\text{avg}} = \frac{n+1}{2} = \Theta(n)
$$

**Giả định B — phân bố lệch (x thường ở đầu):** Ví dụ $P(\text{vị trí } i) \propto 1/i^2$.

$$
T_{\text{avg}} = \sum_{i=1}^{n} i \cdot \frac{c}{i^2} = c \sum_{i=1}^{n} \frac{1}{i} = \Theta(\log n)
$$

**Cùng thuật toán, cùng "average case", nhưng kết quả khác bậc** vì giả định khác.

> **Lý thuyết bên cạnh:** Average case **không có ý nghĩa tuyệt đối** — nó luôn gắn với một **giả định phân bố**. Khi đọc kết quả average case, phải đọc kèm giả định. Đây là lý do worst case được ưu tiên: nó **không cần giả định**.

### 10.3. Kỹ thuật phân tích average case

**Bước 1:** Xác định phép toán cơ bản.

**Bước 2:** Xác định biến ngẫu nhiên đại diện cho chi phí.

**Bước 3:** Dùng **biến chỉ báo** (indicator) để biểu diễn chi phí thành tổng.

**Bước 4:** Áp dụng **linearity of expectation**.

**Bước 5:** Tính xác suất cho mỗi biến chỉ báo.

**Ví dụ mẫu — Insertion Sort (đã trình bày ở §4.4):**

- Biến chỉ báo $X_{ij}$ = 1 nếu cặp $(i, j)$ là nghịch thế.
- $E[X_{ij}] = 1/2$ (do hoán vị đều).
- $E[I] = \sum_{i<j} E[X_{ij}] = \binom{n}{2}/2 = \Theta(n^2)$.

---

## 11. Lower bound và adversary argument

### 11.1. Lower bound (cận dưới)

**Câu hỏi:** Có thuật toán nào **nhanh hơn** một ngưỡng nào đó không?

**Ví dụ:** Mọi thuật toán sắp xếp dựa trên so sánh có worst case $\Omega(n\log n)$.

**Chứng minh (ý tưởng):** Có $n!$ hoán vị cần phân biệt. Cây quyết định có ít nhất $n!$ lá, chiều cao $\ge \log_2(n!) = \Omega(n\log n)$.

### 11.2. Adversary argument (lập luận đối kháng)

**Ý tưởng:** Để chứng minh **worst case** $\Omega(f(n))$, ta xây dựng một **"kẻ thù"** luôn trả lời theo cách **làm chậm thuật toán nhất**.

**Ví dụ:** Với bài toán tìm max trong mảng, adversary luôn trả lời "phần tử mới lớn hơn" — buộc thuật toán phải so sánh **mọi** phần tử. Vậy worst case $\ge n - 1$ phép so sánh.

> **Lý thuyết bên cạnh:** Adversary argument là kỹ thuật mạnh để chứng minh **lower bound của worst case**. Nó khác với phân tích thông thường (đi từ thuật toán cụ thể đến chi phí) — ở đây ta đi từ **bài toán** đến **giới hạn dưới**.

---

## 12. Bảng tổng hợp các thuật toán

| Thuật toán              | Best              | Average           | Worst             |
| ----------------------- | ----------------- | ----------------- | ----------------- |
| **Linear Search**       | $\Theta(1)$       | $\Theta(n)$       | $\Theta(n)$       |
| **Binary Search**       | $\Theta(1)$       | $\Theta(\log n)$  | $\Theta(\log n)$  |
| **Insertion Sort**      | $\Theta(n)$       | $\Theta(n^2)$     | $\Theta(n^2)$     |
| **Bubble Sort**         | $\Theta(n)$*      | $\Theta(n^2)$     | $\Theta(n^2)$     |
| **Selection Sort**      | $\Theta(n^2)$     | $\Theta(n^2)$     | $\Theta(n^2)$     |
| **Merge Sort**          | $\Theta(n\log n)$ | $\Theta(n\log n)$ | $\Theta(n\log n)$ |
| **Heap Sort**           | $\Theta(n\log n)$ | $\Theta(n\log n)$ | $\Theta(n\log n)$ |
| **Quick Sort**          | $\Theta(n\log n)$ | $\Theta(n\log n)$ | $\Theta(n^2)$     |
| **Hash Table (search)** | $O(1)$            | $O(1+\alpha)$     | $O(n)$            |
| **BST (search)**        | $O(\log n)$       | $O(\log n)$       | $O(n)$            |
| **Find Max**            | $\Theta(n)$       | $\Theta(n)$       | $\Theta(n)$       |

*Bubble Sort có best case $\Theta(n)$ nếu có optimization "dừng sớm khi không có swap".

### Nhận xét từ bảng

1. **Selection Sort, Merge Sort, Heap Sort, Find Max:** cả ba trường hợp **giống nhau** — không phụ thuộc dữ liệu.
2. **Insertion Sort, Bubble Sort:** best **khác bậc** — adaptive, tốt cho mảng gần sắp xếp.
3. **Quick Sort:** worst **khác bậc** — cần randomized để tránh.
4. **Hash Table, BST:** worst **khác bậc hoàn toàn** — cần xử lý đặc biệt.

---

## 13. Ứng dụng thực tế — khi nào dùng case nào?

### 13.1. Worst case — khi cần bảo đảm

**Ứng dụng:**
- **Hệ thống real-time:** Cần đảm bảo thời gian phản hồi. Ví dụ: hệ thống điều khiển máy bay, thiết bị y tế.
- **Hệ thống an toàn (security):** Kẻ tấn công có thể tạo adversarial input. Ví dụ: web server dùng hash table cần chống HashDoS.
- **Hệ thống có SLA (Service Level Agreement):** Cam kết thời gian phản hồi tối đa.

**Ví dụ:** Ngân hàng dùng thuật toán sắp xếp giao dịch — cần đảm bảo worst case để không bị treo hệ thống.

### 13.2. Average case — khi biết phân bố

**Ứng dụng:**
- **Hệ thống xử lý dữ liệu thông thường:** Input có phân bố đã biết (ví dụ: dữ liệu người dùng, log).
- **Thuật toán randomized:** Average case trên tính ngẫu nhiên của thuật toán.
- **So sánh thuật toán trong thực tế:** Average case phản ánh hiệu năng điển hình.

**Ví dụ:** Quick Sort trong thư viện chuẩn (C++ `std::sort`, Python `sorted`) dùng pivot ngẫu nhiên — average case $\Theta(n\log n)$ với xác suất cao.

### 13.3. Best case — khi cần adaptive

**Ứng dụng:**
- **Thuật toán adaptive:** Tận dụng cấu trúc dữ liệu. Ví dụ: Insertion Sort cho mảng gần sắp xếp.
- **Timsort (Python, Java):** Kết hợp Merge Sort và Insertion Sort — tận dụng best case của Insertion Sort cho các "run" nhỏ đã sắp xếp.
- **Chứng minh thuật toán "có thể nhanh":** Không phải lúc nào cũng chậm.

**Ví dụ:** Timsort đạt $O(n)$ trên mảng đã sắp xếp — đây là best case được khai thác có chủ đích.

> **Lý thuyết bên cạnh:** Trong thực tế, **worst case** thường được dùng để **chọn thuật toán**, **average case** để **đánh giá hiệu năng điển hình**, và **best case** để **thiết kế thuật toán adaptive**. Cả ba đều có vai trò riêng.

---

## 14. Những hiểu nhầm phổ biến

### Hiểu nhầm 1 — "Best case là trường hợp tốt nhất của thuật toán"

**Đúng một nửa.** Best case là **thời gian chạy nhỏ nhất trên mọi input kích thước $n$** — không phải "trường hợp thuận lợi cho thuật toán" theo nghĩa cảm tính. Nó là một **định nghĩa toán học**.

### Hiểu nhầm 2 — "Worst case là trường hợp hiếm gặp"

**Sai.** Worst case có thể **rất phổ biến** trong thực tế. Ví dụ: Quick Sort với pivot là phần tử cuối — worst case xảy ra khi mảng **đã sắp xếp**, mà mảng đã sắp xếp lại **rất phổ biến** trong thực tế.

### Hiểu nhầm 3 — "Average case luôn giữa best và worst"

**Đúng về giá trị**, nhưng **không nhất thiết về bậc**. Ví dụ: một thuật toán có best $O(1)$, worst $O(n^2)$, average $O(n\log n)$ — average **không phải trung bình cộng** của best và worst.

### Hiểu nhầm 4 — "Average case là trung bình của best và worst"

**Sai.** Average case là **kỳ vọng trên phân bố input**, không phải trung bình cộng của hai cực trị. Ví dụ: Linear Search có best 1, worst $n$, average $(n+1)/2$ — nhưng đây là **do phân bố đều**, không phải quy tắc chung.

### Hiểu nhầm 5 — "Thuật toán có worst case tốt hơn luôn tốt hơn"

**Sai trong thực tế.** Quick Sort có worst $O(n^2)$ nhưng average $O(n\log n)$ với hằng số nhỏ — thực tế **nhanh hơn** Merge Sort ($O(n\log n)$ worst) trong nhiều trường hợp. Chọn thuật toán phụ thuộc vào **ứng dụng cụ thể**.

### Hiểu nhầm 6 — "Average case luôn hữu ích hơn worst case"

**Sai.** Average case cần **giả định phân bố** — nếu sai, kết quả vô nghĩa. Worst case **không cần giả định** — luôn đúng. Trong nhiều ứng dụng (real-time, security), worst case **quan trọng hơn**.

---

## 15. Tóm tắt — checklist phân tích ba trường hợp

Khi phân tích một thuật toán, hãy tự hỏi:

1. **Phép toán cơ bản là gì?** (so sánh, gán, truy cập…)
2. **Số phép toán có phụ thuộc dữ liệu không?**
   - **Không:** Best = Average = Worst → chỉ cần phân tích một lần.
   - **Có:** Cần phân tích cả ba.
3. **Best case xảy ra khi nào?** (input cụ thể)
4. **Worst case xảy ra khi nào?** (input cụ thể)
5. **Average case:** Giả định phân bố gì? (đều, ngẫu nhiên, khác?)
6. **Dùng kỹ thuật gì để tính average?** (indicator + linearity, tổng, xác suất…)
7. **Kết quả có khác bậc không?** (nếu có, cần nhấn mạnh)
8. **Ứng dụng thực tế:** Case nào quan trọng nhất?

---

## 16. Bài tập tự luyện (kèm hướng giải)

**Bài 1:** Phân tích best/average/worst của thuật toán **tìm phần tử nhỏ thứ hai** trong mảng.

*Hướng:* Best: so sánh $n-1$ lần (nếu phần tử đầu là min, phần tử thứ hai là min thứ hai). Worst: $n + \lceil \log n \rceil - 2$ (dùng tournament). Average: tương tự worst.

**Bài 2:** Bubble Sort có optimization "dừng sớm khi không có swap". Phân tích best/average/worst.

*Hướng:* Best: $n-1$ so sánh (mảng đã sắp xếp). Worst: $n(n-1)/2$. Average: $\Theta(n^2)$ — vì xác suất mảng ngẫu nhiên có nhiều swap là cao.

**Bài 3:** Chứng minh rằng với **randomized quicksort**, expected running time là $O(n\log n)$.

*Hướng:* Dùng biến chỉ báo $X_{ij}$, tính $P(\text{so sánh } A[i], A[j]) = 2/(j-i+1)$, cộng lại.

**Bài 4:** Hash table với **linear probing** — phân tích best/average/worst.

*Hướng:* Best: $O(1)$ (ô trống ngay). Worst: $O(n)$ (cluster dài). Average: $O(1 + 1/(1-\alpha))$ với $\alpha < 1$ là hệ số tải.

**Bài 5:** Tại sao nói "worst case của Quick Sort là $\Theta(n^2)$" nhưng "average case là $\Theta(n\log n)$"? Giải thích bằng ví dụ cụ thể.

*Hướng:* Worst: mảng đã sắp xếp + pivot cuối. Average: pivot ngẫu nhiên chia mảng không đều nhưng trung bình vẫn tốt.

---

Nếu bạn muốn, mình có thể đi sâu tiếp vào:
- **Chứng minh đầy đủ** average case của Quick Sort (dùng indicator + harmonic number).
- **Phân tích amortized** — khái niệm liên quan nhưng khác (worst case của chuỗi thao tác).
- **Adversary argument** chi tiết cho các bài toán lower bound.
- **So sánh** best/average/worst trong bối cảnh **randomized algorithms** và **expected running time**.