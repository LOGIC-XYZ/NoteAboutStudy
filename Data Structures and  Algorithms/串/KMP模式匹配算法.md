-- 减少朴素匹配里不必要的回溯，所以需要在查找字符串前，先对要查找的字符串做一个分析，以减少查找的难度并提升速度

-- 前后缀一个字符相等，k值是2，两个字符相等k值是3，n个字符相等，k值就是n+1，存储 k 值的就是 next 数组 -- 此时前缀与后缀相等的最大长度

那么

```c
/* 通过计算返回子串T的next数组。 */
void get_next(String T, int *next)
{
    int i,k;
    i=1;
    k=0;
    next[1]=0;
    while (i<T[0])  /* 此处T[0]表示串T的长度 */
    {
        if(k==0 || T[i] == T[k])
        {
            ++i;  
            ++k;  
            next[i] = k;
        }
        else
            k= next[k]; /* 若字符不相同，则k值回溯 */
    }
}

/* 返回子串T在主串S中第pos个字符之后的位置。若不存在，则函数返回值为0。 */
/*  T非空，1≤pos≤StrLength(S)。 */
int Index_KMP(String S, String T, int pos)
{
    int i = pos;        /* i用于主串S中当前位置下标值，若pos不为1，则从pos位置开始匹配 */
    int j = 1;          /* j用于子串T中当前位置下标值 */
    int next[255];      /* 定义一next数组 */
    get_next(T, next);  /* 对串T作分析，得到next数组 */
    while (i <= S[0] && j <= T[0]) /* 若i小于S的长度并且j小于T的长度时，循环继续 */
    {
        if (j==0 || S[i] == T[j])   /* 两字母相等则继续，与朴素算法增加了j=0判断 */
        {
            ++i;
            ++j;
        }
        else            /* 指针后退重新开始匹配 */
            j = next[j];/* j退回合适的位置，i值不变 */
    }
    if (j > T[0])
        return i-T[0];
    else
        return 0;
}
```

子串的首位与后续的某些字符相等，也可以省略，故改良为以下
```c
/* 求模式串T的next函数修正值并存入数组nextval */
void get_nextval(String T, int *nextval)
{
    int i,k;
    i=1;
    k=0;
    nextval[1]=0;
    while (i<T[0])  /* 此处T[0]表示串T的长度 */
    {
        if(k==0 || T[i]== T[k])     /* T[i]表示后缀的单个字符，T[k]表示前缀的单个字符 */
        {
            ++i;  
            ++k;  
            if (T[i]!=T[k])      /* 若当前字符与前缀字符不同 */
                nextval[i] = k; /* 则当前的j为nextval在i位置的值 */
            else
                nextval[i] = nextval[k];    /* 如果与前缀字符相同，则将前缀字符的 */
                                            /* nextval值赋值给nextval在i位置的值 */
        }
        else
            k= nextval[k];          /* 若字符不相同，则k值回溯 */
    }
}

int Index_KMP1(String S, String T, int pos)
{
    int i = pos;        /* i用于主串S中当前位置下标值，若pos不为1，则从pos位置开始匹配 */
    int j = 1;          /* j用于子串T中当前位置下标值 */
    int nextval[255];       /* 定义一next数组 */
    get_nextval(T, nextval);    /* 对串T作分析，得到next数组 */
    while (i <= S[0] && j <= T[0]) /* 若i小于S的长度并且j小于T的长度时，循环继续 */
    {
        if (j==0 || S[i] == T[j])   /* 两字母相等则继续，与朴素算法增加了j=0判断 */
        {
            ++i;
            ++j;
        }
        else            /* 指针后退重新开始匹配 */
            j = nextval[j];/* j退回合适的位置，i值不变 */
    }
    if (j > T[0])
        return i-T[0];
    else
        return 0;
}
```
