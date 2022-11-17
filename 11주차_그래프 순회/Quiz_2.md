```C
#include <stdlib.h>
#include <stdio.h>
#define FALSE 0
#define TRUE 1

// Define Struct
struct Vertex;

typedef struct IncidentEdge
{
    struct IncidentEdge *next;
    struct Vertex *adjV;
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

// Queue Function
typedef struct Queue
{
    Vertex **q;
    int f, r;
    int size;
}Queue;

void initQueue(Queue *q, int n)
{
    q->q = malloc(sizeof(Queue*) * n);
    q->f = q->r = 0;
    q->size = n;
}

int isEmpty(Queue *q)
{
    return (q->r == q->f);
}

void enqueue(Queue *q, Vertex *v)
{
    q->q[++q->r % q->size] = v;
}

Vertex *dequeue(Queue *q)
{
    Vertex *p = q->q[++q->f % q->size];
    return p;
}

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
    addEdge(V1, V2);

    if (v1 != v2)
        addEdge(V2, V1);
}

void BFS(Graph *x, Vertex *s, int n)
{
    Queue Q; initQueue(&Q, n);
    s->isVisit = TRUE;
    printf("%d\n", s->vName);
    enqueue(&Q, s);
    while (!isEmpty(&Q))
    {
        Vertex *p = dequeue(&Q);
        for (IncidentEdge *i = p->iHead; i ; i = i->next)
        {
            if (i->adjV->isVisit) continue;
            i->adjV->isVisit = TRUE;
            printf("%d\n", i->adjV->vName);
            enqueue(&Q, i->adjV);
        }
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
    BFS(&x, findVertex(&x, s), n);

    return 0;
}
```
