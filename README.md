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

N = $p \cdot q$ (p, q là hai số nguyên tố) 

Sau đó tính: phi(N) = $(p - 1) \cdot (q - 1)$ và chọn một số e ( 1 < e < $\phi(N)$ ) sao cho e và $\phi(N)$ là hai số nguyên tố cùng nhau

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
### I. Factoring Large Integers (Phân tích các số nguyên lớn)

Phương thức tiếp cận đầu tiên và trực diện nhất là phân tích N thành các thừa số nguyên tố, đây là lối tấn công đầu tiên đơn giản và trực diện nhất với việc ta đã biết trước số tự nhiên e. 

Như được biết bên trên, N được tạo thành từ 2 số nguyên tố lớn vì vậy nên ta có thể sử dụng các tool như Factordb để có thể phân tích N trực tiếp. Ngoài ra còn có thể sử dụng các thuật toán phân tích số nguyên tố: Các thuật toán như thuật toán chia thử, thuật toán rho của Pollard, thuật toán số sàng,...

Với một số trường hợp khi N không dễ phân tích thì ta sẽ có những hướng tiếp cận khác, tuy nhiên, việc phân tích module N vẫn là một bước thiết yếu và quan trọng trong việc giải các bài toán phức tạp hơn.

### II. Elementary Attacks

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
    m = 7208762213778109154890632829935510436952992780502376829

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
from sympy import gcd
from Crypto.Util.number import long_to_bytes

