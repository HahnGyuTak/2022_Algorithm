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

int h2(int k, int M)
{
    return M - (k % M);
}

int getnExtBucketLinear(int v, int i, int M)
{
    return (v + i) % M;
}

int getNextBucketQuardratic(int v, int i, int M)
{
    return (v + i * i) % M;
}

int getNextBucketDouble(int v, int k, int i, int q, int M)
{
    return (v + i * h2(k, q)) % M;
}

int findElement(HashType *H, int M, int q, int key)
{
    int v = h(key, M);
    int i = 0;
    
    while (i < M)
    {
        //int b = getnExtBucketLinear(v, i, M);
        //int b = getNextBucketQuardratic(v, i, M);
        int b = getNextBucketDouble(v, key, i, q, M);

        if (H->A[b].key == key)
            return printf("%d %d\n", b, key);
            
        i++;
    }
    return printf("-1\n");
}

int removeElement(HashType *H, int M, int q, int key)
{
    int v = h(key, M);
    int i = 0;
    
    while (i < M)
    {
        //int b = getnExtBucketLinear(v, i, M);
        //int b = getNextBucketQuardratic(v, i, M);
        int b = getNextBucketDouble(v, key, i, q, M);
        if (isEmpty(H, b))
            return -1;
        else if (H->A[b].key == key)
        {
            H->A[b].key = H->A[b].proveCount = 0;
            return 1;
        }
        i++;
    }
}


void insertItem(HashType *H, int M, int q, int key)
{
    int v = h(key, M);
    int i = 0;
    int count = 0;
    
    while (i < M)
    {
        count++;
        //int b = getnExtBucketLinear(v, i, M);
        //int b = getNextBucketQuardratic(v, i, M);
        int b = getNextBucketDouble(v, key, i, q, M);
        //printf("b : %d\n", b);
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

void print(HashType *H, int M)
{
    for (int i = 0; i < M; i++)
        printf(" %d", H->A[i].key);
    printf("\n");
}

int main()
{
    int M, n, q;
    scanf("%d %d %d", &M, &n, &q);
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
                insertItem(&H, M, q, k);
                break;
            case 's':
                scanf("%d", &k);
                findElement(&H, M, q, k);
                break;
            case 'p':
                print(&H, M);
                break;
            case 'e':
                print(&H, M);
                return 0;
        }
    }
    return 0;
}
```
