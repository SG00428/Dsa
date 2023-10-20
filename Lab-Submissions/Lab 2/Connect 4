#include <stdio.h>
#include <limits.h>
#include <stdlib.h> // Add this for srand and rand

enum {
    EMPTY = 0,
    RED,
    BLUE,
};

typedef char board_t[4][5];
typedef char player_t;

// Define userColor and aiColor as global variables
player_t userColor, aiColor;

int has_won(board_t board, player_t player) {
    char target;
    if (player == RED)
        target = 'R';
    else
        target = 'B';

    // Check horizontally
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 2; j++) {
            if (board[i][j] == target && board[i][j + 1] == target && board[i][j + 2] == target && board[i][j + 3] == target) {
                return 1;
            }
        }
    }

    // Check vertically
    for (int i = 0; i < 1; i++) {
        for (int j = 0; j < 5; j++) {
            if (board[i][j] == target && board[i + 1][j] == target && board[i + 2][j] == target && board[i + 3][j] == target) {
                return 1;
            }
        }
    }

    // Check diagonally (left-top to right-bottom)
    for (int i = 0; i < 1; i++) {
        for (int j = 0; j < 2; j++) {
            if (board[i][j] == target && board[i + 1][j + 1] == target && board[i + 2][j + 2] == target && board[i + 3][j + 3] == target) {
                return 1;
            }
        }
    }

    // Check diagonally (right-top to left-bottom)
    for (int i = 0; i < 1; i++) {
        for (int j = 3; j < 5; j++) {
            if (board[i][j] == target && board[i + 1][j - 1] == target && board[i + 2][j - 2] == target && board[i + 3][j - 3] == target) {
                return 1;
            }
        }
    }

    return 0;
}

int is_full(board_t board) {
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 5; j++) {
            if (board[i][j] == '*') {
                return 0;
            }
        }
    }
    return 1;
}

typedef struct {
    int col;
    int score;
} move_t;

int drop_piece(board_t board, int col, player_t player) {
    // Find the lowest available row in the specified column
    int row = 3;
    while (row >= 0 && board[row][col] != '*')
        row--;

    if (row >= 0) {
        if (player == userColor)
            board[row][col] = (userColor == RED) ? 'R' : 'B';
        else
            board[row][col] = (aiColor == RED) ? 'R' : 'B';
        return row;  // Return the row where the piece was placed
    } else {
        printf("Column %d is already full. Please choose another column.\n", col);
        return -1;  // Return -1 to indicate failure
    }
}

move_t best_move(board_t board, player_t player, int depth, int alpha, int beta) {
    move_t res;
    move_t AIresponse = {.col = -1, .score = (player == aiColor) ? INT_MIN : INT_MAX};

    if (has_won(board, userColor)) {
        AIresponse.score = -10;
        return AIresponse;
    }
    if (has_won(board, aiColor)) {
        AIresponse.score = 10;
        return AIresponse;
    }

    if (is_full(board) || depth == 0) {
        AIresponse.score = 0;
        return AIresponse;
    }

    for (int col = 0; col < 5; ++col) {
        if (board[0][col] == '*') {
            int row = drop_piece(board, col, player);
            if (row != -1) {
                res = best_move(board, (player == aiColor) ? userColor : aiColor, depth - 1, alpha, beta);
                board[row][col] = '*';

                if (player == aiColor) {
                    if (res.score > AIresponse.score) {
                        AIresponse.col = col;
                        AIresponse.score = res.score;
                    }
                    alpha = (alpha > res.score) ? alpha : res.score;
                } else {
                    if (res.score < AIresponse.score) {
                        AIresponse.col = col;
                        AIresponse.score = res.score;
                    }
                    beta = (beta < res.score) ? beta : res.score;
                }

                if (beta <= alpha) {
                    break; // Alpha-beta pruning
                }
            }
        }
    }

    return AIresponse;
}

void print_board(board_t board) {
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 5; j++) {
            printf("%c", board[i][j]);
        }
        printf("\n");
    }
}

void initialize_board(board_t board) {
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 5; j++) {
            board[i][j] = '*';
        }
    }
}

int main() {
    board_t board;
    initialize_board(board);

    int userColorChoice;
    printf("Welcome to Connect Four!\n");
    printf("Select your color:\n");
    printf("1. Red\n");
    printf("2. Blue\n");
    printf("Enter your choice (1 or 2): ");
    scanf("%d", &userColorChoice);

    if (userColorChoice == 1) {
        userColor = RED;
        aiColor = BLUE;
        printf("You've selected Red (R). AI is Blue (B).\n");
    } else if (userColorChoice == 2) {
        userColor = BLUE;
        aiColor = RED;
        printf("You've selected Blue (B). AI is Red (R).\n");
    } else {
        printf("Invalid choice. Defaulting to Red (R). AI is Blue (B).\n");
        userColor = RED;
        aiColor = BLUE;
    }

    player_t currentPlayer = userColor;

    printf("Connect Four Game\n");
    printf("You are %s, AI is %s.\n", (userColor == RED) ? "RED" : "BLUE", (aiColor == RED) ? "RED" : "BLUE");
    printf("Empty space is denoted by '*'.\n\n");

    while (1) {
        print_board(board);

        if (currentPlayer == userColor) {
            printf("\nUser's turn \n");
            int col;
            printf("Enter column (0-4) to place your piece: ");
            scanf("%d", &col);

            // Ensure the column is valid and not full
            while (col < 0 || col > 4 || board[0][col] != '*') {
                printf("Invalid column. Please choose again: ");
                scanf("%d", &col);
            }

            // Place user's piece
            drop_piece(board, col, userColor);

            // Check if the user has won
            if (has_won(board, userColor)) {
                print_board(board);
                printf("Congratulations! User wins!\n");
                break;
            }

            currentPlayer = aiColor;
        } else {
            printf("\nAI's turn \n");

            // AI selects the best move using minimax
            move_t bestMove = best_move(board, aiColor, 10, INT_MIN, INT_MAX);
            printf("AI selects column %d.\n", bestMove.col);

            // Place AI's piece
            drop_piece(board, bestMove.col, aiColor);

            // Check if the AI has won
            if (has_won(board, aiColor)) {
                print_board(board);
                printf("AI wins! \n");
                break;
            }

            currentPlayer = userColor;
        }

        // Check if the board is full (tie)
        if (is_full(board)) {
            print_board(board);
            printf("It's a tie!\n");
            break;
        }
    }
return 0;
}
