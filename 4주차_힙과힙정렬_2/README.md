[카드 정렬하기](https://www.acmicpc.net/problem/1715)

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct heap
{
    int *h;
    int n;
}Heap;

/*----------Heap-----------*/
int     setHeap(Heap *x); // 입력받고 힙 구성
void    insertHeap(Heap *x, int val); // 힙 삽입
int     removeRoot(Heap *x); // 루트값 삭제
void    downHeap(Heap *x, int parent);
void    rBuildHeap(Heap *x, int i);
void    swap(int *a, int *b);

/*----------Compare--------*/
int     printCompareCnt(Heap *x);

int main()
{
    Heap x;
    if (!setHeap(&x)) // 힙 구성에서 size가 1이면 종료
        return 0;
    
    printf("%d\n", printCompareCnt(&x));
    return 0;
}
//----------Compare--------
int printCompareCnt(Heap *x)
{
    int result = 0;
    
    while (x->n > 1)
    {
        int r1 = removeRoot(x); //최소값 뽑기
        int r2 = removeRoot(x); // 두번째 최소값 뽑기
        
        result += r1 + r2; // 두 묶음 합 = 비교하는 과정 
        if (!x->n) // 힙에 남아있는 value가 없으면 종료
            break;
        insertHeap(x, r1 + r2); // 합친 묶음을 힙에 추가
    }
    free(x->h);
    return result;
}

//----------Heap-----------

int setHeap(Heap *x)
{
    scanf("%d", &x->n);
    if (x->n == 1 && printf("0\n")) // size가 1이면 0 print하고 종료
        return 0;

    x->h = (int *)calloc(x->n + 1, sizeof(int));
    for (int i = 1; i <= x->n; i++)
        scanf("%d", x->h + i);
    
    rBuildHeap(x, 1); // 상향식 힙 구성
    return 1;
}

int removeRoot(Heap *x) // 힙 루트값 삭제
{
    int root = x->h[1];
    swap(x->h + 1, x->h + x->n--);
    if (x->n > 1)
        downHeap(x, 1);
    
    return root;
}

void    insertHeap(Heap *x, int val) // 힙에 삽입
{
    x->h[++x->n] = val;
    int child = x->n;
    int parent;
    while (child > 1)
    {
        parent = child / 2;
        if (parent >= 1 && x->h[parent] > x->h[child])
            swap(x->h + parent, x->h + child);
        child = parent;
    }
}

void rBuildHeap(Heap *x, int i) // 힙으로 만들기
{
    if (i * 2 > x->n)
        return ;
    if (i * 2 <= x->n)
        rBuildHeap(x, i * 2);
    if (i * 2 + 1 <= x->n)
        rBuildHeap(x, i * 2 + 1);
    downHeap(x, i);
}

void downHeap(Heap *x, int parent) // 힙의 알맞은 위치로
{
    int child;
    do
    {
        child = parent * 2;
        if (child < x->n && x->h[child] > x->h[child + 1])
            child++;
        if (child <= x->n && x->h[child] < x->h[parent])
            swap(x->h + child, x->h + parent);
        parent = child;
    }while (child <= x->n);
}

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
```
