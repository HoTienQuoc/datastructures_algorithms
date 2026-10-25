# Asymptotic Notation (Ký hiệu tiệm cận) — Lý thuyết chi tiết + Ví dụ

## 1. Động cơ và vai trò

Khi phân tích thuật toán, ta muốn mô tả **tốc độ tăng** của hàm chi phí $T(n)$ mà **không phụ thuộc** vào:

- Hằng số nhân (do máy, ngôn ngữ, compiler)
- Số hạng bậc thấp (chỉ chi phối khi $n$ nhỏ)
- Giá trị cụ thể của $n$

**Asymptotic notation** là **ngôn ngữ toán học** để diễn đạt các so sánh này một cách chặt chẽ. Nó gồm 5 ký hiệu: $O, \Omega, \Theta, o, \omega$.

### Tại sao cần 5 ký hiệu?

Vì khi so sánh hai hàm, có **5 khả năng**:

| Quan hệ                              | Ký hiệu         | Ví dụ                    |
| ------------------------------------ | --------------- | ------------------------ |
| $f$ tăng **chậm hơn hoặc bằng** $g$  | $f = O(g)$      | $n = O(n^2)$             |
| $f$ tăng **nhanh hơn hoặc bằng** $g$ | $f = \Omega(g)$ | $n^2 = \Omega(n)$        |
| $f$ tăng **đúng bằng** $g$           | $f = \Theta(g)$ | $3n^2 + 5 = \Theta(n^2)$ |
| $f$ tăng **chậm hơn thực sự** $g$    | $f = o(g)$      | $n = o(n^2)$             |
| $f$ tăng **nhanh hơn thực sự** $g$   | $f = \omega(g)$ | $n^2 = \omega(n)$        |

**Tương tự số thực:**

$$
O \leftrightarrow\ \le, \quad \Omega \leftrightarrow\ \ge, \quad \Theta \leftrightarrow\ =, \quad o \leftrightarrow\ <, \quad \omega \leftrightarrow\ >
$$

Đây là **phép loại suy chính xác** và rất hữu ích để nhớ.

> **Lý thuyết bên cạnh:** Có 5 ký hiệu vì ta cần phân biệt "bằng hoặc nhỏ hơn" ($O$) với "nhỏ hơn thực sự" ($o$). Nếu chỉ có $O$ và $\Theta$, ta không diễn đạt được "tăng chậm hơn hẳn".

---

## 2. Nền tảng: định lượng và ký hiệu tập hợp

Trước khi vào định nghĩa, cần nhớ **cấu trúc định lượng** (quantifier structure):

- $\exists$ = "tồn tại"
- $\forall$ = "với mọi"
- Thứ tự quantifier **rất quan trọng**: $\exists c\, \forall n$ khác $\forall n\, \exists c$.

**Ví dụ minh họa tầm quan trọng:**

- "$\exists c \in \mathbb{R}, \forall n \in \mathbb{N}: n \le c$" — **Sai** (không có hằng số chặn trên mọi số tự nhiên).
- "$\forall n \in \mathbb{N}, \exists c \in \mathbb{R}: n \le c$" — **Đúng** (với mỗi $n$, chọn $c = n$).

Trong asymptotic notation, ta luôn dùng **$\exists c, n_0$** trước, rồi **$\forall n \ge n_0$** sau — tức là **hằng số không phụ thuộc $n$**.

**Ký hiệu tập hợp:** $O(g(n))$ là **tập hợp các hàm**, không phải một hàm. Chính xác:

$$
O(g(n)) = \{f(n) : \exists c > 0, n_0 > 0, \forall n \ge n_0,\ 0 \le f(n) \le c\,g(n)\}
$$

> **Lý thuyết bên cạnh:** Đây là lý do dấu $=$ trong $f(n) = O(g(n))$ là **quy ước lạm dụng** (abuse of notation). Đúng ra phải viết $f(n) \in O(g(n))$. Hệ quả quan trọng: **không thể** "trừ hai vế" hay "chuyển vế" như đẳng thức thực sự.

---

## 3. Năm ký hiệu — định nghĩa hình thức

Cho $f(n)$ và $g(n)$ là các hàm xác định trên số nguyên dương, **không âm** với $n$ đủ lớn.

### 3.1. $O$ (big-O) — cận trên tiệm cận

$$
\boxed{f(n) = O(g(n)) \iff \exists\, c > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le f(n) \le c\,g(n)}
$$

**Đọc là:** "$f$ là big-O của $g$" hoặc "$f$ thuộc $O(g)$".

