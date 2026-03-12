## 🔍 Kỹ Thuật Tìm Kiếm Nhị Phân (Binary Search)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm, Phân tích và Cài đặt</div>

### Khái niệm:
- Tìm kiếm nhị phân là thuật toán tìm kiếm một phần tử trong một tập dữ liệu **đã được sắp xếp**. Hoạt động dựa trên tư tuỏng **chia để trị**: 
    - Tại mỗi bước, thuật toán so sánh phần tử cần tìm với phần tử ở chính giữa mảng. 
    - Nếu không khớp, nó sẽ loại bỏ một nửa mảng không thể chứa phần tử đó và tiếp tục tìm kiếm trên nửa còn lại.
### Ví dụ trực quan: 
- Hãy tưởng tượng trò chơi đoán số từ $1$ đến $100$. Nếu tôi chọn số $70$ và bạn đoán $50$, tôi gợi ý "Số của tôi lớn hơn". Ngay lập tức, bạn biết không cần đoán các số từ $1$ đến $50$ nữa, và tiếp tục đoán ở khoảng $51$ đến $100$ (chọn số giữa là $75$). Quá trình lặp lại cho đến khi tìm ra đáp án. 

### Phân tích và Cài đặt (Code mẫu C++):
- Thuật toán duy trì hai con trỏ trái (`left`) và phai (`right`) để giới hạn phạm vi tìm kiếm.
- Nếu giá trị ở giữa (`mid`) bằng mục tiêu $\rightarrow$ tìm thấy.
- Nếu giá trị ở giua nhỏ hơn mục tiêu $\rightarrow$ mục tiêu nằm bên phải $\rightarrow$ `dời trái = giua + 1`.
- Nếu giá trị ở giua lớn hơn mục tiêu $\rightarrow$ mục tiêu nằm bên trái $\rightarrow$ `dời phải = giua - 1`.
- Cú pháp: `binary_search(vị_trí_đầu, vị_trí_cuối, giá_trị_cần_tìm)`
- Kết quả trả về: Trả về true (nếu tìm thấy) hoặc false (nếu không tìm thấy).
- Lưu ý: Hàm này không cho biết vị trí của phần tử nằm ở đâu, chỉ trả lời câu hỏi **"Có hay Không"**.

### Hàm tự xây dựng
```cpp
// Hàm tìm kiếm nhị phân cơ bản (Trả về vị trí, hoặc -1 nếu không thấy)
int timKiemNhiPhan(const vector<int>& A, int muc_tieu) {
    int trai = 0;
    int phai = A.size() - 1;

    while (trai <= phai) {
        int giua = trai + (phai - trai) / 2; // Tránh tràn số khi trai và phai quá lớn

        if (A[giua] == muc_tieu) {
            return giua; // Tìm thấy
        } else if (A[giua] < muc_tieu) {
            trai = giua + 1; // Tìm nửa bên phải
        } else {
            phai = giua - 1; // Tìm nửa bên trái
        }
    }
    return -1; // Không tìm thấy
}
```
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các dạng hàm tìm kiếm nhị phân nâng cao</div>

***Trong C++ (thư viện <algorithm>), có 3 hàm tìm kiếm nhị phân tích hợp sẵn rất mạnh mẽ. Tuy nhiên, học sinh cần hiểu cách tự cài đặt chúng để áp dụng linh hoạt vào các bài toán.***

### A. Hàm lower_bound (Cận dưới)
- **Ý nghĩa:** Tìm vị trí đầu tiên của phần tử có giá trị lớn hơn hoặc bằng ($\ge$) giá trị mục tiêu $X$. Nếu tất cả các phần tử đều nhỏ hơn $X$, trả về vị trí ngoài cùng của mảng.
- **Tư tưởng:** Khi `A[giua] >= X`, ta lưu lại kết quả này, nhưng vẫn tiếp tục tìm kiếm về bên trái (`phai = giua - 1`) để xem có vị trí nào sớm hơn thỏa mãn không.
- **Cú pháp:** `lower_bound(vị_trí_đầu, vị_trí_cuối, giá_trị_cần_tìm)`
- **Kết quả trả về:** Trả về một con trỏ (`iterator`) trỏ đến phần tử thỏa mãn.
- **Cách lấy chỉ số (`Index`):** Để lấy được vị trí (chỉ số mảng) của phần tử đó, ta bắt buộc phải lấy kết quả trừ đi con trỏ đầu tiên (`a.begin()`).
```cpp
vector<int> a = {1, 2, 4, 4, 5, 8};
// Tìm phần tử đầu tiên >= 4
auto it = lower_bound(a.begin(), a.end(), 4);
int vi_tri = it - a.begin(); 
// Kết quả: vi_tri = 2 (tương ứng với số 4 đầu tiên)

// Tìm phần tử đầu tiên >= 6
int vi_tri_6 = lower_bound(a.begin(), a.end(), 6) - a.begin(); 
// Kết quả: vi_tri_6 = 5 (tương ứng với số 8)
```

