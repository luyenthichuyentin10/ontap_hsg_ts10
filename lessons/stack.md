## 🥞 Stack (Ngăn xếp)
<br>
<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Khái niệm</div>

**Stack** là một cấu trúc dữ liệu dạng container adapter trong thư viện **STL** của **C++**, hoạt động theo nguyên tắc **LIFO (Last In, First Out - Vào sau ra trước)**.

**Đặc điểm:** 
* Phần tử nào được thêm vào sau cùng sẽ được lấy ra đầu tiên (tưởng tượng giống như một chồng đĩa, bạn chỉ có thể lấy chiếc đĩa nằm trên cùng ra trước).
* Các thao tác thêm, xóa và lấy dữ liệu chỉ được thực hiện ở một đầu duy nhất gọi là **đỉnh (top)** của ngăn xếp.
* Tốc độ cực kỳ tối ưu: Các thao tác cơ bản (thêm, xóa, xem đỉnh) đều có độ phức tạp là $O(1)$.
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Khai báo</div>

Để sử dụng Stack, bạn cần phải khai báo thư viện `<stack>`.

```cpp
#include <stack>

// Khai báo stack chứa các số nguyên
stack<int> st;

// Khai báo stack chứa kiểu dữ liệu chuỗi
stack<string> str_st;
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Các hàm xử lý thông dụng</div>

```cpp
stack<int> st;

st.push(10);           // Thêm 10 vào đỉnh stack
st.push(20);           // Thêm 20 vào đỉnh stack (hiện tại đỉnh là 20)

st.size();             // Lấy số lượng phần tử hiện có (Kết quả: 2)
st.empty();            // Kiểm tra stack có rỗng không (Kết quả: false)

int top_val = st.top(); // Lấy giá trị ở đỉnh stack (Kết quả: 20). 
                       // Lưu ý: hàm này KHÔNG xóa phần tử.

st.pop();              // Xóa phần tử ở đỉnh stack (Xóa 20). Đỉnh hiện tại quay về 10.
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">4. Duyệt Stack</div>

Khác với Set hay Vector, Stack **KHÔNG** hỗ trợ duyệt bằng chỉ số `st[i]`, Iterator hay vòng lặp For-each. Để duyệt qua các phần tử của Stack, ta bắt buộc phải lấy phần tử ở đỉnh ra và xóa nó đi liên tục cho đến khi Stack rỗng.

```cpp
stack<int> st;
st.push(1); st.push(2); st.push(3);

// In ra các phần tử của stack 
// Lưu ý: Stack sẽ bị làm rỗng hoàn toàn sau vòng lặp này
while (!st.empty()) {
    cout << st.top() << " "; // Lấy phần tử ở đỉnh (In ra thứ tự: 3 2 1)
    st.pop();                // Xóa phần tử ở đỉnh
}
```
*Mẹo:* Nếu bài toán yêu cầu phải duyệt mà vẫn giữ nguyên Stack ban đầu, bạn cần phải gán nó sang một Stack tạm (`stack<int> tam = st;`) và thực hiện thao tác duyệt trên Stack tạm đó.
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Ứng dụng và Lưu ý quan trọng</div>

**Trường hợp áp dụng kinh điển của Stack:**
* **Kiểm tra dãy ngoặc hợp lệ:** (Dấu ngoặc mở được đẩy vào stack, khi gặp dấu ngoặc đóng sẽ kiểm tra và bắt cặp với đỉnh stack).
* **Đảo ngược dữ liệu:** (Đảo ngược chuỗi, mảng, chuyển đổi cơ số từ thập phân sang nhị phân) do tính chất "Vào sau ra trước".
* **Stack đơn điệu (Monotonic Stack):** Dùng để giải quyết siêu nhanh dạng bài "Tìm phần tử lớn hơn/nhỏ hơn gần nhất ở bên trái/phải của một phần tử trong mảng".
* **Duyệt đồ thị / Khử đệ quy:** Thuật toán DFS (Tìm kiếm theo chiều sâu) có bản chất là sử dụng Stack của hệ thống. Ta có thể tự dùng Stack để viết DFS mà không lo bị tràn bộ nhớ đệ quy (Stack Overflow).

**💡 Lưu ý SỐNG CÒN (Lỗi Runtime Error):** Trước khi gọi hàm `st.top()` hoặc `st.pop()`, bạn **bắt buộc** phải kiểm tra xem Stack có đang rỗng hay không bằng `!st.empty()`. Nếu bạn cố gắng xem đỉnh hoặc xóa phần tử trên một Stack đã rỗng, chương trình sẽ lập tức bị "văng" với lỗi **Segmentation Fault (Runtime Error)**.

```cpp
// ❌ CÁCH VIẾT SAI (Dễ dính Runtime Error trên hệ thống chấm)
int x = st.top(); 
st.pop();

// ✅ CÁCH VIẾT CHUẨN
if (!st.empty()) {
    int x = st.top();
    st.pop();
}
```
</div>