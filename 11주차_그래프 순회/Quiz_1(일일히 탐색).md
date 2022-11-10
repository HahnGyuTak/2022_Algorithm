## 탐색할때마다 findVertex로 그때그때 노드 찾기
```C
#include <stdlib.h>
#include <stdio.h>
#define FALSE 0
#define TRUE 1

// Define Struct
typedef struct IncidentEdge
{
    struct IncidentEdge *next;
    int adjV;
}IncidentEdge;
typedef struct Vertex
{
    int vName, isVisit;
    IncidentEdge *iHead;
    struct Vertex *next;
}Vertex;
typedef struct Graph
{
    Vertex *vHead;
}Graph;

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

void addEdge(Vertex *v, int av)
{
    IncidentEdge *new = malloc(sizeof(IncidentEdge));
    new->adjV = av;
    new->next = NULL;

    if (v->iHead == NULL)
        v->iHead = new;
    else
    {
        IncidentEdge *p = v->iHead;
        if (p->adjV > av)
        {
            v->iHead = new;
            new->next = p;
        }
        else
        {
            while (p->next && p->next->adjV < av)
                p = p->next;
            IncidentEdge *q = p->next;
            p->next = new;
            new->next = q;
        }
    }
}

void makeIncidentEdge(Graph *x, int v1, int v2)
{
    Vertex *v = findVertex(x, v1);
    addEdge(v, v2);

    if (v1 != v2)
    {
        v = findVertex(x, v2);
        addEdge(v, v1);
    }
}

void DFS(Graph *x, Vertex *s)
{
    s->isVisit = TRUE;
    printf("%d\n", s->vName);
    IncidentEdge *p = s->iHead;
    while(p)
    {
        Vertex *tmp = findVertex(x, p->adjV);
        p = p->next;
        if (tmp->isVisit) continue;
        DFS(x, tmp);
    }
}

int main()
{
    Graph x; initGraph(&x);
    /*makeVertex(&x, 1); makeVertex(&x, 2); makeVertex(&x, 3);
    makeVertex(&x, 4); makeVertex(&x, 5); makeVertex(&x, 6);
    makeVertex(&x, 7);
    
    makeIncidentEdge(&x, 1, 2); makeIncidentEdge(&x, 3, 2);
    makeIncidentEdge(&x, 3, 4); makeIncidentEdge(&x, 4, 5);
    makeIncidentEdge(&x, 5, 6); makeIncidentEdge(&x, 7, 2);
    makeIncidentEdge(&x, 7, 4); makeIncidentEdge(&x, 7, 5);
    makeIncidentEdge(&x, 1, 6);*/
    int n, m, s;
    scanf("%d %d %d", &n, &m, &s);
    for (int i = 1; i <= n; i++) makeVertex(&x, i);
    for (int i = 1; i <= m; i++)
    {
        int p, q;
        scanf("%d %d", &p, &q);
        makeIncidentEdge(&x, p, q);
    }

    DFS(&x, findVertex(&x, s));

    return 0;
}
```
