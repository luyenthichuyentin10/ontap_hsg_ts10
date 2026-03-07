## 🔁 Kỹ Thuật Vét Cạn (Brute Force)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

**Tư tưởng cốt lõi:** Vét cạn (Brute Force) là phương pháp giải quyết bài toán bằng cách thử tất cả các trường hợp có thể xảy ra để tìm ra đáp án đúng. Thay vì dùng các suy luận toán học phức tạp để đi thẳng đến đích, ta duyệt qua toàn bộ không gian nghiệm (State Space) và kiểm tra xem nghiệm nào thỏa mãn yêu cầu.

**Đặc điểm:**
- **Ưu điểm:** Cực kỳ dễ cài đặt, logic đơn giản, luôn đảm bảo tìm ra nghiệm chính xác (nếu thời gian cho phép). Thường được dùng để viết thuật toán "trâu" (trau cày) để đối chiếu kết quả (stress test) với thuật toán tối ưu.
- **Nhược điểm:** Tốn rất nhiều thời gian chạy và bộ nhớ. Chỉ áp dụng được với giới hạn dữ liệu (N) rất nhỏ.
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các kỹ thuật cài đặt vét cạn</div>

### A. Vét cạn bằng Vòng lặp (Iterative Brute Force)
- **Phân tích:** Sử dụng các vòng lặp for hoặc while lồng nhau để tạo ra các trạng thái. Phương pháp này phù hợp khi số lượng biến (số chiều) của cấu hình nghiệm là cố định và nhỏ (ví dụ: tìm 2 điểm, 3 số, 4 cạnh).
- **Ví dụ minh họa:** Cho mảng $A$ gồm $N$ phần tử. Tìm 3 chỉ số $i, j, k$ ($i < j < k$) sao cho $A[i] + A[j] + A[k] = S$.
- **Code mẫu C++ (Vòng lặp):**
```cpp
// Hàm tìm bộ 3 số có tổng bằng S
void timBaSo(vector<int>& A, int S) {
    int n = A.size();
    bool tim_thay = false;
    
    // Vét cạn 3 vòng lặp lồng nhau O(N^3)
    for (int i = 0; i < n - 2; i++) {
        for (int j = i + 1; j < n - 1; j++) {
            for (int k = j + 1; k < n; k++) {
                if (A[i] + A[j] + A[k] == S) {
                    cout << "Tim thay: " << A[i] << ", " << A[j] << ", " << A[k] << endl;
                    tim_thay = true;
                }
            }
        }
    }
    
    if (!tim_thay) {
        cout << "Khong co bo 3 nao thoa man." << endl;
    }
}
```

### B. Vét cạn bằng Quay lui (Backtracking)
- **Phân tích:** Khi số lượng phần tử cần chọn không cố định (ví dụ $N$ phần tử) hoặc phụ thuộc vào input, ta không thể viết trước $N$ vòng lặp lồng nhau. Quay lui (Backtracking) dùng đệ quy để xây dựng từng phần của cấu hình nghiệm. Khi nhận thấy một hướng đi không thể dẫn tới kết quả đúng, thuật toán sẽ "quay lui" lại bước trước đó để thử hướng khác.
- **Ví dụ minh họa:** In ra tất cả các xâu nhị phân có độ dài $N$. (Tại mỗi vị trí $i$, ta thử đặt giá trị 0, sau đó đệ quy tiếp; rồi thử đặt giá trị 1, đệ quy tiếp).
- **Code mẫu C++ (Backtracking):**
```cpp
int N;
vector<int> xau_nhi_phan;

// Hàm đệ quy quay lui để sinh xâu nhị phân
void thu_gia_tri(int vi_tri) {
    // Bước cơ sở: Đã điền đủ N vị trí thì in ra cấu hình
    if (vi_tri > N) {
        for (int i = 1; i <= N; i++) {
            cout << xau_nhi_phan[i];
        }
        cout << endl;
        return;
    }

    // Tại vi_tri hiện tại, thử các giá trị có thể (0 và 1)
    for (int gia_tri = 0; gia_tri <= 1; gia_tri++) {
        xau_nhi_phan[vi_tri] = gia_tri; // Ghi nhận thử nghiệm
        thu_gia_tri(vi_tri + 1);        // Đệ quy đi tiếp sang vị trí tiếp theo
        // Khi quay lui lên, vòng lặp tự động thử gia_tri tiếp theo (không cần xóa vết với bài này)
    }
}

int main() {
    N = 3; // Sinh xâu nhị phân độ dài 3
    xau_nhi_phan.resize(N + 1);
    cout << "Cac xau nhi phan do dai " << N << " la:" << endl;
    thu_gia_tri(1); // Bắt đầu thử từ vị trí số 1
    return 0;
}
```

