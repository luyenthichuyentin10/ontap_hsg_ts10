## 🔢 Hệ Số (Number Systems)
<br>
<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Khái niệm hệ đếm</div>

**Hệ đếm** là một tập hợp các ký hiệu và quy tắc để biểu diễn các giá trị số. Trong lập trình, chúng ta thường làm việc với 4 hệ cơ bản:
* **Hệ thập phân (Decimal - Base 10):** Dùng các chữ số từ `0` đến `9`.
* **Hệ nhị phân (Binary - Base 2):** Chỉ dùng `0` và `1`. Đây là ngôn ngữ của máy tính.
* **Hệ bát phân (Octal - Base 8):** Dùng từ `0` đến `7`.
* **Hệ thập lục phân (Hexadecimal - Base 16):** Dùng từ `0-9 và A-F` (A=10, B=11, C=12, D=13, E=14, F=15).

**Bảng 16 số đầu tiên của 4 hệ đếm**

<table class="garden-table" style="table-layout: fixed; width: 100%; border-collapse: collapse; background: #1e293b; color: #d4d4d4; font-family: 'Consolas', monospace;">
            <colgroup>
                <col style="width: 25%;">
                <col style="width: 25%;">
                <col style="width: 25%;">
                <col style="width: 25%;">
            </colgroup>
            <thead>
                <tr style="background: #334155; color: #fbbf24; text-align: center; font-weight: bold;">
                    <td style="padding: 10px; border: 1px solid #475569;">Thập phân (10)</td>
                    <td style="padding: 10px; border: 1px solid #475569;">Nhị phân (2)</td>
                    <td style="padding: 10px; border: 1px solid #475569;">Bát phân (8)</td>
                    <td style="padding: 10px; border: 1px solid #475569;">Thập lục (16)</td>
                </tr>
            </thead>
            <tbody>
                <tr style="text-align: center;"><td>0</td><td>0000</td><td>0</td><td>0</td></tr>
                <tr style="text-align: center; background: #0f172a;"><td>1</td><td>0001</td><td>1</td><td>1</td></tr>
                <tr style="text-align: center;"><td>2</td><td>0010</td><td>2</td><td>2</td></tr>
                <tr style="text-align: center; background: #0f172a;"><td>3</td><td>0011</td><td>3</td><td>3</td></tr>
                <tr style="text-align: center;"><td>4</td><td>0100</td><td>4</td><td>4</td></tr>
                <tr style="text-align: center; background: #0f172a;"><td>5</td><td>0101</td><td>5</td><td>5</td></tr>
                <tr style="text-align: center;"><td>6</td><td>0110</td><td>6</td><td>6</td></tr>
                <tr style="text-align: center; background: #0f172a;"><td>7</td><td>0111</td><td>7</td><td>7</td></tr>
                <tr style="text-align: center;"><td>8</td><td>1000</td><td>10</td><td>8</td></tr>
                <tr style="text-align: center; background: #0f172a;"><td>9</td><td>1001</td><td>11</td><td>9</td></tr>
                <tr style="text-align: center; color: #4ec9b0;"><td>10</td><td>1010</td><td>12</td><td>A</td></tr>
                <tr style="text-align: center; background: #0f172a; color: #4ec9b0;"><td>11</td><td>1011</td><td>13</td><td>B</td></tr>
                <tr style="text-align: center; color: #4ec9b0;"><td>12</td><td>1100</td><td>14</td><td>C</td></tr>
                <tr style="text-align: center; background: #0f172a; color: #4ec9b0;"><td>13</td><td>1101</td><td>15</td><td>D</td></tr>
                <tr style="text-align: center; color: #4ec9b0;"><td>14</td><td>1110</td><td>16</td><td>E</td></tr>
                <tr style="text-align: center; background: #0f172a; color: #4ec9b0;"><td>15</td><td>1111</td><td>17</td><td>F</td></tr>
            </tbody>
        </table>
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Đổi từ hệ cơ số $B$ sang hệ Thập phân</div>
    
