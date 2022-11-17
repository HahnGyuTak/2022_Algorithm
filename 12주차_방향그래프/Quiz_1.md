```C
#include <stdlib.h>
#include <stdio.h>
#define N 101

// Define Struct
struct Vertex;

typedef struct DirectEdge
{
    struct DirectEdge *next;
    struct Vertex *adjV;
}DirectEdge;
typedef struct Vertex
{
    char vName;
    int idx;
    DirectEdge *iHead;
    struct Vertex *next;
}Vertex;
typedef struct Graph
{
    Vertex *vHead;
    int size;
}Graph;

typedef struct Queue
{
   Vertex **q;
   int front, rear;
}Queue;

void initGraph(Graph *x)
{
   x->vHead = NULL;
   x->size = 0;
}

void initQueue(Queue *x)
{
   x->q = (Vertex**)malloc(N * sizeof(Vertex*));
   x->front = x->rear = -1;
}
//------Queue Func-------
int isEmpty(Queue x)
{
   return x.rear == x.front;
}

void enqueue(Queue *x, Vertex *v)
{
   x->q[(++x->rear)%N] = v;
}

Vertex* dequeue(Queue *x)
{
   Vertex *p = x->q[++x->front % N];
   return p;
}
//--------Graph Func---------
Vertex *findVertex(Graph *x, char v, int i)
{
    Vertex *p = x->vHead;
    while(p && (p->vName != v && p->idx != i))
        p = p->next;
    return p;
}

void makeVertex(Graph *x, char v, int i)
{
   Vertex *new = malloc(sizeof(Vertex));
   new->iHead = NULL;
   new->next = NULL;
   new->idx = i;
   new->vName = v;
   x->size++;
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
   DirectEdge *new = malloc(sizeof(DirectEdge));
   new->adjV = av;
   new->next = NULL;

   if (v->iHead == NULL)
      v->iHead = new;
   else
   {
      DirectEdge *p = v->iHead;
      v->iHead = new;
      new->next = p;
   }
}

void makeDirectEdge(Graph *x, int to, int from)
{
    Vertex *V1 = findVertex(x, to, -1);
    Vertex *V2 = findVertex(x, from, -1);
    addEdge(V1, V2);
}

int *inDegree(Graph *x)
{
   int *k = calloc(x->size, sizeof(int));
   for (Vertex *p = x->vHead; p != NULL; p = p->next)
      for (DirectEdge *i = p->iHead; i != NULL; i = i->next)
         k[i->adjV->idx]++;
   return k;
}

void topologicalSort(Graph *x)
{
   int *in = inDegree(x), flag = 0;
   for (int i = 0; i < x->size; i++)
      if (in[i] == 0)
         flag = 1;
   if (flag == 0 && printf("0\n"))  return;

   Queue Q; initQueue(&Q);
   char *topOrdr = (char*)malloc(x->size * sizeof(char));
   int cnt = 0;
   for (int i = 0; i < x->size; i++)
      if (in[i] == 0)
         enqueue(&Q, findVertex(x, 0, i));
   while (!isEmpty(Q))
   {
      Vertex *p = dequeue(&Q);
      topOrdr[cnt++] = p->vName;
      for (DirectEdge *i = p->iHead; i != NULL; i = i->next)
      {
         in[i->adjV->idx]--;
         if (in[i->adjV->idx] == 0)
            enqueue(&Q, i->adjV);
      }
   }
   if (cnt != x->size && printf("0\n"))  return;
   else
      for (int i = 0; i < x->size; i++)
         printf("%c ", topOrdr[i]);
}

int main()
{
   Graph x; initGraph(&x);
   int n, m;
   char name, name1;
   scanf("%d", &n); 
   for (int i = 0; i < n; i++)
   {
      getchar();
      scanf("%c", &name); 
      makeVertex(&x, name, i);
   }
   scanf("%d", &m);
   for (int i = 0; i < m; i++)
   {
      getchar();
      scanf("%c %c", &name, &name1);
      makeDirectEdge(&x, name, name1);
   }
   
   topologicalSort(&x);
   return 0;
}
```
