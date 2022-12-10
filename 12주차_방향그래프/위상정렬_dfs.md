```C
#include <stdio.h>
#include <stdlib.h>
#define FALSE 0
#define TRUE 1
#define SIZE 10
typedef struct
{
    char elem[SIZE];
    int top;
}Stack;

void initStack(Stack* S)
{
    S->top = -1;
}

int isStackEmpty(Stack* S)
{
    return S->top == -1;
}

void push(Stack *q, char v)
{
    q->elem[++q->top] = v;
}

char pop(Stack *q)
{
    //printf("%c < 이세끼 뻄\n", q->elem[q->f+1]);
    return q->elem[q->top--];
}

typedef struct IncidentEdge
{
    char aName;
    struct IncidentEdge* next;
}IncidentEdge;

typedef struct Vertex
{
    char vName;
    int isVisit;
    IncidentEdge* iHead;
    struct Vertex* next;
}Vertex;

typedef struct
{
    Vertex* vHead;
    int vcnt;
}GraphType;

void initGraph(GraphType* G)
{
    G->vHead = NULL;
    G->vcnt = 0;
}

Vertex *findVertex(GraphType *G, char v1)
{
    Vertex *p = G->vHead;
    while (p && p->vName != v1)
        p = p->next;
    return p;
}

void makeVertex(GraphType *G, char vn)
{
    Vertex *new = malloc(sizeof(Vertex));
    new->iHead = NULL;
    new->isVisit = FALSE;
    new->next = NULL;
    new->vName = vn;

    Vertex *p = G->vHead;
    if (p == NULL)
        G->vHead = new;
    else
    {
        while (p->next)
            p = p->next;
        p->next = new;
    }
    G->vcnt++;
}

void makeIncidentEdge(Vertex *v, char av)
{
    IncidentEdge *new = malloc(sizeof(IncidentEdge));
    new->aName = av;
    new->next = NULL;

    IncidentEdge *p = v->iHead;
    v->iHead = new;
    new->next = p;
}

void insertEdge(GraphType* G, char v1, char v2)
{
    Vertex* v = findVertex(G, v1);
    makeIncidentEdge(v, v2);
}

void print(GraphType* G)
{
    Vertex* p = G->vHead;
    IncidentEdge* q;
    for (; p != NULL; p = p->next)
    {
        printf("(%c) : ", p->vName);
        for (q = p->iHead; q != NULL; q = q->next)
            printf("(%c) ", q->aName);
        printf("\n");
    }
    printf("\n");
}

void dfs_sort(GraphType *G, Vertex *v, int visit[], Stack *s)
{
    visit[v->vName - G->vHead->vName] = TRUE;
    for (IncidentEdge *q = v->iHead; q; q = q->next)
    {
        if (!visit[q->aName - G->vHead->vName])
            dfs_sort(G, findVertex(G, q->aName), visit, s);
    }
    push(s, v->vName);
}

void topologicalSort(GraphType *G)
{
    int *visit = calloc(G->vcnt, sizeof(int));
    Stack s; initStack(&s);

    for (int i = 0; i < G->vcnt; i++)
        if (!visit[i])
            dfs_sort(G, findVertex(G, G->vHead->vName + i), visit, &s);
    
    for (int i = s.top; i >= 0; i--)
        printf("%c ", s.elem[i]);
}

int main()
{
    GraphType G;
    initGraph(&G);
    char m,n;
    int o;
    scanf("%d", &o); getchar();
    for (int i = 0; i < o; i++)
    {
        scanf("%c ", &m);
        makeVertex(&G, m);
    }
    scanf("%d", &o); 
    for (int i = 0; i < o; i++)
    {
        getchar();
        scanf("%c %c", &m, &n);
        insertEdge(&G, m, n);;
    }

    print(&G);

    topologicalSort(&G);
}
```
