/*

THIS IS A GAME "checkers" in konsole that you can play with my AI (called REKSIO :D),
THE RULES ARE SIMPLE:
0. YOU CHOOSE A COLOR B IS BLACK (acualy blue), AND W IS WHITE (acualy red)
1. THE WHITE HAVE A FIRST MOVE
2. YOU CAN MOVE ONLY FORWARD TO AN EMPTY LOCATION
3. YOU HAVE TO BEAT IF YOU HAVE AN OPURTINITY
4. YOU HAVE TO BEAT BACKWARDS
5. IF YOU CAN YOU MUST BEAT MULTIPLE TIMES IN ONE MOVE
6. IF YOU WERE ABLE TO GO TO THE END OF BOARD YOUR PAWN IS A QUEEN NOW (IT MEANS YOU CAN MOVE FORWARD OR BACKWARD IN ONE DIRECTION AS FAR AS THERE IS EMPTY PLACE
7. IF YOU HAVE OPORTUNITY YOU MUST BEAT USING YOUR QUEEN

THIS IS "CHECKERS",
CREATED BY BARTOSZ
_________________________________________________________
| #### |      | #### |      | #### |      | #### |      |
|######|      |######|      |######|      |######|      |
| #### |______| #### |______| #### |______| #### |______|
|      | #### |      | #### |      | #### |      | #### |
|      |######|      |######|      |######|      |######|
|______| #### |______| #### |______| #### |______| #### |
| #### |      | #### |      | #### |      | #### |      |
|######|      |######|      |######|      |######|      |
| #### |______| #### |______| #### |______| #### |______|
|      |      |      |      |      |      |      |      |
|      |      |      |      |      |      |      |      |
|______|______|______|______|______|______|______|______|
|      |      |      |      |      |      |      |      |
|      |      |      |      |      |      |      |      |
|______|______|______|______|______|______|______|______|
|      | #### |      | #### |      | #### |      | #### |
|      |######|      |######|      |######|      |######|
|______| #### |______| #### |______| #### |______| #### |
| #### |      | #### |      | #### |      | #### |      |
|######|      |######|      |######|      |######|      |
| #### |______| #### |______| #### |______| #### |______|
|      | #### |      | #### |      | #### |      | #### |
|      |######|      |######|      |######|      |######|
|______| #### |______| #### |______| #### |______| #### |
*/
#include <stdio.h>
#include <stdlib.h>
//#include <windows.h>
#include <ctype.h>
#include <string.h>

#define RED   "\x1B[31m"
#define GRN   "\x1B[32m"
#define YEL   "\x1B[33m"
#define BLU   "\x1B[34m"
#define MAG   "\x1B[35m"
#define CYN   "\x1B[36m"
#define WHT   "\x1B[37m"
#define RESET "\x1B[0m"

typedef struct node
{
    int position_before_x;
    int position_before_y;
    int position_after_x;
    int position_after_y;
    struct node* next;
} node;

typedef struct tree
{
    int position_before_x;
    int position_before_y;
    int position_after_x;
    int position_after_y;
    float value;
    int board[8][8];
} tree;

typedef struct
{
    int position_before_x;
    int position_before_y;
    int position_after_x;
    int position_after_y;
} pos;


void start(int player);
void draw(void);
void move(int player);
int  check_win(int player);
void game_status(int status);
int check_move(int* before, int* after, int player);
node* must_beat(int player);
void free_node(node* origin);
void upgrade_to_quin(void);
void upgrade_board(int* before, int* after, int color);
void REKSIO_move(int player);
void print_board(void);
int player_have_valid_move(int player, int i, int y, int boardv2[8][8]);
int change_player(int player);
int check_movev2(int* before, int* after, int player, int boardv2[8][8]);
node* must_beatv2(int player, int boardv2[8][8]);
pos next_move(int more_board[8][8], int player, int i, int y, pos last_move);
void upgrade(int* before, int* after, int color, int boardv2[8][8]);
float board_value(int more_board[8][8], int player);
tree symulate(int player,int test_board[8][8], int times);
void draw_board(int board2[8][8]);
void back_move(tree move, int board2[8][8]);
int check_times(int board_times[8][8], int player);
void upgrade_to_quin_board(int boardw[8][8]);


int board[8][8] = {{0}, {0}};
int REKSIO;
int PLAYER;
int PLAYER_quin;
int REKSIO_quin;
int pawns[3] = {0, 12, 12};
int value = 0;

int main(void)
{
    char color;

    printf("choose color: (type 'b' or 'w' and hit enter)");
    scanf("%c", &color);
    switch (color)
    {
        case 'b':
            start(1); //initzializing a game as black
            break;
        case 'w':
            start(2); //initzializing a game as white
            break;
        default:
            printf("something wrong try again!!!");
            return 1;
    }

    draw(); // draw an empty board

    while(1) // while there isn't a draw
    {
        move(2); //white move
        upgrade_to_quin();
        //print_board();
        draw(); //draw a board after move
        if (check_win(1)) //checking for a posible white win
        {
            game_status(2); // print that you lose or won in order wchich color you are
            return 0;
        }
        move(1); //black move
        upgrade_to_quin();
        draw();
        if (check_win(2)) //checking for a posible white win
        {
            game_status(1); // print that you lose or won in order wchich color you are
            return 0;
        }
    }
    game_status(3); // print that there is a draw
    return 0;
}

