[트리의 부모찾기](https://www.acmicpc.net/problem/11725)

```C
#include <stdlib.h>
#include <stdio.h>

typedef struct element
{
    int n;
    struct element *next;
}Element;

typedef struct 
{
    int *q;
    int front, rear;
    int size;
}Que;

Element *getNode(int val)
{
    Element *p = malloc(sizeof(Element));
    p->n = val;
    p->next = NULL;
    
    return p;
}

void initQue(Que *x, int N) // 큐 초기화
{
    x->q = malloc(sizeof(int) * N);
    x->front = x->rear = 0;
    x->size = N;
}

int isEmpty(Que x)
{
    return (x.front == x.rear);
}

void enqueue(Que *x, int val) // 큐에 삽입
{
    x->rear = ++x->rear % x->size;
    x->q[x->rear] = val;
}

int dequeue(Que *x) // 큐에서 삭제
{
    x->front = ++x->front % x->size;
    return x->q[x->front];
}

Element *setTree(int *N)
{
    scanf("%d", N);
    Element *x = calloc(*N + 1, sizeof(Element)); // 노드 index에 해당하는 Element 배열 생성
    
    for (int i = 1; i <= *N; i++) 
        x[i].next = NULL;

    
    for (int i = 1; i < *N; i++) // a, b입력 받고, 해당 노드와 연결된 노드를 next로 연결리스트 생성
    {
        int a, b;
        scanf("%d %d", &a, &b);
        Element *A = getNode(a); // a 노드의 연결 노드 모음에 b 추가
        A->next = x[b].next;
        x[b].next = A; 
        
        Element *B = getNode(b); // b 노드의 연결 노드 모음에 a 추가
        B->next = x[a].next;
        x[a].next = B;
    }
    
    return x;
}

int *findParent(Element *x, int N)
{
    Que q; initQue(&q, N);
    int *result = calloc(N + 1, sizeof(int)); // 배열 할당 후 0으로 초기화
    
    enqueue(&q, 1); result[1] = 1; // 루트노드인 1부터 탐색
    while (!isEmpty(q)) // 큐에 아무것도 없을때까지 반복
    {
        int k = dequeue(&q); //큐에서 pop
        for (Element *p = x[k].next; p != NULL; p = p->next)
        {// 해당 노드와 연결된 노드들 탐색
            int connect = p->n;
            
            if (result[connect]) continue; // 부모노드가 저장되어있으면 pass
            result[connect] = k;
            enqueue(&q, connect);
        }
    }
    return result;
}

int main()
{
    int N;
    Element *x = setTree(&N); // 입력받아서 트리를 구성
    
    int *result = findParent(x, N);
    
    for (int i = 2; i <= N; i++)
        printf("%d\n", result[i]);
    return 0;
}
```
