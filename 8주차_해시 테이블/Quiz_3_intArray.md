```C
#include <stdlib.h>
#include <stdio.h>

int h(int k, int M)
{
    return k % M;
}

int h2(int k, int M)
{
    return M - k % M;
}

int searchItem(int *H, int M, int q, int key)
{
    int v = h(key, M);
    int tmp = v;
    
    if (H[tmp] == key)
        printf("%d %d\n", tmp, key);
    else
    {
        do
        {
            tmp = (tmp + h2(key, q)) % M;
            if (H[tmp] == key)
                return printf("%d %d\n", tmp, key);
        } while (tmp != v);
        
        return printf("-1\n");
    }
}

void insertItem(int *H, int M, int q, int key)
{
    int v = h(key, M);
    
    if (H[v])
        while(H[v])
        {
            v = (v + h2(key, q)) % M;
            printf("C");
        }
    H[v] = key;
    printf("%d\n", v);
}

void print(int *H, int M)
{
    for (int i = 0; i < M; i++)
        printf(" %d", H[i]);
    printf("\n");
}

int main()
{
    int M, n, q;
    scanf("%d %d %d", &M, &n, &q);
    int *H = calloc(M, sizeof(int));
    
    char fun; int k;
    while (1)
    {
        getchar();
        scanf("%c", &fun);
        switch (fun)
        {
            case 'i':
                scanf("%d", &k);
                insertItem(H, M, q, k);
                break;
            case 's':
                scanf("%d", &k);
                searchItem(H, M, q, k);
                break;
            case 'p':
                print(H, M);
                break;
            case 'e':
                print(H, M);
                return 0;
        }
    }
    return 0;
}
```
