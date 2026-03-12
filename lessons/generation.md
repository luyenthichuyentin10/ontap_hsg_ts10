## ⚙️ Kỹ Thuật Sinh Kế Tiếp (Next Generation Algorithms)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Phân tích thuật toán chung</div>

**Khái niệm:** Thuật toán sinh (Generation) là phương pháp duyệt qua tất cả các cấu hình có thể có của một bài toán tổ hợp (như tập con, tổ hợp, hoán vị) theo thứ tự từ điển (Lexicographical order).Ta bắt đầu từ cấu hình nhỏ nhất, liên tục áp dụng quy tắc để sinh ra cấu hình lớn hơn tiếp theo, cho đến khi đạt được cấu hình lớn nhất thì dừng lại.

**Phân tích 4 bước làm chuẩn mực (Khung thuật toán):** Mọi bài toán sinh kế tiếp đều tuân theo một bộ khung (Template) cố định gồm 4 bước:
1. **Khởi tạo:** Thiết lập cấu hình đầu tiên (nhỏ nhất).
2. **Kiểm tra vòng lặp:** Chừng nào chưa đạt cấu hình cuối cùng (lớn nhất) thì tiếp tục lặp.
3. **In/Xử lý:** Xử lý cấu hình hiện tại (in ra màn hình hoặc tính toán).
4. **Sinh kế tiếp (Next):** Tìm cách biến đổi cấu hình hiện tại thành cấu hình kế tiếp ngay sau nó.
```cpp
// Khung code giả chung cho mọi bài toán Sinh
void ThuatToanSinh() {
    KhoiTaoCauHinhDauTien();
    bool isFinal = false;
    while (!isFinal) {
        InCauHinhHienTai();
        SinhCauHinhKeTiep(isFinal); // Cập nhật isFinal = true nếu đã đến cấu hình cuối
    }
}
```
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các thuật toán sinh kinh điển</div>

### A. Sinh xâu nhị phân độ dài $N$ (Tập con)

- **Phân tích:** 
    - Cấu hình đầu: Toàn số 0 (Ví dụ $N=3 \rightarrow$ 000).
    - Cấu hình cuối: Toàn số 1 (Ví dụ $N=3 \rightarrow$ 111).
    - Quy tắc sinh kế tiếp: Đi từ phải sang trái, tìm số 0 đầu tiên gặp được. Biến nó thành 1, và biến tất cả các số 1 đứng sau nó thành 0.
- **Ví dụ:** Đang có 01011. Đi từ phải sang trái, số 0 đầu tiên nằm ở vị trí thứ 3. Đổi nó thành 1, các số sau nó đổi thành 0 $\rightarrow$ Kết quả: 01100.
- **Code mẫu C++:**
```cpp
void sinhNhiPhan(int N) {
    vector<int> A(N + 1, 0); // Khởi tạo mảng toàn 0 (dùng chỉ số từ 1 đến N)
    bool isFinal = false;

    while (!isFinal) {
        // 1. In cấu hình
        for (int i = 1; i <= N; i++) cout << A[i];
        cout << endl;

        // 2. Sinh cấu hình kế tiếp
        int i = N;
        while (i > 0 && A[i] == 1) { // Tìm số 0 đầu tiên từ phải sang trái
            A[i] = 0; // Đổi các số 1 thành 0
            i--;
        }

        if (i == 0) {
            isFinal = true; // Nếu duyệt hết đến vị trí 0 mà toàn số 1 -> Đã là cấu hình cuối
        } else {
            A[i] = 1; // Đổi số 0 tìm được thành 1
        }
    }
}
```

