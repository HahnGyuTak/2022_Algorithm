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
void    printList(node *x);

//-------Merge Func----------
node    **partition(node *x);
node    *merge(node *x, node *y);
void    mergesort(node **x);

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
    mergesort(&x.H);
    printList(x.H);
    return 0;
}

//--------Merge---------
void mergesort(node **x)
{
    //printf("\nfull list : "); printList(*x);
    if ((*x)->next == NULL)
        return ;
    if ((*x)->next->next == NULL)
    {
        if ((*x)->val > (*x)->next->val)
        {
            (*x)->next->next = (*x);
            (*x) = (*x)->next;
            (*x)->next->next = NULL;
        }
        return ;
    }
    node **p = partition(*x);
    
    mergesort(p);
    mergesort(p + 1);
    //printf("left : "); printList(p[0]);
    //printf("right : "); printList(p[1]);
    *x = merge(p[0], p[1]);
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
    //printf("merged list : "); printList(tmp);
    return tmp;
}

node **partition(node *H)
{
    node    *tmp = H;
    node    **x = malloc(sizeof(node*) * 2);
    int     cnt;
    
    x[0] = tmp;
    for (cnt = 0; tmp != NULL; cnt++)
        tmp = tmp->next;
    tmp = H;
    for (int i = 1; i < cnt / 2; i++)
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
}

void printList(node* x)
{
    for (node *p = x; p != NULL; p = p->next)
        printf(" %d", p->val);
    printf("\n");
}
```
