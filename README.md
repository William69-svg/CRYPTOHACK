# BLOG 1:
# RSA và các phương thức tấn công
## 1. RSA là gì?
Trong mật mã học, RSA là một thuật toán mật mã hoá khoá công khai được sử dụng rộng rãi trong các hệ thống bảo mật hiện đại. Được đặt tên theo tên của ba nhà khoa học Ron Rivest, Adi Shamir và Leonard Adleman, RSA đã trở thành một tiêu chuẩn trong việc bảo vệ thông tin nhạy cảm.

## 2. RSA hoạt động như thế nào?
RSA dựa trên nguyên tắc của một cặp khóa:

Khóa công khai(N, e): Được chia sẻ rộng rãi và dùng để mã hóa dữ liệu.

Khóa riêng(d): Được giữ bí mật và chỉ người sở hữu mới có thể dùng để giải mã dữ liệu đã được mã hóa bằng khóa công khai tương ứng.

**Quá trình mã hóa và giải mã**:

**Tạo cặp khóa**: Người dùng tạo ra một cặp khóa gồm khóa công khai và khóa riêng.

Để tạo ra một cặp khoá công khai ta tạo ra một cặp số (N, e) tương ứng với:

N = p*q (p, q là hai số nguyên tố) 

Sau đó tính: phi(N) = (p - 1)*(q - 1) và chọn một số e ( 1 < e < $\phi(N)$ ) sao cho e và $\phi(N)$ là hai số nguyên tố cùng nhau

Và cuối cùng là tạo ra khoá riêng d 

Với: d = $e^{-1} \pmod{phi(N)}$ hay là $d \cdot e \equiv 1 \pmod{\phi(N)}$


**Mã hóa**: Người gửi sử dụng khóa công khai của người nhận để mã hóa thông điệp.

Với một thông tin cần mã hoá m, ta sẽ mã hoá như sau:

c = $m^e \pmod{N}$

**Giải mã**: Người nhận sử dụng khóa riêng của mình để giải mã thông điệp đã được mã hóa.

Để mã hoá thông điệp c trên, ta sẽ giải mã như sau:

m = $c^d \pmod{N}$ vì khi này $c^d \equiv  m^{ed} \pmod{N}$

**Sự bảo mật của RSA**:

Khó phá vỡ: Việc phá vỡ mã RSA đòi hỏi phải giải quyết một bài toán toán học phức tạp, đó là việc phân tích một số lớn thành tích của các số nguyên tố và bài toán RSA

## 3. Các phương thức tấn công
Như đã tìm hiểu bên trên thì RSA là một thuật toán mã hoá phức tạp và đòi hỏi nhiều sự tính toán 
Sau đây sẽ là một vài phương thức tấn công trong RSA 
**I. Factoring Large Integers (Phân tích các số nguyên lớn)**:

Phương thức tiếp cận đầu tiên và trực diện nhất là phân tích N thành các thừa số nguyên tố, đây là lối tấn công đầu tiên đơn giản và trực diện nhất với việc ta đã biết trước số tự nhiên e. 

Như được biết bên trên, N được tạo thành từ 2 số nguyên tố lớn vì vậy nên ta có thể sử dụng các tool như Factordb để có thể phân tích N trực tiếp. Ngoài ra còn có thể sử dụng các thuật toán phân tích số nguyên tố: Các thuật toán như thuật toán chia thử, thuật toán rho của Pollard, thuật toán số sàng,...

Với một số trường hợp khi N không dễ phân tích thì ta sẽ có những hướng tiếp cận khác, tuy nhiên, việc phân tích module N vẫn là một bước thiết yếu và quan trọng trong việc giải các bài toán phức tạp hơn.

**II. Elementary Attacks**:

**1.COMMON MODULUS**

**1.1 External Attacks**

Trong trường hợp này, sẽ chỉ có một module N được sử dụng, vì vậy với mỗi người dùng thứ i thì sẽ có cặp ($e_i$, $d_i$) tương ứng thì mỗi người sẽ có một khoá cá nhân và khoá công khai lần lượt là $(N, e_i)$ và $(N, d_i)$ 