**Trực giác:** Với $n$ đủ lớn, $f$ **không vượt quá** $g$ sai khác một hằng số $c$.

**Phủ định (để chứng minh $f \ne O(g)$):**

$$
f(n) \ne O(g(n)) \iff \forall c > 0, \forall n_0 > 0, \exists n \ge n_0 : f(n) > c\,g(n)
$$

**Ví dụ đúng:** $3n^2 + 5n + 7 = O(n^2)$. (Chứng minh ở §5.)

**Ví dụ sai:** $n^2 \ne O(n)$. (Chứng minh ở §5.)

### 3.2. $\Omega$ (big-Omega) — cận dưới tiệm cận

$$
\boxed{f(n) = \Omega(g(n)) \iff \exists\, c > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le c\,g(n) \le f(n)}
$$

**Trực giác:** Với $n$ đủ lớn, $f$ **không nhỏ hơn** $g$ sai khác hằng số.

**Ví dụ:** $3n^2 + 5n + 7 = \Omega(n^2)$ (vì $3n^2 + 5n + 7 \ge 3n^2$).

### 3.3. $\Theta$ (big-Theta) — cận chặt

$$
\boxed{f(n) = \Theta(g(n)) \iff f(n) = O(g(n)) \text{ và } f(n) = \Omega(g(n))}
$$

Tương đương:

$$
\exists\, c_1, c_2 > 0,\ n_0 > 0 : \forall n \ge n_0,\ 0 \le c_1 g(n) \le f(n) \le c_2 g(n)
$$

**Trực giác:** $f$ và $g$ **cùng bậc tăng** — bị chặn cả trên lẫn dưới bởi bội số của $g$.

**Tính chất đặc biệt:** $\Theta$ có **tính đối xứng**: $f = \Theta(g) \iff g = \Theta(f)$.

### 3.4. $o$ (little-o) — cận trên chặt

$$
\boxed{f(n) = o(g(n)) \iff \forall c > 0,\ \exists n_0 > 0 : \forall n \ge n_0,\ 0 \le f(n) < c\,g(n)}
$$

Tương đương với (khi $g(n) > 0$ với $n$ đủ lớn):

$$
\lim_{n \to \infty} \frac{f(n)}{g(n)} = 0
$$

**Trực giác:** $f$ tăng **chậm hơn thực sự** so với $g$ — bất kỳ hằng số $c > 0$ nào cũng "thắng" $f$ khi $n$ đủ lớn.

**Khác biệt với $O$:** $O$ cho phép $f$ và $g$ **cùng bậc** (với hằng số). $o$ **bắt buộc** $f$ phải nhỏ hơn $g$ về bậc.

**Ví dụ:** $n = o(n^2)$; $n^2 \ne o(n^2)$ (vì $\lim n^2/n^2 = 1 \ne 0$).

### 3.5. $\omega$ (little-omega) — cận dưới chặt

$$
\boxed{f(n) = \omega(g(n)) \iff \forall c > 0,\ \exists n_0 > 0 : \forall n \ge n_0,\ 0 \le c\,g(n) < f(n)}
$$

Tương đương:

$$
\lim_{n \to \infty} \frac{f(n)}{g(n)} = \infty
$$

**Trực giác:** $f$ tăng **nhanh hơn thực sự** so với $g$.

**Ví dụ:** $n^2 = \omega(n)$; $n^2 \ne \omega(n^2)$.

### 3.6. Bảng đối chiếu 5 ký hiệu

| Ký hiệu  | Định nghĩa                | Giới hạn                 | Loại suy |
| -------- | ------------------------- | ------------------------ | -------- |
| $O$      | $f \le c\,g$, $n \ge n_0$ | $\lim \sup f/g < \infty$ | $\le$    |
| $\Omega$ | $f \ge c\,g$, $n \ge n_0$ | $\lim \inf f/g > 0$      | $\ge$    |
| $\Theta$ | $c_1 g \le f \le c_2 g$   | $0 < \lim f/g < \infty$  | $=$      |
| $o$      | $f < c\,g$, $\forall c$   | $\lim f/g = 0$           | $<$      |
| $\omega$ | $f > c\,g$, $\forall c$   | $\lim f/g = \infty$      | $>$      |

> **Lý thuyết bên cạnh:** Với $O$ và $\Omega$, dùng $\limsup$ và $\liminf$ thay vì $\lim$ vì giới hạn có thể **không tồn tại** (hàm dao động). Với $o$ và $\omega$, giới hạn phải tồn tại và bằng $0$ hoặc $\infty$.

