## 🗺️ Kỹ Thuật Tìm Kiếm Trên Ma Trận (Matrix Search)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Tìm kiếm tuần tự bằng Vòng lặp (Sequential Search)</div>

**Phân tích thuật toán:** Đây là phương pháp cơ bản nhất. Ta sử dụng 2 vòng lặp for lồng nhau để duyệt qua từng hàng và từng cột của ma trận từ trên xuống dưới, từ trái sang phải.
- **Độ phức tạp:** Thời gian $O(N \times M)$, Không gian $O(1)$.
- **Khi nào sử dụng:** Khi cần đếm tần suất, tính tổng, tìm Max/Min, hoặc tìm tọa độ của một giá trị cụ thể mà không liên quan đến đường đi hay tính liên thông.

**Ví dụ minh họa:** Cho ma trận kích thước $N \times M$. Tìm và in ra tọa độ $(i, j)$ của phần tử có giá trị bằng $X$ xuất hiện lần đầu tiên.

**Các bước cài đặt:** 
1. Khởi tạo vòng lặp i chạy từ $0$ đến $N-1$ (duyệt hàng).
2. Khởi tạo vòng lặp j chạy từ $0$ đến $M-1$ (duyệt cột).
3. Nếu A[i][j] == X, in ra tọa độ và dùng return hoặc break để dừng tìm kiếm.

**Code mẫu C++:**
```cpp
void timKiemTuanTu(const vector<vector<int>>& A, int X) {
    int N = A.size();
    int M = A[0].size();

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < M; j++) {
            if (A[i][j] == X) {
                cout << "Tim thay " << X << " tai toa do: (" << i << ", " << j << ")\n";
                return; // Dừng ngay khi tìm thấy
            }
        }
    }
    cout << "Khong tim thay " << X << " trong ma tran.\n";
}
```
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Tìm kiếm theo chiều sâu (Depth-First Search - DFS)</div>

**Phân tích thuật toán:** DFS trên ma trận giống như việc bạn đi vào một mê cung: Bạn sẽ đi cắm đầu về một hướng cho đến khi chạm tường (hoặc ngõ cụt), sau đó mới quay lui (backtrack) để thử các hướng khác. DFS thường được cài đặt bằng Đệ quy (Call Stack).
- **Độ phức tạp:** Thời gian $O(N \times M)$, Không gian $O(N \times M)$ (do độ sâu của Call Stack).
- **Khi nào sử dụng:** Tuyệt vời để giải quyết các bài toán Loang (Flood Fill), tìm Vùng liên thông (đếm số hòn đảo, diện tích lớn nhất của vết dầu loang), hoặc kiểm tra xem có tồn tại đường đi hay không.

**Ví dụ minh họa:** Đếm số hòn đảo (Number of Islands). Cho ma trận nhị phân, 1 là đất liền, 0 là nước. Một hòn đảo được tạo thành từ các ô 1 nối liền nhau theo 4 hướng (Trên, Dưới, Trái, Phải). Hỏi có bao nhiêu hòn đảo?

**Các bước cài đặt:**
1. Khai báo 2 mảng hướng đi `dx` và `dy` để code gọn gàng (không phải viết 4 lệnh `if`).
2. Viết hàm đệ quy `DFS(i, j)`: Đánh dấu ô hiện tại là đã thăm (ví dụ đổi 1 thành 0).
3. Dùng vòng lặp thử 4 hướng xung quanh. Nếu ô kề cạnh hợp lệ (nằm trong bảng và là đất liền), gọi tiếp DFS cho ô đó.
4. Ở hàm chính, duyệt toàn bộ ma trận, cứ gặp ô 1 thì tăng biến đếm đảo lên $1$ và gọi DFS để "nhấn chìm" toàn bộ hòn đảo đó.

**Code mẫu C++:**
```cpp
// 4 hướng di chuyển: Lên, Xuống, Trái, Phải
int dx[4] = {-1, 1, 0, 0};
int dy[4] = {0, 0, -1, 1};

void DFS(vector<vector<char>>& grid, int i, int j) {
    int N = grid.size();
    int M = grid[0].size();

    // Đánh dấu ô này đã thăm bằng cách biến nó thành nước '0'
    grid[i][j] = '0';

    // Loang ra 4 hướng
    for (int k = 0; k < 4; k++) {
        int next_i = i + dx[k];
        int next_j = j + dy[k];

        // Kiểm tra điều kiện: Nằm trong bảng VÀ là đất liền '1'
        if (next_i >= 0 && next_i < N && next_j >= 0 && next_j < M && grid[next_i][next_j] == '1') {
            DFS(grid, next_i, next_j);
        }
    }
}

int demSoHonDao(vector<vector<char>>& grid) {
    int so_dao = 0;
    for (int i = 0; i < grid.size(); i++) {
        for (int j = 0; j < grid[0].size(); j++) {
            if (grid[i][j] == '1') { // Tìm thấy một phần của hòn đảo mới
                so_dao++;
                DFS(grid, i, j); // Nhấn chìm toàn bộ hòn đảo này
            }
        }
    }
    return so_dao;
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Tìm kiếm theo chiều rộng (Breadth-First Search - BFS)</div>

**Phân tích thuật toán:** Trái ngược với DFS, BFS sẽ lan tỏa đều ra mọi hướng cùng một lúc giống như sóng nước. Nó thăm tất cả các ô cách điểm xuất phát 1 bước, rồi mới thăm các ô cách 2 bước. BFS bắt buộc phải sử dụng Hàng đợi (Queue).
- **Độ phức tạp:** Thời gian $O(N \times M)$, Không gian $O(N \times M)$ (Kích thước Queue).
- **Khi nào sử dụng:** Là "Vũ khí tối thượng" để tìm Đường đi ngắn nhất (Shortest Path) trên ma trận không có trọng số (hoặc trọng số các bước đều bằng 1).

**Ví dụ minh họa:** Đường ra khỏi mê cungMa trận có ô S (Start), ô E (End), . (đường đi), # (tường). Tìm số bước ít nhất để đi từ S đến E.

**Các bước cài đặt:**
1. Dùng một `queue` lưu trữ tọa độ các ô và một mảng `dist[][]` để lưu khoảng cách (số bước).
2. Đẩy điểm `S` vào `queue`, đánh dấu khoảng cách là $0$.
3. Chừng nào queue chưa rỗng:Lấy ô ở đầu hàng đợi ra (gọi là ô hiện tại).
4. Nếu là ô E, trả về kết quả dist.Sinh ra 4 ô kề cạnh. Nếu hợp lệ (trong bảng, không phải tường, chưa thăm), cập nhật khoảng cách dist[next] = dist[curr] + 1 và đẩy ô đó vào `queue`.

**Code mẫu C++:**
```cpp
struct ToaDo {
    int r, c;
};