Tuy nhiên đây là một cách tạo khoá có nhiều sơ hở, vì giả sử có Alice và Bob sở hữu 2 cặp $(e_a, d_a)$ và $(e_b, d_b)$

Ta hoàn toàn có thể mã hoá được $(d_a)$ là khoá cá nhân của Alice khi ta có thể phân tích N thành các thừa số nguyên tố, sau đó ta có thể tìm ra $\phi(N)$ = $(p - 1) \cdot (q - 1)$ hay $\phi(N)$ = $N \cdot \prod_{p \mid N} \left( 1 - \frac{1}{p} \right)$

Khi đó: để tìm $d_a$, đây chính nghịch đảo của $e_a$ theo module $\phi(N)$, $d_a \equiv e_a^{-1} \pmod{phi(N)}$ 

Để mở rộng vấn đề ta giả sử có 2 đoạn mã hoá: $C_a$ và $C_b$

Với hai cặp khoá $(N, e_a)$ và $(N, e_b)$, thì: $C_a = m^{e_a} \pmod{N}$ và $C_b = m^{e_b} \pmod{N}$

Khi đó, ta nhận thấy rằng khi ta luỹ thừa hai đoạn Cipher text theo luỹ thừa 2 số nguyên u và v ta sẽ có: $C_a^{u} = (m^{e_a})^{u} \pmod{N}$ và $C_b^{v} = (m^{e_b})^{v} \pmod{N}$

Khi thực hiện phép nhân của hai Cipher text $C_a^{u}$ và $C_b^{v}$ ta sẽ có: $C_a^{u} \cdot C_b^{v}$ = $(m^{{e_a} \cdot u + {e_b} \cdot v}) \pmod{N}$ (*)

Ta thấy $Gcd(e_a, e_b) = 1$, nên theo Định lý Euclid mở rộng: ${e_a} \cdot u + {e_b} \cdot v = 1$ vậy nên (*) trở thành $m^{1} = m \pmod{N}$ và ta đã tìm được thông điệp ban đầu mà không cần phải dùng tới khoá riêng tư d!

Ví dụ ta có challenge sau:

```python

from Crypto.Util.number import getPrime, bytes_to_long

# Tạo challenge RSA với thông điệp cố định
def generate_rsa_challenge():
    # Tạo các số nguyên tố p và q
    p = getPrime(512)  
    q = getPrime(512)  
    n = p * q          

    # Số mũ công khai (nguyên tố cùng nhau)
    e1 = 3
    e2 = 5

    # Thông điệp gốc
    m = 14335093139487056536735638447103685144813354889847868574725083697953

    # Mã hóa thông điệp
    c1 = pow(m, e1, n)  # Mã hóa bằng e1
    c2 = pow(m, e2, n)  # Mã hóa bằng e2

    # Xuất ra challenge
    print("RSA Elementary Attack Challenge:")
    print(f"n = {n}")
    print(f"e1 = {e1}")
    print(f"e2 = {e2}")
    print(f"c1 = {c1}")
    print(f"c2 = {c2}")
    print("\nKhôi phục thông điệp ban đầu!")

# Gọi hàm tạo challenge
generate_rsa_challenge()
```
Đoạn code giải bài toán trên:

```python

from sympy import gcd
from Crypto.Util.number import long_to_bytes

# Hàm mở rộng Euclid để tìm hệ số u và v
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x, y = extended_gcd(b, a % b)
# Đây là phần đệ quy của thuật toán. Lúc đầu, ta bắt đầu với hai số a và b. Sau đó thuật toán đệ quy tính Gcd(b, a mod b)
    return g, y, x - (a // b) * y
```
Công thức: $x \cdot a + y \cdot b = g$