---

## 4. Quan hệ giữa các ký hiệu

### 4.1. Sơ đồ quan hệ

$$
\Theta(g) = O(g) \cap \Omega(g)
$$

$$
o(g) \subset O(g), \quad \omega(g) \subset \Omega(g)
$$

$$
o(g) = O(g) \setminus \Omega(g) \quad \text{(khi giới hạn tồn tại)}
$$

**Minh họa bằng Venn:**

```
        O(g)
    ┌─────────────┐
    │   o(g)      │
    │  ┌───────┐  │
    │  │ Θ(g)  │  │
    │  └───────┘  │
    │             │
    └─────────────┘
    (tương tự với Ω và ω)
```

### 4.2. Bảng suy luận

| Từ                       | Suy ra                        | Không suy ra                                 |
| ------------------------ | ----------------------------- | -------------------------------------------- |
| $f = \Theta(g)$          | $f = O(g)$, $f = \Omega(g)$   | —                                            |
| $f = o(g)$               | $f = O(g)$, $f \ne \Omega(g)$ | $f = \Theta(g)$                              |
| $f = O(g)$               | —                             | $f = \Theta(g)$, $f = o(g)$, $f = \Omega(g)$ |
| $f = O(g)$ và $g = O(f)$ | $f = \Theta(g)$               | —                                            |
| $f = o(g)$               | $g = \omega(f)$               | $f = O(g)$ hay không? Có, luôn đúng          |

> **Lý thuyết bên cạnh:** $f = O(g)$ **không** kéo theo $f = \Theta(g)$ — vì $O$ chỉ là cận trên. Ví dụ: $n = O(n^2)$ nhưng $n \ne \Theta(n^2)$.

### 4.3. Tính chất đại số

**Phản xạ (reflexivity):**

$$
f = O(f), \quad f = \Omega(f), \quad f = \Theta(f)
$$

Nhưng **không** có $f = o(f)$ hay $f = \omega(f)$ (vì $\lim f/f = 1$).

**Bắc cầu (transitivity):** Nếu $f = O(g)$ và $g = O(h)$ thì $f = O(h)$. Tương tự cho $\Omega, \Theta, o, \omega$.

**Đối xứng (symmetry):** Chỉ $\Theta$ có:

$$
f = \Theta(g) \iff g = \Theta(f)
$$

**Phản đối xứng (transpose symmetry):**

$$
f = O(g) \iff g = \Omega(f), \quad f = o(g) \iff g = \omega(f)
$$

> **Lý thuyết bên cạnh:** $O$ và $\Omega$ là **cặp đối ngẫu** qua transpose symmetry. $o$ và $\omega$ cũng vậy. Chỉ $\Theta$ là **tự đối xứng**. Đây là lý do $\Theta$ mạnh hơn — nó nói "đúng bậc" theo cả hai chiều.

---

## 5. Chứng minh bằng định nghĩa — tìm $c$ và $n_0$

### 5.1. Phương pháp

**Bước 1:** Viết bất đẳng thức cần chứng minh: $f(n) \le c\,g(n)$ (với $O$) hoặc $c\,g(n) \le f(n)$ (với $\Omega$).

**Bước 2:** "Nới lỏng" các số hạng — thay bằng phiên bản lớn hơn (với $O$) hoặc nhỏ hơn (với $\Omega$) mà vẫn dễ xử lý.

**Bước 3:** Tìm hằng số $c$ và điều kiện $n_0$.

**Bước 4:** Kiểm tra lại.

**Mẹo quan trọng:** Không cần $c$ tối ưu — chỉ cần **tồn tại** một $c$ hợp lệ. Đây là điều giúp việc chứng minh đơn giản hơn nhiều.

### 5.2. Ví dụ 1 — Chứng minh $f(n) = 3n^2 + 5n + 7 = O(n^2)$

**Cần tìm:** $c > 0, n_0$ sao cho $3n^2 + 5n + 7 \le c\,n^2$ với mọi $n \ge n_0$.

**Nới lỏng:** Với $n \ge 1$:

$$
3n^2 + 5n + 7 \le 3n^2 + 5n^2 + 7n^2 = 15n^2
$$

**Chọn:** $c = 15$, $n_0 = 1$. Kiểm tra: $3n^2 + 5n + 7 \le 15n^2$ với mọi $n \ge 1$. $\blacksquare$

### 5.3. Ví dụ 2 — Chứng minh $f(n) = 3n^2 + 5n + 7 = \Omega(n^2)$

