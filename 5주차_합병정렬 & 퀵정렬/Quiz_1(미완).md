```C
#include <stdio.h>

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

void initList(List *x);
node *getNode(int val);
void addLast(List *x, int val);
List *seperate(List *x);
List *merge(List *x, List *y);
void mergesort(List *x);
void printList(List x);

int main()
{
    List x; initList(&x);
    int val;
    scanf("%d", &x.n);
    for (int i = 0; i < x.n; i++)
    {
        scanf("%d", &val);
        addLast(&x, val);
    }
    mergesort(&x);
    printList(x);
    return 0;
}
void mergesort(List *x)
{
    if (x->n <= 2)
    {
        if (x->n == 1)
            return ;
        if (x->H->next->val > x->H->next->next->val)
            
    }
    List *l = seperate(x);
    mergesort(l);
    mergesort(l + 1);
    
    x = merge(l, l + 1);
}
List* merge(List *x, List *y)
{
    List tmp; initList(&tmp);
    node *px = x->H->next, *py = y->H->next;
    node *pt = tmp.H;
    while (px && py)
    {
        node *t = NULL;
        if (px->val > py->val)
        {
            t = py;
            py = py->next;
        }
        else
        {
            t = px;
            px = px->next;
        }
        pt->next = t;
        t->next = NULL;
        pt = pt->next;
    }
    while (px)
    {
        pt->next = px;
        px = px->next;
        pt->next->next = NULL;
        pt = pt->next;
    }
    while (py)
    {
        pt->next = py;
        py = py->next;
        pt->next->next = NULL;
        pt = pt->next;
    }
    return &tmp;
}

List *seperate(List *p)
{
    List x[2];
    node *tmp = p->H;
    x[0].H->next = p->H.next;
    for (int i = 0; i < p->n; i++)
    {
        if (i == p.n / 2)
            x[1].H->next = tmp;
        
        tmp = tmp->next;
    }
    x[0].n = p.n / 2;
    x[1].n = p.n - n/2;
    return x;
}

void initList(List *x)
{
    x->H->next = NULL;
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
    node *tmp = x.H;
    
    while (tmp->next != NULL)
        tmp = tmp->next;
    tmp->next = p;
}

void printList(List x)
{
    for (node *p = x.H; p != NULL; p = p->next)
        printf(" %d", p->val);
}
```
