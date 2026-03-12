## ✌️ Kỹ Thuật Lập Trình Hai Con Trỏ (Two Pointers)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

**Khái niệm:** Kỹ thuật Hai con trỏ là phương pháp duyệt qua các cấu trúc dữ liệu tuyến tính (như Mảng, Xâu ký tự, hoặc Danh sách liên kết) bằng cách sử dụng hai biến chỉ số (gọi là con trỏ) di chuyển đồng thời. Thay vì dùng hai vòng lặp lồng nhau $O(N^2)$, hai con trỏ phối hợp với nhau để duyệt mảng chỉ trong $1$ vòng lặp duy nhất, giảm độ phức tạp xuống $O(N)$.

**Đặc điểm nhận diện:** 
- Bài toán yêu cầu xử lý trên mảng hoặc xâu đã được sắp xếp (với dạng Left - Right).
- Yêu cầu tìm các cặp phần tử, đoạn con liên tiếp, hoặc loại bỏ phần tử trùng lặp.
- Cần tối ưu thời gian chạy (Time Limit Exceeded - TLE) khi thuật toán "trâu" (vét cạn $O(N^2)$) không qua được.
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các dạng Hai con trỏ điển hình</div>

### A. Dạng Left - Right (Hai con trỏ ngược chiều)
- **Phân tích thuật toán:** Một con trỏ (trai) xuất phát từ đầu mảng bước dần sang phải, con trỏ kia (phai) xuất phát từ cuối mảng bước dần sang trái. Chúng tiến lại gần nhau cho đến khi gặp nhau hoặc thỏa mãn điều kiện bài toán. Dạng này bắt buộc mảng phải được sắp xếp.
- **Ví dụ:** Tìm cặp số có tổng bằng S (Two Sum in Sorted Array). Cho mảng $A$ đã sắp xếp tăng dần và số $S$. Tìm 2 phần tử có tổng bằng $S$.
- **Áp dụng kỹ thuật:**
    - Nếu $A[trai] + A[phai] == S \rightarrow$ Tìm thấy!
    - Nếu $A[trai] + A[phai] < S \rightarrow$ Tổng đang hơi nhỏ, ta tăng trai lên để lấy số lớn hơn.
    - Nếu $A[trai] + A[phai] > S \rightarrow$ Tổng đang hơi lớn, ta giảm phai xuống để lấy số nhỏ hơn.
- **Code mẫu C++:**
```cpp
void timHaiSoLeftRight(const vector<int>& A, int S) {
    int trai = 0;
    int phai = A.size() - 1;

    while (trai < phai) {
        int tong = A[trai] + A[phai];
        
        if (tong == S) {
            cout << "Tim thay cap so: " << A[trai] << " va " << A[phai] << endl;
            return;
        } else if (tong < S) {
            trai++; // Tăng tổng lên
        } else {
            phai--; // Giảm tổng xuống
        }
    }
    cout << "Khong tim thay." << endl;
}
```

### B. Dạng Slow and Fast (Rùa và Thỏ / Hai con trỏ cùng chiều)
- **Phân tích thuật toán:** Hai con trỏ xuất phát từ cùng một đầu mảng và di chuyển theo cùng một hướng, nhưng với tốc độ khác nhau hoặc điều kiện dừng khác nhau. Thường dùng để loại bỏ phần tử trùng lặp, tìm phần tử ở giữa danh sách, hoặc kiểm tra chu trình.
- **Ví dụ:** Xóa phần tử trùng lặp trong mảng đã sắp xếp. Cho mảng $A$ đã sắp xếp. Chuyển các phần tử duy nhất lên đầu mảng và trả về số lượng phần tử duy nhất đó.
- **Áp dụng kỹ thuật:**
    - Con trỏ cham (slow) dùng để lưu vị trí ghi phần tử duy nhất tiếp theo.
    - Con trỏ nhanh (fast) dùng để quét qua toàn bộ mảng tìm các phần tử mới (khác với phần tử cuối cùng đã ghi).
- **Code mẫu C++:**
```cpp
int xoaTrungLap(vector<int>& A) {
    if (A.empty()) return 0;

    int cham = 0; // Vị trí của phần tử duy nhất cuối cùng
    
    // Con trỏ 'nhanh' duyệt qua mọi phần tử
    for (int nhanh = 1; nhanh < A.size(); nhanh++) {
        // Nếu tìm thấy một số mới chưa có trong nhóm duy nhất
        if (A[nhanh] != A[cham]) {
            cham++;                // Di chuyển con trỏ chậm lên 1 bước
            A[cham] = A[nhanh];    // Ghi nhận phần tử mới
        }
    }
    // Số lượng phần tử duy nhất = chỉ số (cham) + 1
    return cham + 1; 
}
```

</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Dạng Sliding Window (Cửa sổ trượt)</div>

**Cửa sổ trượt** là một biến thể mạnh mẽ của "Hai con trỏ cùng chiều". Hai con trỏ `trai` và `phai` tạo thành một "cửa sổ" (đoạn con liên tiếp $[trai \dots phai]$). Con trỏ phai luôn có xu hướng đi sang phải để mở rộng cửa sổ, trong khi con trỏ trai thu hẹp cửa sổ khi cần thiết.

