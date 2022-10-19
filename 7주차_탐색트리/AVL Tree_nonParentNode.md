```C
#include <stdio.h>
#include <stdlib.h>
#define MAX(a, b) (a > b ? a : b)

typedef struct node
{
    struct node *left, *right;
    int data;
}Node;

Node *getNode(int val)
{
    Node *p = malloc(sizeof(Node));
    
    p->left = p->right = NULL;
    p->data = val;
    
    return p;
}

Node *treeSearch(Node *p, int key)
{
    if (p == NULL || p->data == key) return p;
    if (p->data > key) return treeSearch(p->left, key);
    if (p->data < key) return treeSearch(p->right, key);
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
    if (p == NULL) 
        return 0;
    return getHeight(p->left) - getHeight(p->right);
}

Node *rotateleft(Node *p)
{
    Node *x = p->right;
    p->right = x->left;
    x->left = p;
    
    return x;
}

Node *rotateright(Node *p)
{
    Node *x = p->left;
    p->left = x->right;
    x->right = p;
    
    return x;
}

Node* rotate(Node *p, int key)
{
    int b = getBalance(p);
    
    if (b > 1)
    {
        if (p->left->data > key)
            return rotateright(p);
        else if (p->left->data < key)
        {
            p->left = rotateleft(p->left);
            return rotateright(p);
        }
    }
    else if (b < -1)
    {
        if (p->right->data < key)
            return rotateleft(p);
        else if (p->right->data > key)
        {
            p->right = rotateright(p->right);
            return rotateleft(p);
        }
    }
    
    return p;
}

Node *insertItem(Node *p, int key)
{
    if (p == NULL)
        return getNode(key);

    if (p->data == key)
        return p;
    else if (p->data > key)
        p->left = insertItem(p->left, key);
    else
        p->right = insertItem(p->right, key);
    
    p = rotate(p, key);
    
    return p;
}

Node *inOrderSucc(Node *p)
{
    while (p->left != NULL)
        p = p->left;
    return p;
}

Node *removeElement(Node *p, int key)
{
    if (p == NULL && printf("X\n")) return p;
    
    if (key > p->data)
        p->right = removeElement(p->right, key);
    else if (key < p->data)
        p->left = removeElement(p->left, key);
    else
    {
        Node *z;
        if (p->right == NULL)
        {
            z = p->left;
            free(p);
            return z;
        }
        else if (p->left ==NULL)
        {
            z = p->right;
            free(p);
            return z;
        }
        
        z = inOrderSucc(p->right);
        p->data = z->data;
        p->right = removeElement(p->right, z->data);
    }
    int b = getBalance(p);
    
    if (b > 1)
    {
        if (getBalance(p->left) >= 0)
            return rotateright(p);
        else //if (p->left->data < key)
        {
            p->left = rotateleft(p->left);
            return rotateright(p);
        }
    }
    else if (b < -1)
    {
        if (getBalance(p->right))
            return rotateleft(p);
        else //f (p->right->data > key)
        {
            p->right = rotateright(p->right);
            return rotateleft(p);
        }
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

void inOrder(Node *p, int k)
{
    if (p == NULL)
        return ;
    inOrder(p->left, k);
    printf(" %d",p->data);
    inOrder(p->right, k);
}

int main()
{
    Node *root = NULL;
    
    char fun; int k;
    
    while (1)
    {
        scanf("%c", &fun);
        switch (fun)
        {
            case 'i':
                scanf("%d", &k);
                root = insertItem(root, k);
                break;
            case 's':
                scanf("%d", &k);
                Node *q = treeSearch(root, k);
                q == NULL ? printf("X\n") : printf("%d\n", q->data);
                break;
            case 'p':
                preOrder(root); printf("\n");
                break;
            case 'q':
                return 0;
            case 'd':
                scanf("%d", &k);
                Node *p = treeSearch(root, k);
                p == NULL ? printf("X\n") : printf("%d\n", p->data);
                root = removeElement(root, k);
                break;
            case 'o':
                inOrder(root, 1); printf("\n");
                //inOrder(root, 0); printf("\n");
                break;
        }
    }
    
    /*root = insertItem(root, 44);
    root = insertItem(root, 17);
    root = insertItem(root, 78);
    root = insertItem(root, 32);
    root = insertItem(root, 50);
    root = insertItem(root, 48);
    root = insertItem(root, 62);
    root = insertItem(root, 88);
    root = insertItem(root, 54);
    
    preOrder(root); printf("\n");
    
    root = removeElement(root, 32);
    
    preOrder(root);printf("\n");*/
    return 0;
}
```
