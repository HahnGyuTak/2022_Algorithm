```C
#include <stdlib.h>
#include <stdio.h>
#define swap(A, B, type) do{ type tmp = A; A = B; B = tmp;}while(0)

typedef struct node
{
    struct node *next;
    int data;
}Node;

typedef struct
{
    Node *H;
    int N;
}List;

void initList(List *x)
{
    x->N = 0;
    x->H = NULL;
}

Node *getNode(int val)
{
    Node *p = malloc(sizeof(Node));
    p->next = NULL;
    p->data = val;
    
    return p;
}

void addLast(List *H, int val)
{
    Node *p = getNode(val);
    
    if (H->H == NULL)
        H->H = p;
    else
    {
        Node *tmp = H->H;
        while (tmp->next != NULL)
            tmp = tmp->next;
        tmp->next = p;
    }
    H->N++;
}

void *partition(List *L, List **LT, List **EQ, List **GT, int k)
{
    Node *tmp = L->H;
    while (tmp != NULL)
    {
        if (tmp->data < k)
            addLast(*LT, tmp->data);
        else if (tmp->data == k)
            addLast(*EQ, tmp->data);
        else
            addLast(*GT, tmp->data);
        Node *p = tmp;
        tmp = tmp->next;
        free(p);
    }
}

void merge(List **L, List *a, List *b, List *c)
{
    Node *ta = a->H, *tb = b->H, *tc = c->H;
    Node *tmp = (*L)->H;
    
    if (a->N)
    {
        (*L)->H = a->H; tmp = (*L)->H;
        while (tmp->next != NULL) tmp = tmp->next;
        (*L)->N += a->N;
    }
    if (b->N)
    {
        if ((*L)->H != NULL) 
            tmp->next = tb;
        else
        {
            (*L)->H = tb;
            tmp = (*L)->H;
        }
        while (tmp->next != NULL) tmp = tmp->next;
        (*L)->N += b->N;
    }
    if (c->N)
    {
        tmp->next = tc;
        (*L)->N += c->N;
    }
}

void quickSort(List *L)
{
    if (L->N <= 1) return ;
    int pivot = L->H->data;
    List *LT = malloc(sizeof(List)), *EQ = malloc(sizeof(List)), *GT = malloc(sizeof(List));
    initList(LT); initList(EQ); initList(GT);
    
    partition(L, &LT, &EQ, &GT, pivot);

    //printf("\n%d LT : ", LT->N);for (Node *p = LT->H; p != NULL; p = p->next) printf(" %d", p->data);
    //printf("\n%d EQ : ", EQ->N);for (Node *p = EQ->H; p != NULL; p = p->next) printf(" %d", p->data);
    //printf("\n%d GT : ", GT->N);for (Node *p = GT->H; p != NULL; p = p->next) printf(" %d", p->data);    
    
    initList(L);
    quickSort(LT); quickSort(GT);
    
    merge(&L, LT, EQ, GT);
    free(LT); free(GT); free(EQ);
}

int main()
{
    int N, v;
    List L; initList(&L);
    
    scanf("%d", &N); 
    for (int i = 0; i < N; i++)
        { scanf("%d", &v); addLast(&L, v); }
    //for (Node *p = L.H; p != NULL; p = p->next) printf(" [%d]", p->data);
    quickSort(&L);
    //printf("\nHead %d\n", L.H->data);
    
    for (Node *p = L.H; p != NULL; p = p->next) printf(" %d", p->data);
    return 0;
}
/*
8
73 65 48 31 29 20 8 3
*/
```
