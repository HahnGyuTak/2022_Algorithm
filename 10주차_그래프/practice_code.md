```C
#include <stdlib.h>
#include <stdio.h>

//----------구조체 선언----------
typedef struct Vertex
{
    char vName;
    struct Vertex *next;
}Vertex;

typedef struct Edge
{
    int weight;
    Vertex *v1, *v2;
    struct Edge *next;
}Edge;

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
    
    VinsertLast(x, node);
}

Vertex *findVertex(Graph *x, char vName)
{
    Vertex *p = x->vHead;
    while (p->vName != vName)
        p = p->next;
    return p;
}

void insertEdge(Graph *x, int w, char v1, char v2)
{
    Edge *e = malloc(sizeof(Edge));
    e->weight = w;
    e->v1 = findVertex(x, v1);
    e->v2 = findVertex(x, v2);
    e->next = NULL;
    
    EinsetLast(x, e);
}

//----------응용 알고리즘----------
void findAdj(Graph *x, char name)
{
    Edge *p = x->eHead;
    Vertex *v = findVertex(x, name);
    
    printf("Adjacency node of %c : ", name);
    for (; p != NULL; p = p->next)
    {
        if (p->v1 == v)
            printf("(%c) ", p->v2->vName);
        else if (p->v2 == v)
            printf("(%c) ", p->v1->vName);
    }
    printf("\n");
}

//----------출력----------
void printGraph(Graph *x)
{
    Edge *q = x->eHead;
    while (q != NULL)
    {
        printf("(%c)---%d---(%c)\n", q->v1->vName, q->weight, q->v2->vName);
        q = q->next;
    }
    printf("\n");
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
    return 0;
}
```
