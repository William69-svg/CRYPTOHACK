# WRITE-UP CRYPTOHACK: 20 BÀI ĐẦU TIÊN RSA CHALLENGE  
## STARTER
## 1. MODULAR EXPONENTIATION & 2. PUBLIC KEYS

### SỬ DỤNG HAI CÁCH ĐỂ TÍNH HÀM MODULE LUỸ THỪA
### Cách 1:
### Ý tưởng: tạo hàm tính luỹ thừa theo vòng lặp và lấy module theo số m.
```C++
#include <iostream>
#include <math.h>
#include <stdio.h>
using namespace std;
int MOD(int a, int b, int m){    //THIET LAP HAM TINH MODULE
    long long s = 1;              
    for (int i = 1; i <=b; i++){ //TAO VONG LAP THEO DO LON CUA LUY THUA
        s*= a;                   //NHAN SO A VAO BIEN S  
        s%= m;                   //DONG THOI CHIA DU CHO SO m DE TRANH TRAN MAN HINH
    }
    return s; //TRA VE GIA TRI S SAU KHI DUOC GAN HAM MODULE
}
```



Cách 2 (theo em tìm hiểu trên mạng)
### Ý tưởng: dùng phương pháp tính luỹ thừa nhanh(trình bày theo cách hiểu của em)
VÍ DỤ: $a^{10} = a^{5} \cdot a^{5} \lor a^{5} = a^{2} \cdot a^{2} \cdot a$

Khi b chẵn: cơ số tăng 2 và luỹ thừa: $a^{b} = (a^{2})^{\frac{b}{2}}$

Khi b lẻ: luỹ thừa giẳm 1 đơn vị: $a^{b} = (a^{2})^{\frac{b - 1}{2}}$

CÓ THỂ THẤY SẼ GIÚP GIẢM SỐ LẦN THỰC HIỆN PHÉP TOÁN CÒN MỘT NỬA MỖI LẦN THỰC HIỆN VÒNG LẶP
```C++
long long MOD3(int a, int b, int m){    //THIET LAP HAM 
     long long s = 1;                    //KHOI TAO GIA TRI S 
     while (b){                          //DUNG VONG LAP KHI LUY THUA != 0
         if ( b%2==1 ){                  //NEU NHU B LA SO LE   
         s *= a;                         //TA NHAN S VOI A 
         s %= m;                         //THUC HIEN PHEP TOAN MOD DE TOI UU HOA BAI TOAN
     }
         b /= 2;                         //THUC HIEN VIEC GIAM LUY THUA 
         a *= a;                         //BINH PHUONG GIA TRI CUA A THEO NHU MO TA   
         a %= m;                         //VA THUC HIEN PHEP TOAN MOD VOI A SAU KHI BINH PHUONG
     }
         return s;                       //TRA LAI GIA TRI S
}

//PHẦN MAIN EM THỰC HIỆN CẢ YÊU CẦU CỦA BÀI 1 VÀ 2
int main (){
    int a, m;
    int p = 17, q = 23; 
    long long e = 65537; 
    m = p*q;
    cout << "so can nhap la: ";
    cin >> a;
    long long s = MOD(a, e, m);    //THUC HIEN THAM TRI DUA TREN DU LIEU CUA HAM
    cout << "so can tim la: " << s << endl;    //IN KQ KET THUC BAI TOAN
    return 0;
}
```
## 3. Euler's Totient

### Giải bài toán tìm giá trị phi Euler
```c++
#include <iostream>
using namespace std;

int main() {
    long long p = 857504083339712752489993810777LL;
    long long q = 1029224947942998075080348647219LL;
    
    long long phiEuler = (p - 1) * (q - 1);

    cout << phiEuler << endl;

    return 0;
}
```

### Cách hiểu khác: 
```c++
long long EulerTot(long long n){         //Thiet lap ham phan tich thanh thua so nguyen to
     long long Euler = n;
     for (long long i = 2; i <= sqrt(n); i++){
         if (n%i==0){                     //Dieu kien ban dau     
             while (n%i==0){
             n /= i;                      //Khi chia het cho tung so nguyen to thi tiep tuc thuc hien vong lap  
             }
             Euler -= Euler/i;            //Thuc hien loai bo cac boi so cua cac so nguyen to tim duoc
         }
     }
     if (n>1){                            //Sau khi thuc hien cac phep chia nhung n van lon hon 1 thi chua loc ra het toan bo cac thua so nguyen to
         Euler -= Euler/n;                //Tuong tu ben tren vong lap
     }
         return Euler;
 }
 int main(){
     long long p = 857504083339712752489993810777LL, q = 1029224947942998075080348647219LL;
     long long n = p*q;
     cout << EulerTot(n);  
     return 0;
 }

