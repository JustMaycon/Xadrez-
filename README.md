# Xadrez-

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SIZE 8

char board[SIZE][SIZE];

void initializeBoard() {
    char pieces[] = {'R','N','B','Q','K','B','N','R'};
    for (int i = 0; i < SIZE; i++) {
        board[0][i] = pieces[i];
        board[1][i] = 'P';
        board[6][i] = 'p';
        board[7][i] = pieces[i] + 32; // lowercase for black
        for (int j = 2; j < 6; j++) {
            board[j][i] = ' ';
        }
    }
}

void printBoard() {
    printf("  a b c d e f g h\n");
    for (int i = 0; i < SIZE; i++) {
        printf("%d ", 8 - i);
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", board[i][j]);
        }
        printf("\n");
    }
}

int isValidMove(int sx, int sy, int dx, int dy, int whiteTurn) {
    char piece = board[sx][sy];
    if (piece == ' ' || (whiteTurn && piece >= 'a') || (!whiteTurn && piece <= 'Z')) {
        return 0;
    }
    return 1; // Simplificação: não valida regras de cada peça
}

void movePiece(int sx, int sy, int dx, int dy) {
    board[dx][dy] = board[sx][sy];
    board[sx][sy] = ' ';
}

int main() {
    initializeBoard();
    int whiteTurn = 1;
    char input[6];

    while (1) {
        printBoard();
        printf("%s move (e2e4): ", whiteTurn ? "White" : "Black");
        fgets(input, sizeof(input), stdin);
        int sy = input[0] - 'a';
        int sx = 8 - (input[1] - '0');
        int dy = input[2] - 'a';
        int dx = 8 - (input[3] - '0');

        if (sx < 0 || sx >= SIZE || sy < 0 || sy >= SIZE || dx < 0 || dx >= SIZE || dy < 0 || dy >= SIZE) {
            printf("Invalid input.\n");
            continue;
        }

        if (!isValidMove(sx, sy, dx, dy, whiteTurn)) {
            printf("Invalid move.\n");
            continue;
        }

        movePiece(sx, sy, dx, dy);
        whiteTurn = !whiteTurn;
    }

    return 0;
}