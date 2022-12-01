```C
#include <stdio.h>
#include <stdlib.h>
#define TRUE 1
#define FALSE 0
#define INF 1000000000

struct Edge;

typedef struct Vertex
{
	int vName, isVist;
	struct Edge *eHead;
	struct Vertex *next;
}Vertex;

typedef struct Edge
{
	Vertex *av;
	int weight;
	struct Edge *next;
}Edge;

typedef struct Graph
{
	Vertex *vHead;
	int vCnt;
}Graph;

void initGraph(Graph *x)
{
	x->vHead = NULL;
	x->vCnt = 0;
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
	new->eHead = NULL;

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

void addEdge(Vertex *v1, Vertex *v2, int w)
{
	Edge *new = malloc(sizeof(Edge));
	new->av = v2;
	new->weight = w;
	new->next = NULL;

	Edge *q = v1->eHead;
	if (q == NULL)
		v1->eHead = new;
	else
	{
		if (v1->eHead->weight > new->weight)
		{
			v1->eHead = new;
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
}

void makeEdge(Graph *x, int v1, int v2, int w)
{
	Vertex *V1 = findVertex(x, v1);
	Vertex *V2 = findVertex(x, v2);

	addEdge(V1, V2, w);
	if (v1 != v2)
		addEdge(V2, V1, w);
}

void printGraph(Graph *x)
{
	for (Vertex *p = x->vHead; p; p = p->next)
	{
		printf("{%d} :", p->vName);
		for(Edge *q = p->eHead; q; q = q->next)
			printf(" (av : %d, w : %d)",  q->av->vName, q->weight);
		printf("\n");
	}
}

Vertex *getMinVertex(Graph *x, int d[])
{
	Vertex *p = x->vHead;
	Vertex *min;
	for(; p; p = p->next)
		if (p->isVist == FALSE)
		{
			min = p;
			break;
		}
	for(p = x->vHead; p; p = p->next)
		if (p->isVist == FALSE && d[p->vName] < d[min->vName])
			min = p;
	return min;
}

void dijkstra(Graph *x, int v, int dis[])
{
	dis[v] = 0;
	for (int i = 0; i < x->vCnt; i++)
	{
		Vertex *p = getMinVertex(x, dis);
		p->isVist = TRUE;
		for (Edge *q = p->eHead; q; q = q->next)
			if (q->av->isVist == FALSE && dis[q->av->vName] > dis[p->vName] + q->weight )
				dis[q->av->vName] = dis[p->vName] + q->weight;
		//printf("[%d]\n", p->vName);
		// for (int i = 1; i <= x->vCnt; i++)
		// 		printf("%d %d\n", i, dis[i]);
		// getchar();
		p = p->next;
	}
}

int main()
{
	Graph g; initGraph(&g);
	int n, m,s; scanf("%d %d %d", &n, &m, &s);
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

	int *dis = calloc(n+1, sizeof(int));
	for (int i = 0; i < n+1; i++)
		dis[i] = INF;
	dijkstra(&g, s, dis);
	for (int i = 1; i < n+1; i++)
		if (i != s && dis[i] != INF)
			printf("%d %d\n", i, dis[i]);
	return 0;
}
```
