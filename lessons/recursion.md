## 🪆 Tư tưởng Lập trình Đệ quy (Recursion)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm Đệ quy</div>

**Khái niệm:** 
- Đệ quy trong lập trình là hiện tượng một hàm tự gọi lại chính nó bên trong phần thân của hàm đó. 
- Quá trình này bẻ bài toán lớn thành các bài toán con có bản chất giống hệt bài toán ban đầu, chỉ khác là kích thước dữ liệu nhỏ hơn. 
- Việc gọi lại chính nó sẽ tiếp tục cho đến khi bài toán đủ nhỏ để có thể giải quyết trực tiếp mà không cần chia cắt thêm.
**Ví dụ trực quan:** Giống như hình ảnh búp bê Nga (Matryoshka): Mở một con búp bê lớn ra, ta thấy một con búp bê nhỏ hơn y hệt bên trong, mở tiếp lại thấy con nhỏ hơn nữa... cho đến khi gặp con búp bê nhỏ nhất bằng gỗ đặc (không thể mở được nữa).
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Cấu trúc chuẩn của Hàm đệ quy</div>

***Một hàm đệ quy bắt buộc phải có 2 phần độc lập, thiếu một trong hai chương trình sẽ chạy sai hoặc bị treo:***
- **Phần cơ sở (Điều kiện dừng - Base Case):** Là trường hợp bài toán có kích thước nhỏ nhất, có thể trả về kết quả ngay lập tức mà không cần gọi lại hàm. Đây là "chiếc phanh" giúp hàm đệ quy thoát ra.
- **Phần đệ quy (Bước đệ quy - Recursive Step):** Là phần hàm tự gọi lại chính nó với bộ tham số nhỏ hơn, đưa bài toán tiến dần về điều kiện dừng.
- **Khung code mẫu (Template):**
```cpp
KieuDuLieu TenHamDeQuy(ThamSo) {
    // 1. ĐIỀU KIỆN DỪNG (Base Case)
    if (Bai_toan_da_du_nho) {
        return Ket_qua_truc_tiep;
    }
    
    // 2. BƯỚC ĐỆ QUY (Recursive Step)
    ThamSoMoi = Thu_nho(ThamSo);
    return Tinh_toan_voi( TenHamDeQuy(ThamSoMoi) );
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Phân loại Hàm đệ quy</div>

### A. Đệ quy Tuyến tính (Linear Recursion)
- **Cấu trúc:** Trong thân hàm chỉ có 1 lời gọi đệ quy duy nhất. Dòng chảy chương trình đi theo một đường thẳng.
- **Ví dụ:** Tính giai thừa $N! = N \times (N-1)!$
- **Phân tích:** 
    - Điều kiện dừng: $N = 0 \rightarrow 0! = 1$.
    - Bước đệ quy: Trả về $N \times$ tính_giai_thừa($N-1$).
- **Code mẫu:**
```cpp
long long giaiThua(int n) {
    if (n == 0) return 1;                  // Điều kiện dừng
    return n * giaiThua(n - 1);            // Lời gọi đệ quy tuyến tính
}
```

### B. Đệ quy Nhị phân (Binary Recursion)
- **Cấu trúc:** Trong thân hàm có 2 lời gọi đệ quy tới chính nó.
- **Ví dụ:** Dãy số Fibonacci $F(n) = F(n-1) + F(n-2)$.
- **Phân tích:**
    - Điều kiện dừng: $N = 0 \rightarrow 0$; $N = 1 \rightarrow 1$.
    - Bước đệ quy: Trả về tổng của 2 lời gọi hàm với $N-1$ và $N-2$.
- **Code mẫu:**
```cpp
long long fibonacci(int n) {
    if (n <= 1) return n;                  // Điều kiện dừng
    return fibonacci(n - 1) + fibonacci(n - 2); // 2 lời gọi đệ quy
}
```

### C. Đệ quy Phi tuyến (Non-linear Recursion)

- **Cấu trúc:** Lời gọi đệ quy được đặt bên trong một vòng lặp. Điều này tạo ra một cây đệ quy có số lượng nhánh thay đổi tùy thuộc vào vòng lặp.
- **Ví dụ:** Tính dãy số $X_n = X_0 + X_1 + \dots + X_{n-1}$ (với $X_0 = 1$).
- **Phân tích:** 
    - Điều kiện dừng: $N = 0 \rightarrow 1$;
    - Bước đệ quy: Để tính $X_n$, ta phải dùng vòng lặp for chạy từ $0$ đến $n-1$, mỗi lần lặp gọi đệ quy để tính $X_i$ và cộng dồn.
- **Code mẫu:**
```cpp
long long deQuyPhiTuyen(int n) {
    if (n == 0) return 1;                  // Điều kiện dừng
    long long tong = 0;
    // Vòng lặp chứa lời gọi đệ quy
    for (int i = 0; i < n; i++) {
        tong += deQuyPhiTuyen(i);
    }
    return tong;
}
```

### D. Đệ quy Tương hỗ (Mutual Recursion)
- **Cấu trúc:** Hàm A gọi hàm B, và hàm B lại gọi hàm A. (A và B đan chéo nhau).
- **Ví dụ:** Kiểm tra một số nguyên dương $N$ là chẵn hay lẻ mà không dùng phép chia dư (%).
- **Phân tích:**
    - Điều kiện dừng: $N = 0 \rightarrow true$;
    - Bước đệ quy: $N$ là chẵn nếu $N-1$ là lẻ. $N$ là lẻ nếu $N-1$ là chẵn. Dừng khi $N=0$ ($0$ là chẵn).
- **Code mẫu:**
```cpp
bool isOdd(int n); // Khai báo nguyên mẫu hàm trước