### 1. Cửa sổ trượt CỐ ĐỊNH (Fixed Window)
- **Phân tích:** Cửa sổ luôn duy trì kích thước bằng $K$. Khi thêm phần tử mới ở con trỏ `phai`, ta lập tức phải loại bỏ phần tử cũ ở con trỏ `trai` để kích thước không đổi.
- **Ví dụ:** Tìm đoạn con liên tiếp có độ dài đúng bằng $K$ có tổng lớn nhất.
```cpp
int maxTongCuaSoCoDinh(const vector<int>& A, int K) {
    int tong_hien_tai = 0, tong_max = 0;
    
    // 1. Khởi tạo cửa sổ ban đầu (tính tổng K phần tử đầu tiên)
    for (int i = 0; i < K; i++) tong_hien_tai += A[i];
    tong_max = tong_hien_tai;
    
    // 2. Trượt cửa sổ
    for (int i = K; i < A.size(); i++) {
        // Cộng phần tử mới vào, trừ phần tử cũ nhất ra
        tong_hien_tai = tong_hien_tai + A[i] - A[i - K];
        tong_max = max(tong_max, tong_hien_tai);
    }
    return tong_max;
}
```

### 2. Cửa sổ trượt THAY ĐỔI (Variable Window)
- **Phân tích:** Cửa sổ tự động co giãn kích thước. Nó cứ mở rộng (tăng phai) cho đến khi vi phạm điều kiện bài toán, sau đó nó co lại (tăng trai) cho đến khi thỏa mãn điều kiện trở lại.
- **Ví dụ:** Tìm đoạn con liên tiếp dài nhất có tổng $\le S$.
```cpp
int doDaiMaxCuaSoThayDoi(const vector<int>& A, int S) {
    int trai = 0, tong_hien_tai = 0;
    int max_len = 0;

    for (int phai = 0; phai < A.size(); phai++) {
        tong_hien_tai += A[phai]; // Mở rộng cửa sổ

        // Nếu tổng vượt quá S, bắt buộc phải thu hẹp cửa sổ từ bên trái
        while (tong_hien_tai > S && trai <= phai) {
            tong_hien_tai -= A[trai];
            trai++; 
        }

        // Cập nhật độ dài lớn nhất đạt được
        max_len = max(max_len, phai - trai + 1);
    }
    return max_len;
}
```
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Dạng Ba con trỏ (Three Pointers) - Biến thể nâng cao</div>

### Phân tích thuật toán

Đây là sự mở rộng của kỹ thuật Hai con trỏ. Thay vì dùng 2 biến, ta sử dụng 3 biến chỉ số để duyệt qua mảng. Kỹ thuật này thường được áp dụng trong 3 tình huống kinh điển:
- **Cố định 1, chạy 2:** Cố định một con trỏ bằng vòng lặp, dùng kỹ thuật Hai con trỏ ngược chiều (Left-Right) cho phần còn lại của mảng.
- **Phân vùng mảng (Partition):** Dùng 3 con trỏ để chia mảng thành 3 khu vực khác nhau (Ví dụ: Nhỏ hơn, Bằng, Lớn hơn).
- **Duyệt 3 mảng:** Tìm phần tử chung của 3 mảng đã sắp xếp.

### Ví dụ Kinh điển
1. **Bài toán 3Sum (Tìm bộ 3 số có tổng bằng S):** Cho mảng $A$ và số $S$. Tìm 3 phần tử có tổng đúng bằng $S$ trong thời gian $O(N^2)$ (thay vì vét cạn $O(N^3)$).
    - Ý tưởng: 
        - Bước 1: Sắp xếp mảng tăng dần.
        - Bước 2: Dùng vòng lặp for để cố định con trỏ thứ nhất (i).
        - Bước 3: Với mỗi i, bài toán quay về dạng Tìm 2 số có tổng bằng $S - A[i]$ trong đoạn từ i + 1 đến cuối mảng (Dùng 2 con trỏ trai và phai).
    - Code mẫu C++ (3Sum):
