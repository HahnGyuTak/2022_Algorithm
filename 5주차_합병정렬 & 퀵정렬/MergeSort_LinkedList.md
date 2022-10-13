```C
#include <stdlib.h>
#include <stdio.h>

typedef struct node
{
    struct node *next;
    int ele;
}Node;

void printList(Node *H)
{
    for (Node *p = H; p != NULL; p = p->next)
        printf(" %d", p->ele);
    printf("\n");
}

Node *getNode(int val)
{
    Node *p = (Node*)malloc(sizeof(Node));
    p->ele = val;
    p->next = NULL;
    
    return p;
}

void addLast(Node **H, int val)
{
    Node *p = getNode(val);
    Node *tmp = *H;

    if (*H == NULL)
        *H = p;
    else
    {
        while (tmp->next != NULL)
            tmp = tmp->next;
        tmp->next = p;
    }
    
}

void partition(Node *H, Node **L1, Node **L2, int n)
{
    Node *p = H;
    *L1 = H;
    
    for (int i = 0; i < n/2 - 1; i++)
        p = p->next;
    *L2 = p->next;
    p->next = NULL;
}

Node* merge(Node *L1, Node *L2)
{
    Node *H = NULL;
    if (L1->ele < L2->ele)
    {
        H = L1;
        L1 = L1->next;
    }
    else 
    {
        H = L2;
        L2 = L2->next;
    }
    Node *tmp = H;
    while (L1 != NULL && L2 != NULL)
    {
        if (L1->ele < L2->ele)
        {
            tmp->next = L1;
            L1 = L1->next;
        }
        else 
        {
            tmp->next = L2;
            L2 = L2->next;
        }
        tmp = tmp->next;
    }
    if (L1 != NULL)
        tmp->next = L1;
    if (L2 != NULL)
        tmp->next = L2;
    return H;
}

void mergeSort(Node **L, int n)
{
    if (n == 1)
        return ;
    Node *l1, *l2;
    partition(*L, &l1, &l2, n);
    printf("L1 : "); printList(l1);
    printf("L2 : "); printList(l2);
    
    mergeSort(&l1, n / 2);
    mergeSort(&l2, n - n / 2);
    
    *L = merge(l1, l2);
    printf("Merge : "); printList(*L);
}



int main()
{
    int array[9] = {5, 10, 3, 2, 6, 1, 7, 15, 9};
    Node *H = NULL;
    for (int i = 0; i < 9; i++)
        addLast(&H, array[i]);
    printList(H);
    mergeSort(&H, 9);
    
    
    return 0;
}
```
