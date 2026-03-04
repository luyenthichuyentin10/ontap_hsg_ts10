## 📐 Hình học tọa độ phẳng (Geometry)
<br>
<div class="step-card border-blue">
<div class="step-badge bg-blue">1. Khái niệm & Công thức cơ bản</div>

**1. Điểm và Hệ tọa độ Oxy**
- Mỗi điểm $P$ được xác định bởi cặp tọa độ $(x, y)$. 
- Trong C++, ta nên dùng struct để quản lý.
```cpp
struct Point {
    double x, y;
};
```
**2. Khoảng cách giữa hai điểm:** $A(x_1, y_1)$ và $B(x_2, y_2)$
<div class="math-formula">
$d(A, B) = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$
</div>

```cpp
double distance(Point A, Point B) {
    return sqrt((A.x - B.x)*(A.x - B.x) + (A.y - B.y)*(A.y - B.y));
    // Nếu muốn tránh xử lý số thực ta có thể không cần lấy căn bậc 2
}
```

**3. Trung điểm $M$ của đoạn thẳng $AB$**
<div class="math-formula">
$M\left(\frac{x_1 + x_2}{2}, \frac{y_1 + y_2}{2}\right)$
</div>

```cpp
void midpoint(Point A, Point B, Point M) {
    M.x = (A.x + B.x) / 2;
    M.y = (A.y + B.y) / 2;
}
```

**4. Diện tích của tam giác (công thức định thức)**
<div class="math-formula">
$S = \frac{1}{2} |x_1(y_2 - y_3) + x_2(y_3 - y_1) + x_3(y_1 - y_2)|$
</div>

```cpp
double triangleArea(Point A, Point B, Point C) {
    return fabs(A.x*(B.y - C.y) + B.x*(C.y - A.y) + C.x*(A.y - B.y)) / 2.0; // hàm fabs dùng để lấy giá trị tuyệt đối cho số thực
}
```

<div class="important-note">
ℹ️ Ta có thể kiểm tra 3 điểm thẳng hằng bằng cách kiểm tra diện tích của tam giác tạo bởi 3 điểm đó có bằng $0$ hay không
</div>

**5. Diện tích đa giác (Công thức Shoelace)**

Cho đa giác có $n$ đỉnh $(x_i, y_i)$:
<div class="math-formula">
$S = \frac{1}{2} | \sum_{i=1}^{n-1} (x_iy_{i+1} - x_{i+1}y_i) + (x_ny_1 - x_1y_n) |$
</div>

```cpp
struct Point {
    double x, y;
};

/**
 * Hàm tính diện tích đa giác bằng công thức Shoelace (Công thức dây giày)
 * @param p: Danh sách các đỉnh của đa giác được liệt kê theo thứ tự (cùng hoặc ngược chiều kim đồng hồ)
 * @return: Diện tích của đa giác
 */
double calculatePolygonArea(const vector<Point>& p) {
    double area = 0.0;
    int n = p.size();

    // Duyệt qua từng cặp đỉnh liên tiếp
    for (int i = 0; i < n; i++) {
        // Đỉnh kế tiếp (sử dụng toán tử chia lấy dư để quay về đỉnh đầu khi ở đỉnh cuối)
        int j = (i + 1) % n;
        
        // Công thức Shoelace: tổng các tích x[i]*y[j] - x[j]*y[i]
        area += (p[i].x * p[j].y);
        area -= (p[j].x * p[i].y);
    }

    // Lấy trị tuyệt đối và chia 2 theo công thức
    return fabs(area) / 2.0;
}
```
</div>

<div class="step-card border-yellow">
<div class="step-badge bg-yellow">2. Phương trình đường thẳng</div>

Trong chương trình THCS, đường thẳng thường được biểu diễn dưới dạng hàm số bậc nhất:
<div class="math-formula">
$y = ax + b \text{ (với } a \neq 0)$
<div class="formula-notes">

Trong đó:
- **$a$**: Gọi là **Hệ số góc** của đường thẳng. Nó quyết định độ dốc và hướng của đường thẳng.
- **$b$**: Gọi là **Tung độ gốc**. Đây là giao điểm của đường thẳng với trục tung (Oy).
- Nếu $a > 0$: Hàm số đồng biến (đường thẳng đi lên từ trái sang phải).
- Nếu $a < 0$: Hàm số nghịch biến (đường thẳng đi xuống từ trái sang phải).
</div>
</div>

