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

```

#include <stdio.h>
#include <string.h>
#include <ctype.h>

char key[5][5];

void generateKey(char keyword[])
{
    int used[26] = {0};
    int i, j, k = 0;
    char ch;

    used['J' - 'A'] = 1;  // Combine I and J

    // Fill matrix using keyword
    for (i = 0; keyword[i] != '\0'; i++)
    {
        ch = toupper((unsigned char)keyword[i]);

        if (ch >= 'A' && ch <= 'Z')
        {
            if (ch == 'J')
                ch = 'I';

            if (!used[ch - 'A'])
            {
                key[k / 5][k % 5] = ch;
                used[ch - 'A'] = 1;
                k++;
            }
        }
    }

    // Fill remaining letters alphabetically
    for (ch = 'A'; ch <= 'Z'; ch++)
    {
        if (!used[ch - 'A'])
        {
            key[k / 5][k % 5] = ch;
            k++;
        }
    }
}

void displayKey()
{
    int i, j;

    printf("\nKey Matrix:\n");

    for (i = 0; i < 5; i++)
    {
        for (j = 0; j < 5; j++)
            printf("%c ", key[i][j]);

        printf("\n");
    }
}

void findPosition(char ch, int *row, int *col)
{
    int i, j;

    if (ch == 'J')
        ch = 'I';

    for (i = 0; i < 5; i++)
    {
        for (j = 0; j < 5; j++)
        {
            if (key[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void encryptPair(char a, char b, char *x, char *y)
{
    int r1, c1, r2, c2;

    findPosition(a, &r1, &c1);
    findPosition(b, &r2, &c2);

    if (r1 == r2)
    {
        // Same row: move right
        *x = key[r1][(c1 + 1) % 5];
        *y = key[r2][(c2 + 1) % 5];
    }
    else if (c1 == c2)
    {
        // Same column: move down
        *x = key[(r1 + 1) % 5][c1];
        *y = key[(r2 + 1) % 5][c2];
    }
    else
    {
        // Rectangle rule
        *x = key[r1][c2];
        *y = key[r2][c1];
    }
}

int main()
{
    char keyword[200], plaintext[500];
    char text[1000], cipher[1000];
    int i, j = 0, n = 0;

    printf("Enter keyword: ");
    fgets(keyword, sizeof(keyword), stdin);

    printf("Enter plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);

    generateKey(keyword);
    displayKey();

    // Remove spaces and non-alphabetic characters
    // Convert to uppercase and replace J with I
    for (i = 0; plaintext[i] != '\0'; i++)
    {
        char ch = toupper((unsigned char)plaintext[i]);

        if (ch >= 'A' && ch <= 'Z')
        {
            if (ch == 'J')
                ch = 'I';

            text[n++] = ch;
        }
    }

    text[n] = '\0';

    // Form digraphs, inserting X between repeated letters
    i = 0;

    while (i < n)
    {
        char a = text[i];
        char b;

        if (i + 1 >= n)
        {
            b = 'X';
            i++;
        }
        else if (text[i] == text[i + 1])
        {
            b = 'X';
            i++;
        }
        else
        {
            b = text[i + 1];
            i += 2;
        }

        encryptPair(a, b, &cipher[j], &cipher[j + 1]);
        j += 2;
    }

    cipher[j] = '\0';

    printf("\nPrepared Plaintext: %s\n", text);
    printf("Ciphertext: %s\n", cipher);

    return 0;
}

```


## Output:

<img width="1107" height="446" alt="image" src="https://github.com/user-attachments/assets/dec41f96-b1ef-49bc-b9ab-f086d25383af" />

## Result:

Thus, the C program to implement the Playfair Cipher encryption technique was executed successfully, and the plaintext was encrypted into ciphertext.
