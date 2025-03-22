用于求解**带权无向连通图**的最小生成树的[[贪心策略]]算法。
其核心思想是从一个顶点开始，逐步扩展生成树，每次选择连接**已选顶点**集合和**未选顶点**集合的最小权边，直到所有顶点都被包含。

## 邻接矩阵 + 线性搜索 核心步骤
适用于稠密图（边数接近顶点数的完全图）
1. **初始化** 
    - 选择任意一个顶点作为起始点，将其标记为已加入生成树。
    - 记录该顶点与其他顶点的边的权值。
	    %% `lowcost` 数组存储每个未加入生成树的顶点到已加入顶点集的最小边权值。
			`adjvex` 数组记录每个顶点当前连接的已加入顶点（方便输出最小生成树的边）。%%
2. **选择最小权值的边**
    - 在所有连接已选顶点和未选顶点的边中，找到**权值最小**的边，并将该边的另一个顶点加入生成树。
3. **更新权值**
    - 更新新加入顶点与其他未选顶点之间的边的权值。
4. **重复上述步骤**，直到所有顶点都被加入生成树。
    
```cpp
/* Prim算法生成最小生成树  */
void MiniSpanTree_Prim(MGraph G)
{
    int min, i, j, k;
    int adjvex[MAXVEX];     /* 保存相关顶点下标 */
    int lowcost[MAXVEX];    /* 保存相关顶点间边的权值 */
    lowcost[0] = 0;/* 初始化第一个权值为0，即v0加入生成树 */
            /* lowcost的值为0，在这里就是此下标的顶点已经加入生成树 */
    adjvex[0] = 0;          /* 初始化第一个顶点下标为0 */
    for(i = 1; i < G.numVertexes; i++)  /* 循环除下标为0外的全部顶点 */
    {
        lowcost[i] = G.arc[0][i];   /* 将v0顶点与之有边的权值存入数组 */
        adjvex[i] = 0;                  /* 初始化都为v0的下标 */
    }
    for(i = 1; i < G.numVertexes; i++)
    {
        min = GRAPH_INFINITY;   /* 初始化最小权值为∞， */
                        /* 通常设置为不可能的大数字如32767、65535等 */
        j = 1;k = 0;
        while(j < G.numVertexes)    /* 循环全部顶点 */
        {
            if(lowcost[j]!=0 && lowcost[j] < min)/* 如果权值不为0且权值小于min */
            {  
                min = lowcost[j];   /* 则让当前权值成为最小值 */
                k = j;          /* 将当前最小值的下标存入k */
            }
            j++;
        }
        printf("(%d, %d)\n", adjvex[k], k);/* 打印当前顶点边中权值最小的边 */
        lowcost[k] = 0;/* 将当前顶点的权值设置为0,表示此顶点已经完成任务 */
        for(j = 1; j < G.numVertexes; j++)  /* 循环所有顶点 */
        {
            if(lowcost[j]!=0 && G.arc[k][j] < lowcost[j])
            {/* 如果下标为k顶点各边权值小于此前这些顶点未被加入生成树权值 */
                lowcost[j] = G.arc[k][j];/* 将较小的权值存入lowcost相应位置 */
                adjvex[j] = k;              /* 将下标为k的顶点存入adjvex */
            }
        }
    }
}
```

## 邻接表 + 优先队列 核心步骤
%%适用于稀疏图
- **初始化**
    - 选择任意顶点作为起点，并初始化相应的数组。
    - 使用邻接表来表示图，存储每个顶点到其邻接顶点的边的权值。
- **使用优先队列**
    - 用最小堆来存储每个未加入生成树的顶点及其最小权值边。
    - 每次从优先队列中取出最小权值的边，将顶点加入生成树并更新权值。
- **更新邻接表中的权值**
    - 当一个顶点加入生成树时，更新其邻接顶点的权值，并将这些邻接顶点重新加入优先队列。
