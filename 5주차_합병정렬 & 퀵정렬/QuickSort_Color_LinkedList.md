<img width="910" alt="image" src="https://user-images.githubusercontent.com/50629765/195996264-d156ff36-f482-4aef-aaf9-5980c5176ae4.png">


```C
#include <stdlib.h>
#include <stdio.h>
#include <time.h>
#define swap(A, B, type) do{ type tmp = A; A = B; B = tmp;}while(0)

typedef struct node
{
    struct node *next, *prev;
    char color;
}Node;

typedef struct List
{
    Node *H, *T;
    int n;
}List;

Node *getNode(char x)
{
    Node *p = malloc(sizeof(Node));
    p->next = p->prev = NULL;
    p->color = x;
}

void initList(List *x)
{
    x->H = malloc(sizeof(Node));
    x->T = malloc(sizeof(Node));
    
    x->H->next = x->T;
    x->T->prev = x->H;
    x->H->prev = x->T->next = NULL;
    
    x->n = 0;
}

void addLast(List *x, char d)
{
    Node *tmp = x->T->prev;
    Node *p = getNode(d);
    
    tmp->next = p;
    p->next = x->T;
    x->T->prev = p;
    p->prev = tmp;
    x->n++;
}

void inPlaceQuickSort(Node *l, Node *r, char color)
{
    Node *left = l;
    Node *right = r;
    do
    {
        while (left != right && left->color == color) left = left->next;
        while (left != right && right->color != color) right = right->prev;
        if (left != right)
            swap(left->color, right->color, char);
    } while (left != right && left->prev != right);
    
    if (color == 'R') inPlaceQuickSort(left, r, 'G');
}

int main()
{
    int N = 9;
    char c[10] = "BBBBBGGGR";
    List x; initList(&x);
    
    for (int i = 0; i < 9; i++)
        addLast(&x, c[i]);
        
    inPlaceQuickSort(x.H->next, x.T->prev, 'R');
    
    for (Node *p = x.H; p != x.T; p = p->next)
        printf(" %c", p->color);
    return 0;
}
```

## 여러가지 색깔 version
```C
#include <stdlib.h>
#include <stdio.h>
#include <time.h>
#define swap(A, B, type) do{ type tmp = A; A = B; B = tmp;}while(0)

typedef struct node
{
    struct node *next, *prev;
    char color;
}Node;

typedef struct List
{
    Node *H, *T;
    int n;
}List;

Node *getNode(char x)
{
    Node *p = malloc(sizeof(Node));
    p->next = p->prev = NULL;
    p->color = x;
}

void initList(List *x)
{
    x->H = malloc(sizeof(Node));
    x->T = malloc(sizeof(Node));
    
    x->H->next = x->T;
    x->T->prev = x->H;
    x->H->prev = x->T->next = NULL;
    
    x->n = 0;
}

void addLast(List *x, char d)
{
    Node *tmp = x->T->prev;
    Node *p = getNode(d);
    
    tmp->next = p;
    p->next = x->T;
    x->T->prev = p;
    p->prev = tmp;
    x->n++;
}

void inPlaceQuickSort(Node *l, Node *r, char *c, int now)
{
    Node *left = l;
    Node *right = r;
    char color = c[now];
    
    do
    {
        while (left != right && left->color == color) left = left->next;
        while (left != right && right->color != color) right = right->prev;
        if (left != right)
            swap(left->color, right->color, char);
    } while (left != right && left->prev != right);
    
    if (now < 25) inPlaceQuickSort(left, r, c, now + 1);
}

int main()
{
    int N = 34;
    char c[100] = "MNBVCXZLKJJHGFDSAAPOIUYYTTRREUREWQ";
    char k[27] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    List x; initList(&x);
    
    for (int i = 0; i < N; i++)
        addLast(&x, c[i]);
        
    inPlaceQuickSort(x.H->next, x.T->prev, k, 0);
    
    for (Node *p = x.H; p != x.T; p = p->next)
        printf(" %c", p->color);
    return 0;
}
```
