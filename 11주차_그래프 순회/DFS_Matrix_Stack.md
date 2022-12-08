```C
#include <stdio.h>
#include <stdlib.h>
#define TRUE 1
#define FALSE 0

typedef struct Vertex
{
    int vName, isVisit;
    struct Vertex *next;  
}Vertex;

typedef struct Edge
{
    Vertex *v1, *v2;
    int weight;
}Edge;

typedef struct Graph
{
    Vertex *vHead;
    int vcnt;
    Edge ***matrix;
}Graph;

typedef struct Stack
{
    Vertex **v;
    int top;
}Stack;

void initStack(Stack *s, int n)
{
    s->v = calloc(n, sizeof(Vertex *));
    s->top = -1;
}

int isEmpty(Stack *s)
{
    return s->top == -1;
}

void pushStack(Stack *s, Vertex *k)
{
    s->v[++s->top] = k;
}

Vertex *pop(Stack *s)
{
    return s->v[s->top--];
}

void initGraph(Graph *x, int n)
{
    x->matrix = calloc(n, sizeof(Edge**));
    for (int i = 0; i < n; i++)
        x->matrix[i] = calloc(n, sizeof(Edge*));
    x->vHead = NULL;
    x->vcnt = 0;
}

Vertex *findVertex(Graph *x, int v)
{
    Vertex *p = x->vHead;
    while (p!=NULL && p->vName != v)
        p = p->next;
    return p;
}

void makeVertex(Graph *x, int v)
{
    Vertex *new = malloc(sizeof(Vertex));
    new->next = NULL;
    new->vName = v;
    new->isVisit = FALSE;

    if (x->vHead == NULL)
        x->vHead = new;
    else
    {
        Vertex *p = x->vHead;
        while (p->next)
            p = p->next;
        p->next = new;
    }
    x->vcnt++;
}

void insertEdge(Graph *x, int v1, int v2, int w)
{
    Edge *new = malloc(sizeof(Edge));
    new->v1 = findVertex(x, v1);
    new->v2 = findVertex(x, v2);
    new->weight = w;

    x->matrix[v1-1][v2-1] = x->matrix[v2-1][v1-1] = new;
}

void printGraph(Graph *x, int n)
{
    Vertex *p = x->vHead;
    for (; p; p=p->next)
    {
        printf("[%d] :", p->vName);
        for (int i = 0; i < n; i++)
        {
            Edge *q = x->matrix[p->vName-1][i];
            if (q != NULL)
                printf(" (%d-%d, %d)", q->v1->vName, q->v2->vName, q->weight);
        }
        printf("\n");
    }
}

void dfs(Graph *x, Vertex *v)
{
    Stack s; initStack(&s, x->vcnt);
    v->isVisit = TRUE;
    printf("%d\n", v->vName);
    pushStack(&s, v);
    while (!isEmpty(&s))
    {   
        int flag = FALSE;
        v = s.v[s.top];
        for (int i = 0; i < x->vcnt; i++)
        {
            Edge *p = x->matrix[v->vName-1][i];
            if (p)
            {
                if (p->v1->vName == v->vName && !p->v2->isVisit)
                {
                    pushStack(&s, p->v2);
                    p->v2->isVisit = TRUE;
                    printf("%d\n", p->v2->vName);
                    flag = TRUE;
                    v = p->v2;
                    break;
                }
                else if (p->v2->vName == v->vName && !p->v1->isVisit)
                {
                    pushStack(&s, p->v1);
                    p->v1->isVisit = TRUE;
                    printf("%d\n", p->v1->vName);
                    flag = TRUE;
                    v = p->v1;
                    break;
                }
            }
        }
        if (!flag) v = pop(&s);
    }
}

int main()
{
    Graph x;
    int n, m, s;
    scanf("%d %d %d", &n, &m, &s);
    initGraph(&x, n);
    for (int i = 0; i < n; i++)
        makeVertex(&x, i+1);
    for (int i = 0; i < m; i++)
    {
        int a, b;
        scanf("%d %d", &a, &b);
        insertEdge(&x, a, b, 0);
    }
    dfs(&x, findVertex(&x, s));
    //printGraph(&x, 6);
    return 0;
}
```
