```C
#include <stdlib.h>
#include <stdio.h>
#define MAX(a, b) (((a) > (b)) ? (a) : (b))

typedef struct node
{
    struct node *left, *right, *parent;
    int data, height;
}Node;

Node *getNode(Node *parent)
{
    Node *p = malloc(sizeof(Node));
    p->left = p->right = NULL;
    p->parent = parent;
    p->height = 0;
    
    return p;
}

int isExternal(Node *p)
{
    if (p->left == NULL && p->right == NULL) return 1;
    else return 0;
}

Node *treeSearch(Node *p, int key)
{
    if (isExternal(p))
        return p;
    if (p->data < key) return treeSearch(p->right, key);
    else if (p->data > key) return treeSearch(p->left, key);
    else return p;
}

void expandExternal(Node *p)
{
    Node *l = getNode(p);
    Node *r = getNode(p);
    p->left = l;
    p->right = r;
    p->height = 1;
}

int heightUnbalanceNode(Node *p)
{
    
    if (p == NULL) return 0;
    
    int k = p->left->height - p->right->height;
    p->height = MAX(p->left->height, p->right->height) + 1;
    printf("node : %d h : %d\n", p->data, p->height);
    if (k * k <= 1)
        return 0;
    else
        return 1;
}

void restructure(Node **root, Node *x, Node *y, Node *z)
{
    Node *a, *b, *c;
    Node *T0, *T1, *T2, *T3;
    if (z->data < y->data && y->data < x->data)
    {
        a = z; b = y; c = x;
        T0 = z->left;
        T1 = y->left;
        T2 = x->left;
        T3 = x->right;
    }
    else if (x->data < y->data && y->data < z->data)
    {
        a = x; b = y; c = z;
        T0 = a->left;
        T1 = a->right;
        T2 = b->right;
        T3 = c->right;
    }
    else if (z->data < x->data && x->data < y->data)
    {
        a = z; b = x; c = y;
        T0 = a->left;
        T1 = b->left;
        T2 = b->right;
        T3 = c->right;
    }
    else //(y->data < x->data && x->data < z->data)
    {
        a = y; b = x; c = z;
        T0 = a->left;
        T1 = b->left;
        T2 = b->right;
        T3 = c->right;
    }
    
    if (z == *root)
    {
        *root = b;
        b->parent = NULL;
    }
    else if (z->parent->left == z)
    {
        z->parent->left = b;
        b->parent = z->parent;
    }
    else
    {
        z->parent->right = b;
        b->parent = z->parent;
    }
    
    a->parent = b;
    a->left = T0;
    T0->parent = a;
    a->right = T1;
    T1->parent = a;
    a->height = MAX(T0->height, T1->height) + 1;
    
    c->parent = b;
    c->left = T2;
    T2->parent = c;
    c->right = T3;
    T3->parent = c;
    c->height = MAX(T2->height, T3->height) + 1;
    
    b->left = a;
    b->right = c;
    b->height = MAX(a->height, c->height) + 1;
}

void searchAndFixAfterInsertion(Node **root, Node *w)
{
    Node *z = w;
    
    while (!heightUnbalanceNode(z))
    {
        if (z == NULL)
            return;
        z = z->parent;
    }
    printf("%d~\n", z->data);
    Node *y, *x;
    if (z->left->height > z->right->height)
        y = z->left;
    else
        y = z->right;

    if (y->left->height > y->right->height)
        x = y->left;
    else
        x = y->right;
    
    restructure(root, x, y, z);
    
}

void insertItem(Node **root, int val)
{
    Node *s = treeSearch(*root, val);
    if (!isExternal(s))
        return ;
        
    s->data = val;
    expandExternal(s);
    searchAndFixAfterInsertion(root, s);
    printf("~%d\n", s->data);
}

Node *sibiling(Node *p)
{
    if (p->parent->left == p)
        return p->parent->right;
    else
        return p->parent->left;
}

Node *inOrderSucc(Node *p)
{
    Node *tmp = p->right;
    while (tmp->left != NULL)
        tmp = tmp->left;
    return tmp;
}

void reduceExternal(Node **root, Node *z)
{
    
    Node *y = z->parent;
    Node *x = y->parent;
    
    Node *zs = sibiling(z);
    zs->parent = x;
    
    if (x == NULL)
        *root = zs;
    else
        if (y == x->left)
            x->left = zs;
        else 
            x->right = zs;
    free(z); free(y);
    
}

int removeElement(Node **root, int val)
{
    Node *s = treeSearch(*root, val);
    
    if (isExternal(s))
        return -1;
    
    Node *z = s->left;
    if (!isExternal(z))
        z = s->right;
    //printf("z : %d\n", z == NULL ? 1 : 0);
    if (isExternal(z))
        reduceExternal(root, z);
    else
    {
        Node *y = inOrderSucc(s);
        z = y->parent;
        s->data = z->data;
        reduceExternal(root, y);
    }
    return val;
}

void inOrder(Node *p, int k)
{
    if (isExternal(p))
        return ;
    inOrder(p->left, k);
    printf(" %d", k == 1 ? p->data : p->height);
    inOrder(p->right, k);
}

int main()
{
    Node *root = getNode(NULL); 
    insertItem(&root, 44); insertItem(&root, 17);
    insertItem(&root, 78); insertItem(&root, 32);
    insertItem(&root, 50); insertItem(&root, 88);
    insertItem(&root, 48); insertItem(&root, 62);
    
    inOrder(root, 1); printf("\n");
    inOrder(root, 0); printf("\n");
    
    insertItem(&root, 54);
    //printf("%d is delete\n", removeElement(&root, 3));
    
    inOrder(root, 1); printf("\n");
    inOrder(root, 0); printf("\n");
    return 0;
}
```