### B. Sinh Tổ hợp chập $K$ của $N$
- **Phân tích:**
    - Cấu hình đầu: Các số tăng dần từ $1$ đến $K$ (Ví dụ $K=3, N=5 \rightarrow$ 1 2 3).
    - Cấu hình cuối: Các số đạt giới hạn lớn nhất $N-K+i$ (Ví dụ $\rightarrow$ 3 4 5).
    - Quy tắc sinh kế tiếp: Đi từ phải sang trái, tìm phần tử $A[i]$ chưa đạt giới hạn trên của nó (Tức là $A[i] \ne N - K + i$). Tăng $A[i]$ lên 1 đơn vị, sau đó gán lại tất cả các phần tử phía sau nó theo quy tắc tăng dần $1$ đơn vị so với phần tử đứng trước.
- **Ví dụ:** Đang có tổ hợp 1 4 5 ($K=3, N=5$). Giới hạn của từng vị trí là (3, 4, 5).Xét từ cuối: 5 chạm giới hạn, 4 chạm giới hạn. 1 chưa chạm giới hạn (1 < 3).$\rightarrow$ Tăng 1 lên thành 2. Các số sau gán tăng dần: số tiếp theo là 3, số tiếp theo là 4.$\rightarrow$ Kết quả: 2 3 4.
- **Code mẫu C++:**
```cpp
void sinhToHop(int N, int K) {
    vector<int> A(K + 1);
    for (int i = 1; i <= K; i++) A[i] = i; // Khởi tạo 1, 2, ..., K
    bool isFinal = false;

    while (!isFinal) {
        // 1. In cấu hình
        for (int i = 1; i <= K; i++) cout << A[i] << " ";
        cout << endl;

        // 2. Sinh kế tiếp
        int i = K;
        // Tìm phần tử chưa chạm giới hạn trên (N - K + i)
        while (i > 0 && A[i] == N - K + i) {
            i--;
        }

        if (i == 0) {
            isFinal = true;
        } else {
            A[i]++; // Tăng phần tử tìm được lên 1
            // Cập nhật lại các phần tử phía sau
            for (int j = i + 1; j <= K; j++) {
                A[j] = A[j - 1] + 1;
            }
        }
    }
}
```

### C. Sinh Hoán vị của $N$ phần tử
- **Phân tích:** 
    - Cấu hình đầu: Các số sắp xếp tăng dần $1, 2, \dots, N$.
    - Cấu hình cuối: Các số sắp xếp giảm dần $N, N-1, \dots, 1$.
    - Quy tắc sinh:
        1. Tìm vị trí $i$ từ phải sang trái sao cho $A[i] < A[i+1]$ (Tìm phần tử đầu tiên phá vỡ thế giảm dần).
        2. Tìm vị trí $j$ từ phải sang trái sao cho $A[j] > A[i]$.
        3. Đổi chỗ $A[i]$ và $A[j]$.
        4. Lật ngược (Reverse) toàn bộ đoạn từ $A[i+1]$ đến $A[N]$.
