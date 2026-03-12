## 🌀 Quy Hoạch Động (Dynamic Programming)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

**1. Tư tưởng chính**

Quy hoạch động là phương pháp giải quyết các bài toán phức tạp bằng cách **chia nhỏ** chúng thành các bài toán con nhỏ hơn, giải các bài toán con này một lần và lưu trữ kết quả của chúng để tái sử dụng khi cần thiết *(gọi là kỹ thuật ghi nhớ - Memoization hoặc lập bảng - Tabulation)*.

**2. Cấu trúc bài toán DP**
- **Dữ liệu đầu vào:** Thường là một tập hợp các phần tử (mảng, chuỗi) hoặc một giá trị đích cần đạt được.
- **Yêu cầu:** Tìm giá trị tối ưu (lớn nhất/nhỏ nhất) hoặc đếm số cách thực hiện.
- **Kết quả:** Một giá trị duy nhất (đáp án bài toán lớn).

**3. Bài toán con (Sub-problems)**
- Là phiên bản nhỏ hơn của bài toán gốc. Kết quả của bài toán lớn được xây dựng từ kết quả của các bài toán con trước đó.
- Ví dụ minh họa: Tính số Fibonacci thứ $n$. 
    - Để tính $F(5)$, ta cần $F(4)$ và $F(3)$. 
    - Thay vì tính lại nhiều lần, ta lưu $F(1), F(2)...$ vào mảng.

**4. Hàm mục tiêu và Tham số**
- **Hàm mục tiêu:** Là giá trị ta đang tìm kiếm (ví dụ: $dp[i]$ là số cách tối ưu để đạt đến trạng thái $i$).
- **Tham số:** Các biến xác định trạng thái của bài toán (vị trí phần tử, khối lượng còn lại, số chữ số...).
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các bước giải quyết bài toán Quy hoạch động</div>

**Để giải quyết một bài toán DP, chúng ta thực hiện theo quy trình 3 bước chuẩn sau:**

<div class="step-title-box bg-red"> Bước 1: Xác định trạng thái và Bài toán cơ sở </div>

- **Trạng thái:** Xác định ý nghĩa của mảng $dp[]$. 
    - Ví dụ: $dp[i]$ là số tiền ít nhất để đổi được số tiền bằng $i$.
- **Bài toán cơ sở (Base case):** Những trường hợp nhỏ nhất có thể biết ngay đáp án. 
    - Ví dụ: $dp[0] = 0$.

<div class="step-title-box bg-yellow"> Bước 2: Xây dựng công thức truy hồi (Transition) </div>

Đây là linh hồn của bài toán. Tìm mối liên hệ giữa bài toán hiện tại với các bài toán con đã giải trước đó.

**Công thức:** 

<div class="math-formula">$dp[i] = \text{Tối ưu}(dp[i-1], dp[i-k], \dots) + \text{chi phí}$ </div>

<div class="step-title-box bg-green"> Bước 3: Tính toán và Truy vết </div>

- **Tính toán:** Sử dụng vòng lặp để điền kết quả vào bảng (mảng 1 chiều hoặc 2 chiều).
- **Truy vết (Trace):** Nếu bài toán yêu cầu đưa ra cách chọn phần tử (không chỉ là giá trị), ta dùng một mảng phụ để lưu lại "vết" đã chọn ở mỗi bước.
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Ví dụ minh họa</div>

**Bài toán Leo thang:** Có $N$ bậc thang. Mỗi bước có thể bước $1$ hoặc $2$ bậc. Hỏi có bao nhiêu cách để lên đỉnh $N$?

- **Bước 1:**
    - **Trạng thái:** $dp[i]$ là số cách lên bậc $i$.
    - **Bài toán Cơ sở:** $dp[1] = 1, dp[2] = 2$.
