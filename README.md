# BLOG 1:
# RSA và các phương thức tấn công
## 1. RSA là gì?
### Trong mật mã học, RSA là một thuật toán mật mã hoá khoá công khai được sử dụng rộng rãi trong các hệ thống bảo mật hiện đại. Được đặt tên theo tên của ba nhà khoa học Ron Rivest, Adi Shamir và Leonard Adleman, RSA đã trở thành một tiêu chuẩn trong việc bảo vệ thông tin nhạy cảm.

## 2. RSA hoạt động như thế nào?
### RSA dựa trên nguyên tắc của một cặp khóa:

Khóa công khai(N, e): Được chia sẻ rộng rãi và dùng để mã hóa dữ liệu.

Khóa riêng(d): Được giữ bí mật và chỉ người sở hữu mới có thể dùng để giải mã dữ liệu đã được mã hóa bằng khóa công khai tương ứng.

### Quá trình mã hóa và giải mã:

**Tạo cặp khóa**: Người dùng tạo ra một cặp khóa gồm khóa công khai và khóa riêng.

Để tạo ra một cặp khoá công khai ta tạo ra một cặp số (N, e) tương ứng với:

N = p*q (p, q là hai số nguyên tố) 

Sau đó tính: phi(N) = (p - 1)*(q - 1) và chọn một số e (1 < e < phi(N)) sao cho e và phi(N) là hai số nguyên tố cùng nhau

Và cuối cùng là tạo ra khoá riêng d 

Với: d = $e^{-1} \pmod{phi(N)}$ hay là $d \cdot e \equiv 1 \pmod{phi(N)}$


**Mã hóa**: Người gửi sử dụng khóa công khai của người nhận để mã hóa thông điệp.

Với một thông tin cần mã hoá m, ta sẽ mã hoá như sau:

### c = $m^e \pmod{N}$

**Giải mã**: Người nhận sử dụng khóa riêng của mình để giải mã thông điệp đã được mã hóa.

Để mã hoá thông điệp c trên, ta sẽ giải mã như sau:

### m = $c^d \pmod{N}$ vì khi này $c^d \equiv  m^{ed} \pmod{N}$

### Sự bảo mật của RSA:

Khó phá vỡ: Việc phá vỡ mã RSA đòi hỏi phải giải quyết một bài toán toán học phức tạp, đó là việc phân tích một số lớn thành tích của các số nguyên tố và bài toán RSA

## 3. Các phương thức tấn công
### Như đã tìm hiểu bên trên thì RSA là một thuật toán mã hoá phức tạp và đòi hỏi nhiều sự tính toán 
### Sau đây sẽ là một vài phương thức tấn công trong RSA 
### I. Factoring Large Integers (Phân tích các số nguyên lớn):

Phương thức tiếp cận đầu tiên và trực diện nhất là phân tích N thành các thừa số nguyên tố, đây là lối tấn công đầu tiên đơn giản và trực diện nhất với việc ta đã biết trước số tự nhiên e. 

Như được biết bên trên, N được tạo thành từ 2 số nguyên tố lớn vì vậy nên ta có thể sử dụng các tool như Factordb để có thể phân tích N trực tiếp. Ngoài ra còn có thể sử dụng các thuật toán phân tích số nguyên tố: Các thuật toán như thuật toán chia thử, thuật toán rho của Pollard, thuật toán số sàng,...

Với một số trường hợp khi N không dễ phân tích thì ta sẽ có những hướng tiếp cận khác, tuy nhiên, việc phân tích module N vẫn là một bước thiết yếu và quan trọng trong việc giải các bài toán phức tạp hơn.

### II. Elementary Attacks:

**COMMON MODULUS**

Trong trường hợp này, sẽ chỉ có một module N được sử dụng, vì vậy với mỗi người dùng thứ i thì sẽ có cặp ($e_i$, $d_i$) tương ứng thì mỗi người sẽ có một khoá cá nhân và khoá công khai lần lượt là $(N, e_i)$ và $(N, d_i)$ 

Tuy nhiên đây là một cách tạo khoá có nhiều sơ hở, vì giả sử có Alice và Bob sở hữu 2 cặp $(e_a, d_a)$ và $(e_b, d_b)$

Ta hoàn toàn có thể mã hoá được $(d_a) 


