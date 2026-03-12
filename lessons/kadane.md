## 📈 Thuật Toán Kadane (Tổng Dãy Con Liên Tiếp Lớn Nhất)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

**Khái niệm:** Thuật toán Kadane là một phương pháp Quy hoạch động (Dynamic Programming) tối ưu hóa không gian, dùng để tìm ra một dãy con liên tiếp (Subarray) có tổng các phần tử đạt giá trị lớn nhất trong một mảng số nguyên 1 chiều.

**Điểm ưu việt:** Chỉ cần duyệt qua mảng đúng 1 lần duy nhất ($O(N)$ thời gian) và chỉ sử dụng thêm 2 biến để lưu trữ ($O(1)$ không gian).
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Phân tích thuật toán</div>

Tư tưởng cốt lõi của thuật toán Kadane dựa trên việc duy trì 2 biến số khi duyệt qua từng phần tử $A[i]$ của mảng:
1. **Tổng cục bộ (Max Ending Here):** Là tổng lớn nhất của dãy con liên tiếp bắt buộc phải kết thúc tại vị trí hiện tại $i$.
2. **Tổng toàn cục (Max So Far):** Là tổng lớn nhất từng được ghi nhận trong toàn bộ quá trình duyệt mảng. Đây chính là đáp án cuối cùng.
3. **Quy tắc "Cắt lỗ":** Khi ta đang cộng dồn các phần tử vào Tổng cục bộ, nếu tổng này bị rớt xuống mức âm ($< 0$), nó sẽ trở thành một "gánh nặng" cho các phần tử phía sau.

$\rightarrow$ Thay vì tiếp tục mang theo gánh nặng đó, ta vứt bỏ hoàn toàn dãy con cũ, "reset" Tổng cục bộ về $0$ và bắt đầu tính một dãy con mới từ phần tử tiếp theo.

4. **Công thức truy hồi:** 
    - `TongCucBo = max(A[i], TongCucBo + A[i])` (Hoặc nếu TongCucBo < 0 thì TongCucBo = 0, rồi cộng thêm $A[i]$).
    - `TongToanCuc = max(TongToanCuc, TongCucBo)`
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Ví dụ minh họa (Trace tay)</div>

Cho mảng $A = \{-2, 1, -3, 4, -1, 2, 1, -5, 4\}$. 

Khởi tạo `TongCucBo = 0`, `TongToanCuc = -VoCung`.

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 10%;">Bước $i$</th><th style="width: 15%;">Phần tử $A[i]$</th><th style="width: 35%;">Tổng cục bộ (Cộng dồn & Reset)</th><th style="width: 40%;">Tổng toàn cục (Max lớn nhất)</th></tr></thead><tbody><tr><td>0</td><td>-2</td><td>-2 $\rightarrow$ <b>Reset về 0</b> vì $&lt; 0$</td><td><b>-2</b></td></tr><tr style="background-color: #f8fafc;"><td>1</td><td>1</td><td>0 + 1 = <b>1</b></td><td>max(-2, 1) = <b>1</b></td></tr><tr><td>2</td><td>-3</td><td>1 + (-3) = -2 $\rightarrow$ <b>Reset về 0</b></td><td>max(1, -2) = <b>1</b></td></tr><tr style="background-color: #f8fafc;"><td>3</td><td>4</td><td>0 + 4 = <b>4</b></td><td>max(1, 4) = <b>4</b></td></tr><tr><td>4</td><td>-1</td><td>4 + (-1) = <b>3</b> <i>(Chưa âm, giữ lại!)</i></td><td>max(4, 3) = <b>4</b></td></tr><tr style="background-color: #f8fafc;"><td>5</td><td>2</td><td>3 + 2 = <b>5</b></td><td>max(4, 5) = <b>5</b></td></tr><tr><td>6</td><td>1</td><td>5 + 1 = <b>6</b></td><td>max(5, 6) = <b>6</b> <i>(Đáp án tìm được tại đây!)</i></td></tr><tr style="background-color: #f8fafc;"><td>7</td><td>-5</td><td>6 + (-5) = <b>1</b></td><td><b>6</b></td></tr><tr><td>8</td><td>4</td><td>1 + 4 = <b>5</b></td><td><b>6</b></td></tr></tbody></table></div>

Dãy con cho tổng lớn nhất là: ${4, -1, 2, 1}$ với tổng bằng $6$.

</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Code mẫu C++ (Kèm theo lưu vết vị trí)</div>

