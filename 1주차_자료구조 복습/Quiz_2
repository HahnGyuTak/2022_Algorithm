```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct treenode
{
    int data;
    struct treenode *left, *right;
}treenode;

void init_node(treenode *x)
{
    x->left = NULL;
    x->right = NULL;
}

void getnode(treenode **x, int N)
{
    (*x) = (treenode*)malloc(sizeof(treenode));
    init_node(*x);
    (*x)->data = N;
}

void make_tree(treenode *p, int n)
{
    int x, lc, rc;
    
    scanf("%d %d %d", &x, &lc, &rc);
    p->data = x;
    if (lc) 
    {
        getnode(&(p->left), lc);
        make_tree(p->left, lc);
    }
    if (rc) 
    {
        getnode(&(p->right), rc);
        make_tree(p->right, rc);
    }
}

void print(treenode *r)
{
    printf("%d ",r->data);
    if (r->left) print(r->left);
    if (r->right) print(r->right);
}

int main()
{
    int N_node, N_search;
    treenode R, *tmp;
    char s[101];
    
    scanf("%d", &N_node);
    make_tree(&R, N_node);
    
    scanf("%d", &N_search);
    for (int i = 0; i < N_search; i++)
    {
        getchar();
        scanf("%s", s);
        
        tmp = &R;
        int len = strlen(s);
        printf(" %d", tmp->data);
        for (int j = 0; j < len; j++)
        {
            if (s[j] == 'L' && tmp->left) tmp = tmp->left;
            if (s[j] == 'R' && tmp->right) tmp = tmp->right;
            printf(" %d", tmp->data);
        }
        printf("\n");
    }
    
    return 0;
}
```
