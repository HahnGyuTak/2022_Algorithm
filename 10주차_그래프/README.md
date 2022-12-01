[바이러스](https://www.acmicpc.net/problem/2606)

# Python
```Python
# 딕셔너리로 그래프 구현
import sys 
input = sys.stdin.readline
def dfs(i):
    virus[i] = True
    for adj in graph[i]:
        if not virus[adj]:
            dfs(adj)

computer, network = int(input()), int(input())
connect = [list(map(int, input().split())) for _ in range(network)]

virus = [False for _ in range(computer + 1)] 
graph ={}
for p, q in connect:
    if p not in graph:
        graph[p] = []
    if q not in graph:
        graph[q] = []
    graph[p].append(q)
    graph[q].append(p)

dfs(1)
print(virus.count(True)-1)
```

# C
```C
#include <stdlib.h> //그래프 구조 구현
#include <stdio.h>
#define FALSE 0
#define TRUE 1

// Define Struct
struct Vertex; // 상호 참조를 위한 원형 선언

typedef struct IncidentEdge // 간선 노드
{
    struct IncidentEdge *next; // 다음 간선 연결
    struct Vertex *adjV; // 인접 정점의 주소 저장
}IncidentEdge;
typedef struct Vertex // 정점 노드
{
    int vName, isVisit;
    IncidentEdge *iHead; // 인접 정점을 연결한 간선 리스트
    struct Vertex *next; // 정점간의 연결
}Vertex;
typedef struct Graph
{
    Vertex *vHead; // 정점 리스트
}Graph;

//Graph Function
void initGraph(Graph *x)
{
    x->vHead = NULL;
}

Vertex *findVertex(Graph *x, int v)
{
    Vertex *p = x->vHead;
    while(p && p->vName != v)
        p = p->next;
    return p;
}

void makeVertex(Graph *x, int v)
{
    Vertex *new = malloc(sizeof(Vertex));
    new->iHead = NULL;
    new->isVisit = FALSE;
    new->next = NULL;
    new->vName = v;

    if (x->vHead == NULL)
        x->vHead = new;
    else
    {
        Vertex *p = x->vHead;
        while(p->next != NULL)
            p = p->next;
        p->next = new;
    }
}

void addEdge(Vertex *v, Vertex *av)
{
    IncidentEdge *new = malloc(sizeof(IncidentEdge));
    new->adjV = av;
    new->next = NULL;

    if (v->iHead == NULL)
        v->iHead = new;
    else
    {
        IncidentEdge *p = v->iHead;
        if (p->adjV->vName > av->vName)
        {
            v->iHead = new;
            new->next = p;
        }
        else
        {
            while (p->next && p->next->adjV->vName < av->vName)
                p = p->next;
            IncidentEdge *q = p->next;
            p->next = new;
            new->next = q;
        }
    }
}

void makeIncidentEdge(Graph *x, int v1, int v2)
{
    Vertex *V1 = findVertex(x, v1);
    Vertex *V2 = findVertex(x, v2);
    addEdge(V1, V2); //V1과 V2를 연결

    if (v1 != v2)
        addEdge(V2, V1);
}

void DFS(Graph *x, Vertex *s, int *cnt)
{
    s->isVisit = TRUE; // 정점 s의 방문 여부 표시
    (*cnt)++;          // 개수 + 1
    IncidentEdge *p = s->iHead;
    while(p)           // 정점에 연결된 간선들을 탐색
    {
        if (p->adjV->isVisit == FALSE) // 방문 여부가 False면 
            DFS(x, p->adjV, cnt);      // 재귀 호출
        p = p->next;
    }
}

int main()
{
    Graph x; initGraph(&x);

    int n, m, s;
    scanf("%d", &n);
    scanf("%d", &m);
    for (int i = 1; i <= n; i++) makeVertex(&x, i); // 1부터 n까지 정점 생성
    for (int i = 1; i <= m; i++)
    {
        int p, q;
        scanf("%d %d", &p, &q);
        makeIncidentEdge(&x, p, q);
    }
    int cnt = 0;
    DFS(&x, x.vHead, &cnt); // 1 부터 DFS 시작
    printf("%d", cnt - 1);
    return 0;
}
```
