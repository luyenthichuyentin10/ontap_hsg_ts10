## ⚔️ Tư tưởng Lập trình Chia để trị (Divide and Conquer)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

### Khái niệm:
Chia để trị là một tư tưởng thiết kế thuật toán hoạt động dựa trên nguyên tắc: 
- Bẻ gãy một bài toán lớn, phức tạp thành nhiều bài toán con nhỏ hơn (cùng loại). 
- Giải quyết các bài toán con đó, rồi tổng hợp kết quả của chúng lại để có được đáp án cho bài toán ban đầu.
### Ví dụ trực quan: 
Tìm số lớn nhất trong mảng dữ liệu: 8  3  6  2  9  5  4  7

Thay vì duyệt qua từng phần tử từ đầu đến cuối, ta áp dụng tư tưởng chia để trị như sau:
- Chia đôi mảng: [8  3  6  2]   |   [9  5  4  7]
- Tìm Max mỗi nửa (Trị):
    - Nửa trái: Max = 8
    - Nửa phải: Max = 9
- Tổng hợp kết quả: So sánh Max của hai nửa: Lớn nhất giữa 8 và 9 là 9.$\rightarrow$ Kết quả cuối cùng: 9.
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Quy trình 3 bước giải quyết bài toán</div>

### Một bài toán Chia để trị chuẩn mực luôn trải qua 3 giai đoạn:

- **Bước 1: CHIA (Divide):** Chia bài toán ban đầu thành 2 hay nhiều bài toán con nhỏ hơn.
- **Bước 2: TRỊ (Conquer):** Giải quyết các bài toán con. Nếu bài toán con đã đủ nhỏ (chỉ còn 1 hoặc 2 phần tử), ta giải trực tiếp (gọi là Bài toán cơ sở / Base case). Nếu chưa đủ nhỏ, ta lại tiếp tục chia.
- **Bước 3: TỔNG HỢP (Combine):** Kết hợp các kết quả của bài toán con lại để tạo thành kết quả của bài toán lớn.

### Khung mã nguồn chuẩn (Template C++):
```cpp
// Khung thuật toán Chia để trị tổng quát
KieuDuLieu ChiaDeTri(BaiToan P) {
    // 1. Trường hợp cơ sở (Base case)
    if (P du nho) {
        return GiaiQuyetTrucTiep(P);
    }
    
    // 2. CHIA (Divide)
    Chia P thanh cac bai toan con P1, P2, ..., Pk;
    
    // 3. TRỊ (Conquer) bằng cách gọi đệ quy
    KetQua1 = ChiaDeTri(P1);
    KetQua2 = ChiaDeTri(P2);
    ...
    KetQuak = ChiaDeTri(Pk);
    
    // 4. TỔNG HỢP (Combine)
    return TongHop(KetQua1, KetQua2, ..., KetQuak);
}
```
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Vai trò của Đệ quy trong Chia để trị</div>

### Mối liên hệ
- Chia để trị và Đệ quy là một "cặp bài trùng". Vì các bài toán con có tính chất y hệt bài toán ban đầu (chỉ khác là kích thước nhỏ hơn), nên ta dùng Hàm đệ quy (gọi lại chính mình) để giải quyết chúng một cách cực kỳ tự nhiên và mã nguồn cũng vô cùng ngắn gọn. 
- Đệ quy chính là công cụ để thực hiện bước TRỊ.

### Ví dụ minh họa
**Bài toán 1:** Tìm số lớn nhất trong mảng
- **Phân tích:** 
    - Base case: Nếu đoạn mảng chỉ có 1 phần tử, trả về phần tử đó. Nếu có 2 phần tử, trả về phần tử lớn hơn.
    - Chia: Cắt đoạn mảng [left, right] thành 2 nửa tại vị trí mid = (left + right) / 2.
    - Trị: Gọi đệ quy tìm max nửa trái [left, mid] và nửa phải [mid + 1, right].
    - Tổng hợp: Trả về số lớn hơn giữa max trái và max phải.
