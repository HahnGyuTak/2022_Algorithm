```C
#include <stdio.h>
#include <stdlib.h>

void printBGR(char *s);
void seperateBGR(char *s, int right, char color);
void swap(char *a, char *b);

int main()
{
    int n;
    char *ar;
    printf("N of node : "); scanf("%d", &n);
    ar = (char *)malloc(sizeof(char) * (n + 1));
    printf("List : "); scanf("%s", ar);
    
    seperateBGR(ar, n - 1, 'R');
    printBGR(ar);
}

void seperateBGR(char *s, int right, char color)
{
    int low = 0, high = right;
    
    do
    {
        while(s[low] != color) low++;
        while(s[high] == color) high--;
        
        if (low < high)
            swap(s + low++, s + high--);
    } while (low < high);
    
    if (color == 'R') seperateBGR(s, high, 'G');
}

void printBGR(char *s)
{
    printf("\n Answer -> %s\n", s);
}

void swap(char *a, char *b)
{
    char tmp = *a;
    *a = *b;
    *b = tmp;
}
```
