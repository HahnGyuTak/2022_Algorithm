```C
#include <stdio.h>
#include <stdlib.h>

int BinarySearch(int *x, int start, int end, int key)
{
    do
    {
        int mid = (start + end) / 2;
        if (x[mid] > key)
            end = mid - 1;
        else if (x[mid] < key)
            start = mid + 1;
        else
            return mid;
    } while (start <= end);
    
    return start;
}

int main()
{
    int *ar;
    int n, key;
    scanf("%d %d", &n, &key);
    ar = (int *)malloc(sizeof(int) * n);
    for (int i = 0; i < n; i++)
        scanf("%d", ar + i);
    if (ar[n - 1] < key) 
        printf("%d\n", n);
    else
        printf("%d", BinarySearch(ar, 0, n - 1, key));

    return 0;
}
```
