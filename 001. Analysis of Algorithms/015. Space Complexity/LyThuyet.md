# Độ phức tạp không gian (Space Complexity) — Lý thuyết chi tiết + Ví dụ

## 1. Space Complexity là gì?

**Độ phức tạp không gian** (space complexity) của một thuật toán là **tổng bộ nhớ** mà thuật toán cần để chạy xong, biểu diễn như một hàm của **kích thước đầu vào** $n$.

$$
S(n) = \text{tổng bộ nhớ tiêu thụ khi chạy với đầu vào kích thước } n
$$

**Lưu ý quan trọng:** Đây là **bộ nhớ tối đa** tại **bất kỳ thời điểm nào** trong quá trình chạy — không phải tổng bộ nhớ cấp phát cộng dồn. Bộ nhớ được giải phóng (như biến cục bộ ra khỏi scope) sẽ **không tính** vào thời điểm sau.

### Phân biệt với Time Complexity

| Tiêu chí | Time Complexity | Space Complexity |
|---|---|---|
| Đo cái gì | Số phép toán | Bộ nhớ tiêu thụ |
| Tài nguyên | CPU | RAM |
| Thường được ưu tiên? | Có (thường) | Có (khi bộ nhớ hạn chế) |
| Phân tích | Đếm phép toán | Đếm ô nhớ / biến |

> **Lý thuyết bên cạnh:** Trong thực tế, **time complexity** thường được ưu tiên hơn — vì CPU là tài nguyên khan hiếm hơn RAM trong hầu hết ứng dụng. Nhưng với hệ thống nhúng, thiết bị di động, hoặc xử lý dữ liệu cực lớn (big data), **space complexity** có thể là yếu tố quyết định. Một thuật toán $\Theta(n\log n)$ time nhưng $\Theta(n)$ space có thể **không chạy được** trên thiết bị chỉ có 1MB RAM — dù nhanh đến đâu.

---

## 2. Hai thành phần của bộ nhớ

### 2.1. Fixed part (phần cố định)

Bộ nhớ **không phụ thuộc** vào kích thước đầu vào:

- Biến cục bộ đơn giản (số nguyên, boolean, con trỏ).
- Hằng số, biến toàn cục.
- Code của chương trình.
- Không gian cho lời gọi hàm (một phần).

$$
\text{Fixed part} = O(1)
$$

### 2.2. Variable part (phần biến đổi)

Bộ nhớ **phụ thuộc** vào $n$:

- Mảng, danh sách có kích thước $n$.
- Cấu trúc dữ liệu phụ (hash table, stack, queue).
- **Ngăn xếp đệ quy** (recursion stack) — mỗi lời gọi đệ quy chiếm một frame.
- Bộ nhớ cấp phát động (heap).

$$
\text{Variable part} = f(n)
$$

### 2.3. Tổng

$$
\boxed{S(n) = O(1) + f(n) = O(f(n))}
$$

**Vì $O(1)$ bị bỏ qua** khi $f(n)$ tăng — đây là quy tắc tiệm cận.

> **Lý thuyết bên cạnh:** Trong phân tích tiệm cận, **hằng số không quan trọng**. Nhưng trong thực tế, fixed part có thể **lớn** (ví dụ chương trình C nạp cả thư viện chuẩn — vài MB). Khi lập trình cho hệ thống nhúng, cần **giảm cả fixed part** — không chỉ variable part.

---

## 3. Total space vs Auxiliary space

Đây là **phân biệt quan trọng** — nhiều tài liệu nhầm lẫn.

### 3.1. Total space (tổng bộ nhớ)

**Bao gồm cả input:**

$$
S_{\text{total}}(n) = \text{bộ nhớ cho input} + \text{bộ nhớ phụ}
$$

### 3.2. Auxiliary space (bộ nhớ phụ)

**Không tính input** — chỉ tính bộ nhớ **thêm** mà thuật toán cấp phát:

$$
S_{\text{aux}}(n) = \text{bộ nhớ thêm ngoài input}
$$

### 3.3. Khi nào dùng cái nào?

| Ngữ cảnh | Dùng |
|---|---|
| So sánh thuật toán cùng bài toán | Auxiliary space (vì input giống nhau) |
| Đánh giá yêu cầu bộ nhớ tổng | Total space |
| Kiểm tra "in-place" | Auxiliary space |
| Phân tích thuật toán nói chung | Thường auxiliary space |

**Ví dụ:**

- **Merge Sort:** auxiliary space $O(n)$ — cần mảng phụ để trộn.
- **Quick Sort:** auxiliary space $O(\log n)$ — chỉ cần stack đệ quy.
- **Bubble Sort:** auxiliary space $O(1)$ — sắp xếp tại chỗ.

### 3.4. Ví dụ minh họa

**Bài toán:** Đảo ngược mảng $A[1..n]$.

**Cách 1 — Dùng mảng phụ:**

```text
Reverse(A, n):
    B = new Array(n)               // cấp phát n ô
    for i = 1 to n:
        B[i] = A[n - i + 1]
    return B
```

- Total space: $O(n)$ (input) + $O(n)$ (B) = $O(n)$.
- **Auxiliary space: $O(n)$.**

**Cách 2 — Tại chỗ (in-place):**

```text
Reverse(A, n):
    for i = 1 to n/2:
        swap(A[i], A[n - i + 1])
```

- Total space: $O(n)$ (input).
- **Auxiliary space: $O(1)$.**

**Kết luận:** Cả hai có total space $O(n)$ — nhưng cách 2 tốt hơn vì auxiliary space $O(1)$. Đây là lý do **in-place algorithm** được ưa chuộng khi bộ nhớ hạn chế.

