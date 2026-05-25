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

int mod(int a, int p)
{
    int r = a % p;
    if (r < 0)
        r += p;
    return r;
}

int modInverse(int a, int p)
{
    a = mod(a, p);
    for (int x = 1; x < p; x++)
        if ((a * x) % p == 1)
            return x;
    return -1;
}

void pointAdd(int x1,int y1,int x2,int y2,int a,int p,int *x3,int *y3)
{
    int lambda;

    if (x1 == x2 && y1 == y2)
    {
        int num = mod(3*x1*x1 + a, p);
        int den = modInverse(2*y1, p);
        lambda = mod(num * den, p);
    }
    else
    {
        int num = mod(y2 - y1, p);
        int den = modInverse(x2 - x1, p);
        lambda = mod(num * den, p);
    }

    *x3 = mod(lambda*lambda - x1 - x2, p);
    *y3 = mod(lambda*(x1 - *x3) - y1, p);
}

void scalarMultiply(int k,int x,int y,int a,int p,int *rx,int *ry)
{
    int x_temp = x;
    int y_temp = y;

    *rx = x;
    *ry = y;

    for(int i=1;i<k;i++)
    {
        pointAdd(*rx,*ry,x_temp,y_temp,a,p,rx,ry);
    }
}

int main()
{
    int p,a,b;
    int Gx,Gy;
    int dA,dB;

    printf("Enter the prime number (p): ");
    scanf("%d",&p);

    printf("Enter the curve parameters (a and b) for equation y^2 = x^3 + ax + b: ");
    scanf("%d %d",&a,&b);

    printf("Enter the base point G (x and y): ");
    scanf("%d %d",&Gx,&Gy);

    printf("\nEnter Alice's private key: ");
    scanf("%d",&dA);

    printf("Enter Bob's private key: ");
    scanf("%d",&dB);

    int Ax,Ay,Bx,By;

    scalarMultiply(dA,Gx,Gy,a,p,&Ax,&Ay);
    scalarMultiply(dB,Gx,Gy,a,p,&Bx,&By);

    printf("\nAlice's public key: (%d, %d)",Ax,Ay);
    printf("\nBob's public key: (%d, %d)",Bx,By);

    int SAx,SAy,SBx,SBy;

    scalarMultiply(dA,Bx,By,a,p,&SAx,&SAy);
    scalarMultiply(dB,Ax,Ay,a,p,&SBx,&SBy);

    printf("\nShared secret computed by Alice: (%d, %d)",SAx,SAy);
    printf("\nShared secret computed by Bob: (%d, %d)",SBx,SBy);

    if(SAx==SBx && SAy==SBy)
        printf("\nKey exchange successful. Shared secrets match.\n");
    else
        printf("\nKey exchange failed. Shared secrets do not match.\n");

    return 0;
}
```


## Output:
<img width="1920" height="1200" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/c7d48a1c-6e34-4970-845d-bbbd4456030f" />


## Result:
The program is executed successfully

