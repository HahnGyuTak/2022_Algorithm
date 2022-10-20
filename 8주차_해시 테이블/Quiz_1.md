```C
#include <stdlib.h>
#include <stdio.h>
//#define M 13

typedef struct HashNode
{
    int key;
    struct HashNode *next;
}HashNode;

void init(HashNode *H, int M)
{
    for (int i = 0; i < M; i++)
    {
        H[i].key = 0;
        H[i].next = NULL;
    }
}

int h(int k, int M)
{
    return k % M;
}

int searchItem(HashNode *H, int M, int key)
{
    int v = h(key, M);
    int cnt = 0;
    for (HashNode *p = H[v].next; p != NULL; p = p->next)
    {
        cnt++;
        if (p->key == key)
            return cnt;
    }
    return 0;
}

void insertItem(HashNode *H, int M, int key)
{
    int v = h(key, M);
    
    HashNode *p = malloc(sizeof(HashNode));
    p->key = key;
    p->next = H[v].next;
    H[v].next = p;
}

int removeItem(HashNode *H, int M, int key)
{
    int v = h(key, M);
    HashNode *p = H + v;
    int cnt = 1;
    while (p->next != NULL)
    {
        if (p->next->key == key)
            break;
        p = p->next;
        cnt++;
    }
    if (p->next == NULL) return 0;
    
    HashNode *tmp = p->next;
    p->next = p->next->next;
    free(tmp);
    return cnt;
}

void print(HashNode *H, int M)
{
    for (int i = 0; i < M; i++)
    {
        //printf("H[%02d] : ", i);
        for (HashNode *p = H[i].next; p != NULL; p = p->next)
            printf(" %d ", p->key);
        //printf("\n");
    }
}

int main()
{
    int K;
    scanf("%d", &K);
    HashNode *H = malloc(K * sizeof(HashNode));
    init(H, K);
    
    char fun; int k;
    while (1)
    {
        getchar();
        scanf("%c", &fun);
        switch (fun)
        {
            case 'i':
                scanf("%d", &k);
                insertItem(H, K, k);
                break;
            case 's':
                scanf("%d", &k);
                printf("%d\n", searchItem(H, K, k));
                break;
            case 'd':
                scanf("%d", &k);
                printf("%d\n", removeItem(H, K, k));
                break;
            case 'p':
                print(H, K);
                break;
            case 'e':
                return 0;
        }
    }
    return 0;
}
```
