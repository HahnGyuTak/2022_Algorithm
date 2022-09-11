```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <windows.h>

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
    int n, *ar, &br;
    scanf("%d", &n);
    ar = (int *)malloc(sizeof(int) * n);
    br = (int *)malloc(sizeof(int) * n);
    srand(time(NULL));
    for (int i = 0; i < n; i++)
    {
        int x = rand();
        ar[i] = br[i] = x;
    }
    
    LARGE_INTEGER ticksPerSec; 
    LARGE_INTEGER start, end, diff;
    
    QueryPerformanceFrequency(&ticksPerSec); 
    QueryPerformanceCounter(&start);
    selection_sort(ar, n);
    QueryPerformanceCounter(&end);
    diff.QuadPart = end.QuadPart – start.QuadPart;
    printf("time: %.12f sec\n\n", ((double)diff.QuadPart/(double)ticksPerSec.QuadPart))
    
    QueryPerformanceFrequency(&ticksPerSec); 
    QueryPerformanceCounter(&start);
    injection_sort(br, n);
    QueryPerformanceCounter(&end);
    diff.QuadPart = end.QuadPart – start.QuadPart;
    printf("time: %.12f sec\n\n", ((double)diff.QuadPart/(double)ticksPerSec.QuadPart))

    return 0;
}
```
