## 🔢 Ước Số và Bội Số (Divisors & Multiples)
<br>
<div class="step-card border-blue"><div class="step-badge bg-blue">1. Đếm ước và Tính tổng ước</div>

**🔹 Cách 1: Vét cạn ($O(n)$):** Duyệt qua tất cả các số từ $1$ đến $n$. Cách này chỉ dùng khi $n$ nhỏ ($n \le 10^7$).
```cpp
void bruteForceDivisors(long long n, long long & count, long long & sum) {
    count = 0;
    sum = 0;
    for (long long i = 1; i <= n; i++) {
        if (n % i == 0) {
            count++;
            sum += i;
        }
    }
```
**🔹 Cách 2: Duyệt đến căn bậc hai ($O(\sqrt{n})$)**
* **Phân tích:** Nếu $i$ là ước của $n$ thì $n/i$ cũng là ước của $n$. Trong hai ước này, chắc chắn có một ước $\le \sqrt{n}$.
```cpp
void countDivisors(long long n, long long & count, long long & sum) {
    count = 0;
    sum = 0;
    for (int i = 1; i * i <= n; i++) {
        if (n % i == 0) {
            count++; sum += i; // i là ước
            if (i * i != n) { // Tránh đếm trùng nếu n là số chính phương
                count++; sum += (n / i);
            }
        }
    }
}
```
**🔹 Cách 3: Dựa trên phân tích thừa số nguyên tố (Cực nhanh)**
* **Công thức:** Nếu $n = p_1^{a_1} \cdot p_2^{a_2} \cdot \dots \cdot p_k^{a_k}$:
* **Số lượng ước:** $D(n) = (a_1 + 1)(a_2 + 1)\dots(a_k + 1)$
* **Tổng các ước:** $S(n) = \frac{p_1^{a_1+1}-1}{p_1-1} \cdot \frac{p_2^{a_2+1}-1}{p_2-1} \dots$
```cpp
// Hàm đếm ước dựa trên phân tích thừa số nguyên tố
void fastDivisorAnalysis(long long n, long long & count, long long & sum) {
    count = 1; // Tích các (a_i + 1)
    sum = 1;   // Tích các (p^(a+1)-1)/(p-1)
    long long temp = n;

    for (long long i = 2; i * i <= temp; i++) {
        if (temp % i == 0) {
            int exponent = 0;
            long long p_pow = 1;
            
            while (temp % i == 0) {
                exponent++;
                temp /= i;
                p_pow *= i;
            }
            
            // Cập nhật số lượng ước
            count *= (exponent + 1);
            
            // Cập nhật tổng ước theo công thức cấp số nhân
            // sum *= (p^(a+1) - 1) / (p - 1)
            sum *= (p_pow * i - 1) / (i - 1);
        }
    }
    
    // Nếu n là số nguyên tố lớn hơn căn bậc 2 còn sót lại
    if (temp > 1) {
        count *= (1 + 1);
        sum *= (temp * temp - 1) / (temp - 1);
    }
}
```
</div>

<div class="step-card border-orange"><div class="step-badge bg-orange">2. Ước chung lớn nhất (ƯCLN - GCD)</div>

**Thuật toán Euclid:** Dựa trên tính chất $gcd(a, b) = gcd(b, a \pmod b)$.
```cpp
long long gcd(long long a, long long b) {
    while (b) {
        a %= b;
        swap(a, b);
    }
    return a;
}
```
<div class="important-note">

💡 Tìm GCD của n số: `gcd(a1, a2, ..., an) = gcd(a1, gcd(a2, ...))` 
</div>
</div>

<div class="step-card border-yellow"><div class="step-badge bg-yellow">3. Sàng ước số</div>