> **Lý thuyết bên cạnh:** Khi đọc "thuật toán có space complexity $O(1)$", hầu hết tài liệu ngụ ý **auxiliary space** $O(1)$ — không tính input. Đây là quy ước phổ biến. Khi cần chính xác, nên ghi rõ "auxiliary space" hay "total space".

---

## 4. Cách phân tích Space Complexity

### 4.1. Quy trình 4 bước

1. **Xác định biến và cấu trúc dữ liệu** được cấp phát.
2. **Phân loại** — biến cố định ($O(1)$) hay phụ thuộc $n$.
3. **Tính tổng** bộ nhớ lớn nhất tại một thời điểm.
4. **Đơn giản** bằng $\Theta$ hoặc $O$.

### 4.2. Quy tắc cộng (cho biến đồng thời tồn tại)

Nếu nhiều biến cùng tồn tại:

$$
S(n) = S_1(n) + S_2(n) + \cdots = O(\max S_i(n))
$$

### 4.3. Quy tắc nhân (cho mảng đa chiều)

Mảng $n \times m$ chiếm $O(nm)$ — nhân các chiều.

### 4.4. Quy tắc "lấy max theo thời gian"

Nếu biến được giải phóng trước khi biến khác cấp phát:

$$
S(n) = \max_{\text{thời điểm } t} S(t)
$$

**Không cộng dồn** bộ nhớ đã giải phóng.

### 4.5. Ví dụ 1 — biến đơn giản

```text
Sum(A, n):
    total = 0                       // O(1)
    for i = 1 to n:                 // i: O(1)
        total += A[i]
    return total
```

**Phân tích:**

- `total`: $O(1)$.
- `i`: $O(1)$.
- Không có mảng phụ.

**Auxiliary space:** $O(1)$.

### 4.6. Ví dụ 2 — mảng phụ

```text
CopyArray(A, n):
    B = new Array(n)                // O(n)
    for i = 1 to n:
        B[i] = A[i]
    return B
```

**Auxiliary space:** $O(n)$.

### 4.7. Ví dụ 3 — ma trận

```text
MultiplyMatrix(A, B, n):
    C = new Matrix(n, n)            // O(n²)
    for i = 1 to n:
        for j = 1 to n:
            C[i][j] = 0
            for k = 1 to n:
                C[i][j] += A[i][k] * B[k][j]
    return C
```

**Phân tích:**

- `C`: $O(n^2)$.
- `i, j, k`: $O(1)$.
- Biến tạm: $O(1)$.

**Auxiliary space:** $O(n^2)$.

### 4.8. Ví dụ 4 — biến giải phóng

```text
Compute(A, n):
    B = new Array(n)                // O(n) — cấp phát
    for i = 1 to n:
        B[i] = A[i] * 2
    sum = 0
    for i = 1 to n:
        sum += B[i]
    delete B                        // giải phóng B
    C = new Array(n)                // O(n) — cấp phát mới
    for i = 1 to n:
        C[i] = sum - A[i]
    return C
```

**Phân tích:**

- Tại thời điểm có `B`: $O(n)$.
- Tại thời điểm có `C`: $O(n)$.
- **Không** đồng thời $2n$ — vì `B` đã giải phóng trước khi `C` cấp phát.

**Auxiliary space:** $O(n)$ — **không phải** $O(2n)$.

**Bài học:** Phân tích space là **max theo thời gian**, không phải tổng cộng dồn.

---

## 5. Đệ quy — phân tích space quan trọng nhất

### 5.1. Recursion stack

Mỗi lời gọi đệ quy chiếm một **frame** trên ngăn xếp (stack), chứa:

- Tham số.
- Biến cục bộ.
- Địa chỉ trả về.

**Độ sâu đệ quy** quyết định space complexity.

### 5.2. Công thức

$$
S_{\text{recursion}}(n) = \text{độ sâu đệ quy} \times \text{kích thước mỗi frame}
$$

Kích thước mỗi frame thường $O(1)$ (biến cục bộ đơn giản), nên:

$$
S_{\text{recursion}}(n) = O(\text{độ sâu đệ quy})
$$

### 5.3. Ví dụ 5 — Factorial (đệ quy tuyến tính)

```text
Factorial(n):
    if n <= 1: return 1
    return n * Factorial(n-1)
```

**Độ sâu đệ quy:** $n$ (gọi $n$ lần trước khi dừng).

**Auxiliary space:** $O(n)$ — do stack.

**So sánh với vòng lặp:**

```text
FactorialIter(n):
    result = 1
    for i = 2 to n:
        result *= i
    return result
```

**Auxiliary space:** $O(1)$.

**Kết luận:** Cùng kết quả, nhưng đệ quy tốn $O(n)$ space, vòng lặp $O(1)$.

### 5.4. Ví dụ 6 — Binary Search đệ quy

```text
BinarySearch(A, low, high, x):
    if low > high: return -1
    mid = (low + high) / 2
    if A[mid] == x: return mid
    else if A[mid] < x:
        return BinarySearch(A, mid+1, high, x)
    else:
        return BinarySearch(A, low, mid-1, x)
```

**Độ sâu đệ quy:** $\log_2 n$ (mỗi lần giảm một nửa).

**Auxiliary space:** $O(\log n)$.

**So sánh với vòng lặp:**

```text
BinarySearchIter(A, low, high, x):
    while low <= high:
        mid = (low + high) / 2
        if A[mid] == x: return mid
        else if A[mid] < x: low = mid + 1
        else: high = mid - 1
    return -1
```

**Auxiliary space:** $O(1)$.

### 5.5. Ví dụ 7 — Merge Sort

