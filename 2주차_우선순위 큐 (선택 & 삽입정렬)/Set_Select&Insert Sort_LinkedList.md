```C
#include <stdlib.h>
#include <stdio.h>
#include <time.h>
#define SWAP(a, b, type) do{type tmp = a; a = b; b = tmp;}while(0)

typedef struct DListNode {
    int elem;
    struct DListNode* prev, * next; 
    
}DListNode;

typedef struct {
    DListNode* H;
    DListNode* T; 
}SetType;

void initNode(DListNode* H, DListNode* T) 
{
    H->next = T; T->prev = H;
}

void initSet(SetType* s) 
{
    s->H = (DListNode*)malloc(sizeof(DListNode)); 
    s->T = (DListNode*)malloc(sizeof(DListNode)); 
    initNode(s->H, s->T);
} 

void addLast(SetType *x, int val)
{
    DListNode *p = x->T->prev;
    DListNode *newnode = (DListNode*)malloc(sizeof(DListNode));
    
    newnode->elem = val;
    p->next = newnode;
    newnode->next = x->T;
    x->T->prev = newnode;
    newnode->prev = p;
}

void selectSort(SetType *x)
{
    DListNode *p = x->H->next;
    while (p != x->T)
    {
        DListNode *tmp = p->next, *min = p;
        while (tmp != x->T)
        {
            if (tmp->elem < min->elem)
                min = tmp;
            else if (tmp->elem == min->elem)
            {
                tmp->prev->next = tmp->next;
                tmp->next->prev = tmp->prev;
            }
            tmp = tmp->next;
        }
        SWAP(p->elem, min->elem, int);
        p = p->next;
    }
}

void insertionSort(SetType *x)
{
    DListNode *tmp = x->H->next->next;
    while (tmp != x->T)
    {
        DListNode *p = tmp->prev;
        int key = tmp->elem;
        while (p != x->H)
        {
            if (p->elem > key)
            {
                p->next->elem = p->elem;
                p = p->prev;
            }
            else if (p->elem == key)
            {
                p->prev->next = p->next;
                p->next->prev = p->prev;
                p = p->prev;
            }
            else
                break;
        }
        p->next->elem = key;
        tmp = tmp->next;
    }
}

void UnionSet(SetType *x, SetType *a, SetType *b)
{
    DListNode *atmp = a->H->next, *btmp = b->H->next;
    
    while (atmp != a->T && btmp != b->T)
    {
        if (atmp->elem < btmp->elem)
        {
            addLast(x, atmp->elem);
            atmp = atmp->next;
        }
        else if (atmp->elem > btmp->elem)
        {
            addLast(x, btmp->elem);
            btmp = btmp->next;
        }
        else
        {
            addLast(x, btmp->elem);
            btmp = btmp->next;
            atmp = atmp->next;
        }
    }
    while (atmp != a->T)
    {
        addLast(x, atmp->elem);
        atmp = atmp->next;
    }
    while (btmp != b->T)
    {
        addLast(x, btmp->elem);
        btmp = btmp->next;
    }
}

void DiffSet(SetType *x, SetType *a, SetType *b)
{
    DListNode *atmp = a->H->next, *btmp = b->H->next;
    
    while (atmp != a->T && btmp != b->T)
    {
        if (atmp->elem < btmp->elem)
        {
            addLast(x, atmp->elem);
            atmp = atmp->next;
        }
        else if (atmp->elem > btmp->elem)
            btmp = btmp->next;
        else
        {
            btmp = btmp->next;
            atmp = atmp->next;
        }
    }
    while (atmp != a->T)
    {
        addLast(x, atmp->elem);
        atmp = atmp->next;
    }
}

void printList(SetType *x)
{
    for (DListNode *p = x->H->next; p != x->T; p = p->next)
        printf(" %d", p->elem);
    printf("\n");
}

int main() 
{
    SetType* s1 = (SetType*)malloc(sizeof(SetType)); initSet(s1);
    SetType* s2 = (SetType*)malloc(sizeof(SetType)); initSet(s2);
    srand(time(NULL));
    for (int i = 0; i < 10; i++)
    {
        int a = rand() % 30 + 11;
        int b = rand() % 30 + 11;
        addLast(s1, a);
        addLast(s2, b);
    }
    printf("A :"); printList(s1);
    printf("B :"); printList(s2);
    
    insertionSort(s1);
    selectSort(s2);
    
    printf("Sort A :"); printList(s1);
    printf("Sort B :"); printList(s2);
    
    SetType* Union = (SetType*)malloc(sizeof(SetType)); initSet(Union);
    UnionSet(Union, s1, s2);
    printf("Sort Union :"); printList(Union);
    
    SetType* Diff = (SetType*)malloc(sizeof(SetType)); initSet(Diff);
    DiffSet(Diff, s1, s2);
    printf("Sort Diff :"); printList(Diff);
    
}
```
