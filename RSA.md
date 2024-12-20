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
n = 100300337667125211384814961498385747916262207042393747307781775041841729627991753813025308949465695213432515654049618772493432046900250749616497500259572352487923195374326860250635928985679574649936822337361350162334899116009299616578468148993741745022128162404908020544314122221881153379456741853336581541683
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

Mô tả thuật toán Định lý dư Trung Quốc và Ứng dụng vào Hastad's Broadcast Attacks:

```python
from sympy import mod_inverse, integer_nthroot

# Dữ liệu từ bài toán
N1 = 15184735897616140463
N2 = 15734180812753971107
N3 = 14730353456447021839
C1 = 1881365963625
C2 = 1881365963625
C3 = 1881365963625
e = 3

# Hàm CRT (Chinese Remainder Theorem)
def CRT(remainders, module):
    M = 1
    for mod in module:
        M *= mod
    result = 0
    for i in range(len(module)):
        Mi = M // module[i]
        yi = mod_inverse(Mi, module[i])  # Tính nghịch đảo modular
        result += remainders[i] * Mi * yi
    return result % M

# Áp dụng CRT
module = [N1, N2, N3]
ciphertexts = [C1, C2, C3]
combined = CRT(ciphertexts, module)

# Tìm căn bậc ba nguyên của kết quả từ CRT
m, is_perfect = integer_nthroot(combined, e)
if is_perfect:
    print(f"Thông điệp đã giải mã: {m}")
else:
    print("Không tìm thấy căn bậc ba nguyên!")
```
**m = 12345**

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
Trên thực tế, ta không nên sử dụng Kocher Timing Attack, vì nó có thể rất chậm và không đáng tin cậy, đồng thời có nhiều cách an toàn và hiệu quả hơn để thực hiện mã hóa và giải mã RSA. 
Thay vào đó, ta nên sử dụng các thư viện RSA đã được thiết kế có khả năng chống Side-channel Attack, chẳng hạn như OpenSSL hoặc PyCryptodome.

**3. Random Faults**: Lỗ Hổng do Sử Dụng Định Lý Trung Quốc (CRT)

Random faults trong RSA thường được hiểu là những lỗi ngẫu nhiên, xảy ra trong quá trình tính toán hoặc truyền tải thông tin trong hệ thống RSA, có thể dẫn đến các yếu tố thay đổi trong các bước mã hóa hoặc giải mã. Các lỗi ngẫu nhiên này có thể được tạo ra từ phần cứng, phần mềm hoặc môi trường vật lý mà hệ thống hoạt động

Những lỗi ngẫu nhiên trong quá trình tính toán có thể làm sai lệch kết quả của những phép toán này. Tấn công Random Faults là một phương pháp khai thác sự thay đổi này, nhằm làm rối loạn quá trình tính toán của RSA, từ đó phá vỡ hệ thống bảo mật.

Các kỹ thuật khai thác lỗi ngẫu nhiên này có thể được phân thành các loại chính:

Fault Injection: Đây là quá trình tiêm lỗi vào hệ thống, có thể thông qua phần mềm hoặc phần cứng (ví dụ như thay đổi các bit trong bộ nhớ hoặc tạo ra các tín hiệu nhiễu trong vi mạch). Mục đích của phương pháp này là gây ra sự thay đổi ngẫu nhiên trong quá trình mã hóa hoặc giải mã.

Differential Fault Analysis (DFA): Đây là một phương pháp tấn công sử dụng lỗi ngẫu nhiên để thu thập thông tin về khóa bí mật (private key). Kẻ tấn công sẽ gây ra lỗi trong quá trình giải mã, và qua đó có thể phân tích sự thay đổi của kết quả giải mã để rút ra thông tin về khóa riêng $d$

Khai thác lỗ hổng do CRT (Chinese Remainder Theorem) thường thuộc loại tấn công Adaptive Chosen Ciphertext Attack (CCA2). Cụ thể, trong trường hợp của Random Faults trong RSA, khi sử dụng CRT để tăng tốc độ tính toán, một số lỗi ngẫu nhiên (do sự cố phần cứng hoặc can thiệp bên ngoài) có thể dẫn đến việc tính toán sai trong quá trình ký hoặc giải mã, làm lộ khóa riêng tư.