```text
MergeSort(A, low, high):
    if low >= high: return
    mid = (low + high) / 2
    MergeSort(A, low, mid)
    MergeSort(A, mid+1, high)
    Merge(A, low, mid, high)          // Θ(n) space
```

**Phân tích:**

- **Recursion stack:** $O(\log n)$ — độ sâu cây đệ quy.
- **Mảng phụ trong Merge:** $O(n)$ — cấp phát mỗi lần gọi (nhưng có thể tối ưu dùng mảng phụ global).
- **Tại một thời điểm:** stack $O(\log n)$ + mảng phụ $O(n)$ = $O(n)$.

**Auxiliary space:** $O(n)$.

**Tối ưu:** Dùng mảng phụ **global** $O(n)$ — auxiliary space vẫn $O(n)$, nhưng không cấp phát lại mỗi lần.

### 5.6. Ví dụ 8 — Quick Sort

```text
QuickSort(A, low, high):
    if low >= high: return
    p = Partition(A, low, high)      // O(1) space
    QuickSort(A, low, p-1)
    QuickSort(A, p+1, high)
```

**Phân tích:**

- **Recursion stack:** $O(\text{độ sâu cây})$.
- **Best/Average:** cây cân bằng → độ sâu $O(\log n)$.
- **Worst:** cây suy biến → độ sâu $O(n)$.

**Auxiliary space:**

- **Average:** $O(\log n)$.
- **Worst:** $O(n)$.

**Tối ưu:** Tail recursion cho nhánh lớn hơn — giảm xuống $O(\log n)$ **luôn** (kể cả worst case).

### 5.7. Ví dụ 9 — Fibonacci naive

```text
Fib(n):
    if n <= 1: return n
    return Fib(n-1) + Fib(n-2)
```

**Phân tích:**

- **Số lời gọi:** $\Theta(\varphi^n)$ — rất lớn.
- **Độ sâu đệ quy:** $n$ (nhánh dài nhất $n-1, n-2, \ldots, 1$).

**Auxiliary space:** $O(n)$ — **không phải** $O(\varphi^n)$! Vì stack chỉ giữ **một nhánh tại một thời điểm**, không giữ toàn bộ cây.

**Đây là điểm quan trọng:** Time complexity và space complexity có thể **rất khác nhau**. Fibonacci naive: time $\Theta(\varphi^n)$, space $O(n)$.

### 5.8. Ví dụ 10 — Duyệt cây nhị phân

```text
InOrder(node):
    if node == null: return
    InOrder(node.left)
    visit(node)
    InOrder(node.right)
```

**Độ sâu đệ quy:** chiều cao $h$ của cây.

- **Cây cân bằng:** $h = O(\log n)$ ⇒ space $O(\log n)$.
- **Cây suy biến:** $h = O(n)$ ⇒ space $O(n)$.

**Auxiliary space:** $O(h)$.

### 5.9. Bảng tổng hợp recursion space

| Thuật toán | Độ sâu đệ quy | Auxiliary space |
|---|---|---|
| Factorial | $n$ | $O(n)$ |
| Binary Search | $\log n$ | $O(\log n)$ |
| Merge Sort | $\log n$ | $O(n)$ (do mảng phụ) |
| Quick Sort (avg) | $\log n$ | $O(\log n)$ |
| Quick Sort (worst) | $n$ | $O(n)$ |
| Fibonacci naive | $n$ | $O(n)$ |
| Duyệt cây cân bằng | $\log n$ | $O(\log n)$ |
| Duyệt cây suy biến | $n$ | $O(n)$ |

> **Lý thuyết bên cạnh:** Phân tích recursion space thường **bị bỏ qua** — nhưng rất quan trọng. Ví dụ: Quick Sort "in-place" thực ra **không hoàn toàn** in-place — vẫn tốn $O(\log n)$ stack (average) hoặc $O(n)$ (worst). Đây là lý do một số hệ thống giới hạn độ sâu đệ quy (Python mặc định 1000) — để tránh tràn stack.

---

## 6. Phân tích Space cho cấu trúc dữ liệu

### 6.1. Bảng tra nhanh

| CTDL | Space | Ghi chú |
|---|---|---|
| **Array** | $O(n)$ | $n$ ô liên tiếp |
| **Linked List** | $O(n)$ | Mỗi nút có data + pointer |
| **Stack** | $O(n)$ | $n$ phần tử |
| **Queue** | $O(n)$ | $n$ phần tử |
| **Hash Table** | $O(n)$ | Mảng + danh sách collision |
| **BST** | $O(n)$ | Mỗi nút có 2 con trỏ |
| **Heap** | $O(n)$ | Mảng |
| **Trie** | $O(n \cdot L)$ | $L$ = độ dài chuỗi |
| **Graph (adjacency list)** | $O(V + E)$ | Mỗi đỉnh + cạnh |
| **Graph (adjacency matrix)** | $O(V^2)$ | Ma trận $V \times V$ |

### 6.2. Array vs Linked List — cùng $O(n)$ nhưng khác hằng số

- **Array:** $n$ ô, mỗi ô chứa data. Space = $n \cdot \text{sizeof(data)}$.
- **Linked List:** $n$ nút, mỗi nút chứa data + **pointer** (8 byte trên hệ 64-bit). Space = $n \cdot (\text{sizeof(data)} + 8)$.

**Ví dụ:** Lưu $10^6$ số nguyên (4 byte):

- Array: $4 \text{ MB}$.
- Linked List: $12 \text{ MB}$ (gấp 3 lần!).

**Cùng $O(n)$ nhưng Linked List tốn gấp 3 lần** — do con trỏ.

