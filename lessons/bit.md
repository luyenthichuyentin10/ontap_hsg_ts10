## 💻 Kỹ Thuật Xử Lý Bit (Bitwise Operations)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm</div>

**Khái niệm:** Kỹ thuật xử lý bit là việc thao tác trực tiếp trên biểu diễn **nhị phân (các bit 0 và 1)** của các con số. Vì máy tính hoạt động dựa trên hệ nhị phân, các phép toán trên bit được thực thi trực tiếp bởi CPU (ALU) mà không cần qua các bước chuyển đổi phức tạp, do đó tốc độ thực thi của chúng là nhanh nhất trong tất cả các phép toán.

**Tại sao phải học xử lý bit:**
- Tối ưu hóa cực độ thời gian chạy (Time Complexity).
- Tiết kiệm tối đa bộ nhớ (dùng 1 số nguyên 32-bit có thể lưu trạng thái của 32 phần tử thay vì dùng mảng bool 32 phần tử).
- Giải quyết gọn gàng các bài toán tập con (Bitmask), Đồ thị, Cây Fenwick (BIT).
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Bảng các phép toán trên bit (Bitwise Operators)</div>

Trong C++, ta có 6 phép toán thao tác trực tiếp trên bit.(Quy ước: Bit $1$ tương đương với True, Bit $0$ tương đương với False)

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 15%;">Phép toán</th><th style="width: 15%;">Cú pháp C++</th><th style="width: 40%;">Ý nghĩa</th><th style="width: 30%;">Ví dụ (a = 5, b = 3)</th></tr></thead><tbody><tr><td><b>AND</b></td><td style="text-align: center; font-size: 1.2em; font-weight: bold;">&</td><td>Trả về 1 nếu cả hai bit tương ứng đều là 1. Ngược lại trả về 0.</td><td>5 (0101) & 3 (0011) = <b>1</b> (0001)</td></tr><tr style="background-color: #f8fafc;"><td><b>OR</b></td><td style="text-align: center; font-size: 1.2em; font-weight: bold;">|</td><td>Trả về 1 nếu một trong hai bit tương ứng là 1. Ngược lại trả về 0 khi cả hai bit đều là 0.</td><td>5 (0101) | 3 (0011) = <b>7</b> (0111)</td></tr><tr><td><b>XOR</b></td><td style="text-align: center; font-size: 1.2em; font-weight: bold;">^</td><td>Trả về 1 nếu 2 bit tương ứng <b>khác nhau</b>, ngược lại trả về 0.</td><td>5 (0101) ^ 3 (0011) = <b>6</b> (0110)</td></tr><tr style="background-color: #f8fafc;"><td><b>NOT</b></td><td style="text-align: center; font-size: 1.2em; font-weight: bold;">~</td><td>Đảo ngược tất cả các bit (0 thành 1 và 1 thành 0).</td><td>~6 (00000110) = <b>-7</b> (1...1001)</td></tr><tr><td><b>Left Shift</b></td><td style="text-align: center; font-size: 1.2em; font-weight: bold;">&lt;&lt;</td><td>Dịch chuyển các bit sang trái.Tương đương phép nhân: $a \times 2^k$</td><td>5 (0101) &lt;&lt; 2 = <b>20</b> (10100)<i>(tương đương $5 \times 2^2$)</i></td></tr><tr style="background-color: #f8fafc;"><td><b>Right Shift</b></td><td style="text-align: center; font-size: 1.2em; font-weight: bold;">&gt;&gt;</td><td>Dịch chuyển các bit sang phải.Tương đương phép chia: $\lfloor a / 2^k \rfloor$</td><td>5 (0101) &gt;&gt; 2 = <b>1</b> (0001)<i>(tương đương $5 / 2^2$)</i></td></tr></tbody></table></div></div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Các kỹ thuật xử lý bit cơ bản</div>

<div class="important-note">
<b>Quy ước đánh số thứ tự:</b> Các bit của một số được đánh số thứ tự từ <b>phải sang trái</b>, bắt đầu từ vị trí $0$ (Bit có trọng số nhỏ nhất - LSB).

Ví dụ số 13 (Nhị phân `1101`) thì: Bit thứ 0 là 1, bit thứ 1 là 0, bit thứ 2 là 1, bit thứ 3 là 1.
</div>