***Phiên bản dưới đây không chỉ tìm ra giá trị tổng lớn nhất, mà còn lưu lại được chỉ số bắt đầu và kết thúc của dãy con đó – một yêu cầu rất phổ biến trong đề thi**.
```cpp
void thuatToanKadane(const vector<int>& A) {
    long long tong_toan_cuc = -1e18; // Khởi tạo bằng số âm vô cùng lớn
    long long tong_cuc_bo = 0;
    
    int start = 0, end = 0, s_temp = 0;

    for (int i = 0; i < A.size(); i++) {
        tong_cuc_bo += A[i];

        // Nếu ghi nhận được một mức tổng mới lớn hơn
        if (tong_cuc_bo > tong_toan_cuc) {
            tong_toan_cuc = tong_cuc_bo;
            start = s_temp; // Chốt vị trí bắt đầu thực sự
            end = i;        // Chốt vị trí kết thúc thực sự
        }

        // Nếu tổng cục bộ bị âm -> Vứt bỏ đoạn này, bắt đầu đoạn mới từ i + 1
        if (tong_cuc_bo < 0) {
            tong_cuc_bo = 0;
            s_temp = i + 1; // Đánh dấu điểm bắt đầu tạm thời cho dãy con mới
        }
    }

    cout << "Tong lon nhat: " << tong_toan_cuc << "\n";
    cout << "Day con tu vi tri " << start << " den " << end << "\n";
}

int main() {
    vector<int> A = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    thuatToanKadane(A);
    return 0;
}
```
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">5. Ứng dụng nâng cao (CP Tips)</div>

### 1. Dãy con có tổng lớn nhất trên mảng vòng (Circular Subarray Sum)
- **Phân tích thuật toán:** Trong mảng vòng, phần tử cuối cùng được nối liền với phần tử đầu tiên. Do đó, dãy con có tổng lớn nhất sẽ rơi vào 1 trong 2 trường hợp:
    - Trường hợp 1 (Không vòng): Dãy con nằm trọn ở đoạn giữa mảng. Trường hợp này ta chỉ cần chạy thuật toán Kadane bình thường là tìm được (gọi là Max_Kadane).
    - Trường hợp 2 (Có vòng): Dãy con bị vắt ngang qua 2 đầu mảng (lấy một ít ở đầu và một ít ở cuối). Phần không bị lấy chính là một dãy con nằm ở giữa mảng có tổng nhỏ nhất.
    - $\rightarrow$ Công thức: **Tổng phần có vòng = Tổng toàn mảng - Min_Kadane** (với Min_Kadane là tổng dãy con nhỏ nhất tìm bằng thuật toán Kadane đảo ngược).
- **Ví dụ minh họa:** Cho mảng $A = \{5, -3, 5\}$.
    - Tổng toàn mảng = $5 + (-3) + 5 = 7$.
    - Chạy Max_Kadane (Tìm Max): Kết quả là $5$.
    - Chạy Min_Kadane (Tìm Min): Kết quả là $-3$.
    - Tổng phần có vòng = $7 - (-3) = 10$.
    - So sánh: $\max(5, 10) = 10$. Dãy con lấy số $5$ ở đầu và $5$ ở cuối. Kết quả là $10$!
- **Code mẫu C++ (Circular Kadane):**
```cpp
long long circularKadane(const vector<int>& A) {
    long long max_toan_cuc = -1e18, max_cuc_bo = 0;
    long long min_toan_cuc = 1e18, min_cuc_bo = 0;
    long long tong_mang = 0;

    for (int x : A) {
        tong_mang += x;

        // Kadane tìm Max
        max_cuc_bo = max((long long)x, max_cuc_bo + x);
        max_toan_cuc = max(max_toan_cuc, max_cuc_bo);

        // Kadane tìm Min
        min_cuc_bo = min((long long)x, min_cuc_bo + x);
        min_toan_cuc = min(min_toan_cuc, min_cuc_bo);
    }

    // Trường hợp ngoại lệ cực kỳ quan trọng:
    // Nếu mảng toàn số âm, min_toan_cuc sẽ bằng chính tong_mang.
    // Khi đó tong_mang - min_toan_cuc = 0 (dãy rỗng). Ta phải trả về max_toan_cuc.
    if (max_toan_cuc < 0) {
        return max_toan_cuc;
    }

    // Trả về giá trị lớn hơn giữa TH1 (Không vòng) và TH2 (Có vòng)
    return max(max_toan_cuc, tong_mang - min_toan_cuc);
}
```

### 2. Ma trận con có tổng lớn nhất (2D Kadane / Maximum Submatrix)
- **Phân tích thuật toán:** Cho một ma trận kích thước $R \times C$. Nếu vét cạn mọi hình chữ nhật, ta mất $O(R^2 \times C^2)$. Để tối ưu xuống $O(R^2 \times C)$, ta kết hợp Hai con trỏ (chốt hàng) và 1D Kadane:
    1. Dùng 2 vòng lặp lồng nhau để cố định hàng trên cùng (Top) và hàng dưới cùng (Bottom) của hình chữ nhật.
    2. Với mỗi cặp (Top, Bottom), ta nén tất cả các số trong mỗi cột thành một mảng 1 chiều $temp[C]$ (bằng cách cộng dồn các hàng từ Top đến Bottom cho từng cột).
    3. Chạy thuật toán Kadane 1 chiều trên mảng $temp$ này để tìm ra cột giới hạn Trái (Left) và Phải (Right) mang lại tổng lớn nhất.
