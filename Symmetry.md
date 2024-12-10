# MÃ HÓA ĐỐI XỨNG (SYMMETRIC ENCRYPTION)
Như ta đã tìm hiểu trong RSA Challenge, mỗi loại mã hóa sẽ đều tìm cách để biến một bản rõ (**Plaintext**) thành một bản mã khó đọc hơn (**Ciphertext**), đối với RSA, đây là một loại mã bất đối xứng, tức là đối với quá trình mã hóa và giải mã sẽ sử dụng các loại khóa công khai (**Public Key**) và khóa riêng tư (**Private Key**) là hai loại khóa khác nhau, nhưng có mỗi tương quan với nhau, sử dụng các kiến thức về lý thuyết số để giải quyết vấn đề mã hóa. Tuy nhiên đối với mã đối xứng, đó là một câu chuyện hoàn toàn khác, khi ta chỉ sử dụng cùng một khóa trong quá trình mã hóa và giải mã.

Các thuật toán mã hóa đối xứng thường nhanh hơn các thuật toán mã hóa bất đối xứng (asymmetric ciphers) do sử dụng các phép toán đơn giản hơn. Việc bảo mật của hệ thống phụ thuộc hoàn toàn vào việc giữ bí mật khóa. Nếu khóa bị lộ, dữ liệu sẽ không còn an toàn. Thường được sử dụng trong việc mã hóa khối lượng lớn dữ liệu như file, cơ sở dữ liệu, hoặc trong các hệ thống truyền thông thời gian thực (VPN, Wi-Fi).
## Các thuật toán mã hóa đối xứng phổ biến
### DES (Data Encryption Standard)
Có nhiều loại mã hóa đối xứng, với độ bảo mật và mục đích bảo mật khác nhau, trước hết ta sẽ tìm hiểu về một trong những loại mã hóa cổ điển

DES (Data Encryption Standard) là một thuật toán mã hóa khối (block cipher) được phát triển vào những năm 1970 và đã trở thành một tiêu chuẩn mã hóa trong ngành công nghiệp bảo mật. Hoạt động trên các khối dữ liệu kích thước 64-bit bằng cách sử dụng khóa 56-bit. DES thực hiện mã hóa qua 16 vòng lặp (rounds) với các phép toán hoán vị, thay thế, và XOR để tạo ra ciphertext từ plaintext.

**1. Cấu trúc tổng quát của DES**

**Kích thước khối:** 64-bit (64 bit của dữ liệu cần mã hóa).

**Độ dài khóa:** 56-bit (khóa thực tế được sử dụng để mã hóa), nhưng khóa được nhập vào dưới dạng 64-bit, mỗi byte thứ 8 là một bit kiểm tra chẵn lẻ.

**Vòng lặp (rounds):** DES sử dụng 16 vòng (rounds) để mã hóa dữ liệu, trong mỗi vòng, dữ liệu được xử lý bằng các phép biến đổi khác nhau.

**2. Quy trình mã hóa trong DES**

Quy trình mã hóa trong DES có thể được chia thành các bước sau:

Dữ liệu đầu vào:

Plaintext: 0123456789ABCDEF (hex)

*Bước 1: Hoán vị đầu vào (Initial Permutation - IP)*

Trong thuật toán DES, bước Initial Permutation (IP) là bước đầu tiên trong quá trình mã hóa, ngay sau khi dữ liệu được chia thành các khối 64 bit. IP có tác dụng trong việc hoán vị các bit đầu vào theo một thứ tự xác định trước. Mục đích chính là tạo ra sự phân tán thông tin và đảm bảo bảo mật, đồng thời cung cấp một sự chuẩn bị cần thiết cho các bước mã hóa tiếp theo.

Lưu ý: Sau khi quá trình mã hóa hoàn thành, một bước ngược lại với IP sẽ được thực hiện, gọi là Final Permutation (FP), để phục hồi dữ liệu về cấu trúc ban đầu.

Dạng nhị phân của plaintext: 0000000100100011010001010110011110001001101010111100111111010000

Sau khi áp dụng bảng IP, ta có: 0100100001101001010100111100100111010000111101100010111101001100

Sau đó chia thành hai phần, phần trái (L) và phần phải (R), mỗi phần có kích thước 32-bit:

$$
\begin{cases}
L_0 = Dữ liệu đầu vào (64-bit)[0:31]: 01001000011010010101001111001001 \\
R_0 = Dữ liệu đầu vào (64-bit)[32:63]: 11010000111101100010111101001100
\end{cases}
$$

