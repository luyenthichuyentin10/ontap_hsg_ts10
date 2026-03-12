## 🔢 Dấu hiệu chia hết & Nguyên lý Bao hàm - Loại trừ
<br>
<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Các dấu hiệu chia hết cơ bản</div>
    
Trong lập trình, việc kiểm tra `n % k == 0` là đơn giản nhất. Tuy nhiên, với số nguyên lớn **(BigNum)**, ta cần áp dụng các dấu hiệu sau:

**1. Các quy tắc chia hết:**
* **Chia hết cho 2 hoặc 5:** chữ số tận cùng chia hết cho `2` hoặc `5`.
* **Chia hết cho 4 hoặc 25:** 2 chữ số tận cùng chia hết cho `4` hoặc `25`.
* **Chia hết cho 8 hoặc 125:** 3 chữ số tận cùng chia hết cho `8` hoặc `125`.
* **Chia hết cho 3 hoặc 9:** tổng các chữ số chia hết cho `3` hoặc `9`.
* **Chia hết cho 6:** một số chia hết cho `6` nếu số đó chia hết cho `2` và `3`
* **Chia hết cho 11:** hiệu của (Tổng các chữ số ở vị trí lẻ) và (Tổng các chữ số ở vị trí chẵn) chia hết cho `11`.
* **Chia hết cho 7:** lấy chữ số cuối cùng nhân `2`, sau đó lấy phần còn lại của số đó trừ đi kết quả vừa tính. Nếu hiệu thu được chia hết cho `7` (bao gồm cả số `0`), thì số ban đầu chia hết cho `7`.

**2. Ví dụ và thuật toán kiểm tra**
* Quy tắc chia hết cho $2, 5, 4, 8$ ***(Dựa trên chữ số tận cùng)***
    * Các quy tắc này tập trung vào phần đuôi của số, rất hiệu quả khi xử lý xâu ký tự.
    * Ví dụ: Kiểm tra số $N = 1234567890456$ có chia hết cho 8 không?
    * Giải thích: Ta chỉ cần xét 3 chữ số cuối là $456$. Vì $456 \div 8 = 57$ (chia hết), nên $123456 \div 8$.
```cpp
// Kiểm tra chia hết cho 8 (Xét 3 chữ số cuối)
bool div8(string n) {
    if (n.length() < 3) return stoi(n) % 8 == 0;
    string last3 = n.substr(n.length() - 3);
    return stoi(last3) % 8 == 0;
}

// Kiểm tra chia hết cho 5 (Xét chữ số cuối)
bool div5(string n) {
    char last = n.back();
    return (last == '0' || last == '5');
}
```
* Quy tắc chia hết cho $3$ và $9$ ***(Tổng các chữ số)***
    * Quy tắc này giúp tránh việc thực hiện phép chia trực tiếp trên các số có hàng nghìn chữ số.
    * Ví dụ: Số $N = 12345$ có chia hết cho $3$ không?
    * Giải thích: Tổng các chữ số: $1 + 2 + 3 + 4 + 5 = 15$. Vì $15 \div 3 = 5$, nên $12345$ chia hết cho $3$.
```cpp
// Kiểm tra chia hết cho 9 (Tổng chữ số chia hết cho 9)
bool div9(string n) {
    int sum = 0;
    for (char c : n) sum += (c - '0');
    return sum % 9 == 0;
}
```
* Quy tắc chia hết cho $11$ ***(Tổng vị trí chẵn/lẻ)***
    * Đây là kỹ thuật so khớp vị trí thường dùng trong các bài toán đố về số học.
    * Ví dụ: Số $N = 13574$ có chia hết cho $11$ không?
    * Giải thích: 
        * Tổng vị trí lẻ (từ phải sang): $4 + 5 + 1 = 10$.
        * Tổng vị trí chẵn (từ phải sang): $7 + 3 = 10$.
        * Hiệu: $10 - 10 = 0$. Vì $0$ chia hết cho 11, nên $13574$ chia hết cho $11$.