**1. Phương trình đường thẳng dạng tổng quát:**
<div class="math-formula">
$Ax + By + C = 0$
<div class="formula-notes">

- Nếu đường thẳng nằm ngang (song song Ox): $By + C = 0$
- Nếu đường thẳng đứng (song song Oy): $Ax + C = 0$
- Nếu đi qua gốc tọa độ $0$: $Ax + By = 0$ $(A^2 + B^2 \ne 0)$
</div>
</div>

**2. Phương trình đường thẳng đi qua hai điểm phân biệt**

* Xét hai điểm phân biệt $A(x_a, y_a)$ và $B(x_b, y_b)$, ta có phương trình đường thẳng đi qua hai điểm này như sau:
<div class="math-formula">
$\frac{x - x_a}{x_b - x_a} = \frac{y - y_a}{y_b - y_a}$
</div>

* Mối liên hệ với phương trình dạng tổng quát
    - Để thuận tiện cho việc lập trình và tránh lỗi chia cho $0$ khi đường thẳng đứng hoặc nằm ngang, ta chuyển đổi phương trình trên về dạng tổng quát $Ax + By + C = 0$:</p>
<div class="math-formula">
$(x - x_a) \cdot (y_b - y_a) = (y - y_a) \cdot (x_b - x_a)$

$\Leftrightarrow xy_b - xy_a - x_ay_b + x_ay_a = x_by - x_by_a - x_ay + x_ay_a$

$\Leftrightarrow xy_b - xy_a - x_ay_b = x_by - x_by_a - x_ay$

$\Leftrightarrow (y_b - y_a)x + (x_a - x_b)y + x_by_a - x_ay_b = 0$
</div>
    
* Bằng cách đặt các hệ số tương ứng, ta có mối liên hệ trực tiếp:
<div class="math-formula">
$A = y_b - y_a$

$B = x_a - x_b$

$C = x_by_a - x_ay_b$
<div class="formula-notes">

Kết quả ta được phương trình dạng tổng quát:

$Ax + By + C = 0$ **với điều kiện $A^2 + B^2 \neq 0$**

**Ứng dụng lập trình:** Cách tính này giúp xác định phương trình đường thẳng chỉ bằng các phép tính nhân và trừ, đảm bảo tính ổn định cho mọi vị trí của hai điểm $A, B$.
</div>
</div>

**3. Khoảng cách từ điểm $M(x_0, y_0)$ đến đường thẳng $\Delta: ax + by + c = 0$**
<div class="math-formula">
$d(M, \Delta) = \frac{|ax_0 + by_0 + c|}{\sqrt{a^2 + b^2}}$
</div>

```cpp
// Cấu trúc điểm trong mặt phẳng Oxy
struct Point {
    double x, y;
};

// Cấu trúc đường thẳng dạng tổng quát Ax + By + C = 0
struct Line {
    double a, b, c;
};

/**
 * Hàm tính khoảng cách từ điểm M đến đường thẳng d
 * Công thức: d(M, d) = |A*xM + B*yM + C| / sqrt(A^2 + B^2)
 */
double distancePointToLine(Point M, Line d) {
    // Tính tử số: Trị tuyệt đối của (A*x0 + B*y0 + C)
    double numerator = fabs(d.a * M.x + d.b * M.y + d.c);
    
    // Tính mẫu số: Căn bậc hai của (A^2 + B^2)
    double denominator = sqrt(d.a * d.a + d.b * d.b);
    
    // Trả về kết quả khoảng cách
    return numerator / denominator;
}
```
</div>

<div class="step-card border-orange">
<div class="step-badge bg-orange">2. Các dạng toán lập trình thường gặp</div>

**1. Vị trí tương đối của điểm so với đường thẳng, tia và đoạn thẳng**
* Cho điểm $M(x_m, y_m)$ và đoạn thẳng $AB$ xác định bởi hai điểm $A(x_a, y_a), B(x_b, y_b)$.
* Đặt hàm số đường thẳng đi qua $A, B$:
<div class="math-formula">
$F(x, y) = (y_a - y_b)x + (x_b - x_a)y + (x_ay_b - x_ay_b)$
</div>

