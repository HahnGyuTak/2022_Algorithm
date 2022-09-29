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
node    *addLast(node *x, int val);
void    printList(node *x);

//-------Quick Func----------
node    **partition(node *x, int n);
node    *merge(node *x, node *y);
node    *mergesort(node **x, int n);

int main()
{
    List x; initList(&x);
    int N, val;
    node *p = x.H;
    
    scanf("%d", &N); x.n = N;
    for (int i = 0; i < x.n; i++)
    {
        scanf("%d", &val);
        p = addLast(p, val);
    }
    x.H->next = mergesort(&(x.H->next), x.n);
    printList(x.H->next);
    return 0;
}

//--------Merge---------
node *mergesort(node **x, int n)
{
    //printf("\nfull list : "); printList(*x);
    if (n == 1)
        return *x;
    node **p = partition(*x, n / 2);
    
    p[0] = mergesort(p, n / 2);
    p[1] = mergesort(p + 1, n - n / 2);
    //printf("left : "); printList(p[0]); printf("right : "); printList(p[1]);
    return merge(p[0], p[1]);
}

node* merge(node *x, node *y)
{
    node *tmp = NULL;
    if (x->val < y->val)
    {
       tmp = x;
       x = x->next;
    }
    else
    {
       tmp = y;
       y = y->next;
    }
    
    node *p = tmp;
    while (x && y)
    {
        if (x->val < y->val)
        {
            p->next = x;
            x = x->next;
        }
        else
        {
           p->next = y;
           y = y->next;
        }
        p = p->next;
        //printf("px %d py %d\n",x->val, y->val);
    }
    //printf("end of while : "); printList(tmp);
    if (x)
        p->next = x;

    else if (y)
        p->next = y;
    return tmp;
}

node **partition(node *H, int n)
{
    node    *tmp = H;
    node    **x = malloc(sizeof(node*) * 2);
    
    x[0] = tmp;
    for (int i = 1; i < n; i++)
        tmp = tmp->next;
    x[1] = tmp->next;
    tmp->next = NULL;
    
    return x;
}

//------------List--------------
void initList(List *x)
{
    x->H = getNode(0);
    x->n = 0;
}

node *getNode(int val)
{
    node *p = (node *)malloc(sizeof(node));
    p->val = val;
    p->next = NULL;
    
    return p;
}

node *addLast(node *x, int val)
{
    node *p = getNode(val);
    x->next = p;
    
    return p;
}

void printList(node* x)
{
    for (node *p = x; p != NULL; p = p->next)
        printf(" %d", p->val);
    printf("\n");
}
```
