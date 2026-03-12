## ➖ Kỹ Thuật Mảng Hiệu (Difference Array)
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

**Khái niệm:** Mảng hiệu (Difference Array) là một mảng phụ $D$ được tạo ra từ mảng gốc $A$, trong đó mỗi phần tử $D[i]$ lưu độ chênh lệch (hiệu số) giữa phần tử $A[i]$ và phần tử ngay trước nó $A[i-1]$.

**Công thức định nghĩa:** `D[i] = A[i] - A[i-1]`

**Mục đích sử dụng:**
- Giải quyết bài toán: Cập nhật tăng/giảm giá trị trên một đoạn liên tiếp. Giả sử đề bài yêu cầu cộng thêm giá trị $V$ vào tất cả các phần tử từ vị trí $L$ đến $R$.
    - Nếu dùng vòng lặp for chạy từ $L$ đến $R$, ta mất thời gian $O(N)$ cho mỗi lần cập nhật.
    - Nếu dùng Mảng hiệu, ta chỉ cần tác động vào đúng 2 vị trí (điểm đầu và điểm ngay sau điểm cuối) trong thời gian $O(1)$!
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Phân tích thuật toán & Công thức cốt lõi (1 Chiều)</div>

***Để làm chủ Mảng hiệu, học sinh cần nắm vững quy trình 3 bước sau (bắt buộc dùng mảng chỉ số từ 1 - 1-based indexing):***

**Bước 1: Khởi tạo mảng hiệu $D$ từ mảng $A$**
- `D[i] = A[i] - A[i-1]` (Với $A[0] = 0$)

**Bước 2: Cập nhật đoạn $[L, R]$ với giá trị $V$**

Thay vì cộng $V$ cho mọi phần tử từ $L$ đến $R$, ta chỉ làm 2 phép toán:
- `D[L] = D[L] + V`  (Đánh dấu điểm bắt đầu tăng lên $V$)
- `D[R+1] = D[R+1] - V` (Đánh dấu điểm kết thúc hiệu ứng, từ $R+1$ trở đi không được cộng $V$ nữa)

**Bước 3: Khôi phục lại mảng $A$ sau tất cả các lần cập nhật**

Mảng $A$ chính là **mảng cộng dồn** của Mảng hiệu $D$. Ta quét qua mảng $D$ một lần:
- `A[i] = A[i-1] + D[i]` 

***Cập nhật đoạn [L, R] bằng cách chỉ thay đổi giá trị tại chỉ số L và R+1, sau đó khôi phục lại mảng gốc bằng kỹ thuật cộng dồn (prefix sum)***
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Ví dụ minh họa & Code mẫu C++</div>

**Bài toán:** Cho mảng $A$ gồm 5 phần tử ban đầu đều bằng $0$. Có 2 truy vấn cập nhật:
- Cộng $10$ vào đoạn từ $2$ đến $4$.
- Trừ $5$ vào đoạn từ $3$ đến $5$.Hãy in ra mảng $A$ sau khi thực hiện xong tất cả truy vấn.

**Minh họa chạy tay:**

Mảng ban đầu: $A = \{0, 0, 0, 0, 0\}$, kích thước $N = 5$. Mảng hiệu $D$ cũng toàn $0$.
- Truy vấn 1 (Cộng 10 vào $[2, 4]$): D[2] += 10, D[5] -= 10.$\rightarrow D = \{0, 10, 0, 0, -10\}$
- Truy vấn 2 (Trừ 5 vào $[3, 5]$): D[3] -= 5, D[6] += 5.$\rightarrow D = \{0, 10, -5, 0, -10\}$  (D[6] bỏ qua vì ngoài mảng)
- Khôi phục $A$:
    - A[1] = A[0] + D[1] = 0 + 0 = 0
    - A[2] = A[1] + D[2] = 0 + 10 = 10
    - A[3] = A[2] + D[3] = 10 - 5 = 5
    - A[4] = A[3] + D[4] = 5 + 0 = 5
    - A[5] = A[4] + D[5] = 5 - 10 = -5
    - $\rightarrow$ Kết quả: $A = \{0, 10, 5, 5, -5\}$. Chuẩn xác!

