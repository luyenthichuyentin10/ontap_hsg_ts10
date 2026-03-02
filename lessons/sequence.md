## 📈 Dãy số (Sequences)
<br>
<div class="step-card border-blue"><div class="step-badge bg-blue">1. Tổng Gauss (Tổng các số tự nhiên liên tiếp)</div>

Đây là công thức cơ bản nhất để tính tổng các số từ $1$ đến $n$.

**Công thức:**
<div class="math-formula">
 $S = 1 + 2 + 3 + \dots + n = \frac{n(n + 1)}{2}$
</div>

**Ứng dụng:** Tính tổng nhanh mà không cần dùng vòng lặp, giúp đạt độ phức tạp $O(1)$.
```cpp
long long gaussSum(long long n) {
    return n * (n + 1) / 2;
}
```
</div>

<div class="step-card border-orange"><div class="step-badge bg-orange">2. Cấp số cộng (Arithmetic Progression - AP)</div>

Dãy số mà mỗi số hạng (kể từ số hạng thứ hai) bằng số hạng đứng trước nó cộng với một số không đổi $d$ (công sai).
* **Công thức tính số hạng thứ n trong dãy cấp số cộng** 
<div class="math-formula">
$u_n = u_1 + (n - 1)d$.
<div class="formula-notes">

Trong đó:
- $u_n$ là số hạng thứ $n$
- $u_1$ là số hạng đầu tiên
- $n$ thứ tự của số hạng
- $d$ công sai
</div>
</div>

* **Công thức tính tổng $n$ số hạng đầu tiên cấp số cộng:** 
<div class="math-formula">
$S_n = \frac{n(u_1 + u_n)}{2} = \frac{n[2u_1 + (n - 1)d]}{2}$
<div class="formula-notes">

Trong đó:
- $u_n$ là số hạng thứ $n$
- $u_1$ là số hạng đầu tiên
- $n$ thứ tự của số hạng
- $d$ công sai
</div>  
</div>

* **Ví dụ:** Dãy $3, 7, 11, 15, \dots$ có $u_1 = 3, d = 4$.
</div>

<div class="step-card border-green"><div class="step-badge bg-green">3. Cấp số nhân (Geometric Progression - GP)</div>

Dãy số mà mỗi số hạng (kể từ số hạng thứ hai) bằng số hạng đứng trước nó nhân với một số không đổi $q$ (công bội).
* **Công thức tính số hạng thứ n trong dãy cấp số nhân:** 
<div class="math-formula">
$u_n = u_1 \cdot q^{n-1}$.
<div class="formula-notes">

Trong đó:
- $u_n$ là số hạng thứ $n$
- $u_1$ là số hạng đầu tiên
- $n$ thứ tự của số hạng
- $q$ công bội
</div> 
</div>

* **Công thức tính tổng $n$ số hạng đầu tiên cấp số nhân ($q \neq 1$):** 
<div class="math-formula">
$S_n = u_1 \cdot \frac{q^n - 1}{q - 1}$
<div class="formula-notes">

Trong đó:
- $u_1$ là số hạng đầu tiên
- $n$ thứ tự của số hạng
- $q$ công bội
</div> 
</div>
</div>

<div class="step-card border-yellow">
    <div class="step-badge bg-yellow">4. Dãy số Fibonacci</div>
    
Dãy số bắt đầu bằng **0** và **1**, các số hạng sau bằng tổng hai số hạng đứng trước nó.
    
<div class="math-formula">
        $F_n = F_{n-1} + F_{n-2}$
<div class="formula-notes">
            
Trong đó
- $F_n$: Số hạng thứ $n$ cần tìm
- $F_{n-1}, F_{n-2}$: Hai số hạng liền trước trong dãy
- Điều kiện: $F_0 = 0, F_1 = 1$ với $n \ge 2$
</div>
</div>

```cpp
long long getFibonacci(int n) {
    if (n <= 1) return n;
    long long f0 = 0, f1 = 1, fn;
    for (int i = 2; i <= n; i++) {
        fn = f0 + f1;
        f0 = f1; f1 = fn;
    }
    return fn;
}
```
**🔹 Ứng dụng:**
* **Bài toán leo cầu thang:** Có $n$ bậc thang, mỗi bước đi 1 hoặc 2 bậc. Số cách đi là $F_{n+1}$.
* **Lát gạch:** Số cách lát sân $2 \times n$ bằng gạch $1 \times 2$.
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">5. Dãy số Catalan</div>
    
Dãy số Catalan $C_n$ là một dãy số nguyên xuất hiện trong rất nhiều bài toán đếm cấu trúc tổ hợp.
    
<div class="math-formula">
$C_n = \frac{1}{n+1} \binom{2n}{n}$
<div class="formula-notes">

Trong đó:
- $C_n$</b>: Số Catalan thứ $n$
- $\binom{2n}{n}$</b>: Tổ hợp chập $n$ của $2n$ phần tử
</div>
</div>

```cpp
long long getCatalan(int n) {
    if (n == 0) return 1;
    long long res = 1;
    for (int i = 0; i < n; i++) {
        res = res * 2 * (2 * i + 1) / (i + 2);
    }
    return res;
}
```
**🔹 Ứng dụng kinh điển:**
* **Dấu ngoặc:** Số cách đặt $n$ cặp dấu ngoặc `()` hợp lệ.
* **Cây nhị phân:** Số lượng cây nhị phân có $n$ nút.
* **Đường đi Dyck:** Số đường đi từ $(0,0)$ đến $(n,n)$ không vượt quá đường chéo $y=x$.
</div>

<div class="step-card border-red"><div class="step-badge bg-red">4. Ứng dụng trong Lập trình thi đấu</div>

* **Tối ưu hóa thời gian:** Chuyển bài toán duyệt vòng lặp $O(N)$ sang tính toán công thức $O(1)$ hoặc $O(\log N)$ (khi dùng lũy thừa nhanh cho cấp số nhân).
* **Đếm số lượng số hạng:** Trong đoạn $[L, R]$ có bước nhảy $d$, số lượng số hạng là: $\text{count} = \frac{R - L}{d} + 1$ (Điều kiện: $L$ và $R$ là các số hạng của dãy).

* **Bài toán tổng ước:** Tổng các ước của một số $n$ thực chất là tích của các tổng cấp số nhân từ các thừa số nguyên tố: $S(n) = \prod \frac{p_i^{a_i+1}-1}{p_i-1}$.
</div>

<div class="important-note">
💡 <b>Mẹo tránh tràn số:</b>

* Khi tính `n * (n + 1) / 2`, nếu $n$ lớn ($10^9$), kết quả sẽ vượt quá `int`. Luôn sử dụng `long long`.
* Trong phép tính `(a / b) * c` của công thức tổng, hãy thực hiện phép chia sau cùng hoặc kiểm tra tính chia hết để tránh mất dữ liệu phần thập phân.
* Đối với các bài toán cấp số nhân, Catalan hoặc Fibonacci yêu cầu lấy dư cho một số $M$, hãy cẩn thận với phép chia trong công thức. Ta nên kết hợp kiến thức về **Nghịch đảo Modulo** để đảm bảo tính chính xác

</div>