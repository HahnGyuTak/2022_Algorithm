```C
#include <stdlib.h>
#include <stdio.h>
#define swap(A, B, type) do{ type tmp = A; A = B; B = tmp;}while(0)

typedef struct node
{
    struct node *parent, *left, *right;
    int data;
}Node;

typedef struct
{
    Node *root;
    int n;
}Heap;

int isEmpty(Heap *x)
{
    return x->n == 0 ? 1 : 0;
}

Node *getNode(int val, Node *parent)
{
    Node *p = malloc(sizeof(Node));
    p->parent = parent;
    p->left = p->right = NULL;
    p->data = val;
    
    return p;
}

Node *searchLast(Heap *x, int N, int flag) // flag가 0이면 부모노드 찾기
{
    int a[10], tmp = N, i = 0;

    while (tmp > 1)
    {
        a[i++] = tmp % 2;
        tmp /= 2;
    }

    Node *t = x->root;
    for (int j = i - 1; j >= 0; j--)
    {
        if (flag == 1 || j > 0)
        {
            if (a[j] == 0)
                t = t->left;
            else
                t = t->right;
        }
    }
    
    return t;
}

void upHeap(Node *v)
{
    if (v->parent == NULL)
        return ;
    if (v->data < v->parent->data)
        return ;
    swap(v->data, v->parent->data, int);
    upHeap(v->parent);
}

void downHeap(Node *v)
{
    if (v->left == NULL)
        return ;

    Node *child = v->left;
    if (v->right != NULL && child->data < v->right->data)
        child = v->right;

    if (child->data > v->data)
        swap(child->data, v->data, int);
    else
        return ;
    downHeap(child);
}

void insertItem(Heap *x, int val)
{
    if (isEmpty(x))
    {
        x->root = getNode(val, NULL);
        x->n++;
    }
    else
    {
        Node *p = searchLast(x, x->n + 1, 0); // p에 부모노드 저장
        Node *t;
        //printf("parent : %d\n", p->data);
        if (p->left != NULL)
            t = p->right = getNode(val, p);
        else
            t = p->left = getNode(val, p);
        x->n++;
        upHeap(t);
    }
}

int removeMax(Heap *x)
{
    Node *last = searchLast(x, x->n, 1);
    if (last == x->root)
    {
        int tmp = x->root->data;
        free(last);
        return tmp;
    }
    
    int max = x->root->data;
    
    swap(x->root->data, last->data, int);
    x->n--;
    //printf("befor downheap root : %d and deleted is %d\n", x->root->data, last->data);
    
    if (last->parent->left == last)
        last->parent->left = NULL;
    else
        last->parent->right = NULL;
        
    free(last);
    
    downHeap(x->root);
    return max;
}

void printHeap(Node *x)
{
    int f = 0, r = -1;
    Node *queue[101];
    queue[++r] = x;
    while (r - f >= 0)
    {
        Node *v = queue[f++];
        printf(" %d", v->data);
        if (v->left != NULL)  queue[++r] = v->left;
        if (v->right != NULL)  queue[++r] = v->right; 
    }
    printf("\n");
}

int main()
{
    Heap x; x.n = 0; x.root = NULL;
    int N, h[100];
    scanf("%d", &N);
    for (int i = 0; i < N; i++)
    {
        scanf("%d", h + i);
        insertItem(&x, h[i]);
    }
    //printHeap(x.root);
    for (int i = N - 1; i >= 0; i--)
        h[i] = removeMax(&x);
    for (int i = 0; i < N; i++) printf(" %d", h[i]);    
    return 0;
}
```
