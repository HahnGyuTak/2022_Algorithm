<img width="584" alt="스크린샷 2022-12-10 오후 9 36 49" src="https://user-images.githubusercontent.com/50629765/206855462-732f85bf-110f-4c2b-a843-fbab49ecfd14.png">


```C
#include <stdio.h>
#include <stdlib.h>
#define FALSE 0
#define TRUE 1
#define max(a, b) (a > b ? a : b)

void DP(int cost[], int w[])
{
    int dp[6][11];
    for (int i = 0; i < 6; i++)
        for (int j = 0; j < 11; j++)
            dp[i][j] = 0;
    int max = 0;
    for (int i = 1; i < 6; i++)
    {
        for (int j = 1; j < 11; j++)
        {
            if (j < w[i-1]) dp[i][j] = dp[i-1][j];
            else
                dp[i][j] = max(dp[i-1][j], dp[i-1][j - w[i-1]] + cost[i-1]);
        }
    }
    printf("%d\n", dp[5][10]);
}

int getMin(int cw[])
{
    int min;
    for (int i = 0; i < 5; i++)
        if (cw[i] > 0)
        {
            min = i;
            break;
        }
    for (int i = 0; i < 5; i++)
        if (cw[i] > 0 && cw[min] > cw[i])
            min = i;
    return min;
}

void Greedy(int cost[], int w[])
{
    int cw[5], sum_w = 0, sum_c = 0;
    for (int i = 0; i < 5; i++)
        cw[i] = cost[i]/w[i];
    for (int i = 0; i < 5; i++)
    {
        int j = getMin(cw);
        if (sum_w + w[j] <= 10)
        {
            sum_w += w[j];
            sum_c += cost[j];
        }
        cw[j] = -1;
    }
    printf("%d\n", sum_c);
}

int main()
{
    int cost[5] = {32,15,27,5,4};
    int w[5] = {8,3,9,5,2};
    
    DP(cost, w);
    Greedy(cost, w);
    return 0;
}
```
