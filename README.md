```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
//#include "function.h"
//function.h
#define MAXEXPR 256
#define NUMSYM 6

char expr[MAXEXPR];  // string to store the input expression.
int pos;             // current position of parsing, from end to start

typedef enum {ID_A, ID_B, ID_C, ID_D, OP_AND, OP_OR} TokenSet;
char sym[NUMSYM];

typedef struct _Node {
    TokenSet data;
    struct _Node *left, *right;
} BTNode;

BTNode* EXPR();
BTNode* FACTOR();
BTNode* makeNode(char c);
void freeTree(BTNode *root);
void printPrefix(BTNode *root);
//main.c
int main(void){
    sym[0]='A';
    sym[1]='B';
    sym[2]='C';
    sym[3]='D';
    sym[4]='&';
    sym[5]='|';
    while (scanf("%s", expr)!=EOF)
    {
        pos = strlen(expr) - 1;
        BTNode *root = EXPR();
        printPrefix(root);
        printf("\n");
        freeTree(root);
    }

    return 0;
}

/* print a tree by pre-order. */
void printPrefix(BTNode *root){
    if (root != NULL) {
        printf("%c",sym[root->data]);
        printPrefix(root->left);
        printPrefix(root->right);
    }
}

/* clean a tree.*/
void freeTree(BTNode *root){
    if (root!=NULL) {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}

//function.c
BTNode* EXPR()
{
    char c;
    BTNode *node = NULL, *right = NULL;
    if(pos >= 0){
        right = FACTOR();

        if(pos > 0){
            c = expr[pos];
            //printf("pos = %d\n", pos);
            if(c == '&' || c == '|'){
                node = makeNode(c);
                node->right = right;
                pos--;
                node->left = EXPR();
            }
            else if(c == '('){
                node = right;
                pos--;
            }
            else node = right;
        }
        else node = right;
    }
    return node;
}
BTNode* FACTOR()
{
    char c;
    BTNode *node = NULL;
    //printf("In factor: ");
    if(pos >= 0){
        c = expr[pos--];
        if(c >= 'A' && c <= 'D'){
            node = makeNode(c);
        }
        else if(c == ')'){
            node = EXPR(); 
        }
        else if(c == '(') pos--;
    }
    return node;
}
BTNode* makeNode(char c)
{
    //printf("%c\n", c);
    BTNode *newNode = (BTNode* )malloc(sizeof(BTNode));
    for(int i = 0; i < NUMSYM; i++){
        if(c == sym[i]){
            newNode->data = i; break;
        }
    }
    newNode->left = newNode->right = NULL;
    return newNode;
}
//A|(B&C)|D
```