**Cần tìm:** $c > 0, n_0$ sao cho $3n^2 + 5n + 7 \ge c\,n^2$.

**Nới lỏng:** Với $n \ge 1$, $5n + 7 \ge 0$, nên:

$$
3n^2 + 5n + 7 \ge 3n^2
$$

**Chọn:** $c = 3$, $n_0 = 1$. $\blacksquare$

### 5.4. Ví dụ 3 — Kết luận $\Theta$

Từ hai ví dụ trên:

$$
3n^2 \le 3n^2 + 5n + 7 \le 15n^2, \quad \forall n \ge 1
$$

Vậy $3n^2 + 5n + 7 = \Theta(n^2)$ với $c_1 = 3$, $c_2 = 15$, $n_0 = 1$.

### 5.5. Ví dụ 4 — Chứng minh $100n + 5 = O(n)$

**Cần tìm:** $c > 0, n_0$ sao cho $100n + 5 \le c\,n$ với $n \ge n_0$.

**Phân tích:** Hằng số 5 là "bẫy" — với $n$ nhỏ, $5$ chi phối. Ta cần $n_0$ đủ lớn để $5 \le n$.

**Nới lỏng:** Với $n \ge 5$: $5 \le n$, nên:

$$
100n + 5 \le 100n + n = 101n
$$

**Chọn:** $c = 101$, $n_0 = 5$. $\blacksquare$

### 5.6. Ví dụ 5 — Chứng minh $6n^3 \ne O(n^2)$

**Phản chứng.** Giả sử $6n^3 = O(n^2)$. Khi đó tồn tại $c > 0, n_0$ sao cho:

$$
6n^3 \le c\,n^2, \quad \forall n \ge n_0
$$

Chia hai vế cho $n^2 > 0$:

$$
6n \le c, \quad \forall n \ge n_0
$$

Nhưng với $n = \max(n_0, \lceil c/6 \rceil + 1)$, ta có $6n > c$ — **mâu thuẫn**. Vậy $6n^3 \ne O(n^2)$. $\blacksquare$

### 5.7. Ví dụ 6 — Chứng minh $n^2 = o(n^3)$

**Dùng định nghĩa:** Với mọi $c > 0$, cần tìm $n_0$ sao cho $n^2 < c\,n^3$ với $n \ge n_0$.

Bất đẳng thức tương đương $1 < c\,n$, hay $n > 1/c$. Chọn $n_0 = \lfloor 1/c \rfloor + 1$. $\blacksquare$

**Hoặc dùng giới hạn:**

$$
\lim_{n \to \infty} \frac{n^2}{n^3} = \lim_{n \to \infty} \frac{1}{n} = 0
$$

Vậy $n^2 = o(n^3)$.

### 5.8. Ví dụ 7 — Chứng minh $n^2 \ne o(n^2)$

Vì $\lim n^2/n^2 = 1 \ne 0$, nên $n^2 \ne o(n^2)$. (Nhưng $n^2 = O(n^2)$ — đúng, vì $O$ cho phép cùng bậc.)

### 5.9. Ví dụ 8 — Hàm có hằng số cộng "khó chịu" — $n = O(n\log n)$

**Cần tìm:** $c > 0, n_0$ sao cho $n \le c\,n\log n$ với $n \ge n_0$.

Với $n \ge 2$, $\log n \ge 1$, nên $n \le n\log n$.

**Chọn:** $c = 1$, $n_0 = 2$. $\blacksquare$

> **Lý thuyết bên cạnh:** Kỹ thuật "nới lỏng" thường dùng các **hằng số đẹp** như 1, 2, hoặc "tổng các hệ số". Không cần tìm hằng số tối ưu. Đây là điểm mạnh lớn của định nghĩa — chứng minh **dễ hơn** so với việc tính chính xác.

---

## 6. Phương pháp giới hạn (Limit Method)

### 6.1. Công thức

Khi $g(n) > 0$ với $n$ đủ lớn, xét:

$$
L = \lim_{n \to \infty} \frac{f(n)}{g(n)}
$$

| $L$              | Kết luận                                        |
| ---------------- | ----------------------------------------------- |
| $L = 0$          | $f = o(g)$ ⇒ $f = O(g)$, $f \ne \Omega(g)$      |
| $0 < L < \infty$ | $f = \Theta(g)$                                 |
| $L = \infty$     | $f = \omega(g)$ ⇒ $f = \Omega(g)$, $f \ne O(g)$ |
| không tồn tại    | Cần xét $\limsup$ và $\liminf$                  |

