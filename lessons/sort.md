## 🗂️ Kỹ Thuật Sắp Xếp (Sorting Algorithms)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Sử dụng hàm sort() trong C++</div>

Trong **99%** các bài toán lập trình thi đấu, học sinh không cần (và không nên) tự viết lại các thuật toán sắp xếp mà hãy sử dụng ngay hàm `std::sort()` tích hợp sẵn trong thư viện <algorithm>. 

Hàm này sử dụng thuật toán IntroSort (kết hợp giữa Quick Sort, Heap Sort và Insertion Sort), đảm bảo tốc độ luôn là $O(N \log N)$ trong mọi trường hợp.

**Cú pháp cơ bản:**
- Mảng thường (Array): `sort(A, A + N);`
- Vector: `sort(A.begin(), A.end());`
- Sắp xếp giảm dần (dùng thư viện ngoài): `sort(A.begin(), A.end(), greater<int>());`

**Xây dựng hàm so sánh ngoài (Custom Comparator):** Khi cần sắp xếp một cấu trúc dữ liệu phức tạp (Struct/Class) dựa trên nhiều tiêu chí, ta phải tự định nghĩa quy tắc sắp xếp bằng một hàm bool (hoặc biểu thức Lambda).

Ví dụ: Sắp xếp danh sách Học sinh:
- Ưu tiên 1: Điểm Toán giảm dần.
- Ưu tiên 2: Nếu điểm Toán bằng nhau, sắp xếp theo Tên theo thứ tự từ điển (tăng dần ABC).
```cpp
struct HocSinh {
    string ten;
    int diem_toan;
};

// Hàm so sánh tùy chỉnh (Custom Comparator)
bool tieuChiSapXep(const HocSinh& a, const HocSinh& b) {
    if (a.diem_toan != b.diem_toan) {
        return a.diem_toan > b.diem_toan; // Tiêu chí 1: Điểm Toán giảm dần (Lớn đứng trước)
    }
    return a.ten < b.ten;                 // Tiêu chí 2: Tên tăng dần (Bé đứng trước)
}

int main() {
    vector<HocSinh> ds = {{"An", 8}, {"Binh", 9}, {"Chau", 8}};
    
    // Gọi hàm sort truyền theo tên hàm so sánh
    sort(ds.begin(), ds.end(), tieuChiSapXep);
    
    // Kết quả mong đợi: Binh (9), An (8), Chau (8)
    return 0;
}
```
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các thuật toán sắp xếp kinh điển</div>

Dù có hàm `sort()`, học sinh vẫn cần hiểu các thuật toán cơ bản để phục vụ cho các bài toán đếm nghịch thế hoặc tư duy chia để trị.

### A. Bubble Sort (Sắp xếp nổi bọt)
- **Phân tích:** Duyệt qua mảng nhiều lần. Ở mỗi lần duyệt, so sánh 2 phần tử kề nhau, nếu chúng đứng sai thứ tự thì đổi chỗ. Phần tử lớn nhất sẽ "nổi" dần về cuối mảng như bọt khí.
- **Độ phức tạp:** $O(N^2)$.
```cpp
void bubbleSort(vector<int>& A) {
    int N = A.size();
    for (int i = 0; i < N - 1; i++) {
        bool da_doi_cho = false;
        for (int j = 0; j < N - i - 1; j++) {
            if (A[j] > A[j + 1]) {
                swap(A[j], A[j + 1]);
                da_doi_cho = true;
            }
        }
        if (!da_doi_cho) break; // Tối ưu: Nếu mảng đã tăng dần thì dừng sớm
    }
}
```

### B. Selection Sort (Sắp xếp chọn)
- **Phân tích:** Chia mảng thành 2 phần: đã sắp xếp và chưa sắp xếp. Mỗi bước, tìm phần tử nhỏ nhất trong phần chưa sắp xếp và đưa nó về cuối phần đã sắp xếp.
- **Độ phức tạp:** $O(N^2)$.
```cpp
void selectionSort(vector<int>& A) {
    int N = A.size();
    for (int i = 0; i < N - 1; i++) {
        int vi_tri_min = i;
        for (int j = i + 1; j < N; j++) {
            if (A[j] < A[vi_tri_min]) {
                vi_tri_min = j; // Ghi nhận vị trí nhỏ nhất
            }
        }
        swap(A[i], A[vi_tri_min]); // Đưa số nhỏ nhất về đúng vị trí
    }
}
```
### C. Insertion Sort (Sắp xếp chèn)
- **Phân tích:** Lấy từng phần tử ở phần chưa sắp xếp, tìm vị trí thích hợp và "chèn" nó vào phần đã sắp xếp (giống như cách chúng ta xếp bài trên tay). Rất nhanh nếu mảng đã gần như sắp xếp sẵn.
- **Độ phức tạp:** $O(N^2)$ (Tốt nhất $O(N)$).
```cpp
void insertionSort(vector<int>& A) {
    int N = A.size();
    for (int i = 1; i < N; i++) {
        int x = A[i];
        int j = i - 1;
        // Dời các phần tử lớn hơn x sang phải 1 vị trí
        while (j >= 0 && A[j] > x) {
            A[j + 1] = A[j];
            j--;
        }
        A[j + 1] = x; // Chèn x vào vị trí tìm được
    }
}
```