void start(int player)
{
    player == 1 ? (REKSIO = 2, PLAYER = 1) : (REKSIO = 1, PLAYER = 2);
    REKSIO_quin = REKSIO * 10;
    PLAYER_quin = PLAYER * 10;

    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 8; j++)
        {
            if (i % 2 == 0)
            {
                if (((8 * i) + j) % 2 == 1) board[i][j] = REKSIO;
            }
            else if (((8 * i) + j) % 2 == 0) board[i][j] = REKSIO;
        }
    }
    for (int i = 5; i < 8; i++)
    {
        for (int j = 0; j < 8; j++)
        {
            if (i % 2 == 0)
            {
                if (((8 * i) + j) % 2 == 1) board[i][j] = PLAYER;
            }
            else if (((8 * i) + j) % 2 == 0) board[i][j] = PLAYER;
        }
    }
    /*
   board[1][0] = PLAYER;
   board[7][0] = PLAYER;
   board[4][5] = PLAYER;
   //board[3][2] = REKSIO;
   //board[3][4] = REKSIO;
   //board[4][5] = REKSIO;
   board[6][7] = (REKSIO * 10);
   board[0][3] = REKSIO;
   board[2][7] = REKSIO;
    board[4][7] = REKSIO;
    */

    return;
}

void draw(void)
{
    //SetConsoleTextAttribute (GetStdHandle(STD_OUTPUT_HANDLE), BACKGROUND_BLUE);
    for (int i = 0; i < 25; i++)
    {
        for (int j = 0; j < 9; j++)
        {
            if (j == 8)
            {
                i == 0 ? printf(" ") : printf("|");
            }
            else
            {
                if (i == 0) printf("_______");
                else if (i % 3 == 0)
                {
                    if (j % 2 == 0 && i % 6 == 3)
                        printf(RED   "|######"   RESET);
                    else if (j % 2 == 1 && i % 6 == 0)
                        printf(RED   "|######"   RESET);
                    else
                        printf("|______");
                }
                else if (i % 3 == 1)
                {
                    if (j % 2 == 0 && i % 6 == 4)
                        printf("|%c%i", ((j) + 'A'), i/3);
                    else if (j % 2 == 1 && i % 6 == 1)
                        printf("|%c%i", ((j) + 'A'), i/3);
                    else
                        printf("|");
                    if (board[(i - 1)/ 3][j] == 2 ) printf(GRN   "__  "   RESET);
                    else if (board[(i - 1)/ 3][j] == 1 ) printf(BLU   "__  "   RESET);
                    else if (board[(i - 1)/ 3][j] == 20 ) printf(GRN   "__  "   RESET);
                    else if (board[(i - 1)/ 3][j] == 10 ) printf(BLU   "__  "   RESET);
                    else if (j % 2 == 0 && i % 6 == 1)
                        printf(RED   "######"   RESET);
                    else if (j % 2 == 1 && i % 6 == 4)
                        printf(RED   "######"   RESET);
                    else printf(RED   "    "   RESET);
                }
                else if (i % 3 == 2)
                {
                    printf("|");
                    if (board[(i - 1)/ 3][j] == 2 ) printf(GRN   " |__| "   RESET);
                    else if (board[(i - 1)/ 3][j] == 1 ) printf(BLU   " |__| "   RESET);
                    else if (board[(i - 1)/ 3][j] == 20 ) printf(GRN   " |$$| "   RESET);
                    else if (board[(i - 1)/ 3][j] == 10 ) printf(BLU   " |$$| "   RESET);
                    else if (j % 2 == 0 && i % 6 == 2)
                        printf(RED   "######"   RESET);
                    else if (j % 2 == 1 && i % 6 == 5)
                        printf(RED   "######"   RESET);
                    else printf("      ");
                }
            }
        }
        printf("\n");
    }
}

void move(int player)
{
    if (player == PLAYER)
    {
        begining:  ;
        char str[5];
        int pos_before[2];
        int pos_after[2];
        int is_possible = 1;

        printf("choose next move: (egz. A5B4)\n");
        scanf("%4s", str);
        printf("pozycja: %s\n", str);

        //changing a string input to board values
        pos_before[1] = (toupper(str[0]) - 'A');
        if (pos_before[1] < 0 || pos_before[1] > 7) is_possible = 0;
        pos_before[0] = (str[1] - '0');
        if (pos_before[0] < 0 || pos_before[0] > 7) is_possible = 0;
        pos_after[1] = (toupper(str[2]) - 'A');
        if (pos_after[1] < 0 || pos_after[1] > 7) is_possible = 0;
        pos_after[0] = (str[3] - '0');
        if (pos_after[0] < 0 || pos_after[0] > 7) is_possible = 0;

        if (is_possible == 0) {
            printf("1. something wrong!!!\n");
            goto begining;
        }
        // if this move is allow
        if (check_move(pos_before, pos_after, player) == 1)
        {
            //upgrade the board
            //printf("pierwsze\n");
            if (board[pos_before[0]][pos_before[1]] == player * 10) upgrade_board(pos_before, pos_after, player * 10);
            else upgrade_board(pos_before, pos_after, player);

            node* beat = must_beat(player);
            if (beat != NULL)
            {
                if(pos_after[0] == beat->position_before_x && pos_after[1] == beat->position_before_y)
                {
                    //printf("drugie\n");
                    draw();
                    move(player);
                }
            }
            free_node(beat);
        }
        else if (check_move(pos_before, pos_after, player) == 2)
        {
            //printf("trzecie\n");
            if (board[pos_before[0]][pos_before[1]] == player * 10) player *= 10;
            upgrade_board(pos_before, pos_after, player);
        }
        else
        {
            printf("2. something wrong!!!\n");
            goto begining;
        }
        printf("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n");
    }
    else
    {
        REKSIO_move(player);
    }
    return;
}

