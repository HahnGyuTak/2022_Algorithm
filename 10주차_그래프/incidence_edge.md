```C
#include <stdlib.h>
#include <stdio.h>

//----------구조체 선언----------
typedef struct Edge
{
    int weight;
    char v1, v2;
    struct Edge *next;
}Edge;

typedef struct IncidentEdge
{
    char adjVertex;
    Edge *e;
    struct IncidentEdge *next;
}IncidentEdge;

typedef struct Vertex
{
    char vName;
    struct Vertex *next;
    IncidentEdge *iHead;
}Vertex;

typedef struct 
{
    Vertex *vHead;
    Edge *eHead;
}Graph;

//----------그래프 초기화 & 추가----------
void initGraph(Graph *G)
{
    G->vHead = NULL;
    G->eHead = NULL;
}

void VinsertLast(Graph *x, Vertex *node)
{
    Vertex *p = x->vHead;
    if (p == NULL)
        x->vHead = node;
    else
    {
        while(p->next != NULL)
            p = p->next;
        p->next = node;
    }
}

void EinsetLast(Graph *x, Edge *e)
{
    Edge *p = x->eHead;
    if (p == NULL)
        x->eHead = e;
    else
    {
        while (p->next != NULL)
            p = p->next;
        p->next = e;
    }
}

void makeVertex(Graph *x, char v)
{
    Vertex *node = malloc(sizeof(Vertex));
    node->vName = v;
    node->next = NULL;
    node->iHead = NULL;
    
    VinsertLast(x, node);
}

Vertex *findVertex(Graph *x, char vName)
{
    Vertex *p = x->vHead;
    while (p->vName != vName)
        p = p->next;
    return p;
}

void insertIncident(Vertex *v, char av, Edge *e)
{
    IncidentEdge *i = (IncidentEdge*)malloc(sizeof(IncidentEdge));
    i->adjVertex = av;
    i->e = e;
    i->next = NULL;
    
    IncidentEdge *r = v->iHead;
    if (r == NULL)
        v->iHead = i;
    else
    {
        while (r->next != NULL)
            r = r->next;
        r->next = i;
    }
}
void insertEdge(Graph *x, int w, char v1, char v2)
{
    Edge *e = malloc(sizeof(Edge));
    e->weight = w;
    e->v1 = v1;
    e->v2 = v2;
    e->next = NULL;
    
    EinsetLast(x, e);
    
    Vertex *v = findVertex(x, v1);
    insertIncident(v, v2, e);
    
    v = findVertex(x, v2);
    insertIncident(v, v1, e);
}

//----------응용 알고리즘----------
void findAdj(Graph *x, char name)
{
    Edge *p = x->eHead;
    Vertex *v = findVertex(x, name);
    
    printf("Adjacency node of %c : ", name);
    for (; p != NULL; p = p->next)
    {
        if (p->v1 == v->vName)
            printf("(%c) ", p->v2);
        else if (p->v2 == v->vName)
            printf("(%c) ", p->v1);
    }
    printf("\n");
}

void modiftWeight(Graph *x, char v1, char v2, int w)
{
    Edge *q = x->eHead;
    while (q)
    {
        if ((q->v1 == v1 && q->v2 == v2) || (q->v2 == v1 && q->v1 == v2))
        {
            q->weight = w;
            return ;
        }
    }
}

//----------출력----------
void printGraph(Graph *x)
{
    Vertex *p = x->vHead;
    IncidentEdge *r;
    while (p)
    {
        printf("[%c] : ", p->vName);
        if (p->iHead == NULL) printf("fuc");
        for (r = p->iHead; r; r = r->next)
            printf("(%c - %d) ", r->adjVertex, r->e->weight);
        printf("\n");
        p = p->next;
    }
}

int main()
{
    Graph x;
    initGraph(&x);
    
    makeVertex(&x, 'a'); makeVertex(&x, 'b');
    makeVertex(&x, 'c'); makeVertex(&x, 'd');
    makeVertex(&x, 'e'); makeVertex(&x, 'f');

    insertEdge(&x, 1, 'a', 'b');
    insertEdge(&x, 1, 'a', 'c');
    insertEdge(&x, 1, 'a', 'd');
    insertEdge(&x, 2, 'a', 'f');
    insertEdge(&x, 1, 'b', 'c');
    insertEdge(&x, 4, 'c', 'e');
    insertEdge(&x, 4, 'e', 'e');
    insertEdge(&x, 3, 'e', 'f');
    
    printGraph(&x);
    
    modiftWeight(&x, 'a', 'b', 3);
    printf("\n");
    printGraph(&x);
    return 0;
}
```
