## 🔢 Lý thuyết Đồng dư & Nghịch đảo Modulo
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Định nghĩa & Tính chất cơ bản</div>

Hai số nguyên $a$ và $b$ được gọi là đồng dư theo mô-đun $m$ nếu chúng có cùng số dư khi chia cho $m$.
<div class="math-formula">
$a \equiv b \pmod m \iff (a - b) \vdots m$
</div>

Ví dụ: $18 \equiv 3 \pmod 5$ vì $18 \div 5$ dư $3$ và $3 \div 5$ cũng dư $3$ (hoặc $18 - 3 = 15$ chia hết cho $5$).
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các phép toán Đồng dư</div>

Trong lập trình thi đấu, khi xử lý số cực lớn, ta sử dụng quy tắc: ***"Số dư của một tổng / hiệu / tích bằng tổng / hiệu / tích của các số dư"***.
<div class="math-formula">
$(A + B)$ $\%$ $M = ((A$ $\%$ $M) + (B$ $\%$ $M))$ $\%$ $M$<br>
$(A - B)$ $\%$ $M = ((A$ $\%$ $M) - (B$ $\%$ $M) + M)$ $\%$ $M$<br>
$(A \times B)$ $\%$ $M = ((A$ $\%$ $M) \times (B$ $\%$ $M))$ $\%$ $M$
</div>

Ví dụ: Tính $(13 \times 11 \times 8) \% 5$:
<table class="breakdown-table">
                    <tr>
                        <th>Bước 1: Lấy dư từng số</th>
                        <th>Bước 2: Nhân các số dư</th>
                        <th>Bước 3: Lấy dư kết quả</th>
                    </tr>
                    <tr>
                        <td>$13$ $\%$ $5 = \mathbf{3}$<br>$11$ $\%$ $5 = \mathbf{1}$<br>$8$ $\%$ $5 = \mathbf{3}$</td>
                        <td>$3 \times 1 \times 3 = \mathbf{9}$</td>
                        <td>$9$ $\%$ $5 = \mathbf{4}$</td>
                    </tr>
                </table>
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Nghịch đảo Modulo (Modular Inverse)</div>

**Tại sao cần?** 

Phép chia không có tính chất tách nhỏ số dư trực tiếp như cộng, trừ, nhân.
<div class="math-formula" style="background-color: #fee2e2; border-color: #ef4444;">

⚠️ Sai lầm: $(A / B)$ $\%$ $M \neq ((A$ $\%$ $M) / (B$ $\%$ $M))$ $\%$ $M$
</div>

=> Do đó, để thực hiện phép chia cho $B$, ta phải nhân với Nghịch đảo Modulo của $B$ ($B^{-1}$).

**Định nghĩa:**

Số nguyên $x$ là nghịch đảo modulo của $a$ theo mô-đun $m$ nếu:
<div class="math-formula">

$a \cdot x \equiv 1 \pmod m$
</div>

**Điều kiện tồn tại:**

Nghịch đảo modulo của $a \pmod m$ chỉ tồn tại khi $a$ và $m$ nguyên tố cùng nhau: $gcd(a, m) = 1$.

**Ứng dụng:** 
- Tính Tổ hợp $nCk \pmod M$ khi có phép chia giai thừa ở mẫu số.
- Giải phương trình đồng dư bậc nhất.
- Thuật toán mật mã RSA.
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Phương pháp tìm Nghịch đảo Modulo</div>

**Cách 1: Thuật toán Euclid mở rộng**

Áp dụng cho mọi trường hợp khi $gcd(a, m) = 1$ (không yêu cầu $m$ là số nguyên tố).
```cpp
// Hàm tìm gcd(a, b) đồng thời tìm x, y sao cho ax + by = gcd(a, b)
long long extended_gcd(long long a, long long b, long long &x, long long &y) {
    if (b == 0) {
        x = 1; y = 0;
        return a;
    }
    long long x1, y1;
    long long d = extended_gcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - y1 * (a / b);
    return d;
}

// Hàm tìm nghịch đảo modulo
long long modInverseEuclid(long long a, long long m) {
    long long x, y;
    long long g = extended_gcd(a, m, x, y);
    if (g != 1) return -1; // Không tồn tại
    return (x % m + m) % m;
}
```
**Cách 2: Định lý Fermat nhỏ**

Chỉ áp dụng khi $m$ là số nguyên tố. Khi đó, nghịch đảo của $a$ là $a^{m-2} \pmod m$.
```cpp
// Hàm tính lũy thừa nhanh (a^b) % m
long long power(long long a, long long b, long long m) {
    long long res = 1;
    a %= m;
    while (b > 0) {
        if (b % 2 == 1) res = (res * a) % m;
        a = (a * a) % m;
        b /= 2;
    }
    return res;
}

// Hàm tìm nghịch đảo modulo bằng định lý Fermat nhỏ
long long modInverseFermat(long long a, long long m) {
    if (__gcd(a, m) != 1) return -1; // Không tồn tại
    return power(a, m - 2, m);
}
```
</div>

<div class="important-note">
💡 <b>Ghi chú quan trọng:</b>

- Khi thực hiện phép trừ $(A - B) \% M$, kết quả có thể âm. Luôn sử dụng công thức (A - B + M) % M để đảm bảo kết quả luôn dương.
- Trong các bài toán tính tổ hợp với $M$ là số nguyên tố lớn ($10^9 + 7$), phương pháp **Fermat nhỏ** kết hợp với lũy thừa nhanh là lựa chọn phổ biến nhất.
</div>