```cpp
bool div11(string n) {
    int sum_odd = 0, sum_even = 0;
    for (int i = 0; i < n.length(); i++) {
        if (i % 2 == 0) sum_even += (n[i] - '0');
        else sum_odd += (n[i] - '0');
    }
    return abs(sum_odd - sum_even) % 11 == 0;
}
```
* Quy tắc chia hết cho $7$ ***(Cắt và trừ)***
    * Quy tắc này mang tính thuật toán cao, thường dùng minh họa cho các bài toán đệ quy hoặc vòng lặp xử lý xâu.
    * Ví dụ: Số $N = 3101$ có chia hết cho 7 không?
    * Giải thích: 
        1. $310 - (1 \times 2) = 308$.
        2. $30 - (8 \times 2) = 14$.
        3. Vì $14 \div 7 = 2$, nên $3101$ chia hết cho $7$.
```cpp
// Cách tối ưu nhất cho BigNum (Modular Arithmetic)
bool div7_fast(string n) {
    int rem = 0;
    for (char c : n) {
        rem = (rem * 10 + (c - '0')) % 7;
    }
    return rem == 0;
}
```
<div class="important-note">

**💡 Lời khuyên khi lập trình thi đấu:**
* Với các số nhỏ, hãy dùng toán tử `%` truyền thống.
* Với các số cực lớn (`string`), phương pháp chia dư từng chữ số là **"vũ khí"** vạn năng nhất vì nó áp dụng được cho mọi số chia $k$ mà không cần nhớ từng quy tắc riêng lẻ.
```cpp
// Cách tối ưu nhất cho BigNum (Modular Arithmetic)
bool div7_fast(string n) {
    int rem = 0;
    for (char c : n) {
        rem = (rem * 10 + (c - '0')) % 7;
    }
    return rem == 0;
}
```
</div>
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Kỹ thuật đếm số lượng số chia hết</div>

**Bài toán:** Có bao nhiêu số trong đoạn $[1, N]$ chia hết cho $k$?

**Công thức:**
<div class="math-formula"> $count = \lfloor \frac{N}{k} \rfloor$.</div>

**Mở rộng:** Số lượng số trong đoạn $[L, R]$ chia hết cho $k$ là:
<div class="math-formula"> $\lfloor \frac{R}{k} \rfloor - \lfloor \frac{L-1}{k} \rfloor$ </div>

