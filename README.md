# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```

  #include <stdio.h>
  
  // Define a structure for a point on the elliptic curve
  typedef struct {
      int x;
      int y;
      int is_infinity; // Boolean to indicate point at infinity
  } Point;
  
  // Elliptic curve parameters: y^2 = x^3 + ax + b (mod p)
  const int a = 1;
  const int b = 6;
  const int p = 19; // prime field
  
  // Modular operation
  int mod(int value, int m) {
      int result = value % m;
      return (result < 0) ? result + m : result;
  }
  
  // Modular inverse
  int mod_inverse(int k, int m) {
      k = mod(k, m);
      for (int x = 1; x < m; x++) {
          if ((k * x) % m == 1) return x;
      }
      return -1;
  }
  
  // Point addition
  Point point_addition(Point P, Point Q) {
      if (P.is_infinity) return Q;
      if (Q.is_infinity) return P;
  
      Point result;
      result.is_infinity = 0;
      int lambda;
  
      if (P.x == Q.x && P.y == Q.y) {
          // Point doubling
          int denominator = mod_inverse(2 * P.y, p);
          if (denominator == -1) {
              result.is_infinity = 1;
              return result;
          }
          lambda = mod((3 * P.x * P.x + a) * denominator, p);
      } else {
          // Point addition
          int denominator = mod_inverse(Q.x - P.x, p);
          if (denominator == -1) {
              result.is_infinity = 1;
              return result;
          }
          lambda = mod((Q.y - P.y) * denominator, p);
      }
  
      result.x = mod(lambda * lambda - P.x - Q.x, p);
      result.y = mod(lambda * (P.x - result.x) - P.y, p);
  
      return result;
  }
  
  // Scalar multiplication
  Point scalar_multiplication(Point P, int n) {
      Point result;
      result.is_infinity = 1; // Point at infinity
      Point addend = P;
  
      while (n > 0) {
          if (n & 1) {
              result = point_addition(result, addend);
          }
          addend = point_addition(addend, addend);
          n >>= 1;
      }
      return result;
  }
  
  // Main function
  int main() {
      Point G = {2, 5, 0}; // New base point
      int n = 9;           // New scalar
  
      printf("Base point G: (%d, %d)\n", G.x, G.y);
      Point R = scalar_multiplication(G, n);
  
      if (R.is_infinity) {
          printf("Result of %d * G: Point at Infinity\n", n);
      } else {
          printf("Result of %d * G: (%d, %d)\n", n, R.x, R.y);
      }
  
      return 0;
  }
```


## Output:
<img width="1520" height="821" alt="image" src="https://github.com/user-attachments/assets/38376eab-0a19-4693-bbb5-51f7e4b40474" />



## Result:
The program is executed successfully

