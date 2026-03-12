## ❗ Giai thừa (Factorial)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm</div>

Giai thừa của một số nguyên dương $n$ là tích của tất cả các số nguyên dương từ $1$ đến $n$.
- **Ký hiệu:** $n! = 1 \times 2 \times 3 \times \dots \times n$.
- **Ví dụ:** $5! = 1 \times 2 \times 3 \times 4 \times 5 = 120$. 
- **Quy ước:** $0! = 1$.
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Bậc của thừa số nguyên tố p trong n!</div>

**Định nghĩa:** Là số mũ cao nhất $x$ sao cho $n!$ chia hết cho $p^x$.

**Ví dụ:** với $n = 10$, $p = 2$
- $10! = 1 * 2 * 3 * 4 * 5 * 6 * 7 * 8 * 9 * 10 = 3628800$
- Phân tích thừa số nguyên tố: $10! = 2^8 * 3^4 * 5^2 * 7$
- Vậy bậc của $2$ trong $n!$ là $8$

**Phương pháp 1: Vét cạn (Duyệt bội số)**

Ta duyệt qua tất cả các số từ $1$ đến $n$, với mỗi số, ta đếm xem nó chứa bao nhiêu thừa số $p$.

Độ phức tạp: $O(n \log_p n)$.
```cpp
int countPrimeFactor(int n, int p) {
    int count = 0;
    for (int i = p; i <= n; i += p) {
        int temp = i;
        while (temp % p == 0) {
            count++;
            temp /= p;
        }
    }
    return count;
}
```
**Phương pháp 2: Công thức Legendre (Tối ưu)**

Thay vì đếm thủ công Legendre nhận ra quy luật
- Số lượng số $p$ trong đoạn $[1, ..., N]$ là $N/p$
- Số lượng số chia hết cho $p^2$ (đóng góp thêm một thừa số nữa) là $N/p^2$
- Tương tự với $p^3, p^4, ...$

=> Nên số lượng thừa số $p$ trong $N!$ bằng tổng các phần nguyên của $N$ chia cho các lũy thừa của $p$.
<div class="math-formula">

$v_p(n!) = \sum_{k=1}^{\infty} \lfloor \frac{n}{p^k} \rfloor = \lfloor \frac{n}{p} \rfloor + \lfloor \frac{n}{p^2} \rfloor + \lfloor \frac{n}{p^3} \rfloor + \dots$
<div class="formula-notes">

**$n$:** Số cần tính giai thừa.

**$p$:** Thừa số nguyên tố cần tìm bậc.

**$\lfloor \rfloor$:** Ký hiệu phần nguyên (phép chia nguyên trong C++).
</div>
</div>

**Ví dụ:** Tìm bậc của $3$ trong $35!$: $\lfloor 35/3 \rfloor + \lfloor 35/9 \rfloor + \lfloor 35/27 \rfloor = 11 + 3 + 1 = 15$.
```cpp
int legendre(int n, int p) {
    int count = 0;
    while (n > 0) {
        n /= p;
        count += n;
    }
    return count;
}
```
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Bậc của hợp số m trong n!</div>

**Vấn đề:** Ta không thể áp dụng trực tiếp công thức Legendre cho hợp số vì các thừa số cấu thành hợp số có thể xuất hiện rải rác.

**Chiến lược:** Nguyên lý mắt xích yếu nhất (Min ratio)
- Để tìm bậc của hợp số $m$, ta phân tích $m$ ra thừa số nguyên tố: $m = p_1^{a_1} \cdot p_2^{a_2} \dots$.
- Bậc của $m$ trong $n!$ sẽ bị giới hạn bởi thừa số nguyên tố nào "khan hiếm" nhất sau khi đã chia cho số mũ của nó.
<div class="math-formula">
$ans = \min \left( \frac{v_{p_1}(N!)}{e_1}, \frac{v_{p_2}(N!)}{e_2}, \dots, \frac{v_{p_k}(N!)}{e_k} \right)$
</div>