### 1. Kiểm tra bit thứ $K$ của số $N$ (Check bit)
- **Phân tích:** Ta đẩy bit thứ $K$ của $N$ về sát lề phải (bằng phép dịch phải >> K). Sau đó AND với $1$. Nếu kết quả là $1$ thì bit đó bật, nếu là $0$ thì bit đó tắt.
- **Code:** `bool check = (N >> K) & 1;`
- **Ví dụ:** Kiểm tra bit $K = 1$ của $N = 10$ (1010).
    - 10 >> 1 biến 1010 thành 0101 (số $5$).
    - 0101 & 0001 = 0001 (số $1$). $\rightarrow$ Kết luận: Bit thứ 1 đang Bật (1).

### 2. Bật bit thứ $K$ của số $N$ (Set bit)
- **Phân tích:** Ta tạo ra một mặt nạ (mask) chỉ có bit thứ $K$ là $1$ bằng cách lấy số $1$ dịch trái $K$ lần (1 << K). Sau đó dùng phép OR với $N$.
- **Code:** `N = N | (1 << K);`
- **Ví dụ:** Bật bit $K = 2$ của $N = 10$ (1010). (Hiện tại bit 2 đang là 0).
    - Mặt nạ: 1 << 2 tạo ra 0100 (số $4$).
    - 10 | 4 tương đương 1010 | 0100 = 1110.
    - Số 1110 trong hệ thập phân là $14$. $\rightarrow$ Kết quả: $N$ biến thành $14$.

### 3. Tắt bit thứ $K$ của số $N$ (Clear bit)
- **Phân tích:** Tạo mask có duy nhất bit thứ $K$ là $0$, còn lại toàn $1$ bằng cách ~(1 << K). Dùng phép AND với $N$ sẽ giữ nguyên các bit khác và dập bit thứ $K$ về $0$.
- **Code:** `N = N & ~(1 << K);`
- **Ví dụ:** Tắt bit $K = 1$ của $N = 10$ (1010). (Hiện tại bit 1 đang là 1).
    - Mặt nạ ban đầu: 1 << 1 tạo ra 0010.
    - Đảo ngược mặt nạ: ~0010 tạo ra 1101.10 & mask tương đương 1010 & 1101 = 1000.
    - Số 1000 trong hệ thập phân là $8$. $\rightarrow$ Kết quả: $N$ biến thành $8$.

### 4. Lật bit thứ $K$ của số $N$ (Toggle bit)
- **Phân tích:** Phép XOR với $1$ sẽ làm đảo ngược bit (0 thành 1, 1 thành 0). Tạo mask (1 << K) và XOR với $N$.
- **Code:** `N = N ^ (1 << K);`
- **Ví dụ 1** (Lật 0 thành 1): 
    - Lật bit $K = 2$ của $N = 10$ (1010).
    - Mặt nạ: 1 << 2 tạo ra 0100.10 ^ 4 tương đương 1010 ^ 0100 = 1110 (số $14$).
- **Ví dụ 2** (Lật 1 thành 0): 
    - Lật bit $K = 1$ của $N = 10$ (1010).
    - Mặt nạ: 1 << 1 tạo ra 0010.10 ^ 2 tương đương 1010 ^ 0010 = 1000 (số $8$).

### 5. Xóa bit 1 cuối cùng bên phải (Clear lowest set bit)
- **Phân tích:** Số $(N - 1)$ có tính chất là lật toàn bộ các bit từ bit $1$ ngoài cùng bên phải của $N$ trở đi. Khi lấy N & (N - 1), bit $1$ cuối cùng đó bị triệt tiêu thành $0$.
- **Code:** N = N & (N - 1);
- **Ví dụ:** $N = 12$ (1100). $N - 1 = 11$ (1011). 1100 & 1011 = 1000 (đã bay mất bit 1 ở vị trí số 2).

### 6. Lấy bit 1 cuối cùng bên phải (Get lowest set bit / LSB)
- **Phân tích:** Trong hệ nhị phân bù 2 (cách máy tính lưu số âm), số $-N$ được tính bằng cách đảo tất cả các bit của $N$ rồi cộng thêm $1$ (-N = ~N + 1). Tính chất kỳ diệu là N & (-N) sẽ triệt tiêu mọi bit, chỉ giữ lại duy nhất bit $1$ ngoài cùng bên phải.
- **Code:** `int lsb = N & (-N);`
- **Ví dụ:** $N = 10$ (0000...1010). $-N = -10$ (1111...0110). N & (-N) = ...1010 & ...0110 = 0000...0010 (Tức là $2$).
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Thuật toán kinh điển áp dụng xử lý bit</div>