### D. Merge Sort (Sắp xếp trộn)
- **Phân tích:** Áp dụng tư tưởng Chia để trị (Divide and Conquer). Chia đôi mảng liên tục cho đến khi mỗi mảng chỉ còn 1 phần tử. Sau đó "Trộn" (Merge) 2 mảng con đã sắp xếp thành 1 mảng lớn hơn đã sắp xếp.
- **Độ phức tạp:** $O(N \log N)$ ở mọi trường hợp. Cần thêm mảng phụ $O(N)$.
```cpp
// Hàm trộn 2 nửa [L, mid] và [mid+1, R]
void merge(vector<int>& A, int L, int mid, int R) {
    vector<int> tam(R - L + 1);
    int i = L, j = mid + 1, k = 0;
    while (i <= mid && j <= R) {
        if (A[i] <= A[j]) tam[k++] = A[i++];
        else tam[k++] = A[j++];
    }
    while (i <= mid) tam[k++] = A[i++];
    while (j <= R) tam[k++] = A[j++];
    for (int p = 0; p < k; p++) A[L + p] = tam[p];
}

void mergeSort(vector<int>& A, int L, int R) {
    if (L >= R) return;
    int mid = L + (R - L) / 2;
    mergeSort(A, L, mid);       // Sắp xếp nửa trái
    mergeSort(A, mid + 1, R);   // Sắp xếp nửa phải
    merge(A, L, mid, R);        // Trộn lại
}
```

