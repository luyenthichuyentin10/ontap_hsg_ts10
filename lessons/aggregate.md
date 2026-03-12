## 🧩 Lý thuyết Tập hợp & Tổ hợp
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Quy tắc đếm cơ bản</div>

**Quy tắc cộng:** Nếu một công việc có thể thực hiện theo một trong hai **phương án** (trường hợp) $A$ (có $n$ cách) hoặc $B$ (có $m$ cách), và các phương án này không trùng lặp, thì có $n + m$ cách hoàn thành công việc.

**Quy tắc nhân:** Nếu một công việc bao gồm hai giai đoạn liên tiếp: **giai đoạn 1** có $n$ cách thực hiện, **giai đoạn 2** có $m$ cách thực hiện, thì có $n \times m$ cách hoàn thành công việc.

**Ví dụ:** Để đi từ thành phố A đến C phải qua B. Từ A đến B có 3 đường, từ B đến C có 4 đường. Số cách đi là $3 \times 4 = 12$ cách.
```cpp
// Ứng dụng quy tắc nhân tính số lượng ước (xem bài div_mul.md)
long long countDivisors(int n) {
    long long res = 1;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            int cnt = 0;
            while (n % i == 0) { cnt++; n /= i; }
            res *= (cnt + 1); // Quy tắc nhân các lựa chọn số mũ
        }
    }
    if (n > 1) res *= 2;
    return res;
}
```
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Hoán vị (Permutation)</div>

**Khái niệm:** Cho tập hợp $n$ phần tử khác nhau. Mỗi cách sắp xếp thứ tự của $n$ phần tử này được gọi là một hoán vị.

**Công thức:** 
<div class="math-formula">$P_n = n! = 1 \cdot 2 \cdot 3 \dots n$</div>

**Ví dụ:** Có 5 học sinh xếp hàng ngang, số cách xếp là $5! = 120$ cách.
```cpp
#include <algorithm>
// Sử dụng hàm có sẵn trong C++ để liệt kê hoán vị
void listPermutations(vector<int> a) {
    sort(a.begin(), a.end());
    do {
        for(int x : a) cout << x << " ";
        cout << endl;
    } while (next_permutation(a.begin(), a.end()));
}
```
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Chỉnh hợp (Arrangement)</div>

**Khái niệm:** Lấy ra $k$ phần tử từ $n$ phần tử và sắp xếp chúng theo một thứ tự nào đó.

**Công thức:** 
<div class="math-formula">$A_n^k = \frac{n!}{(n-k)!} = n(n-1)\dots(n-k+1)$</div>

**Ví dụ:** Một lớp có 30 học sinh, bầu ra 1 lớp trưởng và 1 lớp phó. Số cách là $A_{30}^2 = 30 \times 29 = 870$.
```cpp
long long arrangement(int n, int k) {
    if (k > n) return 0;
    long long res = 1;
    for (int i = 0; i < k; i++) res *= (n - i);
    return res;
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">4. Chỉnh hợp lặp</div>

**Khái niệm:** Lấy ra $k$ phần tử từ $n$ phần tử, mỗi phần tử có thể lặp lại nhiều lần, và có tính thứ tự.

**Công thức:**
<div class="math-formula">$\bar{A}_n^k = n^k$</div>

**Ví dụ:** Có thể tạo bao nhiêu mã PIN gồm 4 chữ số (mỗi số từ 0-9)? Đáp án: $10^4 = 10,000$.
```cpp
// Tính n^k bằng lũy thừa nhanh (xem bài modular.md)
long long fastPow(long long n, long long k) {
    long long res = 1;
    while (k > 0) {
        if (k % 2 == 1) res *= n;
        n *= n; k /= 2;
    }
    return res;
}
```
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">5. Tổ hợp (Combination)</div>

**Khái niệm:** Lấy ra $k$ phần tử từ $n$ phần tử nhưng không quan tâm đến thứ tự.

**Công thức:**

<div class="math-formula">$$C_n^k = \binom{n}{k} = \frac{n!}{k!(n-k)!}$$</div>

**Tính chất Pascal:** $C_n^k = C_{n-1}^{k-1} + C_{n-1}^k$ (Dùng để lập bảng tổ hợp bằng quy hoạch động).
```cpp
// Tính tổ hợp bằng Quy hoạch động (Tránh tràn số)
long long C[100][100];
void prepareCombination(int n) {
    for (int i = 0; i <= n; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++)
            C[i][j] = C[i-1][j-1] + C[i-1][j];
    }
}
```
</div>

<div class="important-note">
💡 <b>Ghi chú lập trình:</b>

- **Nhận diện dạng toán:** Bài toán có cần thứ tự không? Nếu có dùng Chỉnh hợp, không có dùng Tổ hợp.
- **Xử lý số dư:** Khi $n, k$ lớn, các bài toán thường yêu cầu lấy dư ($MOD$). Hãy sử dụng Nghịch đảo Modulo để tính $C_n^k = \frac{n!}{k!(n-k)!} \pmod M$.
- **Tràn số:** $n!$ tăng rất nhanh, hãy dùng long long hoặc làm việc trực tiếp với logarit nếu chỉ cần so sánh.
</div>