```C
#include <stdlib.h>
#include <stdio.h>
#include <time.h>
//#define swap(A, B, type) do{ type tmp = A; A = B; B = tmp;}while(0)

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int findPivot(int l, int r)
{
    
    int k = rand() % (r - l + 1) + l;
    //printf("pivot : %d\n", k);
    return k;
}

int inPlacePartition(int *ar, int left, int right, int pivot)
{
    //printf("p : %d\n", pivot);
    int k = ar[pivot];
    swap(ar + pivot, ar + right);
    int l = left - 1, r = right;
    
    do
    {
        do l++; while (ar[l] < k);
        do r--; while (ar[r] > k);
        if (l < r)
            swap(ar + l, ar + r);
    } while (l < r);
    swap(ar + l, ar + right);
    return l;
}

void inPlaceQuickSort(int *x, int start, int end)
{
    
    if (end <= start) return ;
    //printf("%d to %d : ", start, end);
    //for (int i = start; i <= end; i++)
    //    printf(" [%d]", x[i]);
    //printf("\n");
    int q = inPlacePartition(x, start, end, findPivot(start, end));
    //printf("pivot %d idx %d\n", x[q], q);
    inPlaceQuickSort(x, start, q - 1);
    inPlaceQuickSort(x, q + 1, end);
}

int main()
{
    int N, *ar;
    
    srand(time(NULL));
    scanf("%d", &N);
    ar = (int *)malloc(sizeof(int) * N);
    for (int i = 0; i < N; i++)
        scanf("%d", ar + i);
    inPlaceQuickSort(ar, 0, N - 1);
    for (int i = 0; i < N; i++)
        printf(" %d", ar[i]);
    return 0;
}
/*
8
73 65 48 31 29 20 8 3
*/

```