int dx[4] = {-1, 1, 0, 0};
int dy[4] = {0, 0, -1, 1};

int BFS_MeCung(vector<vector<char>>& grid, ToaDo S, ToaDo E) {
    int N = grid.size();
    int M = grid[0].size();
    
    // Mảng lưu khoảng cách, khởi tạo -1 (chưa thăm)
    vector<vector<int>> dist(N, vector<int>(M, -1));
    queue<ToaDo> q;

    // Bắt đầu từ S
    q.push(S);
    dist[S.r][S.c] = 0; // Số bước tại vạch xuất phát là 0

    while (!q.empty()) {
        ToaDo hien_tai = q.front();
        q.pop();

        // Nếu đã đến đích, trả về số bước
        if (hien_tai.r == E.r && hien_tai.c == E.c) {
            return dist[hien_tai.r][hien_tai.c];
        }

        // Lan tỏa 4 hướng
        for (int k = 0; k < 4; k++) {
            int next_r = hien_tai.r + dx[k];
            int next_c = hien_tai.c + dy[k];

            // Kiểm tra: Trong bảng, không phải tường, và CHƯA THĂM (dist == -1)
            if (next_r >= 0 && next_r < N && next_c >= 0 && next_c < M 
                && grid[next_r][next_c] != '#' && dist[next_r][next_c] == -1) {
                
                // Khoảng cách ô mới = Khoảng cách ô cũ + 1
                dist[next_r][next_c] = dist[hien_tai.r][hien_tai.c] + 1;
                q.push({next_r, next_c});
            }
        }
    }
    return -1; // Không có đường tới đích
}
```
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Tổng kết & Lưu ý sống còn (CP Tips)</div>
<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 20%;">Tiêu chí</th><th style="width: 40%;">DFS (Chiều sâu)</th><th style="width: 40%;">BFS (Chiều rộng)</th></tr></thead><tbody><tr><td><b>Cấu trúc dữ liệu</b></td><td>Đệ quy (Call Stack)</td><td>Hàng đợi (Queue)</td></tr><tr style="background-color: #f8fafc;"><td><b>Mục đích chính</b></td><td>Vét cạn, loang vùng liên thông, kiểm tra tính kết nối.</td><td>Tìm <b>đường đi ngắn nhất</b> trên đồ thị không trọng số.</td></tr><tr><td><b>Rủi ro bùng nổ</b></td><td>Ma trận quá to (vd $1000 \times 1000$) có thể gây <b>Tràn bộ nhớ Stack (Stack Overflow)</b> do đệ quy quá sâu $\rightarrow$ Sinh ra lỗi <i>Runtime Error</i>.</td><td>Queue chiếm nhiều RAM nhưng nằm trên Heap nên an toàn hơn. Rất hiếm khi bị tràn bộ nhớ.</td></tr><tr style="background-color: #f8fafc;"><td><b>Lời khuyên thi đấu</b></td><td>Code DFS ngắn và dễ viết hơn nhiều. Nếu chỉ cần đếm hòn đảo, hãy dùng DFS cho nhanh.</td><td>Cứ thấy chữ <b>"ít bước nhất", "ngắn nhất", "nhanh nhất"</b> thì tự động lấy <b>BFS</b> ra dùng, không phải suy nghĩ!</td></tr></tbody></table></div>

<div class="important-note"><ul><li><b>Kỹ thuật mảng hướng đi (dx, dy):</b> Bắt buộc học sinh phải quen với việc dùng mảng <code>dx, dy</code>. Viết 4 lệnh <code>if</code> thủ công cực kỳ dễ sai sót và làm code rất dài. Nếu bài toán cho phép đi chéo (8 hướng), chỉ cần mở rộng mảng <code>dx, dy</code> thành 8 phần tử là xong.</li><li><b>Điều kiện Boundaries (Biên):</b> Lỗi phổ biến nhất khi code DFS/BFS trên ma trận là gọi <code>A[next_i][next_j]</code> <b>TRƯỚC KHI</b> kiểm tra xem chỉ số có bị âm hay vượt quá $N, M$ không. Hãy luôn nhớ: Kiểm tra biên trước, gọi mảng sau!</li></ul>
</div>
</div>