Trong quá trình tạo chữ ký RSA sử dụng Định lý Trung Quốc (CRT) để tối ưu hóa tính toán, nếu một lỗi ngẫu nhiên xảy ra trong các phép toán, nó có thể dẫn đến một chữ ký không hợp lệ, điều này mở ra cơ hội cho tấn công từ bên ngoài. Cụ thể, giả sử, $C_p$ (chữ ký phần $p$)tính đúng, nhưng $C_q$ (chữ ký phần $q$) lại tính sai do lỗi ngẫu nhiên.

**Mô Tả Tấn Công**

Khi Bob sử dụng **CRT** để tính chữ ký trong RSA, thay vì tính trực tiếp $m^{d} \pmod{N}$ (với N là tích của hai số nguyên tố $p$ và $q$), Bob chia bài toán thành hai phần nhỏ hơn và tính theo module $p$ và $q$ như sau:

$$
\begin{cases}
    s_p &= m^{d_p} + k_1 \cdot p \\
    s_q &= m^{d_q} + k_2 \cdot q
\end{cases}
$$

Trong đó: 

$$
\begin{cases}
    d_p &= d \pmod{p - 1} \\
    d_q &= d \pmod{q - 1}
\end{cases}
$$

Khi gộp hai hệ trên vào theo **CRT**, ta sẽ có chữ ký: $s = m^{d} + k_3 \cdot p \cdot q$, bằng cách:

Nếu như không có gì sai sót trong quá trình tính toán, thì trong quá trình xác minh chữ ký $s_p$ và $s_q$ ta sẽ được như sau:

$$
\begin{cases}
    s^{e} \equiv m \pmod{p} \rightarrow s^{e} - m \equiv 0 \pmod{p} \\
    s^{e} \equiv m \pmod{q} \rightarrow s^{e} - m \equiv 0 \pmod{q}
\end{cases}
$$

$\rightarrow s^{e} - m = k_3 \cdot p \cdot q$
                 
Như vậy thì, chữ ký $s^{e}$ phù hợp với m theo module $p$ và $q$, có nghĩa là $s^{e} - m$ chính là bội số chung của $p$ và $q$, khiến cho $s^{e} - m$ cũng được tính là một bội số của module $N$.

Tức: $Gcd(s^{e} - m, N) = N$

Vậy thì, lỗi xảy ra khi chỉ cần $s_p$ hay $s_q$ lệch một vài bit thì ngay lập tức:

$$ 
            s^{e} \not\equiv m \pmod{p} \vee s^{e} \not\equiv m \pmod{q}
\Rightarrow s^{e} - m = k_3 \cdot q \vee s^{e} - m = k_3 \cdot p 
$$

Có thể hiểu, khi đó chữ ký của Bob chỉ phù hợp với m theo module $p$ hoặc $q$. Vậy nên khi đó $s^{e} - m$ là bội của $p$ hoặc $q$, điều này đồng nghĩa với việc, nếu ta tính $Gcd(s^{e} - m, N) = p \vee Gcd(s^{e} - m, N) = q$.

Từ đây ta có thể khai thác được một trong hai số nguyên tố cấu thành nên module $N$.

Tuy nhiên, phương thức tấn công này chỉ có tác dụng khi kẻ tấn công biết trước plaintext m (chưa qua Random Padding).

Sau đây là challenge:

```python
from Crypto.Util.number import getPrime, inverse, bytes_to_long
from sympy import mod_inverse
# Khởi tạo hệ thống RSA với các số nguyên tố p, q
p, q = getPrime(1024), getPrime(1024)
e = 65537
n = p * q  # modulus
flag = b'{r4nd0m_f4ult_l0l}'
d = inverse(e, (p - 1) * (q - 1))  # khóa bí mật
c = pow(bytes_to_long(flag), e, n)  # mã hóa thông điệp bằng RSA

print("n =", n)
print("e =", e)
print("c =", c)

# Định lý Trung Quốc (Chinese Remainder Theorem)
def CRT(remainders, module):
    M = 1
    for mod in module:
        M *= mod
    result = 0
    for i in range(len(module)):
        Mi = M // module[i]
        yi = mod_inverse(Mi, module[i])  # Tính nghịch đảo modular
        result += remainders[i] * Mi * yi
    return result % M

# Hàm ký
def sign(m, fault=False):
    d_p = inverse(e, p - 1)
    d_q = inverse(e, q - 1)
    s_p = pow(m, dp, p)
    s_q = pow(m, dq, q)
    
    # Nếu có lỗi ngẫu nhiên, làm sai một phần trong chữ ký
    if fault:
        s_q += 1  # Giới thiệu lỗi vào C_q
    
    s = CRT([s_p, s_q], [p, q])
    return s

# Vòng lặp nhận đầu vào và ký chữ ký có lỗi
while True:
    m = int(input("Nhập m = "))
    print("Chữ ký: ", sign(m, True))  # Chữ ký với lỗi ngẫu nhiên
```
Và sau đây là bài giải
```python
from Crypto.Util.number import GCD, inverse, long_to_bytes

# Các giá trị cần thiết (thay thế bằng giá trị từ challenge)
n = 13298815912594907741162789273924783694427810137974964685074065570038696990765058272308290777219436648571009499441775367039733443066748334515060312903284820049512007113692824230891260174629418408254875214033940930179284430333304255372970029725056730672656166578002757131220490186290364867752945521064208967063881391140364524364724173801518407379066129468674250369457241805906037952941951460110467465998029575187817251676275381101601190180674947687794286231391506621111572723056387802463361411160975223513069695639884475231118635852106800547254569480322149088832642164396173004926199658708114441911817634227224392320617
e = 65537
m = 43232543534
c_fault = 8637912216348962969690978514982310068686977432660946987016530628036777297501321811206184982493081790639741799171802822361150051531239276699146771914353878603787713587274713862050159070174002966774873947343542272889645509119277986327812409983999756778186164003842674110093128362782802918544479855013150874682735759853325574767440022421286146685321742628365943688529997004395759015878492910565637592442919870325893520513350059988206935427650243892564168209045232415100913728412819345660212613729302522707335607868786090816948863421267966763414979765894964550333751317460687027949996433347236779104289896745056708451948

# Tính C^e - m mod n
M_computed = pow(c_fault, e, n)

# Tính GCD(n, C^e - m)
factor = GCD(n, M_computed - m)

if factor > 1:
    print(f"Thừa số tìm được: {factor}")
    p = factor
    q = n // p
    print(f"p = {p}")
    print(f"q = {q}")

    # Xây dựng lại khóa bí mật
    phi = (p - 1) * (q - 1)
    d = inverse(e, phi)
    print(f"Khóa bí mật (d) = {d}")

    # Giải mã flag nếu cần
    c = 9530889903254179857834114277683181648032972928254792234079867087380240581939753035269354834250142164060984745000659971696929642299049969220184540171449399895563314103335599945485271868736726880098751108952977580443772243911572560989453490006829733057454211222115563094975858376389740376110775904859065319058247144268492914402194659004465050381536534785098903805524818247538320760070768858774477480463801728116568061455577919992389380586475318869985063460662120788763395874249527686939553973326481725189980178660586464474540374711242650002391839204036969002996006568291949126223848089821449327236325616691379563539936
    flag = pow(c, d, n)
    print("Flag:", long_to_bytes(flag))
else:
    print("Không tìm được thừa số!")
```
Biện Pháp Phòng Ngừa:

Kiểm tra chữ ký: Trước khi gửi chữ ký ra ngoài, Bob có thể kiểm tra lại chữ ký của mình để chắc chắn rằng không có lỗi ngẫu nhiên xảy ra trong quá trình tính toán.

Sử dụng padding ngẫu nhiên: Việc thêm padding ngẫu nhiên vào thông điệp trước khi ký sẽ giúp bảo vệ hệ thống khỏi việc kẻ tấn công khai thác lỗi ngẫu nhiên, vì thông điệp m sẽ thay đổi trong mỗi lần ký

**4. Bleichenbacher's Attack on PKCS#1(MSB)** 

Tấn công Bleichenbacher là một trong những tấn công nổi tiếng nhất đối với các hệ thống mã hóa RSA sử dụng chuẩn PKCS #1 (Public Key Cryptography Standards #1), đặc biệt là khi sử dụng RSA để mã hóa các thông điệp (chẳng hạn trong SSL/TLS hoặc chữ ký số). Tấn công này được phát hiện bởi Daniel Bleichenbacher vào năm 1998 và có khả năng khai thác các lỗ hổng trong việc triển khai PKCS #1 v1.5 (một tiêu chuẩn cũ cho việc mã hóa RSA).

Để hiểu tấn công này, ta cần hiểu những khái niệm sau:

