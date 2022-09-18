[외계인의 기타연주 _ 스택활용 문제](https://www.acmicpc.net/problem/2841)

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct stack
{
    int s[500];
    int top;
}stack;

int result = 0;

void init_stack(stack *s) // 6개의 각 line별 스택 초기화
{
    for (int i = 0; i < 7; i++)
        s[i].top = -1; // 비어있다는 의미로 top = -1로 초기화
}

int isEmpty(stack *s) // 스택이 비어있는지 확인하는 함수
{
    return s->top == -1 ? 1 : 0;
}

void push(stack *s, int f) // 스택에 push 하고 result(횟수) 1 증가
{
    s->s[++(s->top)] = f;
    result++;
}

void pop_and_push(stack *x, int f) // 스택에서 프렛이 작거나 같아질때까지 pop, 그 후 push
{
    while (x->s[x->top] > f && !isEmpty(x))
    {
        x->top--;
        result++;
    }
    if (isEmpty(x) || x->s[x->top] < f)
        push(x, f);
}

int main()
{
    int n, p;
    stack line[7]; // 6개의 줄번호를 그대로 사용하기 위해 구조체 배열 7개
    
    scanf("%d %d", &n, &p);
    init_stack(line); // 구조체 초기화
    
    for (int i = 0; i < n; i++)
    {
        int l, f;
        scanf("%d %d", &l, &f);
        
        if (isEmpty(line + l)) // 비어있으면 -> 해당 line스택에 push
            push(line + l, f);
            
        else if (line[l].s[line[l].top] < f) //연주 예정 프렛이 스택의 top보다 크면 -> 해당 line 스택에 push
            push(line + l, f);
            
        else if (line[l].s[line[l].top] > f) // 프렛이 top보다 작으면 -> 작지 않을때 까지 스택에서 pop 후 해당 프렛 push
            pop_and_push(line + l, f);
    }
    printf("%d\n", result);
    return 0;
}
```
