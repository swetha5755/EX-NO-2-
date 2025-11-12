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




Program:
~~~
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5
char matrix[SIZE][SIZE];
// Remove duplicate characters from keyword and fill matrix
void generateMatrix(char key[]) {
 int alphabet[26] = {0};
 int row = 0, col = 0;
 int k = 0;
 for (int i = 0; key[i]; i++) {
 char ch = toupper(key[i]);
 if (ch == 'J') ch = 'I';
 if (!alphabet[ch - 'A']) {
 matrix[row][col++] = ch;
 alphabet[ch - 'A'] = 1;
 if (col == SIZE) {
 col = 0;
 row++;
 }
 }
 }
 // Fill remaining letters
 for (char ch = 'A'; ch <= 'Z'; ch++) {
 if (ch == 'J') continue;
 if (!alphabet[ch - 'A']) {
 matrix[row][col++] = ch;
 alphabet[ch - 'A'] = 1;
 if (col == SIZE) {
 col = 0;
 row++;
 }
 }
 }
}
// Locate position of a character in matrix
void findPosition(char ch, int *row, int *col) {
 if (ch == 'J') ch = 'I';
 for (int i = 0; i < SIZE; i++) {
 for (int j = 0; j < SIZE; j++) {
 if (matrix[i][j] == ch) {
 *row = i;
 *col = j;
 return;
 }
 }
 }
}
// Encrypt digraphs
void encrypt(char digraph[], char result[]) {
 int r1, c1, r2, c2;
 findPosition(digraph[0], &r1, &c1);
 findPosition(digraph[1], &r2, &c2);
 if (r1 == r2) {
 result[0] = matrix[r1][(c1 + 1) % SIZE];
 result[1] = matrix[r2][(c2 + 1) % SIZE];
 } else if (c1 == c2) {
 result[0] = matrix[(r1 + 1) % SIZE][c1];
 result[1] = matrix[(r2 + 1) % SIZE][c2];
 } else {
 result[0] = matrix[r1][c2];
 result[1] = matrix[r2][c1];
 }
}
// Prepare plaintext into digraphs
int prepareText(char *input, char digraphs[][2]) {
 int len = 0, i = 0;
 while (input[i]) {
 char a = toupper(input[i]);
 if (a == 'J') a = 'I';
 if (!isalpha(a)) {
 i++;
 continue;
 }
 char b = toupper(input[i+1]);
 if (b == 'J') b = 'I';
 if (!isalpha(b) || a == b) {
 digraphs[len][0] = a;
 digraphs[len][1] = 'X'; // padding
 len++;
 i++;
 } else {
 digraphs[len][0] = a;
 digraphs[len][1] = b;
 len++;
 i += 2;
 }
 }
 if (len > 0 && digraphs[len - 1][1] == '\0') {
 digraphs[len - 1][1] = 'X';
 }
 return len;
}
// Display matrix (for debugging)
void displayMatrix() {
 printf("Playfair Matrix:\n");
 for (int i = 0; i < SIZE; i++) {
 for (int j = 0; j < SIZE; j++) {
 printf("%c ", matrix[i][j]);
 }
 printf("\n");
 }
}
int main() {
 char key[] = "KEYWORD"; // You can change this key
 char plaintext[] = "shankarswetha";
 char digraphs[50][2];
 char encryptedText[100] = "";
 char encryptedPair[3];
 encryptedPair[2] = '\0';
 generateMatrix(key);
 displayMatrix();
 int count = prepareText(plaintext, digraphs);
 printf("\nPlaintext Digraphs:\n");
 for (int i = 0; i < count; i++) {
 printf("%c%c ", digraphs[i][0], digraphs[i][1]);
 }
 printf("\n\nEncrypted Text: ");
 for (int i = 0; i < count; i++) {
 encrypt(digraphs[i], encryptedPair);
 strcat(encryptedText, encryptedPair);
 printf("%s ", encryptedPair);
 }
 printf("\n\nFinal Encrypted Output: %s\n", encryptedText);
 return 0;
}
~~~
## Output:

<img width="525" height="133" alt="image" src="https://github.com/user-attachments/assets/9dc54719-acdd-4ade-90a2-0d60a51ab6a4" />

## RESULT:
Thus, the program is executed successfully.