```cpp
long long countDivisible(long long L, long long R, long long k) {
    return (R / k) - ((L - 1) / k);
}
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Nguyên lý Bao hàm - Loại trừ (Inclusion-Exclusion)</div>
    
**Bài toán:** Đếm số lượng số trong đoạn $[1, N]$ chia hết cho $a$ hoặc chia hết cho $b$.

**Phân tích các bước thực hiện:** 
1. Các số chia hết cho $a$: $N/a$.
2. Các số chia hết cho $b$: $N/b$.
3. Các số chia hết cho cả $a$ và $b$ (tức là chia hết cho $BCNN(a, b)$): $N/BCNN(a, b)$.

**Công thức** 
<div class="important-note" style="display: flex; flex-direction: column; align-items: center; justify-content: center; background: #1e293b; padding: 25px; border-radius: 12px; margin: 20px 0; border: 1px dashed #334155; font-family: 'Consolas', monospace; color: #d4d4d4;">
    <div style="display: flex; justify-content: center; align-items: center; gap: 15px; font-weight: bold; margin-bottom: 20px;">
        <span style="color: #4ec9b0;">|A ∪ B|</span> = 
        <span style="color: #569cd6;">|A|</span> + 
        <span style="color: #f59e0b;">|B|</span> - 
        <span style="color: #fbbf24;">|A ∩ B|</span>
    </div>
    <div style="position: relative; width: 300px; height: 180px;">
        <div style="position: absolute; left: 0; top: 0; width: 180px; height: 180px; border-radius: 50%; background-color: rgba(86, 156, 214, 0.4); border: 2px solid #569cd6; display: flex; align-items: center; justify-content: flex-start; padding-left: 20px; box-sizing: border-box;">
            <b style="color: #fff; font-size: 1.2rem; transform: translateX(-10px);">A</b>
        </div>
        <div style="position: absolute; right: 0; top: 0; width: 180px; height: 180px; border-radius: 50%; background-color: rgba(245, 158, 11, 0.4); border: 2px solid #f59e0b; display: flex; align-items: center; justify-content: flex-end; padding-right: 20px; box-sizing: border-box;">
            <b style="color: #fff; font-size: 1.2rem; transform: translateX(10px);">B</b>
        </div>
        <div style="position: absolute; left: 120px; top: 0px; width: 60px; height: 180px; background-color: transparent; border-radius: 50% / 0 100% 100% 0; border-left: 1px solid transparent; display: flex; align-items: center; justify-content: center; z-index: 1;">
            </div>
        <div style="position: absolute; left: 120px; top: 0; width: 60px; height: 180px; display: flex; align-items: center; justify-content: center; z-index: 10;">
             <b style="color: #fbbf24; font-size: 0.9rem;">A ∩ B</b>
        </div>
    </div>
    <p style="color: #94a3b8; font-size: 0.85rem; margin-top: 15px; font-style: italic; text-align: center;">
        <i>(Vùng <span style="color: #4ec9b0;">A ∪ B</span> là phần hợp của hai tập hợp sẽ bằng số phần tử trong tập A cộng với số phần tử trong tâp B sau đó trừ đi vùng <span style="color: #fbbf24;">A ∩ B</span> là phần giao nhau của hai tập hợp, bị tính hai lần nên cần trừ đi một lần </i>
    </p>
</div>

```cpp
long long gcd(long long a, long long b) {
    while (b) { a %= b; swap(a, b); }
    return a;
}

long long lcm(long long a, long long b) {
    return (a / gcd(a, b)) * b;
}

long long countAorB(long long n, long long a, long long b) {
    return (n / a) + (n / b) - (n / lcm(a, b));
}
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">4. Bao hàm - Loại trừ với 3 tập hợp</div>
    
**Bài toán:** Đếm số lượng số chia hết cho $a, b$ hoặc $c$.

**Phân tích các bước thực hiện:**
* **Bước 1:** Tính các tập hợp đơn lẻ
    * Số các số chia hết cho $a$: $N/a$.
    * Số các số chia hết cho $b$: $N/b$.
    * Số các số chia hết cho $c$: $N/c$.
* **Bước 2:** Loại bỏ các phần giao đôi (bị tính lặp 2 lần)
    * Trừ đi số các số chia hết cho cả $a$ và $b$ (tức chia hết cho $lcm(a, b)$): $N/lcm(a, b)$.
    * Trừ đi số các số chia hết cho cả $a$ và $c$ (tức chia hết cho $lcm(a, c)$): $N/lcm(a, c)$.
    * Trừ đi số các số chia hết cho cả $b$ và $c$ (tức chia hết cho $lcm(b, c)$): $N/lcm(b, c)$.
* **Bước 3:** Cộng lại phần giao ba (bị trừ đi quá mức)
    * Vì phần chung của cả $a, b, c$ đã bị trừ đi ở bước 2 quá nhiều lần, ta cần cộng lại số các số chia hết cho cả 3 số (tức chia hết cho $lcm(a, b, c)$): $N/lcm(a, b, c)$.