- **Ví dụ minh họa:** Giả sử cố định Top = 0 và Bottom = 1. Ta nén ma trận thành mảng $temp$:
    - Ma trận gốc:
        - Cột 0:  1, Cột 1:  2, Cột 2: -1
        - Cột 0: -8, Cột 1:  3, Cột 2:  4
        - $\rightarrow$ Mảng $temp$ (Cộng dồn theo cột): {-7, 5, 3}.
    - Chạy Kadane 1D trên $temp$, ta được dãy con lớn nhất là {5, 3} (ứng với cột 1 và cột 2). Tổng hình chữ nhật này là $8$.
- **Code mẫu C++ (2D Kadane):**
```cpp
// Hàm Kadane 1 chiều chuẩn
long long kadane1D(const vector<long long>& temp) {
    long long max_toan_cuc = -1e18, max_cuc_bo = 0;
    for (long long x : temp) {
        max_cuc_bo = max(x, max_cuc_bo + x);
        max_toan_cuc = max(max_toan_cuc, max_cuc_bo);
    }
    return max_toan_cuc;
}

long long kadane2D(const vector<vector<int>>& M) {
    int SoHang = M.size();
    if (SoHang == 0) return 0;
    int SoCot = M[0].size();

    long long max_hcn = -1e18;

    // Cố định hàng Top
    for (int top = 0; top < SoHang; top++) {
        // Mảng temp nén các cột từ hàng Top đến hàng Bottom
        vector<long long> temp(SoCot, 0); 

        // Trượt hàng Bottom từ Top xuống cuối
        for (int bottom = top; bottom < SoHang; bottom++) {
            // Cộng dồn hàng mới vào mảng temp
            for (int cot = 0; cot < SoCot; cot++) {
                temp[cot] += M[bottom][cot];
            }

            // Tìm Max của mảng temp nén bằng 1D Kadane
            long long tong_hien_tai = kadane1D(temp);
            max_hcn = max(max_hcn, tong_hien_tai);
        }
    }
    return max_hcn;
}
```
<div class="important-note">
💡 <b>CP Tip cực quan trọng:</b>

- Thuật toán 2D Kadane chạy trong thời gian $O(R^2 \times C)$. 
- Nếu đề bài cho ma trận $M$ có số Hàng rất lớn nhưng số Cột lại rất nhỏ ($R = 10000, C = 100$), ta cần <b>đảo ngược lại thuật toán</b>: 
    - Cố định 2 Cột (Left, Right) và nén các Hàng. 
    - Khi đó độ phức tạp là $O(C^2 \times R)$, chạy qua test trong chớp mắt thay vì bị TLE!
</div>
</div>

<div class="step-card border-red"><div class="step-badge bg-red">6. Lưu ý sống còn trong lập trình thi đấu</div><div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Vấn đề / Lỗi thường gặp</th><th style="width: 75%;">Phân tích & Kinh nghiệm khắc phục</th></tr></thead><tbody><tr><td><b>Trường hợp Mảng toàn số âm</b></td><td>Nếu mảng chỉ toàn số âm (vd: <code>{-3, -5, -2}</code>), nhiều học sinh viết Kadane khởi tạo <code>tong_cuc_bo = 0</code> sẽ ra kết quả là $0$ (dãy con rỗng). Đề thi thường yêu cầu dãy con <b>phải có ít nhất 1 phần tử</b>.<b>Khắc phục:</b> Khởi tạo <code>tong_toan_cuc</code> bằng một số cực âm (như code mẫu), hoặc gán nó bằng <code>A[0]</code> trước khi chạy vòng lặp. Nếu duyệt xong thấy kết quả vẫn âm thì đó chính là số âm lớn nhất mảng.</td></tr><tr><td><b>Nhầm lẫn với Sliding Window</b></td><td>Nhiều em lầm tưởng đây là Sliding Window (cửa sổ trượt). Hãy nhấn mạnh: Kadane <b>không có con trỏ <code>left</code> để thu hẹp dần</b>. Nó vứt bỏ toàn bộ cửa sổ cũ (đặt tổng về 0) ngay khi tổng mang giá trị âm. Đây là quy hoạch động trạng thái tối ưu, không phải 2 con trỏ!</td></tr><tr><td><b>Lỗi tràn bộ nhớ (Overflow)</b></td><td>Biến lưu tổng (<code>tong_cuc_bo</code> và <code>tong_toan_cuc</code>) bắt buộc phải khai báo là <code>long long</code>, bởi vì tổng của mảng $10^5$ số nguyên có thể vượt quá giới hạn của <code>int</code> ($2 \times 10^9$).</td></tr></tbody></table></div></div>