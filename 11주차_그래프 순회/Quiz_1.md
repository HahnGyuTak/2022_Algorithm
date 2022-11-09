```C
#include <stdlib.h>
#include <stdio.h>

// Define Struct
typedef struct Edge
{
    int v1, v2;
    struct Edge *next;
    int weight;    
}Edge;
typedef struct IncidentEdge
{
    Edge *e;
    struct IncidentEdge *next;
    int adjV;
}IncidentEdge;
typedef struct Vertex
{
    int vName;
    IncidentEdge *iHead;
    struct Vertex *next;
}Vertex;
typedef struct Graph
{
    Vertex *vHead;
    Edge *eHead;
}Graph;

void initGraph(Graph *x)
{
    x->eHead = NULL;
    x->vHead = NULL;
}

Vertex *findVertex(Graph *x, int v)
{
    Vertex *p = x->vHead;
    while (p && p->vName != v)
        p = p->next;
    return p;
}

//Make Graph Func
void makeVertex(Graph *x, int v)
{
    Vertex *newNode = malloc(sizeof(Vertex));
    newNode->vName = v;
    newNode->iHead = NULL;
    newNode->next = NULL;

    if (x->vHead == NULL)
        x->vHead = newNode;
    else
    {
        Vertex *p = x->vHead;
        while(p->next != NULL)
            p = p->next;
        p->next = newNode;
    }
}

void makeIncidentEdge(Vertex *v, Edge *e, int av)
{
    IncidentEdge *new = malloc(sizeof(IncidentEdge));
    new->adjV = av;
    new->e = e;
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
            return ;
        }
        while(p->next != NULL && p->next->adjV < av)
            p = p->next;
        if (p->next)
            new->next = p->next->next;
        p->next = new;
    }
}

void makeEdge(Graph *x, int v1, int v2, int w)
{
    Edge *new = malloc(sizeof(Edge));
    new->next = NULL;
    new->v1 = v1;
    new->v2 = v2;
    new->weight = w;

    if (x->eHead == NULL)
        x->eHead = new;
    else
    {
        Edge *p = x->eHead;
        while(p->next != NULL)
            p = p->next;
        p->next = new;
    }

    Vertex *v = findVertex(x, v1);
    makeIncidentEdge(v, new, v2);
    if (v1 != v2)
    {
        Vertex *v = findVertex(x, v2);
        makeIncidentEdge(v, new, v1);
    }
}

// Print Graph
void printGrap(Graph *x)
{
    Edge *p = x->eHead;
    while (p)
    {
        printf("v1 : %d, v2 : %d, weight : %d\n", p->v1, p->v2, p->weight);
        p = p->next;
    }
}

void printAdjacencyVertex(Graph *x)
{
    Vertex *p = x->vHead;
    while (p)
    {
        printf("%d Vertex connect : ", p->vName);
        IncidentEdge *q = p->iHead;
        while (q)
        {
            printf("%d ",q->adjV);
            q = q->next;
        }
        printf("\n");
        p = p->next;
    }
}

void DFS(Graph *x, Vertex *s, int *check)
{
    printf(" %d", s->vName);
    check[s->vName] = 1;
    IncidentEdge *p = s->iHead;
    while (p)
    {
        int v = p->adjV;
        if (!check[v])
            DFS(x, findVertex(x, v), check);
        p = p->next;
    }
}

int main()
{
    Graph G; initGraph(&G);
    int n ,m, s;
    scanf("%d %d %d", &n, &m, &s);
    for (int i = 1; i <= n; i++) makeVertex(&G, i);
    for (int i = 0; i < m; i++) 
    {
        int a, b;
        scanf("%d %d", &a, &b);
        makeEdge(&G, a, b, 1);
    }
    printAdjacencyVertex(&G);
    Vertex *start = findVertex(&G, s);
    int *check = calloc(n + 1, sizeof(int));
    DFS(&G, start, check);
}
```