1. PKCS #1 v1.5 Padding: PKCS #1 v1.5 là một kỹ thuật "padding" (đệm) được sử dụng trong quá trình mã hóa với RSA. Khi một thông điệp được mã hóa bằng RSA, nó phải có một kích thước nhỏ hơn kích thước của module $N$. Một thông điệp $M$ được mã hoá dưới dạng $C = M^{e} \pmod{N}$, với $M$ là thông điệp được đệm theo chuẩn PKCS#1 v1.5. Tuy nhiên, vấn đề là nếu không có một cơ chế xác thực mạnh mẽ, kẻ tấn công có thể tận dụng lỗ hổng trong cách mà padding được thực hiện để tấn công hệ thống.
2. Tấn Công Bleichenbacher: Tấn công Bleichenbacher là một kiểu tấn công adaptive chosen ciphertext attack (CCA2), nơi kẻ tấn công có thể "chọn" các thông điệp mã hóa và gửi chúng tới hệ thống để nhận phản hồi, từ đó suy luận ra thông tin về khóa bí mật hoặc thông điệp gốc.

**Các bước trong tấn công**:
1. Thu thập các thông điệp mã hóa: Kẻ tấn công có thể thu thập được một số thông điệp mã hóa từ hệ thống (chẳng hạn là những thông điệp đã được mã hóa trong quá trình giao tiếp).
2. Gửi các thông điệp mã hóa giả mạo: Kẻ tấn công sẽ gửi một thông điệp mã hóa giả mạo $C'$ tới hệ thống và yêu cầu kiểm tra xem thông điệp này có hợp lệ hay không.
3. Phản hồi của hệ thống: Hệ thống sẽ phản hồi cho kẻ tấn công biết liệu thông điệp giả mạo $C'$ có hợp lệ hay không. Phản hồi này sẽ cung cấp thông tin về việc liệu padding của thông điệp có đúng hay không, từ đó giúp kẻ tấn công điều chỉnh các thử nghiệm tiếp theo.
4. Dự đoán thông điệp gốc: Với mỗi phản hồi, kẻ tấn công có thể làm suy giảm không gian tìm kiếm, dẫn đến việc dần dần xác định được thông điệp gốc hoặc khóa bí mật.

**Giải thích Bleichenbacher's Attack trên PKCS#1**

Khi mã hoá một thông điệp $M$, nó được "padding" để đạt độ dài $n$ bits, bằng cách thêm:

$02 || Random Padding || 00 || M$

$\cdot$ 02: Tiền tố đặc biệt (16 bits) chỉ định rằng padding đã được thêm.

$\cdot$ Random Padding: Một chuỗi các bit ngẫu nhiên.

$\cdot$ 00: Dấu phân cách.

$\cdot M: Thông điệp thực tế.

Kẻ tấn công Marvin thực hiện như sau:

1. **Chọn ngẫu nhiên sổ r**: Marvin chọn một số $r \in \mathbb{Z_N}^*$
2. **Tạo ciphertext giả $C'$: Tính: $C' = r \cdot C \pmod{N}$
3. **Sau khi gửi $C'$ tới hệ thống, kiểm tra xem ciphertext giả có hợp lệ không dựa vào phản hồi của hệ thống, phản hồi này sẽ cho phép Marvin suy ra thông điệp về plaintext M ban đầu

Phản hồi của hệ thống:

$\cdot$ Nếu phản hồi hợp lệ: hệ thống sẽ xử lý thành công và phản hồi cho Marvin (hoặc không trả lỗi gì). Điều này giúp Marvin biết rằng $MSB_16((r \cdot M) \pmod{N}) = 0x02$. Bit cao nhất của $M' = (r \cdot N) \pmod{N}$ là hợp lệ (0x02)

$\cdot$ Nếu phản hồi không hợp lệ: Marvin sẽ thử với một giá trị $r$ khác và lặp lại quá trình.

4. Marvin sử dụng thông tin này để lặp lại quá trình cho đến khi xác định được $M$ ban đầu.