* **Các điều kiện kiểm tra:**
    * Điểm $M$ thuộc đường thẳng $AB$: Khi $F(x_m, y_m) = 0$.
    * Điểm $M$ thuộc đoạn thẳng $AB$: 
        * Khi $F(x_m, y_m) = 0$ 
        * Đồng thời tọa độ $M$ nằm trong khoảng giới hạn của $A$ và $B$:
            * $\text{Min}(x_a, x_b) \le x_m \le \text{Max}(x_a, x_b)$
            * $\text{Min}(y_a, y_b) \le y_m \le \text{Max}(y_a, y_b)$
    * Điểm $M$ thuộc tia $AB$: 
        * Khi $F(x_m, y_m) = 0$ 
        * Điểm $M$ nằm cùng phía với $B$ so với gốc $A$: $(x_m - x_a)(x_b - x_a) \ge 0$$(y_m - y_a)(y_b - y_a) \ge 0$
```cpp
const double EPS = 1e-9;

struct Point {
    double x, y;
};

// Hàm bổ trợ tính giá trị F(x, y) dựa trên đường thẳng AB
double F(Point M, Point A, Point B) {
    return (A.y - B.y) * M.x + (B.x - A.x) * M.y + (A.x * B.y - B.x * A.y);
}

// 1. Kiểm tra M thuộc đường thẳng AB
bool isPointOnLine(Point M, Point A, Point B) {
    return fabs(F(M, A, B)) < EPS;
}

// 2. Kiểm tra M thuộc đoạn thẳng AB
bool isPointOnSegment(Point M, Point A, Point B) {
    // Phải thuộc đường thẳng trước
    if (!isPointOnLine(M, A, B)) return false;
    
    // Kiểm tra giới hạn tọa độ (Bounding Box)
    return M.x >= min(A.x, B.x) - EPS && M.x <= max(A.x, B.x) + EPS &&
           M.y >= min(A.y, B.y) - EPS && M.y <= max(A.y, B.y) + EPS;
}

// 3. Kiểm tra M thuộc tia AB (Gốc A, đi qua B)
bool isPointOnRay(Point M, Point A, Point B) {
    // Phải thuộc đường thẳng trước
    if (!isPointOnLine(M, A, B)) return false;
    
    // Kiểm tra cùng hướng với vector AB
    return (M.x - A.x) * (B.x - A.x) >= -EPS && 
           (M.y - A.y) * (B.y - A.y) >= -EPS;
}
```

**2. Vị trí tương đối giữa 2 đường thẳng**

* Cho 2 đường thẳng có phương trình dạng tổng quát:
    * $d_1: A_1x + B_1y + C_1 = 0$
    * $d_2: A_2x + B_2y + C_2 = 0$
* Để xét vị trí tương đối, ta lập các định thức của hệ phương trình:
<div class="math-formula">
$D = A_1B_2 - A_2B_1$

$D_x = B_1C_2 - B_2C_1$

$D_y = C_1A_2 - C_2A_1$
</div>

* Các điều kiện kiểm tra:
    * Cắt nhau: Nếu $D \neq 0$. Hai đường thẳng cắt nhau tại điểm phân biệt $M(x_m, y_m)$ với tọa độ: 
        * $x_m = D_x / D$ 
        * $y_m = D_y / D$
    * Song song: Nếu $D = 0$ và ($D_x$ hoặc $D_y$ khác $0$).
    * Trùng nhau: Nếu $D = D_x = D_y = 0$.
    * Vuông góc: Nếu $A_1A_2 + B_1B_2 = 0$.
