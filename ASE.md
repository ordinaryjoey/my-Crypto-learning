# AES: Advanced Encryption Standard #
Reference:  
  1.https://csrc.nist.gov/csrc/media/publications/fips/197/final/documents/fips-197.pdf   
  2.模的四则运算、求逆等：https://blog.csdn.net/qq_41897386/article/details/82289975  


The input and output for the AES algorithm each consist of sequences of 128 bits (digits with values of 0 or 1).  

The Cipher Key for the AES algorithm is a sequence of 128, 192 or 256 bits.  

The basic unit for processing in the AES algorithm is a **byte**.  

Internally, the AES algorithm’s operations are performed on a two-dimensional array of bytes called the **State**.  
![image](https://user-images.githubusercontent.com/34599267/176986498-95534a66-40ae-4668-ac12-6917b7239738.png)

## Finite Field ##
All bytes in the AES algorithm are interpreted as finite field elements.  
### Addition ###
The addition is performed with the XOR operation 
### Multiplication ###
In the polynomial representation, multiplication in GF($2^8$) (denoted by •) corresponds with the multiplication of polynomials modulo an irreducible polynomial of degree 8. A polynomial is irreducible if its only divisors are one and itself. **For the AES algorithm, this irreducible polynomial is**:  
$$m(x) = x^8+ x^4 + x^3 + x +1$$
or {01}{1b} in hexadecimal notation.  
欧拉定理证明：https://www.cnblogs.com/1024th/p/11349355.html
### Multiplication by x ###
Denoted by x • a(x)
First, left shift. Second, mod m(x)

### Polynomials with Coefficients in GF($2^8$) ###
Four-term polynomials can be defined - with coefficients that are finite field elements - as:
$$a(x) = a_3x^3 + a_2x^2 + a_1x + a_0 $$
其中每个$a_i$均是一个上面所述的8-bit多项式。
Polynomial multiplication in ASE uses a differnt reduction polynomial: $x^4+1$  
The modular product of a(x) and b(x), denoted by $a(x) \otimes b(x)$, is given by the four-term polynomial d(x), defined as follows:
![image](https://user-images.githubusercontent.com/34599267/176997321-d68ad600-1835-400d-ab1d-a7930fc519a0.png)

## Algorithm Specification ##
For the AES algorithm, the length of the input block, the output block and the State is 128 bits. This is represented by Nb = 4.  
For the AES algorithm, the length of the Cipher Key, K, is 128, 192, or 256 bits. The key length is represented by Nk = 4, 6, or 8.  
For the AES algorithm, the number of rounds to be performed during the execution of the algorithm is dependent on the key size. The number of rounds is represented by Nr, where Nr = 10 when Nk = 4, Nr = 12 when Nk = 6, and Nr = 14 when Nk = 8.  
![image](https://user-images.githubusercontent.com/34599267/176997500-7e932ce0-f42a-4b92-93d5-ff072d39a376.png)

**注意，只有AES-128被SHE使用。**
Below is the algorithm pseudo code:  
![image](https://user-images.githubusercontent.com/34599267/177031301-f67e849a-7f73-4546-a6f3-56f698aae42d.png)


### SubBytes() ###  
This operation is on a single byte.
1.Take the multiplicative inverse in the finite field GF($2^8$), described in Sec. 4.2; the element {00} is mapped to itself. 
2.Apply the following affine transformation (over GF(2) ):
![image](https://user-images.githubusercontent.com/34599267/177031488-de9737b3-fc28-4a42-9a20-bfa88442c476.png)
Which can be represented in matrix form as below:
![image](https://user-images.githubusercontent.com/34599267/177031568-21e64071-29de-4889-9fd7-004aa29b30c0.png)
Same effect can also be done by using the S-box, a simple look-up-table which can map $S_{r,c}$ to SubBytes($S^{'}_{r,c}$):
![image](https://user-images.githubusercontent.com/34599267/177031690-99e9aa7f-85cc-4c57-8725-8ba591793c29.png)

### ShiftRows() ###
Shift each $S_{r,c}$ as below:
![image](https://user-images.githubusercontent.com/34599267/177032018-24a4392d-a1c5-49ec-b4f3-278df6f5bbcf.png)

### MixColumns() ###
Uses a fixed  polinomial:
$$ a(x) = 'h03x^3 + 'h01x^2 + 'h01x + 'h02 $$
to calculate:
$$ s^{'}(x) = a(x) \otimes s(x) $$

### AddRoundKey() ###
![image](https://user-images.githubusercontent.com/34599267/177032807-6c1501f3-a642-45ee-acd3-8740878a399f.png)

### Key expansion ###
Key expansion 将一个原本Nb words的key扩展为Nb(Nr+1) words.扩展的key将被应用到AddRoundKey()函数之中。
