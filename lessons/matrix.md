## 🔢 Ma Trận (Mảng 2 chiều)
<br>
<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Khái niệm</div>

Ma trận là một bảng gồm các hàng (row) và cột (column). Trong C++, chỉ số hàng và cột đều bắt đầu từ $0$.

📊 Minh họa ma trận $3 * 4$ (3 hàng, 4 cột):
        
<table class="garden-table">
            <tr class="idx-row">
                <td>Row \ Col</td>
                <td>0</td>
                <td>1</td>
                <td>2</td>
                <td>3</td>
            </tr>
            <tr>
                <td class="idx-row">0</td>
                <td class="val-cell">12</td>
                <td class="val-cell">7</td>
                <td class="val-cell">5</td>
                <td class="val-cell">19</td>
            </tr>
            <tr>
                <td class="idx-row">1</td>
                <td class="val-cell">3</td>
                <td class="val-cell">11</td>
                <td class="val-cell">8</td>
                <td class="val-cell">2</td>
            </tr>
            <tr>
                <td class="idx-row">2</td>
                <td class="val-cell">6</td>
                <td class="val-cell">14</td>
                <td class="val-cell">1</td>
                <td class="val-cell">10</td>
            </tr>
        </table>

💡 Truy xuất: $A[1][2]$ có giá trị là $8$ (Hàng 1, Cột 2).
    
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Khai báo & Nhập ma trận</div>
    
**Dạng 1:** Biết trước $R$ dòng, $C$ cột

```cpp
const int MAXN = 1005;
int a[MAXN][MAXN];
int r, c;

cin >> r >> c;
for (int i = 0; i < r; i++)
    for (int j = 0; j < c; j++) cin >> a[i][j];
```

**Dạng 2:** Không biết trước số dòng cột (đọc đến hết file) **kỹ thuật Stringstream**

Sử dụng **getline** để đọc từng dòng và **stringstream** để tách từng số, từ đó xác định được số cột thực tế.

```cpp
#include <sstream>
int a[MAXN][MAXN], R = 0, C = 0;
string line;

while (getline(cin, line)) {
    if (line.empty()) break;
    stringstream ss(line);
    int val, tempC = 0;
    while (ss >> val) {
        a[R][tempC++] = val;
    }
    C = max(C, tempC);
    R++;
}
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Các kỹ thuật duyệt ma trận</div>

**A. Trên xuống dưới, phải qua trái:**

```cpp
void DuyetTBRL(double mt[dongmax][cotmax], int d, int c)
{
    for (int i = 0; i < d; ++i) {
        for (int j = c-1; j >= 0; --j)
            cout << mt[i][j] << " ";
        cout << "\n";
    }
}
```
**B. Từ dưới lên trên, phải qua trái:**
```cpp
void DuyetBTRL(double mt[dongmax][cotmax], int d, int c)
{
    for (int i = d-1; i >= 0; --i) {
        for (int j = c-1; j >= 0; --j)
            cout << mt[i][j] << " ";
        cout << "\n";
    }
}
```
**C. Từ trái qua phải, từ trên xuống dưới:**
```cpp
void DuyetLRTB(double mt[dongmax][cotmax], int d, int c)
{
    for (int i = 0; i < c; ++i) {
        for (int j = 0; j < d; ++j)
            cout << mt[j][i] << " ";
        cout << "\n";
    }
}
```
**D. Từ trái qua phải, từ dưới lên trên:**
```cpp
void DuyetRLBT(double mt[dongmax][cotmax], int d, int c)
{
    for (int i = c-1; i >= 0; --i) {
        for (int j = d-1; j >= 0; --j)
            cout << mt[j][i] << " ";
        cout << "\n";
    }
}
```

**E. Duyệt theo Đường chéo (Ma trận vuông N x N):**
```cpp
// Đường chéo chính (i == j)
for (int i = 0; i < n; i++) cout << a[i][i];

