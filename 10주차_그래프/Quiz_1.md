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

typedef struct IncidentEdge
{
    int adjVertex;
    Edge *e;
    struct IncidentEdge *next;
}IncidentEdge;

typedef struct Vertex
{
    int vName;
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

void makeVertex(Graph *x, int v)
{
    Vertex *node = malloc(sizeof(Vertex));
    node->vName = v;
    node->next = NULL;
    node->iHead = NULL;
    
    VinsertLast(x, node);
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

void removeEdge(Vertex *v, int av)
{
    IncidentEdge  *p = v->iHead;
    if (p->adjVertex == av)
    {
        v->iHead = p->next;
        free(p);
    }
    else
    {
        while (p->next->adjVertex != av)
            p = p->next;
        IncidentEdge *q = p->next;
        p->next = q->next;
        free(q);
    }
}

void modiftWeight(Graph *x, int v1, int v2, int w)
{
    Edge *q = x->eHead;
    if (findVertex(x, v1) == NULL || findVertex(x, v2) == NULL) 
    {
        printf("-1\n");
        return ;
    }

    if ((q->v1 == v1 && q->v2 == v2) || (q->v2 == v1 && q->v1 == v2))
    {
        if (w)
            q->weight = w;
        else
        {
            Vertex *v = findVertex(x, q->v1);
            removeEdge(v, q->v2);

            if (v1 != v2)
            {
                v = findVertex(x, q->v2);
                removeEdge(v, q->v1);
            }
            x->eHead = q->next;
            free(q);
        }
        return ;
    }

    while (q->next)
    {
        if ((q->next->v1 == v1 && q->next->v2 == v2) || (q->next->v2 == v1 && q->next->v1 == v2))
            break;
        q = q->next;
    }
    if (q->next != NULL)
    {
        if (w)
            q->next->weight = w;
        else
        {
            Vertex *v = findVertex(x, q->next->v1);
            removeEdge(v, q->next->v2);
            if (v1 != v2)
            {
                v = findVertex(x, q->next->v2);
                removeEdge(v, q->next->v1);
            }

            Edge *k = q->next;
            q->next = k->next;
            free(k);
        }
        return ;
    }
    insertEdge(x, w, v1, v2);
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