```cpp
const double EPS = 1e-9;

struct Line {
    double a, b, c;
};

struct Point {
    double x, y;
};

// 0: hai đường thẳng song song, 1: hai đường thẳng cắt nhau, 2: hai đường thẳng vuông góc, 3: hai đường thẳng trùng nhau
int checkRelativePosition(Line d1, Line d2, Point & M) {
    // Tính các định thức Cramer
    double D  = d1.a * d2.b - d2.a * d1.b;
    double Dx = d1.b * d2.c - d2.b * d1.c;
    double Dy = d1.c * d2.a - d2.c * d1.a;

    // Kiểm tra vuông góc (điều kiện bổ sung)
    if (fabs(d1.a * d2.a + d1.b * d2.b) < EPS) return 2;

    if (fabs(D) > EPS) {
        // Trường hợp cắt nhau
        M.x = Dx / D;
        M.y = Dy / D;
        return 1;
    } 
    else if (fabs(Dx) > EPS || fabs(Dy) > EPS) return 0; // D = 0
        else return 3;
}
```

**3. Tìm giao điểm của 2 đoạn thẳng**

Cho 2 đoạn thẳng $AB$ và $CD$ với tọa độ các đầu mút $A(x_a, y_a), B(x_b, y_b), C(x_c, y_c), D(x_d, y_d)$.

Phương pháp giải quyết bài toán:
* B1. Tìm giao điểm $M$ của 2 đường thẳng chứa đoạn thẳng $AB$ và $CD$:
    * Xác định phương trình đường thẳng $AB$ và $CD$ dưới dạng $Ax + By + C = 0$.
    * Sử dụng định thức Cramer ($D, D_x, D_y$) để tìm tọa độ giao điểm $M(x_m, y_m)$.
* B2. Kiểm tra $M$ có thuộc đồng thời cả 2 đoạn $AB$ và $CD$ hay không:
    * Nếu $M$ nằm trong phạm vi tọa độ của cả hai đoạn thẳng, thì $M$ chính là giao điểm cần tìm.
    * Ngược lại, kết luận hai đoạn thẳng không có giao điểm.
* Điều kiện để $M$ thuộc đoạn $AB$:
<div class="math-formula">
$\min(x_a, x_b) \le x_m \le \max(x_a, x_b)$

$\min(y_a, y_b) \le y_m \le \max(y_a, y_b)$
</div>

```cpp
const double EPS = 1e-9;

struct Point {
    double x, y;
};

// Bước 1: Hàm tìm giao điểm M của 2 đường thẳng chứa đoạn thẳng
bool findLineIntersection(Point A, Point B, Point C, Point D, Point &M) {
    // Phương trình d1 qua AB: a1x + b1y + c1 = 0
    double a1 = A.y - B.y;
    double b1 = B.x - A.x;
    double c1 = -a1 * A.x - b1 * A.y;

    // Phương trình d2 qua CD: a2x + b2y + c2 = 0
    double a2 = C.y - D.y;
    double b2 = D.x - C.x;
    double c2 = -a2 * C.x - b2 * C.y;

    // Định thức Cramer
    double D_det = a1 * b2 - a2 * b1;
    if (fabs(D_det) < EPS) return false; // Song song hoặc trùng

    M.x = (b1 * c2 - b2 * c1) / D_det;
    M.y = (c1 * a2 - c2 * a1) / D_det;
    return M.x >= min(A.x, B.x) - EPS && M.x <= max(A.x, B.x) + EPS &&
           M.y >= min(A.y, B.y) - EPS && M.y <= max(A.y, B.y) + EPS;
}
```

**4. Vị trí điểm so với đa giác** 
* **Phương pháp vị trí điểm so với các cạnh của đa giác (đúng với đa giác lồi)**.
    1. **Thiết lập phương trình:** Với mỗi cạnh của đa giác đi qua hai điểm $d_i(x_1, y_1)$ và $d_{i+1}(x_2, y_2)$, ta lập hàm số: $F(x, y) = (y_1 - y_2)x + (x_2 - x_1)y + (x_1y_2 - x_2y_1)$
    2. **Kiểm tra phía:** Thay tọa độ điểm $M(x_m, y_m)$ vào hàm số trên để tính giá trị $F(x_m, y_m)$.
    3. **Xét dấu:** Nếu tất cả các kết quả $F(x_m, y_m)$ tính được từ mọi cạnh đều cùng dấu (tất cả cùng dương hoặc tất cả cùng âm), thì điểm $M$ nằm bên trong đa giác.
        * Nếu có một kết quả bằng $0$, điểm $M$ nằm trên cạnh đó.
        * Nếu dấu bị thay đổi (có cả dương và âm), điểm $M$ nằm bên ngoài đa giác.
