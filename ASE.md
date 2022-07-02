# AES: Advanced Encryption Standard #
Reference: https://csrc.nist.gov/csrc/media/publications/fips/197/final/documents/fips-197.pdf  

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