- **Code mẫu** 
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Hàm đệ quy tìm Max
int timMax(const vector<int>& A, int left, int right) {
    // 1. Base case: Chỉ có 1 phần tử
    if (left == right) {
        return A[left];
    }
    
    // 2. Bước CHIA: Tìm điểm giữa
    int mid = (left + right) / 2;
    
    // 3. Bước TRỊ: Gọi đệ quy cho 2 nửa
    int max_trai = timMax(A, left, mid);
    int max_phai = timMax(A, mid + 1, right);
    
    // 4. Bước TỔNG HỢP
    return max(max_trai, max_phai);
}

int main() {
    vector<int> A = {8, 3, 6, 2, 9, 5, 4, 7};
    cout << "So lon nhat la: " << timMax(A, 0, A.size() - 1) << endl;
    return 0;
}
```

**Bài toán 2:** Lũy thừa nhị phân (Binary Exponentiation)
- **Phân tích:** Cần tính $a^n$. Khác với vòng lặp for chạy $n$ lần (tốn $O(N)$), ta áp dụng chia để trị: Nếu $n$ chẵn: $a^n = a^{n/2} \times a^{n/2}$ nếu $n$ lẻ: $a^n = a^{n/2} \times a^{n/2} \times a$
    - Base case: Nếu $n = 0$, trả về 1.
    - Chia: Bài toán $a^n$ sẽ chia thành $a^{n/2}
    - Trị: Sau khi đã xác định được bài toán con là $a^{\lfloor n/2 \rfloor}$, ta gọi đệ quy để máy tính tự đi tìm đáp án cho phần này và lưu vào một biến tạm tên là `tam`. Thay vì tính $a^{n/2$ hai lần, ta chỉ gọi đệ quy 1 lần, lưu vào biến tam, rồi lấy `tam * tam`. Thuật toán được tăng tốc độ lên mức $O(\log N)$ (cực kỳ nhanh).Code mẫu C++ (Lũy thừa nhanh)
    - Tổng hợp: Ta đã có kết quả của nửa bài toán trong tay (biến tam). Ta chia làm 2 trường hợp dựa vào tính chẵn lẻ của $n$:
        - Nếu $n$ chẵn ta sẽ tính `tam * tam`
        - Nếu $n$ lẻ ta sẽ tính `tam * tam * a` 
- **Code mẫu** 
```cpp
#include <iostream>

using namespace std;

// Hàm đệ quy tính a^n
long long luyThuaNhanh(long long a, long long n) {
    // 1. Base case
    if (n == 0) return 1;
    
    // 2 & 3. CHIA và TRỊ: Gọi đệ quy tính a^(n/2)
    long long tam = luyThuaNhanh(a, n / 2);
    
    // 4. TỔNG HỢP
    if (n % 2 == 0) {
        // Nếu n chẵn
        return tam * tam;
    } else {
        // Nếu n lẻ
        return tam * tam * a;
    }
}

int main() {
    long long a = 2, n = 10;
    cout << a << "^" << n << " = " << luyThuaNhanh(a, n) << endl;
    return 0;
}
```
</div>

<div class="important-note">

### 💡 Lưu ý khi lập trình thi đấu (CP Tips):
- **Lỗi tràn bộ nhớ (Stack Overflow):** Vì dùng đệ quy, nếu không có điểm dừng (Base case) hoặc chia bài toán sai cách, hàm đệ quy sẽ gọi vô hạn gây crash chương trình.
- **Phép Modulo:** Trong các bài Lũy thừa nhị phân, số thường rất khổng lồ. Đề bài thường yêu cầu chia lấy dư cho một số $MOD$ (ví dụ $10^9 + 7$). ta cần thêm phép <code>% MOD</code> vào từng bước nhân <code>tam * tam % MOD</code>.
- **Tối ưu mảng:** Khi truyền mảng vào hàm đệ quy, luôn nhớ truyền theo dạng tham chiếu <code>const vector&lt;int&gt;&amp; A</code> để tránh việc copy mảng liên tục làm chương trình chạy chậm và cạn kiệt bộ nhớ.
</div>