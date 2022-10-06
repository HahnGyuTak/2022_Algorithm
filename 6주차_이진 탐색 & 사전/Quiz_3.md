```C
#include <stdio.h>
#include <stdlib.h>

int BinarySearch(char *x, int start, int end)
{
    int k = 0;
    do
    {
        int mid = (start + end) / 2;
        if (x[k] == 'N')
            end = mid;
        else if (x[k] == 'Y')
            start = mid + 1;
        k++;
    } while (start < end);
    
    return start;
}

int main()
{
    char *yn;
    int start, end, n;
    scanf("%d %d %d",&start, &end, &n);
    yn = (char *)malloc(sizeof(char) * (n + 1));
    scanf("%s", yn);
    
    printf("%d", BinarySearch(yn, start, end));
    free(yn);
    return 0;
}

```