# Hàm mở rộng Euclid để tìm hệ số u và v
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x, y = extended_gcd(b, a % b)
# Đây là phần đệ quy của thuật toán. Lúc đầu, ta bắt đầu với hai số a và b. Sau đó thuật toán đệ quy tính Gcd(b, a mod b)
    return g, y, x - (a // b) * y
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
n = 79945526004734811953572024086926772735950893553855946432038546320173776660115913045585212354489028515035466898302683334411386409202119127568258098285725628950850142094860126995395094419447180420550429241096361927675436800953059723215887958005128129540840828522080412834795778285146152822823787541788405928137
e1 = 3
e2 = 5
c1 = 374612358529533015605568557411909428293011874085637778130758716365190839543803654771881131465722131647814783251277739686429354547680420678242863104547681126835570789
c2 = 19467200470954385828008450982346974286417659821340317392603334245524978816550453408513764650068590612874151577620383416679737706319067432818858551589728604635837689506643307371846700467589773646565395492561696503369798994806420872800402978229207702427383182110186817921515149

# Giải bài toán
m = rsa_elementary_attack(n, e1, e2, c1, c2)

# In thông điệp khôi phục
print("Thông điệp gốc:", (m))
```
**Thông điệp gốc: b'KCSC{3xt3rn4l_4tt4ack!}'**

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
from Crypto.Util.number import bytes_to_long, long_to_bytes
from math import gcd

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

    # Làm giả thông điệp
    m = bytes_to_long(message)  # Chuyển thông điệp thành số nguyên
    m_faked = (m * pow(r, e, n)) % n  # Làm mờ thông điệp

    # Chữ ký của nạn nhân lên tin nhắn giả
    S_faked = pow(m_faked, d, n)

    # Trả về challenge
    return r, S_faked

# Tạo khóa RSA
key, n, e, d = khoa_rsa()
# Tạo challenge
r, S_faked = rsa_blinding(n, e, message)

# In kết quả
print(f"Thông điệp giả được mã hóa (chữ ký giả): {S_faked}")
print(f"Giá trị của r: {r}")
print(f"Modulus n: {n}")
print(f"Public exponent e: {e}")
print(f"Private exponent d: {d}")


```
Và sau đây là cách giải:

```python
from Crypto.Util.number import inverse, long_to_bytes

n = 131669885485087509228893061402339628693586379136963554539806193434671941205155723765182710736556099170713876045741375046514308270961200092631052742728133126940719637083617931899825906193392150890695200829242075330007631271307978213320167308758191948835443842001092856702411114337308858426633806397544211045829
e = 65537
r = 262608834238963214120698582043409572984
S_faked = 103136974452834977362039354776043205943084050903316978666520689639000977507631988702493560773199205398328800429496200862207523268670065566980764528540884850141621921965289818046268258775628874013752215448555411069745107638327749922367293589053239275696993384029146117589261158677374277808646557398147601257409
d = 21256197086107478945354358448460461595406318434915763721731991493947375344470261950282025109369722892810974084317923432444594374273608755054954270382587671677818039284336270016563352173464348309156343479126003765980705946233347793898910965375857470017904901395299350881766553283692260114732434420744329260033


def rsa_blinding_attack(n, e, d, r):
    r_expo = pow(r, e, n) # tính r^e module N
    r_inverse = inverse(r_expo, n)  # Tính nghịch đảo của r^e module n
    S_blinded = pow(S_faked, e, n)
    message = (S_blinded * r_inverse) % n 
    return long_to_bytes(message)  # Chuyển lại thành bytes
m = rsa_blinding_attack(n, e, d, r)
print(m)

```
**Thông điệp gốc: b'KCSC{Un533n_m3sS4g3}'**

### III. Low Private Exponent (Số mũ riêng d nhỏ)

**WIENER ATTACK**

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

Ta có challenge như sau:

```python

from Crypto.PublicKey import RSA
from Crypto.Util.number import getPrime, inverse, bytes_to_long
import random

# Tạo một khóa RSA với khóa riêng nhỏ d
def khoa_rsa(bits=1024):
    # Tạo hai số nguyên tố ngẫu nhiên p và q
    p = getPrime(bits // 2)
    q = getPrime(bits // 2)
    
    # Tính n và phi(n)
    n = p * q
    phi_n = (p - 1) * (q - 1)
    e = 65537
    d = inverse(e, phi_n)
    # Đảm bảo d đủ nhỏ (nếu không sẽ không hợp lệ cho Weiner attack)
    if d < (1/3)* (n ** (1/4)):
    # Trả về khóa công khai (e, n) và khóa riêng (d)
        return (e, n, d)

# Tạo challenge với khóa RSA có d nhỏ
def rsa_challenge():
    e, n, d = khoa_rsa(bits=1024)
    
    # In ra khóa công khai để sử dụng trong challenge
    print(f"Khóa công khai: (e={e}, n={n})")
    
    # Tạo thông điệp 
    m = 30961397952818271268472563258576978866974420779321329871889130621
    print(f"Thông điệp: {m}")
    return e, n, d, m

# Tạo challenge và in ra khóa công khai
e, n, d, m = rsa_challenge()

# Tạo challenge
print(f"\n=== CHALLENGE ===")
print(f"Khóa công khai (e, n): ({e}, {n})")
print(f"Thông điệp: {m}")

```
Ta có bài giải bằng hai cách như sau:

**SỬ DỤNG THUẬT TOÁN THEO LÝ THUYẾT**
```python
from sympy import continued_fraction, continued_fraction_convergents, Rational
from Crypto.Util.number import long_to_bytes
def wiener_attack(e, n):
    # Tính phân số liên tục của e/n
    cf = continued_fraction(Rational(e, n))
    convergents = continued_fraction_convergents(cf)
    for frac in convergents:
        k, d = frac.numerator, frac.denominator
        if k == 0 or d == 0:
            continue
```
Check xem tử hoặc mẫu có bằng 0 hay không, nếu có thì không hợp lệ vì $d \!= 0$ và nếu k = 0, thì thuật toán không thể tiếp tục

```python
# Kiểm tra tính hợp lệ của d
        if (e * d - 1) % k == 0:
            phi_n = (e * d - 1) // k
            # Tính thử p, q
            b = n - phi_n + 1    #b = p + q
            delta = b * b - 4 * n
            if delta >= 0:
                sqrt_delta = int(delta**0.5)
                if sqrt_delta * sqrt_delta == delta:
                    p = (b + sqrt_delta) // 2
                    q = (b - sqrt_delta) // 2
                    if p * q == n:
                        return d  # Trả về d nếu hợp lệ
    return None
```
Đây là một phần quan trọng của thuật toán bởi lẽ ta phải kiểm tra điều kiện của d: Vì $\phi{N}$ là một số nguyên nên khi $(e \cdot d - 1)$ % k nó phải = 0

Một bước kĩ càng hơn để kiểm tra tính đúng đắn của thuật toán và tính hợp lệ của d:

Ta sẽ kiểm tra nếu: $p \cdot q = n$.

Dựa vào công thức tính hàm phi Euler trong RSA, ta có: $\phi{N} = (p - 1) \cdot (q - 1) = p \cdot q - (p + q) + 1$ hay $\phi{N} = N - (p + q) + 1$ HAY $(p + q) = N - \phi{N} + 1$

Theo định lý Viete đảo, khi này ta sẽ có một phương trình bậc 2: $X^{2} - b \cdot x + N$ (với $b = p + q$ và $N = p \cdot q$)

Và việc giải phương trình bậc 2 trên sử dụng delta = $b^{2} - 4 \cdot (1) \cdot (N)$ với p, q là hai nghiệm $x_1$ và $x_2$ của phương trình đó.

Việc còn lại là tính N bằng p và q vừa tìm, nếu như chúng thoả toàn bộ điều kiện nghĩa là d chính xác.
```python
n = 100300337667125211384814961498385747916262207042393747307781775041841729627991753813025308949465695213432515654049618772493432046900250749616497500259572352487923195374326860250635928985679574649936822337361350162334899116009299616578468148993741745022128162404908020544314122221881153379456741853336581541683
e = 65537
d_found = wiener_attack(e, n)
if d_found:
    print(f"Khóa riêng d tìm thấy: {d_found}")
    print(f"Khóa riêng thực tế: {d}")

    # Giải mã thông điệp
    c = pow(m, e, n)  # Mã hóa thông điệp
    m = pow(c, d_found, n)  # Giải mã thông điệp
    print(f"Thông điệp sau giải mã: {m_decrypted}")
else:
    print("Không thể tìm thấy khóa riêng d.")
```
**Hoặc ta có thể sử dụng thư viện Owiener như một tool để giải nhanh hơn**
```python
import owiener
from Crypto.Util.number import bytes_to_long
n = 83683933725963697774642197417455080110119746479900456011241186890282779653911901207907421456724308436788981656329779711794412935691627010837168641321633118559815642375093711414267116664873546099584844994790061438286731227165312963077819598764901582045554060551675181453089310394700533016708464974967378662049
e = 65537
d_found = owiener.attack(e, n)
if d_found:
    print(f"Khóa riêng d tìm được: {d_found}")
    print(f"Khóa riêng d thực tế: {d_actual}")
    print(f"Khóa tìm được {'đúng' if d_found == d_actual else 'sai'}!")
else:
    print("Không thể tìm thấy d bằng Wiener Attack.")
```
**Thông điệp gốc: b'KCSC{5m4LL_w31n3r_5m4LL_xd}'**

Weiner đưa ra cách chống lại phương pháp tấn công trên sử dụng 2 cách:

**Sử dụng e lớn**: Thay e bằng e' với $e' = e + k\phi(N)$ sao cho k là số nguyên đủ lớn để $e' > n^{\frac{3}{2}}$ 

**Dùng định lý dư Trung Quốc(CRT)**: Chọn hai số nhỏ $d_p$ và $d_q$ lần lượt đồng dư theo module (p - 1) và (q - 1), và $d$ là một số lớn

Quá trình giải mã có thể diễn ra như sau: ta tính $m_p = C^{d_p} \pmod {p}$ và $m_q = C^{d_q} \pmod {q}$, dùng CRT để tính m với m = $m_p \pmod{p}$ và $m_q \pmod{q}$

Với m tìm được thì sẽ thoả $m = c^{d} \pmod{N}$ và vì vậy với d lớn thì lúc này Weiner Attack không còn hiệu quả.

### IV. Low Public Exponent (Số mũ công khai e nhỏ)

ĐỐI VỚI PHẦN NÀY, PHƯƠNG PHÁP MẠNH MẼ NHẤT ĐỂ GIẢI QUYẾT SẼ LÀ ĐỊNH LÝ COPPERSMITH TUY NHIÊN TRONG CRYPTOHACK: RSA CHALLENGE CHƯA ĐỀ CẬP ĐẾN NÊN TA SẼ TÌM HIỂU VỀ MỘT TRONG SỐ NHỮNG ỨNG DỤNG ĐẦU TIÊN CỦA NÓ.

**HASTAD'S BROADCAST ATTACK**

Với số mũ công khai là nhỏ và ta cần ít nhất biết được giá trị của các khoá $(N_i, e_i)$ với cùng một số mũ e, và N đôi một nguyên tố với nhau và cùng với những thông điệp M tương ứng của 1 giá trị m duy nhất:

GIẢ SỬ: e = 3 do đó M = m^3 và ta phải giải hệ phương trình:

$c_1 = m^3 \pmod{n_1}$

$c_2 = m^3 \pmod{n_2}$

$c_3 = m^3 \pmod{n_3}$

CÁC BƯỚC THỰC HIỆN BÀI TOÁN ĐỊNH LÝ DƯ TRUNG QUỐC

Bước đầu tiên: ta lấy số dư $c_i$ của các phép toán M $\pmod{n_i}$ (VỚI $c_i$ = $M^{3} \pmod{n_i}$) 

Tiếp theo: tìm Modulus $N_i$ = $\frac{N}{n_i}$ (với N là tích của i phần tử $n_i$)

Từ đó: với $N_i$ tương ứng với $M_i$ là nghịch đảo Modulus của $N_i \pmod{n_i}$

Và ta có thể tính $M = (\sum_{i = 1}^{e} r_i * N_i * M_i \pmod{N}$

VỚI MỖI LẦN THỰC HIỆN TA LÀM VỚI BỘ 3 SỐ ĐỂ THOẢ ĐIỀU KIỆN BAN ĐẦU

Khi đó $m < n_i$ nên $m^{3} < N$ tìm m là thông điệp gốc ta chỉ cần lấy $\sqrt[3]{M}$

Định lý Håstad nhấn mạnh rằng việc sử dụng cùng một thông điệp với số mũ công khai nhỏ e trong RSA là không an toàn. Để ngăn chặn tấn công này:

Sử dụng padding ngẫu nhiên (chẳng hạn như OAEP) để đảm bảo mỗi ciphertext là duy nhất.

Tránh sử dụng số mũ công khai nhỏ khi truyền thông điệp giống nhau cho nhiều người nhận.

### V. Implementation Attacks

**1. Fermat Attacks**

Fermat Attacks là một phương pháp khai thác RSA khi hai số nguyên tố $p$ và $q$ của Module $N = p \cdot q$ có số bit bằng nhau, nhưng có giá trị gần nhau $|p - q| < \sqrt[4]{N}$

Ta có : $N = p \cdot q = (\frac{p + q}{2})^{2} - (\frac{p - q}{2})^{2} = a^{2} - b^{2} = (a - b) \cdot (a + b)$

Khi này ta dặt: $p = (a + b)$ và $q = (a - b)$ và $b^{2} = a^{2} - N$

Và nhiệm vụ của thuật toán là tìm a để $a^{2} - n$ là một số chính phương thì $b^{2}$ cũng là một số chính phương

Khi này thì việc tìm p, q là dễ dàng và thuật toán dừng lại khi $p \cdot q = N$

Về mặt toán học thì: với $b = \frac{p - q}{2}$ thì b sẽ rất nhỏ vì p và q thực tế có giá trị gần nhau, tức là $a \approx \sqrt{N}$. 
                     Vì $b$ nhỏ, phép tính $b^{2} = a^{2} - N$ sẽ nhanh chóng dấn đến một số chính phương

**2. Timing Attacks(Side-channel Attacks)**                     

RSA Timing Attack là một loại side-channel attack, dựa trên việc đo lường thời gian mà một hệ thống sử dụng để thực hiện các phép toán mật mã, cụ thể là việc giải mã RSA hoặc ký số RSA. Tấn công này tận dụng các khác biệt về thời gian thực thi, do chúng tiết lộ thông tin về các bit của khóa riêng tư $d$ trong thuật toán RSA.

**Ý Tưởng Cơ Bản Của Timing Attack**

Giả sử: bạn sở hữu một bộ khoá RSA$(N, e, d)$

Khi bạn giải mã một thông tin: $m = C^{d} \pmod{N}$ hay tạo chữ ký: $s = m^{d} \pmod{N}$, ta đều phải dùng tới khoá riêng tư $d$. 

Trong RSA, thuật toán lũy thừa module (modular exponentiation) thường sử dụng các phương pháp như square-and-multiply. Trong phương pháp này:

Nếu một bit trong $d$ bằng 1, thì cần thực hiện phép nhân.

Nếu một bit trong $d$ bằng 0, thì không cần thực hiện phép nhân.

Điều này dẫn đến việc thời gian thực hiện phụ thuộc vào giá trị cụ thể của các bit trong $d$.

Kẻ tấn công sẽ: đo thời gian thực thi của từng ciphertext $C_i$, khi hệ thống thực hiện giải mã.

Nếu thời gian giải mã dài hơn, điều đó gợi ý rằng bit hiện tại của $d$ có thể là 1.

Nếu thời gian giải mã ngắn hơn, điều đó gợi ý rằng bit hiện tại của $d$ có thể là 0.

Sau khi thu thập đủ số lượng ciphertext và đo thời gian tương ứng, kẻ tấn công có thể ghép các bit đã suy luận được để xây dựng lại khóa $d$, khóa được hoàn thiện dựa trên các bit từ cao đến thấp, cho đến khi đủ toàn bộ giá trị $d$.

Vậy thì làm sao kẻ tấn công có thể tính được khoảng thời gian chênh lệch giữa các lần thực hiện phép toán và đoán được giá trị của $d$, đó là dựa vào thuật toán "Repeated Squaring"

Khi ta giải mã một ciphertext hoặc ký lên một thông điệp: $C = m^{d} \pmod{N}$

Việc tính toán trực tiếp $m^{d}$ là một công việc tốn nhiều tài nguyên

Vậy nên ta sẽ viết $d$ dưới dạng thập phân: $d = 2^{n} \cdot d_n + 2^{n - 1} \cdot d_{n - 1} +...+ 2^{1} \cdot d_1 + 2^{0} \cdot d_0$.

Và viết dưới dạng tổng quát sẽ là: **$d = \sum_{i = 0}^{n} 2^{i} \cdot d_i$**.

Khi đó $C = m^{\sum_{i = 0}^{n} 2^{i} \cdot d_i} \rightarrow C = \prod_{i = 0}^{n} m^{2^{i} \cdot d_i}$ 

Ta đặt: $z = m$ và $C$ = 1

Vậy để tính $C$ ta thực hiện thuật toán sau:

Với mỗi bit $d_0$ thấp nhất tới bit cao nhất:

Nếu $d_i = 1: C = C \cdot z \pmod{N}$

Và bình phương $z: z = z^{2} \pmod{N}$

Nếu $d_i = 0$ ta không thực hiện phép nhân $C \cdot z$ tuy nhiên vẫn thực hiện phép bình phương $z$ theo module N

Giải thích: 

Tại sao cập nhật biến $C = C \cdot z$, bởi lẽ khi $d_i = 1$ thì ta cần kết hợp giá trị $m^{2^{i}}$ được lưu trữ trong z vào kết quả cuối cùng.

Tại sao phải bình phương $z$ theo module N, đó là để chuẩn bị cho phép tính $m^{2^{i + 1}} \pmod{N}$ ở bước tiếp theo.

Và tại $d_i = 0$ thì ta không thực hiện phếp tính vì khi này giá trị $m^{2{i}}$ không làm ảnh hưởng tới kết quả phép tính.

Vì vậy nên khi bit $d_i = 1$, nó phải thực hiện phép nhân bổ sung nên sẽ tạo ra sự chênh lệch trong thời gian thực hiện phép tính còn khi $d_i = 0$ không cần thực hiện phép nhân bổ sung, vì vậy nên thời gian thực hiện sẽ diễn ra ngắn hơn! 

Sau khi thực hiện tính mỗi phép nhân xong, ta so sánh $t_1$ và $T_1$

Với:

$t_1$: Thời gian dự đoán của từng thông điệp $m_i$, giả định $d_i$ = 1

$T_1$: Thời gian thực tế của hệ thống mật mã giải mã

Nếu $d_i = 1$, hệ thống mật mã thực hiện phép nhân $C \cdot z$ khiến cho $T_1$ bị ảnh hưởng theo $t_1$. Vậy nên giữa $t_1$ và $T_1$ có mối tương quan cao, mô phỏng theo chính xác những gì hệ thống mật mã đang thực hiện.

Nếu $d_i = 0$, $t_1$ và $T_1$ độc lập với nhau và không có sự tương quan so sánh rõ ràng.

Khi so sánh $t_1$ và $T_1$ hoàn tất ta sẽ suy ra được từng bit của d và tìm ra được d!

Ở đây chúng ta có đặt ra hai câu hỏi.

**1. Tại sao đổi với Timing Attacks ta cần thử với nhiều ciphertext khác nhau ?**: Chính là vì hệ thống mã hoá không giới hạn số lần giải mã, đồng thời việc thực hiện nhiều giải mã hơn thì chênh lệch thời gian của từng bit sẽ được xác định kỹ càng hơn từ đó có thể đưa ra một số d chính xác hơn.

**2. Ta nên chọn thông điệp(message) như thế nào ?**:

Thứ nhất là chọn $m < N$.

Thứ hai là kiểm tra các đặc điểm của $m$ và thử nghiệm với các thông điệp có cấu trúc đặc biệt (ví dụ như các thông điệp có giá trị bit 1 tại các vị trí quan trọng trong khóa $d$.

Thử ba là chọn $m$ sao cho giá trị $m^{d} \pmod{N}$ tạo ra các khác biệt rõ rệt trong các phép toán (chẳng hạn như chọn các giá trị $m$ sao cho khi thực hiện phép toán mũ sẽ khiến một số bit của $d$ thay đổi dễ dàng hơn).

Ta có challenge như sau:

```python
from Crypto.PublicKey import RSA
from Crypto.Util.number import getPrime, inverse
import random

# Tạo một khóa RSA với khóa riêng nhỏ d
def khoa_rsa(bits=1024):
    p = getPrime(bits // 2)
    q = getPrime(bits // 2)
    
    # Tính n và phi(n)
    n = p * q
    phi_n = (p - 1) * (q - 1)
    e = 65537  # Giá trị thường được chọn cho e
    d = inverse(e, phi_n)
    
    # Đảm bảo d đủ nhỏ (nếu không sẽ không hợp lệ cho Weiner attack)
    if d >= n // 2:
        d = random.randint(1, n // 2)  # Giới hạn d nhỏ hơn n/2
    
    # Trả về khóa công khai (e, n) và khóa riêng (d)
    return (e, n, d)

# Tạo challenge với khóa RSA có d nhỏ
def rsa_challenge():
    e, n, d = khoa_rsa(bits=1024)
    
    # In ra khóa công khai để sử dụng trong challenge
    print(f"Khóa công khai: (e={e}, n={n})")
    
    # Tạo thông điệp ngẫu nhiên 
    ciphertext = [340567812360240512137833226672357117084722024613943503125843, 30961397952818271268472563258576978866974420779321329871889130621, 22634328374632864873648378563856438764385847356384]
    print(f"Thông điệp: {m}")
    return e, n, d, m

# Tạo challenge và in ra khóa công khai
e, n, d, m = rsa_challenge()

# Tạo challenge
print(f"\n=== CHALLENGE ===")
print(f"Khóa công khai (e, n): ({e}, {n})")
print(f"Thông điệp cần giải mã: {ciphertext}")
```
Bài giải cho challenge phía trên:
```python
import time
from Crypto.Util.number import long_to_bytes
# Tạo hàm tính C, thuật toán "Repeated Squaring"
def repeated_squaring(m, d, n):
    C = 1
    z = m
    while d:
        d >>= 1
        if d & 1:
            C = (C * z) % n
        z = (z * z) % n
    return C
# Tạo hàm tính thời gian giải mã ciphertext và trả về thời gian thực thi
def RSA_decrypt(ciphertext, d, n):
    start = time.time()
    plaintext = repeated_squaring(ciphertext, d, n)
    end = time.time()
    return end - start
```
Tại đây, sử dụng binary search để tìm d

Giả sử khoá d nằm trong khoảng [0; n - 1] (lower_bound, upper_bound)

Sử dụng binary search để thu hẹp khoảng giá trị của d dựa trên thời gian giải mã ciphertext

Nếu thời gian giải mã > 0.1s, hàm sẽ giảm upper_bound, còn nếu < 0.1s thì hàm tăng lower_bound, quá trình này nhằm khôi phục khoá riêng d
```python

def Kocher_Timing_Attack(ciphertext, n):
    lower_bound = 0
    upper_bound = n - 1
    while lower_bound <= upper_bound:
        d = (lower_bound + upper_bound) // 2
        time_elapsed = RSA_decrypt(ciphertext, d, n)
        # Phân tích thời gian trung bình để điều chỉnh upper_bound và lower_bound
        if avg_time > 0.1:
            upper_bound = d - 1
        elif avg_time < 0.1:
            lower_bound = d + 1
        else:
            return d  # Dừng lại khi tìm thấy d chính xác
# Ví dụ sử dụng:
n = 68591886373406102359935093451287663915214276998250804541059603814667838778649011715751231500875696597801497992611102354043220100576401835908053647949285396723536370551747427416369282636138603890883481866058750574927543233949147205279935298849065837404418400736910298342425733328624431620925159173947444517913
e = 65537
ciphertext = 340567812360240512137833226672357117084722024613943503125843

# Tấn công để khôi phục khóa riêng d
private_key = Kocher_Timing_Attack(ciphertext, n)
print("The private key is:", private_key)

# Kiểm tra tính đúng đắn của khóa riêng
plaintext = repeated_squaring(ciphertext, private_key, n)
print("The plaintext is:", plaintext)
print(long_to_bytes(plaintext))  # Hiển thị bản rõ dưới dạng byte
```




**VỪA RỒI LÀ TÓM TẮT TOÀN BỘ LÝ THUYẾT VỀ RSA HỌC QUA 20 BÀI ĐẦU TIÊN CỦA RSA CHALLENGE THEO CÁCH HIỂU CỦA BẢN THÂN**.