Sau đó giả trị x: $x' = x - (\frac{a}{b} \cdot y)$
```python
# Hàm giải RSA elementary attack
def rsa_elementary_attack(n, e1, e2, c1, c2):
    # Kiểm tra e1 và e2 nguyên tố cùng nhau
    if gcd(e1, e2) != 1:
        raise ValueError("e1 và e2 phải nguyên tố cùng nhau!")

    # Tìm u, v sao cho u * e1 + v * e2 = 1
    _, u, v = extended_gcd(e1, e2)     #_ là nơi giữ giá trị trả về của GCD, nhưng vì không cần dùng giá trị này, nên chỉ cần dùng dấu gạch dưới để bỏ qua.

    # Xử lý trường hợp u hoặc v âm
    if u < 0:
        u = -u
        c1 = pow(c1, -1, n)  # Lấy nghịch đảo module n
    if v < 0:
        v = -v
        c2 = pow(c2, -1, n)  # Lấy nghịch đảo module n

    # Tính m = (c1^u * c2^v) mod n
    m = (pow(c1, u, n) * pow(c2, v, n)) % n

    # Chuyển m từ số nguyên về bytes
    return long_to_bytes(m)

# Thông số từ challenge
n = 102509801431475513113632759405021708207134482900946523740258105155721915721875987591259354369957545676459355717081653385140592880676859853394283415003940246191917673354284924121245541547108316804097707125324211253771419523408358416892644012599120115213560052716513150166241662681492648587784410763296823670151
e1 = 3
e2 = 5
c1 = 246067863435481974501802737137906280268222622493918282682418086185011658939126985505645917416601616817614711313309629622700545683826512334889550539439238671008
c2 = 79505305669227621112673189279973329448042909831993679816277760788060431964712986127036555109086883211685907323323313568226807615634856742932649964186790580299

# Giải bài toán
m = rsa_elementary_attack(n, e1, e2, c1, c2)

# In thông điệp khôi phục
print("Thông điệp gốc:", m.decode())
```
Thông điệp gốc: b"KCSC{3xt3rn4l_4tt4ack!}"

**1.2 Internal Attacks**

Trong một trường hợp khác ta sở hữu bộ khoá $(N, e_0, d_0)$ và đồng thời nạn nhân cũng sở hữu một bộ khoá $(N, e_{victim}, d_{victim})$

Ở đây ta có: $e_0 \cdot d_0 \equiv 1 \pmod{\phi{N}}$ và  $e_{victim} \cdot d_{victim} \equiv 1 \pmod{\phi{N}}$ 

Khi đó, ta nhận thấy: $(e_0 \cdot d_0 - 1) = k \cdot \phi{N}$ và $(e_{victim} \cdot d_{victim} - 1) = k \cdot \phi{N}$ và $\phi{N}$ không thể phân tích thành các thừa số nguyên tố

Vậy thì ta có thể tìm k (với $k \in \mathbb N, \exists! k$) sao cho khi sử dụng bộ số của chúng ta:

$k = \frac{e_0 \cdot d_0 - 1}{\phi{N}} > \frac{e_0 \cdot d_0 - 1}{N}$ 

Sau khi tìm k với bộ thương $\frac{e_0 \cdot d_0 - 1}{N}$ thì ta tăng tiến giá trị k tới khi nhận được giá trị $\phi{N} = \frac{e_0 \cdot d_0 - 1}{k}$ (với $\phi{N} \in \mathbb N$)

Khi đã tìm được ra $\phi{N}$ thì việc giải mã $d_{victim}$ là chuyện đơn giản

**2. BLINDING**

Marvin muốn Bob ký vào một $message(M)$. Nhưng Bob từ chối, vây nên Marvin cố gắng tìm ra một số r sao cho $gcd(N, r) = 1$ 

