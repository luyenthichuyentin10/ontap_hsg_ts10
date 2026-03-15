<h1 style="text-align: center; color: #1e3a8a; margin-bottom: 30px;">🎯 🥇 🏆 CẨM NANG CHIẾN THUẬT LẬP TRÌNH THI ĐẤU (HSG & CHUYÊN 10) 🏆 🥇 🎯</h1>

## PHẦN 1: KỸ NĂNG VÀ CHIẾN THUẬT PHÒNG THI

<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Trước khi phát đề (Giai đoạn khởi động)</div>

*Giai đoạn này giúp làm nóng người và đảm bảo vũ khí (máy tính) ở trạng thái tốt nhất.*

* ⌨️ **Kiểm tra thiết bị:** Kiểm tra máy tính, bàn phím, chuột. Nếu có dấu hiệu khó sử dụng, yêu cầu đổi máy ngay lập tức.
* ⚙️ **Chuẩn bị môi trường:** * Tạo ngay dự án mẫu (Project) trên Code::Blocks (hoặc IDE quy định).
  * Viết khung chương trình cơ bản, thiết lập đọc/ghi file `INP`, `OUT` và biên dịch chạy thử.
  * Test kỹ chức năng **Debug** của phần mềm.
* 💾 **Lưu trữ an toàn:** Lưu file đúng vào ổ đĩa và thư mục mà giám thị quy định. Thử khởi động lại máy (Reset) rồi mở lại xem project có bị mất không (đề phòng máy bị đóng băng ổ cứng Deep Freeze).
* ⚔️ **"Cày" trước vũ khí:** Tranh thủ code trước một số hàm kinh điển luôn dùng tới như: *Sàng nguyên tố, tìm UCLN (GCD), kiểm tra số nguyên tố, đổi hệ cơ số...*
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Thời gian làm bài (Giai đoạn thực chiến)</div>

* ⏱️ **Đọc đề và Phân loại (5 - 10 phút đầu):** Đọc toàn bộ đề thi để xác định độ khó. Phân loại ngay: Bài Dễ, Bài Khó, Bài Quen thuộc, Bài Đúng dạng. **Bắt tay vào làm từ bài Dễ nhất.**
* 🔍 **Phân tích đề:** Đọc kỹ nhiều lần, dùng bút dạ quang/bút đỏ đánh dấu các từ khóa, giới hạn (Ràng buộc dữ liệu). Kết hợp test ví dụ của đề để đảm bảo mình hiểu đúng yêu cầu.

    *(Mẹo: Nếu đọc đề thấy bất hợp lý hoặc nghi ngờ đề sai sót, hãy mạnh dạn hỏi giám thị. Trong lúc chờ trả lời, chuyển sang làm bài khác).*
* ⛔ **Không được bỏ bài:** Tất cả các bài trong đề đều được chia subtask phù hợp để phân loại học sinh. Subtask 1 cho giải ba, subtaks 2 cho giải nhì, giải toàn bộ sẽ được giải nhất (💵 10.000.000 đồng)
* 🧩 **Chiến thuật "Chia để trị" (Subtask):** 
    * **Vét cạn (Brute-force) là cứu cánh:** Nếu không nghĩ ra thuật toán chuẩn, hãy dùng Vét cạn để ăn điểm các Subtask nhỏ. Lấy được điểm nào hay điểm đó.
    * **Code vét cạn (Brute-force) không phải là dở:** mà code vét cạn để dùng làm cơ sở kiểm tra kết quả code tổng quát, trong quá trình code vét cạn sẽ giúp ta nắm rỏ và hiểu sâu bài toán đang giải hơn.
    * **Bảo hiểm code (If - Else):** Nếu đã nghĩ ra thuật toán nhưng chưa chắc chắn, hãy gộp cả Vét cạn và Thuật toán tối ưu vào cùng 1 file bằng lệnh `if...else`.

```cpp
if (N <= 1000) {
    VetCan(); // Chắc chắn ăn điểm test nhỏ
} else {
    Solve();  // Hên xui ăn điểm test lớn
}
```