- **Bước 2: công thức truy hồi:** 
    - Trường hợp 1: Bước từ bậc $i - 1$ lên bậc $i$. Số cách chính là tích 2 giai đoạn
        - Giai đoạn 1: Bước từ bậc $1$ đến bậc $i - 1$: số cách là $dp[i - 1]$
        - Giai đoạn 2: Bước từ bậc $i - 1$ lên bậc $i$: số cacsu là $1$
        - Vậy số cách của trường hợp 1 là $dp[i - 1] * 1 = dp[i - 1]$ 
    - Trường hợp 1: Bước từ bậc $i - 2$ lên bậc $i$. Số cách chính là tích 2 giai đoạn
        - Giai đoạn 1: Bước từ bậc $1$ đến bậc $i - 2$: số cách là $dp[i - 2]$
        - Giai đoạn 2: Bước từ bậc $i - 2$ lên bậc $i$: số cacsu là $1$
        - Vậy số cách của trường hợp 1 là $dp[i - 2] * 1 = dp[i - 2]$ 
    - Số cách để đi từ bậc thang $1$ đến bậc thang $i$ là tổng 2 trường hợp: $dp[i] = dp[i - 1] + dp[i - 2]$.
- **Bước 3: Tính toán và truy vết**
    - Ví dụ với $N = 9$ ta có bảng dp như bên dưới 

<div class="geometry-table-container">
    <table class="styled-table">
        <thead>
            <tr>
                <th style="width: 15%;">index (i)</th>
                <th style="width: 8.5%;">0</th>
                <th style="width: 8.5%;">1</th>
                <th style="width: 8.5%;">2</th>
                <th style="width: 8.5%;">3</th>
                <th style="width: 8.5%;">4</th>
                <th style="width: 8.5%;">5</th>
                <th style="width: 8.5%;">6</th>
                <th style="width: 8.5%;">7</th>
                <th style="width: 8.5%;">8</th>
                <th style="width: 8.5%;">9</th>
            </tr>
        </thead>
        <tbody>
            <tr style="text-align: center; font-weight: bold;">
                <td style="background-color: #f8fafc; color: #1e3a8a;">dp[i]</td>
                <td>0</td>
                <td>1</td>
                <td>2</td>
                <td>3</td>
                <td>5</td>
                <td style="background-color: #dcfce7;">8</td>
                <td style="background-color: #bbf7d0;">13</td>
                <td style="background-color: #22c55e; color: white;">21</td>
                <td>34</td>
                <td>55</td>
            </tr>
        </tbody>
    </table>
</div>

<div class="formula-notes" style="margin-top: 10px;">
    <p><b>Giải thích ví dụ (Leo thang/Fibonacci):</b></p>
    <ul>
        <li>Các ô màu xanh minh họa cho việc tính toán trạng thái hiện tại dựa trên các trạng thái trước đó.</li>
        <li>Giá trị tại <b>index 7</b> (đáp án là 21) được tính bằng tổng của hai bài toán con trước đó: $dp[7] = dp[6] + dp[5] = 13 + 8$.</li>
    </ul>
</div> 
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Ứng dụng tư tưởng Quy hoạch động (THCS)</div>

**Hai lớp bài toán chính**

- **📊 Bài toán Đếm:** Đếm số lượng cách thỏa mãn điều kiện. (Ví dụ: Đếm số cách đổi tiền, đếm số đường đi trong mê cung).

- **🏆 Bài toán Tối ưu:** Tìm giá trị lớn nhất (Max) hoặc nhỏ nhất (Min). (Ví dụ: Bài toán Cái túi, tìm dãy con tăng dài nhất).

**Các dạng Quy hoạch động thường gặp**

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th>Dạng DP</th><th>Đặc điểm</th><th>Ví dụ tiêu biểu</th></tr></thead><tbody><tr><td><b>Mảng 1 chiều</b></td><td>Trạng thái chỉ phụ thuộc vào 1 tham số duy nhất (vị trí hoặc giá trị).</td><td>Dãy con có tổng bằng $S$, bậc thang, đổi tiền.</td></tr><tr><td><b>Mảng 2 chiều</b></td><td>Trạng thái phụ thuộc vào 2 tham số (thường là 2 đối tượng hoặc tọa độ $i, j$).</td><td>Xâu con chung dài nhất (LCS), Cái túi (Knapsack), Đường đi trên lưới.</td></tr><tr><td><b>DP Chữ số (Digit DP)</b></td><td>Dùng để đếm các số trong đoạn $[L, R]$ thỏa mãn tính chất chữ số đặc biệt.</td><td>Đếm số có tổng các chữ số là số nguyên tố, đếm số không có chữ số 4.</td></tr></tbody></table></div></div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">4. Phân tích chi tiết: Quy hoạch động trên Ma trận (Grid DP)</div>