*Bước 2: Tạo khóa con*

Khóa ban đầu 56-bit (khóa thực sự, không tính các bit kiểm tra) sẽ được chia thành 16 khóa con, mỗi khóa con dài 48-bit. Quá trình này được thực hiện qua các bước:

1. **Permuted Choice 1 (PC-1)**: Loại bỏ các bit kiểm tra (bits 8, 16, 24, 32, 40, 48, 56, 64) và chia khóa thành hai nửa 28 bit. Trong DES, mỗi byte của khóa chính có 1 bit kiểm tra tính chẵn lẻ (parity bit). Như vậy, có 8 parity bits (1 bit trong mỗi 8 bit).

**Cách thực hiện**: Một bảng hoán vị cố định (PC-1) được sử dụng để chọn ra 56 bit từ 64-bit khóa đầu vào. Bảng này chỉ giữ lại các bit quan trọng, loại bỏ các parity bits.

**BẢNG PC-1**:

![image](https://github.com/user-attachments/assets/9119e3f4-62f6-4805-8459-2d205cc2fbcd)

**Mỗi số trong bảng biểu thị vị trí ban đầu của bit, và vị trí trong bảng là vị trí mới của bit đó trong dãy 56-bit

Khóa chính (64-bit): 10101011 01111001 10110100 11001010 01110111 00100110 11100011 00011101

Sau PC-1 (56-bit): 01001101 01011010 01110111 10010111 10011011 01001000 10110110
 
2. **Chia khóa thành 2 phần (Splitting Key)**:

Sau khi loại bỏ các bit kiểm tra, khóa 56-bit được chia thành 2 nửa bằng nhau:

**$C_0$**: Phần đầu tiên, gồm 28 bit: 0100110101011010011101111001

**$D_0$**: Phần thứ hai, gồm 28 bit: 0111100110110100100010110110

3. **Dịch vòng trái (Left Circular Shift)**

Trong mỗi vòng lặp của DES, các phần C và D được dịch vòng sang trái một số vị trí cố định:

Mỗi nửa (28-bit) được dịch vòng (circular left shift) để tạo ra các giá trị mới ($C_1, D_1, ..., C_{16}, D_{16}$). Nhằm Đảm bảo sự biến đổi liên tục của khóa trong các vòng.

4. **Hoán vị nén (Compression Permutation - PC-2)**

Kết hợp hai nửa khóa (C và D) sau khi dịch vòng và nén chúng từ 56-bit xuống còn 48-bit, tạo ra khóa con (**subkey**) cho mỗi vòng. Một bảng hoán vị nén (PC-2) được áp dụng. Bảng này chọn ra 48 bit từ 56 bit của C và D, sắp xếp lại để tạo ra khóa con.

**Bảng PC-2**:

![image](https://github.com/user-attachments/assets/53eb2e65-90e5-4afd-8fb9-a9da39a14431)

Sau bước dịch vòng: 1001101010110100111011110010111100110110100100010110110

Sau PC-2 (48-bit): 11111100 00101111 00011000 10100010 10011010 01001111

5. **Tạo 16 khóa con (Subkeys)**

Sau 16 vòng lặp dịch vòng và nén, chúng ta có 16 khóa con, mỗi khóa dài 48-bit. Mỗi khóa con được sử dụng trong một vòng của thuật toán Feistel để thực hiện mã hóa dữ liệu.

Khóa con được biến đổi qua từng vòng nhằm tăng cường tính bảo mật của DES. Điều này đảm bảo rằng mỗi vòng mã hóa sử dụng một khóa khác nhau, làm cho quá trình mã hóa trở nên khó bị tấn công hơn.

*Bước 3: 16 vòng mã hóa (Feistel Network)*

Tính toán dựa trên nửa phải $R_{n - 1} và khóa con $K_n$ trong vòng thử n:

Tiếp tục với dữ liệu đầu vào: 0123456789ABCDEF

1. Mở rộng (Expansion): Phần phải $R_{n - 1}$ được mở rộng từ 32 bit thành 48 bit bằng cách sử dụng bảng mở rộng E (Expansion Permutation). sao chép một số bit. Các bit mở rộng sẽ được XOR với khóa con $K_{n - 1}$ của vòng thử $n$.

Ví dụ như đã đề cập ở khối dữ liệu đầu vào đã cho bên trên và nó sau khi được mở rộng (48-bit): $R_{n - 1} = 111010011101100011000011110011011111101011100010

Giả sử XOR với khóa $K_{n - 1}$ = 010110001101101001100010111010011101010110101001

Lúc này $R_{n - 1}$ = 101100010000001010100001001001000010111101001011

2. Sự thay thế (Substitution): Sau phép XOR, kết quả sẽ được chia thành 8 khối 6-bit. Mỗi khối 6-bit sẽ được thay thế bằng 4 bit thông qua một bảng thay thế S-Box. Các bảng S-Box này là các bảng được định nghĩa sẵn và có nhiệm vụ thay thế các giá trị 6-bit thành 4-bit.

Sau khi thay thế S-box, ta có kết quả là: 10101100001000100010110011101011

3. Hoán vị (Permutation): Kết quả sau khi thay thế sẽ trải qua một bước hoán vị (Permutation P) để tạo ra một 32-bit.

Và sau khi được hoán vị theo bảng P, ta có nửa phải của ta là: 11011010101101011011001110110101

4. XOR với phần trái: Kết quả cuối cùng từ phép hoán vị này sẽ được XOR với phần trái $L_{n - 1}$ của dữ liệu để tạo ra phần phải của vòng mã hóa. Phần trái của vòng sau sẽ là phần phải của vòng trước.

$$
\begin{cases}
L_n = R_{n - 1} \\
R_n = L_{n - 1} \oplus f(R_{n - 1}, K_n)
\end{cases}
$$

*Bước 4: Hoán vị cuối cùng (Final Permutation)* Sau 16 vòng, phần trái và phải của dữ liệu sẽ được ghép lại và trải qua một bước hoán vị cuối cùng (inverse initial permutation, $IP^{-1}$) để tạo ra ciphertext.

Sau 16 vòng mã hóa, kết quả là ciphertext 64-bit.

Với ví dụ trên, ciphertext của ta là: 85E813540F0AB405.

Trong DES, quá trình mã hóa bắt đầu với IP, là một bước hoán vị làm thay đổi vị trí các bit của plaintext (dữ liệu gốc). Sau đó, thuật toán tiếp tục thực hiện 16 vòng mã hóa, trong đó dữ liệu (plaintext) bị thay đổi qua nhiều phép toán phức tạp. Đến vòng mã hóa cuối cùng (vòng 16), dữ liệu bị phân thành hai phần (L16 và R16) và được kết hợp lại, nhưng vẫn chưa hoàn thiện. Mặc dù dữ liệu đã trải qua nhiều vòng mã hóa, nhưng nó vẫn chưa được hoàn thiện để trở thành ciphertext. Mục đích của FP là đảo ngược lại IP, giúp kết quả mã hóa cuối cùng có thể dễ dàng được giải mã bằng cách đảo ngược chính các bước hoán vị đã thực hiện trong IP.

DES là một thuật toán mã hóa đối xứng, có nghĩa là quá trình mã hóa và giải mã có thể sử dụng cùng một khóa. Điều này đòi hỏi phải có một sự đối xứng trong cách thức mã hóa và giải mã. FP giúp duy trì sự đối xứng này. Trong khi IP thay đổi các bit của plaintext, thì FP sẽ đảo ngược quá trình này và giúp kết quả cuối cùng (ciphertext) có thể được giải mã lại bằng cách áp dụng lại IP (hoặc FP, vì IP và FP là đảo ngược của nhau).

FP là một bước cuối cùng để "làm mịn" kết quả, giúp cho các bit của ciphertext trở nên khó đoán hơn, và đảm bảo rằng bất kỳ mẫu lặp lại nào trong dữ liệu gốc sẽ không dễ dàng bị nhận diện từ ciphertext.

**3. Các tính chất quan trọng của DES**:

Khóa dài 56-bit: Độ dài khóa của DES là một trong những yếu tố quyết định đến tính bảo mật của thuật toán. Với khóa 56-bit, DES dễ dàng bị tấn công brute-force.

Hiệu quả tính toán: DES có thể được triển khai rất nhanh, đặc biệt trên phần cứng.

Các tấn công vào DES:

$\cdot$ Brute-force attack: Với 56-bit khóa, việc thử tất cả các khả năng khóa có thể được thực hiện trong một thời gian xác định bằng phương pháp brute-force.

$\cdot$ Tấn công dựa trên phân tích (Differential và Linear Cryptanalysis): Các tấn công này có thể khai thác các mối quan hệ giữa plaintext và ciphertext để giảm số lượng phép thử cần thiết để tìm khóa.