%%
```cpp
#include <stdio.h>
#include <stdlib.h>

#define MAXVEX 100
#define GRAPH_INFINITY 32767

// 定义图的邻接表结构
typedef struct {
    int adjvex;     // 邻接点下标
    int weight;     // 边的权值
    struct EdgeNode *next; // 下一条边
} EdgeNode;

typedef struct {
    EdgeNode *edges[MAXVEX];  // 邻接表
    int numVertexes;  // 顶点数
    int numEdges;     // 边数
} MGraph;

// 优先队列节点
typedef struct {
    int vertex;   // 顶点
    int weight;   // 权值
} MinHeapNode;

// 优先队列（最小堆）结构
typedef struct {
    MinHeapNode heap[MAXVEX];
    int size;  // 堆的大小
} MinHeap;

// 初始化堆
void initMinHeap(MinHeap *heap) {
    heap->size = 0;
}

// 向堆中插入一个节点
void insertMinHeap(MinHeap *heap, int vertex, int weight) {
    heap->heap[heap->size].vertex = vertex;
    heap->heap[heap->size].weight = weight;
    int i = heap->size;
    heap->size++;

    // 上浮操作
    while (i > 0 && heap->heap[i].weight < heap->heap[(i - 1) / 2].weight) {
        MinHeapNode temp = heap->heap[i];
        heap->heap[i] = heap->heap[(i - 1) / 2];
        heap->heap[(i - 1) / 2] = temp;
        i = (i - 1) / 2;
    }
}

// 从堆中取出最小值节点
MinHeapNode extractMin(MinHeap *heap) {
    MinHeapNode minNode = heap->heap[0];
    heap->heap[0] = heap->heap[heap->size - 1];
    heap->size--;

    int i = 0;
    while (2 * i + 1 < heap->size) {
        int smallest = i;
        if (heap->heap[2 * i + 1].weight < heap->heap[smallest].weight) {
            smallest = 2 * i + 1;
        }
        if (2 * i + 2 < heap->size && heap->heap[2 * i + 2].weight < heap->heap[smallest].weight) {
            smallest = 2 * i + 2;
        }
        if (smallest == i) break;
        MinHeapNode temp = heap->heap[i];
        heap->heap[i] = heap->heap[smallest];
        heap->heap[smallest] = temp;
        i = smallest;
    }

    return minNode;
}

// Prim算法的实现（邻接表 + 优先队列）
void MiniSpanTree_Prim(MGraph G) {
    int i;
    int lowcost[MAXVEX];   // 存储每个顶点与生成树的最小权值
    int adjvex[MAXVEX];     // 存储每个顶点的前驱节点
    MinHeap heap;

    // 初始化数组和堆
    for (i = 0; i < G.numVertexes; i++) {
        lowcost[i] = GRAPH_INFINITY;
        adjvex[i] = -1;
    }
    lowcost[0] = 0;  // 选择第一个顶点作为起点
    initMinHeap(&heap);
    insertMinHeap(&heap, 0, 0);  // 向堆中插入起点

    // 存储每个顶点是否已经在生成树中
    int inTree[MAXVEX] = {0};

    while (heap.size > 0) {
        // 从堆中取出最小权值的顶点
        MinHeapNode node = extractMin(&heap);
        int u = node.vertex;
        inTree[u] = 1;  // 标记顶点u已经加入生成树

        // 遍历u的所有邻接点
        EdgeNode *e = G.edges[u];
        while (e != NULL) {
            int v = e->adjvex;
            if (!inTree[v] && e->weight < lowcost[v]) {
                lowcost[v] = e->weight;  // 更新v的最小权值
                adjvex[v] = u;  // 更新v的前驱节点
                insertMinHeap(&heap, v, e->weight);  // 将v加入堆中
            }
            e = e->next;
        }
    }

    // 输出最小生成树
    for (i = 1; i < G.numVertexes; i++) {
        printf("(%d, %d)\n", adjvex[i], i);  // 打印每条边
    }
}

```