int check_win(int player)
{
    for (int i = 0; i < 8; i++)
    {
        for (int y = 0; y < 8; y++)
        {
            if (board[i][y] == player || board[i][y] == (player * 10))
            {
                int board2[8][8];
                memcpy(board2, board, sizeof(board2));
                if (player_have_valid_move(player, i, y, board2))
                    return 0;
            }
        }
    }
    return 1;
}

void game_status(int status)
{
    switch (status)
    {
        case 1:
            if (PLAYER == 1)
                printf("YOU WON!!!\n");
            else
                printf("YOU LOSE HAHAHAHA!!!");
            break;
        case 2:
            if (PLAYER == 2)
                printf("YOU WON!!!\n");
            else
                printf("YOU LOSE HAHAHAHA!!!");
            break;
        case 3:
            printf("DRAW, FAIR GAME!!!");
            break;
    }
    return;
}

int check_move(int* before, int* after, int player)
{
    int moves_counter = 0;
    node* new_beat = must_beat(player);
    if(new_beat != NULL)
    {
        //check if this move is a beat move; and if yes return 1
        for(/*until there is a node with *null*/node* tmp = new_beat; tmp!= NULL; tmp = tmp->next)
        {
            if(before[0] == tmp->position_before_x && before[1] == tmp->position_before_y)
            {
                if(after[0] == tmp->position_after_x && after[1] == tmp->position_after_y)
                {
                    moves_counter++;
                }
            }
        }
        if (moves_counter > 0)
        {
            free_node(new_beat);
            return 1;
        }
        else
        {
            free_node(new_beat);
            return 0;
        }
    }
    //check if the move is correct
    if (board[before[0]][before[1]] == player)
    {
        //printf("elo\n");
        if (board[after[0]][after[1]] == 0)
        {
            //printf("elo\n");
            if (after[0] - before[0] == -1)
            {
                //printf("elo\n");
                if (after[1] - before[1] == 1 || after[1] - before[1] == -1)
                {
                    //printf("dane: board[%i][%i] = %i (player): %i\n",before[0], before[1], board[before[0]][before[1]], player);
                    return 2;
                }
            }
        }
    }
    else if (board[before[0]][before[1]] == player * 10)
    {
        int vector_x = after[0] - before[0];
        int vector_y = after[1] - before[1];
        if (vector_x == 0 || vector_y == 0) return 0;
        for (int i = (vector_x / abs(vector_x)), y = (vector_y / abs(vector_y)); abs(i) <= abs(vector_x) || abs(y) <= abs(vector_y); i += (vector_x / abs(vector_x)), y += (vector_y / abs(vector_y)))
        {
            if (board[before[0] + i][before[1] + y] != 0)
            {
                return 0;
            }
        }
        if (vector_x != 0 && vector_y != 0 && abs(vector_x) == abs(vector_y)) return 2;
    }
        //printf("dane: board[%i][%i] = %i (player): %i\n",before[0], before[1], board[before[0]][before[1]], player);
        return 0;

}

