## ✅ Array (mảng tĩnh)

<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Định nghĩa</div>

Mảng tĩnh là một tập hợp các phần tử có **cùng kiểu dữ liệu**, nằm liên tiếp nhau trong bộ nhớ. Số lượng phần tử của mảng phải được xác định ngay khi khai báo và không thay đổi trong suốt quá trình chạy chương trình.

<table class="garden-table">
    <tr class="idx-row">
        <td>Index (Chỉ số)</td>
        <td>0</td>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
    </tr>
    <tr>
        <td class="idx-row">Value (Giá trị)</td>
        <td class="val-cell">10</td>
        <td class="val-cell">25</td>
        <td class="val-cell">-5</td>
        <td class="val-cell">18</td>
        <td class="val-cell">42</td>
    </tr>
</table>

💡 **Lưu ý:** Trong C++, mảng luôn bắt đầu từ chỉ số **0**. Nếu mảng có $n$ phần tử, chỉ số sẽ chạy từ $0$ đến $n-1$.

</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Khai báo mảng</div>
    Có hai cách khai báo phổ biến:

```cpp
// Cách 1: Khai báo mảng bình thường (chưa có giá trị)
int A[100005]; 

// Cách 2: Khai báo và khởi tạo giá trị ban đầu
int B[5] = {10, 20, 30, 40, 50};
```

🚩 **Quy tắc vàng trong thi đấu:**
- **Khai báo toàn cục:** Nên khai báo mảng bên ngoài hàm `main()` để có thể sử dụng bộ nhớ lớn hơn và các phần tử tự động được gán bằng $0$.
- **Dư 5 phần tử:** Luôn khai báo mảng lớn hơn giới hạn của đề bài $5$ đơn vị (Ví dụ: đề cho $N=10^5$, ta khai báo $100005$).
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Nhập, Duyệt và Truy xuất</div>

```cpp
#include <iostream>
#include <algorithm> // Thư viện dùng hàm sort

using namespace std;

// Khai báo mảng dư 5 đơn vị so với giới hạn 10^5
const int MAXN = 100005;
int a[MAXN];
int n;

int main() {
    // 1. Nhập mảng khi biết trước số phần tử n
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i]; // Nhập giá trị cho phần tử thứ i
    }

    // 2. Sắp xếp mảng TĂNG DẦN
    sort(a, a + n);

    // 3. Sắp xếp mảng GIẢM DẦN
    sort(a, a + n, greater<int>());

    // 4. Duyệt và in các phần tử trong mảng
    for (int i = 0; i < n; i++) {
        cout << a[i] << " ";
    }

    return 0;
}
```

💡 <b>Mẹo nhập mảng không biết trước số phần tử:</b>
```cpp
while (cin >> a[n]) { n++; }
```
</div>