### C. Vét cạn bằng Bitmask (Sinh tập con bằng Nhị phân)
- **Cơ chế hoạt động:** 
    - Giả sử ta có một tập hợp gồm $N$ phần tử. Mỗi phần tử sẽ có 2 trạng thái: Được chọn (1) hoặc Không được chọn (0).
    - Ghép trạng thái của $N$ phần tử lại, ta sẽ được một chuỗi nhị phân có độ dài $N$. Chuỗi nhị phân này tương đương với một số nguyên trong hệ thập phân có giá trị từ $0$ đến $2^N - 1$.
    - Do đó, thay vì đệ quy, ta chỉ cần cho một biến mask (mặt nạ) chạy từ $0$ đến $2^N - 1$. Với mỗi giá trị của mask, ta phân tích các bit nhị phân của nó để biết phần tử nào đang được chọn
- **Dấu hiệu áp dụng:** 
    - Bài toán yêu cầu duyệt qua tất cả các tập con của một mảng.
    - Giới hạn dữ liệu rất nhỏ: $N \le 20$ (vì $2^{20} \approx 10^6$, máy tính chạy trong chớp mắt. Nếu $N \ge 30$, bitmask sẽ bị quá thời gian).
- **Ví dụ minh họa:** Cho mảng $A = \{10, 20, 30\}$ ($N = 3$). Hãy in ra tất cả các tập con có thể tạo ra từ mảng này.
    - Số lượng tập con là $2^3 = 8$ (tương ứng với mask chạy từ 0 đến 7).
        - mask = 0 (Nhị phân 000): Không chọn phần tử nào $\rightarrow$ Tập rỗng {}
        - mask = 3 (Nhị phân 011): Chọn phần tử ở vị trí số 0 và số 1 $\rightarrow$ Tập {10, 20}
        - mask = 5 (Nhị phân 101): Chọn phần tử ở vị trí số 0 và số 2 $\rightarrow$ Tập {10, 30}
        - mask = 7 (Nhị phân 111): Chọn tất cả $\rightarrow$ Tập {10, 20, 30}
- **Code mẫu C++ (Backtracking):**
```cpp
// Hàm duyệt và in tất cả các tập con
void duyetTatCaTapCon(vector<int>& A) {
    int N = A.size();
    
    // (1 << N) tương đương với 2 lũy thừa N
    int tong_so_tap_con = 1 << N; 
    
    // Vòng lặp mask chạy từ 0 đến (2^N - 1)
    for (int mask = 0; mask < tong_so_tap_con; mask++) {
        cout << "Tap con: { ";
        
        // Kiểm tra xem phần tử thứ i có được chọn trong mask hiện tại không
        for (int i = 0; i < N; i++) {
            // Phép toán (mask & (1 << i)) dùng để lấy bit thứ i của biến mask
            // Nếu bit đó là 1 (tức là giá trị khác 0), ta chọn phần tử A[i]
            if (mask & (1 << i)) {
                cout << A[i] << " ";
            }
        }
        
        cout << "}" << endl;
    }
}
```
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Các dạng bài bắt buộc phải dùng Vét cạn</div>

