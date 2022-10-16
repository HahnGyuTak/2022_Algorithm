```C
#include <stdlib.h>
#include <stdio.h>

void mergeSort(int *ar, int left, int right, int *buff)
{
    if (left >= right) return ;
    int mid = (left + right) / 2;
    
    mergeSort(ar, left, mid, buff);
    mergeSort(ar, mid + 1, right, buff);
    
    int ib = 0, i = left;
    while (i <= mid)
        buff[ib++] = ar[i++];
    
    int jb = 0, j = i, ix = left;
    while (jb < ib && j <= right)
        ar[ix++] = ar[j] < buff[jb] ? ar[j++] : buff[jb++];
        
    while (jb < ib)
        ar[ix++] = buff[jb++];
}

int main()
{
    int N; scanf("%d", &N);
    int *ar = (int *)malloc(sizeof(int) * N);
    int *buff = (int *)malloc(sizeof(int) * N);
    
    for (int i = 0; i < N; i++) scanf("%d", ar + i);
    
    mergeSort(ar, 0, N - 1, buff);
    
    for (int i = 0; i < N; i++) printf(" %d", ar[i]);

    return 0;
}
/*
8
73 65 48 31 29 20 8 3
*/
```
