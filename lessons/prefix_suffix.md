### 📊 Kỹ Thuật Mảng Tiền Tố - Hậu Tố (Prefix & Suffix Arrays - Min/Max)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

**Khái niệm:**
- **Tiền tố (Prefix):** Là đoạn phần tử tính từ vị trí đầu tiên của mảng kéo dài đến vị trí $i$.
- **Hậu tố (Suffix):** Là đoạn phần tử tính từ vị trí cuối cùng của mảng kéo ngược về vị trí $i$.
- **Mảng Tiền tố / Hậu tố:** Là kỹ thuật tính toán trước (Precomputation). Ta sử dụng các mảng phụ để lưu trữ kết quả (Max hoặc Min) của các đoạn tiền tố và hậu tố. Nhờ vậy, thay vì phải dùng vòng lặp chạy lại từ đầu để tìm Max/Min mất thời gian $O(N)$, ta chỉ cần gọi ra kết quả từ mảng phụ trong thời gian $O(1)$.
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Phân tích thuật toán & Công thức truy hồi</div>

Giả sử ta có mảng gốc $A$ gồm $N$ phần tử (chỉ số từ $1$ đến $N$).

Tư tưởng cốt lõi của kỹ thuật này dựa trên **quy hoạch động cơ bản**: Kết quả của đoạn chiều dài $i$ được kế thừa từ kết quả của đoạn chiều dài $i-1$.
- **Mảng Tiền tố Max (preMax):** preMax[i] lưu giá trị lớn nhất từ $A[1]$ đến $A[i]$.
    - Công thức: `preMax[i] = max(preMax[i-1], A[i])`
- **Mảng Hậu tố Max (sufMax):** sufMax[i] lưu giá trị lớn nhất từ $A[i]$ đến $A[N]$.
    - Công thức: `sufMax[i] = max(sufMax[i+1], A[i])`

***Tương tự cho phép toán Min***
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Các dạng Mảng Tiền tố / Hậu tố (Max, Min)</div>

Hãy xem cách các mảng này được tạo ra từ một mảng gốc cụ thể:Mảng gốc $A$: {3, 1, 4, 2, 5}

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 15%;">Chỉ số $i$</th><th>1</th><th>2</th><th>3</th><th>4</th><th>5</th></tr></thead><tbody><tr style="font-weight: bold; background-color: #f8fafc;"><td style="color: #1e3a8a;">Mảng A</td><td>3</td><td>1</td><td>4</td><td>2</td><td>5</td></tr><tr><td><b>preMax</b></td><td>3</td><td>3 <i>(max của 3, 1)</i></td><td>4 <i>(max của 3, 4)</i></td><td>4 <i>(max của 4, 2)</i></td><td>5 <i>(max của 4, 5)</i></td></tr><tr><td><b>preMin</b></td><td>3</td><td>1</td><td>1</td><td>1</td><td>1</td></tr><tr><td><b>sufMax</b></td><td>5 <i>(max của 3, 5)</i></td><td>5 <i>(max của 4, 5)</i></td><td>5 <i>(max của 4, 5)</i></td><td>5 <i>(max của 2, 5)</i></td><td>5</td></tr>
<tr><td><b>sufMin</b></td><td>1</td><td>1</td><td>2</td><td>2</i></td><td>5</td></tr>
</tbody></table></div>
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">4. Ví dụ minh họa: Bài toán Hứng Nước (Trapping Rain Water)</div>

**Bài toán:** Cho một mảng $A$ biểu diễn độ cao của các bức tường cạnh nhau. Hỏi cấu trúc này có thể giữ được bao nhiêu đơn vị nước sau khi trời mưa?(Đây là một dạng bài kinh điển rất hay ra trong thi HSG).

**Phân tích giải thuật:** Nước chỉ có thể đọng lại ở một cột $i$ nếu cột đó bị kẹp giữa hai cột cao hơn ở bên trái và bên phải. Cụ thể, mực nước tại cột $i$ được quyết định bởi:
1. Bức tường cao nhất nằm ở bên trái nó: Chính là preMax[i].
2. Bức tường cao nhất nằm ở bên phải nó: Chính là sufMax[i].
3. Lượng nước đọng tại $i$ = min(preMax[i], sufMax[i]) - A[i].

