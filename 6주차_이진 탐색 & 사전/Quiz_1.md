```C
#include <stdio.h>
#include <stdlib.h>

int binary_search(int *x, int start, int end, int key)
{
    if (start >= end) 
    {
        if (x[start] == key)
            return start;
        else
            return x[start] > key ? start - 1 : start;
    }
    int mid = (start + end) / 2;
    
    if (x[mid] > key) 
        binary_search(x, start, mid - 1, key);
    else if (x[mid] < key)
        binary_search(x, mid + 1, end, key);
    else
    {
        while (x[mid + 1] == key) mid++;
        return mid;
    }
}

int main()
{
    int *ar;
    int n, key;
    scanf("%d %d", &n, &key);
    ar = (int *)malloc(sizeof(int) * n);
    for (int i = 0; i < n; i++)
        scanf("%d", ar + i);
    printf("%d", binary_search(ar, 0, n - 1, key));

    return 0;
}

```
