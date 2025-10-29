用于求解**带权无向连通图**的最小生成树的[[NoteAboutStudy/Data Structures and  Algorithms/贪心/贪心算法|贪心策略算法]]。
其核心思想是**按边权从小到大排序，依次选择不形成环的边加入生成树**，直到所有顶点都被连接。(对于稠密图较好)

```c
/* 查找连线顶点的尾部下标 */
int Find(int *parent, int f)
{
    while ( parent[f] > 0)
    {
        f = parent[f];
    }
    return f;
}
  
/* 生成最小生成树 */
void MiniSpanTree_Kruskal(MGraph G)
{
    int i, j, n, m;
    int k = 0;
    int parent[MAXVEX];/* 定义一数组用来判断边与边是否形成环路 */
    Edge edges[MAXEDGE];/* 定义边集数组,edge的结构为begin,end,weight,均为整型 */
    
    /* 用来构建边集数组并排序********************* */
    for ( i = 0; i < G.numVertexes-1; i++)
    {
        for (j = i + 1; j < G.numVertexes; j++)
        {
            if (G.arc[i][j]<GRAPH_INFINITY)
            {
                edges[k].begin = i;
                edges[k].end = j;
                edges[k].weight = G.arc[i][j];
                k++;
            }
        }
    }
    sort(edges, &G);
    /* **************************************** */

    for (i = 0; i < G.numVertexes; i++)
        parent[i] = 0;  /* 初始化数组值为0 */

    printf("打印最小生成树：\n");
    for (i = 0; i < G.numEdges; i++)    /* 循环每一条边 */
    {
        n = Find(parent,edges[i].begin);
        m = Find(parent,edges[i].end);
        if (n != m) /* 假如n与m不等，说明此边没有与现有的生成树形成环路 */
        {
            parent[n] = m;  /* 将此边的结尾顶点放入下标为起点的parent中。 */
                            /* 表示此顶点已经在生成树集合中 */
            printf("(%d, %d) %d\n", edges[i].begin, edges[i].end, edges[i].weight);
        }
    }
}
```