### A. Kiểm tra tính chẵn lẻ cực nhanh
- **Phân tích:** Số lẻ luôn có bit cuối cùng (bit 0) bằng $1$. Số chẵn có bit cuối cùng bằng $0$. Thay vì dùng phép chia dư N % 2 (tốn nhiều chu kỳ CPU), ta chỉ cần AND với 1.
- **Code mẫu C++:** 
```cpp
bool kiemTraLe(int N) {
    return (N & 1) == 1; // Nhanh hơn rất nhiều so với N % 2 != 0
}
```

### B. Hàm đếm số lượng bit 1 (Thuật toán Brian Kernighan)
- **Phân tích:** Dựa vào thủ thuật "Xóa bit 1 cuối cùng" N & (N - 1) đã học ở trên. Thay vì phải dịch bit 32 lần để đếm, ta chỉ việc lặp thao tác xóa bit 1 cuối cùng cho đến khi $N = 0$. Số lần lặp chính là số lượng bit 1. (Ví dụ số có 2 bit 1 thì chỉ lặp đúng 2 vòng).
- **Code mẫu C++:**
```cpp
int demSoBit1(int N) {
    int dem = 0;
    while (N > 0) {
        N = N & (N - 1); // Cắt bỏ dần bit 1 cuối cùng
        dem++;
    }
    return dem;
}
// Lưu ý: C++ có hàm tích hợp sẵn là __builtin_popcount(N) làm việc này.
```

### C. Kiểm tra số có phải là lũy thừa của 2 hay không
- **Phân tích:** Một số là lũy thừa của 2 (như $2, 4, 8, 16 \dots$) thì trong hệ nhị phân của nó có duy nhất một bit 1, còn lại toàn bit 0 (ví dụ $8$ là 1000). Nếu ta áp dụng phép xóa bit 1 cuối cùng N & (N - 1), số đó sẽ lập tức biến thành $0$.
- **Code mẫu C++:**
```cpp
bool isPowerOfTwo(long long N) {
    // Phải đảm bảo N > 0 vì số 0 không phải lũy thừa của 2
    return (N > 0) && ((N & (N - 1)) == 0); 
}
```
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">5. Các ứng dụng mở rộng & Lưu ý (CP Tips)</div>

Kỹ thuật bit không chỉ dừng ở tính toán cơ bản, mà nó là linh hồn của rất nhiều cấu trúc dữ liệu và giải thuật nâng cao:

- **Duyệt tập con (Bitmask):** Như đã học ở chuyên đề Vét cạn, dùng số nguyên từ $0$ đến $2^N-1$ để sinh ra mọi tập con. 1 là được chọn, 0 là không chọn.
- **Cấu trúc dữ liệu Cây Fenwick (Binary Indexed Tree):** 
    - Đây là cấu trúc dùng để tính Tổng cộng dồn (Prefix Sum) và Cập nhật điểm trong thời gian $O(\log N)$.
    - Linh hồn của nó chính là thao tác nhảy bước idx += (idx & -idx) (Cộng thêm bit 1 cuối cùng) và idx -= (idx & -idx) (Trừ đi bit 1 cuối cùng).
- **Phép XOR ma thuật:**
    - Tính chất 1: A ^ A = 0 (2 số giống nhau XOR sẽ triệt tiêu).
    - Tính chất 2: A ^ 0 = A.
    - Bài toán kinh điển: Cho mảng có $2N+1$ phần tử, trong đó mọi số đều xuất hiện 2 lần, chỉ có 1 số xuất hiện 1 lần. Tìm số đó?
    - Giải: Chỉ cần dùng vòng lặp XOR tất cả các phần tử lại với nhau. Các số giống nhau tự triệt tiêu thành 0, số còn sót lại cuối cùng chính là đáp án. Tốn $O(N)$ thời gian và $O(1)$ bộ nhớ!
- **Lỗi tràn số (Overflow):** Khi muốn lấy bit số $1$ dịch trái lớn hơn $30$ vị trí, nếu viết 1 << 35, máy sẽ báo lỗi hoặc ra số sai vì số 1 mặc định là kiểu int (chỉ có 32 bit). Học sinh bắt buộc phải ép kiểu thành long long bằng cách viết 1LL << 35.
</div>