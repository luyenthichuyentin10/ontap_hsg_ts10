## 🔍 Thuật Toán KMP (Knuth-Morris-Pratt)
<br>
<div class="step-card border-blue">
    <div class="step-badge bg-blue">1. Ý tưởng thuật toán</div>

**🔹 Khái niệm:**
Thuật toán KMP (Knuth-Morris-Pratt) là một thuật toán tìm kiếm chuỗi con (Pattern) bên trong một chuỗi mẹ (Text) hoặc tìm mảng con trong mảng lớn. 
- Thay vì duyệt trâu (Brute Force) với độ phức tạp $O(N \times M)$ (thử khớp tại mọi vị trí), KMP đạt được tốc độ $O(N + M)$.

**🔹 Ý tưởng cốt lõi:**
Khi đang so khớp mà gặp một ký tự **không khớp (mismatch)**, thuật toán duyệt trâu sẽ quay lùi lại từ đầu và dịch chuỗi con đi 1 vị trí. 
KMP thông minh hơn: Nó tận dụng những ký tự **đã khớp thành công** trước đó để biết chính xác cần phải dịch chuỗi con đi bao nhiêu vị trí mà không cần phải lùi con trỏ duyệt của chuỗi mẹ lại.

Để làm được điều này, KMP sử dụng một mảng phụ gọi là **Mảng LPS** (Longest Prefix Suffix - Tiền tố dài nhất đồng thời là hậu tố) hoặc mảng $\pi$ (Pi array).
- `LPS[i]` lưu độ dài của đoạn tiền tố dài nhất (không chứa toàn bộ chuỗi) trùng với đoạn hậu tố kết thúc tại vị trí $i$.
</div>

<div class="step-card border-orange">
    <div class="step-badge bg-orange">2. Ví dụ minh họa (Trace tay Mảng LPS)</div>

Cho chuỗi mẫu (Pattern) $P = \text{"ABABAC"}$. Cùng xây dựng mảng $LPS$.

<div class="geometry-table-container">
    <table class="styled-table">
        <thead>
            <tr>
                <th style="width: 10%;">i</th>
                <th style="width: 15%;">Chuỗi con P[0..i]</th>
                <th style="width: 45%;">Tiền tố = Hậu tố (Dài nhất)</th>
                <th style="width: 30%;">Giá trị LPS[i]</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>0</td>
                <td><code>A</code></td>
                <td>Không có (Quy ước luôn bằng 0)</td>
                <td><b>0</b></td>
            </tr>
            <tr style="background-color: #f8fafc;">
                <td>1</td>
                <td><code>AB</code></td>
                <td>Tiền tố {A} khác Hậu tố {B}</td>
                <td><b>0</b></td>
            </tr>
            <tr>
                <td>2</td>
                <td><code>ABA</code></td>
                <td>Tiền tố {<b>A</b>} = Hậu tố {<b>A</b>}</td>
                <td><b>1</b></td>
            </tr>
            <tr style="background-color: #f8fafc;">
                <td>3</td>
                <td><code>ABAB</code></td>
                <td>Tiền tố {<b>AB</b>} = Hậu tố {<b>AB</b>}</td>
                <td><b>2</b></td>
            </tr>
            <tr>
                <td>4</td>
                <td><code>ABABA</code></td>
                <td>Tiền tố {<b>ABA</b>} = Hậu tố {<b>ABA</b>}</td>
                <td><b>3</b></td>
            </tr>
            <tr style="background-color: #f8fafc;">
                <td>5</td>
                <td><code>ABABAC</code></td>
                <td>Ký tự 'C' phá vỡ chuỗi, không có tiền/hậu tố chung</td>
                <td><b>0</b></td>
            </tr>
        </tbody>
    </table>
</div>

Kết quả mảng $LPS = [0, 0, 1, 2, 3, 0]$.
**Cách dùng LPS so khớp:** Nếu Text là `ABABABC...`, khi khớp đến `P[5] = 'C'` bị sai (vì Text là 'B'). Nhìn vào `LPS[4] = 3`, KMP sẽ dịch chuỗi mẫu sao cho 3 ký tự đầu `ABA` khớp luôn vào vị trí hiện tại mà không cần xét lại!
</div>

<div class="step-card border-purple">
    <div class="step-badge bg-purple">3. Các bước cài đặt</div>

Thuật toán KMP được chia làm 2 giai đoạn độc lập:

1. **Giai đoạn Tiền xử lý (Xây dựng mảng LPS):** - Dùng 2 con trỏ `i` (chạy từ 1) và `len` (lưu độ dài chuỗi tiền tố-hậu tố hiện tại).
   - Nếu `P[i] == P[len]`: `len` tăng 1, `LPS[i] = len`, `i` tăng 1.
   - Nếu `P[i] != P[len]`: 
     - Nếu `len > 0`: Cập nhật `len = LPS[len - 1]` (Lùi lại tìm tiền/hậu tố ngắn hơn).
     - Nếu `len == 0`: `LPS[i] = 0`, `i` tăng 1.

2. **Giai đoạn So khớp (Searching):**
   - Dùng con trỏ `i` duyệt Text, `j` duyệt Pattern.
   - Tương tự như trên, nếu khớp thì tăng cả `i` và `j`.
   - Nếu `j == M` (khớp toàn bộ chuỗi): In ra vị trí tìm thấy, và lùi `j = LPS[j - 1]` để tiếp tục tìm các vị trí khác.
   - Nếu không khớp: Lùi `j = LPS[j - 1]`.