Khi này $message(M)$ được mã hoá thành $message(M')$ với $M' \equiv M*r^{e} \pmod{N}$

Bob ký vào $message(M')$ và khi này có được chữ ký $S' = M'^{d} \pmod{N}$ của Bob bằng cách lấy $S = \frac{S'}{r} \pmod{N}$

$\Rightarrow S^{e} = \frac{S'^{e}}{r^{e}} = \frac{M'^{ed}}{r^{e}} \equiv \frac{M'}{r^{e}} = M \pmod{N}$

Kỹ thuật trên được Marvin sử dụng, đã giúp tìm ra được chữ ký S của Bob.

Ta có challenge như sau:

```python
from Crypto.PublicKey import RSA
from Crypto.Random import get_random_bytes
from Crypto.Util.number import getPrime, inverse, bytes_to_long, long_to_bytes

# Tạo một khóa RSA
def khoa_rsa(bits=1024):
    key = RSA.generate(bits)
    n = key.n
    e = key.e
    d = key.d
    return key, n, e, d

# Tạo challenge với blinding attack
def rsa_blinding(n, e, message):
    # Tạo một giá trị ngẫu nhiên r sao cho gcd(r, n) = 1
    r = get_random_bytes(16)  # Lấy một chuỗi ngẫu nhiên
    r = bytes_to_long(r) % n  # Chuyển nó thành số nguyên module n

    while gcd(r, n) != 1:  # Nếu r không nguyên tố cùng nhau với n, thử lại
        r = get_random_bytes(16)
        r = bytes_to_long(r) % n
    # 2. Mã hóa thông điệp đã được "blinded"
    m = bytes_to_long(message)  # Chuyển thông điệp thành số nguyên
    m' = (m * pow(r, e, n)) % n  # Làm mờ thông điệp

    # 3. Chữ ký của nạn nhân lên tin nhắn giả
    S' = pow(m', d, n)

    # Trả về challenge
    return r, S'
# Tạo khóa RSA
key, n, e, d = khoa_rsa()

# Thông điệp ban đầu
m = 429675711022679059865832445382534783564488586109
# Tạo challenge
r, S' = rsa_blinding(n, e, message)
print(f"thông điệp giả được mã hoá : {S'}")
```
Và sau đây là cách giải:

```python
def rsa_blinding_attack(n, e, d, r, ciphertext):
    r' = pow(r, e, n) # tính r^e module N
    r'' = inverse(r, n)  # Tính nghịch đảo của r^e module n
    S'' = pow(S', e, n)
    message = (S'' * r'') % n 
    return long_to_bytes(message)  # Chuyển lại thành bytes
```
Thông điệp gốc: b"{KCSC{Un533n_m3sS4g3}

III. Low Private Exponent (Số mũ riêng d nhỏ)
**WEINER ATTACK**

Cho N = $p \cdot q$

Điều kiện của bài toán:

$d < \frac{1}{3} \cdot N^{\frac{1}{4}}$

$q < p < 2q$

$e' < N^{\frac{3}{2}}$ với $e' = e \pmod{N}$ (e' là một số không quá lớn)

Với một cặp khoá công khai (N, e), ta có thể tìm ra d và phá vỡ hệ thống mã hoá

**CHỨNG MINH**

Phép chứng minh sử dụng tính chất liên phân số. 

Ta có: $e \cdot d \equiv 1 \pmod{\phi(N)}$

Tồn tại $k$ là một số thoả $e \cdot d - k \cdot \phi(N) = 1$. Suy ra:

$|\frac{e}{\phi(N)} - \frac{k}{d}| = \frac{1}{d \cdot \phi(N)}$

Từ đây, $\frac{k}{d} \approx \frac{e}{\phi(N)}$, vì $|N - \phi(N)| < 3 \cdot \sqrt(N)$ (do $p + q = n - \phi(N) + 1$ và $p + q - 1 < 3 \sqrt{N}$)  ta có $\phi(N) \approx N$, thay $\phi(N)$ bằng N, ta được:

$|\frac{e}{N} - \frac{k}{d}| = |\frac {e \cdot d - k \cdot N - k \cdot \phi(N) + k \cdot \phi(N)}{ N \cdot d}| = |\frac{1 - k \cdot (N - \phi(N)}{N \cdot d}|$

Khi đó:

**$\Rightarrow |\frac{e}{N} - \frac{k}{d}| <= | \frac {3k \cdot \sqrt{N}}{N \cdot d} | = \frac{3k}{ d \sqrt{N}} <= \frac{1}{d N^{\frac{1}{4}}} < \frac{1}{2d^{2}}$**

Từ đó, áp dụng định lý về dãy hội tụ liên phân số, ta tìm trong dãy hội tụ của khai triển liên phân số $\frac{e}{n}$ sẽ tìm được $\frac{k}{d}$. 

Và vì $ed - k\phi(N) \equiv 1$ nên $gcd(k, d) = 1$ chứng tỏ phân số $\frac{k}{d}$ là một phân số tối giản, và ta có thể dễ dàng tìm được d 

Weiner đưa ra cách chống lại phương pháp tấn công trên sử dụng 2 cách:

**Sử dụng e lớn**: Thay e bằng e' với $e' = e + k\phi(N)$ sao cho k là số nguyên đủ lớn để $e' > n^{\frac{3}{2}}$ 

**Dùng định lý dư Trung Quốc(CRT)**: Chọn hai số nhỏ $d_p$ và $d_q$ lần lượt đồng dư theo module (p - 1) và (q - 1), và $d$ là một số lớn

Quá trình giải mã có thể diễn ra như sau: ta tính $m_p = C^{d_p} \pmod {p}$ và $m_q = C^{d_q} \pmod {q}$, dùng CRT để tính m với m = $m_p \pmod{p}$ và $m_q \pmod{q}$

Với m tìm được thì sẽ thoả $m = c^{d} \pmod{N}$ và vì vậy với d lớn thì lúc này Weiner Attack không còn hiệu quả.

IV. Low Public Exponent (Số mũ công khai e nhỏ)

ĐỐI VỚI PHẦN NÀY, PHƯƠNG PHÁP MẠNH MẼ NHẤT ĐỂ GIẢI QUYẾT SẼ LÀ ĐỊNH LÝ COPPERSMITH TUY NHIÊN TRONG CRYPTOHACK: RSA CHALLENGE CHƯA ĐỀ CẬP ĐẾN NÊN TA SẼ TÌM HIỂU VỀ MỘT TRONG SỐ NHỮNG ỨNG DỤNG ĐẦU TIÊN CỦA NÓ.

**HASTAD'S BROADCAST ATTACK**

Với số mũ công khai là nhỏ và ta cần ít nhất biết được giá trị của các khoá $(N_i, e_i)$ với cùng một số mũ e, và N đôi một nguyên tố với nhau và cùng với những thông điệp M tương ứng của 1 giá trị m duy nhất:

GIẢ SỬ: e = 3 do đó M = m^3 và ta phải giải hệ phương trình:

$c_1 = M^3 \pmod{n_1}$

$c_2 = M^3 \pmod{n_2}$

$c_3 = M^3 \pmod{n_3}$

CÁC BƯƠC THỰC HIỆN BÀI TOÁN ĐỊNH LÝ DƯ TRUNG QUỐC

Bước đầu tiên: ta lấy số dư $c_i$ của các phép toán M $\pmod{n_i}$ (VỚI $c_i$ = $M^{3} \pmod{n_i}$) 

Tiếp theo: tìm Modulus $N_i$ = $\frac{N}{n_i}$ (với N là tích của i phần tử $n_i$)

Từ đó: với $N_i$ tương ứng với $M_i$ là nghịch đảo Modulus của $N_i \pmod{n_i}$

Và ta có thể tính $M = (\sum_{i = 1}^{e} r_i * N_i * M_i \pmod{N}$

VỚI MỖI LẦN THỰC HIỆN TA LÀM VỚI BỘ 3 SỐ ĐỂ THOẢ ĐIỀU KIỆN BAN ĐẦU

Khi đó $m < n_i$ nên $m^{3} < N$ tìm m là thông điệp gốc ta chỉ cần lấy $\sqrt[3]{M}$

Định lý Håstad nhấn mạnh rằng việc sử dụng cùng một thông điệp với số mũ công khai nhỏ e trong RSA là không an toàn. Để ngăn chặn tấn công này:

Sử dụng padding ngẫu nhiên (chẳng hạn như OAEP) để đảm bảo mỗi ciphertext là duy nhất.

Tránh sử dụng số mũ công khai nhỏ khi truyền thông điệp giống nhau cho nhiều người nhận.




**VỪA RỒI LÀ TÓM TẮT TOÀN BỘ LÝ THUYẾT VỀ RSA HỌC QUA 20 BÀI ĐẦU TIÊN CỦA RSA CHALLENGE THEO CÁCH HIỂU CỦA BẢN THÂN**.