Sau đây là một hệ thống kiểm tra theo PKCS#1 v1.5 Padding:
```python

from Crypto.PublicKey import RSA
from Crypto.Util.Padding import pad
from Crypto.Cipher import PKCS1_OAEP
import random
import binascii

# Giả sử đây là thông điệp gốc
message = b'KCSC{p4dd1n9_1s_fUn!}'

# Giả sử chúng ta có một RSA key pair
key = RSA.generate(2048)
public_key = key.publickey()
private_key = key

# Mã hóa thông điệp với PKCS1
cipher = PKCS1_OAEP.new(public_key)
ciphertext = cipher.encrypt(message)

# In ra ciphertext đã mã hóa
print("Ciphertext:", binascii.hexlify(ciphertext))

# Mô phỏng hệ thống kiểm tra padding (oracle)
def check_padding(ciphertext):
    try:
        # Thử giải mã và kiểm tra padding
        decrypted_message = cipher.decrypt(ciphertext)
        if decrypted_message.startswith(b'\x02') and b'\x00' in decrypted_message:
            return True  # Padding hợp lệ
        else:
            return False  # Padding không hợp lệ
    except ValueError:
        return False  # Nếu không thể giải mã được, padding không hợp lệ
```
Để tấn công vào hệ thống kiểm tra PKCS#1 v1.5 padding như đã xây dựng trong ví dụ trên, chúng ta sẽ thực hiện một Bleichenbacher Attack. Mục tiêu của cuộc tấn công này là lợi dụng thông báo lỗi về tính hợp lệ của padding để dần dần tìm ra thông điệp ban đầu mà không cần biết khóa riêng
```python
def bleichenbacher_attack(ct, oracle):
    lb = 0
    ub = public_key.n
    for i in range(100):  # Giới hạn số lần thử
        r = random.randint(1, public_key.n)
        new_ct = (pow(r, 1, public_key.n) * ct) % public_key.n
        
        # Kiểm tra padding hợp lệ
        if oracle(new_ct):
            ub = (ub + lb) // 2
        else:
            lb = (ub + lb) // 2
        
        if (ub - lb) < 65537:
            return ub  # Trả về kết quả gần đúng

# Thực hiện tấn công
recovered_message = bleichenbacher_attack(ciphertext, check_padding)
print("Recovered Message:", recovered_message)
```
Như vậy là đã hoàn thành mô phỏng một cuộc tấn công vào PKCS #1 v1.5 với message là KCSC{p4dd1n9_1s_fUn!}

**5. Parity Oracle Attack(LSB)**

Tấn công Parity Oracle là một dạng của tấn công ciphertext đã chọn có tính thích ứng (CCA2), khai thác các hệ thống cung cấp thông tin về tính chẵn lẻ (parity) của bản rõ sau khi giải mã mà không kiểm tra tính hợp lệ của ciphertext.

Tấn công này đặc biệt nhắm đến các hệ thống mà khi giải mã, chúng trả về thông tin về tính chất (ví dụ như tính chẵn hay lẻ) của thông điệp sau giải mã. Điều này xảy ra khi các hệ thống bị lỗi trong cách xử lý hoặc xác thực thông điệp, hoặc khi hệ thống tiết lộ thông tin về tính chẵn lẻ của bản rõ mà không bảo vệ an toàn.

Các Khái Niệm Cơ Bản:

1. Parity (Tính chẵn lẻ): Trong ngữ cảnh mã hóa, "parity" đề cập đến việc một số có phải là số chẵn hay không. Khi một thông điệp được giải mã, hệ thống có thể tiết lộ xem giá trị của thông điệp (hoặc một phần của nó, ví dụ như một byte cụ thể) là chẵn hay lẻ.
2. Oracle: Trong mật mã học, "oracle" chỉ hệ thống cung cấp thông tin về quá trình mã hóa hoặc giải mã, thường giúp kẻ tấn công thao túng ciphertext. Trong tấn công Parity Oracle, oracle sẽ tiết lộ thông tin về tính chẵn lẻ của thông điệp sau khi giải mã.

Cách Thức Hoạt Động của Tấn Công Parity Oracle:

1. Mục Tiêu: Kẻ tấn công muốn giải mã một ciphertext $C$
2. Truy Cập Oracle: Kẻ tấn công gửi một ciphertext đã chọn $C'$ đến hệ thống và yêu cầu kiểm tra xem bản rõ sau khi giải mã có tính chẵn hay lẻ.
3. Phản Hồi Oracle: Hệ thống sau đó sẽ trả về thông tin về tính chẵn lẻ của thông điệp đã giải mã.
4. Bản Rõ: Bằng cách gửi đi các ciphertexts giả và quan sát phản hồi của oracle, kẻ tấn công dần dần có thể suy luận ra bản rõ của thông điệp ban đầu mà không cần biết khóa giải mã.

Điều Kiện để Tấn Công Parity Oracle Thành Công:

$\cdot$ Thông tin chẵn lẻ.

$\cdot$ Thiếu Kiểm Tra Tính Hợp Lệ: Nếu hệ thống không kiểm tra đúng tính hợp lệ của thông điệp sau khi giải mã, kẻ tấn công có thể thử nghiệm với nhiều ciphertexts và nhận được phản hồi từ hệ thống.

Điểm khác biệt MSB với LSB:

Bleichenbacher Attack tập trung vào MSB: Phản hồi từ Oracle giúp xác định xem plaintext giải mã có khớp định dạng đầu tiên hay không.

