# EX. NO: 3  HILL CIPHER
## AIM:
To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).

## ALGORITHM:

STEP-1: Read the plain text and key from the user. STEP-2: Split the plain text into groups of length three. STEP-3: Arrange the keyword in a 3*3 matrix.
STEP-4: Multiply the two matrices to obtain the cipher text of length three.
STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM 
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define BLOCK 3
#define ALPHABET 26

/* Key matrix (3x3) and its integer inverse matrix (precomputed)
   These must be such that det(keymat) is invertible mod 26 and
   invkeymat is the modular inverse matrix (integer matrix) such that
   invkeymat * keymat ≡ I (mod 26). */
int keymat[BLOCK][BLOCK] = {
    { 1, 2, 1 },
    { 2, 3, 2 },
    { 2, 2, 1 }
};

int invkeymat[BLOCK][BLOCK] = {
    { -1,  0,  1 },
    {  2, -1,  0 },
    { -2,  2, -1 }
};

char alpha[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

/* Safe modulo that always returns 0..25 for possibly negative values */
static int mod26(int x) {
    x %= ALPHABET;
    if (x < 0) x += ALPHABET;
    return x;
}

/* Encode a block of 3 characters: P (plaintext vector) -> C = K * P (mod 26) */
void encode_block(char a, char b, char c, char *out) {
    int p[BLOCK];
    int res[BLOCK] = {0};

    p[0] = toupper((unsigned char)a) - 'A';
    p[1] = toupper((unsigned char)b) - 'A';
    p[2] = toupper((unsigned char)c) - 'A';

    for (int i = 0; i < BLOCK; i++) {
        for (int j = 0; j < BLOCK; j++) {
            res[i] += keymat[i][j] * p[j];
        }
        res[i] = mod26(res[i]);
        out[i] = alpha[res[i]];
    }
    out[BLOCK] = '\0';
}

/* Decode a block of 3 characters: C -> P = K^{-1} * C (mod 26) */
void decode_block(char a, char b, char c, char *out) {
    int p[BLOCK];
    int res[BLOCK] = {0};

    p[0] = toupper((unsigned char)a) - 'A';
    p[1] = toupper((unsigned char)b) - 'A';
    p[2] = toupper((unsigned char)c) - 'A';

    for (int i = 0; i < BLOCK; i++) {
        for (int j = 0; j < BLOCK; j++) {
            res[i] += invkeymat[i][j] * p[j];
        }
        res[i] = mod26(res[i]);
        out[i] = alpha[res[i]];
    }
    out[BLOCK] = '\0';
}

int main(void) {
    char msg[1024];
    char enc[1024] = "";
    char dec[1024] = "";
    char temp[BLOCK + 1];
    size_t len;

    /* Example input (you can replace this or read from stdin) */
    strcpy(msg, "shini");

    printf("Simulation of Hill Cipher (3x3)\n");
    printf("Input message : %s\n", msg);

    /* Convert to uppercase and remove non-alpha if you prefer.
       Here we only uppercase; non-alpha would cause issues in mapping. */
    for (size_t i = 0; i < strlen(msg); i++) {
        msg[i] = toupper((unsigned char)msg[i]);
    }

    /* Padding to multiple of 3 using 'X' */
    len = strlen(msg);
    if (len % BLOCK != 0) {
        int pad = BLOCK - (len % BLOCK);
        for (int i = 0; i < pad; i++) {
            strncat(msg, "X", sizeof(msg) - strlen(msg) - 1);
        }
    }

    printf("Padded message : %s\n", msg);

    /* Encoding */
    for (size_t i = 0; i < strlen(msg); i += BLOCK) {
        encode_block(msg[i], msg[i + 1], msg[i + 2], temp);
        strncat(enc, temp, sizeof(enc) - strlen(enc) - 1);
    }
    printf("Encoded message : %s\n", enc);

    /* Decoding */
    for (size_t i = 0; i < strlen(enc); i += BLOCK) {
        decode_block(enc[i], enc[i + 1], enc[i + 2], temp);
        strncat(dec, temp, sizeof(dec) - strlen(dec) - 1);
    }
    printf("Decoded message : %s\n", dec);

    return 0;
}
```

## OUTPUT

<img width="795" height="474" alt="image" src="https://github.com/user-attachments/assets/dc8fbfac-0764-4b4e-85b1-feba9a425e91" />

## RESULT
The program is executed successfully.
