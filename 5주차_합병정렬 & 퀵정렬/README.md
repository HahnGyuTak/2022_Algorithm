[보석 도둑](https://www.acmicpc.net/problem/1202)
[C언어 구현](```C)
[Python 구현](```Python)
```C
#include <stdlib.h>
#include <stdio.h>
#define SWAP(A, B, type) do{ type tmp = A; A = B; B = tmp;}while(0)

typedef struct
{
    int weight;
    int price;
}Gem;

typedef struct
{
    int *                           h;
    int n;
}Heap;

Heap *initHeap(int n);
void insertHeap(Heap *x, int val);
void downHeap(Heap *x, int parent);
int removeRoot(Heap *x);

void setGemBag(Gem **x, int *N, int *K, int **bag);
void sort_Gem(Gem *a, int start, int end, Gem *tmpG);
void sort_int(int *a, int start, int end, int *tmpI);

void SangDeok(Gem *x, int n, int *bag, int k);

int main()
{
    int N, K, *bag;
    Gem *x;
    setGemBag(&x, &N, &K, &bag);
    
    //for (int i = 0; i < N; i++) printf("%d %d\n", x[i].weight, x[i].price);
    //for (int i = 0; i < K; i++) printf(" %d", bag[i]);
    SangDeok(x, N, bag, K);
    
    return 0;
}

void SangDeok(Gem *x, int n, int *bag, int k)
{
    long long sum_price = 0;
    int j = 0;
    Heap *h = initHeap(n);
    
    for (int i = 0; i < k; i++)
    {
        for ( ; j < n; j++)
        {
            if (x[j].weight > bag[i]) break;
            insertHeap(h, x[j].price);
            //printf(" %d일때 %d넣음\n", bag[i], x[j].price);
        }
        //printf("Heap : %d %d\n", h->h[1], h->h[2]);
        if (!h->n) continue;
        sum_price += removeRoot(h);
        //printf("%d 까지%lld들어감\n", bag[i], sum_price);
    }
    printf("%lld\n", sum_price);
    free(x); free(bag); free(h->h);
}

void sort_Gem(Gem *a, int start, int end, Gem *tmpG)
{
    if ( start >= end ) return;
    
	int mid = (start + end) / 2;
	
	sort_Gem(a, start, mid, tmpG);
	sort_Gem(a, mid + 1, end, tmpG);
	
	int i, t = 0, left = start, j=0;
	
	for(i = start; i <= mid; i++) tmpG[t++] = a[i];

	while(i <= end && j < t)
		a[left++] = a[i].weight > tmpG[j].weight ? tmpG[j++] : a[i++];
	
	while(j < t) a[left++] = tmpG[j++];
    
}

void sort_int(int *a, int start, int end, int *tmpI)
{
    if ( start >= end ) return;
    
	int mid = (start + end) / 2;
	
	sort_int(a, start, mid, tmpI);
	sort_int(a, mid + 1, end, tmpI);
	
	int i, t = 0, left = start, j=0;
	
	for(i = start; i <= mid; i++) tmpI[t++] = a[i];

	while(i <= end && j < t)
		a[left++] = a[i] > tmpI[j] ? tmpI[j++] : a[i++];
	
	while(j < t) a[left++] = tmpI[j++];
    
}

void setGemBag(Gem **x, int *N, int *K, int **bag)
{
    scanf("%d %d", N, K);
    
    *x = (Gem *)calloc(*N, sizeof(Gem));
    *bag = (int *)calloc(*K, sizeof(int));
    
    for (int i = 0; i < *N; i++)
        scanf("%d %d", &(*x)[i].weight, &(*x)[i].price);
    for (int i = 0; i < *K; i++)
        scanf("%d", *bag + i);
    
    int *tmpI = (int *)malloc(sizeof(int) * *K);
    Gem *tmpG = (Gem *)malloc(sizeof(Gem) * *N);
    
    sort_Gem(*x, 0, *N - 1, tmpG);
    sort_int(*bag, 0, *K - 1, tmpI);
    free(tmpI);free(tmpG);
}

int removeRoot(Heap *x) // 힙 루트값 삭제
{
    int root = x->h[1];
    SWAP(x->h[1], x->h[x->n], int); x->n--;
    if (x->n > 1)
        downHeap(x, 1);
    
    return root;
}

void insertHeap(Heap *x, int val)
{
    x->h[++x->n] = val;
    int child = x->n;
    int parent;
    while (child > 1)
    {
        parent = child / 2;
        if (parent >= 1 && x->h[parent] < x->h[child])
            SWAP(x->h[child], x->h[parent], int);
        child = parent;
    }
}

void downHeap(Heap *x, int parent) // 힙의 알맞은 위치로
{
    int child;
    do
    {
        child = parent * 2;
        if (child < x->n && x->h[child] < x->h[child + 1])
            child++;
        if (child <= x->n && x->h[child] > x->h[parent])
            SWAP(x->h[child], x->h[parent], int);
        parent = child;
    }while (child <= x->n);
}

Heap *initHeap(int n)
{
    Heap *x = (Heap *)malloc(sizeof(Heap));
    x->n = 0;
    x->h = (int*)calloc(n + 1, sizeof(int));
    return x;
}
/*
4 4
1 100
2 200
13 300
10 500
10
10
10
14*/
```

```Python
import heapq
import sys

N, K = map(int, sys.stdin.readline().split())
gem = []
bag = []

max = []
for i in range(N):
    a, b = map(int, sys.stdin.readline().split())
    gem.append([a, b])

for i in range(K):
    bag.append(int(sys.stdin.readline()))

gem = sorted(gem, key=lambda g : g[0])
bag = sorted(bag)

sum_price = 0
j = 0
for i in range(K):
    while j < N and gem[j][0] <= bag[i]:
        heapq.heappush(max, -gem[j][1])
        j += 1
    if len(max):
        sum_price += -heapq.heappop(max)
print(sum_price)
```
