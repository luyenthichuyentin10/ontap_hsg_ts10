## 💰 Tư tưởng Lập trình Tham lam (Greedy Algorithm)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Đặc điểm</div>

- **Khái niệm:** Tư tưởng Tham lam (Greedy) là phương pháp giải quyết bài toán bằng cách luôn đưa ra lựa chọn tốt nhất ngay tại thời điểm hiện tại (tối ưu cục bộ) với hy vọng rằng chuỗi các lựa chọn đó sẽ dẫn đến kết quả tốt nhất cho toàn bộ bài toán (tối ưu toàn cục).Nói cách khác, giống như một người "tham lam", thuật toán chỉ nhìn vào lợi ích ngay trước mắt mà không cần quan tâm đến hậu quả về sau.
- **Đặc điểm:** 
    - *Ưu điểm:* Thuật toán chạy cực kỳ nhanh, tốn ít bộ nhớ và mã nguồn thường rất ngắn gọn, dễ hiểu.
    - *Nhược điểm:* Không có cơ chế quay lui (không sửa sai). Lựa chọn đã đưa ra là không thể thay đổi. Do đó, không phải bài toán nào dùng Tham lam cũng cho ra kết quả đúng.
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">2. Điều kiện áp dụng (Khi nào được phép Tham lam?)</div>

Để thuật toán Tham lam cho ra kết quả chính xác, bài toán bắt buộc phải thỏa mãn 2 tính chất toán học sau:
- **Tính chất lựa chọn tham lam (Greedy Choice Property):** Lựa chọn tối ưu cục bộ tại mỗi bước chắc chắn không làm mất đi cơ hội đạt được giải pháp tối ưu toàn cục. Ta có thể chứng minh được lựa chọn này là an toàn.
- **Cấu trúc con tối ưu (Optimal Substructure):** Giải pháp tối ưu cho bài toán lớn được tạo thành từ giải pháp tối ưu của các bài toán con nhỏ hơn (giống như Quy hoạch động).
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">3. Các bước giải bài toán bằng Tham lam</div>

### Ba bước cốt lõi:
- **Bước 1 sắp xếp dữ liệu (Sort):** Đây là bước quan trọng nhất. Dữ liệu thường phải được sắp xếp theo một tiêu chí nào đó (tăng dần, giảm dần, hoặc theo tỷ lệ).
- **Bước 2 lựa chọn Tham lam:** Duyệt qua tập dữ liệu đã sắp xếp, liên tục lấy phần tử tốt nhất thỏa mãn điều kiện bài toán.
- **Bước 3 cập nhật và Lặp lại:** Cập nhật lại kết quả và điều kiện giới hạn, lặp lại bước 2 cho đến khi đạt mục tiêu hoặc hết dữ liệu.

### Ví dụ minh họa: 
Bài toán Đổi Tiền (mệnh giá chuẩn) có các tờ tiền mệnh giá $\{1, 2, 5, 10, 20, 50\}$. Cần đổi số tiền $S = 68$ sao cho số lượng tờ tiền là ít nhất.

- **Tư tưởng tham lam:** Ưu tiên chọn tờ tiền có mệnh giá lớn nhất có thể.
- **Các bước chọn:**
    - Lấy 1 tờ 50 (còn 18).
    - Lấy 1 tờ 10 (còn 8).
    - Lấy 1 tờ 5 (còn 3).
    - Lấy 1 tờ 2 (còn 1).
    - Lấy 1 tờ 1 (còn 0).
    - **Kết quả:** 5 tờ (50, 10, 5, 2, 1).
### Code mẫu C++ 
```cpp
void doiTienThamLam(int S) {
    // Bước 1: Dữ liệu đã được sắp xếp giảm dần
    vector<int> menh_gia = {50, 20, 10, 5, 2, 1}; 
    int so_to_tien = 0;

    cout << "Cac to tien duoc chon: ";
    // Bước 2 & 3: Lựa chọn và cập nhật
    for (int i = 0; i < menh_gia.size(); i++) {
        while (S >= menh_gia[i]) {
            S -= menh_gia[i]; // Cập nhật số tiền còn lại
            so_to_tien++;
            cout << menh_gia[i] << " ";
        }
    }
    cout << "\nTong so to tien it nhat: " << so_to_tien << endl;
}
```
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Các dạng bài thường gặp ở THCS</div>