**Phân tích:** Một số $N$ ở hệ cơ số $B$ có các chữ số $d_n d_{n-1} ... d_1 d_0$ được tính theo công thức tổng các lũy thừa.

**Công thức:** 
<div class="math-formula"> $N_B = d_n \times B^n + d_{n-1} \times B^{n-1} + ... + d_1 \times B^1 + d_0 \times B^0$</div>

```cpp
long long toDecimal(string s, int b) {
    long long res = 0;
    long long power = 1;
    for (int i = s.length() - 1; i >= 0; i--) {
        int val;
        if (s[i] >= '0' && s[i] <= '9') val = s[i] - '0';
        else val = s[i] - 'A' + 10;
        res += val * power;
        power *= b;
    }
    return res;
}
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Đổi từ hệ thập phân sang hệ cơ số $B$</div>

**Phân tích:** Ta chia liên tiếp số thập phân $N$ cho cơ số $B$ và lấy phần dư theo thứ tự ngược lại.

**Quy tắc:** Chia cho $B$, lấy dư, viết ngược danh sách các số dư thu được.
```cpp
string fromDecimal(long long n, int b) {
    string res = "";
    string digits = "0123456789ABCDEF";
    while (n > 0) {
        res = digits[n % b] + res;
        n /= b;
    }
    return res.empty() ? "0" : res;
}
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">4. Quy tắc đổi nhanh 2 ↔ 8 và 2 ↔ 16</div>
    
* Thay vì đổi trung gian qua hệ 10, ta sử dụng quy tắc **gom nhóm bit** vì $8 = 2^3$ và $16 = 2^4$.
    * Hệ 2 ↔ Hệ 8 (Nhóm 3): `101 110₂ = 56₈`
    * Hệ 2 ↔ Hệ 16 (Nhóm 4): 1010 1111₂ = AF₁₆

* **Hàm chuyển từ Hệ 2 sang Hệ 8/16:**
    * **Quy tắc:** Thêm các số `0` vào đầu xâu nhị phân để đủ nhóm **3** hoặc **4** ký tự, sau đó tra bảng để đổi từng nhóm.

```cpp
string binaryToOctal(string bin) {
    // 1. Bổ sung số 0 vào đầu để độ dài chia hết cho 3
    while (bin.length() % 3 != 0) bin = "0" + bin;
    string res = "";
    string OctalDigits = "01234567";
    // 2. Duyệt từng nhóm 3 ký tự
    for (int i = 0; i < bin.length(); i += 3) {
        string group = bin.substr(i, 3);
        // Đổi nhóm 3 bit nhị phân sang số thập phân tạm thời
        int val = 0;
        for (int j = 0; j < 3; j++) {
            val = val * 2 + (group[j] - '0');
        }
        res += OctalDigits[val];
    }
    return res;
}

string binaryToHex(string bin) {
    // 1. Bổ sung số 0 vào đầu để độ dài chia hết cho 4
    while (bin.length() % 4 != 0) bin = "0" + bin;
    string res = "";
    string hexDigits = "0123456789ABCDEF";
    // 2. Duyệt từng nhóm 4 ký tự
    for (int i = 0; i < bin.length(); i += 4) {
        string group = bin.substr(i, 4);
        // Đổi nhóm 4 bit nhị phân sang số thập phân tạm thời
        int val = 0;
        for (int j = 0; j < 4; j++) {
            val = val * 2 + (group[j] - '0');
        }
        res += hexDigits[val];
    }
    return res;
}
```
* **Hàm chuyển từ Hệ 16/8 sang Hệ 2:**
    * **Quy tắc:** Mỗi ký tự hệ **16** hoặc **8** sẽ được bung ra thành đúng **4** hoặc **3** ký tự nhị phân.
