```C
#include <stdio.h>

typedef struct Heap
{
    int H[100];
    int n;
}Heap;

void swap(int *a, int *b);
void initHeap(Heap *h);
int isEmpty(Heap h);
int insertitem(Heap *h, int key);
int removeMax(Heap *h);
void upHeap(Heap *h, int idx);
void downheap(Heap *h, int idx);
void printHeap(Heap h);

void heapADT(Heap *heap)
{
    char func;
    int key;
    
    while (1)
    {
        scanf("%c", &func);
        switch(func)
        {
            case 'i':
                scanf("%d", &key);
                getchar();
                printf("%d\n", insertitem(heap, key));
                break;
            case 'd':
                printf("%d\n",removeMax(heap));
                break;
            case 'p':
                printHeap(*heap);
                break;
            case 'q':
                return ;
        }
    }
}

int main()
{
    Heap heap;
    initHeap(&heap);
    heapADT(&heap);

    return 0;
}

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

void initHeap(Heap *h)
{
    h->n = 0;
}

int isEmpty(Heap h)
{
    return h.n == 0 ? 1 : 0;
}

int insertitem(Heap *h, int key)
{
    if (h->n >= 99)
        return -1;
    h->H[++h->n] = key;
    upHeap(h, h->n);
    
    return 0;
}

int removeMax(Heap *h)
{
    int del = h->H[1];
    
    h->H[1] = h->H[h->n--];
    downheap(h, 1);
    
    return del;
}

void upHeap(Heap *h, int idx)
{
    int child = idx;
    int val_child = h->H[idx];
    
    while (child > 1)
    {
        int parent = child / 2;
        int val_parent = h->H[parent];
        if (val_parent < val_child)
            swap(&h->H[child], &h->H[parent]);
        child = parent;
    }
}

void downheap(Heap *h, int idx)
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
```