### E. Quick Sort (Sắp xếp nhanh)
- **Phân tích:** Cũng dùng Chia để trị. Chọn 1 phần tử làm "Chốt" (Pivot). Đưa tất cả các số nhỏ hơn chốt về bên trái, các số lớn hơn chốt về bên phải. Sau đó gọi đệ quy sắp xếp tiếp 2 bên.
- **Độ phức tạp:** Trung bình $O(N \log N)$. Xấu nhất $O(N^2)$ (nếu chọn chốt không tốt).
```cpp
void quickSort(vector<int>& A, int L, int R) {
    if (L >= R) return;
    int chot = A[L + (R - L) / 2]; // Chọn chốt ở giữa
    int i = L, j = R;
    while (i <= j) {
        while (A[i] < chot) i++;
        while (A[j] > chot) j--;
        if (i <= j) {
            swap(A[i], A[j]);
            i++; j--;
        }
    }
    quickSort(A, L, j); // Sắp xếp nửa trái
    quickSort(A, i, R); // Sắp xếp nửa phải
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Bảng so sánh các thuật toán sắp xếp</div>

***Tính ổn định (Stable): Nếu 2 phần tử có giá trị bằng nhau, thuật toán Stable sẽ giữ nguyên thứ tự xuất hiện ban đầu của chúng.***

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 15%;">Thuật toán</th><th style="width: 20%;">TG Tốt nhất</th><th style="width: 20%;">TG Trung bình</th><th style="width: 20%;">TG Xấu nhất</th><th style="width: 15%;">Bộ nhớ</th><th style="width: 10%;">Ổn định</th></tr></thead><tbody><tr><td><b>Bubble Sort</b></td><td>$O(N)$</td><td>$O(N^2)$</td><td>$O(N^2)$</td><td>$O(1)$</td><td style="color: #16a34a; font-weight: bold;">Có</td></tr><tr style="background-color: #f8fafc;"><td><b>Selection Sort</b></td><td>$O(N^2)$</td><td>$O(N^2)$</td><td>$O(N^2)$</td><td>$O(1)$</td><td style="color: #ea580c; font-weight: bold;">Không</td></tr><tr><td><b>Insertion Sort</b></td><td>$O(N)$</td><td>$O(N^2)$</td><td>$O(N^2)$</td><td>$O(1)$</td><td style="color: #16a34a; font-weight: bold;">Có</td></tr><tr style="background-color: #f8fafc;"><td><b>Merge Sort</b></td><td>$O(N \log N)$</td><td>$O(N \log N)$</td><td>$O(N \log N)$</td><td>$O(N)$</td><td style="color: #16a34a; font-weight: bold;">Có</td></tr><tr><td><b>Quick Sort</b></td><td>$O(N \log N)$</td><td>$O(N \log N)$</td><td>$O(N^2)$</td><td>$O(\log N)$</td><td style="color: #ea580c; font-weight: bold;">Không</td></tr></tbody></table></div>
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Ứng dụng của Thuật toán Sắp xếp</div>

***Sắp xếp thường không phải là đáp án cuối cùng của bài toán, mà là bước tiền xử lý cực kỳ quan trọng để mở khóa các thuật toán tối ưu khác:***
- **Tìm kiếm nhị phân (Binary Search):** Thuật toán tìm kiếm $O(\log N)$ bắt buộc mảng phải được sắp xếp trước.
- **Kỹ thuật Tham lam (Greedy):** Luôn ưu tiên chọn các phần tử Lớn nhất/Nhỏ nhất trước (Bài toán Đổi tiền, Xếp lịch).
- **Kỹ thuật Hai con trỏ (Two Pointers):** Các bài toán tìm 2 số có tổng bằng $S$ ($O(N)$) yêu cầu mảng phải được sắp xếp.
- **Loại bỏ phần tử trùng lặp:** Sau khi sort(), các phần tử giống nhau sẽ nằm cạnh nhau. Dùng hàm unique(A.begin(), A.end()) của C++ để dồn chúng lại và xóa đi trong $O(N)$.
- **Bài toán khoảng cách gần nhất:** Cặp điểm (hoặc số) có khoảng cách nhỏ nhất trên trục tọa độ chắc chắn phải là 2 phần tử nằm kề nhau sau khi mảng đã sắp xếp.
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">5. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div>
<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Vấn đề / Lỗi thường gặp</th><th style="width: 75%;">Phân tích & Kinh nghiệm khắc phục</th></tr></thead><tbody><tr><td><b>Lỗi So sánh sai (Strict Weak Ordering)</b></td><td>Khi tự viết hàm <code>bool tieuChiSapXep(a, b)</code>, học sinh rất hay viết <code>return a <= b;</code>. Đây là lỗi <b>CHẾT NGƯỜI</b> khiến hàm <code>sort()</code> của C++ bị lặp vô hạn hoặc Runtime Error (Segfault).<b>Khắc phục:</b> Bắt buộc chỉ dùng dấu <code>&lt;</code> hoặc <code>&gt;</code>. Nếu <code>a</code> và <code>b</code> bằng nhau, hàm bắt buộc phải trả về <code>false</code>.</td></tr><tr><td><b>Quên dùng <code>stable_sort</code></b></td><td>Hàm <code>sort()</code> thông thường là Unstable (giống Quick Sort). Nếu bài toán yêu cầu: "Sắp xếp theo điểm, ai nộp bài trước thì xếp trước", ta bắt buộc phải dùng <code>stable_sort(A.begin(), A.end(), cmp);</code> để giữ nguyên thứ tự ban đầu của những người bằng điểm.</td></tr><tr><td><b>Truyền tham chiếu <code>const &</code></b></td><td>Trong hàm Comparator, nếu sắp xếp cấu trúc dữ liệu lớn (như String dài, hoặc Struct nhiều trường), luôn phải truyền tham số theo kiểu <code>const HocSinh&amp; a</code>. Nếu chỉ ghi <code>HocSinh a</code>, máy tính sẽ phải copy lại toàn bộ chuỗi mỗi lần so sánh, dẫn đến TLE cực nặng.</td></tr><tr><td><b>Đếm số nghịch thế (Inversion Count)</b></td><td>Bài toán <i>"Có bao nhiêu cặp $i &lt; j$ mà $A[i] &gt; A[j]$"</i> là một bài kinh điển. Không thể dùng hàm <code>sort()</code> có sẵn để đếm. Học sinh <b>bắt buộc</b> phải tự code thuật toán <b>Merge Sort</b>, vì số lượng nghịch thế chính xác bằng số lượng phần tử nhảy qua mặt nhau trong bước Trộn (Merge).</td></tr></tbody></table></div>
</div>