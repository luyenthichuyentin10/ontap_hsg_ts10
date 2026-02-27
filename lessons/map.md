## 🗺️ Map (Ánh xạ / Từ điển)

<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Khái niệm</div>
    
**Map** là một container trong **STL** của **C++**, lưu trữ dữ liệu dưới dạng các cặp **Key - Value** (Khóa - Giá trị).

<div style="display: flex; flex-direction: column; align-items: center; background: #1e293b; padding: 20px; border-radius: 12px; margin: 20px 0; border: 1px dashed #334155;">
        <p style="color: #fbbf24; font-weight: bold; margin-bottom: 15px;">📊 Mô phỏng cấu trúc Cặp (Pair) trong Map</p>
        <div style="display: flex; gap: 40px; justify-content: center; flex-wrap: wrap;">
            <div style="display: flex; border: 2px solid #569cd6; border-radius: 8px; overflow: hidden; background: #0f172a;">
                <div style="padding: 10px 15px; background: #569cd6; color: white; font-weight: bold;">Key: "apple"</div>
                <div style="padding: 10px 15px; color: #4ec9b0;">Value: 10</div>
            </div>
            <div style="display: flex; border: 2px solid #569cd6; border-radius: 8px; overflow: hidden; background: #0f172a;">
                <div style="padding: 10px 15px; background: #569cd6; color: white; font-weight: bold;">Key: "banana"</div>
                <div style="padding: 10px 15px; color: #4ec9b0;">Value: 20</div>
            </div>
            <div style="display: flex; border: 2px solid #569cd6; border-radius: 8px; overflow: hidden; background: #0f172a;">
                <div style="padding: 10px 15px; background: #569cd6; color: white; font-weight: bold;">Key: "orange"</div>
                <div style="padding: 10px 15px; color: #4ec9b0;">Value: 15</div>
            </div>
        </div>
        <p style="color: #94a3b8; font-size: 0.85rem; margin-top: 15px;"><i>(Các phần tử được sắp xếp tự động theo thứ tự từ điển của Key)</i></p>
</div>

**Đặc điểm:**
* Mỗi một **Key** là duy nhất trong Map.
* Các phần tử luôn được tự động sắp xếp tăng dần theo **Key**.
* Được cài đặt bằng cây nhị phân tìm kiếm cân bằng, giúp thao tác tìm kiếm đạt độ phức tạp $O(\log n)$.
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Khai báo</div>

Để sử dụng, ta cần khai báo kiểu dữ liệu cho cả **khóa (key)** và **giá trị (value)**:

```cpp
// Khai báo map có Key là string, Value là int
map<string, int> mp;

// Khai báo và khởi tạo giá trị
map<int, string> sv = {{1, "An"}, {2, "Binh"}, {3, "Chi"}};
```
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Các hàm và thao tác cơ bản</div>

```cpp
map<string, int> mp;

// 1. Thêm phần tử
mp["apple"] = 10;
mp.insert({"banana", 20});

// 2. Truy xuất giá trị qua Key
cout << mp["apple"]; // Kết quả: 10

// 3. Kiểm tra sự tồn tại
if (mp.count("orange")) cout << "Tim thay cam";

// 4. Xóa phần tử
mp.erase("apple"); // Xóa phần tử có Key là "apple"

// 5. Lấy kích thước
int n = mp.size();
```
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">4. Duyệt Map</div>

Mỗi phần tử trong **map** là một **pair** gồm `.first` (key) và `.second` (value).

```cpp
map<string, int> mp = {{"A", 1}, {"B", 2}, {"C", 3}};

// Cách 1: Duyệt bằng for-each (C++11)
for (auto x : mp) {
    cout << x.first << " : " << x.second << endl;
}

// Cách 2: Duyệt bằng Iterator
for (map<string, int>::iterator it = mp.begin(); it != mp.end(); ++it) {
    cout << it->first << " : " << it->second << endl;
}
```
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Phân biệt Map, Multimap và Unordered_map</div>

<table class="garden-table" style="table-layout: fixed; width: 100%;">
    <colgroup>
        <col style="width: 25%;">
        <col style="width: 25%;">
        <col style="width: 25%;">
        <col style="width: 25%;">
    </colgroup>
    <tr class="idx-row">
        <td>Đặc điểm</td>
        <td>Map</td>
        <td>Multimap</td>
        <td>Unordered_map</td>
    </tr>
    <tr>
        <td class="idx-row">Key trùng</td>
        <td class="val-cell">Không</td>
        <td class="val-cell">Có</td>
        <td class="val-cell">Không</td>
    </tr>
    <tr>
        <td class="idx-row">Sắp xếp Key</td>
        <td class="val-cell">Tăng dần</td>
        <td class="val-cell">Tăng dần</td>
        <td class="val-cell">Không</td>
    </tr>
    <tr>
        <td class="idx-row">Độ phức tạp</td>
        <td class="val-cell">$O(\log n)$</td>
        <td class="val-cell">$O(\log n)$</td>
        <td class="val-cell">$O(1)$ trung bình</td>
    </tr>
</table>

**Trường hợp áp dụng:**
* **Map:** Khi cần đếm tần suất xuất hiện và in ra theo thứ tự từ điển (Vd: Đếm số lần xuất hiện của các từ trong văn bản).
* **Unordered_map:** Khi cần tốc độ truy xuất nhanh nhất và không quan tâm thứ tự (Vd: Dùng làm bảng băm kiểm tra ID người dùng).

<div class="important-note">
💡 <b>Lưu ý:</b> Khi truy cập vào một Key chưa tồn tại bằng toán tử <b>mp[key]</b>, Map sẽ tự động tạo ra một phần tử mới với giá trị mặc định (số là 0, xâu là rỗng). Để tránh điều này, hãy dùng hàm <b>mp.count(key)</b> hoặc <b>mp.find(key)</b> trước khi truy xuất.
</div>
</div>

