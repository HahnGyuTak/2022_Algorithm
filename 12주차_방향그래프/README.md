[RGB거리](https://www.acmicpc.net/problem/1149)

# Python
```Python
import sys
read = sys.stdin.readline
price = [0]*3 # Memoization of Cost
for _ in range(int(read())):
    R, G, B = map(int, read().split())
    R += min(price[1], price[2]) # minimum Cost of painting with other colors
    G += min(price[0], price[2])
    B += min(price[1], price[0])
    price[0], price[1], price[2] = R, G, B # Cost Update
print(min(price))

'''
3
26 40 83       26    40    83
49 60 57   ->  49+40 60+26 57+26
13 89 99       13+83 89+83 99+86 -> 최소값 96
'''
```

# C
```C
#include <stdio.h>
#include <stdlib.h>
#define min(a, b) (a > b ? b : a) //Func of Minimum

int main()
{
    int n, rgb[3] = {0}; // Memoization of Cost
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
    {
        int r, g, b;
        scanf("%d %d %d", &r, &g, &b);
        r += min(rgb[1], rgb[2]);
        g += min(rgb[0], rgb[2]);
        b += min(rgb[1], rgb[0]);
        rgb[0] = r;
        rgb[1] = g;
        rgb[2] = b;
    }
    printf("%d\n", min(rgb[0], min(rgb[1], rgb[2])));
    return 0;
}
```
