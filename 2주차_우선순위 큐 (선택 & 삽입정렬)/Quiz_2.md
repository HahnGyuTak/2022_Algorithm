```C
#include <stdio.h>
#include <stdlib.h>

void injection_sort(int *arr, int n)
{
    for (int i = 1; i < n; i++)
    {
        int j, inject = arr[i];
        for (j = i - 1; j >= 0 && arr[j] > inject; j--)
            arr[j + 1] = arr[j];
        arr[j + 1] = inject;
    }
}

int main()
{
    int n, *arr;
    scanf("%d", &n);
    arr = (int *)malloc(sizeof(int) * n);
    for (int i = 0; i < n; i++)
        scanf("%d", arr + i);
    injection_sort(arr, n);
    for (int i = 0; i < n; i++)
        printf(" %d", arr[i]);
    return 0;
}
```