**Đặc điểm nhận diện:** Bài toán cho một bảng (lưới) kích thước $N \times M$. Yêu cầu tìm đường đi từ ô $(1, 1)$ đến ô $(N, M)$ sao cho tổng chi phí là nhỏ nhất/lớn nhất, hoặc đếm số cách đi. Ràng buộc thường gặp là chỉ được di chuyển sang phải (Right) hoặc xuống dưới (Down).

**Ví dụ minh họa:** Cho ma trận $A$ kích thước $N \times M$ chứa các số nguyên dương (chi phí). Tìm đường đi từ góc trái trên $(1, 1)$ xuống góc phải dưới $(N, M)$ sao cho tổng chi phí trên đường đi là nhỏ nhất. Mỗi bước chỉ được đi sang phải hoặc đi xuống.

<div class="step-title-box bg-red"> Bước 1: Xác định trạng thái và Bài toán cơ sở </div>

- **Trạng thái:** Gọi $dp[i][j]$ là tổng chi phí nhỏ nhất để đi từ ô $(1, 1)$ đến ô $(i, j)$.
- **Bài toán cơ sở:** $dp[1][1] = A[1][1]$ (Chi phí để đứng ở ngay ô xuất phát chính là giá trị của ô đó).
- **Xử lý viền:** Để tránh lỗi khi gọi $i-1$ hoặc $j-1$, ta khởi tạo hàng $0$ và cột $0$ của mảng $dp$ bằng một giá trị vô cùng lớn (Ví dụ: 1e9), vì ta đang tìm Min.

<div class="step-title-box bg-yellow"> Bước 2: Xây dựng công thức truy hồi </div>

- Để đến được ô $(i, j)$, ta chỉ có thể đến từ 2 hướng:
    1. Từ ô ngay phía trên đi xuống: $(i-1, j)$
    2. Từ ô ngay bên trái đi sang: $(i, j-1)$
- Vì muốn chi phí nhỏ nhất, ta chọn hướng có chi phí thấp hơn rồi cộng với chi phí tại ô hiện tại.
<div class="math-formula">$dp[i][j] = \min(dp[i-1][j], dp[i][j-1]) + A[i][j]$</div>

<div class="step-title-box bg-green"> Bước 3: Tính toán và Code mẫu </div>