### 6.3. Graph — adjacency list vs matrix

- **Adjacency list:** $O(V + E)$ — tốt cho đồ thị **thưa** ($E \ll V^2$).
- **Adjacency matrix:** $O(V^2)$ — tốt cho đồ thị **dày** ($E \approx V^2$), hoặc khi cần kiểm tra cạnh $O(1)$.

**Ví dụ:** Đồ thị $V = 10^4$ đỉnh, $E = 10^5$ cạnh:

- Adjacency list: $\approx 10^4 + 10^5 = 1.1 \times 10^5$ ô.
- Adjacency matrix: $10^8$ ô.

**Adjacency list tiết kiệm hơn 900 lần** — đây là lý do nó được ưu tiên cho đồ thị thưa.

> **Lý thuyết bên cạnh:** Cùng **bậc tiệm cận** ($O(n)$ hoặc $O(V+E)$), nhưng **hằng số** có thể khác biệt lớn. Khi bộ nhớ là yếu tố then chốt (như hệ thống nhúng), cần xem xét **hằng số** — không chỉ bậc.

---

## 7. Space-Time Tradeoff (Đánh đổi không gian - thời gian)

### 7.1. Nguyên lý

Trong nhiều bài toán, có thể **giảm thời gian** bằng cách **tăng bộ nhớ** — và ngược lại.

$$
\text{Time} \uparrow \quad \Leftrightarrow \quad \text{Space} \downarrow
$$

### 7.2. Ví dụ 11 — Fibonacci: naive vs DP vs tối ưu

**Cách 1 — Naive (đệ quy):**

- Time: $\Theta(\varphi^n)$.
- Space: $O(n)$.

**Cách 2 — Memoization (top-down DP):**

```text
Fib(n, memo):
    if n <= 1: return n
    if memo[n] != -1: return memo[n]
    memo[n] = Fib(n-1, memo) + Fib(n-2, memo)
    return memo[n]
```

- Time: $O(n)$.
- Space: $O(n)$ (memo + stack).

**Cách 3 — Tabulation (bottom-up DP):**

```text
Fib(n):
    dp = new Array(n+1)
    dp[0] = 0; dp[1] = 1
    for i = 2 to n:
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

- Time: $O(n)$.
- Space: $O(n)$.

**Cách 4 — Tối ưu space:**

```text
Fib(n):
    if n <= 1: return n
    a = 0; b = 1
    for i = 2 to n:
        c = a + b
        a = b
        b = c
    return b
```

- Time: $O(n)$.
- Space: $O(1)$.

**So sánh:**

| Cách | Time | Space |
|---|---|---|
| Naive | $\Theta(\varphi^n)$ | $O(n)$ |
| Memo | $O(n)$ | $O(n)$ |
| Tabulation | $O(n)$ | $O(n)$ |
| Tối ưu | $O(n)$ | $O(1)$ |

**Nhận xét:** Cách 4 **tốt nhất cả hai chiều** — không đánh đổi! Đây là lý do luôn tìm cách **tối ưu cả time và space** nếu có thể.

### 7.3. Ví dụ 12 — Đếm số lần xuất hiện

**Bài toán:** Đếm số lần xuất hiện của mỗi phần tử trong mảng $A[1..n]$.

**Cách 1 — Sort rồi đếm:**

```text
Sort(A)                                 // Θ(n log n) time, O(log n) space
count = 1
for i = 2 to n:
    if A[i] == A[i-1]: count++
    else: print(A[i-1], count); count = 1
```

- Time: $\Theta(n\log n)$.
- Space: $O(\log n)$ (Quick Sort) hoặc $O(n)$ (Merge Sort).

**Cách 2 — Hash table:**

```text
count = new HashMap()
for i = 1 to n:
    count[A[i]] = count.get(A[i], 0) + 1
for key in count:
    print(key, count[key])
```

- Time: $O(n)$ average.
- Space: $O(n)$.

**So sánh:**

| Cách | Time | Space |
|---|---|---|
| Sort | $O(n\log n)$ | $O(\log n)$ hoặc $O(n)$ |
| Hash | $O(n)$ | $O(n)$ |

**Đây là đánh đổi điển hình:** Hash nhanh hơn nhưng tốn space hơn. Sort chậm hơn nhưng tiết kiệm space.

### 7.4. Ví dụ 13 — Ma trận nhân

**Naive:** $O(n^3)$ time, $O(n^2)$ space (ma trận kết quả).

**Strassen:** $O(n^{2.81})$ time, $O(n^2)$ space.

**Có thể giảm space?** Không — kết quả là ma trận $n \times n$, không thể ít hơn $O(n^2)$.

**Bài học:** Space complexity có **cận dưới tự nhiên** — do kích thước output. Không thể giảm dưới mức đó.

> **Lý thuyết bên cạnh:** Khi output có kích thước $f(n)$, mọi thuật toán **phải** tốn ít nhất $\Omega(f(n))$ space (để chứa output). Đây là **cận dưới tự nhiên** của space complexity. Ví dụ: sắp xếp tại chỗ (in-place) tốn $O(\log n)$ hoặc $O(1)$ space, nhưng nhân ma trận **không thể** in-place — vì output $n^2$ phần tử.

---

## 8. In-place algorithms

### 8.1. Định nghĩa

**In-place algorithm** là thuật toán có **auxiliary space** $O(1)$ hoặc $O(\log n)$ — không cấp phát thêm mảng kích thước $n$.

**Chú ý:** $O(\log n)$ vẫn được coi là "gần in-place" — vì $\log n$ rất nhỏ so với $n$ (với $n = 10^6$, $\log_2 n \approx 20$).

### 8.2. Ví dụ in-place

| Thuật toán | Auxiliary space |
|---|---|
| Bubble Sort | $O(1)$ |
| Selection Sort | $O(1)$ |
| Insertion Sort | $O(1)$ |
| Heap Sort | $O(1)$ |
| Quick Sort | $O(\log n)$ average |
| Reverse Array | $O(1)$ |
| Cycle Sort | $O(1)$ |

### 8.3. Ví dụ không in-place

| Thuật toán | Auxiliary space |
|---|---|
| Merge Sort | $O(n)$ |
| Counting Sort | $O(n + k)$ |
| Radix Sort | $O(n + k)$ |
| Bucket Sort | $O(n)$ |

### 8.4. Ví dụ 14 — Sắp xếp tại chỗ vs không tại chỗ

**Merge Sort (không in-place):**

```text
MergeSort(A):
    if len(A) <= 1: return A
    mid = len(A) / 2
    left = MergeSort(A[0..mid])
    right = MergeSort(A[mid..len])
    return Merge(left, right)             // tạo mảng mới