bool isEven(int n) {
    if (n == 0) return true;               // 0 là số chẵn
    return isOdd(n - 1);                   // Gọi sang hàm isOdd
}

bool isOdd(int n) {
    if (n == 0) return false;              // 0 không phải số lẻ
    return isEven(n - 1);                  // Gọi sang hàm isEven
}
```

### E. Đệ quy Quay lui (Backtracking Recursion)
- **Cấu trúc:** Là dạng nâng cao của đệ quy phi tuyến. Nó duyệt qua mọi khả năng (nhánh). Nếu nhánh đi vào ngõ cụt hoặc không thỏa mãn, nó sẽ hủy bỏ kết quả thử nghiệm (Undo) và "quay lui" về trạng thái trước đó để thử nhánh khác.
- **Khung code mẫu (Template):**
```cpp
KieuDuLieu TenHamDeQuyQuayLui(ThamSo) {
    // 1. ĐIỀU KIỆN DỪNG (Base Case)
    if (Bai_toan_da_du_nho) {
        return Ket_qua_truc_tiep;
    }
    
    // 2. BƯỚC ĐỆ QUY (Recursive Step)
    Khối lệnh thực hiện trước khi gọi vào hàm đệ quy (nhóm lệnh đi vào)
    ...
    // LỜi gọi đệ quy
    TenHamDeQuyQuayLui(ThamSoMoi)
    Khối lệnh thực hiện sau khi gọi vào hàm đệ quy (nhóm lệnh đi ra)
    ...
}
```
- **Ví dụ:** Sinh tất cả xâu nhị phân độ dài $N$.
- **Phân tích:** Đề sinh dãy nhị phân có $N$ phần tử ta cần điền các số $0$ hoặc $1$ vào $N$ ô trống (từ ô số $1$ đến ô số $N$). Hàm đệ quy `sinhNhiPhan(vi_tri)` sẽ đảm nhận nhiệm vụ điền giá trị cho ô trống tại vi_tri.
    - Điều kiện dừng (base case): Khi biến `vi_tri > N`, điều đó có nghĩa là $N$ ô trước đó đã đưuọc điền đầy đủ. Lúc này ta xuất dãy nhị phân hiện tại sau đó `return;` quay lui lại bước trước đó để thử các cách khác.
    - Bước đệ quy: Tại ô hiện tại (`vi_tri`), ta luôn có $2$ sự lựa chọn là điền số $0$ hoặc số $1$. Ta dùng vòng lặp for để lần lượt thử các lựa chọn này:
        - Bước Thử (Try): Điền giá trị đang thử vào mảng xau[vi_tri] = gia_tri; (Ví dụ điền số $0$).
        - Bước Tiến (Recurse): Sau khi điền xong ô hiện tại, ta gọi đệ quy sinhNhiPhan(vi_tri + 1); để tiếp tục nhờ máy tính điền ô tiếp theo.
        - Bước Quay lui (Backtrack/Undo): Khi lời gọi đệ quy ở ô tiếp theo kết thúc (đã in ra kết quả hoặc đi vào ngõ cụt) và quay ngược trở lại, vòng lặp for hiện tại sẽ tự động chuyển sang gia_tri tiếp theo (từ $0$ lên $1$). Hành động ghi đè số $1$ vào vị trí cũ chính là ta đã "từ bỏ" sự lựa chọn số $0$ trước đó để thử sang một nhánh mới.
- **Code mẫu:**
```cpp
vector<int> xau;
int N = 3;

