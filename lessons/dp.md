## 🌀 Quy Hoạch Động (Dynamic Programming)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm và Tư tưởng chính</div>

**1. Tư tưởng chính**

Quy hoạch động là phương pháp giải quyết các bài toán phức tạp bằng cách **chia nhỏ** chúng thành các bài toán con nhỏ hơn, giải các bài toán con này một lần và lưu trữ kết quả của chúng để tái sử dụng khi cần thiết *(gọi là kỹ thuật ghi nhớ - Memoization hoặc lập bảng - Tabulation)*.

**2. Cấu trúc bài toán DP**
- **Dữ liệu đầu vào:** Thường là một tập hợp các phần tử (mảng, chuỗi) hoặc một giá trị đích cần đạt được.
- **Yêu cầu:** Tìm giá trị tối ưu (lớn nhất/nhỏ nhất) hoặc đếm số cách thực hiện.
- **Kết quả:** Một giá trị duy nhất (đáp án bài toán lớn).

**3. Bài toán con (Sub-problems)**
- Là phiên bản nhỏ hơn của bài toán gốc. Kết quả của bài toán lớn được xây dựng từ kết quả của các bài toán con trước đó.
- Ví dụ minh họa: Tính số Fibonacci thứ $n$. 
    - Để tính $F(5)$, ta cần $F(4)$ và $F(3)$. 
    - Thay vì tính lại nhiều lần, ta lưu $F(1), F(2)...$ vào mảng.

**4. Hàm mục tiêu và Tham số**
- **Hàm mục tiêu:** Là giá trị ta đang tìm kiếm (ví dụ: $dp[i]$ là số cách tối ưu để đạt đến trạng thái $i$).
- **Tham số:** Các biến xác định trạng thái của bài toán (vị trí phần tử, khối lượng còn lại, số chữ số...).
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các bước giải quyết bài toán Quy hoạch động</div>

**Để giải quyết một bài toán DP, chúng ta thực hiện theo quy trình 3 bước chuẩn sau:**

<div class="step-title-box bg-red"> Bước 1: Xác định trạng thái và Bài toán cơ sở </div>

- **Trạng thái:** Xác định ý nghĩa của mảng $dp[]$. 
    - Ví dụ: $dp[i]$ là số tiền ít nhất để đổi được số tiền bằng $i$.
- **Bài toán cơ sở (Base case):** Những trường hợp nhỏ nhất có thể biết ngay đáp án. 
    - Ví dụ: $dp[0] = 0$.

<div class="step-title-box bg-yellow"> Bước 2: Xây dựng công thức truy hồi (Transition) </div>

Đây là linh hồn của bài toán. Tìm mối liên hệ giữa bài toán hiện tại với các bài toán con đã giải trước đó.

**Công thức:** 

<div class="math-formula">$dp[i] = \text{Tối ưu}(dp[i-1], dp[i-k], \dots) + \text{chi phí}$ </div>

<div class="step-title-box bg-green"> Bước 3: Tính toán và Truy vết </div>

- **Tính toán:** Sử dụng vòng lặp để điền kết quả vào bảng (mảng 1 chiều hoặc 2 chiều).
- **Truy vết (Trace):** Nếu bài toán yêu cầu đưa ra cách chọn phần tử (không chỉ là giá trị), ta dùng một mảng phụ để lưu lại "vết" đã chọn ở mỗi bước.
</div>

<div class="step-card border-purple">
<div class="step-badge bg-purple">3. Ví dụ minh họa</div>

**Bài toán Leo thang:** Có $N$ bậc thang. Mỗi bước có thể bước $1$ hoặc $2$ bậc. Hỏi có bao nhiêu cách để lên đỉnh $N$?

- **Bước 1:**
    - **Trạng thái:** $dp[i]$ là số cách lên bậc $i$.
    - **Bài toán Cơ sở:** $dp[1] = 1, dp[2] = 2$.
- **Bước 2: công thức truy hồi:** 
    - Trường hợp 1: Bước từ bậc $i - 1$ lên bậc $i$. Số cách chính là tích 2 giai đoạn
        - Giai đoạn 1: Bước từ bậc $1$ đến bậc $i - 1$: số cách là $dp[i - 1]$
        - Giai đoạn 2: Bước từ bậc $i - 1$ lên bậc $i$: số cacsu là $1$
        - Vậy số cách của trường hợp 1 là $dp[i - 1] * 1 = dp[i - 1]$ 
    - Trường hợp 1: Bước từ bậc $i - 2$ lên bậc $i$. Số cách chính là tích 2 giai đoạn
        - Giai đoạn 1: Bước từ bậc $1$ đến bậc $i - 2$: số cách là $dp[i - 2]$
        - Giai đoạn 2: Bước từ bậc $i - 2$ lên bậc $i$: số cacsu là $1$
        - Vậy số cách của trường hợp 1 là $dp[i - 2] * 1 = dp[i - 2]$ 
    - Số cách để đi từ bậc thang $1$ đến bậc thang $i$ là tổng 2 trường hợp: $dp[i] = dp[i - 1] + dp[i - 2]$.