### 6.2. Ví dụ 9 — $f(n) = 5n^2 + 3n$, $g(n) = n^2$

$$
L = \lim_{n\to\infty} \frac{5n^2 + 3n}{n^2} = \lim_{n\to\infty}\left(5 + \frac{3}{n}\right) = 5
$$

$0 < 5 < \infty$ ⇒ $f = \Theta(n^2)$.

### 6.3. Ví dụ 10 — $f(n) = n^3$, $g(n) = 2^n$

$$
L = \lim_{n\to\infty} \frac{n^3}{2^n} \overset{\text{L'Hôpital}}{=} \lim_{n\to\infty} \frac{3n^2}{2^n \ln 2} = \cdots = 0
$$

(Ap dụng L'Hôpital 3 lần.) Vậy $n^3 = o(2^n)$.

### 6.4. Ví dụ 11 — $f(n) = n$, $g(n) = n\log n$

$$
L = \lim_{n\to\infty} \frac{n}{n\log n} = \lim_{n\to\infty} \frac{1}{\log n} = 0
$$

Vậy $n = o(n\log n)$.

### 6.5. Ví dụ 12 — $f(n) = \log_2 n$, $g(n) = \ln n$

$$
L = \lim_{n\to\infty} \frac{\log_2 n}{\ln n} = \lim_{n\to\infty} \frac{\ln n / \ln 2}{\ln n} = \frac{1}{\ln 2} \approx 1.44
$$

$0 < L < \infty$ ⇒ $\log_2 n = \Theta(\ln n)$. **Đây là lý do "cơ số log không quan trọng"** trong phân tích tiệm cận.

### 6.6. Ví dụ 13 — $f(n) = n!$, $g(n) = n^n$

Dùng **xấp xỉ Stirling**:

$$
n! \approx \sqrt{2\pi n}\left(\frac{n}{e}\right)^n
$$

$$
\frac{n!}{n^n} \approx \frac{\sqrt{2\pi n}}{e^n} \to 0
$$

Vậy $n! = o(n^n)$.

### 6.7. Ví dụ 14 — $f(n) = n!$, $g(n) = 2^n$

$$
\frac{n!}{2^n} \approx \frac{\sqrt{2\pi n} \cdot n^n}{e^n \cdot 2^n} = \sqrt{2\pi n} \cdot \left(\frac{n}{2e}\right)^n \to \infty
$$

Vậy $n! = \omega(2^n)$.

### 6.8. Ví dụ 15 — Hàm dao động (không tồn tại giới hạn)

Xét $f(n) = n$, $g(n) = n(1 + \sin n)$.

Vì $1 + \sin n$ dao động giữa $0$ và $2$, tỉ số $f/g$ **không có giới hạn**. Tuy nhiên:

- Vì $1 + \sin n \le 2$, ta có $f(n) \ge g(n)/2$ ⇒ $f = \Omega(g)$.
- Vì $1 + \sin n$ có thể tùy ý gần 0, $f/g$ có thể lớn tùy ý ⇒ $f \ne O(g)$.

Vậy $f = \Omega(g)$ nhưng $f \ne O(g)$, và $f \ne \Theta(g)$. **Đây là lý do phải dùng $\limsup$ và $\liminf$ trong trường hợp tổng quát.**

> **Lý thuyết bên cạnh:** Phương pháp giới hạn **rất tiện** khi hàm "đẹp" (đa thức, log, mũ). Nhưng với hàm dao động hoặc có hằng số phức tạp, phải quay lại **định nghĩa** hoặc dùng $\limsup/\liminf$.

---

## 7. Quy tắc tính toán

### 7.1. Quy tắc cộng

$$
O(f(n)) + O(g(n)) = O(\max(f(n), g(n)))
$$

Tương tự cho $\Omega$ và $\Theta$.

**Ví dụ:** $O(n) + O(n^2) = O(n^2)$.

**Cảnh báo:** Quy tắc này chỉ đúng khi **số hạng tử là cố định**. Nếu tổng có $n$ số hạng, mỗi $O(n)$, tổng là $n \cdot O(n) = O(n^2)$ — không phải $O(n)$.

### 7.2. Quy tắc nhân

$$
O(f(n)) \cdot O(g(n)) = O(f(n) \cdot g(n))
$$

Tương tự cho $\Omega, \Theta, o, \omega$.

**Ví dụ:** Vòng ngoài $O(n)$, vòng trong $O(n)$ ⇒ tổng $O(n^2)$.

### 7.3. Quy tắc với hằng số

Với $c > 0$ bất kỳ:

$$
c \cdot O(f) = O(f)
$$

Hằng số nhân **không ảnh hưởng** đến bậc.

### 7.4. Bỏ số hạng bậc thấp

$$
n^2 + n\log n + n + \log n = \Theta(n^2)
$$

Chỉ giữ số hạng **tăng nhanh nhất**.

### 7.5. Ví dụ tổng hợp

Cho:

$$
T(n) = 3n^2 + 2n\log n + 100n + 5000
$$

**Phân tích:**

- $3n^2 = \Theta(n^2)$
- $2n\log n = \Theta(n\log n) = o(n^2)$
- $100n = \Theta(n) = o(n^2)$
- $5000 = \Theta(1) = o(n^2)$

Tổng: $T(n) = \Theta(n^2)$.

> **Lý thuyết bên cạnh:** Quy tắc cộng $O(f) + O(g) = O(\max(f, g))$ đúng cho **số hạng tử cố định**, nhưng cần cẩn thận khi số hạng tử phụ thuộc $n$. Đây là bẫy phổ biến trong phân tích vòng lặp.

---

## 8. Bảng thứ bậc tăng trưởng

Từ **chậm đến nhanh**:

$$
1 \ \prec\ \log\log n \ \prec\ \log n \ \prec\ \sqrt{\log n} \ \prec\ \log^2 n \ \prec\ \sqrt{n} \ \prec\ n \ \prec\ n\log n \ \prec\ n^2 \ \prec\ n^3 \ \prec\ 2^n \ \prec\ n! \ \prec\ n^n
$$

Ký hiệu $\prec$ = $o$ ("tăng chậm hơn thực sự").

### Các quan hệ "vàng" cần nhớ

Với mọi $k > 0$ và $\varepsilon > 0$:

$$
\log^k n = o(n^{\varepsilon})
$$

Với mọi $k > 0$ và $c > 1$:

$$
n^k = o(c^n)
$$

Với mọi $c > 1$:

$$
c^n = o(n!)
$$

**Ví dụ áp dụng:**

- $\log^{100} n = o(n^{0.001})$ — log mũ 100 vẫn chậm hơn căn bậc nghìn của $n$.
- $n^{1000} = o(1.001^n)$ — đa thức bậc 1000 vẫn chậm hơn hàm mũ cơ số 1.001.
- $2^n = o(n!)$.

> **Lý thuyết bên cạnh:** Ba quan hệ trên là "xương sống" của thứ bậc tăng trưởng. Chúng giải thích vì sao $O(\log n)$ được coi là "gần như hằng số", và vì sao thuật toán $O(2^n)$ chỉ dùng được cho $n$ rất nhỏ.

---

## 9. Bảng tóm tắt minh họa trực quan

Với $n = 1{,}000{,}000$ (một triệu):

| Hàm         | Giá trị xấp xỉ   | Ý nghĩa thực tế     |
| ----------- | ---------------- | ------------------- |
| $\log_2 n$  | $20$             | Tức thời            |
| $\sqrt{n}$  | $1{,}000$        | Tức thời            |
| $n$         | $10^6$           | $\sim 1$ ms         |
| $n\log_2 n$ | $2 \cdot 10^7$   | $\sim 20$ ms        |
| $n^2$       | $10^{12}$        | $\sim 17$ phút      |
| $n^3$       | $10^{18}$        | $\sim 31{,}000$ năm |
| $2^n$       | $10^{301{,}030}$ | **không thể**       |

**Kết luận thực tiễn:** Sự khác biệt giữa $O(n^2)$ và $O(n^3)$ không phải "vài lần" — mà là **hàng triệu lần**. Order of growth quyết định **tính khả thi**.

---

## 10. Những hiểu nhầm phổ biến

### Hiểu nhầm 1 — "$f = O(g)$ nghĩa là $f$ luôn nhỏ hơn $g$"

**Sai.** $f = O(g)$ chỉ nói $f \le c\,g$ với $n$ đủ lớn. Với $n$ nhỏ, $f$ có thể lớn hơn $g$. Ví dụ: $n = O(n^2)$, nhưng với $n = 0.5$ thì $n > n^2$.

### Hiểu nhầm 2 — "$O$ là worst case, $\Omega$ là best case"

**Sai.** $O$ và $\Omega$ mô tả **quan hệ giữa hai hàm**, không phải case. Tuy nhiên, có **quy ước thực tế**: khi nói "thuật toán là $O(n^2)$" không nói case, thường ngầm hiểu là worst case.

### Hiểu nhầm 3 — "$f = O(g)$ và $g = O(h)$ suy ra $f = \Theta(h)$"

**Sai.** Ví dụ: $n = O(n^2)$ và $n^2 = O(n^3)$, nhưng $n \ne \Theta(n^3)$ — chênh hai bậc.

**Đúng:** Nếu $f = O(g)$ **và** $g = O(f)$ thì $f = \Theta(g)$.

### Hiểu nhầm 4 — "$f = O(g)$ và $f = \Omega(g)$ suy ra $f = \Theta(g)$"

**Đúng.** Đây là **định nghĩa** của $\Theta$.

### Hiểu nhầm 5 — "$O(2n) = O(n)$ nên mọi hằng số đều bỏ được"

**Đúng về lý thuyết tiệm cận.** Nhưng khi **so sánh hai thuật toán cùng bậc**, hằng số quan trọng. Ví dụ: $10^9 n$ và $n$ cùng $O(n)$, nhưng thực tế chênh $10^9$ lần.

### Hiểu nhầm 6 — "$O(f)$ là một hàm"

**Sai.** $O(f)$ là một **tập hợp hàm**. Ký hiệu $f = O(g)$ là lạm dụng; đúng ra $f \in O(g)$.

**Hệ quả:** Không thể nói $O(n) = O(n^2)$ theo nghĩa đẳng thức. Đúng ra: nếu $f \in O(n)$ thì $f \in O(n^2)$ (vì $O(n) \subset O(n^2)$).

### Hiểu nhầm 7 — "Bỏ hằng số là luôn đúng"

**Sai với $o$ và $\omega$.** Với $o$ và $\omega$, hằng số **không** được bỏ qua: $2n \ne o(n)$ (vì $\lim 2n/n = 2 \ne 0$).

### Hiểu nhầm 8 — "$f = o(g)$ suy ra $f \le g$"

**Sai.** $f = o(g)$ chỉ nói **tỉ số** $f/g \to 0$, không nói $f \le g$ tại mọi $n$. Ví dụ: $f(n) = 2n$, $g(n) = n^2$ — ta có $f = o(g)$, nhưng $f(1) = 2 > g(1) = 1$.

### Hiểu nhầm 9 — "$\Theta$ mạnh hơn $O$ nên luôn dùng $\Theta$"

**Sai.** $\Theta$ chỉ tồn tại khi $f$ và $g$ **cùng bậc**. Nếu $f$ có thể nhỏ hơn $g$ rất nhiều (ví dụ best case), chỉ nói được $f = O(g)$ hoặc $f = \Omega(g)$, không phải $\Theta$.

### Hiểu nhầm 10 — "$\log n$ cùng bậc với $\log_2 n$ nên mọi log đều như nhau"

**Đúng về bậc**, nhưng **khác hằng số**. $\log_2 n \approx 3.32 \ln n$. Trong thực tế, cơ số ảnh hưởng hằng số, không ảnh hưởng bậc.

> **Lý thuyết bên cạnh:** Hầu hết hiểu nhầm đến từ việc **quên rằng $O$ là quan hệ giữa các hàm**, không phải giá trị. Khi đọc $f = O(g)$, hãy đọc là "$f$ thuộc lớp hàm bị chặn trên bởi $g$ (sai khác hằng số)". Điều này giúp tránh nhiều lỗi.

---

## 11. Tổng hợp — khi nào dùng ký hiệu nào?

| Mục đích                | Ký hiệu  | Ví dụ                             |
| ----------------------- | -------- | --------------------------------- |
| Nói "không chậm hơn"    | $O$      | Worst case $O(n^2)$               |
| Nói "không nhanh hơn"   | $\Omega$ | Best case $\Omega(n)$             |
| Nói "đúng bậc"          | $\Theta$ | Average $\Theta(n\log n)$         |
| Nói "chậm hơn thực sự"  | $o$      | $n = o(n\log n)$                  |
| Nói "nhanh hơn thực sự" | $\omega$ | $n^2 = \omega(n\log n)$           |
| Chứng minh lower bound  | $\Omega$ | Comparison sort $\Omega(n\log n)$ |
| Chứng minh upper bound  | $O$      | Merge sort $O(n\log n)$           |

**Quy ước thực tế:** Khi nói "thuật toán X là $O(f)$" **mà không nói case**, thường ngầm hiểu là **worst case**. Khi cần chính xác, hãy ghi rõ "worst case", "average case", "best case".

---

## 12. Bài tập tự luyện

**Bài 1:** Chứng minh $n^2 + 3n + 2 = \Theta(n^2)$ bằng định nghĩa.

*Hướng:* Chọn $c_1 = 1$ (vì $n^2 + 3n + 2 \ge n^2$), $c_2 = 6$ (vì $n^2 + 3n + 2 \le n^2 + 3n^2 + 2n^2 = 6n^2$ với $n \ge 1$), $n_0 = 1$.

**Bài 2:** Chứng minh $f(n) = n^3 - 5n \ne \Omega(n^4)$.

*Hướng:* Phản chứng. Giả sử $n^3 - 5n \ge c\,n^4$ với $n \ge n_0$. Chia $n^3$: $1 - 5/n^2 \ge c\,n$. Vế trái $\le 1$, vế phải $\to \infty$ — mâu thuẫn.

**Bài 3:** So sánh $2^{2n}$ và $2^n$.

*Hướng:* $\lim 2^{2n}/2^n = \lim 2^n = \infty$ ⇒ $2^{2n} = \omega(2^n)$.

**Bài 4:** Xác định quan hệ giữa $f(n) = n^{\log n}$ và $g(n) = 2^{n}$.

*Hướng:* Lấy log cả hai: $\log f = (\log n)^2 = o(n) = \log g$. Vậy $f = o(g)$.

**Bài 5:** Chứng minh $\log(n!) = \Theta(n\log n)$.

*Hướng:* Dùng chặn: $\log(n!) = \sum_{i=1}^n \log i \ge \sum_{i=\lceil n/2 \rceil}^n \log(n/2) = \frac{n}{2}\log(n/2) = \Omega(n\log n)$. Và $\log(n!) \le n\log n = O(n\log n)$. Vậy $\Theta(n\log n)$.

**Bài 6:** Cho $f(n) = 3n + 2$ và $g(n) = n + 1$. Chứng minh $f = \Theta(g)$.

*Hướng:* $c_1 = 2$, $c_2 = 4$, $n_0 = 1$ hoạt động.

**Bài 7:** Tìm phản ví dụ cho khẳng định sai: "Nếu $f = O(g)$ và $h = O(g)$ thì $f = \Theta(h)$."

*Hướng:* $f(n) = n$, $g(n) = n^2$, $h(n) = 1$. Cả $f = O(g)$ và $h = O(g)$ nhưng $f \ne \Theta(h)$.

---

## 13. Tóm tắt một trang

**Định nghĩa cốt lõi:**

- $f = O(g)$: $\exists c, n_0, \forall n \ge n_0: f(n) \le c\,g(n)$.
- $f = \Omega(g)$: $\exists c, n_0, \forall n \ge n_0: f(n) \ge c\,g(n)$.
- $f = \Theta(g)$: $f = O(g)$ và $f = \Omega(g)$.
- $f = o(g)$: $\lim f/g = 0$.
- $f = \omega(g)$: $\lim f/g = \infty$.

**Phương pháp chứng minh:**

1. Định nghĩa: tìm $c$ và $n_0$ (thường bằng "nới lỏng").
2. Giới hạn: tính $\lim f/g$ (dùng L'Hôpital, Stirling).
3. Phản chứng: giả sử sai → mâu thuẫn.

**Quy tắc:**

- Bỏ hằng số nhân.
- Bỏ số hạng bậc thấp.
- Tổng cố định: max.
- Tích: nhân các bậc.
- Log: bỏ cơ số.

**Thứ bậc:**

$$
1 \prec \log n \prec \sqrt{n} \prec n \prec n\log n \prec n^2 \prec n^3 \prec 2^n \prec n! \prec n^n
$$

**Quan hệ "vàng":**

- $\log^k n = o(n^\varepsilon)$.
- $n^k = o(c^n)$ với $c > 1$.
- $c^n = o(n!)$.

**Lưu ý:** Dấu $=$ trong $f = O(g)$ là lạm dụng; đúng ra $f \in O(g)$. Không "chuyển vế" được.

---

Nếu bạn muốn đi sâu tiếp, mình có thể:
- **Chứng minh tính chất** transitivity, transpose symmetry cho từng ký hiệu.
- **Phân tích chi tiết** average case với ký hiệu tiệm cận (biến chỉ báo + linearity of expectation).
- **Bài tập nâng cao** về so sánh hàm phức tạp (ví dụ $n^{\log\log n}$ vs $2^{\sqrt{n}}$, $n^{\log n}$ vs $(\log n)^n$).
- **Ứng dụng** asymptotic notation trong phân tích thuật toán cụ thể (DFS, Dijkstra, DP).