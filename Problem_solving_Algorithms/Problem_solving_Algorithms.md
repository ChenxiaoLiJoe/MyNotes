# Dijkstra算法

1.必要条件：只能用在权重为正的图中

2.学习链接

图文：https://www.freecodecamp.org/chinese/news/dijkstras-shortest-path-algorithm-visual-introduction/

视频：https://www.bilibili.com/video/BV1uT4y1p7Jy/?spm_id_from=333.337.search-card.all.click&vd_source=70bf859ea4e2d3993c7e7667eacfa667

3.例题：

给定一个n($1 \leq n \leq 10^5$)个点、m($1 \leq m \leq 2\times 10^5$)条有向边的带非负权图，请你计算从s出发，到每个点的距离。数据保证能从s出发到任意点。

```python
try:  # 引入优先队列模块
    import Queue as pq  # python version < 3.0
except ImportError:
    import queue as pq  # python3.*

N = int(1e5 + 5) # 最大节点数 + 缓冲区
M = int(2e5 + 5) # 最大边数 + 缓冲区
INF = 0x3F3F3F3F # 足够大的数字，模拟无穷大

class qxx:  # 前向星类（结构体）
    def __init__(self):
        self.nex = 0  # 指向下一条边的索引
        self.t = 0    # 边的目标节点
        self.v = 0    # 边的权重

e = [qxx() for i in range(M)]  # 边的存储数组
h = [0 for i in range(N)]      # 将每个节点的第一条边的索引初始化为0
cnt = 0                        # 边的计数器

dist = [INF for i in range(N)] # 把每个节点的第一条边都初始化模拟为无穷大，后续使用Dijktra算法进行更新
q = pq.PriorityQueue()  # 定义优先队列，默认第一元小根堆，按最短距离优先处理节点

def add_path(f, t, v):  # 在前向星中加边
    global cnt, e, h
    cnt += 1 # 新边编号
    e[cnt].nex = h[f]  # 链表头插法：新边的next指向原头边，相当于把新边指向链表头部
    e[cnt].t = t       # 设置目标节点
    e[cnt].v = v       # 设置边权重
    h[f] = cnt         # 更新头边指针

# Dijkstra算法
def nextedgeid(u):  # 生成器，用于遍历从节点u出发的所有边
    i = h[u] # h[u]是节点u的第一条出边编号
    while i:
        yield i # yield相当于多次返回的return，但效率更高
        i = e[i].nex # e[i].nex是下一条边编号

def dijkstra(s):
    dist[s] = 0 # 定义源节点到自身的距离
    q.put((0, s))  # 将起点加入优先队列，（距离，节点编号）
    while not q.empty():
        u = q.get()  # 取出当前距离最小的节点
        if dist[u[1]] < u[0]:  # 如果已有更短路径，跳过
            continue
        for i in nextedgeid(u[1]):  # 遍历所有邻边
            v = e[i].t # v是目标节点
            w = e[i].v # w是权重
            if dist[v] <= dist[u[1]] + w:  # 如果不需要更新，跳过
                continue
            dist[v] = dist[u[1]] + w  # 更新距离
            q.put((dist[v], v))       # 将更新后的节点加入队列

if __name__ == "__main__":
    n, m, s = map(int, input().split())  # 读入节点数、边数、起点
    for i in range(m):
        u, v, w = map(int, input().split())  # 读入每条边
        add_path(u, v, w)  # 添加边到图中

    dijkstra(s)  # 执行Dijkstra算法

    for i in range(1, n + 1):  # 输出结果
        print("{}".format(dist[i]), end=" ")
```



