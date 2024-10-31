#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <stdbool.h>

#define ROW 5 // define doesnt take up any memory, is more efficient, and since no input needed so variables r not needed.
#define COL 9 // define is before int main, its a part of the pre processor directives, it should compile before any code does and it prevents error

int main()
{
    char grid[ROW][COL];
    int pea_positions[ROW][COL] = {0}; // Track pea positions separately // its an int array thats why initialised to 0

    for (int i = 0; i < ROW; i++)
    {
        for (int j = 0; j < COL; j++)
        {
            grid[i][j] = ' '; // initialise the grid with space to avoid garbage value
        }
    }

    bool play = false; // the game remains in setup mode unless a command r is entered to play

    while (true) // to keep the loop running unless terminated
    {
        system("clear"); // this clears or updates the terminal everytime an action occurs for example adding a plant
                         // for windows use system("cls")

        // Print the lanes
        for (int i = 0; i < ROW; i++)
        {
            if (i > 0)
                printf("------------------------------------------------------------------\n");
            for (int j = 0; j < COL; j++)
            {
                printf("   ");
                printf("%c", grid[i][j]);
            }
            printf("\n");
        }

        // Move pea (bullet)
        for (int i = 0; i < ROW; i++)
        {
            for (int j = COL - 1; j >= 0; j--) // j-- is used to avoid rewriting and clearing the passage for the pea to travel freely
            {
                if (pea_positions[i][j] == 1)
                {
                    pea_positions[i][j] = 0;
                    if (j + 1 < COL)
                    {
                        pea_positions[i][j + 1] = 1;
                    }
                    else
                    {
                        pea_positions[i][j] = 0;
                    }
                }
            }
        }

        // display pea (bullet)
        for (int i = 0; i < ROW; i++)
        {
            for (int j = 0; j < COL; j++) // checking that if a plant is visible in the input row from the left
            {
                if (pea_positions[i][j] == 1)
                {
                    grid[i][j] = 'o';
                }
                else if (grid[i][j] != 'P')
                {
                    grid[i][j] = ' '; // Clear cell if not a plant
                }
            }
        }

        // Check for user input to place a new plant
        if (play == false)
        {

            char input[10];
            printf("Enter plant command (e.g., p12 for plant in lane 1, position 2) or 'q' to quit: ");
            fgets(input, sizeof(input), stdin);

            // Exit the loop if the user enters 'q'
            if (input[0] == 'q')
            {
                break;
            }
            else if (input[0] == 'r')
            {
                play = true;
            }
            // Place a plant if the input is valid
            else if (input[0] == 'p')
            {
                int lane = input[1] - '0';     // Convert character to integer for lane
                int position = input[2] - '0'; // Convert character to integer for position

                // Validate lane and position
                if (lane >= 1 && lane <= ROW && position >= 0 && position < COL)
                {
                    grid[lane - 1][position] = 'P'; // Place 'P' for plant lane -1 to avoid the confusion as it started from 0

                    pea_positions[lane - 1][position + 1] = 1; // Initial pea right next to plant addition in y axis
                }
                else
                {
                    printf("Invalid lane or position\n");
                    return 0;
                }
            }
            else
            {
                printf("Invalid format. Use 'p' followed by lane and position (e.g., p12)\n");

                return 0;
            }
        }
        usleep(300000); // Delay for easier visualization (300 milliseconds)
    }

    return 0;
}