```cpp
int main() {
    int N = 3, M = 3;
    // Bảng chi phí A (dùng 1-based index)
    vector<vector<int>> A = {
        {0, 0, 0, 0},
        {0, 1, 3, 1},
        {0, 1, 5, 1},
        {0, 4, 2, 1}
    };

    // Khởi tạo mảng DP với giá trị vô cực (1e9)
    vector<vector<long long>> dp(N + 1, vector<long long>(M + 1, 1e9));

    // Bài toán cơ sở
    dp[1][1] = A[1][1];

    // Tính toán theo công thức truy hồi
    for (int i = 1; i <= N; i++) {
        for (int j = 1; j <= M; j++) {
            if (i == 1 && j == 1) continue; // Bỏ qua ô cơ sở
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + A[i][j];
        }
    }

    cout << "Chi phi nho nhat: " << dp[N][M] << endl; 
    // Kỳ vọng: 1 -> 1 -> 2 -> 1 = 5 (Đi theo đường: Dưới, Phải, Dưới, Phải)
    return 0;
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">5. Phân tích chi tiết: Quy hoạch động Chữ số (Digit DP)</div>

### Cách 1: Ma trận tiền xử lý
**Đặc điểm nhận diện:** Sử dụng khi bài toán có rất nhiều truy vấn (Ví dụ $Q = 10^5$, mỗi truy vấn hỏi một đoạn $[L, R]$ khác nhau). Ta sẽ tính toán trước toàn bộ các tổ hợp số không bị giới hạn (tức là điền thoải mái từ 0 đến 9), lưu vào một ma trận. Sau đó, với mỗi số $X$, ta chỉ việc "ráp" các giá trị từ ma trận này vào để tính kết quả.

**Ví dụ minh họa:** Bài toán đếm số lượng các số $\le X$ có tổng các chữ số bằng đúng $S$.
<div class="step-title-box bg-red"> Bước 1: Tiền xử lý - Xây dựng ma trận DP không giới hạn </div>

- **Trạng thái:** Gọi $dp[i][j]$ là số lượng các số có độ dài đúng bằng $i$ (tính cả các số có số 0 ở đầu) và có tổng các chữ số bằng đúng $j$.
- **Bài toán cơ sở:** $dp[0][0] = 1$ (Số có độ dài 0, tổng 0 có đúng 1 cách chọn là "không chọn gì cả").
- **Công thức xây dựng:** Để tạo ra số có độ dài $i$ và tổng $j$, ta thêm một chữ số $d \in [0, 9]$ vào trước số có độ dài $i-1$. Khi đó tổng cần đạt được ở phần còn lại là $j - d$.
    <div class="math-formula">$dp[i][j] = \sum_{d=0}^{9} dp[i-1][j - d]$ (với điều kiện $j \ge d$)</div>
<div class="step-title-box bg-yellow"> Bước 2: Truy vấn - Kỹ thuật khớp Tiền tố (Prefix Matching) </div>

- Để đếm số lượng số $\le X$ có tổng bằng $S$: Giả sử $X = 324$ và $S = 5$. Chiều dài mảng là $3$. Ta duyệt từng chữ số của $X$ từ trái sang phải:
    1. Xét chữ số hàng trăm ($3$): Ta có thể điền các chữ số nhỏ hơn $3$ (tức là $0, 1, 2$).
        - Nếu điền $0$, ta còn 2 vị trí trống, cần tổng là $5 - 0 = 5 \rightarrow$ Cộng thêm $dp[2][5]$.
        - Nếu điền $1$, còn 2 vị trí trống, cần tổng là $5 - 1 = 4 \rightarrow$ Cộng thêm $dp[2][4]$.
        - Nếu điền $2$, còn 2 vị trí trống, cần tổng là $5 - 2 = 3 \rightarrow$ Cộng thêm $dp[2][3]$.
        - Sau khi thử xong các số nhỏ hơn, ta chốt cứng chữ số hàng trăm là 3 (để tiếp tục bám theo số $X$). Tổng hiện tại là $3$.
    2. Xét chữ số hàng chục ($2$): Ta thử điền nhỏ hơn $2$ (tức là $0, 1$).
        - Nếu điền $0$, còn 1 vị trí trống, cần tổng là $5 - (3+0) = 2 \rightarrow$ Cộng thêm $dp[1][2]$.
        - Tương tự cho $1$.
        - Sau đó chốt cứng hàng chục là $2$. Tổng hiện tại là $3 + 2 = 5$.
    3. Cứ tiếp tục như vậy. Cuối cùng, kiểm tra xem bản thân số $X$ có thỏa mãn không.
<div class="step-title-box bg-green"> Bước 3: Code mẫu C++ </div>

```cpp
// Ma trận lưu sẵn: dp[độ_dài][tổng_chữ_số]
long long dp[20][165]; 

// Hàm chạy 1 lần duy nhất lúc bắt đầu chương trình
void tienXuLy() {
    dp[0][0] = 1;
    for (int i = 1; i < 20; i++) {
        for (int j = 0; j < 165; j++) {
            for (int d = 0; d <= 9; d++) {
                if (j >= d) {
                    dp[i][j] += dp[i-1][j - d];
                }
            }
        }
    }
}

