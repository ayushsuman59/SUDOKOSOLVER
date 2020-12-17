# SUDOKOSOLVER
#include <stdio.h>
#define EMPTY 0
#define SIZE 9

//A sudoku extists of a 9*9 playfield
//In order to place a number (1-9) in an empty spot there are 3 rules
//1. The number can't exist in the same row(x) it's in
//2. The number can't exist in the same column(y)
//3. The number can't exist in the same 3x3 square

void display(int grid[SIZE][SIZE]);

//Checks row
int isInRow(int grid[SIZE][SIZE], int row, int number)
{
    for(int i = 0; i < SIZE; i++)
    {
        if(grid[row][i] == number)
        {
            return 1;
        }
    }
    return 0;

}

//Checks column
int isInCol(int grid[SIZE][SIZE], int col, int number)
{
    for(int i = 0; i < SIZE; i++)
    {
        if(grid[i][col] == number)
        {
            return 1;
        }
    }
    return 0;
}

//Checks box
int isInBox(int grid[SIZE][SIZE], int row, int col, int number)
{
    int r = row - (row % 3);
    int c = col - (col % 3);

    for (int i = r; i < r + 3; i++)
    {
        for (int j = c; j < c + 3; j++)
        {
            if (grid[i][j] == number)
            {
                return 1;
            }
        }
    }
    return 0;
}

//Method that combines the methods to see if position in row coland box is ok
int isOk(int grid[SIZE][SIZE], int row, int col, int number)
{

    return !isInCol(grid, col, number) && !isInRow(grid, row, number) && !isInBox(grid, row, col, number);

}


//Method for going over the whole sudoku
int solve(int grid[SIZE][SIZE])
{

    for(int row = 0; row < SIZE; row++)
    {
        for(int col = 0; col < SIZE; col++)
        {
            if(grid[row][col] == EMPTY)
            {
                for (int number = 1; number <= SIZE; number++)
                {
                    if(isOk(grid, row, col, number))
                    {
                        grid[row][col] = number;

                        if (solve(grid))
                        {
                            return 1;
                        }
                        else
                        {
                            grid[row][col] = EMPTY;
                        }
                    }
                }
                return 0;
            }
        }
    }
    return 1;
}

//Displays solved board
void display(int grid[SIZE][SIZE])
{
    for (int row = 0; row < SIZE; row++) {
			for (int col = 0; col < SIZE; col++)
			{
				printf("%i ", grid[row][col]);
			    if(col == 2 || col == 5)
			    {
			        printf("|");
			    }
			}
		printf("\n");
        if(row == 2 || row == 5)
            {
                printf("--------------------\n");
            }
    }

}

int main (void)
{
    printf("sudoku to solve: \n\n");

    int grid[SIZE][SIZE] =
    {
		{9,0,0,1,0,0,0,0,5},
		{0,0,5,0,9,0,2,0,1},
		{8,0,0,0,4,0,0,0,0},
		{0,0,0,0,8,0,0,0,0},
		{0,0,0,7,0,0,0,0,0},
		{0,0,0,0,2,6,0,0,9},
		{2,0,0,3,0,0,0,0,6},
		{0,0,0,2,0,0,9,0,0},
		{0,0,1,9,0,4,5,7,0}
    };
    display(grid);
    printf("\n\n");

    if(solve(grid))
    {
        printf("sudoku solved: \n\n");
        display(grid);
    }
    else
    {
        printf("unsolvable! \n");
        display(grid);
    }


}