**Công thức** 
<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; background: #1e293b; padding: 25px; border-radius: 12px; margin: 20px 0; border: 1px dashed #334155; font-family: 'Consolas', monospace; color: #d4d4d4; max-width: 100%; overflow-x: auto;">
        <div style="font-weight: bold; margin-bottom: 20px; text-align: center; line-height: 1.6;">
        <span style="color: #4ec9b0;">|A ∪ B ∪ C|</span> = 
        (<span style="color: #569cd6;">|A|</span> + <span style="color: #f59e0b;">|B|</span> + <span style="color: #c586c0;">|C|</span>) - 
        (<span style="color: #fbbf24;">|A ∩ B|</span> + <span style="color: #fbbf24;">|A∩C|</span> + <span style="color: #fbbf24;">|B ∩ C|</span>) + 
        <span style="color: #f87171;">|A ∩ B ∩ C|</span>
    </div>
    <div style="position: relative; width: 340px; height: 320px; margin: 10px 0;">
                <div style="position: absolute; left: 0; top: 0; width: 180px; height: 180px; border-radius: 50%; background-color: rgba(86, 156, 214, 0.5); border: 2px solid #569cd6; display: flex; align-items: flex-start; justify-content: flex-start; padding: 15px; box-sizing: border-box;">
            <b style="color: #fff; font-size: 1.2rem;">A</b>
        </div>
                <div style="position: absolute; right: 0; top: 0; width: 180px; height: 180px; border-radius: 50%; background-color: rgba(245, 158, 11, 0.5); border: 2px solid #f59e0b; display: flex; align-items: flex-start; justify-content: flex-end; padding: 15px; box-sizing: border-box;">
            <b style="color: #fff; font-size: 1.2rem;">B</b>
        </div>
                <div style="position: absolute; left: 80px; bottom: 0; width: 180px; height: 180px; border-radius: 50%; background-color: rgba(197, 134, 192, 0.5); border: 2px solid #c586c0; display: flex; align-items: flex-end; justify-content: center; padding: 15px; box-sizing: border-box;">
            <b style="color: #fff; font-size: 1.2rem;">C</b>
        </div>
        <div style="position: absolute; left: 150px; top: 110px; width: 40px; height: 40px; background-color: rgba(248, 113, 113, 0.9); border-radius: 50%; border: 2px solid #f87171; display: flex; align-items: center; justify-content: center; z-index: 10; box-shadow: 0 0 10px rgba(248, 113, 113, 0.5);">
            <b style="color: #fff; font-size: 0.6rem; text-align: center;">A∩B∩C</b>
        </div>
    </div>
    <p style="color: #94a3b8; font-size: 0.85rem; margin-top: 15px; font-style: italic; max-width: 400px; text-align: center; line-height: 1.4;">
        <i>(Vùng màu đỏ <span style="color: #f87171;">A ∩ B ∩ C</span> bị trừ đi 3 lần khi loại bỏ phần giao đôi, nên cần cộng lại một lần)</i>
    </p>
</div>

```cpp
long long countAorBorC(long long n, long long a, long long b, long long c) {
    // Tính các tập đơn lẻ
    long long single = (n / a) + (n / b) + (n / c);
    
    // Tính các phần giao đôi
    long long double_intersect = (n / lcm(a, b)) + (n / lcm(a, c)) + (n / lcm(b, c));
    
    // Tính phần giao ba
    long long triple_intersect = n / lcm(a, lcm(b, c));
    
    return single - double_intersect + triple_intersect;
}
```
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Ví dụ mẫu: Số không chia hết cho các số nguyên tố</div>

**Đề bài:** Đếm xem trong đoạn $[1, N]$ có bao nhiêu số không chia hết cho bất kỳ số nào trong tập $S = \{2, 3, 5\}$.

**Giải pháp:** 
1. Tính tổng số lượng số chia hết cho 2, 3 hoặc 5 
2. Áp dụng nguyên lý Bao hàm (tất các các số từ 1 đến N) - Loại trừ (tổng số lượng số chia hết cho 2, 3 hoặc 5).

**Công thức** 
<div class="math-formula">$N - (\text{Tổng vừa tính})$ </div>

```cpp
// Ví dụ với N = 100, S = {2, 3, 5}
long long total = 100;
long long divisible = countAorBorC(100, 2, 3, 5);
long long result = total - divisible; // Các số không chia hết cho 2, 3, 5
```
</div>