* **Ví dụ:** Khi đi vòng quanh đa giác theo chiều kim đồng hồ, điểm $M$ "ở bên trong" nghĩa là nó luôn nằm bên phải của tất cả các con đường (cạnh) mà ta đi qua.
```cpp
const double EPS = 1e-9;

struct Point {
    double x, y;
};

/**
 * Bước 1: Hàm tính giá trị F(x, y) của một điểm M đối với đường thẳng đi qua A, B
 * Công thức bậc nhất: F(x, y) = (ya - yb)*x + (xb - xa)*y + (xa*yb - xb*ya)
 */
double calculateF(Point M, Point A, Point B) {
    return (A.y - B.y) * M.x + (B.x - A.x) * M.y + (A.x * B.y - B.x * A.y);
}

/**
 * Bước 2: Kiểm tra vị trí của điểm M so với đa giác lồi N đỉnh
 */
bool checkPointInConvexPolygon(Point M, const vector<Point>& p) {
    int n = p.size();
    bool isPositive = false;
    bool isNegative = false;

    for (int i = 0; i < n; i++) {
        // Lấy đỉnh thứ i và đỉnh kế tiếp (i+1) để tạo thành một cạnh
        int j = (i + 1) % n;
        
        // Thế tọa độ điểm M vào phương trình đường thẳng của cạnh d[i]d[j]
        double val = calculateF(M, p[i], p[j]);

        // Kiểm tra dấu của giá trị vừa tính được
        if (val > EPS) isPositive = true;
        else if (val < -EPS) isNegative = true;
        else {
            // Nếu F(x, y) xấp xỉ bằng 0, điểm M nằm trên cạnh
            return true;
        }

        // Nếu tồn tại cả dấu dương và dấu âm, điểm M nằm ở hai phía khác nhau của các cạnh
        if (isPositive && isNegative) return false;
    }

    // Nếu chỉ toàn dấu dương hoặc chỉ toàn dấu âm, điểm M luôn nằm về một phía
    return true;
}
```

* **Vị trí điểm so với đa giác bất kỳ (Phương pháp Bắn tia - Ray Casting)**: Đây là thuật toán tổng quát nhất, áp dụng được cho cả đa giác lồi, đa giác lõm và đa giác tự cắt.
    1. **Nguyên lý:** 
        * Vẽ một tia nằm ngang (song song trục hoành) bắt đầu từ điểm $M$ kéo dài vô tận về phía bên phải. 
        * Đếm số lần tia này cắt các cạnh của đa giác.
    2. **Các bước thực hiện:**
        * B1: Kiểm tra xem $M$ có thuộc cạnh nào của đa giác không. Nếu có, kết luận $M$ thuộc miền trong và kết thúc.
        * B2: Kẻ tia $MN$ song song với trục hoành, với điểm $N$ có hoành độ rất lớn (lớn hơn giá trị hoành độ lớn nhất của các đỉnh đa giác).
        * B3: Xác định $d$ là số giao điểm của $MN$ với các cạnh của đa giác. Các trường hợp đặc biệt để tính là 1 giao điểm:
            * Đỉnh $d_i$ không thuộc $MN$, đỉnh $d_{i+1}$ nằm trên $MN$, và 2 đỉnh $d_i, d_{i+2}$ nằm về hai phía khác nhau so với đường thẳng $MN$.
            * Đỉnh $d_{i-1}, d_{i+2}$ nằm ngoài $MN$, hai đỉnh $d_i, d_{i+1}$ thuộc $MN$, và $d_{i-1}, d_{i+1}$ nằm về hai phía khác nhau so với $MN$.
            * Cạnh $(d_i, d_{i+1})$ không có đỉnh nào thuộc $MN$ nhưng lại cắt đoạn thẳng $MN$.
    3. **Kết luận:** Nếu số giao điểm $d$ là số lẻ, điểm $M$ nằm bên trong đa giác. Nếu $d$ là số chẵn, điểm $M$ nằm bên ngoài.
