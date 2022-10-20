```C
#include <stdlib.h>
#include <stdio.h>

typedef struct HashNode
{
    int key;
    int proveCount;
}HashNode;

typedef struct
{
    HashNode *A;
}HashType;

void init(HashType *H, int M)
{
    for (int i = 0; i< M; i++)
    {
        H->A[i].key = 0;
        H->A[i].proveCount;
    }
}

int isEmpty(HashType *H, int i)
{
    return H->A[i].key == 0 ? 1 : 0;
}

int h(int k, int M)
{
    return k % M;
}

int getnExtBucketLinear(int v, int i, int M)
{
    return (v + i) % M;
}

void searchItem(HashType *H, int M, int key)
{
    int v = h(key, M);
    int i = 0;
    
    while (i < M)
    {
        int b = getnExtBucketLinear(v, i, M);
        if (H->A[b].key == key)
        {
            printf("%d %d\n", b, key);
            return ;
        }
        i++;
    }
    printf("-1\n");

}

void insertItem(HashType *H, int M, int key)
{
    int v = h(key, M);
    int i = 0;
    int count = 0;
    
    while (i < M)
    {
        count++;
        int b = getnExtBucketLinear(v, i, M);
        if (isEmpty(H, b))
        {
            H->A[b].key = key;
            H->A[b].proveCount = count;
            printf("%d\n", b);
            return ;
        }
        i++;
        printf("C");
    }
}

int main()
{
    int M, n;
    scanf("%d %d", &M, &n);
    HashType H;
    H.A = calloc(M, sizeof(HashNode));
    
    char fun; int k;
    while (1)
    {
        getchar();
        scanf("%c", &fun);
        switch (fun)
        {
            case 'i':
                scanf("%d", &k);
                insertItem(&H, M, k);
                break;
            case 's':
                scanf("%d", &k);
                searchItem(&H, M, k);
                break;
            case 'e':
                return 0;
        }
    }
    return 0;
}
```