- **Hàm tự xây dựng**
```cpp
int canDuoi(const vector<int>& A, int X) {
    int trai = 0, phai = A.size() - 1;
    int ket_qua = -1; // Lưu vị trí tìm được
    
    while (trai <= phai) {
        int giua = trai + (phai - trai) / 2;
        if (A[giua] >= X) {
            ket_qua = giua;    // Ghi nhận đáp án tạm thời
            phai = giua - 1;   // Ép giới hạn về bên trái để tìm vị trí TỐT HƠN
        } else {
            trai = giua + 1;
        }
    }
    return ket_qua;
}
```

### B. Hàm upper_bound (Cận trên)
- **Ý nghĩa:** Tìm vị trí đầu tiên của phần tử có giá trị lớn hơn hẳn ($>$) giá trị mục tiêu $X$.
- **Tư tưởng:** Gần giống lower_bound, nhưng điều kiện cập nhật đáp án là `A[giua] > X`. Học sinh thường dùng upper_bound(X) - lower_bound(X) để đếm số lượng phần tử có giá trị bằng $X$ trong mảng.
- **Cú pháp:** `upper_bound(vị_trí_đầu, vị_trí_cuối, giá_trị_cần_tìm)`
- **Kết quả trả về:** Trả về một con trỏ (`iterator`) trỏ đến phần tử thỏa mãn. Giống như lower_bound, ta cũng phải trừ đi `a.begin()` để lấy chỉ số.
```cpp
vector<int> a = {1, 2, 4, 4, 5, 8};
// Tìm phần tử đầu tiên > 4
auto it = upper_bound(a.begin(), a.end(), 4);
int vi_tri = it - a.begin(); 
// Kết quả: vi_tri = 4 (tương ứng với số 5)
```

### C. Hàm equal_range (Khoảng giá trị bằng nhau)
- **Mục đích:** Tìm ra toàn bộ đoạn (range) các phần tử có giá trị bằng đúng với giá trị $X$. Thực chất, hàm này gộp cả lower_bound và upper_bound vào làm một.
- **Cú pháp:** `equal_range(vị_trí_đầu, vị_trí_cuối, giá_trị_cần_tìm)`
- **Kết quả trả về:** Trả về một kiểu `pair` chứa 2 con trỏ (`iterators`):
    - pair.first: Trỏ đến vị trí của lower_bound (phần tử đầu tiên $\ge X$).
    - pair.second: Trỏ đến vị trí của upper_bound (phần tử đầu tiên $> X$).
```cpp
vector<int> a = {1, 2, 4, 4, 4, 5, 8};
    
    // Tìm khoảng các phần tử có giá trị bằng 4
    auto khoang = equal_range(a.begin(), a.end(), 4);
    
    // 1. Tính số lần xuất hiện của số 4 cực kỳ nhanh chóng:
    int so_luong = khoang.second - khoang.first; 
    cout << "So luong so 4: " << so_luong << endl; // Kết quả: 3
    
    // 2. Lấy chỉ số (index) bắt đầu và kết thúc của đoạn chứa số 4:
    int vi_tri_dau = khoang.first - a.begin();  // Kết quả: 2
    int vi_tri_cuoi = khoang.second - a.begin() - 1; // Kết quả: 4
```

</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Tìm kiếm nhị phân dựa trên đáp án (Binary Search on Answer)</div>

