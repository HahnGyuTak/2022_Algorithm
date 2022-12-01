[유기농 배추](https://www.acmicpc.net/problem/1012)

# Python
```Python
import sys
read = sys.stdin.readline
sys.setrecursionlimit(10**6)
direction = [(0,1), (0,-1), (1,0), (-1,0)]
def dfs(x, y):
    field[y][x] = 2
    for a, b in direction:
        if 0 <= y + b < n and 0 <= x + a < m and field[y+b][x+a] == 1:
            dfs(x + a, y + b)

tn = int(read())
for _ in range(tn):
    m, n, k = map(int, read().split())
    field = [[0] * m for _ in range(n)]
    for _ in range(k):
        x, y = map(int, read().split())
        field[y][x] = 1
    answer = 0
    for i in range(n):
        for j in range(m):
            if field[i][j] == 1:
                dfs(j, i)
                answer += 1
    print(answer)
```

# C
```C
#include <stdlib.h>
#include <stdio.h>

typedef struct map
{
   int **map;
   int m, n; // m : 가로, n : 세로
}Map;


int x_dir[4] = {0, 0, 1, -1}; //상하좌우
int y_dir[4] = {1, -1, 0, 0};

void dfs(int x, int y, Map *M)
{
   M->map[y][x] = 2;
   for (int p = 0; p < 4; p++)
   {
      int nx = x + x_dir[p];
      int ny = y + y_dir[p];
      if (nx >= 0 && nx < M->m && ny >= 0 && ny < M->n && M->map[ny][nx] == 1)
         dfs(nx, ny, M);
   }
}

int solved() 
{
   Map m;
   
   int k;
   scanf("%d %d %d", &m.m, &m.n, &k);
   m.map = (int**)malloc(m.n*sizeof(int*));
   for (int i = 0; i < m.n; i++)
      m.map[i] = calloc(m.m, sizeof(int));
   for (int i = 0; i < k; i++)
   {
      int x, y;
      scanf("%d %d", &x, &y);
      m.map[y][x] = 1;
   }
   int answer = 0;
   for (int i = 0; i < m.n; i++)
      for (int j = 0; j < m.m; j++)
         if (m.map[i][j] == 1)
         {
            dfs(j, i, &m);
            answer++;
         }
   return answer;
}

int main()
{
   int TN;
   scanf("%d", &TN);
   while (TN--)
      printf("%d\n", solved());
   return 0;
}
```