**🔹 Phân tích toán học:**
* **Ý tưởng cốt lõi:** Một số $i$ là ước của $j$ nếu $j$ là bội của $i$.
* **Cơ chế:** Thay vì xét từng số $n$ rồi đi tìm các ước của nó ($O(N\sqrt{N})$), ta xét từng số $i$ từ $1$ đến $N$ và đi "tặng" một đơn vị ước cho tất cả các bội số của nó là $i, 2i, 3i, \dots$ trong đoạn $[1, N]$.
* **Độ phức tạp:** Tổng số lần thực hiện các phép tính là $N/1 + N/2 + N/3 + \dots + N/N$. Đây là một dãy điều hòa có tổng xấp xỉ $O(N \ln N)$. Tốc độ này nhanh hơn rất nhiều so với $O(N\sqrt{N})$ khi $N$ lớn.

**🔹 Ví dụ minh họa:** (Đếm số lượng ước cho đoạn $[1, 6]$)
1. Khởi tạo: Mảng `count[7]` gồm toàn số `0`.
2. Xét $i = 1$: Các bội của $1$ là $\{1, 2, 3, 4, 5, 6\}$. Tăng count của chúng lên 1. `count = {0, 1, 1, 1, 1, 1, 1}`
3. Xét $i = 2$: Các bội của $2$ là $\{2, 4, 6\}$. Tăng count của chúng lên 1. `count = {0, 1, 2, 1, 2, 1, 2}`
4. Xét $i = 3$: Các bội của $3$ là $\{3, 6\}$. Tăng count của chúng lên 1. `count = {0, 1, 2, 2, 2, 1, 3}`
5. Xét $i = 4, 5, 6$: Tương tự, mỗi số chỉ có đúng một bội trong đoạn là chính nó. Kết quả cuối cùng: `count[6] = 4` (Các ước của 6 là 1, 2, 3, 6).

```cpp
const int MAXN = 1e6+5;
int numDivisors[MAXN];  // Mảng lưu số lượng ước
long long sumDivisors[MAXN]; // Mảng lưu tổng các ước

void sieveOfDivisors(int n) {
    // Khởi tạo
    for (int i = 1; i <= n; i++) {
        numDivisors[i] = 0;
        sumDivisors[i] = 0;
    }

    // Tiến hành sàng
    for (int i = 1; i <= n; i++) {
        // i đóng vai trò là ước, j là bội của i
        for (int j = i; j <= n; j += i) {
            numDivisors[j]++;      // Tăng số lượng ước của j thêm 1 (là i)
            sumDivisors[j] += i;   // Cộng giá trị i vào tổng ước của j
        }
    }
}
```
</div>

</div><div class="step-card border-green"><div class="step-badge bg-green">4. Hai số nguyên tố cùng nhau</div>

* **Khái niệm:** Hai số $a, b$ được gọi là nguyên tố cùng nhau nếu $gcd(a, b) = 1$.
* **Tính chất:** Nếu $a, b$ nguyên tố cùng nhau thì $gcd(a \cdot c, b) = gcd(c, b)$.
* **Ứng dụng:** Dùng để tối giản phân số, tính toán trong các bài toán đồng dư và hàm Phi Euler.
</div>

<div class="step-card border-purple"><div class="step-badge bg-purple">5. Bội chung nhỏ nhất (BCNN - LCM)</div>

* **Khái niệm:** Bội chung nhỏ nhất của $a$ và $b$ là số nhỏ nhất chia hết cho cả $a$ và $b$.
* **Thuật toán:** Dựa trên mối liên hệ giữa GCD và LCM.
<div class="math-formula">
$lcm(a, b) = \frac{|a \cdot b|}{gcd(a, b)}$
</div>

```cpp
long long lcm(long long a, long long b) {
    if (a == 0 || b == 0) return 0;
    // Chia trước khi nhân để tránh tràn số long long
    return (a / gcd(a, b)) * b;
}
```
</div>

<div class="important-note">
💡 <b>Lưu ý lập trình:</b>

* Kỹ thuật <b>Sàng ước số</b> tương tự như Sàng Eratosthenes để đạt độ phức tạp $O(N \log N)$ thay vì $O(N\sqrt{N})$ cực kỳ hiệu quả khi bài toán yêu cầu xử lý nhiều truy vấn về ước số trong dải $N \le 10^6$.
* Luôn lưu ý trường hợp Số chính phương khi đếm ước bằng vòng lặp $\sqrt{n}$ để không bị đếm lặp ước $i$ khi $i = n/i$.
</div>