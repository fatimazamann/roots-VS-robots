# roots-VS-robots
#include <stdio.h>
#include <stdlib.h>
int main()
{
#define row 3
#define col 9
    int grid[row][col] = {0};

    // lanes
    while (1)
    {
        system("clear");

        for (int i = 0; i < row; i++)
        {
            if (i > 0)
                printf("------------------------------------------------------------------\n");
            for (int j = 0; j < col; j++)
            {
                printf("   ");
                printf("%d", grid[i][j]);
            }
            printf("\n");
        }
        int lane, position;
        printf("enter plant lane: ");
        scanf("%d", &lane);

        if (lane == -1)
        {
            break;
        }

        printf("enter plant position in lane: ");
        scanf("%d", &position);

        if ((lane >= 0 && lane <= row) && (position >= 0 && position <= col))
        {

            grid[lane][position] = 1;
        }

        else
        {
            printf("invalid entry\n");
        }
    }
    return 0;
}
