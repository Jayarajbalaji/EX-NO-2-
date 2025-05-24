## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)
## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




## Program:
~~~
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

char keyTable[SIZE][SIZE];

void generateKeyTable(char key[]) {
    int dict[26] = {0}, k = 0;
    for (int i = 0; key[i]; i++) {
        if (key[i] == 'J') key[i] = 'I';
        if (!dict[key[i] - 'A']) {
            dict[key[i] - 'A'] = 1;
            keyTable[k / SIZE][k % SIZE] = key[i];
            k++;
        }
    }
    for (int i = 0; i < 26; i++) {
        if (i + 'A' != 'J' && !dict[i]) {
            keyTable[k / SIZE][k % SIZE] = i + 'A';
            k++;
        }
    }
}

void findPos(char c, int *r, int *c_) {
    if (c == 'J') c = 'I';
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            if (keyTable[i][j] == c) {
                *r = i; *c_ = j;
                return;
            }
}
void encryptPair(char a, char b, char *x, char *y) {
    int r1, c1, r2, c2;
    findPos(a, &r1, &c1);
    findPos(b, &r2, &c2);
    if (r1 == r2) {
        *x = keyTable[r1][(c1 + 1) % SIZE];
        *y = keyTable[r2][(c2 + 1) % SIZE];
    } else if (c1 == c2) {
        *x = keyTable[(r1 + 1) % SIZE][c1];
        *y = keyTable[(r2 + 1) % SIZE][c2];
    } else {
        *x = keyTable[r1][c2];
        *y = keyTable[r2][c1];
    }
}

void encrypt(char text[], char cipher[]) {
    for (int i = 0; text[i]; i += 2) {
        encryptPair(text[i], text[i + 1], &cipher[i], &cipher[i + 1]);
    }
    cipher[strlen(text)] = '\0';
}

int main() {
    char key[100], text[100], cipher[100];
    printf("Enter key: ");
    scanf("%s", key);
    printf("Enter plain text: ");
    scanf("%s", text);
    
    generateKeyTable(key);
    encrypt(text, cipher);
    printf("Cipher Text: %s\n", cipher);
    return 0;
}


~~~




## Output:
![image](https://github.com/user-attachments/assets/1bdec6f2-ac6d-4ba4-8306-e9afc164b083)
## Result:
The program is executed successfully

