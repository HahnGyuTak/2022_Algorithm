```C
#include <stdio.h>
#include <stdlib.h>

void swap(int *x, int a, int b)
{
    int tmp = x[a];
    x[a] = x[b];
    x[b] = tmp;
}

void selection_sort(int *arr, int n)
{
    for(int i = n - 1; i >= 0; i--)
    {
        int max_idx = 0;
        for (int j = 1; j < i; j++)
            if (arr[max_idx] < arr[j])
                max_idx = j;
        if (arr[max_idx] > arr[i])
            swap(arr, max_idx, i);
    }
}

int main()
{
    int n, *arr;
    scanf("%d", &n);
    arr = (int *)malloc(sizeof(int) * n);
    for (int i = 0; i < n; i++)
        scanf("%d", arr + i);
    selection_sort(arr, n);
    for (int i = 0; i < n; i++)
        printf(" %d", arr[i]);
    return 0;
}
```