- **Phân tích thuật toán:** Đây là dạng bài "đỉnh cao" của tìm kiếm nhị phân. Thay vì tìm một số trong mảng, ta tìm kiếm chính đáp án của bài toán.
- **Dấu hiệu nhận biết:** Đề bài yêu cầu tìm giá trị "Lớn nhất sao cho..." hoặc "Nhỏ nhất sao cho...".
- **Điều kiện áp dụng (Tính đơn điệu):** Nếu một giá trị $X$ thỏa mãn yêu cầu, thì mọi giá trị nhỏ hơn $X$ (hoặc lớn hơn $X$) cũng thỏa mãn. Hàm kiểm tra (Check function) sẽ có dạng True, True, True, False, False... hoặc ngược lại.
- **Ví dụ kinh điển:** Chặt Cây (EKO - SPOJ), có $N$ cái cây với chiều cao $H_i$. Bạn cần lấy được đúng $M$ mét gỗ. Máy cưa được đặt ở độ cao $X$ (những phần ngọn cây cao hơn $X$ sẽ bị cắt rơi xuống). Hỏi độ cao máy cưa $X$ lớn nhất là bao nhiêu để vẫn thu đủ $M$ mét gỗ?
- **Code mẫu C++ (BS trên đáp án):** Cấu trúc code luôn gồm 2 phần: Hàm kiểm tra (Check) và Vòng lặp BS.
```cpp
// Hàm kiểm tra: Với độ cao máy cưa là X, ta có thu ĐỦ M mét gỗ không?
bool kiemTraToiUu(const vector<long long>& cay, int N, long long M, long long do_cao_X) {
    long long tong_go_thu_duoc = 0;
    for (int i = 0; i < N; i++) {
        if (cay[i] > do_cao_X) {
            tong_go_thu_duoc += (cay[i] - do_cao_X);
        }
    }
    return tong_go_thu_duoc >= M; // True nếu thu đủ hoặc dư gỗ
}

// Hàm tìm kiếm nhị phân đáp án
long long timDoCaoMax(const vector<long long>& cay, int N, long long M) {
    long long trai = 0;
    long long phai = 2000000000; // Chiều cao tối đa của cây (theo đề bài)
    long long dap_an = 0;

    while (trai <= phai) {
        long long giua = trai + (phai - trai) / 2;

        if (kiemTraToiUu(cay, N, M, giua)) {
            // Nếu cắt ở độ cao 'giua' mà VẪN ĐỦ gỗ -> Lưu đáp án.
            dap_an = giua;
            // Nhưng ta muốn độ cao máy cưa LỚN NHẤT có thể, nên thử nâng máy cưa lên.
            trai = giua + 1; 
        } else {
            // Nếu KHÔNG ĐỦ gỗ -> Phải hạ máy cưa xuống để cắt được nhiều hơn.
            phai = giua - 1;
        }
    }
    return dap_an;
}
```
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">4. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div>

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Lỗi thường gặp</th><th style="width: 75%;">Cách khắc phục & Lời khuyên</th></tr></thead><tbody><tr><td><b>Chưa sắp xếp mảng</b></td><td>Tìm kiếm nhị phân trên mảng <b>bắt buộc</b> mảng phải có tính thứ tự (tăng/giảm). Nếu học sinh quên gọi <code>sort(A.begin(), A.end())</code> trước khi gọi BS, chương trình sẽ chạy sai hoàn toàn.</td></tr><tr><td><b>Tràn số khi tính <code>mid</code></b></td><td>Dùng công thức <code>int giua = (trai + phai) / 2;</code> sẽ bị tràn kiểu <code>int</code> nếu <code>trai</code> và <code>phai</code> đều xấp xỉ $2 \times 10^9$. <b>Giải pháp:</b> Luôn dùng công thức <code>trai + (phai - trai) / 2</code>.</td></tr><tr><td><b>Lặp vô hạn (Infinite Loop)</b></td><td>Xảy ra khi dùng công thức gán <code>trai = giua</code> hoặc <code>phai = giua</code> mà không có sự cẩn thận về việc làm tròn. Để an toàn, hãy luôn thiết kế theo mẫu cập nhật đáp án ra một biến riêng (như biến <code>dap_an</code> trong code ví dụ) và dời mốc <code>trai = giua + 1</code> hoặc <code>phai = giua - 1</code>.</td></tr><tr><td><b>BS trên số thực (Double)</b></td><td>Nếu đáp án là số thực, ta không dùng <code>trai <= phai</code> mà dùng vòng lặp số lần cố định (vd: <code>for(int i = 0; i < 100; i++)</code>) hoặc dùng sai số <code>while (phai - trai > 1e-9)</code>. Lặp 100 lần là quá đủ để đạt độ chính xác $10^{-18}$ cho kiểu <code>double</code>.</td></tr><tr><td><b>Xây dựng hàm <code>Check</code></b></td><td>Trong dạng <i>Tìm kiếm nhị phân trên đáp án</i>, việc khó nhất là viết hàm <code>kiemTraToiUu()</code>. Ta cứ coi như đáp án đã biết là $X$, làm sao để kiểm tra $X$ có hợp lệ không bằng thuật toán tham lam (Greedy) $O(N)$. Nếu xây dựng được hàm Check trong $O(N)$, bài toán sẽ giải được trong $O(N \log(\text{Max\_Value}))$.</td></tr></tbody></table></div>
</div>