```cpp
string OctalToBinary(string otc) {
    string res = "";
    for (char c : otc) {
        int val = c - '0';
        // Chuyển val sang 4 bit nhị phân
        string b = "";
        for (int i = 0; i < 4; i++) {
            b = (char)((val % 2) + '0') + b;
            val /= 2;
        }
        res += b;
    }
    // Xóa các số 0 dư thừa ở đầu (tùy chọn)
    while (res.length() > 1 && res[0] == '0') res.erase(0, 1);
    return res;
}

string hexToBinary(string hex) {
    string res = "";
    for (char c : hex) {
        int val;
        if (c >= '0' && c <= '9') val = c - '0';
        else val = c - 'A' + 10;
        // Chuyển val sang 4 bit nhị phân
        string b = "";
        for (int i = 0; i < 4; i++) {
            b = (char)((val % 2) + '0') + b;
            val /= 2;
        }
        res += b;
    }
    // Xóa các số 0 dư thừa ở đầu (tùy chọn)
    while (res.length() > 1 && res[0] == '0') res.erase(0, 1);
    return res;
}
```
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Tính toán Nhị phân trên Xâu (String)</div>

<table class="garden-table" style="table-layout: fixed; width: 100%; border-collapse: collapse; background: #1e293b; color: #d4d4d4; font-family: 'Consolas', monospace;">
        <colgroup>
            <col style="width: 25%;">
            <col style="width: 25%;">
            <col style="width: 25%;">
            <col style="width: 25%;">
        </colgroup>
        <thead>
            <tr style="background: #334155; color: #fbbf24; text-align: center; font-weight: bold;">
                <td style="padding: 10px; border: 1px solid #475569;">CỘNG (+)</td>
                <td style="padding: 10px; border: 1px solid #475569;">TRỪ (-)</td>
                <td style="padding: 10px; border: 1px solid #475569;">NHÂN (×)</td>
                <td style="padding: 10px; border: 1px solid #475569;">CHIA (÷)</td>
            </tr>
        </thead>
        <tbody>
            <tr style="text-align: center;">
                <td style="padding: 12px; border: 1px solid #334155;">0 + 0 = 0</td>
                <td style="padding: 12px; border: 1px solid #334155;">0 - 0 = 0</td>
                <td style="padding: 12px; border: 1px solid #334155;">0 × 0 = 0</td>
                <td style="padding: 12px; border: 1px solid #334155;">0 ÷ 1 = 0</td>
            </tr>
            <tr style="text-align: center;">
                <td style="padding: 12px; border: 1px solid #334155;">1 + 0 = 1</td>
                <td style="padding: 12px; border: 1px solid #334155;">1 - 0 = 1</td>
                <td style="padding: 12px; border: 1px solid #334155;">0 × 1 = 0</td>
                <td style="padding: 12px; border: 1px solid #334155;">1 ÷ 1 = 1</td>
            </tr>
            <tr style="text-align: center;">
                <td style="padding: 12px; border: 1px solid #334155;">0 + 1 = 1</td>
                <td style="padding: 12px; border: 1px solid #334155;">1 - 1 = 0</td>
                <td style="padding: 12px; border: 1px solid #334155;">1 × 0 = 0</td>
                <td style="padding: 12px; border: 1px solid #334155; color: #f87171; font-style: italic;">x ÷ 0 (Lỗi)</td>
            </tr>
            <tr style="text-align: center;">
                <td style="padding: 12px; border: 1px solid #334155; background: #0f172a;">
                    <span style="color: #4ec9b0;">1 + 1 = 0</span><br>
                    <small style="color: #ce9178;">(nhớ 1)</small>
                </td>
                <td style="padding: 12px; border: 1px solid #334155; background: #0f172a;">
                    <span style="color: #4ec9b0;">0 - 1 = 1</span><br>
                    <small style="color: #ce9178;">(mượn 1)</small>
                </td>
                <td style="padding: 12px; border: 1px solid #334155;">1 × 1 = 1</td>
                <td style="padding: 12px; border: 1px solid #334155;">-</td>
            </tr>
        </tbody>
    </table>
    
<p style="font-size: 0.85rem; color: #94a3b8; margin-top: 10px; text-align: center;">
        <i>* Lưu ý: Trong phép trừ, 0 - 1 thực chất là 10₂ - 1 = 1. Trong phép chia, không chia được cho 0.</i>
    </p>