</div>

<div class="step-card border-green">
    <div class="step-badge bg-green">4. Code mẫu C++</div>

Dưới đây là phiên bản code C++ chuẩn mực và tối ưu nhất của thuật toán KMP.

```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Hàm 1: Xây dựng mảng LPS
vector<int> buildLPS(const string& P) {
    int M = P.length();
    vector<int> lps(M, 0); // Khởi tạo mảng LPS toàn số 0
    
    int len = 0; // Độ dài của tiền tố-hậu tố dài nhất hiện tại
    int i = 1;   // Con trỏ duyệt chuỗi P bắt đầu từ vị trí 1
    
    while (i < M) {
        if (P[i] == P[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            // Không khớp
            if (len != 0) {
                // Lùi len lại dựa vào thông tin đã tính toán
                // Chú ý: i KHÔNG TĂNG ở bước này
                len = lps[len - 1]; 
            } else {
                // Nếu len đã lùi về 0 mà vẫn không khớp
                lps[i] = 0;
                i++;
            }
        }
    }
    return lps;
}

// Hàm 2: So khớp KMP
void KMP_Search(const string& Text, const string& P) {
    int N = Text.length();
    int M = P.length();
    
    if (M == 0) return; // Tránh lỗi chuỗi mẫu rỗng
    
    // Bước 1: Xây dựng mảng LPS
    vector<int> lps = buildLPS(P);
    
    // Bước 2: So khớp
    int i = 0; // Con trỏ duyệt Text
    int j = 0; // Con trỏ duyệt Pattern
    
    bool found = false;
    
    while (i < N) {
        if (P[j] == Text[i]) {
            j++;
            i++;
        }
        
        if (j == M) {
            // Đã khớp toàn bộ chuỗi mẫu P
            cout << "Tim thay chuoi mau tai vi tri (index): " << i - j << "\n";
            found = true;
            // Tiếp tục tìm kiếm các trường hợp khớp tiếp theo (gối lên nhau)
            j = lps[j - 1]; 
        } 
        // Phát hiện không khớp
        else if (i < N && P[j] != Text[i]) {
            if (j != 0) {
                j = lps[j - 1]; // Lùi con trỏ j dựa vào mảng LPS
            } else {
                i++; // Nếu j đã bằng 0 thì chỉ việc tịnh tiến i
            }
        }
    }
    
    if (!found) cout << "Khong tim thay chuoi mau.\n";
}

int main() {
    string Text = "ABABDABACDABABCABAB";
    string Pattern = "ABABCABAB";
    
    cout << "Text: " << Text << "\n";
    cout << "Pattern: " << Pattern << "\n";
    
    KMP_Search(Text, Pattern);
    
    return 0;
}
```
</div>

<div class="step-card border-red">
    <div class="step-badge bg-red">5. Lưu ý sống còn trong lập trình thi đấu (CP Tips)</div>

<div class="geometry-table-container">
    <table class="styled-table">
        <thead>
            <tr>
                <th style="width: 25%;">Kỹ thuật / Sai lầm</th>
                <th style="width: 75%;">Phân tích & Kinh nghiệm</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><b>Lỗi tăng <code>i</code> sai lúc tính LPS</b></td>
                <td>Trong vòng lặp <code>while</code> của hàm <code>buildLPS</code>, khi <code>P[i] != P[len]</code> và <code>len > 0</code>, ta thực hiện lùi <code>len = lps[len-1]</code>. Lúc này <b>tuyệt đối không được tăng <code>i</code></b>. Ta phải giữ nguyên <code>i</code> để ở vòng lặp tiếp theo, nó được so sánh lại với ký tự tại cái <code>len</code> mới vừa bị lùi về.</td>
            </tr>
            <tr>
                <td><b>Khớp chuỗi gối lên nhau (Overlapping)</b></td>
                <td>Thuật toán KMP cho phép tìm các chuỗi gối lên nhau. Ví dụ: Text = <code>AAAA</code>, Pattern = <code>AA</code>. KMP sẽ in ra 3 vị trí (index 0, 1, 2). Nhờ dòng lệnh <code>j = lps[j - 1]</code> sau khi tìm thấy, nó không bỏ sót trường hợp nào. Nếu muốn không gối lên nhau, ta sửa thành <code>j = 0;</code> sau khi tìm thấy.</td>
            </tr>
            <tr>
                <td><b>Mảng con thay vì Chuỗi con</b></td>
                <td>KMP không chỉ dùng cho kiểu <code>string</code>. Trong các bài tập khó, Đề bài thường yêu cầu tìm <b>mảng con</b> trong một mảng số nguyên. Thầy hãy nhắc học sinh: Chỉ cần thay đổi tham số truyền vào từ <code>string</code> sang <code>vector&lt;int&gt;</code>, mọi logic của <code>LPS</code> và <code>KMP_Search</code> giữ nguyên 100%!</td>
            </tr>
            <tr>
                <td><b>Chu kỳ của chuỗi (String Period)</b></td>
                <td>Ứng dụng cực hay của mảng LPS: Một chuỗi có độ dài $N$ có thể được lặp lại từ một chuỗi con nhỏ hơn. Chu kỳ nhỏ nhất của nó chính là $T = N - LPS[N-1]$. Nếu $N$ chia hết cho $T$, thì chuỗi đó được lặp lại từ chuỗi con độ dài $T$.</td>
            </tr>
        </tbody>
    </table>
</div>
</div>