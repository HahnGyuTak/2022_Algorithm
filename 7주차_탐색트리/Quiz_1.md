```C
#include <stdlib.h>
#include <stdio.h>

typedef struct TreeNode
{
    struct TreeNode *lChild, *rChild, *parent;
    int key;
}TreeNode;

TreeNode *getnode(int val, TreeNode *p)
{
    TreeNode *tmp = malloc(sizeof(TreeNode));
    tmp->lChild = tmp->rChild = NULL;
    tmp->parent = p;
    tmp->key = val;
}

int isExternal(TreeNode *v)
{
    return (!v->lChild && !v->rChild);
}

TreeNode* treeSearch(TreeNode *v, int k)
{
    if(isExternal(v))
        return v;
    if (k == v->key)
        return v;
    else if (k < v->key)
        return treeSearch(v->lChild, k);
    else
        return treeSearch(v->rChild, k);
}

/*void insertItem(TreeNode *root, int val)
{
    if (root->key == -1)
        root->key = val;
    else
    {
        TreeNode *tmp = root;
        
        while (!isExternal(tmp))
        {
            if (tmp_>rChild && tmp->key < val)
                tmp = tmp->rChild;
            else if (tmp->lChild && tmp->key >= val)
                tmp = tmp->lChild;
        }
        if (tmp->key < val)
            tmp->rChild = getnode(val, tmp);
        if (tmp->key > val)
            tmp->lChild = getnode(val, tmp)
    }
}*/

void expandExternal(TreeNode *x)
{
    TreeNode *ltmp = getnode(-1, x);
    TreeNode *rtmp = getnode(-1, x);
    
    x->lChild = ltmp;
    x->rChild = rtmp;
}

void insertItem(TreeNode *root, int val)
{
    TreeNode *w = treeSearch(root, val);
    
    if (!isExternal(w))
        return ;
    else
    {
        w->key = val;
        expandExternal(w);
    }
}

TreeNode *inOrderSucc(TreeNode *w)
{
    TreeNode *y = w->rChild;
    while (y->lChild != NULL)
        y = y->lChild;
    return y;
}

TreeNode *sibling(TreeNode *z)
{
    if (z->parent->lChild == z)
        return z->parent->rChild;
    else
        return z->parent->lChild;
}

void reduceExternal(TreeNode *z, TreeNode **root)
{
    TreeNode *w = z->parent;
    TreeNode *g = w->parent;
    TreeNode *zs = sibling(z);
    
    zs->parent = g;
    
    if (g == NULL)
        *root = zs;
    else
        if (w == g->lChild)
            g->lChild = zs;
        else
            g->rChild = zs;
    
    free(z);
    free(w);
}

int removeElement(TreeNode **root, int key)
{
    TreeNode *w = treeSearch(*root, key);
    if (isExternal(w))
        return -1;
        
    TreeNode *z = w->lChild;
    if (!isExternal(z))
        z = w->rChild;
        
    if (isExternal(z))
        reduceExternal(z, root);
        
    else
    {
        TreeNode *y = inOrderSucc(w);

        z = y->parent;
        w->key = z->key;
        reduceExternal(y, root);
    }
    
    return key;
}

void preOrder(TreeNode *root)
{
    if (isExternal(root))
        return ;

    printf(" %d", root->key);    
    
    preOrder(root->lChild);
    
    preOrder(root->rChild);
}

int main()
{
    TreeNode *root = getnode(-1, NULL);
    char fun;
    int k;
    while (1)
    {
        scanf("%c", &fun);
        switch (fun)
        {
            case 'i':
                scanf("%d", &k);
                insertItem(root, k);
                break;
            case 'd':
                scanf("%d", &k);
                int q = removeElement(&root, k);
                if (q == -1) printf("X\n");
                else printf("%d\n", q);
                break;
            case 's':
                scanf("%d", &k);
                int g = treeSearch(root, k)->key;
                if (g == -1)
                    printf("X\n");
                else
                    printf("%d\n", g);
                break;
            case 'p':
                preOrder(root); printf("\n");
                break;
            case 'q':
                return 0;
        }
        getchar();
    }
    return 0;
}
```
