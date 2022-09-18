[백준 : 국회의원 선거 _ 우선순위 큐(힙) 사용](https://www.acmicpc.net/problem/1417)
```C
#include <stdio.h>
#include <stdlib.h>

typedef struct heap
{
    int N;         // 힙 크기
    int h[50];     // 힙에 사용할 배열
}Heap;

void    swap(int *a, int *b);
void    downheap(Heap *h, int parent);
void    make_heap(Heap *h);
int     give_me_your_vote(Heap *v);

int main()
{
    Heap vote;
    
    scanf("%d", &vote.N);
    for (int i = 0; i < vote.N; i++)
        scanf("%d", vote.h + i);
    make_heap(&vote);//     다솜의 표를 제외하고 index 1부터 힙상태로 만들기
    
   printf("%d\n", give_me_your_vote(&vote));  // give_me_your_vote함수를 통해 매수해야할 사람 수 출력
    return 0;
}

//------------------------------------------------------------

int give_me_your_vote(Heap *v)
{
    if (!v->N)
        return 0;
    int dasom = v->h[0]; // 다솜이의 최종 득표 수
    int *max_vote = v->h + 1; // 기존의 득표 수
    
    while (dasom <= *max_vote) // 다솜이 득표 수가 최대힙의 루트값보다 작을때 동작
    {
        dasom++;
        v->h[1]--;
        downheap(v, 1); // 감소한 루트값을 힙의 올바른 자리로 이동
    }
    return (dasom - v->h[0]);
}

void make_heap(Heap *h) // 상향식 힙 생성
{
    h->N--;   // 다솜이를 제외 -> 1 감소
    for (int i = h->N / 2; i >= 1; i--)
        downheap(h, i);
}

void downheap(Heap *h, int parent) // 재귀 형식의 downheap
{
    int child = parent * 2;
    if (child > h->N)
        return ;
        
    if (child < h->N && h->h[child] < h->h[child + 1])
        child ++;
    
    if (child <= h->N && h->h[child] > h->h[parent])
        swap(h->h + child, h->h + parent);
    
    downheap(h, child);
}

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
```