```

**Auxiliary space:** $O(n)$ — mảng `left`, `right`, và mảng kết quả.

**Heap Sort (in-place):**

```text
HeapSort(A):
    BuildMaxHeap(A)                       // O(1) space
    for i = n downto 2:
        swap(A[1], A[i])
        MaxHeapify(A, 1, i-1)             // O(1) space
```

**Auxiliary space:** $O(1)$.

**So sánh:**

| Thuật toán | Time (worst) | Auxiliary space |
|---|---|---|
| Merge Sort | $\Theta(n\log n)$ | $O(n)$ |
| Heap Sort | $\Theta(n\log n)$ | $O(1)$ |

**Heap Sort** là lựa chọn tốt khi cần **cả** time $\Theta(n\log n)$ **và** space $O(1)$.

> **Lý thuyết bên cạnh:** "In-place" là tính chất **quan trọng** trong hệ thống nhúng, game console (RAM hạn chế), và xử lý dữ liệu lớn. Nhưng in-place thường **phức tạp hơn** để code và **chậm hơn** (do swap nhiều). Đây là đánh đổi điển hình: **simplicity vs memory efficiency**.

---

## 9. Space phụ thuộc dữ liệu (Data-dependent space)

### 9.1. Khái niệm

Với một số thuật toán, space complexity phụ thuộc vào **giá trị** của input — không chỉ kích thước.

### 9.2. Ví dụ 15 — Đệ quy có điều kiện dừng phụ thuộc dữ liệu

```text
FindPath(graph, start, target):
    if start == target: return [start]
    for neighbor in graph[start]:
        path = FindPath(graph, neighbor, target)
        if path: return [start] + path
    return null
```

**Phân tích:**

- **Best case** (target ngay cạnh start): độ sâu 1 ⇒ space $O(1)$.
- **Worst case** (target sâu nhất hoặc không có): độ sâu $\Theta(V)$ ⇒ space $O(V)$.

**Auxiliary space:** phụ thuộc đồ thị — $O(V)$ worst.

### 9.3. Ví dụ 16 — Quick Sort

- **Average:** pivot chia đều ⇒ độ sâu $O(\log n)$ ⇒ space $O(\log n)$.
- **Worst:** pivot luôn ở biên ⇒ độ sâu $O(n)$ ⇒ space $O(n)$.

**Space phụ thuộc dữ liệu** — không chỉ $n$.

### 9.4. Ví dụ 17 — BFS vs DFS

**BFS (dùng queue):**

```text
BFS(graph, s):
    visited = new Set()                    // O(V)
    queue = new Queue()                    // O(V) worst — chứa cả tầng
    queue.enqueue(s)
    while queue:
        u = queue.dequeue()
        for v in graph[u]:
            if v not in visited:
                visited.add(v)
                queue.enqueue(v)
```

**Space:** $O(V)$ — queue có thể chứa toàn bộ một tầng.

**DFS (dùng stack/đệ quy):**

```text
DFS(graph, u, visited):
    visited.add(u)
    for v in graph[u]:
        if v not in visited:
            DFS(graph, v, visited)