Khi các số nhị phân quá lớn (vượt quá 64 bit), chúng ta phải xử lý chúng như những xâu ký tự. Dưới đây là bộ 4 hàm tính toán cơ bản:
### A. Phép Cộng (Addition):
**Quy tắc:** Cộng từ phải sang trái, có biến `remember` để nhớ sang hàng tiếp theo.

```cpp
string addBinary(string a, string b) {
    string res = "";
    int i = a.size() - 1, j = b.size() - 1, carry = 0;
    while (i >= 0 || j >= 0 || carry) {
        int sum = carry;
        if (i >= 0) sum += a[i--] - '0';
        if (j >= 0) sum += b[j--] - '0';
        res = (char)(sum % 2 + '0') + res;
        carry = sum / 2;
    }
    return res;
}
```
### B. Phép Trừ (Subtraction):
**Quy tắc:** Nếu $0 - 1$, ta mượn $1$ ở hàng trước (trở thành $2 - 1 = 1$). Lưu ý: Hàm này giả định $a \ge b$.
```cpp
string subBinary(string a, string b) {
    while (b.size() < a.size()) b = "0" + b;
    string res = "";
    int borrow = 0;
    for (int i = a.size() - 1; i >= 0; i--) {
        int sub = (a[i] - '0') - (b[i] - '0') - borrow;
        if (sub < 0) {
            sub += 2;
            borrow = 1;
        } else borrow = 0;
        res = (char)(sub + '0') + res;
    }
    while (res.size() > 1 && res[0] == '0') res.erase(0, 1);
    return res;
}
```
### C. Phép Nhân (Multiplication):
**Quy tắc:** Giống nhân số thập phân, nếu gặp chữ số `1` thì cộng dồn xâu đã được dịch trái.
```cpp
string mulBinary(string a, string b) {
    string res = "0";
    for (int i = b.size() - 1; i >= 0; i--) {
        if (b[i] == '1') {
            string temp = a;
            // Dịch trái tương ứng với vị trí hàng
            for (int j = 0; j < (b.size() - 1 - i); j++) temp += '0';
            res = addBinary(res, temp);
        }
    }
    return res;
}
```
### D. Phép Chia (Division):
**Quy tắc:** Lấy từng phần của số bị chia so sánh với số chia, nếu lớn hơn thì trừ đi và ghi nhận bit `1`.
```cpp
string divBinary(string a, string b) {
    if (b == "0") return "ERROR";
    string quotient = "", remainder = "";
    for (char c : a) {
        remainder += c;
        // Xóa số 0 ở đầu phần dư tạm thời
        while (remainder.size() > 1 && remainder[0] == '0') remainder.erase(0, 1);
        // So sánh phần dư tạm thời với số chia b
        if (remainder.size() < b.size() || (remainder.size() == b.size() && remainder < b)) {
            quotient += '0';
        } else {
            quotient += '1';
            remainder = subBinary(remainder, b);
        }
    }
    while (quotient.size() > 1 && quotient[0] == '0') quotient.erase(0, 1);
    return quotient;
}
```
### 💡 Lưu ý quan trọng:

1.  **Phép Trừ:** Trong lập trình thi đấu, nếu $a < b$, học sinh nên đổi chỗ để tính `subBinary(b, a)` rồi thêm dấu trừ `-` ở phía trước kết quả.
2.  **Phép Chia:** Đây thực chất là thuật toán chia ròng (long division) mà chúng ta học ở tiểu học, nhưng thực hiện trên hệ nhị phân nên mỗi bước chỉ có thể là `0` hoặc `1`, giúp thuật toán đơn giản hơn nhiều so với hệ thập phân.
3.  **Tối ưu:** Các hàm trên sử dụng phép cộng xâu `res = char + res`, thao tác này có độ phức tạp cao ($O(N)$ cho mỗi lần cộng). Nếu làm bài toán yêu cầu tốc độ cực nhanh, hãy dùng `.push_back()` rồi `reverse` xâu lại sau cùng.
</div>

