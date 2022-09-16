```C
#include <stdio.h>
#include <stdlib.h>

typedef struct Heap
{
    int H[100];
    int n;
}Heap;

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int isEmpty(Heap h)
{
    return h.n == 0 ? 1 : 0;
}

void DownHeap(Heap *h, int idx) // 비재귀
{
    int parent = idx;
    int child;
    do
    {
        child = parent * 2;
        if (child < h->n && h->H[child] < h->H[child + 1])
            child++;
        if (child <= h->n && h->H[child] > h->H[parent])
            swap(&h->H[child], &h->H[parent]);
        parent = child;
    }while(child <= h->n);
}

void rDownHeap(Heap *h, int parent) //재귀
{
    int child = parent * 2;
    if (parent > h->n )
        return ;
    if (child < h->n && h->H[child] < h->H[child + 1])
            child++;
    if (child <= h->n && h->H[child] > h->H[parent])
        swap(&h->H[child], &h->H[parent]);
    rDownHeap(h, child);
}

void makeHeap(Heap *h)
{
    for (int i = h->n / 2; i >= 1; i--)
        rDownHeap(h, i);
}

void printHeap(Heap h)
{
    if (isEmpty(h))
    {
        printf("There is nothing!\n");
        return ;
    }
    for (int i = 1; i <= h.n; i++)
        printf(" %d", h.H[i]);
    printf("\n");
}

int main()
{
    Heap h;
    
    scanf("%d", &h.n);
    for (int i = 0; i < h.n ; i++)
        scanf("%d", h.H + i + 1);
    
    makeHeap(&h);
    printHeap(h);
    
    return 0;
}
```