```

**Space:** $O(V)$ worst (stack sâu) — nhưng **thường ít hơn BFS** trên đồ thị rộng.

**So sánh:**

| Thuật toán | Space (worst) | Space (thực tế) |
|---|---|---|
| BFS | $O(V)$ | Nhiều — chứa cả tầng rộng |
| DFS | $O(V)$ | Ít — chỉ chứa một nhánh |

**Cùng bậc $O(V)$ nhưng DFS thường tiết kiệm hơn** trên đồ thị rộng (như cây).

> **Lý thuyết bên cạnh:** Space complexity **worst case** có thể đánh lừa — như DFS và BFS đều $O(V)$ worst, nhưng DFS thực tế tốn ít hơn nhiều trên đồ thị rộng. Khi phân tích, cần xem xét **hằng số** và **hành vi thực tế** — không chỉ bậc.

---

## 10. Space Complexity của các thuật toán phổ biến

### 10.1. Sắp xếp

| Thuật toán | Time (worst) | Auxiliary Space |
|---|---|---|
| Bubble Sort | $O(n^2)$ | $O(1)$ |
| Insertion Sort | $O(n^2)$ | $O(1)$ |
| Selection Sort | $O(n^2)$ | $O(1)$ |
| Merge Sort | $O(n\log n)$ | $O(n)$ |
| Quick Sort | $O(n^2)$ | $O(\log n)$ avg, $O(n)$ worst |
| Heap Sort | $O(n\log n)$ | $O(1)$ |
| Counting Sort | $O(n+k)$ | $O(n+k)$ |
| Radix Sort | $O(d(n+k))$ | $O(n+k)$ |
| Bucket Sort | $O(n^2)$ worst | $O(n)$ |

### 10.2. Tìm kiếm

| Thuật toán | Time | Space |
|---|---|---|
| Linear Search | $O(n)$ | $O(1)$ |
| Binary Search (iter) | $O(\log n)$ | $O(1)$ |
| Binary Search (rec) | $O(\log n)$ | $O(\log n)$ |
| Hash Search | $O(1)$ avg | $O(n)$ (cho hash table) |
| BST Search | $O(\log n)$ avg | $O(n)$ |

### 10.3. Đồ thị

| Thuật toán | Time | Space |
|---|---|---|
| BFS | $O(V+E)$ | $O(V)$ |
| DFS | $O(V+E)$ | $O(V)$ |
| Dijkstra (heap) | $O((V+E)\log V)$ | $O(V)$ |
| Bellman-Ford | $O(VE)$ | $O(V)$ |
| Floyd-Warshall | $O(V^3)$ | $O(V^2)$ |
| Kruskal | $O(E\log E)$ | $O(V+E)$ |
| Prim (heap) | $O((V+E)\log V)$ | $O(V)$ |

### 10.4. Quy hoạch động

| Bài toán | Time | Space |
|---|---|---|
| Fibonacci (tối ưu) | $O(n)$ | $O(1)$ |
| LCS | $O(nm)$ | $O(nm)$ — có thể tối ưu $O(\min(n,m))$ |
| Knapsack 0/1 | $O(nW)$ | $O(nW)$ — có thể tối ưu $O(W)$ |
| Matrix Chain | $O(n^3)$ | $O(n^2)$ |
| Edit Distance | $O(nm)$ | $O(nm)$ — có thể tối ưu $O(\min(n,m))$ |
| TSP (Held-Karp) | $O(n^2 2^n)$ | $O(n 2^n)$ |

**Nhận xét:** Nhiều bài toán DP có thể **tối ưu space** bằng cách chỉ giữ **hàng trước** (rolling array) — giảm từ $O(nm)$ xuống $O(\min(n,m))$.

> **Lý thuyết bên cạnh:** Kỹ thuật **rolling array** (mảng cuộn) rất quan trọng trong DP — giảm space từ $O(nm)$ xuống $O(m)$ bằng cách nhận ra rằng bước $i$ chỉ cần bước $i-1$. Ví dụ: Knapsack 0/1 có thể dùng mảng 1 chiều $O(W)$ thay vì 2 chiều $O(nW)$ — chỉ cần duyệt ngược.

---

## 11. Những hiểu nhầm phổ biến

### Hiểu nhầm 1 — "Space complexity tính cả input"

**Sai.** Space complexity thường là **auxiliary space** — không tính input. Nếu tính cả input, mọi thuật toán đều có ít nhất $O(n)$ space (để chứa input) — không phân biệt được.

### Hiểu nhầm 2 — "Đệ quy luôn tốn $O(n)$ space"

**Sai.** Độ sâu đệ quy quyết định — có thể $O(\log n)$ (Binary Search), $O(n)$ (Factorial), hoặc $O(\varphi^n)$? — Không, tối đa $O(n)$ cho đệ quy tuyến tính, nhưng Fibonacci naive cũng chỉ $O(n)$.

**Thực tế:** Space của đệ quy = **độ sâu tối đa** — không phải số lời gọi.

### Hiểu nhầm 3 — "Cùng $O(n)$ là như nhau"

**Sai.** Cùng bậc, khác hằng số:

- Array $n$ số nguyên: $4n$ byte.
- Linked List $n$ số nguyên: $12n$ byte (do con trỏ).

**Chênh 3 lần** — quan trọng trong thực tế.

### Hiểu nhầm 4 — "Space complexity cộng dồn theo thời gian"

**Sai.** Space là **max tại một thời điểm** — không phải tổng cộng dồn. Biến giải phóng được **không tính** vào thời điểm sau.

### Hiểu nhầm 5 — "In-place nghĩa là $O(1)$ space"

**Gần đúng, nhưng không chính xác.** In-place thường có auxiliary space $O(1)$ hoặc $O(\log n)$. Quick Sort được gọi là "in-place" dù tốn $O(\log n)$ stack — vì $\log n$ rất nhỏ so với $n$.

### Hiểu nhầm 6 — "Time và Space luôn đánh đổi"

**Sai.** Có thuật toán tối ưu cả hai. Ví dụ: Fibonacci tối ưu — $O(n)$ time, $O(1)$ space. Đây là lý do luôn tìm cách **cải thiện cả hai** nếu có thể.

### Hiểu nhầm 7 — "Recursion space = số nút trong cây đệ quy"

**Sai.** Recursion space = **độ sâu tối đa** — không phải số nút. Fibonacci naive có $\varphi^n$ nút nhưng chỉ $O(n)$ độ sâu ⇒ $O(n)$ space.

### Hiểu nhầm 8 — "Space complexity của đệ quy đuôi (tail recursion) là $O(n)$"

**Sai.** Tail recursion có thể tối ưu thành vòng lặp — space $O(1)$. Nhưng **không phải ngôn ngữ nào cũng hỗ trợ** (Python, Java không tối ưu tail recursion mặc định; C, Scheme có).

### Hiểu nhầm 9 — "Bỏ qua space vì RAM rẻ"

**Sai trong nhiều ngữ cảnh.** Hệ thống nhúng, thiết bị di động, xử lý dữ liệu lớn (big data) — space là yếu tố **then chốt**. Ngay cả trên server, cache miss do dữ liệu lớn có thể làm chậm đáng kể.

### Hiểu nhầm 10 — "Space complexity chỉ áp dụng cho mảng"

**Sai.** Space áp dụng cho **mọi** cấu trúc dữ liệu — mảng, danh sách, cây, đồ thị, hash table, stack, queue. Mỗi loại có đặc điểm space riêng.

> **Lý thuyết bên cạnh:** Hầu hết hiểu nhầm đến từ việc **quên rằng space là max theo thời gian** và **không tính input**. Khi đọc "space complexity $O(f)$", hãy hiểu là "auxiliary space tối đa tại một thời điểm là $O(f)$". Điều này giúp tránh nhiều lỗi.

---

## 12. Ví dụ tổng hợp — phân tích đầy đủ

### Ví dụ 18 — Phân tích cả time và space

```text
Function Process(A, n):
    B = new Array(n)                       // O(n) space
    for i = 1 to n:                        // O(n) time
        B[i] = A[i] * 2
    sort(B)                                // O(n log n) time, O(log n) space
    C = new Matrix(n, n)                   // O(n²) space
    for i = 1 to n:                        // O(n²) time
        for j = 1 to n:
            C[i][j] = B[i] + B[j]
    return C