```cpp    
void timBaSo(vector<int>& A, int S) {
    // Bắt buộc sắp xếp mảng
    sort(A.begin(), A.end()); 
    int N = A.size();

    // Con trỏ 1: Cố định i
    for (int i = 0; i < N - 2; i++) {
        // Bỏ qua các giá trị i trùng lặp để tránh in kết quả giống nhau
        if (i > 0 && A[i] == A[i-1]) continue;

        int trai = i + 1;      // Con trỏ 2
        int phai = N - 1;      // Con trỏ 3
        int muc_tieu = S - A[i];

        while (trai < phai) {
            int tong_hai_so = A[trai] + A[phai];
            
            if (tong_hai_so == muc_tieu) {
                cout << "Bo ba: " << A[i] << ", " << A[trai] << ", " << A[phai] << endl;
                return; // Tìm thấy 1 bộ thì dừng (hoặc tiếp tục tăng trai, giảm phai nếu muốn tìm hết)
            } else if (tong_hai_so < muc_tieu) {
                trai++;
            } else {
                phai--;
            }
        }
    }
    cout << "Khong tim thay." << endl;
}
```
2. **Bài toán cờ Hà Lan (Dutch National Flag / Sort Colors):** Cho một mảng chỉ chứa các số $0, 1$ và $2$ (tương ứng với 3 màu Đỏ, Trắng, Xanh). Hãy sắp xếp mảng này trong $O(N)$ thời gian và chỉ duyệt mảng đúng $1$ lần (Không dùng hàm sort).
    - Ý tưởng: Dùng 3 con trỏ:
        - thap (low): Ranh giới của vùng chứa số $0$.
        - giua (mid): Con trỏ duyệt mảng.
        - cao (high): Ranh giới của vùng chứa số $2$.
    - Nguyên tắc: Quét giua từ trái sang phải. Gặp $0$ thì đẩy về đầu (đổi chỗ với thap). Gặp $2$ thì đẩy về cuối (đổi chỗ với cao). Gặp $1$ thì để yên và đi tiếp.
    - Code mẫu C++ (Sort 0, 1, 2):
```cpp
void sapXepBaMau(vector<int>& A) {
    int thap = 0;
    int giua = 0;
    int cao = A.size() - 1;

    while (giua <= cao) {
        if (A[giua] == 0) {
            // Gặp số 0: Đổi chỗ cho con trỏ 'thap', tăng cả 2 con trỏ
            swap(A[thap], A[giua]);
            thap++;
            giua++;
        } 
        else if (A[giua] == 1) {
            // Gặp số 1: Đúng vị trí ở giữa rồi, chỉ cần đi tiếp
            giua++;
        } 
        else { // A[giua] == 2
            // Gặp số 2: Đẩy về cuối mảng bằng cách đổi chỗ cho con trỏ 'cao'
            swap(A[giua], A[cao]);
            cao--; 
            // Lưu ý: Không tăng 'giua' ở đây vì số vừa được đổi từ 'cao' về có thể là 0, 1 hoặc 2, cần kiểm tra lại ở vòng lặp sau.
        }
    }
}
```
</div>

</div><div class="step-card border-red"><div class="step-badge bg-red">4. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div><div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Lỗi thường gặp</th><th style="width: 75%;">Phân tích & Cách khắc phục</th></tr></thead><tbody><tr><td><b>Quên điều kiện "Mảng đã sắp xếp"</b></td><td>Dạng <i>Left-Right</i> (kiểu <code>A[trai] + A[phai]</code>) <b>bắt buộc</b> mảng phải có tính thứ tự. Nếu đề chưa cho mảng sắp xếp, ta phải tự gọi <code>sort()</code> trước khi dùng 2 con trỏ, nếu không kết quả sẽ sai hoàn toàn.</td></tr><tr><td><b>Lỗi tràn chỉ số (Index Out of Bounds)</b></td><td>Trong vòng lặp <code>while</code> của Sliding Window, khi co cửa sổ (tăng <code>trai</code>), học sinh hay quên kiểm tra điều kiện <code>trai <= phai</code> hoặc <code>trai < N</code>, dẫn đến truy cập ngoài bộ nhớ mảng.</td></tr><tr><td><b>Lặp vô hạn (Infinite Loop)</b></td><td>Khi dùng vòng lặp <code>while (trai < phai)</code>, luôn phải đảm bảo bên trong thân vòng lặp ít nhất một trong hai con trỏ <code>trai++</code> hoặc <code>phai--</code> phải được thực thi ở mọi trường hợp nhánh <code>if-else</code>.</td></tr><tr><td><b>Khởi tạo giá trị Max/Min</b></td><td>Với các bài tìm Max/Min trong cửa sổ, nếu có số âm, biến <code>tong_max</code> không được khởi tạo bằng <code>0</code>, mà phải khởi tạo bằng một số vô cùng bé (như <code>-1e9</code>) để tránh bị sai kết quả.</td></tr>
<tr><td><b>Sliding Window với số âm</b></td><td>Kỹ thuật Sliding Window tìm đoạn con $\le S$ chỉ áp dụng đúng khi <b>tất cả phần tử trong mảng đều không âm</b>. Nếu mảng có số âm, việc trừ bớt <code>A[trai]</code> không đảm bảo tổng sẽ giảm xuống. Lúc này phải dùng Kỹ thuật Mảng cộng dồn (Prefix Sum) kết hợp với Tìm kiếm nhị phân hoặc cấu trúc dữ liệu khác.</td></tr>
<tr><td><b>Quên loại bỏ phần tử trùng lặp (Bài 3Sum)</b></td><td>Nếu mảng có nhiều số giống nhau (vd: $[1, 1, 2, 2, ...]$), thuật toán 3Sum rất dễ in ra các bộ 3 giống y hệt nhau. <b>Cách khắc phục:</b> Thêm các câu lệnh bỏ qua phần tử trùng như `if (i > 0 && A[i] == A[i-1]) continue;` và làm tương tự cho 2 con trỏ trai, phai sau khi tìm thấy 1 nghiệm.</td></tr>
</tbody></table></div>