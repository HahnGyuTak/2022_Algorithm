```C
#include <stdlib.h>
#include <stdio.h>

//----------구조체 선언----------
typedef struct Edge
{
    int weight;
    int v1, v2;
    struct Edge *next;
}Edge;

typedef struct Vertex
{
    int vName;
    struct Vertex *next;
}Vertex;

typedef struct 
{
    Vertex *vHead;
    Edge *eHead;
    Edge ***matrixEdge;
    int n;
}Graph;

//----------그래프 초기화 & 추가----------
void initGraph(Graph *G, int n)
{
    G->vHead = NULL;
    G->eHead = NULL;
    G->matrixEdge = malloc(sizeof(Edge**) * (n + 1));
    for (int i = 1; i <= n; i++)
        G->matrixEdge[i] = calloc(n + 1, sizeof(Edge*));
    G->n = n;
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

void makeVertex(Graph *x, int v)
{
    Vertex *node = malloc(sizeof(Vertex));
    node->vName = v;
    node->next = NULL;
    
    VinsertLast(x, node);
}

Vertex *findVertex(Graph *x, int vName)
{
    Vertex *p = x->vHead;
    while (p != NULL && p->vName != vName)
        p = p->next;
    return p;
}

void insertEdge(Graph *x, int w, int v1, int v2)
{
    Edge *e = malloc(sizeof(Edge));
    e->weight = w;
    e->v1 = v1;
    e->v2 = v2;
    e->next = NULL;
    
    EinsetLast(x, e);
    
    x->matrixEdge[v1][v2] = x->matrixEdge[v2][v1] = e;
}

//----------응용 알고리즘----------
void findAdj(Graph *x, int name)
{
    Vertex *v = findVertex(x, name);
    if (v == NULL) { printf("-1\n"); return ;}

    for (int i = 1; i <= x->n; i++)
        if (x->matrixEdge[name][i] != NULL)
            printf(" %d %d", i, x->matrixEdge[name][i]->weight);

    printf("\n");
}

void removeEdge(Graph *x, int v1, int v2)
{
    x->matrixEdge[v1][v2] = x->matrixEdge[v2][v1] = NULL;
}

void modiftWeight(Graph *x, int v1, int v2, int w)
{
    Edge *q = x->eHead;
    if (findVertex(x, v1) == NULL || findVertex(x, v2) == NULL) 
    {
        printf("-1\n");
        return ;
    }

    if (x->matrixEdge[v1][v2])
        if (w)
            x->matrixEdge[v1][v2]->weight = x->matrixEdge[v2][v1]->weight = w;
        else
            removeEdge(x, v1, v2);
    else
        insertEdge(x, w, v1, v2);
}

//----------출력----------
void printGraph(Graph *x)
{
    Vertex *p = x->vHead;
    while (p)
    {
        printf("[%d] : ", p->vName);
        for (int i = 1; i <= x->n; i++)
            if (x->matrixEdge[p->vName][i] != NULL)
                printf("(%d - %d) ", i, x->matrixEdge[p->vName][i]->weight);
        printf("\n");
        p = p->next;
    }
}

int main()
{
    Graph x;
    initGraph(&x, 6);
    
    makeVertex(&x, 1); makeVertex(&x, 2);
    makeVertex(&x, 3); makeVertex(&x, 4);
    makeVertex(&x, 5); makeVertex(&x, 6);

    insertEdge(&x, 1, 1, 2);
    insertEdge(&x, 1, 1, 3);
    insertEdge(&x, 1, 1, 4);
    insertEdge(&x, 2, 1, 6);
    insertEdge(&x, 1, 2, 3);
    insertEdge(&x, 4, 3, 5);
    insertEdge(&x, 4, 5, 5);
    insertEdge(&x, 3, 5, 6);
    
    //printGraph(&x);
    
    char fun;
    int a, b, c;
    while (1)
    {
        scanf("%c", &fun);
        switch (fun)
        {
            case 'a':
                scanf("%d", &a);
                findAdj(&x, a);
                break;
            case 'm':
                scanf("%d %d %d", &a, &b, &c);
                modiftWeight(&x, a, b, c);
                break;
            case 'q':
                return 0;
        }
        getchar();
    } 
    
    return 0;
}
```
