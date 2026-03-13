## 🚶 Queue (Hàng đợi)
<br>
<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Khái niệm</div>

**Queue** là một cấu trúc dữ liệu dạng container adapter trong thư viện **STL** của **C++**, hoạt động theo nguyên tắc **FIFO (First In, First Out - Vào trước ra trước)**.

**Đặc điểm:** * Tưởng tượng nó giống hệt như việc **xếp hàng mua vé**: Ai xếp hàng trước sẽ được phục vụ và đi ra trước, ai đến sau phải đứng vào cuối hàng.
* Thao tác thêm dữ liệu chỉ được thực hiện ở **cuối (back/rear)** hàng đợi.
* Thao tác xóa và lấy dữ liệu chỉ được thực hiện ở **đầu (front)** hàng đợi.
* Tốc độ cực kỳ tối ưu: Các thao tác cơ bản (thêm, xóa, xem đầu/cuối) đều có độ phức tạp là $O(1)$.
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Khai báo</div>

Để sử dụng Queue, bạn cần khai báo thư viện `<queue>`.

```cpp
#include <queue>

// Khai báo queue chứa các số nguyên
queue<int> q;

// Khai báo queue chứa tọa độ (dùng Struct/Pair)
queue<pair<int, int>> q_toa_do;
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Các hàm xử lý thông dụng</div>

Khác với Stack dùng `top()`, Queue có hai đầu nên sử dụng `front()` và `back()`.

```cpp
queue<int> q;

q.push(10);            // Thêm 10 vào cuối hàng đợi
q.push(20);            // Thêm 20 vào cuối hàng đợi (Hàng đợi: [10, 20])

q.size();              // Lấy số lượng phần tử hiện có (Kết quả: 2)
q.empty();             // Kiểm tra queue có rỗng không (Kết quả: false)

int dau = q.front();   // Lấy giá trị ở đầu hàng đợi (Kết quả: 10)
int cuoi = q.back();   // Lấy giá trị ở cuối hàng đợi (Kết quả: 20)

q.pop();               // Xóa phần tử ở ĐẦU hàng đợi (Xóa 10). Queue còn [20].
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">4. Duyệt Queue</div>

Giống y hệt như Stack, Queue **KHÔNG** hỗ trợ duyệt bằng chỉ số `q[i]` hay Iterator. Để duyệt, ta phải lấy phần tử ở `front` ra và `pop` nó đi liên tục.

```cpp
queue<int> q;
q.push(1); q.push(2); q.push(3);

// In ra các phần tử của queue theo đúng thứ tự vào 
// Lưu ý: Queue sẽ bị làm rỗng sau vòng lặp
while (!q.empty()) {
    cout << q.front() << " "; // Kết quả in ra: 1 2 3
    q.pop();                  // Xóa phần tử ở đầu
}
```
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Ứng dụng, Lưu ý và Mở rộng (Priority Queue)</div>

**Trường hợp áp dụng kinh điển của Queue:**
* **Tìm kiếm theo chiều rộng (BFS):** Đây là ứng dụng quan trọng số 1 của Queue. BFS loang ra các đỉnh kề cạnh theo từng lớp, ai gần được đưa vào Queue trước, xử lý trước. Tìm đường đi ngắn nhất trên lưới tọa độ không trọng số bắt buộc phải dùng Queue.
* **Xử lý xoay vòng:** Các bài toán lập lịch, truyền cờ, vòng tròn đếm số (Josephus problem).

**💡 Lưu ý SỐNG CÒN (Runtime Error):**
Tuyệt đối không gọi `q.front()`, `q.back()` hay `q.pop()` khi Queue đang rỗng. Luôn luôn phải bọc trong câu lệnh kiểm tra: `if (!q.empty()) { ... }`.

**🔥 MỞ RỘNG: Hàng đợi ưu tiên (Priority Queue)**
Cùng nằm trong thư viện `<queue>`, nhưng `priority_queue` không tuân theo luật "Vào trước ra trước". Nó luôn tự động sắp xếp để **phần tử lớn nhất (hoặc nhỏ nhất) luôn nổi lên đỉnh**, bất kể thứ tự bạn đẩy vào. Nó sử dụng cấu trúc Max-Heap / Min-Heap với tốc độ thêm/xóa là $O(\log N)$.

<div class="geometry-table-container">
    <table class="styled-table">
        <thead>
            <tr>
                <th style="width: 20%;">So sánh</th>
                <th style="width: 40%;">Queue thông thường</th>
                <th style="width: 40%;">Priority Queue</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><b>Lấy phần tử</b></td>
                <td>Dùng <code>q.front()</code></td>
                <td>Dùng <code>pq.top()</code> (giống Stack)</td>
            </tr>
            <tr style="background-color: #f8fafc;">
                <td><b>Thứ tự ra</b></td>
                <td>Vào trước ra trước</td>
                <td>Giá trị Lớn nhất / Nhỏ nhất ra trước</td>
            </tr>
            <tr>
                <td><b>Ứng dụng thi đấu</b></td>
                <td>Thuật toán BFS cơ bản</td>
                <td>Thuật toán Dijkstra (Đường đi ngắn nhất có trọng số)</td>
            </tr>
        </tbody>
    </table>
</div>

```cpp
// Khai báo Priority Queue mặc định (Phần tử lớn nhất nằm trên đỉnh)
priority_queue<int> pq_max;

// Khai báo Priority Queue tìm Min (Phần tử nhỏ nhất nằm trên đỉnh)
priority_queue<int, vector<int>, greater<int>> pq_min;
```
</div>