void sinhNhiPhan(int vi_tri) {
    // 1. Điều kiện dừng: Đã sinh đủ N phần tử
    if (vi_tri > N) {
        for(int i=1; i<=N; i++) cout << xau[i];
        cout << "\n";
        return;
    }
    
    // 2. Bước đệ quy có vòng lặp (Quay lui)
    for (int gia_tri = 0; gia_tri <= 1; gia_tri++) {
        xau[vi_tri] = gia_tri;             // THỬ chọn giá trị
        sinhNhiPhan(vi_tri + 1);           // Gọi đệ quy đi tiếp
        // (Quay lui: Với mảng ghi đè thì không cần lệnh Undo thủ công)
    }
}
```
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">4. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div>
<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Vấn đề</th><th style="width: 75%;">Phân tích & Cách khắc phục</th></tr></thead><tbody><tr><td><b>Tràn ngăn xếp (Stack Overflow)</b></td><td>Mỗi lần gọi đệ quy, hệ điều hành phải cấp phát một bộ nhớ trong <b>Stack</b>. Nếu không có Điều kiện dừng, hoặc N quá lớn (ví dụ gọi đệ quy $10^5$ lần), bộ nhớ Stack sẽ đầy và chương trình bị Crash đột ngột.<b>$\rightarrow$ Khắc phục:</b> Luôn kiểm tra kỹ Điều kiện dừng. Nếu $N$ rất lớn, hãy dùng vòng lặp thay vì đệ quy.</td></tr><tr><td><b>Độ phức tạp Thời gian (TLE)</b></td><td>Đệ quy Nhị phân và Phi tuyến (như bài Fibonacci ngây thơ) có thời gian chạy tăng theo hàm mũ $O(2^N)$. Với $N = 50$, chương trình sẽ chạy mất hàng năm mới xong do bị lặp lại việc tính toán các bài toán con.<b>$\rightarrow$ Khắc phục:</b> Áp dụng kỹ thuật <b>Ghi nhớ (Memoization)</b> của Quy hoạch động. Lưu kết quả đệ quy vào mảng, nếu gọi lại thì lấy từ mảng ra dùng luôn.</td></tr><tr><td><b>Vị trí lệnh <code>return</code></b></td><td>Nhiều học sinh quên viết chữ <code>return</code> trước lời gọi đệ quy đối với các hàm có kiểu trả về (như <code>int</code>, <code>long long</code>), dẫn đến nhận kết quả rác (Garbage value).</td></tr><tr><td><b>Truyền tham biến</b></td><td>Khi truyền các cấu trúc dữ liệu lớn (như <code>vector</code>, <code>string</code>) vào tham số của hàm đệ quy, hãy nhớ dùng ký hiệu tham chiếu <code>&amp;</code> (ví dụ: <code>void deQuy(string &amp;s)</code>) để tránh việc copy dữ liệu liên tục gây cạn kiệt bộ nhớ.</td></tr></tbody></table>
</div>
</div>