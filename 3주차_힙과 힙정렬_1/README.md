[문제 바로가기](https://www.acmicpc.net/problem/1655)

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct heap
{
    int N;
    int h[50000];
}Heap;

//----------Heap Func--------------
int     isHeapEmpty(Heap x);
void    swap(int *a, int *b);
void    initHeap(Heap **x);
void    upMaxHeap(Heap *heap, int idx);
void    upMinHeap(Heap *heap, int idx);
void    downMaxHeap(Heap *h, int idx);
void    downMinHeap(Heap *h, int idx);

//----------Solution Func--------------
void    printMidValues(Heap *small, Heap *large);
int     getMidValues(Heap *small, Heap *large, int input);

int main()
{
    Heap *small, *large; // small은 비교적 작은 수들의 최대힙, large는 큰 수들의 최소힙
    
    initHeap(&small); initHeap(&large); // 힙 초기화
    printMidValues(small, large);
    free(small);
    free(large);

    return 0;
}

//----------------------------------------------
//------------------Functions-------------------
void printMidValues(Heap *small, Heap *large)
{
    int N, input;
    
    scanf("%d", &N);
    for (int i = 0; i < N; i++)
    {
        scanf("%d", &input);
        printf("%d\n", getMidValues(small, large, input));
    }
}

int getMidValues(Heap *small, Heap *large, int input)
{
    //small은 large보다 value개수가 같거나 1개 더 많아야 함.
    // 조건에 따라 삽입 후 upHeap 해준다
    if (small->N == large->N) 
    {
        small->h[++(small->N)] = input;
        upMaxHeap(small, small->N);
    }
    else if (small->N - large->N >= 1)
    {
        large->h[++(large->N)] = input;
        upMinHeap(large, large->N);
    }
    
    //-------------- small(최대힙)의 root값이 large(최소힙)의 root값 보다 클 경우, swap해준다 
    if (!isHeapEmpty(*large) && small->h[1] > large->h[1])
    {
        swap(small->h + 1, large->h + 1);
        downMinHeap(large, 1);
        downMaxHeap(small, 1);
    }
    return small->h[1];
}

//------------------Heap Func------------------------
void    upMaxHeap(Heap *heap, int idx)
{
    int child = idx;
    int parent;
    while (child > 1)
    {
        parent = child / 2;
        if (parent >= 1 && heap->h[parent] < heap->h[child])
            swap(heap->h + parent, heap->h + child);
        child = parent;
    }
}

void    upMinHeap(Heap *heap, int idx)
{
    int child = idx;
    int parent;
    while (child > 1)
    {
        parent = child / 2;
        if (parent >= 1 && heap->h[parent] > heap->h[child])
            swap(heap->h + parent, heap->h + child);
        child = parent;
    }
}

void downMaxHeap(Heap *h, int idx)
{
    int parent = idx;
    int child;
    do
    {
        child = parent * 2;
        if (child < h->N && h->h[child] < h->h[child + 1])
            child++;
        if (child <= h->N && h->h[child] > h->h[parent])
            swap(&h->h[child], &h->h[parent]);
        parent = child;
    }while(child <= h->N);
}

void downMinHeap(Heap *h, int idx)
{
    int parent = idx;
    int child;
    do
    {
        child = parent * 2;
        if (child < h->N && h->h[child] > h->h[child + 1])
            child++;
        if (child <= h->N && h->h[child] < h->h[parent])
            swap(&h->h[child], &h->h[parent]);
        parent = child;
    }while(child <= h->N);
}

void initHeap(Heap **x)
{
    (*x) = malloc(sizeof(Heap));
    (*x)->N = 0;
}

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int isHeapEmpty(Heap x)
{
    return x.N == 0 ? 1 : 0;
}
```