```cpp
struct Point { double x, y; };

// Kiểm tra M có nằm trong đa giác bất kỳ p (N đỉnh)
bool isInsidePolygon(Point M, vector<Point>& p) {
    int n = p.size();
    bool inside = false;
    for (int i = 0, j = n - 1; i < n; j = i++) {
        // Kiểm tra xem M có nằm trong khoảng tung độ của cạnh (pi, pj) không
        bool intersectY = ((p[i].y > M.y) != (p[j].y > M.y));
        
        // Tính hoành độ giao điểm của tia ngang từ M với cạnh (pi, pj)
        // Dựa trên phương trình đoạn thẳng đi qua 2 điểm phân biệt
        if (intersectY) {
            double intersectX = (p[j].x - p[i].x) * (M.y - p[i].y) / (p[j].y - p[i].y) + p[i].x;
            if (M.x < intersectX) {
                inside = !inside; // Mỗi lần cắt thì đảo trạng thái (chẵn/lẻ)
            }
        }
    }
    return inside;
}
```
<div class="geometry-table-container">
    <table class="styled-table">
        <thead>
            <tr>
                <th>Đặc điểm</th>
                <th>Phương pháp Xét phía (Tích chéo)</th>
                <th>Phương pháp Bắn tia (Ray Casting)</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Loại đa giác</td>
                <td>Chỉ áp dụng chính xác cho <b>Đa giác lồi</b>.</td>
                <td>Áp dụng cho <b>mọi loại đa giác</b> (lồi, lõm, hình sao).</td>
            </tr>
            <tr>
                <td>Nguyên lý</td>
                <td>Kiểm tra điểm $M$ luôn nằm về một phía so với các cạnh.</td>
                <td>Đếm số giao điểm của tia ngang phát ra từ $M$.</td>
            </tr>
            <tr>
                <td>Độ khó cài đặt</td>
                <td><span class="text-easy">Dễ</span>: Công thức đơn giản, ít rẽ nhánh.</td>
                <td><span class="text-medium">Trung bình - Khó</span>: Cần xử lý nhiều trường hợp biên.</td>
            </tr>
            <tr>
                <td>Độ ổn định</td>
                <td>Rất cao, ít bị ảnh hưởng bởi sai số.</td>
                <td>Dễ sai nếu không xử lý tốt điểm đi qua đỉnh.</td>
            </tr>
            <tr>
                <td>Khuyên dùng</td>
                <td>Dùng cho bài toán bao lồi, đa giác lồi.</td>
                <td>Dùng khi đa giác có hình dạng bất kỳ (lõm).</td>
            </tr>
        </tbody>
    </table>
</div>

</div>

<div class="important-note">
💡 <b>Lưu ý trong lập trình thi đấu:</b>

*Hình học trong lập trình thi đấu không khó về tư duy thuật toán nhưng lại cực kỳ khắt khe về <b>độ chính xác</b> và các <b>trường hợp đặc biệt</b>. Một sai số nhỏ hoặc quên kiểm tra đường thẳng đứng có thể dẫn đến kết quả sai ***(WA)***.*

- **Sai số số thực (EPS):** Tuyệt đối không so sánh bằng `== 0`. Luôn dùng `fabs(x) < 1e-9`
- **Kiểu dữ liệu:** Ưu tiên dùng `double` cho tọa độ. Nếu tọa độ là số nguyên rất lớn ($> 10^9$), hãy tính tích trước bằng `long long` rồi mới chuyển sang `double` để tránh mất độ chính xác.
- **Tránh xử lý số thực:** Trong các công thức, nếu có thể nhân chéo để so sánh thì hãy nhân chéo thay vì chia, hoặc bình phương thay vì lấy căn bậc 2 vì phép chia luôn tạo ra sai số.
- **Trường hợp đặc biệt:** Luôn kiểm tra các trường hợp đường thẳng đứng (song song Oy) vì hệ số góc $a$ sẽ không xác định (mẫu số bằng 0).
- **Tư duy đơn giản hóa vấn đề:** 
    - Một số bài hình học đặc biệt có thể chuyển về dạng duyệt ma trận để giải như tính diện tích phủ, diện tích giao nhau.
    - Một số bài có thể sử dụng phương pháp chiếu xuống trục tung và trục hoành sau đó xét trong khoảng và ngoài khoảng thay vì áp dụng các công thức phức tạp.
</div>