```

**Time complexity:**

- Vòng 1: $O(n)$.
- Sort: $O(n\log n)$.
- Vòng 2: $O(n^2)$.
- Tổng: $O(n^2)$.

**Space complexity:**

- `B`: $O(n)$ — tồn tại đến cuối.
- `C`: $O(n^2)$ — cấp phát sau.
- Sort stack: $O(\log n)$ — tạm thời.
- **Max tại một thời điểm:** $O(n) + O(n^2) = O(n^2)$ (sau khi C cấp phát, B vẫn tồn tại).

**Kết luận:** Time $O(n^2)$, space $O(n^2)$.

### Ví dụ 19 — Tối ưu space

```text
Function BetterProcess(A, n):
    sort(A)                                // in-place sort — O(log n) space
    C = new Matrix(n, n)                   // O(n²) space
    for i = 1 to n:
        for j = 1 to n:
            C[i][j] = A[i] + A[j]
    return C
```

**So sánh:**

- Time: $O(n^2)$ — giống trên (sort $O(n\log n)$ bị áp đảo).
- Space: $O(n^2)$ — **không cần B**, tiết kiệm $O(n)$.

**Bài học:** Đôi khi có thể **loại bỏ biến phụ** — giảm space mà không tăng time.

### Ví dụ 20 — DP với rolling array

**Bài toán Knapsack 0/1 — naive 2D:**

```text
Knapsack(weights, values, n, W):
    dp = new Matrix(n+1, W+1)              // O(nW) space
    for i = 1 to n:
        for w = 0 to W:
            if weights[i] > w:
                dp[i][w] = dp[i-1][w]
            else:
                dp[i][w] = max(dp[i-1][w], dp[i-1][w-weights[i]] + values[i])
    return dp[n][W]
```

- Time: $O(nW)$.
- Space: $O(nW)$.

**Tối ưu — rolling array 1D:**

```text
Knapsack(weights, values, n, W):
    dp = new Array(W+1)                    // O(W) space
    for i = 1 to n:
        for w = W downto weights[i]:       // duyệt ngược!
            dp[w] = max(dp[w], dp[w-weights[i]] + values[i])
    return dp[W]
```

- Time: $O(nW)$ — **giống hệt**.
- Space: $O(W)$ — **giảm từ $O(nW)$**.

**Tại sao duyệt ngược?** Để không dùng lại `dp[w-weights[i]]` đã cập nhật ở bước $i$ — đảm bảo mỗi vật chỉ dùng một lần.

**Bài học:** Kỹ thuật **rolling array** giảm space từ $O(nW)$ xuống $O(W)$ — cực kỳ quan trọng khi $n$ lớn.

> **Lý thuyết bên cạnh:** Rolling array là kỹ thuật **chuẩn** trong DP — dựa trên quan sát rằng bước $i$ chỉ cần bước $i-1$. Khi đó, thay vì giữ toàn bộ bảng $n \times W$, chỉ giữ một hàng (hoặc hai hàng — "previous" và "current"). Đây là ví dụ điển hình của việc **giảm space mà không tăng time**.

---

## 13. Bài tập tự luyện

**Bài 1:** Phân tích auxiliary space của:

```text
Sum(A, n):
    s = 0
    for i = 1 to n:
        s += A[i]
    return s
```

*Đáp:* $O(1)$ — chỉ có biến `s`, `i`.

**Bài 2:** Phân tích auxiliary space của:

```text
Copy(A, n):
    B = new Array(n)
    for i = 1 to n:
        B[i] = A[i]
    return B
```

*Đáp:* $O(n)$ — mảng B.

**Bài 3:** Phân tích auxiliary space của Binary Search đệ quy.

*Đáp:* $O(\log n)$ — độ sâu đệ quy $\log_2 n$.

**Bài 4:** Phân tích auxiliary space của Merge Sort.

*Đáp:* $O(n)$ — mảng phụ trong Merge (stack $O(\log n)$ bị áp đảo).

**Bài 5:** Phân tích auxiliary space của Fibonacci naive.

*Đáp:* $O(n)$ — độ sâu đệ quy $n$, **không phải** $\varphi^n$ (số nút).

**Bài 6:** Tối ưu space của đoạn code sau:

```text
Sum(A, n):
    prefix = new Array(n)
    prefix[0] = A[0]
    for i = 1 to n:
        prefix[i] = prefix[i-1] + A[i]
    return prefix[n-1]