- **Code mẫu C++ (Sử dụng hàm có sẵn cực nhanh):** Trong thực tế thi đấu, C++ đã cung cấp sẵn hàm `next_permutation` trong thư viện `<algorithm>`, học sinh không cần (và không nên) code tay thuật toán này để tránh lỗi.
```cpp
void sinhHoanVi(int N) {
    vector<int> A(N);
    for (int i = 0; i < N; i++) A[i] = i + 1; // Khởi tạo 1, 2, ..., N

    // Vòng lặp do-while rất phù hợp vì ta in cấu hình đầu trước, rồi mới sinh cấu hình mới
    do {
        for (int i = 0; i < N; i++) cout << A[i] << " ";
        cout << endl;
    } while (next_permutation(A.begin(), A.end())); 
    // Hàm tự sinh cấu hình kế tiếp. Nếu mảng bị lùi về cấu hình đầu (sau khi đạt cấu hình cuối) sẽ trả về false.
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. So sánh: Sinh kế tiếp vs Đệ quy Quay lui (Backtracking)</div>

**Cả hai kỹ thuật đều dùng để liệt kê cấu hình, nhưng có triết lý và ứng dụng khác nhau:**

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 20%;">Tiêu chí</th><th style="width: 40%;">Phương pháp Sinh (Generation)</th><th style="width: 40%;">Quay lui (Backtracking)</th></tr></thead><tbody><tr><td><b>Bản chất</b></td><td>Dùng <b>vòng lặp (Iterative)</b> để biến đổi trực tiếp mảng hiện tại thành mảng tiếp theo.</td><td>Dùng <b>hàm đệ quy (Recursive)</b> để xây dựng từng phần tử một trên cây không gian trạng thái.</td></tr><tr style="background-color: #f8fafc;"><td><b>Bộ nhớ (Memory)</b></td><td>Cực kỳ tối ưu $O(1)$ không gian phụ trợ (vì chỉ biến đổi mảng gốc).</td><td>Tốn bộ nhớ Stack cho mỗi lần gọi đệ quy $O(N)$. Dễ bị Stack Overflow nếu $N$ lớn.</td></tr><tr><td><b>Tính linh hoạt</b></td><td>Kém linh hoạt. Rất khó cài đặt nếu đề bài có thêm các <b>điều kiện ràng buộc phức tạp</b>.</td><td>Cực kỳ linh hoạt. Dễ dàng thêm các lệnh <code>if</code> để <b>Cắt tỉa nhánh (Pruning)</b>, bỏ qua cấu hình sai từ sớm.</td></tr><tr style="background-color: #f8fafc;"><td><b>Thời điểm dùng</b></td><td>Bài toán chuẩn mực: Liệt kê Hoán vị, Tổ hợp cơ bản không có ràng buộc lạ.</td><td>Bài toán N-Hậu, Mã đi tuần, Giải Sudoku, Tìm đường đi trên mê cung.</td></tr></tbody></table></div></div>

<div class="step-card border-red">
<div class="step-badge bg-red">4. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div>

<ul><li><b>Giới hạn thời gian sinh (TLE):</b> Thuật toán sinh duyệt qua <i>tất cả</i> các cấu hình.<ul><li>Sinh nhị phân tốn $O(2^N) \rightarrow$ Chỉ chạy nổi với $N \le 22$.</li><li>Sinh hoán vị tốn $O(N!) \rightarrow$ Chỉ chạy nổi với $N \le 11$.</li></ul>Nhắc học sinh nhìn vào $N$ của đề bài, nếu $N = 100$ mà đi viết thuật toán Sinh thì chắc chắn nhận 0 điểm vì TLE.</li><li><b>Bẫy của <code>next_permutation</code>:</b> Hàm này chỉ sinh ra các hoán vị lớn hơn cấu hình hiện tại. Nếu mảng ban đầu của học sinh là <code>{3, 2, 1}</code>, hàm sẽ lập tức trả về <code>false</code> và không in ra gì cả. <b>Bắt buộc phải <code>sort</code> mảng tăng dần</b> trước khi đưa vào vòng lặp <code>do-while</code>.</li><li><b>Sinh hoán vị có phần tử lặp lại:</b> Nếu mảng có các phần tử giống nhau (vd: <code>1 1 2</code>), hàm <code>next_permutation</code> cực kỳ thông minh, nó sẽ sinh ra đúng 3 cấu hình phân biệt (<code>1 1 2</code>, <code>1 2 1</code>, <code>2 1 1</code>) mà không bị trùng lặp. Đây là ưu điểm tuyệt đối so với việc tự code bằng Backtracking.</li><li><b>Sinh phân tích số:</b> Ngoài 3 dạng trên, bài toán <i>Liệt kê các cách phân tích số nguyên N thành tổng các số nguyên dương</i> cũng là một dạng sinh kế tiếp hay gặp (VD: $4 = 3+1 = 2+2 = 2+1+1 = 1+1+1+1$). Thầy có thể mở rộng nếu có thời gian.</li></ul>
</div>