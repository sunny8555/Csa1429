#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

#define MAX_RULES 10
#define MAX_SYMBOLS 10

char grammar[MAX_RULES][MAX_SYMBOLS];
int numRules;

void leftFactor(char);

int main() {
    printf("Enter the number of rules: ");
    scanf("%d", &numRules);

    printf("Enter the grammar rules:\n");
    for (int i = 0; i < numRules; i++) {
        printf("Rule %d: ", i + 1);
        scanf("%s", grammar[i]);
    }

    leftFactor('S'); // Apply left factoring to 'S'

    printf("\nGrammar after left factoring:\n");
    for (int i = 0; i < numRules; i++) {
        printf("Rule %d: %s\n", i + 1, grammar[i]);
    }

    return 0;
}

void leftFactor(char nonTerminal) {
    int count = 0;
    char commonPrefix[MAX_SYMBOLS];

    for (int i = 0; i < numRules; i++) {
        if (grammar[i][0] == nonTerminal) {
            if (count == 0) {
                strcpy(commonPrefix, grammar[i] + 1); // Copy the suffix of the first rule
            } else {
                int j;
                for (j = 1; j < MAX_SYMBOLS; j++) {
                    if (commonPrefix[j - 1] == '\0' || grammar[i][j] == '\0' || commonPrefix[j - 1] != grammar[i][j]) {
                        break;
                    }
                }
                commonPrefix[j - 1] = '\0'; // Null terminate the common prefix
            }
            count++;
        }
    }

    if (count > 1) {
        // Apply left factoring by creating a new non-terminal
        char newNonTerminal = nonTerminal + 1; // Create a new non-terminal

        // Update the original rules
        for (int i = 0; i < numRules; i++) {
            if (grammar[i][0] == nonTerminal && strncmp(grammar[i] + 1, commonPrefix, strlen(commonPrefix)) == 0) {
                // Remove the common prefix from the rule
                memmove(grammar[i] + 1, grammar[i] + 1 + strlen(commonPrefix), MAX_SYMBOLS - 1 - strlen(commonPrefix));
                grammar[i][strlen(grammar[i])] = '\0'; // Null terminate the rule
            }
        }

        // Add a new rule for the common prefix
        sprintf(grammar[numRules], "%c%s%c", newNonTerminal, commonPrefix, nonTerminal);
        numRules++;

        // Add epsilon rule if necessary
        if (commonPrefix[0] == '\0') {
            sprintf(grammar[numRules], "%c", newNonTerminal);
            numRules++;
        }

        // Recursively apply left factoring to the new non-terminal
        leftFactor(newNonTerminal);
    }
}