// Hàm giải quyết truy vấn cho số X bất kỳ
long long solve(long long num, int S) {
    if (num < 0) return 0;
    string X = to_string(num);
    int n = X.length();
    
    long long so_cach = 0;
    int tong_hien_tai = 0;

    // Duyệt qua từng chữ số của X từ trái sang phải
    for (int i = 0; i < n; i++) {
        int chu_so_X = X[i] - '0';
        int so_luong_o_trong = n - 1 - i; // Số vị trí còn lại phía sau

        // THỬ: Đặt các chữ số d nhỏ hơn hẳn chữ số hiện tại của X
        for (int d = 0; d < chu_so_X; d++) {
            int tong_con_thieu = S - tong_hien_tai - d;
            if (tong_con_thieu >= 0) {
                // Ráp ngay kết quả từ ma trận đã tiền xử lý vào
                so_cach += dp[so_luong_o_trong][tong_con_thieu];
            }
        }

        // CHỐT: Điền đúng chữ số của X để đi tiếp xuống nhánh dưới
        tong_hien_tai += chu_so_X;
        
        // Tối ưu cắt tỉa: Nếu tổng đã vượt quá S, không cần đi tiếp nữa
        if (tong_hien_tai > S) break; 
    }

    // Cuối cùng, xét riêng trường hợp bản thân số X có thỏa mãn không
    if (tong_hien_tai == S) {
        so_cach++;
    }

    return so_cach;
}

int main() {
    tienXuLy(); // Chỉ gọi 1 lần!

    long long L = 10, R = 100;
    int S = 5;
    
    // Xử lý truy vấn cực nhanh trong O(Độ dài số X)
    long long ket_qua = solve(R, S) - solve(L - 1, S);
    
    cout << "So luong so thoa man (Bottom-Up): " << ket_qua << endl;
    return 0;
}
```
### Cách 2: Đệ quy có nhớ 
**Đặc điểm nhận diện:** Đề bài yêu cầu đếm số lượng các số hoặc tính tổng các số nằm trong một đoạn rất lớn $[L, R]$ (thường $R \le 10^{18}$) thỏa mãn một điều kiện đặc biệt về các chữ số (Ví dụ: Tổng các chữ số bằng $S$, số không chứa chữ số 4, số có các chữ số tăng dần...).

**Tư duy cốt lõi:** Thay vì duyệt từ $L$ đến $R$ (chắc chắn TLE), ta tách bài toán thành đếm số lượng trong đoạn $[0, R]$ trừ đi đoạn $[0, L-1]$. Hàm giải quyết sẽ là Solve(R) - Solve(L-1).

**Ví dụ minh họa:** Vẫn là bài toán đếm xem có bao nhiêu số trong đoạn $[0, X]$ mà tổng các chữ số của nó bằng đúng $S$. (Thực hành bằng phương pháp Đệ quy có nhớ - Top Down).
<div class="step-title-box bg-red"> Bước 1: Xác định trạng thái và Bài toán cơ sở </div>

- Chuyển số $X$ thành một mảng/chuỗi các chữ số.
    - **Trạng thái:** Mảng $dp[idx][sum][is\_limit]$
        - $idx$: Vị trí chữ số đang xét (từ trái sang phải).
        - $sum$: Tổng các chữ số đã chọn từ đầu đến hiện tại.
        - $is\_limit$: Kiểu bool. Nếu true, chữ số tiếp theo chỉ được chọn tối đa bằng chữ số tương ứng của $X$. Nếu false, được chọn thoải mái từ $0$ đến $9$.
    - **Bài toán cơ sở:** Khi $idx$ vượt quá độ dài chuỗi số, nếu $sum == S$ thì trả về $1$ (tìm được 1 số hợp lệ), ngược lại trả về $0$.
<div class="step-title-box bg-yellow"> Bước 2: Xây dựng công thức truy hồi </div>
    
- Tại vị trí $idx$, ta thử điền một chữ số $d$ (từ $0$ đến max_digit). Ta cộng dồn số cách chọn của các nhánh con.
    <div class="math-formula">$dp[idx][sum][limit] = \sum dp[idx+1][sum+d][new\_limit]$</div>

<div class="step-title-box bg-green"> Bước 3: Tính toán và Code mẫu </div>

```cpp
string X;
int S;
// dp[idx][sum][is_limit]
long long dp[20][165][2]; 

