```C
#include <stdio.h>
#include <stdlib.h>

typedef struct Edge
{
    int v1, v2;
    int weight;
    struct Edge *next;
}Edge;

typedef struct incidentEdge
{
    Edge *e;
    int av;
    struct incidentEdge *next;
}incidentEdge;

typedef struct Vertex
{
    int vName, isVisit;
    incidentEdge *iHead;
    struct Vertex *next;  
}Vertex;

typedef struct Graph
{
    Vertex *vHead;
    Edge *eHead;
	int vcnt;
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

void initGraph(Graph *x)
{
    x->eHead = NULL;
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
    new->iHead = NULL;
    new->next = NULL;
    new->vName = v;
	new->isVisit = 0;

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

void addIncidentEdge(Vertex *v, int av, Edge *e)
{
    incidentEdge *new = malloc(sizeof(incidentEdge));
    new->av = av;
    new->e = e;
    new->next = NULL;

    if (v->iHead == NULL)
        v->iHead = new;
    else
    {
        incidentEdge *p = v->iHead;
        if (p->av > new->av)
        {
            v->iHead = new;
            new->next = p;
        }
        else
        {
            while (p->next && p->next->av < new->av)
                p = p->next;
            new->next = p->next;
            p->next = new;
        }
    }
}

void insertEdge(Graph *x, int v1, int v2, int w)
{
    Edge *new = malloc(sizeof(Edge));
    new->v1 = v1;
    new->v2 = v2;
    new->weight = w;
    new->next = NULL;

    if (x->eHead == NULL)
        x->eHead = new;
    else
    {
        Edge *p = x->eHead;
        while (p->next)
            p = p->next;
        p->next = new;
    }
    Vertex *V1 = findVertex(x, v1);
    addIncidentEdge(V1, v2, new);

    if (v1 != v2)
    {
        Vertex *V2 = findVertex(x, v2);
        addIncidentEdge(V2, v1, new);
    }
}

void printGraph(Graph *x)
{
    Vertex *p = x->vHead;
    for (; p; p=p->next)
    {
        printf("[%d] :", p->vName);
        for (incidentEdge *q = p->iHead; q; q = q->next)
            printf("(v:%d w:%d)", q->av, q->e->weight);
        printf("\n");
    }
}

void dfs(Graph *x, Vertex *v)
{
	Stack s; initStack(&s, x->vcnt);
	v->isVisit = 1;
	printf("%d\n", v->vName);
	pushStack(&s, v);
	while (!isEmpty(&s))
	{
		int flag = 1;
		v = s.v[s.top];
		//printf("%d 탐색 ->", v->vName);
		for (incidentEdge *p = v->iHead; p; p = p->next)
		{
			Vertex *k = findVertex(x, p->av);
			//printf(" (%d %d)", p->av, k->isVisit);
			if (k->isVisit == 0)
			{
				pushStack(&s, k);
				k->isVisit = 1;
				v = k;
				printf("%d\n", v->vName);
				flag = 0;
				break;
			}
		}
		//printf("\n");
		if (flag) {v = pop(&s);}// printf("%d 이세끼 스택에서 뺌\n", v->vName);}
	}
}

int main()
{
    Graph x;
    int n, m, s;
    scanf("%d %d %d", &n, &m, &s);
    initGraph(&x);
    for (int i = 0; i < n; i++)
        makeVertex(&x, i+1);
    for (int i = 0; i < m; i++)
    {
        int a, b;
        scanf("%d %d", &a, &b);
        insertEdge(&x, a, b, 0);
    }
    dfs(&x, findVertex(&x, s));
    //printGraph(&x);
    return 0;
}
```
