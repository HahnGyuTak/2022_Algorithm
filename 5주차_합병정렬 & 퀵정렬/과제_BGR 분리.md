```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void swap(char *a, char *b);
void printBGR(char *s);
int checkArray(char *s, int n);
void seperateBGR(char *s, int right, char color);

int main()
{
    int     n;
    char    *ar;
    
    printf("N of node : "); scanf("%d", &n);
    ar = (char *)malloc(sizeof(char) * (n + 1));
    printf("List      : "); scanf("%s", ar);
    
    if (!checkArray(ar, n))
        return 0;
    
    seperateBGR(ar, n - 1, 'R');
    
    printBGR(ar);
    
    free(ar);
    
    return 0;
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

int checkArray(char *s, int n)
{
    int checkbox[256] = { 0 };
    
    checkbox['B'] = checkbox['G'] = checkbox['R'] = 1;
    
    for (int i = 0; i < n; i++)
        if (!checkbox[s[i]] && printf("Wrong Input\n"))
            return 0;
    return 1;
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