long long digitDP(int idx, int sum, bool is_limit) {
    // Nếu tổng đã vượt quá S thì dừng sớm (Cắt tỉa)
    if (sum > S) return 0;
    
    // Bài toán cơ sở: Đã xét hết các chữ số
    if (idx == X.length()) {
        return (sum == S) ? 1 : 0;
    }

    // Nếu bài toán con này đã được giải và không bị giới hạn, lấy ra dùng luôn
    if (!is_limit && dp[idx][sum][is_limit] != -1) {
        return dp[idx][sum][is_limit];
    }

    long long so_cach = 0;
    // Xác định chữ số lớn nhất có thể điền tại vị trí hiện tại
    int limit_digit = is_limit ? (X[idx] - '0') : 9;

    // Thử điền các chữ số d từ 0 đến limit_digit
    for (int d = 0; d <= limit_digit; d++) {
        // Trạng thái giới hạn mới: Chỉ giữ limit nếu hiện tại đang limit VÀ chữ số điền vào vừa chạm đỉnh
        bool new_limit = is_limit && (d == limit_digit);
        
        // Gọi đệ quy cho bước tiếp theo
        so_cach += digitDP(idx + 1, sum + d, new_limit);
    }

    // Ghi nhớ kết quả (Chỉ ghi nhớ khi is_limit = false để dùng lại cho các nhánh khác)
    if (!is_limit) {
        dp[idx][sum][is_limit] = so_cach;
    }

    return so_cach;
}

long long solve(long long num) {
    if (num < 0) return 0;
    X = to_string(num);
    // Reset mảng dp bằng -1 trước mỗi lần gọi hàm solve
    for(int i=0; i<20; i++) 
        for(int j=0; j<165; j++) 
            dp[i][j][0] = dp[i][j][1] = -1;
            
    return digitDP(0, 0, true);
}

int main() {
    long long L = 10, R = 100;
    S = 5; // Tìm các số từ 10 đến 100 có tổng chữ số bằng 5 (VD: 14, 23, 32, 41, 50)
    
    // Áp dụng tính chất: Số lượng trong [L, R] = Số lượng [0, R] - Số lượng [0, L-1]
    long long ket_qua = solve(R) - solve(L - 1);
    
    cout << "So luong so thoa man: " << ket_qua << endl; // Kỳ vọng: 5
    return 0;
}
```
</div>

<div class="important-note">
💡 <b>Lưu ý quan trọng khi thi đấu:</b>

- **Khởi tạo:** Luôn khởi tạo mảng DP với giá trị vô cùng lớn (nếu tìm Min) hoặc vô cùng nhỏ (nếu tìm Max) trước khi chạy vòng lặp.
- **Kích thước mảng:** Chú ý giới hạn bộ nhớ (Memory Limit). Mảng 2 chiều $10^4 \times 10^4$ thường sẽ gây lỗi quá bộ nhớ.
- **Kiểm soát dấu:** Đối với bài toán đếm số cách lớn, hãy luôn nhớ thực hiện phép chia lấy dư (Modulo) ở mỗi bước cộng dồn.
- **Đối với dạng DP digit:**
    - Nếu gặp bài toán DP Chữ số mà điều kiện quá phức tạp, liên tục biến đổi (ví dụ: chia hết cho số $K$ thay đổi theo từng Test) $\rightarrow$ Hãy dùng Đệ quy có nhớ (Top-Down) vì nó dễ code và dễ nghĩ.
    - Nếu gặp bài toán có hàng vạn Test cases hỏi đi hỏi lại một quy luật $\rightarrow$ Hãy dùng Ma trận tiền xử lý (Bottom-Up) để tránh TLE.
</div>