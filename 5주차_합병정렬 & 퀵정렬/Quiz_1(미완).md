```C
#include <stdio.h>
#include <stdlib.h>

typedef struct node
{
    int val;
    struct node *next;
}node;

typedef struct List
{
    node *H;
    int n;
}List;

//-------List Func-----------
void    initList(List *x);
node    *getNode(int val);
void    addLast(List *x, int val);
void    printList(List x);

//-------Merge Func----------
node    **partition(node *x);
node    *merge(node *x, node *y);
void    mergesort(node *x);

int main()
{
    List x; initList(&x);
    int N, val;
    
    scanf("%d", &N); x.n = N;
    for (int i = 0; i < x.n; i++)
    {
        scanf("%d", &val);
        addLast(&x, val);
    }
    printList(x);
    mergesort(x.H);
    printList(x);
    return 0;
}

//--------Merge---------
void mergesort(node *x)
{
    if (x->next->next == NULL)
    {
        if (x->val > x->next->val)
        {
            x->next->next = x;
            x = x->next;
            x->next->next = NULL;
        }
        return ;
    }
    if (x->next == NULL)
        return ;
    node **p = partition(x);
    mergesort(p[0]);
    mergesort(p[1]);
    x = merge(p[0], p[1]);
}

node* merge(node *x, node *y)
{
    node *tmp = NULL;
    node *p = tmp, *px = x, *py = y;
    while (px && py)
    {
        if (px->val < py->val)
        {
            if (!p)
                p = px;
            else
            {
                p->next = px;
                p = p->next;
            }
            px = px->next;
        }
        else
        {
            if (!p)
                p = py;
            else
            {
                p->next = py;
                p = p->next;
            }
            py = py->next;
        }
    }
    while (!px)
        p->next = px;
 
    while (!py)
        p->next = py;

    return tmp;
}

node **partition(node *H)
{
    node    *tmp = H;
    node    **x = malloc(sizeof(node*) * 2);
    int     cnt;
    
    x[1] = tmp;
    for (cnt = 0; tmp != NULL; cnt++)
        tmp = tmp->next;
    tmp = H;
    for (int i = 0; i < cnt / 2; i++)
        tmp = tmp->next;
    x[1] = tmp->next;
    tmp->next = NULL;
    
    return x;
}

//------------List--------------
void initList(List *x)
{
    x->H = NULL;
    x->n = 0;
}

node *getNode(int val)
{
    node *p = (node *)malloc(sizeof(node));
    p->val = val;
    p->next = NULL;
    
    return p;
}

void addLast(List *x, int val)
{
    node *p = getNode(val);
    node *tmp = x->H;
    
    if (!x->H)
        x->H = p;
    else
    {
        while (tmp->next != NULL)
            tmp = tmp->next;
        tmp->next = p;
    }
    printList(*x);
}

void printList(List x)
{
    for (node *p = x.H; p != NULL; p = p->next)
        printf(" %d", p->val);
}
```