**Code mẫu C++:**
```cpp
int main() {
    int N = 5;
    // Khởi tạo mảng hiệu D (Cần N + 2 phần tử để an toàn cho D[R+1])
    vector<long long> D(N + 2, 0); 
    
    // Hàm thực hiện cập nhật đoạn [L, R] thêm V
    auto capNhatDoan = [&](int L, int R, long long V) {
        D[L] += V;
        D[R + 1] -= V;
    };

    // Thực hiện các truy vấn trong O(1)
    capNhatDoan(2, 4, 10);
    capNhatDoan(3, 5, -5);

    // Mảng A để khôi phục kết quả
    vector<long long> A(N + 1, 0);

    cout << "Mang A sau khi cap nhat: ";
    for (int i = 1; i <= N; i++) {
        // Khôi phục bằng Mảng cộng dồn
        A[i] = A[i - 1] + D[i]; 
        cout << A[i] << " ";
    }
    cout << endl;

    return 0;
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">4. Mảng Hiệu 2 Chiều (2D Difference Array)</div>

**Phân tích**

Nếu Mảng cộng dồn 2 chiều dùng nguyên lý bù trừ (Inclusion-Exclusion) thì Mảng hiệu 2 chiều cũng vậy! 

Để cộng thêm giá trị $V$ vào một hình chữ nhật con từ góc trái trên $(R_1, C_1)$ đến góc phải dưới $(R_2, C_2)$, ta thao tác trên 4 điểm của mảng hiệu $D$:
- `D[R1][C1] += V` (Bắt đầu vùng tăng)
- `D[R2 + 1][C1] -= V` (Triệt tiêu phần thừa phía dưới)
- `D[R1][C2 + 1] -= V` (Triệt tiêu phần thừa phía bên phải)
- `D[R2 + 1][C2 + 1] += V` (Cộng lại phần góc chéo dưới bị trừ lặp 2 lần)

Sau khi thực hiện mọi truy vấn, ta khôi phục mảng $A$ bằng công thức tính Mảng cộng dồn 2 chiều tiêu chuẩn.

**Ví dụ minh họa** 
Cho một mảnh đất (ma trận) kích thước $3 \times 3$ ban đầu độ cao các ô đều bằng $0$.Thực hiện 2 thao tác đắp đất:
- Cộng thêm $5$ vào vùng chữ nhật từ ô $(1, 1)$ đến $(2, 2)$.
- Trừ đi $2$ vào vùng chữ nhật từ ô $(2, 2)$ đến $(3, 3)$.
- In ra độ cao của mảnh đất sau khi thực hiện xong các thao tác.

**Minh họa chạy tay:**
- Ban đầu, ta có ma trận gốc $A$ kích thước $3 \times 3$ toàn số $0$.
- Mảng hiệu $D$ được khởi tạo kích thước $4 \times 4$ (chỉ số từ $1$ đến $4$) toàn số $0$ để chứa các mốc đánh dấu.
    - **Bước 1: Xử lý Truy vấn 1**
    - Yêu cầu: Cộng $5$ vào vùng $(1, 1)$ đến $(2, 2)$.
    - Tác động vào 4 điểm của mảng hiệu $D$:
        - D[1][1] += 5 (Góc trái trên)
        - D[3][1] -= 5 (Chặn ranh giới dưới)
        - D[1][3] -= 5 (Chặn ranh giới phải)
        - D[3][3] += 5 (Bù lại góc chéo dưới)
    - **Bước 2: Xử lý Truy vấn 2**
    - Yêu cầu: Trừ $2$ (cộng $-2$) vào vùng $(2, 2)$ đến $(3, 3)$.
    - Tác động vào 4 điểm của mảng hiệu $D$:
        - D[2][2] += -2
        - D[4][2] -= -2 $\rightarrow$ thành $+2$
        - D[2][4] -= -2 $\rightarrow$ thành $+2$
        - D[4][4] += -2
    - 👉 Trạng thái của mảng hiệu $D$ sau 2 truy vấn: (Các ô không ghi gì mặc định là 0)
    | Mảng D | Cột 1 | Cột 2 | Cột 3 | Cột 4 |
    | :---: | :---: | :---: | :---: | :---: |
    | Hàng 1 | 5 | 0 | -5 | 0 |
    | Hàng 2 | 0 | -2 | 0 | 2 |
    | Hàng 3 | -5 | 0 | 5 | 0 |
    | Hàng 4 | 0 | 2 | 0 | -2 |
    - **Bước 3: Khôi phục lại mảng gốc $A$ (Dùng cộng dồn 2D):** 
        - Công thức: A[i][j] = A[i-1][j] + A[i][j-1] - A[i-1][j-1] + D[i][j]
        - Tính ô $A[1][1]$: A[1][1] = 0 + 0 - 0 + D[1][1] $= 0 + 5 =$ $5$ (Đúng, vì nằm trong vùng cộng 5)
        - Tính ô $A[1][2]$ (Mực loang sang phải):
            - A[1][2] = A[0][2] + A[1][1] - A[0][1] + D[1][2]
            - A[1][2] = 0 + 5 - 0 + 0 = $5$ (Đúng, vùng cộng 5 loang sang)
        - Tính ô $A[1][3]$ (Bị chặn bởi biên phải):
            - A[1][3] = A[0][3] + A[1][2] - A[0][2] + D[1][3]
            - A[1][3] = 0 + 5 - 0 + (-5) = $0$ (Chính xác, giá trị 5 đã bị triệt tiêu bởi số -5 ở biên)
        - Tính ô $A[2][2]$ (Điểm giao thoa 2 truy vấn):
            - A[2][2] = A[1][2] + A[2][1] - A[1][1] + D[2][2]
            - A[2][2] = 5 + 5 - 5 + (-2) = $3$ (Tuyệt vời! Ô này được cộng 5 từ truy vấn 1, rồi bị trừ 2 từ truy vấn 2 $\rightarrow$ Kết quả là 3).
        - Tính ô $A[3][3]$ (Điểm cần bù góc chéo):
            - A[3][3] = A[2][3] + A[3][2] - A[2][2] + D[3][3]
            - A[3][3] = -2 + (-2) - 3 + 5 = $-2$ (Chính xác! Chỉ nằm trong vùng bị trừ 2 của truy vấn 2).
    - 👉 Kết quả mảng $A$ cuối cùng ($3 \times 3$):
    | Mảng A | Cột 1 | Cột 2 | Cột 3 |
    | :---: | :---: | :---: | :---: |
    | Hàng 1 | 5 | 5 | 0 |
    | Hàng 2 | 5 | 3 | -2 |
    | Hàng 3 | 0 | -2 | -2 |

**Code mẫu C++**
```cpp
int main() {
    int Hang = 3, Cot = 3;
    
    // 1. Khai báo Mảng hiệu 2 chiều D
    // Kích thước (Hang + 2) x (Cot + 2) để tránh lỗi tràn chỉ số tại R2+1 và C2+1
    vector<vector<long long>> D(Hang + 2, vector<long long>(Cot + 2, 0));

    // Hàm cập nhật vùng hình chữ nhật (R1, C1) đến (R2, C2) với giá trị V
    auto capNhatHCN = [&](int R1, int C1, int R2, int C2, long long V) {
        D[R1][C1] += V;               // Góc trái trên
        D[R2 + 1][C1] -= V;           // Xóa thừa phía dưới
        D[R1][C2 + 1] -= V;           // Xóa thừa phía phải
        D[R2 + 1][C2 + 1] += V;       // Bù lại góc chéo dưới (bị trừ 2 lần)
    };

    // 2. Thực hiện các truy vấn cập nhật trong O(1)
    capNhatHCN(1, 1, 2, 2, 5);  // Cộng 5 vào vùng (1,1) -> (2,2)
    capNhatHCN(2, 2, 3, 3, -2); // Trừ 2 vào vùng (2,2) -> (3,3)

    // 3. Khôi phục lại Ma trận gốc A bằng Mảng cộng dồn 2 chiều
    vector<vector<long long>> A(Hang + 2, vector<long long>(Cot + 2, 0));
    
    cout << "Ma tran sau khi cap nhat la:\n";
    for (int i = 1; i <= Hang; i++) {
        for (int j = 1; j <= Cot; j++) {
            // Công thức Mảng cộng dồn 2 chiều kinh điển
            A[i][j] = A[i-1][j] + A[i][j-1] - A[i-1][j-1] + D[i][j];
            
            // In kết quả có định dạng khoảng cách
            cout << A[i][j] << "\t";
        }
        cout << "\n";
    }

    return 0;
}
```
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">5. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div>
<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Vấn đề / Kỹ thuật</th><th style="width: 75%;">Phân tích & Kinh nghiệm</th></tr></thead><tbody><tr><td><b>Offline Queries vs Online Queries</b></td><td>Mảng hiệu <b>CHỈ</b> dùng được cho các bài toán <b>Offline</b> (Tức là cho phép làm xong TẤT CẢ các thao tác cập nhật rồi mới hỏi kết quả). Nếu đề bài vừa bắt cập nhật, vừa xen kẽ hỏi kết quả ngay (Online), Mảng hiệu sẽ thất bại $\rightarrow$ Lúc này phải dùng <b>Segment Tree</b> hoặc <b>Fenwick Tree (BIT)</b>. Thầy nhớ dặn kỹ học sinh điểm chết người này!</td></tr><tr><td><b>Lỗi tràn chỉ số (Out of Bounds) ở <code>R+1</code></b></td><td>Khi cập nhật đoạn $[L, R]$, ta phải tác động vào <code>D[R+1]</code>. Nếu đoạn cập nhật kéo dài đến tận cuối mảng ($R = N$), thì <code>D[R+1]</code> sẽ là <code>D[N+1]</code>. Do đó, khi khai báo mảng <code>D</code>, phải <b>luôn khai báo kích thước là N + 2</b> để tránh lỗi truy cập vùng nhớ lạ.</td></tr><tr><td><b>Sự kết đôi hoàn hảo</b></td><td><b>Mảng $A \xrightarrow{\text{lấy hiệu}} \text{Mảng } D$</b><b>Mảng $D \xrightarrow{\text{cộng dồn}} \text{Mảng } A$</b>Học sinh nhớ được vòng tuần hoàn này thì sẽ không bao giờ quên bài.</td></tr><tr><td><b>Kiểu dữ liệu</b></td><td>Số lần cập nhật nhiều có thể làm phần tử vượt mốc <code>int</code>, mảng <code>D</code> và mảng <code>A</code> nên được khai báo bằng kiểu <code>long long</code>.</td></tr></tbody></table>
</div>