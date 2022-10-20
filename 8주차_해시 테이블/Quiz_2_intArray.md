```C
#include <stdlib.h>
#include <stdio.h>

int h(int k, int M)
{
    return k % M;
}

int searchItem(int *H, int M, int key)
{
    int v = h(key, M);
    int tmp = v;
    
    if (H[tmp] == key)
        printf("%d %d\n", tmp, key);
    else
    {
        do
        {
            tmp = ++tmp % M;
            if (H[tmp] == key)
                return printf("%d %d", tmp, key);
        } while (tmp != v);
        
        return printf("-1");
    }
}

void insertItem(int *H, int M, int key)
{
    int v = h(key, M);
    
    if (H[v])
        while(H[v])
        {
            v = ++v % M;
            printf("C");
        }
    H[v] = key;
    printf("%d\n", v);
}

int main()
{
    int M, n;
    scanf("%d %d", &M, &n);
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
                insertItem(H, M, k);
                break;
            case 's':
                scanf("%d", &k);
                searchItem(H, M, k);
                break;
            case 'e':
                return 0;
        }
    }
    return 0;
}
```