LSB Oracle Attack tập trung vào các bit cuối cùng: Khai thác Oracle để thu hẹp dần các bit từ phía bên phải của plaintext.

Ta có challenge, là một server oracle như sau:

```python
from Crypto.Util.number import *
from os import urandom

flag = (30961397952826794826368629267252806679740709656474098228292952445)
p, q = getPrime(512), getPrime(512)
n = p * q
e = 0x10001
d = inverse(e, (p-1)*(q-1))
assert flag < n
ct = pow(flag, e, n)

print(f"{n = }")
print(f"{e = }")
print(f"{ct = }")

while True:
    try:
        ct_input = int(input("> "))
        pt = pow(ct_input, d, n)
        if pt == flag:
            print("Access granted")
            exit()
        else:
            if pt % 2 == 0:
                print("Even")
            else:
                print("Odd")
    except ValueError:
        print("Invalid input. Please provide a valid ciphertext.")
```
Sau đây là, đoạn code kết nối vào localhost port 1337 để lấy những thông tin từ n, e, cipertext và thông tin của plaintext sau khi hệ thống giãi mã và đưa ra thông tin về parity của plaintext
```python
from Crypto.Util.number import *
from pwn import *

def get_process():
    return remote("localhost", 1337)  # Kết nối với máy chủ đang chạy Oracle

def oracle(x):
    io.sendlineafter(b'> ', str(pow(x, e, n) * ct % n).encode())
    return io.recvline()

# Kết nối tới dịch vụ
io = get_process()

e = 0x10001
io.recvuntil(b'n = ')
n = int(io.recvuntil(b"\n").decode())  # Đọc modulus n
io.recvuntil(b'ct = ')
ct = int(io.recvuntil(b"\n").decode())  # Đọc ciphertext
```
oracle(x): Hàm này sẽ gửi một ciphertext đã thay đổi, và nhận lại kết quả "Even" hoặc "Odd".

Chiến thuật Tìm kiếm Nhị phân: Bạn thực hiện tìm kiếm nhị phân, bắt đầu với phạm vi từ 0 đến n. Mỗi lần, bạn sẽ thử một giá trị trung gian và kiểm tra xem giá trị đã giải mã là chẵn hay lẻ. Sau đó, bạn thu hẹp phạm vi tìm kiếm.

Điều kiện dừng: Khi khoảng cách giữa lb và ub nhỏ hơn 65537, bạn sẽ dừng lại vì đã thu hẹp phạm vi đủ nhỏ để xác định được flag.
```python
# Khởi tạo phạm vi tìm kiếm cho p
lb = 0
ub = n

# Chiến thuật tìm kiếm nhị phân để xác định giá trị của flag
while True:
    # Tiến hành nhân gấp đôi
    k = (lb + ub) // 2  # Tạo giá trị để thử
    if oracle(k) == b"Even\n":
        ub = k  # Nếu kết quả là chẵn, giới hạn phạm vi trên
    else:
        lb = k  # Nếu kết quả là lẻ, giới hạn phạm vi dưới

    # Kiểm tra khi khoảng cách giữa lb và ub nhỏ hơn 65537 (ngưỡng dừng)
    if ub - lb < 65537:
        print(f"Found value: {ub}, {lb}")
        break
```
Ta sẽ có Flag của chúng ta là: b'KCSC{L5B_0r4cl3_svgm4c0ck!}'

## 4. KẾT LUẬN 

**VỪA RỒI LÀ TÓM TẮT TOÀN BỘ LÝ THUYẾT VỀ RSA HỌC QUA 20 BÀI ĐẦU TIÊN CỦA RSA CHALLENGE THEO CÁCH HIỂU CỦA BẢN THÂN**.

Với vài phương thức tấn công và các challenge mẫu, đó là những gì dược học trong khoảng thời gian vừa qua... Một số phương pháp đúc kết được:

Khai thác lỗ hổng dựa vào việc khai thác vào chính hệ thống RSA nói chung, và các khoá bảo mật nói riêng. Đa phần dựa vào các phép toán đồng dư, phép liên phân số, phép tính Binary Search,... Cùng với các định lý liên quan như Định lý Euclid mở rộng hay định lý dư Trung Quốc. Ngoài ra, còn dựng lại các phép tấn công dựa vào CCA2, nắm bắt lỗ hổng trong quá trình tính toán, tấn công vào MSB và LSB lên các hệ thống padding cũ như PKCS #1.






