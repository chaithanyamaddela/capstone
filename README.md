# capstone
capstone project bnased on railfence ciper which includes both encryption and decryption
#include <stdio.h>
#include <string.h>

int main() {
    char text[100], encrypted[100], decrypted[100];
    int key;

    printf("Enter text to encrypt: ");
    scanf("%[^\n]%*c", text);
    printf("Enter number of rails: ");
    scanf("%d", &key);

    int len = strlen(text);
    int index = 0;
    char temp[len];
    memset(temp, ' ', len);

    // Print Rail Fence pattern
    printf("\nRail Fence Pattern:\n");
    for (int i = 0; i < key; i++) {
        for (int j = 0, r = 0, d = 1; j < len; j++) {
            if (r == i)
                printf("%c ", text[j]);
            else
                printf("  ");
            if (r == 0)
                d = 1;
            else if (r == key - 1)
                d = -1;
            r += d;
        }
        printf("\n");
    }

    // Encryption
    for (int i = 0; i < key; i++) {
        for (int j = 0, r = 0, d = 1; j < len; j++) {
            if (r == i)
                encrypted[index++] = text[j];
            if (r == 0)
                d = 1;
            else if (r == key - 1)
                d = -1;
            r += d;
        }
    }
    encrypted[index] = '\0';
    printf("\nEncrypted Text: %s\n", encrypted);

    // Decryption
    printf("\nEnter text to decrypt: ");
    scanf(" %[^\n]%*c", encrypted); 
    printf("Enter number of rails for decryption: ");
    scanf("%d", &key);
    len = strlen(encrypted);
    index = 0;

    int pos[len];
    memset(pos, -1, sizeof(pos));
    int row = 0, dir = 1;
    for (int i = 0; i < len; i++) {
        pos[i] = row;
        if (row == 0)
            dir = 1;
        else if (row == key - 1)
            dir = -1;
        row += dir;
    }

    char rail[key][len];
    memset(rail, 0, sizeof(rail));
    int k = 0;
    for (int i = 0; i < key; i++) {
        for (int j = 0; j < len; j++) {
            if (pos[j] == i)
                rail[i][j] = encrypted[k++];
        }
    }

    index = 0;
    row = 0, dir = 1;
    for (int i = 0; i < len; i++) {
        decrypted[i] = rail[row][i];
        if (row == 0)
            dir = 1;
        else if (row == key - 1)
            dir = -1;
        row += dir;
    }
    decrypted[len] = '\0';

    printf("\nDecrypted Text: %s\n", decrypted);

    return 0;
}