***Trong lập trình thi đấu, có những bài toán không tồn tại thuật toán đa thức (như Quy hoạch động hay Tham lam) để giải quyết triệt để, hoặc giới hạn bài toán gợi ý rõ việc dùng vét cạn:***
### 1. Bài toán yêu cầu "Liệt kê tất cả":
- In ra tất cả các hoán vị của mảng.
- In ra mọi tổ hợp chập $K$ của $N$ phần tử.
- Dấu hiệu: Đề bài yêu cầu in ra mọi trường hợp, không phải đếm số lượng hay tìm Max/Min.
### 2. Giới hạn dữ liệu ($N$) cực kỳ nhỏ:
- $N \le 20$: Phù hợp vét cạn tập con (Độ phức tạp $O(2^N)$).
- $N \le 10$: Phù hợp vét cạn hoán vị (Độ phức tạp $O(N!)$).
### 3. **Bài toán NP-Hard cỡ nhỏ:
- Bài toán người đi du lịch (TSP) đi qua dưới 12 thành phố.
- Bài toán xếp hậu (N-Queens) trên bàn cờ $N \times N$ ($N \le 12$).
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">4. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div> 

<div class="important-note">
💡 <b>Bí kíp cho học sinh:</b> 

- Trong phòng thi ta **luôn bắt đầu** giải bài toán bằng **vét cạn** để từ đó có thể hiểu chính xác hơn đề bài, dễ tìm ra thuật toán tổng quát và dùng kết quả vét cạn để kiểm chứng kết quả của thuật toán tổng quát.
- Việc vét cạn không phải chúng ta dở mà đó là một **chiến thuật làm bài thông minh** giúp lấy được tối đa số điểm có thể đạt được, tạo lợi thế tâm lý tốt trước khi bắt tay vào giải tổng quát các bài khó trong đề.
- **Vét cạn** không chỉ là "cứ viết bừa là xong". Một thuật toán **vét cạn khôn ngoan** (có cắt tỉa nhánh) có thể chạy nhanh gấp hàng vạn lần vét cạn thuần túy.
</div>
<div class="geometry-table-container"><table class="styled-table"><thead><tr><th>Chiến thuật</th><th>Mô tả chi tiết & Ứng dụng</th></tr></thead><tbody><tr><td><b>Nhẩm tính độ phức tạp</b></td><td>Máy chấm (Judge) thường chạy được khoảng $10^8$ phép tính / 1 giây. Nếu $N=100$, vòng lặp $O(N^3)$ mất $10^6$ phép tính (OK). Nhưng nếu $N=1000$, vòng lặp $O(N^3)$ mất $10^9$ phép tính $\rightarrow$ Chắc chắn bị Quá thời gian (TLE).</td></tr><tr><td><b>Cắt tỉa nhánh (Pruning)</b></td><td>Trong lúc đệ quy Backtracking, nếu phát hiện nhánh hiện tại <b>chắc chắn không thể</b> cho ra đáp án đúng (ví dụ: tổng hiện tại đã lớn hơn mục tiêu), phải dùng lệnh <code>return;</code> để dừng ngay lập tức, không đi sâu thêm nữa.</td></tr><tr><td><b>Thứ tự thử giá trị</b></td><td>Nếu bài toán yêu cầu in cấu hình theo thứ tự "Từ điển" (Lexicographical order), hãy sắp xếp mảng đầu vào từ bé đến lớn trước khi Backtracking, và lặp <code>for</code> thử giá trị từ nhỏ đến lớn.</td></tr><tr><td><b>Duyệt tập con bằng Bitmask</b></td><td>Thay vì viết Backtracking dài dòng để chọn / không chọn, với $N \le 20$, hãy dùng vòng lặp <code>for (int mask = 0; mask < (1 << N); mask++)</code>. Các bit 1 trong <code>mask</code> tương ứng với các phần tử được chọn. Chạy cực kỳ nhanh và code ngắn gọn.</td></tr></tbody></table>
</div>