// Đường chéo phụ (i + j == n - 1)
for (int i = 0; i < n; i++) cout << a[i][n - 1 - i];
```
**F. Hướng đường chéo chính (Bắt đầu từ (n-1, 0) đến (0, n-1)):**

***Đặc điểm:*** Các phần tử cùng đường chéo chính có hiệu $i - j$ bằng nhau.
```cpp
// Duyệt qua từng đường chéo (có 2*n - 1 đường)
for (int k = 1 - n; k <= n - 1; k++) {
    for (int i = 0; i < n; i++) {
        int j = i - k;
        if (j >= 0 && j < n) cout << a[i][j] << " ";
    }
}
```
**E. Hướng đường chéo phụ (Bắt đầu từ (n-1, n-1) đến (0, 0)):**

***Đặc điểm:*** Các phần tử cùng đường chéo có tổng $i + j$ bằng nhau.
```cpp
for (int k = 2 * (n - 1); k >= 0; k--) {
    for (int i = 0; i < n; i++) {
        int j = k - i;
        if (j >= 0 && j < n) cout << a[i][j] << " ";
    }
}
```
**G. Duyệt Zig-zag theo dòng: (Dòng chẵn trái->phải, dòng lẻ phải->trái)**
```cpp
for (int i = 0; i < r; i++) {
    if (i % 2 == 0) 
        for (int j = 0; j < c; j++) cout << a[i][j];
    else 
        for (int j = c - 1; j >= 0; j--) cout << a[i][j];
}
```
**H. Duyệt Xoắn ốc (Spiral):**
```cpp
int h1 = 0, h2 = r-1, c1 = 0, c2 = c-1;
while(h1 <= h2 && c1 <= c2) {
    for(int i=c1; i<=c2; i++) cout << a[h1][i]; h1++;
    for(int i=h1; i<=h2; i++) cout << a[i][c2]; c2--;
    if(h1 <= h2) {
        for(int i=c2; i>=c1; i--) cout << a[h2][i]; h2--;
    }
    if(c1 <= c2) {
        for(int i=h2; i>=h1; i--) cout << a[i][c1]; c1++;
    }
}
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">4. Kiểm tra Ma trận</div>

**Ma trận đối xứng tâm:** Phần tử tại $(i, j)$ phải bằng $(r-1-i, c-1-j)$.
```cpp
bool isSymmetric = true;
for (int i = 0; i < r; i++)
    for (int j = 0; j < c; j++)
        if (a[i][j] != a[r-1-i][c-1-j]) isSymmetric = false;
```

**Ma phương (Magic Square):** Tổng mỗi hàng, mỗi cột và 2 đường chéo đều bằng nhau.
<div class="math-formula">

Tổng ma phương bậc $n$: $S = \frac{n(n^2 + 1)}{2}$

</div>

```cpp
bool KTMP(int d, int c) {
    // Bước 1: Kiểm tra ma trận vuông
    if (d != c) return false;

    // Bước 2 & 3: Kiểm tra giá trị [1..d*d] và tính duy nhất
    int marking[d * c + 1] = {0};
    for (int i = 0; i < d; ++i) {
        for (int j = 0; j < c; ++j) {
            int val = (int)mt[i][j];
            if (val < 1 || val > d * d) return false;
            if (marking[val] == 1) return false;
            marking[val] = 1;
        }
    }

    // Bước 4: Kiểm tra tổng các dòng = tổng các cột = tổng CC = tổng CP
    long long tcc = 0, tcp = 0, targetSum = 0;
    for (int i = 0; i < d; i++) {
        long long td = 0, tc = 0;
        tcc += mt[i][i];
        tcp += mt[i][d - i - 1];
        for (int j = 0; j < c; j++) {
            td += mt[i][j];
            tc += mt[j][i];
        }
        if (i == 0) targetSum = td;
        if (td != targetSum || tc != targetSum) return false;
    }
    if (tcc != targetSum || tcp != targetSum) return false;
    return true;
}
```

<div class="important-note">

**Lưu ý:** Cần dùng thêm mảng đánh dấu hoặc **std::set** để đảm bảo $n^2$ phần tử là các số phân biệt từ $1$ đến $n^2$.

</div>
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Thuật toán tạo Ma Phương</div>
    
**Tùy vào bậc $n$, ta có các thuật toán xây dựng khác nhau:**

<div style="margin-top: 20px; display: flex; gap: 20px; flex-wrap: wrap;">
    <div style="flex: 2; min-width: 300px;">
    
**A. Ma phương bậc lẻ ($n = 2k + 1$)**
        
Sử dụng **thuật toán Siamese (De la Loubère):**
        <ul>
            <li>Bắt đầu tại ô giữa của hàng đầu tiên.</li>
            <li>Di chuyển theo hướng <b>lên trên 1 hàng và sang phải 1 cột</b>.</li>
            <li>Nếu vượt ra ngoài ma trận, ta "vòng" lại phía đối diện.</li>
            <li>Nếu ô định di chuyển tới đã có số, ta di chuyển <b>xuống dưới 1 ô</b> so với ô hiện tại.</li>
        </ul>
    </div>
    <div style="flex: 1; min-width: 200px; background: #f1f5f9; padding: 10px; border-radius: 8px;">
        <p style="text-align: center; font-weight: bold; font-size: 0.9rem; margin-top: 0;">Minh họa n=5</p>
        <table class="garden-table" style="font-size: 0.75rem; margin: 0 auto; width: 100%;">
            <tr><td>17</td><td>24</td><td class="val-cell">1</td><td>8</td><td>15</td></tr>
            <tr><td>23</td><td>5</td><td>7</td><td>14</td><td>16</td></tr>
            <tr><td>4</td><td>6</td><td>13</td><td>20</td><td>22</td></tr>
            <tr><td>10</td><td>12</td><td>19</td><td>21</td><td>3</td></tr>
            <tr><td>11</td><td>18</td><td>25</td><td>2</td><td>9</td></tr>
        </table>
    </div>
