## 📦 Vector (mảng động)

<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Khái niệm</div>
    
**Vector** là một container trong thư viện **STL** của **C++**, hoạt động như một mảng động có khả năng tự thay đổi kích thước khi thêm hoặc xóa phần tử.
    
* **Ưu điểm:**
    * Không cần khai báo kích thước cố định ban đầu.
    * Tự động quản lý bộ nhớ, tránh lãng phí vùng nhớ thừa.
    * Hỗ trợ nhiều hàm tiện ích để thêm, xóa, sắp xếp nhanh chóng.
* **Nhược điểm:**
    * Tốn bộ nhớ hơn mảng tĩnh một chút (để dự phòng việc mở rộng).
    * Thao tác chèn/xóa ở đầu hoặc giữa vector chậm ($O(n)$) hơn so với mảng liên kết.
    * Khó xem dữ liệu khi thực hiện debug
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Các kiểu khai báo</div>

```cpp
// 1. Khai báo vector rỗng kiểu int
vector<int> v; 

// 2. Khai báo và khởi tạo giá trị ban đầu
vector<int> a = {10, 20, 30};

// 3. Khai báo biết trước số phần tử (ví dụ 10 phần tử, mặc định là số 0)
vector<int> b(10); 

// 4. Khai báo n phần tử, mỗi phần tử có giá trị x (ví dụ 10 số 5)
vector<int> c(10, 5);

// 5. Khai báo ma trận vector (Mảng 2 chiều động)
// Khai báo ma trận R hàng, C cột giá trị 0
int R = 5, C = 10;
vector<vector<int>> matrix(R, vector<int>(C, 0));
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">3. Khái niệm Iterator và ứng dụng</div>

**Iterator** giống như một **"con trỏ thông minh"** dùng để duyệt qua các phần tử trong container.
```cpp
vector<int> v = {1, 2, 3};
// Trỏ đến phần tử đầu tiên của vector.
v.begin(): 

// Trỏ đến vị trí sau phần tử cuối cùng của vector.
v.end(): 

// Khai báo iterator
vector<int>::iterator it = v.begin();

// Ứng dụng: Dùng trong các hàm STL như sort, reverse
sort(v.begin(), v.end()); // Sắp xếp toàn bộ vector
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">4. Các thao tác và hàm xử lý</div>

```cpp
vector<int> v = {10, 20, 30, 40};

// --- TRUY XUẤT VÀ GÁN ---
int n = v.size();        // Lấy kích thước (Kết quả: 4)
int x = v[0];            // Truy xuất phần tử đầu (Kết quả: 10)
v[1] = 99;               // Gán giá trị mới cho phần tử index 1

vector<int> v2 = v;      // Gán nguyên cả vector v cho v2 (Copy hoàn toàn)

// --- DUYỆT VECTOR ---
// Cách 1: Dùng for index
for(int i = 0; i < v.size(); i++) cout << v[i] << " ";

// Cách 2: Dùng Iterator
for(auto it = v.begin(); it != v.end(); it++) cout << *it << " ";

// Cách 3: For-each (C++11 trở lên)
for(int x : v) cout << x << " ";

// --- THÊM PHẦN TỬ ---
v.push_back(50);         // Thêm 50 vào cuối [10, 99, 30, 40, 50]
v.insert(v.begin(), 5);  // Chèn 5 vào đầu (Vị trí begin)
v.insert(v.begin() + 2, 100); // Chèn 100 vào index 2

// --- XÓA PHẦN TỬ ---
v.pop_back();            // Xóa phần tử cuối cùng
v.erase(v.begin());      // Xóa phần tử đầu tiên
v.erase(v.begin() + 1);  // Xóa phần tử tại index 1
v.clear();               // Xóa toàn bộ phần tử (Kích thước về 0)

// --- KIỂM TRA RỖNG ---
if (v.empty()) cout << "Vector dang rong!";
```

</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">💡 Sử dụng Vector với Index âm</div>

Để sử dụng vector với index âm yêu cầu vector phải được khai báo dưới dạng **biết trước số phần tử**

```cpp
vector<int> v(100);
for (int i = -10; i <= 10; i++)
    v[i] = i;
```
</div>