```

*Đáp:* Không cần mảng prefix — chỉ cần biến tổng:

```text
Sum(A, n):
    total = 0
    for i = 1 to n:
        total += A[i]
    return total
```

Space giảm từ $O(n)$ xuống $O(1)$.

**Bài 7:** Tại sao Quick Sort được gọi là in-place dù tốn $O(\log n)$ stack?

*Đáp:* Vì $\log n$ rất nhỏ so với $n$ — coi là "gần như không tốn space". Theo quy ước, in-place = auxiliary space $O(1)$ hoặc $O(\log n)$.

**Bài 8:** So sánh space của BFS và DFS trên cây nhị phân.

*Đáp:*

- BFS: $O(n)$ worst — queue chứa cả tầng cuối (có thể $n/2$ nút).
- DFS: $O(\log n)$ trên cây cân bằng, $O(n)$ worst trên cây suy biến.
- **DFS thường tiết kiệm hơn** trên cây rộng.

**Bài 9:** Tối ưu space của LCS từ $O(nm)$ xuống $O(\min(n,m))$.

*Hướng:* Chỉ giữ hai hàng (previous và current) — vì $dp[i][j]$ chỉ phụ thuộc $dp[i-1][j]$ và $dp[i][j-1]$.

**Bài 10:** Tại sao nhân ma trận không thể in-place?

*Đáp:* Vì output là ma trận $n \times n$ — cần ít nhất $\Omega(n^2)$ space để chứa kết quả. Không thể giảm dưới ngưỡng này.

---

## 14. Tóm tắt một trang

### Định nghĩa

$$
S(n) = \text{bộ nhớ tối đa tiêu thụ khi chạy với đầu vào kích thước } n
$$

**Auxiliary space** = space **không tính input** — thường được dùng khi so sánh thuật toán.

### Hai thành phần

- **Fixed part:** $O(1)$ — biến đơn giản, hằng số.
- **Variable part:** $f(n)$ — mảng, cấu trúc dữ liệu, recursion stack.

$$
S(n) = O(1) + f(n) = O(f(n))
$$

### Recursion space

$$
S_{\text{recursion}} = O(\text{độ sâu đệ quy})
$$

**Không phải** số nút trong cây đệ quy.

### Các pattern thường gặp

| Cấu trúc | Space |
|---|---|
| Biến đơn giản | $O(1)$ |
| Mảng $n$ | $O(n)$ |
| Ma trận $n \times n$ | $O(n^2)$ |
| Recursion depth $d$ | $O(d)$ |
| Hash table $n$ | $O(n)$ |
| Adjacency list | $O(V+E)$ |
| Adjacency matrix | $O(V^2)$ |

### In-place algorithms

**Định nghĩa:** Auxiliary space $O(1)$ hoặc $O(\log n)$.

| In-place | Không in-place |
|---|---|
| Bubble, Selection, Insertion Sort | Merge Sort |
| Heap Sort | Counting Sort |
| Quick Sort ($O(\log n)$) | Radix Sort |

### Space-Time tradeoff

- Tăng space có thể giảm time (hash table).
- Giảm space có thể tăng time (sort thay hash).
- **Có thể tối ưu cả hai** (Fibonacci tối ưu).

### Kỹ thuật tối ưu space

- **Rolling array:** DP từ $O(nm)$ xuống $O(m)$.
- **Loại biến phụ:** Không lưu những gì có thể tính lại.
- **In-place:** Swap tại chỗ thay vì tạo mảng mới.
- **Iterative thay recursive:** Giảm stack.

### Bẫy cần tránh

1. Space **không tính input** (thường).
2. Space là **max theo thời gian**, không cộng dồn.
3. Recursion space = **độ sâu**, không phải số nút.
4. Cùng $O(n)$ nhưng **hằng số khác** (Array vs Linked List).
5. "In-place" = $O(1)$ **hoặc** $O(\log n)$ — không nhất thiết $O(1)$.
6. Time và space **có thể cùng tối ưu** — không nhất thiết đánh đổi.
7. Output lớn ⇒ space **không thể** nhỏ hơn output (cận dưới tự nhiên).

---

## 15. So sánh với Time Complexity — bảng tổng hợp

| Tiêu chí | Time | Space |
|---|---|---|
| **Đo** | Số phép toán | Bộ nhớ tối đa |
| **Phụ thuộc** | Thuật toán | Thuật toán + dữ liệu |
| **Recursion** | Số lời gọi | Độ sâu |
| **Tối ưu** | Giảm vòng lặp | Giảm biến phụ |
| **Tradeoff** | Có | Có (ngược chiều) |
| **Cận dưới** | Bài toán (lower bound) | Output size |
| **In-place** | — | $O(1)$ hoặc $O(\log n)$ |

### Quy trình phân tích đầy đủ

1. **Phân tích time** — đếm phép toán, xác định bậc.
2. **Phân tích space** — liệt kê biến, xác định max.
3. **Kiểm tra tradeoff** — có thể tối ưu thêm không?
4. **Xem xét ứng dụng** — time hay space quan trọng hơn?

---

Nếu bạn muốn, mình có thể đi sâu:
- **Phân tích space** cho thuật toán cụ thể (DP, đồ thị, chuỗi) từng bước.
- **Kỹ thuật tối ưu space** chi tiết (rolling array, bit manipulation, compression).
- **Space complexity của recursion** — tail call optimization, continuation.
- **Bài tập nâng cao** — phân tích space cho hệ thống lớn, streaming algorithms (space $O(1)$ với input vô hạn).
- **Ứng dụng thực tế** — thiết kế hệ thống với ràng buộc bộ nhớ.