[패션왕 신혜빈](https://www.acmicpc.net/problem/9375)

# Python
```Python
tn = int(input())

for i in range(tn):
    n = int(input())
    cnt = {}
    
    for j in range(n):
        wear, category= input().split(" ")
        if category not in cnt:
            cnt[category] = 1
        else:
            cnt[category] += 1
    
    result = 1
    for num in cnt.values():
        result *= num + 1
    print(result - 1)
```

# C
```C
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

typedef struct Clothes
{
    char category[21]; // 카테고리 이름
    int cnt;
    struct Clothes *next;
}Clothes;

Clothes *getNode(char *s) // Clothe 노드 초기화
{
    Clothes *p = (Clothes*)malloc(sizeof(Clothes));
    strcpy(p->category, s);
    p->cnt = 1;
    p->next = NULL;

    return p;
}

int addList(Clothes **x, char *category, int N) // 해당 카테고리가 이미 있으면 cnt 증가, 없으면 노드 생성
{
    if (*x == NULL)
    {
        *x = getNode(s);
        return 1;
    }
    else
    {
        for (Clothes *p = *x; p != NULL; p = p->next)
            if (strcmp(p->category, category) == 0)
            {
                p->cnt++;
                return N;
            }
        Clothes *new = getNode(category);
        new->next = *x;
        *x = new;
        return N + 1;
    }
}

int calculate(Clothes *x) // a + b + c + ab + bc + ac + abc = (a+1)(b+1)(c+1) - 1
{
    int result = 1;
    for (Clothes *p = x; p != NULL; p = p->next)
        result *= p->cnt + 1;
    return result - 1;
}

int main()
{
    int TN;
    scanf("%d", &TN);
    for (int i = 0; i < TN; i++) // 테스트케이스
    {
        int n, cnt = 0;
        Clothes *x = NULL;
        scanf("%d", &n); 
        
        for (int j = 0; j < n; j++)
        {
            char n[21], c[21]; // name과 category
            scanf("%s" , n); scanf("%s", c); getchar();
            
            cnt = addList(&x, c, cnt);
        }
        printf("%d\n", calculate(x));
    }

    return 0;
}
```
