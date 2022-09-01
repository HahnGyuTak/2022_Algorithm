```C
#include <stdio.h>
#include <stdlib.h>

typedef struct node
{
    char data;
    struct node *next, *prev;
}node;

void init_node(node *x)
{
    x->next = NULL;
    x->prev = NULL;
}

void getnode(node **x)
{
    (*x) = (node*)malloc(sizeof(node));
    init_node(*x);
}

void add_list(node *H, int pos, char ele)
{
    node *newnode, *tmp = H;
    getnode(&newnode);
    newnode->data = ele;
    
    for (int i = 1; i < pos; i++)
        tmp = tmp->next;
    newnode->next = tmp->next;
    tmp->next = newnode;
    newnode->prev = tmp;
    if (newnode->next != NULL)
        newnode->next->prev = newnode;
}

void delete_list(node *H, int pos)
{
    node *tmp = H, *p;
    
    for (int i = 1; i < pos; i++)
        tmp = tmp->next;
    p = tmp->next;
    tmp->next = p->next;
    if (p->next != NULL)
        tmp->next->prev = tmp;
    free(p);
}

char get_list(node *H, int pos)
{
    node *p = H;
    
    for(int i = 1; i <= pos; i++)
        p = p->next;
    return (p->data);
}

void print_list(node *H)
{
    node *p = H->next;
    do
    {
        printf("%c", p->data);
        p = p->next;
    }while (p != NULL);
    printf("\n");
}

int main()
{
    int     n, pos, N = 0;
    char    f, ele;
    node    H;
    init_node(&H);
    
    scanf("%d", &n);
    for (int i = 0; i < n; i++)
    {
        getchar();
        scanf("%c", &f);
        switch(f)
        {
            case 'A':
                scanf("%d %c", &pos, &ele);
                if (pos > N + 1)
                    printf("invalid position\n");
                else
                {
                    add_list(&H, pos, ele);
                    N++;
                }
                break;
            case 'D':
                scanf("%d", &pos);
                if (pos > N)
                    printf("invalid position\n");
                else
                {
                    delete_list(&H, pos);
                    N--;
                }
                break;
            case 'G':
                scanf("%d", &pos);
                if (pos > N)
                    printf("invalid position\n");
                else
                    printf("%c\n", get_list(&H, pos));
                break;
            case 'P':
                print_list(&H);
                break;
        }
    }
    return 0;
}
```
