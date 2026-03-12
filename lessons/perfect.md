## 🌟 Số Hoàn Hảo (Perfect Number)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Định nghĩa</div>

Số hoàn hảo là số nguyên dương mà tổng của tất cả các ước số thực sự của nó (không kể chính nó) bằng chính số đó.
* **Ví dụ:** $6$ là số hoàn hảo vì các ước của nó là $1, 2, 3$ và $1 + 2 + 3 = 6$.
* **Ví dụ khác:** $28$ ($1+2+4+7+14 = 28$), $496$, $8128$.
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Tìm số hoàn hảo bằng Vét cạn</div>

Sử dụng hàm tính tổng ước đã học ở bài Ước và Bội để kiểm tra từng số.
```cpp
bool isPerfectBrute(long long n) {
    if (n < 6) return false;
    long long sum = 1; // 1 luôn là ước
    for (long long i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            sum += i;
            if (i * i != n) sum += n / i;
        }
    }
    return sum == n;
}
```
**Nhận xét:** Cách này chỉ chạy được với $n \le 10^{12}$ do độ phức tạp $O(\sqrt{n})$.
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Định lý Euclid - Euler (Tối ưu nhất)</div>

**🔹 Số nguyên tố Mersenne**
Số nguyên tố Mersenne là số nguyên tố có dạng $M_p = 2^p - 1$, trong đó $p$ cũng phải là một số nguyên tố.

**🔹 Nội dung định lý**
Một số chẵn là số hoàn hảo khi và chỉ khi nó có dạng:
<div class="math-formula">
    $$n = 2^{p-1} \times (2^p - 1)$$
<div class="formula-notes">

- **$p$:** phải là số nguyên tố
- **$2^p - 1$:** cũng phải là số nguyên tố (Số nguyên tố Mersenne)
</div>
</div>

**🔹 Các bước thực hiện:**
1. Duyệt các số nguyên tố $p$ nhỏ.
2. Kiểm tra xem $M_p = 2^p - 1$ có phải là số nguyên tố hay không.
3. Nếu thỏa mãn, tính số hoàn hảo tương ứng theo công thức trên.

</div>

<div class="step-card border-purple"><div class="step-badge bg-purple">4. Ví dụ minh họa và Code mẫu</div>
<div style="overflow-x: auto; margin: 20px 0;">
    <table style="width: 100%; border-collapse: collapse; background: #1e293b; color: #d4d4d4; font-family: 'Consolas', monospace; border: 1px solid #334155; border-radius: 8px; overflow: hidden;">
        <thead>
            <tr style="background: #334155; color: #fbbf24; text-align: center;">
                <th style="padding: 12px; border: 1px solid #475569;">Số nguyên tố $p$</th>
                <th style="padding: 12px; border: 1px solid #475569;">Số Mersenne $M_p = 2^p - 1$</th>
                <th style="padding: 12px; border: 1px solid #475569;">Kiểm tra SNT</th>
                <th style="padding: 12px; border: 1px solid #475569;">Số hoàn hảo $2^{p-1} \times M_p$</th>
            </tr>
        </thead>
        <tbody>
            <tr style="text-align: center;">
                <td style="padding: 12px; border: 1px solid #475569; color: #569cd6;">2</td>
                <td style="padding: 12px; border: 1px solid #475569;">$2^2 - 1 = 3$</td>
                <td style="padding: 12px; border: 1px solid #475569; color: #4ec9b0;">Có</td>
                <td style="padding: 12px; border: 1px solid #475569; font-weight: bold;">$2^1 \times 3 = \mathbf{6}$</td>
            </tr>
            <tr style="text-align: center; background: rgba(255, 255, 255, 0.03);">
                <td style="padding: 12px; border: 1px solid #475569; color: #569cd6;">3</td>
                <td style="padding: 12px; border: 1px solid #475569;">$2^3 - 1 = 7$</td>
                <td style="padding: 12px; border: 1px solid #475569; color: #4ec9b0;">Có</td>
                <td style="padding: 12px; border: 1px solid #475569; font-weight: bold;">$2^2 \times 7 = \mathbf{28}$</td>
            </tr>
            <tr style="text-align: center;">
                <td style="padding: 12px; border: 1px solid #475569; color: #569cd6;">5</td>
                <td style="padding: 12px; border: 1px solid #475569;">$2^5 - 1 = 31$</td>
                <td style="padding: 12px; border: 1px solid #475569; color: #4ec9b0;">Có</td>
                <td style="padding: 12px; border: 1px solid #475569; font-weight: bold;">$2^4 \times 31 = \mathbf{496}$</td>
            </tr>
            <tr style="text-align: center; background: rgba(255, 255, 255, 0.03);">
                <td style="padding: 12px; border: 1px solid #475569; color: #569cd6;">7</td>
                <td style="padding: 12px; border: 1px solid #475569;">$2^7 - 1 = 127$</td>
                <td style="padding: 12px; border: 1px solid #475569; color: #4ec9b0;">Có</td>
                <td style="padding: 12px; border: 1px solid #475569; font-weight: bold;">$2^6 \times 127 = \mathbf{8128}$</td>
            </tr>
        </tbody>
    </table>
</div>

```cpp
// Hàm kiểm tra số hoàn hảo cực nhanh dựa trên định lý Euclid-Euler
bool isPerfectEuclid(long long n) {
    long long p[] = {2, 3, 5, 7, 13, 17, 19, 31}; // Các số p tạo ra SNT Mersenne
    for (long long x : p) {
        long long mersenne = pow(2, x) - 1;
        long long perfect = pow(2, x - 1) * mersenne;
        if (n == perfect) return true;
    }
    return false;
}
```
</div>

<div class="important-note">
💡 <b>Ghi chú quan trọng:</b>

* Cho đến nay, người ta vẫn chưa tìm thấy bất kỳ số hoàn hảo lẻ nào. Tất cả các số hoàn hảo đã biết đều là số chẵn và tuân theo định lý Euclid-Euler.
* Trong lập trình thi đấu, vì số lượng số hoàn hảo trong phạm vi `long long` rất ít (chỉ khoảng 8-10 số), các em dùng phương pháp **khởi tạo trước** (tạo mảng hằng số) để kiểm tra trong $O(1)$.
</div>