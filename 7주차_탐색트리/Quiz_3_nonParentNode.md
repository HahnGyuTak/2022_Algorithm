```C
#include <stdlib.h>
#include <stdio.h>
#include <time.h>
#define MAX(a, b) (a > b ? a : b)

typedef struct Node
{
    struct Node *left, *right;
    int data;
}Node;

Node *getNode(int val)
{
    Node *p = malloc(sizeof(Node));
    p->data = val;
    p->left = p->right = NULL;
    
    return p;
}

Node *findTree(Node *p, int k)
{
    if (p == NULL)
        return p;

    if (p->data > k)
        return findTree(p->left, k);
    else if (p->data < k)
        return findTree(p->right, k);
    else
        return p;
}

int getHeight(Node *p)
{
    int h = 0;
    if (p != NULL)
        h += MAX(getHeight(p->left), getHeight(p->right)) + 1;
    return h;
}

int getBalance(Node *p)
{
    if (p == NULL) return 0;
    
    return getHeight(p->left) - getHeight(p->right);
}

Node *Rightrotate(Node *p)
{
    Node *x = p->left;
    p->left = x->right;
    x->right = p;
    
    return x;
}

Node *Leftrotate(Node *p)
{
    Node *x = p->right;
    p->right = x->left;
    x->left = p;
    
    return x;
}

Node* rotate(Node *p, int key)
{
    int b = getBalance(p);
    if (b > 1)
    {
        if (p->left->data > key)
            return Rightrotate(p);
        else
        {
            p->left = Leftrotate(p->left);
            return Rightrotate(p);
        }
    }
    else if (b < -1)
    {
        if (p->right->data > key)
        {
            p->right = Rightrotate(p->right);
            return Leftrotate(p);
        }
        else
            return Leftrotate(p);
    }
    
    return p;
}

Node* insertItem(Node *p, int k)
{
    if (p == NULL)
        return getNode(k);

    if (p->data > k)
        p->left = insertItem(p->left, k);
    else if (p->data < k)
        p->right = insertItem(p->right, k);
    
    p = rotate(p, k);
    return p;
}

Node* removeItem(Node *p, int k, int *flag)
{
    if (p == NULL) return p;

    if (p->data > k)
        p->left = removeItem(p->left, k, flag);
    else if (p->data < k)
        p->right = removeItem(p->right, k, flag);
    else
    {   
        *flag = 1;
        if (p->left == NULL)
        {
            Node *tmp = p->right;
            free(p);
            return tmp;
        }
        else if (p->right == NULL)
        {
            Node *tmp = p->left;
            free(p);
            return tmp;
        }
        
        else
        {
            Node *w = p->right;
            int tmp;
            while (w->left != NULL) w = w->left;
            p->data = w->data;
            p->right = removeItem(p->right, w->data, &tmp);
        }
    }
    
    int b = getBalance(p);
    if (b > 1)
    {
        if (getBalance(p->left) >= 0)
            return Rightrotate(p);
        else
        {
            p->left = Leftrotate(p->left);
            return Rightrotate(p);
        }
    }
    else if (b < -1)
    {
        if (getBalance(p->right) > 0)
        {
            p->right = Rightrotate(p->right);
            return Leftrotate(p);
        }
        else
            return Leftrotate(p);
    }
    return p;
}

void preOrder(Node *p)
{
    if (p == NULL) return ;
    printf(" %d", p->data);
    preOrder(p->left);
    preOrder(p->right);
}

int main()
{
    Node *root = NULL;
    int k;
    char func;
    
    while (1)
    {
        scanf("%c", &func);
        switch(func)
        {
            case 'i':
                scanf("%d", &k);
                root = insertItem(root, k);
                break;
            case 'd':
                scanf("%d", &k);
                int f = 0;
                root = removeItem(root, k, &f);
                !f ? printf("X\n") : printf("%d\n", k);
                break;
            case 's':
                scanf("%d", &k);
                Node *q = findTree(root, k);
                q == NULL ? printf("X\n") : printf("%d\n", q->data);
                break;
            case 'p':
                preOrder(root);
                printf("\n");
                break;
            case 'q':
                return 0;
        }
    }
    
    return 0;
}
```