- **Bước 3: Tính toán và truy vết**
    - Ví dụ với $N = 9$ ta có bảng dp như bên dưới 

<div class="geometry-table-container">
    <table class="styled-table">
        <thead>
            <tr>
                <th style="width: 15%;">index (i)</th>
                <th style="width: 8.5%;">0</th>
                <th style="width: 8.5%;">1</th>
                <th style="width: 8.5%;">2</th>
                <th style="width: 8.5%;">3</th>
                <th style="width: 8.5%;">4</th>
                <th style="width: 8.5%;">5</th>
                <th style="width: 8.5%;">6</th>
                <th style="width: 8.5%;">7</th>
                <th style="width: 8.5%;">8</th>
                <th style="width: 8.5%;">9</th>
            </tr>
        </thead>
        <tbody>
            <tr style="text-align: center; font-weight: bold;">
                <td style="background-color: #f8fafc; color: #1e3a8a;">dp[i]</td>
                <td>0</td>
                <td>1</td>
                <td>2</td>
                <td>3</td>
                <td>5</td>
                <td style="background-color: #dcfce7;">8</td>
                <td style="background-color: #bbf7d0;">13</td>
                <td style="background-color: #22c55e; color: white;">21</td>
                <td>34</td>
                <td>55</td>
            </tr>
        </tbody>
    </table>
</div>

<div class="formula-notes" style="margin-top: 10px;">
    <p><b>Giải thích ví dụ (Leo thang/Fibonacci):</b></p>
    <ul>
        <li>Các ô màu xanh minh họa cho việc tính toán trạng thái hiện tại dựa trên các trạng thái trước đó.</li>
        <li>Giá trị tại <b>index 7</b> (đáp án là 21) được tính bằng tổng của hai bài toán con trước đó: $dp[7] = dp[6] + dp[5] = 13 + 8$.</li>
    </ul>
</div> 
</div>

<div class="step-card border-green">
<div class="step-badge bg-green">3. Ứng dụng tư tưởng Quy hoạch động (THCS)</div>

**Hai lớp bài toán chính**

- **📊 Bài toán Đếm:** Đếm số lượng cách thỏa mãn điều kiện. (Ví dụ: Đếm số cách đổi tiền, đếm số đường đi trong mê cung).

- **🏆 Bài toán Tối ưu:** Tìm giá trị lớn nhất (Max) hoặc nhỏ nhất (Min). (Ví dụ: Bài toán Cái túi, tìm dãy con tăng dài nhất).

**Các dạng Quy hoạch động thường gặp**

<div class="geometry-table-container"><table class="styled-table"><thead><tr><th>Dạng DP</th><th>Đặc điểm</th><th>Ví dụ tiêu biểu</th></tr></thead><tbody><tr><td><b>Mảng 1 chiều</b></td><td>Trạng thái chỉ phụ thuộc vào 1 tham số duy nhất (vị trí hoặc giá trị).</td><td>Dãy con có tổng bằng $S$, bậc thang, đổi tiền.</td></tr><tr><td><b>Mảng 2 chiều</b></td><td>Trạng thái phụ thuộc vào 2 tham số (thường là 2 đối tượng hoặc tọa độ $i, j$).</td><td>Xâu con chung dài nhất (LCS), Cái túi (Knapsack), Đường đi trên lưới.</td></tr><tr><td><b>DP Chữ số (Digit DP)</b></td><td>Dùng để đếm các số trong đoạn $[L, R]$ thỏa mãn tính chất chữ số đặc biệt.</td><td>Đếm số có tổng các chữ số là số nguyên tố, đếm số không có chữ số 4.</td></tr></tbody></table></div></div>

<div class="important-note">
💡 <b>Lưu ý quan trọng khi thi đấu:</b>

- **Khởi tạo:** Luôn khởi tạo mảng DP với giá trị vô cùng lớn (nếu tìm Min) hoặc vô cùng nhỏ (nếu tìm Max) trước khi chạy vòng lặp.
- **Kích thước mảng:** Chú ý giới hạn bộ nhớ (Memory Limit). Mảng 2 chiều $10^4 \times 10^4$ thường sẽ gây lỗi quá bộ nhớ.
- **Kiểm soát dấu:** Đối với bài toán đếm số cách lớn, hãy luôn nhớ thực hiện phép chia lấy dư (Modulo) ở mỗi bước cộng dồn.
</div>