Nếu dùng vòng lặp tìm max trái/phải cho mỗi cột, độ phức tạp là $O(N^2)$. Nhưng dùng Mảng tiền tố/hậu tố, ta chỉ tốn $O(N)$!

**Code mẫu C++ (Trapping Rain Water):**
```cpp
C++#include <iostream>
long long tinhLuongNuoc(const vector<int>& A) {
    int N = A.size();
    if (N <= 2) return 0; // Ít hơn 3 cột thì không thể giữ nước

    vector<int> preMax(N);
    vector<int> sufMax(N);

    // 1. Xây dựng mảng Tiền tố Max (Chạy từ Trái sang Phải)
    preMax[0] = A[0];
    for (int i = 1; i < N; i++) {
        preMax[i] = max(preMax[i - 1], A[i]);
    }

    // 2. Xây dựng mảng Hậu tố Max (Chạy từ Phải sang Trái)
    sufMax[N - 1] = A[N - 1];
    for (int i = N - 2; i >= 0; i--) {
        sufMax[i] = max(sufMax[i + 1], A[i]);
    }

    // 3. Tính toán lượng nước đọng lại
    long long tong_nuoc = 0;
    for (int i = 0; i < N; i++) {
        int muc_nuoc_toi_da = min(preMax[i], sufMax[i]);
        if (muc_nuoc_toi_da > A[i]) {
            tong_nuoc += (muc_nuoc_toi_da - A[i]);
        }
    }

    return tong_nuoc;
}
```
</div>
<div class="step-card border-red">
<div class="step-badge bg-red">5. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div>
<div class="geometry-table-container"><table class="styled-table"><thead><tr><th style="width: 25%;">Vấn đề</th><th style="width: 75%;">Phân tích & Kinh nghiệm</th></tr></thead><tbody><tr><td><b>Chỉ số Mảng (0-based vs 1-based)</b></td><td>Trong C++, mảng bắt đầu từ $0$. Nếu học sinh muốn dùng chỉ số từ $1$ cho dễ tư duy công thức, hãy nhớ cấp phát kích thước <code>vector&lt;int&gt; preMax(N + 2, 0);</code> để có chỗ trống ở đầu và cuối, tránh lỗi tràn biên (Out of Bounds).</td></tr><tr><td><b>Giới hạn bộ nhớ (Memory Limit)</b></td><td>Kỹ thuật này đánh đổi <b>Không gian (Bộ nhớ) lấy Thời gian</b>. Ta tốn thêm 2 mảng kích thước $N$. Nếu $N = 10^7$, việc khai báo thêm nhiều <code>vector</code> có thể bị Memory Limit Exceeded (MLE). Khi đó, ta có thể tối ưu bộ nhớ bằng Kỹ thuật <b>Hai con trỏ</b> (với bài Hứng nước).</td></tr><tr><td><b>Giá trị Khởi tạo mảng Min</b></td><td>Khi xây dựng mảng <code>preMin</code> hoặc <code>sufMin</code>, học sinh thường quên và khởi tạo giá trị ban đầu là $0$. Nếu mảng chứa toàn số dương, kết quả Min sẽ luôn ra $0$ (sai).<b>Khắc phục:</b> Luôn gán <code>preMin[0] = A[0]</code>, tuyệt đối không gán bằng $0$.</td></tr><tr><td><b>Dấu hiệu nhận biết bài toán</b></td><td>Khi đề bài yêu cầu tìm một cặp chỉ số $(i, j)$ thỏa mãn điều kiện nào đó, và yêu cầu tính giá trị tốt nhất ở <b>bên trái</b> $i$, <b>bên phải</b> $j$, hoặc ở <b>giữa</b> chúng. Ví dụ: <i>"Mua chứng khoán 1 lần"</i> (Dùng mảng <code>sufMax</code> để biết giá cao nhất trong tương lai).</td></tr></tbody></table>
</div>