</div>

```cpp
void taoMaPhuongLe(int n) {
    int i = 0, j = n / 2;
    for (int num = 1; num <= n * n; ++num) {
        mt[i][j] = num;
        int ni = (i - 1 + n) % n;
        int nj = (j + 1) % n;
        if (mt[ni][nj]) i++;
        else { i = ni; j = nj; }
    }
}
```
<hr style="border: 0; border-top: 1px solid #e2e8f0; margin: 20px 0;">

<div style="display: flex; gap: 20px; flex-wrap: wrap;">
    <div style="flex: 2; min-width: 300px;">
        
**B. Ma phương bậc 4n ($n = 4, 8, 12...$)**

Sử dụng phương pháp **Đảo ngược đường chéo:**
        <ul>
            <li>Điền số từ $1$ đến $n^2$ từ trái qua phải, trên xuống dưới.</li>
            <li>Chia ma trận thành các khối $4 \times 4$.</li>
            <li>Giữ nguyên số trên đường chéo chính/phụ của khối $4 \times 4$.</li>
            <li>Các số còn lại đảo ngược: $mt[i][j] = (n^2 + 1) - mt[i][j]$.</li>
        </ul>
    </div>
    <div style="flex: 1; min-width: 200px; background: #f1f5f9; padding: 10px; border-radius: 8px;">
        <p style="text-align: center; font-weight: bold; font-size: 0.9rem; margin-top: 0;">Minh họa n=4</p>
        <table class="garden-table" style="font-size: 0.75rem; margin: 0 auto; width: 100%;">
            <tr><td class="val-cell">16</td><td>2</td><td>3</td><td class="val-cell">13</td></tr>
            <tr><td>5</td><td class="val-cell">11</td><td class="val-cell">10</td><td>8</td></tr>
            <tr><td>9</td><td class="val-cell">7</td><td class="val-cell">6</td><td>12</td></tr>
            <tr><td class="val-cell">4</td><td>14</td><td>15</td><td class="val-cell">1</td></tr>
        </table>
    </div>
</div>

```cpp
void taoMaPhuong4n(int n) {
    int cnt = 1;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) mt[i][j] = cnt++;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if ((i % 4 == j % 4) || (i % 4 + j % 4 == 3)) {
                mt[i][j] = (n * n + 1) - mt[i][j];
            }
        }
    }
}
```
<hr style="border: 0; border-top: 1px solid #e2e8f0; margin: 20px 0;">

<div style="display: flex; gap: 20px; flex-wrap: wrap;">
    <div style="flex: 2; min-width: 300px;">
    
**C. Thuật toán Strachey ($n = 4k+2$)**
        
Giả sử $m = n/2$ (số lẻ).
        <ol>
            <li>Chia ma trận thành 4 khối $m \times m$: <b>A (Top-Left), B (Bottom-Right), C (Top-Right), D (Bottom-Left)</b>.</li>
            <li>Điền ma phương lẻ cấp $m$ vào <b>A</b> (từ $1$ đến $m^2$).</li>
            <li>Điền <b>B</b> (cộng $m^2$), <b>C</b> (cộng $2m^2$), <b>D</b> (cộng $3m^2$).</li>
            <li><b>Swap:</b> Hoán đổi cột giữa A-D và C-B theo quy tắc $k$.</li>
        </ol>
    </div>
    <div style="flex: 1; min-width: 200px; background: #f1f5f9; padding: 10px; border-radius: 8px;">
        <p style="text-align: center; font-weight: bold; font-size: 0.9rem; margin-top: 0;">Minh họa n=6</p>
        <table class="garden-table" style="font-size: 0.65rem; margin: 0 auto; width: 100%;">
            <tr style="background: #dcfce7;"><td>35</td><td>1</td><td>6</td><td>26</td><td>19</td><td>24</td></tr>
            <tr style="background: #dcfce7;"><td>3</td><td>32</td><td>7</td><td>21</td><td>23</td><td>25</td></tr>
            <tr style="background: #dcfce7;"><td>31</td><td>9</td><td>2</td><td>22</td><td>27</td><td>20</td></tr>
            <tr style="background: #fee2e2;"><td>8</td><td>28</td><td>33</td><td>17</td><td>10</td><td>15</td></tr>
            <tr style="background: #fee2e2;"><td>30</td><td>5</td><td>34</td><td>12</td><td>14</td><td>16</td></tr>
            <tr style="background: #fee2e2;"><td>4</td><td>36</td><td>29</td><td>13</td><td>18</td><td>11</td></tr>
        </table>
    </div>