node* must_beat(int player)
{
    node* beat = NULL;
    node* beat_quin = NULL;
    int choises = 0;
    int ch_quin = 0;
    int vector_beat[2] = {0};
    int b = 0;
    int v = 0;
    for (int i = 0; i < 8; i++)
    {
        for (int y = 0; y < 8; y++)
        {
            if (board[i][y] == player * 10)
            {
                b = y;
                for (int a = i; a < 7; a++) // to do: zronic gdy jest królówka
                {
                    b++;
                    if (b > 7) break;
                    if (board[a + 1][b] != player && board[a + 1][b] != player * 10)
                    {
                        if (board[a + 1][b] == 0)
                        {}
                        else
                        {
                            if (board[a+2][b+1] == player || board[a+2][b+1] == player * 10) break;
                            v = b;
                            for(int c = a + 2; c < 8; c++)
                            {
                                v++;
                                if(v > 7) break;
                                if (board[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto c4;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                c4:;
                b = y;
                for (int a = i; a < 7; a++) // to do: zronic gdy jest królówka
                {
                    b--;
                    if (b < 1) break;
                    if (board[a + 1][b] != player && board[a + 1][b] != player * 10)
                    {
                        if (board[a + 1][b] == 0)
                        {}
                        else
                        {
                            if (board[a+2][b-1] == player || board[a+2][b-1] == player * 10) break;
                            v = b;
                            for(int c = a + 2; c < 8; c++)
                            {
                                v--;
                                if(v < 0) break;
                                if (board[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto c3;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                c3:;
                b = y;
                for (int a = i; a > 0; a--) // to do: zronic gdy jest królówka
                {
                    b++;
                    if (b > 7) break;
                    if (board[a - 1][b] != player && board[a - 1][b] != player * 10)
                    {
                        if (board[a - 1][b] == 0)
                        {}
                        else
                        {
                            if (board[a-1][b+1] == player || board[a-1][b+1] == player * 10) break;
                            v = b;
                            for(int c = a - 2; c >= 0; c--)
                            {
                                v++;
                                if(v > 7) break;
                                if (board[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto c2;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                c2:;
                b = y;
                for (int a = i; a > 0; a--) // to do: zronic gdy jest królówka
                {
                    b--;
                    if (b < 1) break;
                    if (board[a - 1][b] != player && board[a - 1][b] != player * 10)
                    {
                        if (board[a - 1][b] == 0)
                        {}
                        else
                        {
                            if (board[a-1][b-1] == player || board[a-1][b-1] == player * 10) break;
                            v = b;
                            for(int c = a - 2; c >= 0; c--)
                            {
                                v--;
                                if(v < 0) break;
                                if (board[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto c1;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                c1:;
                if (beat_quin != NULL)
                    return beat_quin;
            }
            else if (board[i][y] == player)
            {
                for (int u = (i - 1); u <= (i + 1); u++)
                {
                    for (int t = (y - 1); t <= (y + 1); t++)
                    {
                        if (u > 0 && u < 7 && t > 0 && t < 7)
                        {
                            if (board[u][t] != player && board[u][t] != 0 && board[u][t] != player * 10) // if there is oponent near to you
                            {
                                vector_beat[0] = u - i;
                                vector_beat[1] = t - y;
                                // if there is a place to put the pawn
                                if (u + vector_beat[0] >= 0 && u + vector_beat[0] <= 7 && t + vector_beat[1] >= 0 && t + vector_beat[1] <= 7 )
                                {
                                    if (board[u + vector_beat[0]][t + vector_beat[1]] == 0)
                                    {
                                        //add next chois you have
                                        choises++;
                                        //-----------------------> to do
                                        // save this move as a next node;
                                        if (choises == 1)
                                        {
                                            node* n = malloc(sizeof(node));
                                            n->position_after_x = u + vector_beat[0];
                                            n->position_after_y = t + vector_beat[1];
                                            n->position_before_x = i;
                                            n->position_before_y= y;
                                            n->next = NULL;
                                            beat = n;
                                            //free(n);
                                        }
                                        else
                                        {
                                            node* n = malloc(sizeof(node));
                                            n->position_after_x = u + vector_beat[0];
                                            n->position_after_y = t + vector_beat[1];
                                            n->position_before_x = i;
                                            n->position_before_y= y;
                                            n->next = beat->next;
                                            beat->next = n;
                                            //free(n);
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }
    return beat;
}

void free_node(node* head)
{
   node* tmp;

   while (head != NULL)
    {
       tmp = head;
       head = head->next;
       free(tmp);
    }

}

void upgrade_board(int* before, int* after, int color)
{
    int x;
    int y;
    board[after[0]][after[1]] = color;
    board[before[0]][before[1]] = 0;
    if (abs(after[0] - before[0]) == 2)
    {
        board[before[0] + ((after[0] - before[0]) / 2)][before[1] + ((after[1]-before[1]) / 2)] = 0;
        switch (color)
        {
            case 1:
                pawns[2]--;
                break;
            case 2:
                pawns[1]--;
                break;
            case 10:
                pawns[2]--;
                break;
            case 20:
                pawns[1]--;
                break;
        }
    }
    else if (abs(after[0] - before[0]) > 2)
    {
        after[0] - before[0] > 0 ? x = 1 : (x = -1);
        after[1] - before[1] > 0 ? y = 1 : (y = -1);
        for(int i = before[0]; i < before[0] + abs(after[0] - before[0]); i++)
        {
            if (board[before[0] + x][before[1] + y] != color && board[before[0] + x][before[1] + y] != 0 && board[before[0] + x][before[1] + y] != color * 10)
            {
                board[before[0] + x][before[1] + y] = 0;
            }
            x > 0 ? x++ : x--;
            y > 0 ? y++ : y--;
        }
    }
    return;
}

void upgrade_to_quin(void)
{
    for (int i = 0; i < 8; i++)
    {
        if (board[0][i] == PLAYER)
        {
            board[0][i] = PLAYER_quin;
        }
        if (board[7][i] == REKSIO)
        {
            board[7][i] = REKSIO_quin;
        }
    }
}

void print_board(void)
{
    for (int i = 0; i < 8; i++)
    {
        for (int y = 0; y < 8; y++)
        {
            printf("%i  ", board[i][y]);
        }
        printf("\n\n");
    }
}
/*

##############  #############  ###        ###
###        ###  ###            ###      ###
###        ###  ###            ###     ###
###        ###  ###            ###    ###
###        ###  ###            ###  ###
##############  #############  ########
###    ###      ###            ###    ###
###     ###     ###            ###     ###
###      ###    ###            ###      ###
###       ###   ###            ###       ###
###        ###  #############  ###        ###

*/

void REKSIO_move(int player)
{
    int test_board[8][8];
    pos best_move;
    //int ways = posible_moves(player);
    tree begin;
    memcpy(test_board, board, sizeof(test_board));
    begin = symulate(player, test_board, 7);
    memcpy(board, begin.board, sizeof(board));
    //printf("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n");
    printf("value: %f\n", begin.value);
    //printf("last move: %c%i %c%i\n", begin.position_before_y + 'A', begin.position_before_x, begin.position_after_y + 'A', begin.position_after_x);
    //draw_board(begin.board);
    //printf("%f\n", board_value(begin.board, player));

    return ;
}

// dla każdego ruchu oblicz min lub  max i na końcu wraca spowrotem rekurencja

tree symulate(int player,int test_board[8][8], int times)
{
    int leg = 0;
    int more_board[8][8];
    int reset_board[8][8];
    tree ret;
    memcpy(more_board, test_board, sizeof(more_board));
    memcpy(reset_board, test_board, sizeof(reset_board));
    tree branch;
    for(int i = 0; i < 8; i++)
    {
        for (int y = 0; y < 8; y++)
        {
            if (more_board[i][y] == player || more_board[i][y] == player * 10)
            {
                pos last_move;
                last_move.position_before_x = 0;
                last_move.position_before_y = 0;
                last_move.position_after_x = 0;
                last_move.position_after_y = 0;
                int phvm = player_have_valid_move(player, i, y, more_board);
                for (int a = 0; a < phvm; a++)
                {
                    pos more_move = next_move(more_board, player, i, y, last_move);
                    last_move = more_move;
                    int pos_before[2]; pos_before[0] = i; pos_before[1] = y;
                    int pos_after[2]; pos_after[0] = last_move.position_after_x; pos_after[1] = last_move.position_after_y;
                    int pos_plus[2]; pos_plus[0] = more_move.position_before_x; pos_plus[1] = more_move.position_before_y;
                    if (more_board[more_move.position_before_x][more_move.position_before_y] == (player * 10)) upgrade(pos_plus, pos_after, (player * 10), more_board);
                    else upgrade(pos_plus, pos_after, player, more_board);  //01 12
                    upgrade_to_quin_board(more_board);
                    //printf("co jest aslkcasdjkncajskd\n %i %i, %i %i", pos_before[0], pos_before[1], pos_after[0], pos_after[1]);
                    //draw_board(more_board);
                    //printf("%f\n", board_value(more_board, player));
                    branch.position_before_x = pos_before[0];
                    branch.position_before_y = pos_before[1];
                    branch.position_after_x = pos_after[0];
                    branch.position_after_y = pos_after[1];
                    memcpy(branch.board, more_board, sizeof(branch.board));
                    if (check_times(more_board, player)) times = 0;
                    if (times == 0)
                    {
                        if ( leg == 0)
                        {
                            branch.value = board_value(more_board, player);
                            ret = branch;
                            //draw_board(more_board);
                            //printf("%i\n", branch.value);
                        }
                        if (player == PLAYER)
                        {
                            if (board_value(more_board, player) > branch.value)
                            {
                                branch.value = board_value(more_board, player);
                                ret = branch;
                                //draw_board(more_board);
                                //printf("%i\n", branch.value);
                            }
                        }
                        else
                        {
                            if (board_value(more_board, player) < branch.value)
                            {
                                branch.value = board_value(more_board, player);
                                ret = branch;
                                //draw_board(more_board);
                                //printf("%i\n", branch.value);
                            }
                        }
                        //return branch;
                    }
                    else
                    {
                        player = change_player(player);
                        tree spk = symulate(player, more_board, (times - 1));
                        branch.value = spk.value;
                        //draw_board(more_board);
                        //printf("%i\n", branch.value);
                        if (leg == 0)
                            ret = branch;
                        else
                        {
                            if (player == PLAYER)
                            {
                                if (branch.value > ret.value)
                                {
                                    ret = branch;
                                    //draw_board(more_board);
                                    //printf("%i\n", branch.value);
                                }
                            }
                            else
                            {
                                if (branch.value < ret.value)
                                {
                                    ret = branch;
                                    //draw_board(more_board);
                                   // printf("%i\n", branch.value);
                                }
                            }
                        }
                        player = change_player(player);
                    }
                    leg++;
                    memcpy(more_board, reset_board, sizeof(more_board));
                    //draw_board(more_board);
                }
            }
        }
    }
    return ret;
}
/*
                            _________________________0________________________
                           /                                                  \
               ___________0____________                             ___________1____________
              /                        \                           /                        \
      _______0_______            _______1_______           _______0_______            _______1_______
     /       |       \          /       |       \         /       |       \          /       |       \
    0        1        2        0        1        2       0        1        2        0        1        2
  / | \    / | \    / | \    / | \    / | \    / | \   / | \    / | \    / | \    / | \    / | \    / | \
 0  1  2  0  1  2  0  1  2  0  1  2  0  1  2  0  1  2 0  1  2  0  1  2  0  1  2  0  1  2  0  1  2  0  1  2
 */

void upgrade_to_quin_board(int boardw[8][8])
{
    for (int i = 0; i < 8; i++)
    {
        if (boardw[0][i] == PLAYER)
        {
            boardw[0][i] = PLAYER_quin;
        }
        if (boardw[7][i] == REKSIO)
        {
            boardw[7][i] = REKSIO_quin;
        }
    }
}

int check_times(int board_times[8][8], int player)
{
    int count = 0;
    for (int i = 0; i < 8; i++)
    {
        for (int j = 0; j < 8; j++)
        {
            if (board_times[i][j] != player && board_times[i][j] != (player * 10) && board_times[i][j] != 0)
            {
                if (player_have_valid_move(change_player(player), i, j, board_times) > 0)
                    return 0;
            }
        }
    }
    return 1;
}

void back_move(tree move, int board2[8][8])
{
    board2[move.position_before_x][move.position_before_y] = board2[move.position_after_x][move.position_after_y];
    board2[move.position_before_x][move.position_before_y] = 0;
}

int player_have_valid_move(int player, int i, int y, int boardv2[8][8])
{
    int counter = 0;
    int pos_before[2]; pos_before[0] = i; pos_before[1] = y;
    int pos_after[2];
    for (int a = 0; a < 8; a++)
    {
        if (boardv2[i][y] == player)
            {
                if (abs(i - a <= 2))
                {
                    pos_after[0] = a;
                    for (int b = 0; b < 8; b += 2)
                    {
                        if (b == 0)
                            a % 2 == 0 ? b = 1 : (b = 0);
                        if (abs(y - b <= 2))
                        {
                            pos_after[1] = b;
                            if (boardv2[a][b] == 0)
                            {
                                if (check_movev2(pos_before, pos_after, player, boardv2))
                                {
                                    counter++;
                                }
                            }
                        }
                    }
                }
            }
            else
            {
                pos_after[0] = a;
                    for (int b = 0; b < 8; b += 2)
                    {
                        if (b == 0)
                            a % 2 == 0 ? b = 1 : (b = 0);
                        pos_after[1] = b;
                            if (boardv2[a][b] == 0)
                            {
                                if (check_movev2(pos_before, pos_after, player, boardv2))
                                {
                                    counter++;
                                }
                            }
                    }
            }
    }
    return counter;
}

int change_player(int player)
{
    switch (player)
    {
        case 1:
        return 2;

        case 2:
        return 1;

        default:
        return 0;
    }
}

pos next_move(int more_board[8][8], int player, int i, int y, pos last_move)
{
    //printf("sdajcvflvnvjfjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjjj\n");
    pos move;
    int pos_before[2]; pos_before[0] = i; pos_before[1] = y;
    int pos_after[2];
    pos multi;
    multi.position_before_x = 0;
    multi.position_before_y = 0;
    multi.position_after_x = 0;
    multi.position_after_y = 0;
    move.position_before_x = 0;
    move.position_before_y = 0;
    move.position_after_x = 0;
    move.position_after_y = 0;
    int reset_board[8][8];
    memcpy(reset_board, more_board, sizeof(reset_board));
    int lopo;
    int c = 0;
    for (int a = last_move.position_after_x; a < 8; a++)
    {
        if (more_board[i][y] == player)
            {
                if (abs(i - a <= 2))
                {
                    pos_after[0] = a;
                    for (int b = last_move.position_after_y + 1; b < 8; b++)
                    {
                        if (abs(y - b <= 2))
                        {
                            pos_after[1] = b;
                            if (more_board[a][b] == 0)
                            {
                                lopo = check_movev2(pos_before, pos_after, player, more_board);
                                if (lopo == 1)
                                {
                                    move.position_before_x = pos_before[0];
                                    move.position_before_y = pos_before[1];
                                    move.position_after_x = pos_after[0];
                                    move.position_after_y = pos_after[1];
                                    if (more_board[pos_before[0]][pos_before[1]] == (player * 10)) upgrade(pos_before, pos_after, (player * 10), reset_board);
                                    else upgrade(pos_before, pos_after, player, reset_board);
                                    //printf("reset_board\n");
                                    //draw_board(reset_board);
                                    node* beat = must_beatv2(player, reset_board);
                                    if (beat != NULL)
                                    {
                                        if (pos_after[0] == beat->position_before_x && pos_after[1] == beat->position_before_y)
                                        {
                                            if (more_board[pos_before[0]][pos_before[1]] == (player * 10)) upgrade(pos_before, pos_after, (player * 10), more_board);
                                            else upgrade(pos_before, pos_after, player, more_board);
                                            //printf("more_board\n");
                                            //draw_board(more_board);
                                            move = next_move(more_board, player, move.position_after_x, move.position_after_y, multi);
                                        }
                                        else goto p1;
                                    }
                                    free_node(beat);
                                    return move;
                                }
                                else if (lopo)
                                {
                                // printf("no tak jak myslałem\n");
                                    move.position_before_x = pos_before[0];
                                    move.position_before_y = pos_before[1];
                                    move.position_after_x = pos_after[0];
                                    move.position_after_y = pos_after[1];
                                    return move;
                                }
                                p1:;
                            }
                        }
                    }
                }
            }
            else
            {
                pos_after[0] = a;
                    for (int b = last_move.position_after_y + 1; b < 8; b++)
                    {
                            pos_after[1] = b;
                            if (more_board[a][b] == 0)
                            {
                                lopo = check_movev2(pos_before, pos_after, player, more_board);
                                if (lopo == 1)
                                {
                                    move.position_before_x = pos_before[0];
                                    move.position_before_y = pos_before[1];
                                    move.position_after_x = pos_after[0];
                                    move.position_after_y = pos_after[1];
                                    if (more_board[pos_before[0]][pos_before[1]] == (player * 10)) upgrade(pos_before, pos_after, (player * 10), reset_board);
                                    else upgrade(pos_before, pos_after, player, reset_board);
                                    //printf("reset_board\n");
                                    //draw_board(reset_board);
                                    node* beat = must_beatv2(player, reset_board);
                                    if (beat != NULL)
                                    {
                                        if (pos_after[0] == beat->position_before_x && pos_after[1] == beat->position_before_y)
                                        {
                                            if (more_board[pos_before[0]][pos_before[1]] == (player * 10)) upgrade(pos_before, pos_after, (player * 10), more_board);
                                            else upgrade(pos_before, pos_after, player, more_board);
                                            //printf("more_board\n");
                                            //draw_board(more_board);
                                            move = next_move(more_board, player, move.position_after_x, move.position_after_y, multi);
                                        }
                                        else goto p2;
                                    }
                                    free_node(beat);
                                    return move;
                                }
                                else if (lopo)
                                {
                                // printf("no tak jak myslałem\n");
                                    move.position_before_x = pos_before[0];
                                    move.position_before_y = pos_before[1];
                                    move.position_after_x = pos_after[0];
                                    move.position_after_y = pos_after[1];
                                    return move;
                                }
                                p2:;
                            }
                    }
            }
        last_move.position_after_y = -1;
    }
    return move;
}

int check_movev2(int* before, int* after, int player, int boardv2[8][8])
{
    int moves_counter = 0;
    node* new_beat = must_beatv2(player, boardv2);
    if(new_beat != NULL)
    {
        //check if this move is a beat move; and if yes return 1
        for(/*until there is a node with *null*/node* tmp = new_beat; tmp!= NULL; tmp = tmp->next)
        {
            if(before[0] == tmp->position_before_x && before[1] == tmp->position_before_y)
            {
                if(after[0] == tmp->position_after_x && after[1] == tmp->position_after_y)
                {
                    moves_counter++;
                }
            }
        }
        if (moves_counter > 0)
        {
            free_node(new_beat);
            return 1;
        }
        else
        {
            free_node(new_beat);
            return 0;
        }
    }
    //check if the move is correct
    if (boardv2[before[0]][before[1]] == player)
    {
        //printf("elo\n");
        if (boardv2[after[0]][after[1]] == 0)
        {
            //printf("elo\n");
            if (player == PLAYER)
            {
                if (after[0] - before[0] == -1)
                {
                    //printf("elo\n");
                    if (after[1] - before[1] == 1 || after[1] - before[1] == -1)
                    {
                        //printf("dane: board[%i][%i] = %i (player): %i\n",before[0], before[1], board[before[0]][before[1]], player);
                        return 2;
                    }
                }
            }
            else
            {
                if (after[0] - before[0] == 1)
                {
                    //printf("elo\n");
                    if (after[1] - before[1] == 1 || after[1] - before[1] == -1)
                    {
                        //printf("dane: board[%i][%i] = %i (player): %i\n",before[0], before[1], board[before[0]][before[1]], player);
                        return 2;
                    }
                }
            }
        }
    }
    else if (boardv2[before[0]][before[1]] == player * 10)
    {
        int vector_x = after[0] - before[0];
        int vector_y = after[1] - before[1];
        if (vector_x == 0 || vector_y == 0) return 0;
        for (int i = (vector_x / abs(vector_x)), y = (vector_y / abs(vector_y)); abs(i) <= abs(vector_x) || abs(y) <= abs(vector_y); i += (vector_x / abs(vector_x)), y += (vector_y / abs(vector_y)))
        {
            if (board[before[0] + i][before[1] + y] != 0)
            {
                return 0;
            }
        }
        if (vector_x != 0 && vector_y != 0 && abs(vector_x) == abs(vector_y)) return 2;
    }
        //printf("dane: board[%i][%i] = %i (player): %i\n",before[0], before[1], board[before[0]][before[1]], player);
        return 0;

}

node* must_beatv2(int player, int boardv2[8][8])
{
    node* beat = NULL;
    node* beat_quin = NULL;
    int choises = 0;
    int ch_quin = 0;
    int vector_beat[2] = {0};
    int b = 0;
    int v = 0;
    for (int i = 0; i < 8; i++)
    {
        for (int y = 0; y < 8; y++)
        {
            if (boardv2[i][y] == player * 10)
            {
                b = y;
                for (int a = i; a < 7; a++) // to do: zronic gdy jest królówka
                {
                    b++;
                    if (b > 7) break;
                    if (boardv2[a + 1][b] != player && boardv2[a + 1][b] != player * 10)
                    {
                        if (boardv2[a + 1][b] == 0)
                        {}
                        else
                        {
                            if (board[a+2][b+1] == player || board[a+2][b+1] == player * 10) break;
                            v = b;
                            for(int c = a + 2; c < 8; c++)
                            {
                                v++;
                                if(v > 7) break;
                                if (boardv2[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto b1;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                b1:;
                b = y;
                for (int a = i; a < 7; a++) // to do: zronic gdy jest królówka
                {
                    b--;
                    if (b < 1) break;
                    if (boardv2[a + 1][b] != player && boardv2[a + 1][b] != player * 10)
                    {
                        if (boardv2[a + 1][b] == 0)
                        {}
                        else
                        {
                            if (board[a+2][b-1] == player || board[a+2][b-1] == player * 10) break;
                            v = b;
                            for(int c = a + 2; c < 8; c++)
                            {
                                v--;
                                if(v < 0) break;
                                if (boardv2[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto b2;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                b2:;
                b = y;
                for (int a = i; a > 0; a--) // to do: zronic gdy jest królówka
                {
                    b++;
                    if (b > 7) break;
                    if (boardv2[a - 1][b] != player && boardv2[a - 1][b] != player * 10)
                    {
                        if (boardv2[a - 1][b] == 0)
                        {}
                        else
                        {
                            if (board[a-1][b+1] == player || board[a-1][b+1] == player * 10) break;
                            v = b;
                            for(int c = a - 2; c >= 0; c--)
                            {
                                v++;
                                if(v > 7) break;
                                if (boardv2[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto b3;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                b3:;
                b = y;
                for (int a = i; a > 0; a--) // to do: zronic gdy jest królówka
                {
                    b--;
                    if (b < 1) break;
                    if (boardv2[a - 1][b] != player && boardv2[a - 1][b] != player * 10)
                    {
                        if (boardv2[a - 1][b] == 0)
                        {}
                        else
                        {
                            v = b;
                            for(int c = a - 2; c >= 0; c--)
                            {
                                v--;
                                if(v < 0) break;
                                if (boardv2[c][v] == 0)
                                {
                                    ch_quin++;
                                    if (ch_quin == 1)
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = NULL;
                                        beat_quin = n;
                                        //free(n);
                                    }
                                    else
                                    {
                                        node* n = malloc(sizeof(node));
                                        n->position_after_x = c;
                                        n->position_after_y = v;
                                        n->position_before_x = i;
                                        n->position_before_y= y;
                                        n->next = beat_quin->next;
                                        beat_quin->next = n;
                                        //free(n);
                                    }
                                }
                                else goto b4;
                            }
                            break;
                        }

                    }
                    else
                    {
                        break;
                    }
                }
                b4:;
                if (beat_quin != NULL)
                    return beat_quin;
            }
            else if (boardv2[i][y] == player)
            {
                for (int u = (i - 1); u <= (i + 1); u++)
                {
                    for (int t = (y - 1); t <= (y + 1); t++)
                    {
                        if (u > 0 && u < 7 && t > 0 && t < 7)
                        {
                            if (boardv2[u][t] != player && boardv2[u][t] != 0 && boardv2[u][t] != player * 10) // if there is oponent near to you
                            {
                                vector_beat[0] = u - i;
                                vector_beat[1] = t - y;
                                // if there is a place to put the pawn
                                if (u + vector_beat[0] >= 0 && u + vector_beat[0] <= 7 && t + vector_beat[1] >= 0 && t + vector_beat[1] <= 7 )
                                {
                                    if (boardv2[u + vector_beat[0]][t + vector_beat[1]] == 0)
                                    {
                                        //add next chois you have
                                        choises++;
                                        //-----------------------> to do
                                        // save this move as a next node;
                                        if (choises == 1)
                                        {
                                            node* n = malloc(sizeof(node));
                                            n->position_after_x = u + vector_beat[0];
                                            n->position_after_y = t + vector_beat[1];
                                            n->position_before_x = i;
                                            n->position_before_y= y;
                                            n->next = NULL;
                                            beat = n;
                                            //free(n);
                                        }
                                        else
                                        {
                                            node* n = malloc(sizeof(node));
                                            n->position_after_x = u + vector_beat[0];
                                            n->position_after_y = t + vector_beat[1];
                                            n->position_before_x = i;
                                            n->position_before_y= y;
                                            n->next = beat->next;
                                            beat->next = n;
                                            //free(n);
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }
    return beat;
}

void upgrade(int* before, int* after, int color, int boardv2[8][8])
{
    int x;
    int y;
    //printf("%i %i\n", after[0], after[1]);
    boardv2[after[0]][after[1]] = color;
    boardv2[before[0]][before[1]] = 0;
    if (abs(after[0] - before[0]) == 2)
    {
        boardv2[before[0] + ((after[0] - before[0]) / 2)][before[1] + ((after[1]-before[1]) / 2)] = 0;
    }
    else if (abs(after[0] - before[0]) > 2)
    {
        after[0] - before[0] > 0 ? x = 1 : (x = -1);
        after[1] - before[1] > 0 ? y = 1 : (y = -1);
        for(int i = before[0]; i < before[0] + abs(after[0] - before[0]); i++)
        {
            if (boardv2[before[0] + x][before[1] + y] != color && boardv2[before[0] + x][before[1] + y] != 0 && boardv2[before[0] + x][before[1] + y] != color * 10)
            {
                boardv2[before[0] + x][before[1] + y] = 0;
            }
            x > 0 ? x++ : x--;
            y > 0 ? y++ : y--;
        }
    }
    return;
}

float board_value(int more_board[8][8], int player)
{
    float PLAYER_pawns = 0;
    float REKSIO_pawns = 0;
    for (int i = 0; i < 8; i++)
    {
        for (int y = 0; y < 8; y++)
        {
                if (more_board[i][y] == PLAYER)
                {
                    PLAYER_pawns += 1 + ((7 - i) * 0.01);
                }
                else if (more_board[i][y] == (PLAYER * 10))
                {
                    PLAYER_pawns += 1.1;
                }
                else if (more_board[i][y] == REKSIO)
                {
                    REKSIO_pawns += 1 + ((i) * 0.01);
                }
                else if (more_board[i][y] == (REKSIO * 10))
                {
                    REKSIO_pawns += 1.1;
                }
                //printf("wynik: %f\n", (REKSIO_pawns - PLAYER_pawns));
        }
    }
    if (PLAYER_pawns == 0) REKSIO_pawns = 1000; 
    return (REKSIO_pawns - PLAYER_pawns);
}

void draw_board(int board2[8][8])
{
    //SetConsoleTextAttribute (GetStdHandle(STD_OUTPUT_HANDLE), BACKGROUND_BLUE);
    printf("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n");
    for (int i = 0; i < 25; i++)
    {
        for (int j = 0; j < 9; j++)
        {
            if (j == 8)
            {
                i == 0 ? printf(" ") : printf("|");
            }
            else
            {
                if (i == 0) printf("_______");
                else if (i % 3 == 0) printf("|______");
                else if (i % 3 == 1)
                {
                    printf("|%c%i", ((j) + 'A'), i/3);
                    if (board2[(i - 1)/ 3][j] == 2 ) printf(GRN   "__  "   RESET);
                    else if (board2[(i - 1)/ 3][j] == 1 ) printf(BLU   "__  "   RESET);
                    else if (board2[(i - 1)/ 3][j] == 20 ) printf(GRN   "__  "   RESET);
                    else if (board2[(i - 1)/ 3][j] == 10 ) printf(BLU   "__  "   RESET);
                    else printf("    ");
                }
                else if (i % 3 == 2)
                {
                    printf("| ");
                    if (board2[(i - 1)/ 3][j] == 2 ) printf(GRN   "|__| "   RESET);
                    else if (board2[(i - 1)/ 3][j] == 1 ) printf(BLU   "|__| "   RESET);
                    else if (board2[(i - 1)/ 3][j] == 20 ) printf(GRN   "|$$| "   RESET);
                    else if (board2[(i - 1)/ 3][j] == 10 ) printf(BLU   "|$$| "   RESET);
                    else printf("     ");
                }
            }
        }
        printf("\n");
    }
    return;
}
