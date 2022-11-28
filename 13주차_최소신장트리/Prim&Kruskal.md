#include <stdlib.h>
#include <stdio.h>
#define TRUE 1
#define FALSE 0
#define INF 1000

//----------구조체 선언----------
typedef struct Edge
{
    int weight;
    int v1, v2;
    struct Edge *next;
}Edge;

typedef struct IncidentEdge
{
    int adjVertex;
    Edge *e;
    struct IncidentEdge *next;
}IncidentEdge;

typedef struct Vertex
{
    int vName, isVisit;
    struct Vertex *next;
    IncidentEdge *iHead;
}Vertex;

typedef struct 
{
    Vertex *vHead;
    Edge *eHead;
    int ecnt, vcnt;
}Graph;

//----------그래프 초기화 & 추가----------
void initGraph(Graph *G)
{
    G->vHead = NULL;
    G->eHead = NULL;
    G->ecnt = G->vcnt = 0;
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
    node->iHead = NULL;
    node->isVisit =FALSE;
    
    VinsertLast(x, node);
    x->vcnt++;
}

Vertex *findVertex(Graph *x, int vName)
{
    Vertex *p = x->vHead;
    while (p != NULL && p->vName != vName)
        p = p->next;
    return p;
}

void insertIncident(Vertex *v, int av, Edge *e)
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
        if (r->adjVertex > av)
        {
            v->iHead = i;
            i->next = r;
            return ;
        }
        while (r->next != NULL && r->next->adjVertex < av)
            r = r->next;
        i->next = r->next;
        r->next = i;
    }
}
void insertEdge(Graph *x, int w, int v1, int v2)
{
    Edge *e = malloc(sizeof(Edge));
    e->weight = w;
    e->v1 = v1;
    e->v2 = v2;
    e->next = NULL;
    
    EinsetLast(x, e);
    x->ecnt++;
    Vertex *v = findVertex(x, v1);
    insertIncident(v, v2, e);
    
    if (v1 != v2)
    {
        v = findVertex(x, v2);
        insertIncident(v, v1, e);
    }
}

//----------응용 알고리즘----------
void findAdj(Graph *x, int name)
{
    Vertex *v = findVertex(x, name);
    if (v == NULL) { printf("-1\n"); return ;}

    IncidentEdge *p = v->iHead;
    for (; p != NULL; p = p->next)
        printf(" %d %d", p->adjVertex, p->e->weight);
    
    printf("\n");
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

void incSort(Graph *G, Edge *e[])
{
    int i, min;
    Edge *p = G->eHead;
    for (i = 0; i < G->ecnt; i++)
    {
        e[i] = p;
        p = p->next;
    }

    for (i = 0; i < G->ecnt - 1; i++)
    {
        min = i;
        for (int j = i + 1; j < G->ecnt; j++)
            if(e[j]->weight < e[min]->weight)
                min = j;
        p = e[min];
        e[min] = e[i];
        e[i] = p;
    }
    for (i = 0; i < G->ecnt; i++)
        printf("[%d %d (w %d)] ",e[i]->v1, e[i]->v2, e[i]->weight);
    printf("\n\n");
}

int rFind(int v[], int vName)
{
    if (v[vName] == -1)
        return vName;
    while (v[vName] != -1)
        vName = v[vName];
    return vName;
}

void vUnion(int v[], int v1, int v2)
{
    int r1 = rFind(v, v1);
    int r2 = rFind(v, v2);

    v[r2] = r1;
    printf("    ㄴv[%d] <= %d\n", r2, r1);
}

void kruskal(Graph *g, Edge *e[], int v[])
{
    int eCnt = 0, i = 0;
    int vN1, vN2;
    Edge *p;

    while (eCnt < g->vcnt - 1)
    {
        p = e[i];
        vN1 = rFind(v, p->v1);
        vN2 = rFind(v, p->v2);
        if (vN1 != vN2)
        {
            eCnt++;
            printf("%d. [(%d)(%d) %d\n", eCnt, p->v1, p->v2, p->weight);
            vUnion(v, vN1, vN2);
        }
        i++;
    }
}

int getMinVertex(Graph *g, int dist[])
{
    Vertex *p = g->vHead;
    int vn;
    for (; p != NULL; p = p->next)
        if (p->isVisit == FALSE)
        {
            vn = p->vName;
            break;
        }
    for (p = g->vHead; p != NULL; p = p->next)
        if (p->isVisit == FALSE && dist[p->vName] < dist[vn])
            vn = p->vName;
    return vn;
}

void prim(Graph *g, int vName, int dist[])
{
    Vertex *p = findVertex(g, vName);
    IncidentEdge *q;
    int c;

    dist[p->vName] = 0;
    printf("\n\nPrim : ");
    for (int i = 0; i < g->vcnt; i++)
    {
        c = getMinVertex(g, dist);
        p = findVertex(g, c);
        p->isVisit = TRUE;
        printf("[%d] ", p->vName);

        for (q = p->iHead; q != NULL; q = q->next)
        {
            Vertex *tmp = findVertex(g, q->adjVertex);
            if (tmp->isVisit == FALSE && dist[q->adjVertex] > q->e->weight)
                dist[q->adjVertex] = q->e->weight;
        }
    }
}

int main()
{
    Graph x;
    initGraph(&x);
    
    makeVertex(&x, 1); makeVertex(&x, 2);
    makeVertex(&x, 3); makeVertex(&x, 4);
    makeVertex(&x, 5); makeVertex(&x, 6);
    makeVertex(&x, 7);

    insertEdge(&x, 29, 1, 2);
    insertEdge(&x, 10, 1, 6);
    insertEdge(&x, 16, 2, 3);
    insertEdge(&x, 12, 3, 4);
    insertEdge(&x, 15, 2, 7);
    insertEdge(&x, 18, 4, 7);
    insertEdge(&x, 22, 4, 5);
    insertEdge(&x, 27, 5, 6);
    insertEdge(&x, 25, 5, 7);

    Edge *e[20];
    incSort(&x, e);
    
    int v[10] = {-1, -1, -1, -1, -1, -1, -1, -1, -1, -1};
    kruskal(&x, e, v);
    
    int dist[10] = {INF, INF, INF, INF, INF, INF, INF, INF, INF, INF};
    prim(&x, 7, dist);

    return 0;
}