* 🧪 **Quy trình Test (Sinh dữ liệu):** Phải tự sinh test để kiểm tra chương trình theo trình tự: `Test ví dụ (đề cho) -> Test bình thường tự tính tay -> Test đặc biệt (N=1, mảng toàn số 0...) -> Test ở biên giới hạn (N Max, số lớn nhất)`.
* 🚨 **Thao tác sống còn:** LƯU BÀI THƯỜNG XUYÊN (`Ctrl + S`) để tránh treo máy mất bài.
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">3. Kỹ thuật viết Code (Tránh sai số và Bug)</div>

* 🧠 **Tư duy dữ liệu:** Một số bài toán sẽ trở nên rất dễ nếu ta thao tác **sắp xếp lại (Sort)** dữ liệu ban đầu.
* 💥 **Tràn số (Overflow):** Cực kỳ cẩn thận với phép nhân các số lớn. Luôn dùng `long long` khi cần thiết.
* 📐 **Kẻ thù "Số thực":** * Tuyệt đối HẠN CHẾ so sánh hai số thực bằng toán tử `==` (do sai số thập phân).
    * 🎯 **Cách so sánh đúng (Dùng Epsilon):** Nếu BẮT BUỘC phải kiểm tra hai số thực `a` và `b` có bằng nhau hay không, ta phải xét xem khoảng cách (trị tuyệt đối) giữa chúng có nhỏ hơn một sai số vô cùng nhỏ `epsilon` (thường là $10^{-9}$) hay không. 
    * *Code chuẩn:* `if (abs(a - b) <= 1e-9)`
    * Hạn chế dùng phép chia `/` và căn bậc hai `sqrt`. Hãy dùng toán học để biến đổi:
        * Thay vì `(a + b) / 2 == c`, hãy dùng `a + b == 2 * c`.
        * Thay vì `sqrt(a*a + b*b) == c`, hãy dùng `a*a + b*b == c*c`.
* 📦 **Khai báo mảng:** Kích thước mảng tĩnh luôn phải **cộng thêm 5** so với giới hạn của đề bài (Ví dụ: Đề cho $N \le 10^5$, hãy khai báo `int a[100005];`).
* 🔢 **Chỉ số mảng:** Nên sử dụng vòng lặp và mảng **bắt đầu từ index 1** để khớp hoàn toàn với mô tả của đề bài, tránh sai sót khi xuất kết quả.
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">4. Thời gian nộp bài (Giai đoạn chốt sổ)</div>

<div class="important-note">
    <ul>
        <li>🛑 <b>Quy tắc 10 phút:</b> Dừng mọi việc code thêm trước 10 - 15 phút khi hết giờ. Không tham lam sửa code vào phút chót.</li>
        <li>🔎 <b>Kiểm tra sinh tử:</b> Soi lại tên file chương trình <code>.cpp</code>, tên file <code>.INP</code>, <code>.OUT</code> từng chữ một. Biên dịch lại lần cuối chắc chắn không có lỗi Cú pháp (Compile Error).</li>
        <li>🔒 <b>Copy file an toàn:</b> Phải <b>TẮT</b> phần mềm Code::Blocks (hoặc IDE) trước khi copy file sang thư mục nộp bài (để file không bị khóa lock).</li>
        <li>⚖️ <b>Xác nhận file nộp:</b> Khi giám thị thu bài, hãy so sánh <b>số byte (dung lượng)</b> của file trên máy và file đã nộp xem có khớp nhau không. Mở lại file đã nộp để kiểm tra (nếu phát hiện sai tên file thì có thể năn nỉ giám thị xin sửa).</li>
        <li>✍️ <b>Ký tên</b> Chỉ kí tên xác nhận nộp bài khi 100% các bài đã kiểm tra chính xác. Hãy luôn nhớ câu <b><i>"bút sa gà chết"</i></b></li>
    </ul>
</div>
</div>

<hr style="margin: 40px 0; border: 1px solid #e2e8f0;">

## PHẦN 2: TRẮC NGHIỆM CŨNG CỐ CHIẾN THUẬT THI ĐẤU

