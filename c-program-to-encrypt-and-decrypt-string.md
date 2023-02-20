## C program to encrypt and decrypt the string using Caesar Cypher Algorithm.

For encryption and decryption, we have used 3 as a key value. While encrypting
the given string, 3 is added to the ASCII value of the characters. Similarly,
for decrypting the string, 3 is subtracted from the ASCII value of the characters
to print an original string.

```c
/* Simple C program to encrypt and decrypt a string */
#include <stdio.h>

int main(void) {
    int i, x;
    char str[100];

    printf("\nPlease enter a string:\t");
    gets(str);

    printf("\nPlease choose following options:\n");
    printf("1 = Encrypt the string.\n");
    printf("2 = Decrypt the string.\n");
    scanf("%d", &x);

    /* using switch case statements */
    switch(x) {
        case 1: {
            for(i = 0; (i < 100 && str[i] != '\0'); i++) {
                str[i] = str[i] + 3; /* the key for encryption is 3 that is added to ASCII value */
            }
            printf("\nEncrypted string: %s\n", str);
        } break;

        case 2: {
            for(i = 0; (i < 100 && str[i] != '\0'); i++) {
                str[i] = str[i] - 3; /* the key for encryption is 3 that is subtracted to ASCII value */
            }
            printf("\nDecrypted string: %s\n", str);
        } break;

        default: {
            printf("\nError\n");
        } break;
    }
    return 0;
}
```

In the above program, we have used simple logic for encrypting and decrypting
a given string by simply adding and subtracting the particular key from
ASCII value.

## C program to encrypt and decrypt the string using RSA algorithm.

RSA is another method for encrypting and decrypting the message. It involves
public key and private key, where the public key is known to all and is used
to encrypt the message whereas private key is only used to decrypt the
encrypted message.

It has mainly 3 steps:

1. Creating Keys

  - Select two large prime numbers $x$ and $y$
  - Compute $n = x * y$ where $n$ is the modulus of private and the public key
  - Calculate totient function, $\phi(n) = (x − 1)(y − 1)$
  - Choose an integer $e$ such that $e$ is coprime to $\phi(n)$ and $1 < e < \phi(n)$. $e$ is the public key exponent used for encryption
  - Now choose $d$, so that $d * e \pmod \phi(n) = 1$, i.e., $d$ is the multiplicative inverse of $e$ in $\pmod \phi(n)$

2. Encrypting Message

  - Messages are encrypted using the Public key generated and is known to all.
  - The public key is the function of both $e$ and $n$ i.e. ${e,n}$.
  - If $M$ is the message(plain text), then ciphertext $C = M ^ n( \pmod n )$

3. Decrypting Message

  - The private key is the function of both $d$ and $n$ i.e ${d,n}$.
  - If $C$ is the encrypted ciphertext, then the plain decrypted text $M$ is $M \equiv C ^ d ( \pmod n )$

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <string.h>

int x, y, n, t, i, flag;
long int e[50], d[50], temp[50], j, m[50], en[50];
char msg[100];
int prime(long int);
void encryption_key();
long int cd(long int);
void encrypt();
void decrypt();

int main(void) {
    printf("\nENTER FIRST PRIME NUMBER\n");
    scanf("%d", &x);
    flag = prime(x);
    if(flag == 0) {
        printf("\nINVALID INPUT\n");
        exit(0);
    }
    printf("\nENTER SECOND PRIME NUMBER\n");
    scanf("%d", &y);
    flag = prime(y);
    if(flag == 0 || x == y) {
        printf("\nINVALID INPUT\n");
        exit(0);
    }
    printf("\nENTER MESSAGE OR STRING TO ENCRYPT\n");

    scanf("%s",msg);
    for(i = 0; msg[i] != NULL; i++) {
        m[i] = msg[i];
    }
    n = x * y;
    t = (x-1) * (y-1);
    encryption_key();
    printf("\nPOSSIBLE VALUES OF e AND d ARE\n");
    for(i = 0; i < j-1; i++) {
        printf("\n%ld\t%ld", e[i], d[i]);
    }
    encrypt();
    decrypt();
    return 0;
}
int prime(long int pr) {
    int i;
    j = sqrt(pr);
    for(i = 2; i <= j; i++) {
        if(pr % i == 0) return 0;
    }
    return 1;
}

/* function to generate encryption key */
void encryption_key() {
    int k;
    k = 0;
    for(i = 2; i < t; i++) {
        if(t % i == 0) continue;
        flag = prime(i);
        if(flag == 1 && i != x && i != y) {
            e[k] = i;
            flag = cd(e[k]);
            if(flag > 0) {
                d[k] = flag;
                k++;
            }
            if(k == 99) break;
        }
    }
}

long int cd(long int a) {
    long int k = 1;
    while(1) {
        k = k + t;
        if(k % a == 0) return(k / a);
    }
}

/* function to encrypt the message */
void encrypt() {
    long int pt, ct, key = e[0], k, len;
    i = 0;
    len = strlen(msg);
    while(i != len) {
        pt = m[i];
        pt = pt - 96;
        k = 1;
        for(j = 0; j < key; j++) {
            k = k * pt;
            k = k % n;
        }
        temp[i] = k;
        ct = k + 96;
        en[i] = ct;
        i++;
    }
    en[i] = -1;
    printf("\n\nTHE ENCRYPTED MESSAGE IS\n");
    for(i = 0; en[i] != -1; i++) {
        printf("%c", en[i]);
    }
}

/* function to decrypt the message */
void decrypt() {
    long int pt, ct, key = d[0], k;
    i = 0;
    while(en[i] != -1) {
        ct = temp[i];
        k = 1;
        for(j = 0; j < key; j++) {
            k = k * ct;
            k = k % n;
        }
        pt = k + 96;
        m[i] = pt;
        i++;
    }
    m[i] = -1;
    printf("\n\nTHE DECRYPTED MESSAGE IS\n");
    for(i = 0; m[i] != -1; i++) {
        printf("%c", m[i]);
    }
    printf("\n");
}
```
