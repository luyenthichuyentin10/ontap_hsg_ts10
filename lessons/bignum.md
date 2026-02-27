## 🐘 Số Nguyên Lớn (Big Number)

<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Khái niệm Số nguyên lớn</div>

Trong **C++**, kiểu dữ liệu lớn nhất là `unsigned long long` cũng chỉ lưu trữ được tối đa khoảng 20 chữ số ($1.8 \times 10^{19}$). Với các bài toán yêu cầu xử lý số có hàng trăm, hàng nghìn chữ số, ta phải biểu diễn chúng dưới dạng **Xâu ký tự (String)**.

```cpp
// Ví dụ biểu diễn số lớn
string bigNum = "123456789012345678901234567890";
```
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Kỹ thuật So sánh hai số nguyên lớn</div>

**Phân tích:** 
* Nếu độ dài khác nhau: Số nào dài hơn thì số đó lớn hơn.
* Nếu độ dài bằng nhau: So sánh theo thứ tự từ điển từ trái qua phải.
```cpp
// Trả về: 1 (a > b), -1 (a < b), 0 (a == b)
int compare(string a, string b) {
    if (a.length() > b.length()) return 1;
    if (a.length() < b.length()) return -1;
    if (a > b) return 1;
    if (a < b) return -1;
    return 0;
}
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Phép Cộng và Trừ hai số nguyên lớn</div>

**Phân tích:** Thực hiện đặt tính rồi tính như tiểu học, tính từ cuối xâu lên đầu xâu.
```cpp
// Phép Cộng: a + b
string add(string a, string b) {
    while (a.size() < b.size()) a = "0" + a;
    while (b.size() < a.size()) b = "0" + b;
    string res = "";
    int carry = 0;
    for (int i = a.size() - 1; i >= 0; i--) {
        int sum = (a[i] - '0') + (b[i] - '0') + carry;
        res = (char)(sum % 10 + '0') + res;
        carry = sum / 10;
    }
    if (carry) res = "1" + res;
    return res;
}

// Phép Trừ: a - b (Giả định a >= b)
string sub(string a, string b) {
    while (b.size() < a.size()) b = "0" + b;
    string res = "";
    int borrow = 0;
    for (int i = a.size() - 1; i >= 0; i--) {
        int s = (a[i] - '0') - (b[i] - '0') - borrow;
        if (s < 0) { s += 10; borrow = 1; }
        else borrow = 0;
        res = (char)(s + '0') + res;
    }
    while (res.size() > 1 && res[0] == '0') res.erase(0, 1);
    return res;
}
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">4. Nhân/Chia số lớn với số nhỏ (long long)</div>

**Phân tích:** Nhân từng chữ số của số lớn với số nhỏ rồi xử lý biến nhớ. Với phép chia, lấy từng cụm chữ số chia lấy dư.
```cpp
// Nhân số lớn với long long k
string mulSmall(string a, long long k) {
    if (k == 0 || a == "0") return "0";
    string res = "";
    long long carry = 0;
    for (int i = a.size() - 1; i >= 0; i--) {
        long long val = (a[i] - '0') * k + carry;
        res = (char)(val % 10 + '0') + res;
        carry = val / 10;
    }
    while (carry) {
        res = (char)(carry % 10 + '0') + res;
        carry /= 10;
    }
    return res;
}

// Chia số lớn cho long long k (k != 0)
string divSmall(string a, long long k) {
    if (a == "0") return "0";
    string res = "";
    long long remainder = 0;
    for (char c : a) {
        remainder = remainder * 10 + (c - '0');
        res += (char)(remainder / k + '0');
        remainder %= k;
    }
    while (res.size() > 1 && res[0] == '0') res.erase(0, 1);
    return res;
}
```
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Nhân/Chia hai số nguyên lớn</div>

**Phân tích:** 
* **Phép nhân:** nhân từng chữ số của xâu b với xâu a rồi cộng dồn kết quả (kèm dịch chuyển hàng).
```cpp
// Nhân hai số lớn (O(N*M))
string multiply(string a, string b) {
    if (a == "0" || b == "0") return "0";
    string res = "0";
    for (int i = b.size() - 1; i >= 0; i--) {
        string temp = mulSmall(a, b[i] - '0');
        for (int j = 0; j < (b.size() - 1 - i); j++) temp += "0";
        res = add(res, temp);
    }
    return res;
}
```

* **Phép chia:** dể chia số lớn $A$ cho số lớn $B$, ta thực hiện thuật toán chia ròng (long division) như sau:
1. Lấy từng chữ số của $A$ hạ xuống để tạo thành một số dư tạm thời (`remainder`).
2. So sánh `remainder` với số chia $B$.
3. Nếu `remainder >= B`, ta tìm xem $B$ nhân với mấy (từ 1 đến 9) thì được giá trị lớn nhất nhưng vẫn $\le$ `remainder`. Trong lập trình thi đấu, cách đơn giản nhất là dùng một vòng lặp trừ liên tiếp.
4. Ghi nhận số lần trừ được vào thương (`quotient`) và cập nhật lại `remainder`.
```cpp
// Hàm chia 2 số lớn: Trả về thương (quotient)
string divide(string a, string b) {
    if (b == "0") return "ERROR"; // Chia cho 0
    if (compare(a, b) < 0) return "0";
    string quotient = "";
    string remainder = "";
    for (int i = 0; i < a.size(); i++) {
        remainder += a[i];
        // Xóa các số 0 vô nghĩa ở đầu remainder tạm thời
        while (remainder.size() > 1 && remainder[0] == '0') 
            remainder.erase(0, 1);
        int count = 0;
        // Chừng nào số dư tạm thời còn lớn hơn hoặc bằng số chia b
        while (compare(remainder, b) >= 0) {
            remainder = sub(remainder, b);
            count++;
        }
        quotient += (char)(count + '0');
    }
    // Xóa số 0 ở đầu thương kết quả
    while (quotient.size() > 1 && quotient[0] == '0') 
        quotient.erase(0, 1);
    return quotient;
}

// Nếu muốn lấy dư (modulo), chỉ cần sửa hàm trên để trả về biến remainder
string modulo(string a, string b) {
    // ... nội dung giống hệt hàm divide ...
    return remainder; 
}
```
<div class="important-note">
    <b>💡 Mẹo tối ưu:</b> Trong các bài toán yêu cầu tốc độ cực cao: <br> 
    - Thay vì dùng vòng lặp while để trừ liên tiếp (có thể chạy tới 9 lần), ta có thể dùng <b>tìm kiếm nhị phân</b> để tìm chữ số thương trong khoảng [0..9] nhằm giảm số phép tính.<br>
    - thay vì cộng xâu theo kiểu <b>res = char + res</b>, ta nên dùng <b>res.push_back()</b> rồi dùng hàm <b>reverse()</b> để đảo ngược xâu kết quả sau cùng nhằm đạt độ phức tạp $O(1)$ cho mỗi thao tác thêm chữ số.
</div>
</div>