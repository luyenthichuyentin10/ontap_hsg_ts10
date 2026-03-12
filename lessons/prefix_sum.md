## ➕ Kỹ Thuật Mảng Cộng Dồn (Prefix Sum 1D & 2D)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Mảng Cộng Dồn 1 Chiều (1D Prefix Sum)</div>

**Khái niệm:** 
- Mảng cộng dồn 1 chiều là mảng phụ (thường đặt tên là pre), trong đó phần tử `pre[i]` lưu tổng của tất cả các phần tử từ vị trí đầu tiên đến vị trí $i$ trong mảng gốc $A$.
- Mục đích chính: Trả lời nhanh truy vấn "Tính tổng các phần tử từ vị trí $L$ đến vị trí $R$.

**Công thức cốt lõi:**
- Xây dựng mảng: 
<div class="math-formula"> 

`pre[i] = pre[i - 1] + A[i]`
</div>

- Truy vấn tổng đoạn $[L, R]$: 
<div class="math-formula"> 

`Tong(L, R) = pre[R] - pre[L - 1]` 
<div class="formula-notes">Giải thích: Tổng từ $L$ đến $R$ bằng tổng từ $1$ đến $R$ trừ đi phần không lấy là từ $1$ đến $L-1$. </div>
</div>

**Code mẫu C++ (Mảng cộng dồn 1 chiều):**
```cpp
int main() {
    int N = 5;
    // Mảng gốc A (chỉ số 1 đến 5), A[0] = 0 để làm mốc
    vector<int> A = {0, 3, 1, 4, 2, 5}; 
    
    // 1. Khai báo và xây dựng mảng cộng dồn
    vector<long long> pre(N + 1, 0);
    for (int i = 1; i <= N; i++) {
        pre[i] = pre[i - 1] + A[i];
    }
    
    // 2. Truy vấn: Tính tổng từ L = 2 đến R = 4 (Kỳ vọng: 1 + 4 + 2 = 7)
    int L = 2, R = 4;
    long long tong_doan = pre[R] - pre[L - 1];
    
    cout << "Tong tu " << L << " den " << R << " la: " << tong_doan << endl;
    
    return 0;
}
```
<div class="important-note">

**Lưu ý:** Bắt buộc dùng chỉ số mảng bắt đầu từ 1 (1-based indexing) để tránh lỗi số âm khi gọi L - 1 (nếu $L=0$)
</div>
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">2. Mảng Cộng Dồn 2 Chiều (2D Prefix Sum)</div>

**Phân tích thuật toán:** 
- Mở rộng lên không gian 2 chiều (Ma trận), `pre[i][j]` sẽ lưu tổng của toàn bộ hình chữ nhật con có góc trên cùng bên trái là $(1, 1)$ và góc dưới cùng bên phải là $(i, j)$. 
- Kỹ thuật này dựa trên nguyên lý Bù trừ (Inclusion-Exclusion).

**Công thức cốt lõi:** Giả sử ta có ma trận gốc $A$.
- Xây dựng mảng: 
<div class="math-formula"> 

`pre[i][j] = pre[i-1][j] + pre[i][j-1] - pre[i-1][j-1] + A[i][j]` 
<div class="formula-notes">Giải thích: Tổng Hcn = Hcn phía trên + Hcn phía trái - Hcn bị giao nhau (cộng 2 lần nên phải trừ đi 1 lần) + Ô hiện tại. </div>
</div>

- Truy vấn tổng hcn từ $(R1, C1)$ đến $(R2, C2)$: 
<div class="math-formula">

`Tong = pre[R2][C2] - pre[R1-1][C2] - pre[R2][C1-1] + pre[R1-1][C1-1]`
</div>

**Code mẫu C++ (Mảng cộng dồn 2 chiều):**
```cpp
int main() {
    int Hang = 3, Cot = 4;
    // Ma trận gốc A (dùng 1-based indexing, chèn hàng 0 và cột 0 bằng 0)
    vector<vector<int>> A = {
        {0, 0, 0, 0, 0},
        {0, 1, 2, 3, 4},
        {0, 5, 6, 7, 8},
        {0, 9, 10, 11, 12}
    };
    
    // 1. Xây dựng mảng cộng dồn 2 chiều
    vector<vector<long long>> pre(Hang + 1, vector<long long>(Cot + 1, 0));
    for (int i = 1; i <= Hang; i++) {
        for (int j = 1; j <= Cot; j++) {
            pre[i][j] = pre[i-1][j] + pre[i][j-1] - pre[i-1][j-1] + A[i][j];
        }
    }
    
    // 2. Truy vấn: Tính tổng hình chữ nhật từ góc (2, 2) đến (3, 4)
    int R1 = 2, C1 = 2, R2 = 3, C2 = 4;
    long long tong_hcn = pre[R2][C2] - pre[R1-1][C2] - pre[R2][C1-1] + pre[R1-1][C1-1];
    
    cout << "Tong hinh chu nhat la: " << tong_hcn << endl;
    // Kỳ vọng: 6 + 7 + 8 + 10 + 11 + 12 = 54
    
    return 0;
}
```
</div>

<div class="step-card border-red"><div class="step-badge bg-red">3. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div><div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Vấn đề / Lỗi thường gặp</th><th style="width: 75%;">Phân tích & Kinh nghiệm</th></tr></thead><tbody><tr><td><b>Lỗi tràn bộ nhớ (Out of Bounds)</b></td><td>Nếu dùng mảng bắt đầu từ $0$, khi truy vấn <code>pre[R] - pre[L-1]</code> với $L = 0$, ta sẽ gọi <code>pre[-1]</code> gây lỗi thoát chương trình.<b>Khuyên dùng:</b> Bắt buộc dạy học sinh dùng <b>1-based indexing (chỉ số mảng bắt đầu từ 1)</b> đối với mọi bài toán Mảng cộng dồn. Phần tử số 0 luôn gán bằng 0 làm "mốc".</td></tr><tr><td><b>Lỗi tràn kiểu dữ liệu (Overflow)</b></td><td>Mảng $A$ có thể chứa số <code>int</code>, nhưng Mảng <code>pre</code> phải luôn được khai báo là <code>long long</code>. Vì tổng của $10^5$ phần tử có giá trị $10^9$ sẽ đạt $10^{14}$, gây tràn số nguyên <code>int</code> ($2 \times 10^9$).</td></tr><tr><td><b>Dấu hiệu nhận biết bài toán</b></td><td>Khi đề bài cho một dãy số/ma trận tĩnh (không cập nhật thêm phần tử) và có rất nhiều truy vấn $Q$ (vd: $Q = 10^5$) yêu cầu tính <b>Tổng đoạn</b>, <b>Đếm số lượng</b> phần tử thỏa mãn điều kiện trong đoạn.</td></tr><tr><td><b>Kết hợp với Hash Map / Mảng đếm</b></td><td>Một bài kinh điển: <i>"Tìm đoạn con có tổng bằng K"</i>. Thay vì dùng 2 vòng lặp, ta duyệt mảng tiền tố và dùng Map để tìm xem giá trị <code>pre[i] - K</code> đã xuất hiện trước đó chưa. Đây là kỹ thuật cực mạnh.</td></tr></tbody></table></div>