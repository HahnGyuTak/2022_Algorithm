```C
#include <stdlib.h>
#include <stdio.h>

void mergerSort(int *ar, int *tmp, int left, int right)
{
    if (right <= left) return ;
    int mid = (left + right) / 2;
    
    mergerSort(ar, tmp, left, mid);
    mergerSort(ar, tmp, mid + 1, right);
    
    int idx_ar = left, idx_tmp = 0, j = 0;
    for ( ; idx_ar <= mid; idx_ar++)
        tmp[idx_tmp++] = ar[idx_ar];
    
    for ( ; idx_ar <= right && j < idx_tmp; )
        ar[left++] = ar[idx_ar] < tmp[j] ? ar[idx_ar++] : tmp[j++];
    
    while (j < idx_tmp)
        ar[left++] = tmp[j++];
   
}

int main()
{
    int array[9] = {5, 10, 3, 2, 6, 1, 7, 15, 9};
    int tmp[9];
    
    mergerSort(array, tmp, 0, 8);
    
    for (int i = 0; i < 9; i++)
        printf(" %d", array[i]);
    return 0;
}
```