</div>

```cpp
void taoMaPhuong4n2(int n) {
    int m = n / 2, m2 = m * m, k = n / 4;
    // ... (Điền 4 vùng A, B, C, D) ...
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < m; j++) {
            mt[i][j] = sub[i][j]; mt[i+m][j+m] = sub[i][j] + m2;
            mt[i][j+m] = sub[i][j] + 2*m2; mt[i+m][j] = sub[i][j] + 3*m2;
        }
    }
    // Swap A-D và C-B
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < k; j++) {
            if (i == m/2) swap(mt[i][j+1], mt[i+m][j+1]);
            else swap(mt[i][j], mt[i+m][j]);
        }
    }
}
```
</div>

<div class="step-card border-yellow">
    <div class="step-badge bg-yellow">4. Lưu ý khi sử dụng ma trận</div>
<div class="important-note">

**1. Thứ tự ưu tiên duyệt phần tử (Cache Locality):** đây là lưu ý quan trọng nhất về hiệu suất. Trong C++, ma trận được lưu trữ theo từng hàng liên tiếp nhau trong bộ nhớ.
* Nên: Duyệt theo hàng trước, cột sau (Vòng lặp i hàng bên ngoài, j cột bên trong).
* Không nên: Duyệt theo cột trước, hàng sau. Việc này bắt CPU phải nhảy cóc qua các vùng nhớ không liên tiếp, làm chương trình chạy chậm hơn từ **5-10** lần đối với ma trận lớn.

**2. Giới hạn bộ nhớ (Memory Limit):** ma trận tốn bộ nhớ theo cấp số nhân ($R \times C$).
* Một ma trận số nguyên int a[10000][10000] sẽ chiếm: $10^8 \times 4$ bytes $\approx 400$ MB.
* Nếu giới hạn bộ nhớ của bài thi là 128MB hoặc 256MB, khai báo trên sẽ gây lỗi MLE (Memory Limit Exceeded) ngay lập tức.
* ***Lưu ý:*** Luôn tính toán kỹ tổng số phần tử trước khi khai báo MAXN.

**3. Khai báo mảng toàn cục (Global Array):** giống như mảng 1 chiều, ma trận nên được khai báo bên ngoài hàm `main`.
* Ma trận khai báo trong hàm main sẽ nằm ở bộ nhớ **Stack**, vốn có dung lượng rất nhỏ (thường chỉ vài MB), dễ gây lỗi **Stack Overflow** dù chưa vượt quá giới hạn bộ nhớ tổng của bài.
* Khai báo toàn cục giúp ma trận nằm ở bộ nhớ **Static**, tận dụng được tối đa **RAM** cho phép.

**4. Vùng đệm an toàn (Padding):** luôn khai báo kích thước ma trận lớn hơn yêu cầu của đề bài một khoảng nhỏ (thường là 5 đến 10 đơn vị).
* Ví dụ: Đề cho $N, M \le 1000$, hãy khai báo int a[1005][1005].
* Việc này giúp tránh lỗi **Access Violation** khi thực hiện các kỹ thuật như loang **(BFS/DFS)** hoặc kiểm tra các ô lân cận ($i-1, j-1$) mà quên kiểm tra biên.

**5. Khởi tạo giá trị (Initialization):** 
* Với ma trận toàn cục, tất cả phần tử mặc định là **0**.
* Nếu cần đặt lại giá trị (ví dụ tất cả bằng -1 hoặc 0 cho nhiều bộ test), hãy dùng `memset:memset(a, 0, sizeof(a));`
* ***Cẩn thận:*** `memset` chỉ hoạt động chính xác với giá trị `0` và `-1` cho ma trận (mảng) kiểu `int`. Với các giá trị khác, Thầy nên dùng vòng lặp hoặc `std::fill`.

**6. Truyền ma trận vào hàm:** Khi truyền ma trận tĩnh vào một hàm, bắt buộc phải ghi rõ kích thước của cột.
* Ví dụ: `void process(int a[][MAXN], int r, int c)`.
* Điều này là do trình biên dịch cần biết số cột để tính toán vị trí ô nhớ: $A[i][j] = \text{địa chỉ đầu} + (i \times \text{số cột} + j) \times \text{kích thước kiểu dữ liệu}$.
</div>