**Ví dụ:** Tìm bậc của $12$ trong $10!$.
- Phân tích thừa số nguyên tố: 12 = $2^2 * 3^1$
- Bậc của $2$ trong $10!$ là $8$. Số bộ $2^2$ tạo được là $8/2 = 4$
- Bậc của $3$ trong $10!$ là $4$. Số bộ $3^1$ tạo được là $4/1 = 4$
- Kết quả: `min(4, 4) = 4`
```cpp
int countCompositeFactor(int n, int m) {
    int res = 2e9; // Khởi tạo giá trị vô cùng lớn
    int temp = m;
    for (int i = 2; i * i <= temp; i++) {
        if (temp % i == 0) {
            int count_m = 0;
            while (temp % i == 0) {
                count_m++;
                temp /= i;
            }
            res = min(res, legendre(n, i) / count_m);
        }
    }
    if (temp > 1) res = min(res, legendre(n, temp) / 1);
    return res;
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">4. Đếm số 0 liên tiếp tận cùng của $n!$</div>

- Số lượng chữ số $0$ tận cùng của $n!$ chính là bậc của hợp số $10$ trong $n!$.
- Vì $10 = 2 \times 5$, theo nguyên lý mắt xích yếu nhất, bậc của $5$ trong $n!$ luôn nhỏ hơn hoặc bằng bậc của $2$. 
- Do đó, ta chỉ cần đếm số lượng thừa số $5$.
- Thuật toán: Tính $v_5(n!)$ bằng công thức Legendre.
```cpp
int countTrailingZeros(int n) {
    return legendre(n, 5);
}
```

- Ví dụ minh họa:
<div style="overflow-x: auto; margin: 20px 0;">
<table style="width: 100%; border-collapse: collapse; background: #1e293b; color: #d4d4d4; font-family: 'Consolas', monospace; border: 1px solid #334155; border-radius: 8px; overflow: hidden;">
<thead>
<tr style="background: #334155; color: #fbbf24; text-align: center;">
<th style="padding: 12px; border: 1px solid #475569;">Giai thừa ($n!$)</th>
<th style="padding: 12px; border: 1px solid #475569;">Công thức $v_5(n!)$</th>
<th style="padding: 12px; border: 1px solid #475569;">Số chữ số 0</th>
<th style="padding: 12px; border: 1px solid #475569;">Giá trị minh họa</th>
</tr>
</thead>
<tbody>
<tr style="text-align: center;">
<td style="padding: 12px; border: 1px solid #475569;">$5!$</td>
<td style="padding: 12px; border: 1px solid #475569;">$\lfloor 5/5 \rfloor = 1$</td>
<td style="padding: 12px; border: 1px solid #475569; color: #4ec9b0;">1</td>
<td style="padding: 12px; border: 1px solid #475569;">12<b>0</b></td>
</tr>
<tr style="text-align: center; background: rgba(255, 255, 255, 0.03);">
<td style="padding: 12px; border: 1px solid #475569;">$10!$</td>
<td style="padding: 12px; border: 1px solid #475569;">$\lfloor 10/5 \rfloor = 2$</td>
<td style="padding: 12px; border: 1px solid #475569; color: #4ec9b0;">2</td>
<td style="padding: 12px; border: 1px solid #475569;">36288<b>00</b></td>
</tr>
<tr style="text-align: center;">
<td style="padding: 12px; border: 1px solid #475569;">$25!$</td>
<td style="padding: 12px; border: 1px solid #475569;">$\lfloor 25/5 \rfloor + \lfloor 25/25 \rfloor = 6$</td>
<td style="padding: 12px; border: 1px solid #475569; color: #4ec9b0;">6</td>
<td style="padding: 12px; border: 1px solid #475569;">...<b>000000</b></td>
</tr>
</tbody>
</table>
</div>
</div>

<div class="important-note">
💡 <b>Ghi chú quan trọng:</b>

- Công thức Legendre chỉ áp dụng được cho thừa số nguyên tố. Nếu $p$ không là số nguyên tố, bắt buộc phải phân tích ra thừa số nguyên tố và dùng nguyên lý $\min$.
- Trong các bài toán số lớn, $n!$ tăng cực nhanh. Chúng ta thường không tính trực tiếp giá trị $n!$ mà chỉ làm việc với số mũ của các thừa số nguyên tố cấu thành nó.
</div>