### Dấu hiệu nhận biết chung:
- Đề bài yêu cầu tìm **Lớn nhất (Max), Nhỏ nhất (Min), Số lượng nhiều nhất, ít nhất**.
- Dữ liệu các phần tử độc lập với nhau, có thể sắp xếp lại mà không làm hỏng logic bài toán.

### Dạng 1: Chọn số lượng nhiều nhất với chi phí tối thiểu (hoặc ngược lại)
- **Dấu hiệu nhận biết:** Bài toán cho một ngân sách (hoặc sức chứa) có hạn là $S$. Yêu cầu chọn ra nhiều đồ vật nhất có thể sao cho tổng chi phí không vượt quá $S$. (Hoặc ngược lại: Cần chọn đủ số lượng $K$ món đồ với chi phí thấp nhất / lợi nhuận cao nhất).
- **Ví dụ kinh điển (mua quà siêu thị):** Có $N$ món quà với giá tiền lần lượt là $A_1, A_2, \dots, A_N$. Bạn có số tiền là $S$. Hỏi bạn có thể mua được tối đa bao nhiêu món quà? (Mỗi món chỉ mua 1 lần
- **Phân tích tư tưởng tham lam:** Để mua được số lượng nhiều nhất, ta phải ưu tiên mua những món quà có giá rẻ nhất. Cứ mua liên tục từ rẻ đến đắt cho tới khi cạn tiền.
- **Code mẫu**
```cpp
int muaNhieuNhat(vector<int>& gia_tien, int S) {
    // Bước 1: Sắp xếp giá tiền tăng dần (Rẻ nhất xếp trước)
    sort(gia_tien.begin(), gia_tien.end());

    int so_luong = 0;
    
    // Bước 2 & 3: Lựa chọn và cập nhật
    for (int i = 0; i < gia_tien.size(); i++) {
        // Nếu số tiền còn lại đủ mua món quà hiện tại
        if (S >= gia_tien[i]) {
            S -= gia_tien[i]; // Trừ tiền
            so_luong++;       // Tăng số lượng quà
        } else {
            // Vì mảng đã sắp xếp tăng dần, nếu không mua nổi món này
            // thì chắc chắn không mua nổi các món đắt hơn phía sau -> Dừng luôn.
            break; 
        }
    }
    
    return so_luong;
}
```
<div class="important-note">

**💡Lưu ý:** Ngược lại, nếu bài toán yêu cầu chọn $K$ phần tử sao cho tổng giá trị là lớn nhất (ví dụ: chọn $K$ học sinh giỏi nhất), ta chỉ cần sắp xếp mảng giảm dần và lấy đúng $K$ phần tử đầu tiên.
</div>

### Dạng 2: Bài toán Lựa chọn hoạt động (Xếp lịch / Activity Selection)
- **Ví dụ:** Cho $N$ cuộc họp, mỗi cuộc họp có giờ bắt đầu $S_i$ và kết thúc $E_i$. Một hội trường chỉ chứa được 1 cuộc họp tại 1 thời điểm. Chọn sao cho số cuộc họp diễn ra được nhiều nhất.
- **Phân tích tư tưởng tham lam:** Để xếp được nhiều cuộc họp nhất, ta phải ưu tiên **cuộc họp kết thúc sớm nhất** để chừa lại nhiều thời gian trống nhất cho các cuộc họp phía sau.
- **Code mẫu** 
```cpp
struct CuocHop {
    int bat_dau, ket_thuc;
};

// Hàm sắp xếp: Ưu tiên cuộc họp KẾT THÚC sớm nhất
bool soSanh(CuocHop a, CuocHop b) {
    return a.ket_thuc < b.ket_thuc;
}

int demCuocHop(vector<CuocHop>& ds_hop) {
    // Bước 1: Sắp xếp
    sort(ds_hop.begin(), ds_hop.end(), soSanh);

    int dem = 1; // Luôn chọn cuộc họp đầu tiên sau khi sắp xếp
    int thoi_gian_hien_tai = ds_hop[0].ket_thuc;

    // Bước 2: Duyệt và chọn
    for (int i = 1; i < ds_hop.size(); i++) {
        // Nếu cuộc họp tiếp theo bắt đầu sau khi cuộc họp trước kết thúc
        if (ds_hop[i].bat_dau >= thoi_gian_hien_tai) {
            dem++;
            thoi_gian_hien_tai = ds_hop[i].ket_thuc; // Bước 3: Cập nhật
        }
    }
    return dem;
}
```

### Dạng 3: Tham lam dựa trên tỷ trọng (Fractional Knapsack)
- **Ví dụ:** Có các mỏ vàng, mỏ $i$ có khối lượng $W_i$ và giá trị $V_i$. Ta được mang theo một cái bao có sức chứa tối đa là $M$. Ta có thể lấy một phần của mỏ vàng (chặt nhỏ ra). Lấy sao cho tổng giá trị là lớn nhất.
- **Phân tích tư tưởng:** Vì được phép lấy một phần, ta chỉ cần tính Đơn giá = Giá trị / Khối lượng. Ưu tiên hốt hết những mỏ vàng có đơn giá cao nhất trước, còn dư chỗ mới hốt mỏ rẻ hơn.
- **Code mẫu** 
```cpp
// Cấu trúc mô tả một mỏ vàng
struct MoVang {
    double khoi_luong;
    double gia_tri;
    double don_gia; // Tiêu chí quan trọng nhất để tham lam
};

// Hàm sắp xếp: Ưu tiên mỏ vàng có ĐƠN GIÁ cao nhất (giảm dần)
bool soSanhDonGia(MoVang a, MoVang b) {
    return a.don_gia > b.don_gia;
}

double layVangToiDa(vector<MoVang>& ds_mo, double suc_chua_M) {
    // Bước 1: Tính đơn giá cho từng mỏ và Sắp xếp
    for (int i = 0; i < ds_mo.size(); i++) {
        // Lưu ý: Đảm bảo biến là double để phép chia không bị làm tròn mất số thập phân
        ds_mo[i].don_gia = ds_mo[i].gia_tri / ds_mo[i].khoi_luong;
    }
    sort(ds_mo.begin(), ds_mo.end(), soSanhDonGia);

    double tong_gia_tri = 0.0;

    // Bước 2 & 3: Lựa chọn tham lam và cập nhật
    for (int i = 0; i < ds_mo.size(); i++) {
        // Thoát sớm nếu bao đã đầy
        if (suc_chua_M <= 0) break; 

        // Trường hợp 1: Sức chứa còn đủ lấy TOÀN BỘ mỏ vàng hiện tại
        if (suc_chua_M >= ds_mo[i].khoi_luong) {
            tong_gia_tri += ds_mo[i].gia_tri;
            suc_chua_M -= ds_mo[i].khoi_luong; // Cập nhật lại sức chứa
        } 
        // Trường hợp 2: Sức chứa KHÔNG ĐỦ, bắt buộc phải chặt nhỏ mỏ vàng (Lấy phần lẻ)
        else {
            tong_gia_tri += ds_mo[i].don_gia * suc_chua_M; // Chỉ lấy phần giá trị tương ứng
            suc_chua_M = 0; // Chắc chắn bao đã đầy sau bước này
        }
    }

    return tong_gia_tri;
}
```
</div>

<div class="step-card border-red">
<div class="step-badge bg-red">5. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div> 

### 💡 Cảnh báo cho học sinh:
<div class="important-note">

- Tham lam là con dao hai lưỡi. Cảm giác code xong rất nhanh và dễ, nhưng nếu tư tưởng sai từ đầu thì sẽ sai toàn bộ các test khó.
- Mẹo khi quyết định giải bài toán bằng tham lam:
    - 1️⃣ Thử sắp xếp dữ liệu trước
    - 2️⃣ Tìm cách chọn tốt nhất tại mỗi bước
    - 3️⃣ Kiểm tra bằng ví dụ nhỏ
    - 4️⃣ Nếu sai → thử DP
- Những sai lầm thường gặp
    - ❌ Không chứng minh được Greedy đúng
    - ❌ Sắp xếp sai tiêu chí
    - ❌ Greedy nhưng bài cần Dynamic Programming
</div>

### 🚨 Các vấn đề cần phân tích và giải pháp khi áp dụng tư tưởng lập trình tham lam
<div class="geometry-table-container"><table class="styled-table"><thead><tr><th>Vấn đề</th><th>Phân tích & Giải pháp</th></tr></thead><tbody><tr><td><b>Hàm <code>sort()</code> là linh hồn</b></td><td>99% các bài toán Tham lam bắt đầu bằng việc Sắp xếp. Thường học sinh sẽ phải tự viết hàm <code>bool soSanh(A, B)</code> (Custom Comparator) để sắp xếp cấu trúc dữ liệu theo ý muốn.</td></tr><tr><td><b>Luôn tìm Phản ví dụ (Counter-example)</b></td><td>Trước khi code, hãy tự nhẩm xem: "Có trường hợp nào mình chọn tham lam nhưng kết quả lại không phải là tốt nhất không?". Nếu có, bài đó phải giải bằng Quy hoạch động (DP) hoặc Vét cạn.</td></tr><tr><td><b>Bài toán đổi tiền bị sai khi nào?</b></td><td>Nếu bộ tiền tệ không chuẩn, ví dụ mệnh giá là $\{1, 3, 4\}$. Cần đổi $S = 6$.- <b>Tham lam:</b> Chọn 4, còn 2 $\rightarrow$ chọn 1, 1 (Cần <b>3 tờ</b>).- <b>Thực tế tối ưu (DP):</b> Chọn 3, 3 (Chỉ cần <b>2 tờ</b>).</td></tr><tr><td><b>Nhầm lẫn với Quy hoạch động</b></td><td>Bài toán <b>Cái túi dạng nguyên</b> (0/1 Knapsack - Không được chặt nhỏ đồ vật) KHÔNG thể giải bằng tham lam đơn giá, bắt buộc phải dùng DP mảng 2 chiều. Thầy cô cần nhấn mạnh kỹ sự khác biệt này.</td></tr><tr><td><b>Kiểu dữ liệu (Overflow)</b></td><td>Khi gom những phần tử lớn nhất/nhỏ nhất lại với nhau, tổng rất dễ vượt quá giới hạn của <code>int</code> ($2 \cdot 10^9$). Luôn khai báo biến tổng là <code>long long</code>.</td></tr></tbody></table></div>

### ⚖️ So sánh Tham lam (Greedy) và Quy hoạch động (DP)
<div class="geometry-table-container">
<table class="styled-table">
<thead>
<tr>
<th style="width: 20%;">Tiêu chí</th>
<th style="width: 40%;">Tư tưởng Tham lam (Greedy)</th>
<th style="width: 40%;">Quy hoạch động (Dynamic Programming)</th>
</tr>
</thead>
<tbody>
<tr>
<td><b>Bản chất cốt lõi</b></td>
<td>Lựa chọn cái tốt nhất <b>ngay tại thời điểm hiện tại</b> (Tối ưu cục bộ).</td>
<td>Giải quyết mọi bài toán con rồi tổng hợp lại để tìm ra cái tốt nhất (Tối ưu toàn cục).</td>
</tr>
<tr style="background-color: #f8fafc;">
<td><b>Khả năng "sửa sai"</b></td>
<td><span style="color: #ea580c; font-weight: bold;">Không thể.</span> Đã chọn là chốt, không quan tâm hậu quả các bước sau.</td>
<td><span style="color: #16a34a; font-weight: bold;">Có thể.</span> Lưu lại kết quả trước đó để so sánh và thay đổi quyết định nếu cần.</td>
</tr>
<tr>
<td><b>Tốc độ & Bộ nhớ</b></td>
<td><b>Rất nhanh, tốn ít bộ nhớ.</b> Thường là $O(N \log N)$ (chỉ mất thời gian sắp xếp).</td>
<td><b>Chậm hơn, tốn nhiều bộ nhớ.</b> Thường là $O(N^2)$ hoặc mảng 2 chiều $O(N \cdot M)$.</td>
</tr>
<tr style="background-color: #f8fafc;">
<td><b>Độ chính xác</b></td>
<td>Chỉ đúng nếu bài toán thỏa mãn <b>Tính chất lựa chọn tham lam</b>. Rất dễ bị sai ở các Test Case đặc biệt.</td>
<td><b>Luôn đúng 100%</b> nếu xây dựng được trạng thái và công thức truy hồi chính xác.</td>
</tr>
<tr>
<td><b>Dấu hiệu nhận biết</b></td>
<td>
<ul>
<li>Các phần tử có thể đổi chỗ tự do.</li>
<li>Được phép chặt nhỏ đồ vật (Fractional Knapsack).</li>
<li>Đề bài có tính "tỷ lệ" rõ ràng.</li>
</ul>
</td>
<td>
<ul>
<li>Thứ tự các phần tử bị ràng buộc chặt chẽ.</li>
<li>Đồ vật phải lấy nguyên vẹn (0/1 Knapsack).</li>
<li>Cần thử nhiều trường hợp để chọn ra Max/Min.</li>
</ul>
</td>
</tr>
</tbody>
</table>
</div>
</div>