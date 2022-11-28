```C
#include <stdio.h>
#include <stdlib.h>
#define TRUE 1
#define FALSE 0
#define INF 1000000000

typedef struct Vertex
{
	int vName, isVist;
	struct Vertex *next;
}Vertex;

typedef struct Edge
{
	Vertex *v1, *v2;
	int weight;
	struct Edge *next;
}Edge;

typedef struct Graph
{
	Vertex *vHead;
	Edge *eHead;
	int eCnt, vCnt;
}Graph;

void initGraph(Graph *x)
{
	x->vHead = NULL;
	x->eHead = NULL;
	x->eCnt = x->vCnt = 0;
}

Vertex *findVertex(Graph *x, int v)
{
	Vertex *p = x->vHead;
	while (p->vName != v)
		p = p->next;
	return p;
}

void insertVertex(Graph *g, int vName)
{
	Vertex *new = malloc(sizeof(Vertex));
	new->vName = vName;
	new->isVist = FALSE;
	new->next = NULL;

	if (g->vHead == NULL)
		g->vHead = new;
	else
	{
		Vertex *p = g->vHead;
		while (p->next)
			p = p->next;
		p->next = new;
	}
	g->vCnt++;
}

void makeEdge(Graph *x, int v1, int v2, int w)
{
	Edge *new = malloc(sizeof(Edge));
	new->v1 = findVertex(x, v1);
	new->v2 = findVertex(x, v2);
	new->weight = w;
	new->next = NULL;

	Edge *q = x->eHead;
	if (q == NULL)
		x->eHead = new;
	else
	{
		if (x->eHead->weight > new->weight)
		{
			x->eHead = new;
			new->next = q;
		}
		else
		{
			while(q->next && q->next->weight < new->weight)
				q = q->next;
			new->next = q->next;
			q->next = new;
		}
	}
	x->eCnt++;
}

int rFind(int n, int v[])
{
	if (v[n] == INF)
		return n;
	while (v[n] != INF)
		n = v[n];
	return n;
}

void vUnion(int v1, int v2, int v[])
{
	int r1 = rFind(v1, v);
	int r2 = rFind(v2, v);

	v[r2] = r1;
}

int kruskal(Graph *g, Edge *e[], int v[])
{
	int k = 0, sum = 0;
	for(int i = 1; i <= g->vCnt - 1; k++)
	{
		int v1 = rFind(e[k]->v1->vName, v);
		int v2 = rFind(e[k]->v2->vName, v);
		if (v1 != v2)
		{
			vUnion(v1, v2, v);
			printf(" %d", e[k]->weight);
			sum += e[k]->weight;
			i++;
		}
	}
	return sum;
}

int main()
{
	Graph g; initGraph(&g);
	int n, m; scanf("%d %d", &n, &m);
	for (int i = 1; i <= n; i++)
		insertVertex(&g, i);
	for (int i = 0; i < m; i++)
	{
		int v1,v2, w;
		scanf("%d %d %d", &v1, &v2, &w);
		makeEdge(&g, v1, v2, w);
	}
	// makeEdge(&g, 1, 2, 2); makeEdge(&g, 1, 3, 6); makeEdge(&g, 1, 5, 7);
	// makeEdge(&g, 2, 3, 5); makeEdge(&g, 2, 4, 7);
	// makeEdge(&g, 3, 4, 9); makeEdge(&g, 3, 5, 8);
	// makeEdge(&g, 4, 6, 4); makeEdge(&g, 5, 6, 3);
	//printGraph(&g);
	
	Edge **e = (Edge**)calloc(m, sizeof(Edge*)), *q = g.eHead;

	for (int i = 0; i < m; i++, q = q->next)
	{
		e[i] = q;
		//printf("(%d)--- %d ---(%d)\n", q->v1->vName, q->weight, q->v2->vName);
	}
	int *v = calloc(n+1, sizeof(int));
	for (int i = 0; i < n+1; i++)
		v[i] = INF;
	printf("\n%d", kruskal(&g, e, v));
	return 0;
}
```