<style>
    .quiz-card {
        background-color: #f8fafc; border: 1px solid #cbd5e1;
        border-radius: 8px; padding: 20px; margin-bottom: 20px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.05); line-height: 1.6;
    }
    .quiz-card input[type="radio"] { display: none; }
    .quiz-q { font-weight: bold; color: #1e293b; margin-bottom: 15px; font-size: 1.1em; }
    .q-label {
        display: block; padding: 12px 15px; margin-bottom: 8px;
        background-color: white; border: 1px solid #cbd5e1;
        border-radius: 6px; cursor: pointer; transition: all 0.2s;
    }
    .q-label:hover { background-color: #f1f5f9; border-color: #94a3b8; }

    /* Khi click vào 1 đáp án -> Khóa không cho click lại */
    .quiz-card input:checked ~ .q-label { pointer-events: none; }

    /* Luôn luôn hiện đáp án ĐÚNG màu Xanh Lá khi có thao tác click */
    .quiz-card input:checked ~ .correct-label {
        background-color: #dcfce7 !important;
        border-color: #22c55e !important;
        color: #166534 !important;
        font-weight: bold;
    }
    .quiz-card input:checked ~ .correct-label::after { content: " (✅ Chính xác)"; }

    /* Nếu click trúng đáp án SAI -> Hiện màu Đỏ */
    .quiz-card input.is-wrong:nth-of-type(1):checked ~ .q-label:nth-of-type(1),
    .quiz-card input.is-wrong:nth-of-type(2):checked ~ .q-label:nth-of-type(2),
    .quiz-card input.is-wrong:nth-of-type(3):checked ~ .q-label:nth-of-type(3),
    .quiz-card input.is-wrong:nth-of-type(4):checked ~ .q-label:nth-of-type(4) {
        background-color: #fee2e2 !important;
        border-color: #ef4444 !important;
        color: #991b1b !important;
    }
    .quiz-card input.is-wrong:nth-of-type(1):checked ~ .q-label:nth-of-type(1)::after,
    .quiz-card input.is-wrong:nth-of-type(2):checked ~ .q-label:nth-of-type(2)::after,
    .quiz-card input.is-wrong:nth-of-type(3):checked ~ .q-label:nth-of-type(3)::after,
    .quiz-card input.is-wrong:nth-of-type(4):checked ~ .q-label:nth-of-type(4)::after {
        content: " (❌ Sai)";
    }
</style>

<div class="quiz-card">
    <input type="radio" name="q1" id="q1-1" class="is-wrong">
    <input type="radio" name="q1" id="q1-2" class="is-correct">
    <input type="radio" name="q1" id="q1-3" class="is-wrong">
    <input type="radio" name="q1" id="q1-4" class="is-wrong">
    <div class="quiz-q">Câu 1: Trong 15 phút trước khi phát đề, thí sinh KHÔNG NÊN làm việc gì sau đây?</div>
    <label for="q1-1" class="q-label">A. Kiểm tra chuột, bàn phím và máy tính.</label>
    <label for="q1-2" class="q-label correct-label">B. Ngồi im chờ đợi giám thị phát đề để tránh vi phạm quy chế.</label>
    <label for="q1-3" class="q-label">C. Mở IDE, viết bộ khung đọc/ghi file và test chức năng biên dịch.</label>
    <label for="q1-4" class="q-label">D. Code trước một số hàm cơ bản như sàng nguyên tố, tìm ước chung lớn nhất.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q2" id="q2-1" class="is-wrong">
    <input type="radio" name="q2" id="q2-2" class="is-correct">
    <input type="radio" name="q2" id="q2-3" class="is-wrong">
    <input type="radio" name="q2" id="q2-4" class="is-wrong">
    <div class="quiz-q">Câu 2: Việc thiết lập "dự án mẫu" (project) kết nối sẵn đọc/ghi file in, out trước giờ thi mang lại lợi ích gì?</div>
    <label for="q2-1" class="q-label">A. Giúp máy tính chạy nhanh hơn bình thường.</label>
    <label for="q2-2" class="q-label correct-label">B. Tiết kiệm thời gian gõ lại các dòng code thủ tục và đảm bảo hệ thống chấm đọc được file bài làm.</label>
    <label for="q2-3" class="q-label">C. Tránh bị giám thị trừ điểm kỹ năng.</label>
    <label for="q2-4" class="q-label">D. Giúp code không bao giờ bị lỗi cú pháp.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q3" id="q3-1" class="is-wrong">
    <input type="radio" name="q3" id="q3-2" class="is-correct">
    <input type="radio" name="q3" id="q3-3" class="is-wrong">
    <input type="radio" name="q3" id="q3-4" class="is-wrong">
    <div class="quiz-q">Câu 3: Khi mới nhận đề thi, thí sinh nên dành bao nhiêu thời gian để đọc toàn bộ đề?</div>
    <label for="q3-1" class="q-label">A. Vừa đọc câu 1 là cắm đầu vào code ngay lập tức.</label>
    <label for="q3-2" class="q-label correct-label">B. 5 đến 10 phút.</label>
    <label for="q3-3" class="q-label">C. 30 phút.</label>
    <label for="q3-4" class="q-label">D. Gần hết giờ mới đọc các bài khó.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q4" id="q4-1" class="is-wrong">
    <input type="radio" name="q4" id="q4-2" class="is-wrong">
    <input type="radio" name="q4" id="q4-3" class="is-correct">
    <input type="radio" name="q4" id="q4-4" class="is-wrong">
    <div class="quiz-q">Câu 4: Nếu đang làm bài thi mà thấy một câu trong đề có vẻ bị sai hoặc không rõ ràng, cách xử lý tốt nhất là gì?</div>
    <label for="q4-1" class="q-label">A. Bỏ qua bài đó không làm nữa vì đề sai.</label>
    <label for="q4-2" class="q-label">B. Tự ý sửa đề theo ý mình và làm bình thường.</label>
    <label for="q4-3" class="q-label correct-label">C. Báo ngay và hỏi giám thị coi thi, trong lúc chờ đợi thì làm bài khác.</label>
    <label for="q4-4" class="q-label">D. Hỏi các bạn thí sinh xung quanh xem đề có sai không.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q5" id="q5-1" class="is-wrong">
    <input type="radio" name="q5" id="q5-2" class="is-wrong">
    <input type="radio" name="q5" id="q5-3" class="is-correct">
    <input type="radio" name="q5" id="q5-4" class="is-wrong">
    <div class="quiz-q">Câu 5: Khi đi thi, nếu đã hết thời gian phân bổ cho một bài mà bạn vẫn chưa nghĩ ra thuật toán tối ưu, bạn nên làm gì?</div>
    <label for="q5-1" class="q-label">A. Bỏ trống bài đó để dành thời gian nghĩ bài khác.</label>
    <label for="q5-2" class="q-label">B. Cố gắng ngồi suy nghĩ thuật toán tối ưu cho đến khi nộp bài.</label>
    <label for="q5-3" class="q-label correct-label">C. Viết code Vét cạn (Brute-force) để lấy điểm từng phần (Subtask) rồi mới chuyển bài.</label>
    <label for="q5-4" class="q-label">D. Nộp bài sớm đi về.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q6" id="q6-1" class="is-wrong">
    <input type="radio" name="q6" id="q6-2" class="is-wrong">
    <input type="radio" name="q6" id="q6-3" class="is-correct">
    <input type="radio" name="q6" id="q6-4" class="is-wrong">
    <div class="quiz-q">Câu 6: Giả sử bạn đã code xong thuật toán tối ưu nhưng chưa thực sự tự tin nó đúng 100%. Cách xử lý "bảo hiểm" điểm số tốt nhất là gì?</div>
    <label for="q6-1" class="q-label">A. Xóa code tối ưu, chỉ nộp code Vét cạn cho an toàn.</label>
    <label for="q6-2" class="q-label">B. Vẫn nộp code tối ưu, chấp nhận rủi ro sai ở các test nhỏ.</label>
    <label for="q6-3" class="q-label correct-label">C. Code cả 2 hàm: Vét cạn cho Subtask nhỏ (dùng lệnh if) và thuật tối ưu cho Subtask lớn (dùng else).</label>
    <label for="q6-4" class="q-label">D. Xin thêm giấy nháp để chứng minh lại thuật toán.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q7" id="q7-1" class="is-wrong">
    <input type="radio" name="q7" id="q7-2" class="is-wrong">
    <input type="radio" name="q7" id="q7-3" class="is-correct">
    <input type="radio" name="q7" id="q7-4" class="is-wrong">
    <div class="quiz-q">Câu 7: Trình tự tự sinh test (thử nghiệm) code nào sau đây là chuẩn mực nhất?</div>
    <label for="q7-1" class="q-label">A. Test đặc biệt -> Test bình thường -> Test ở biên -> Test ví dụ.</label>
    <label for="q7-2" class="q-label">B. Test ở biên -> Test ví dụ -> Test đặc biệt -> Test bình thường.</label>
    <label for="q7-3" class="q-label correct-label">C. Test ví dụ -> Test bình thường tự tính -> Test đặc biệt -> Test ở biên.</label>
    <label for="q7-4" class="q-label">D. Test bình thường -> Test ở biên -> Test ví dụ -> Test đặc biệt.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q8" id="q8-1" class="is-wrong">
    <input type="radio" name="q8" id="q8-2" class="is-correct">
    <input type="radio" name="q8" id="q8-3" class="is-wrong">
    <input type="radio" name="q8" id="q8-4" class="is-wrong">
    <div class="quiz-q">Câu 8: Trước khi thực hiện các phép toán phức tạp trên mảng dữ liệu đầu vào, ta nên cân nhắc hành động nào để dễ giải quyết hơn?</div>
    <label for="q8-1" class="q-label">A. Gán tất cả mảng bằng 0.</label>
    <label for="q8-2" class="q-label correct-label">B. Sắp xếp lại dữ liệu của mảng.</label>
    <label for="q8-3" class="q-label">C. Đổi kiểu dữ liệu sang số thực (double).</label>
    <label for="q8-4" class="q-label">D. Xóa bớt các phần tử ở cuối mảng.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q9" id="q9-1" class="is-wrong">
    <input type="radio" name="q9" id="q9-2" class="is-wrong">
    <input type="radio" name="q9" id="q9-3" class="is-correct">
    <input type="radio" name="q9" id="q9-4" class="is-wrong">
    <div class="quiz-q">Câu 9: Khi thực hiện phép NHÂN hai số nguyên trong lập trình thi đấu, rủi ro lớn nhất dễ làm học sinh mất điểm là gì?</div>
    <label for="q9-1" class="q-label">A. Phép nhân chạy quá chậm gây TLE (Time Limit Exceeded).</label>
    <label for="q9-2" class="q-label">B. Lỗi biên dịch (Compile Error).</label>
    <label for="q9-3" class="q-label correct-label">C. Tràn kiểu dữ liệu (Overflow).</label>
    <label for="q9-4" class="q-label">D. Phép nhân sinh ra số âm vô lý.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q10" id="q10-1" class="is-wrong">
    <input type="radio" name="q10" id="q10-2" class="is-correct">
    <input type="radio" name="q10" id="q10-3" class="is-wrong">
    <input type="radio" name="q10" id="q10-4" class="is-wrong">
    <div class="quiz-q">Câu 10: Khi làm bài thi, tại sao phải HẠN CHẾ dùng toán tử == để so sánh hai số thực (float, double)?</div>
    <label for="q10-1" class="q-label">A. Vì C++ không hỗ trợ toán tử == cho số thực.</label>
    <label for="q10-2" class="q-label correct-label">B. Vì máy tính lưu trữ số thực không chính xác tuyệt đối, dẫn đến sai số thập phân.</label>
    <label for="q10-3" class="q-label">C. Vì dùng == với số thực sẽ làm chương trình bị chậm đi.</label>
    <label for="q10-4" class="q-label">D. Vì dùng == sẽ bị tràn bộ nhớ.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q11" id="q11-1" class="is-wrong">
    <input type="radio" name="q11" id="q11-2" class="is-wrong">
    <input type="radio" name="q11" id="q11-3" class="is-correct">
    <input type="radio" name="q11" id="q11-4" class="is-wrong">
    <div class="quiz-q">Câu 11: Để kiểm tra điều kiện trung bình cộng (a + b) / 2 == c mà không dùng phép chia, ta nên viết lại thế nào?</div>
    <label for="q11-1" class="q-label">A. a + b == c / 2</label>
    <label for="q11-2" class="q-label">B. a == 2 * c - b</label>
    <label for="q11-3" class="q-label correct-label">C. a + b == c * 2</label>
    <label for="q11-4" class="q-label">D. (a + b) * 2 == c</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q12" id="q12-1" class="is-wrong">
    <input type="radio" name="q12" id="q12-2" class="is-wrong">
    <input type="radio" name="q12" id="q12-3" class="is-correct">
    <input type="radio" name="q12" id="q12-4" class="is-wrong">
    <div class="quiz-q">Câu 12: Để kiểm tra điều kiện tam giác vuông bằng công thức Pythagoras sqrt(a*a + b*b) == c mà không dùng hàm sqrt, ta viết lại thế nào?</div>
    <label for="q12-1" class="q-label">A. a + b == c * c</label>
    <label for="q12-2" class="q-label">B. a*a + b*b == c</label>
    <label for="q12-3" class="q-label correct-label">C. a*a + b*b == c*c</label>
    <label for="q12-4" class="q-label">D. (a + b)*(a + b) == c*c</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q13" id="q13-1" class="is-wrong">
    <input type="radio" name="q13" id="q13-2" class="is-wrong">
    <input type="radio" name="q13" id="q13-3" class="is-correct">
    <input type="radio" name="q13" id="q13-4" class="is-wrong">
    <div class="quiz-q">Câu 13: Nếu đề bài cho giới hạn mảng tối đa là N ≤ 10^5, cách khai báo mảng tĩnh nào sau đây là AN TOÀN nhất?</div>
    <label for="q13-1" class="q-label">A. int a[100000];</label>
    <label for="q13-2" class="q-label">B. int a[10000];</label>
    <label for="q13-3" class="q-label correct-label">C. int a[100005];</label>
    <label for="q13-4" class="q-label">D. Khai báo ngay trong hàm main bằng lệnh int a[n];</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q14" id="q14-1" class="is-wrong">
    <input type="radio" name="q14" id="q14-2" class="is-wrong">
    <input type="radio" name="q14" id="q14-3" class="is-correct">
    <input type="radio" name="q14" id="q14-4" class="is-wrong">
    <div class="quiz-q">Câu 14: Tại sao tài liệu lại khuyên thí sinh nên bắt đầu sử dụng mảng (index) từ vị trí số 1 thay vì vị trí số 0?</div>
    <label for="q14-1" class="q-label">A. Để tiết kiệm 1 byte bộ nhớ.</label>
    <label for="q14-2" class="q-label">B. Vì C++ bắt buộc mảng phải bắt đầu từ 1.</label>
    <label for="q14-3" class="q-label correct-label">C. Để dễ dàng xuất kết quả đúng logic của đề bài, tránh việc cộng/trừ 1 khi in đáp án.</label>
    <label for="q14-4" class="q-label">D. Để chương trình biên dịch nhanh hơn.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q15" id="q15-1" class="is-wrong">
    <input type="radio" name="q15" id="q15-2" class="is-correct">
    <input type="radio" name="q15" id="q15-3" class="is-wrong">
    <input type="radio" name="q15" id="q15-4" class="is-wrong">
    <div class="quiz-q">Câu 15: Thao tác nào phải thực hiện liên tục (thành thói quen) trong suốt quá trình code để tránh rủi ro mất trắng bài thi?</div>
    <label for="q15-1" class="q-label">A. Khởi động lại máy tính.</label>
    <label for="q15-2" class="q-label correct-label">B. Lưu bài thường xuyên (Bấm Ctrl + S).</label>
    <label for="q15-3" class="q-label">C. Nộp bài lên hệ thống.</label>
    <label for="q15-4" class="q-label">D. Xóa bộ nhớ đệm (Clear cache).</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q16" id="q16-1" class="is-wrong">
    <input type="radio" name="q16" id="q16-2" class="is-wrong">
    <input type="radio" name="q16" id="q16-3" class="is-correct">
    <input type="radio" name="q16" id="q16-4" class="is-wrong">
    <div class="quiz-q">Câu 16: Thời điểm TỐT NHẤT để thí sinh DỪNG MỌI HOẠT ĐỘNG CODE THÊM và bắt đầu kiểm tra chuẩn bị nộp bài là lúc nào?</div>
    <label for="q16-1" class="q-label">A. Khi giám thị hô 'Hết giờ'.</label>
    <label for="q16-2" class="q-label">B. Trước khi hết giờ 5 phút.</label>
    <label for="q16-3" class="q-label correct-label">C. Trước khi hết giờ 15 đến 20 phút.</label>
    <label for="q16-4" class="q-label">D. Khi đã code xong hết tất cả các câu trong đề.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q17" id="q17-1" class="is-wrong">
    <input type="radio" name="q17" id="q17-2" class="is-wrong">
    <input type="radio" name="q17" id="q17-3" class="is-correct">
    <input type="radio" name="q17" id="q17-4" class="is-wrong">
    <div class="quiz-q">Câu 17: Trước khi copy file .cpp bỏ vào thư mục nộp bài, thao tác BẮT BUỘC nào sau đây phải được thực hiện?</div>
    <label for="q17-1" class="q-label">A. Đổi tên file thành tiếng Việt có dấu.</label>
    <label for="q17-2" class="q-label">B. Chuyển code sang file Word.</label>
    <label for="q17-3" class="q-label correct-label">C. Tắt hẳn phần mềm IDE (ví dụ Code::Blocks) để tránh file đang bị khóa (lock) lưu không hoàn tất.</label>
    <label for="q17-4" class="q-label">D. Xóa file .INP và .OUT.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q18" id="q18-1" class="is-wrong">
    <input type="radio" name="q18" id="q18-2" class="is-wrong">
    <input type="radio" name="q18" id="q18-3" class="is-correct">
    <input type="radio" name="q18" id="q18-4" class="is-wrong">
    <div class="quiz-q">Câu 18: Lỗi sai nào ở giai đoạn nộp bài sẽ khiến toàn bộ công sức làm bài của thí sinh đổ sông đổ biển (0 điểm)?</div>
    <label for="q18-1" class="q-label">A. Viết thiếu comment giải thích trong code.</label>
    <label for="q18-2" class="q-label">B. Khai báo dư biến không sử dụng.</label>
    <label for="q18-3" class="q-label correct-label">C. Đặt sai tên file chương trình (chấm cpp) hoặc sai tên file Input/Output so với yêu cầu đề.</label>
    <label for="q18-4" class="q-label">D. Khai báo mảng dư 5 phần tử.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q19" id="q19-1" class="is-wrong">
    <input type="radio" name="q19" id="q19-2" class="is-correct">
    <input type="radio" name="q19" id="q19-3" class="is-wrong">
    <input type="radio" name="q19" id="q19-4" class="is-wrong">
    <div class="quiz-q">Câu 19: Khi giám thị yêu cầu kiểm tra lại file đã nộp, tiêu chí trực quan đầu tiên để xác nhận file copy có khớp với máy là gì?</div>
    <label for="q19-1" class="q-label">A. Xem ngày tạo file.</label>
    <label for="q19-2" class="q-label correct-label">B. So sánh SỐ BYTE (Dung lượng) của file trên máy và file đã nộp.</label>
    <label for="q19-3" class="q-label">C. Biên dịch lại file đã nộp.</label>
    <label for="q19-4" class="q-label">D. Đếm số dòng code.</label>
</div>

<div class="quiz-card">
    <input type="radio" name="q20" id="q20-1" class="is-wrong">
    <input type="radio" name="q20" id="q20-2" class="is-wrong">
    <input type="radio" name="q20" id="q20-3" class="is-wrong">
    <input type="radio" name="q20" id="q20-4" class="is-correct">
    <div class="quiz-q">Câu 20: Nếu lúc giám thị thu bài, bạn kiểm tra lại file đã nộp và phát hiện mình vừa gõ sai tên file (Ví dụ: BAI1.IN thay vì BAI1.INP), bạn NÊN làm gì?</div>
    <label for="q20-1" class="q-label">A. Bỏ cuộc và chấp nhận 0 điểm.</label>
    <label for="q20-2" class="q-label">B. Tự ý lấy USB của giám thị cắm vào sửa lại.</label>
    <label for="q20-3" class="q-label">C. Giữ im lặng và hi vọng hệ thống chấm thi tự động nhận diện được.</label>
    <label for="q20-4" class="q-label correct-label">D. Báo cáo ngay cho giám thị, trình bày rõ lý do và 'năn nỉ' xin phép đổi lại tên file trước khi chốt danh sách.</label>
</div>

<h1 style="text-align: center; color: #1e3a8a; margin-bottom: 30px;">🎯 🥇 🏆 CHÚC CÁC EM THI TỐT GẶP NHIỀU MAY MẮN VÀ ĐẠT KẾT QUẢ NHƯ Ý 🏆 🥇 🎯</h1>