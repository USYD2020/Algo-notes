# Graph

## LeetCode 743. [网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)
### :smiley: To find the time taken for all nodes to receive the signal sourcing from K
Dijkstra算法[以及延伸](https://www.cnblogs.com/thousfeet/p/9229395.html):
*        1. 所有结点分为两部分：已确定最短路的结点集合P、未知最短路的结点集合Q。最开始，P中只有源点这一个结点。（可用一个visited数组来维护是否在P中）
         2. 在Q中选取一个离源点最近的结点u（dis[u]最小）加入集合P。然后考察u的所有出边，做松弛操作。
         3. 重复第二步，直到集合Q为空。最终dis数组的值就是源点到所有顶点的最短路。
         
[解法延伸阅读](https://leetcode-cn.com/problems/network-delay-time/solution/dan-yuan-zui-duan-lu-po-su-de-dijkstra-dui-you-hua/)

基础实现O(N^2 + E）如下：

*        1. 构建图，记录K到其他节点的距离 2. 遍历所有节点找出[没有更新过且距离最短]的节点 3. 更新这个节点的所有邻节点

```python
class Solution(object):
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        """
        dijskra算法 基本实现：稠密图常用 每次选择距离最短的目的地x加入目的地集合，更新K到x的邻接点的距离
        """
        #构建图,记录K到其他目的地的距离
        dist = [float('inf') for i in range(N+1)]
        visited = [False]*(N+1)
        dist[K] = 0
        hashMap = collections.defaultdict(list)
        for u,v,w in times:
            hashMap[u].append((v,w))
        # 每次循环找到一个距离最短且不在集合中的目的地加入集合
        while True:
            next_nearest = 0
            # 寻找不在集合中且距离最短的目的地
            for i in range(1,N+1):
                if not visited[i] and dist[i]<dist[next_nearest]:
                    next_nearest = i
            # 没找到
            if next_nearest == 0: break
            # 找到
            visited[next_nearest] = True
            # 更新K到它的邻接点的距离,没有邻接点则无须更新
            for neighbor , w in hashMap[next_nearest]:
                dist[neighbor] = min(dist[neighbor], w+dist[next_nearest])
        #dist[0] is irrelevant as 'inf'
        ans = max(dist[1:])
        return  ans if ans < float('inf') else -1
```
堆实现O(ElogE）如下：
*        1. 记录K到其他节点的距离 2. 用PQ找出距离最短的（距离d，节点）元组 3. 更新该节点到K的距离为d，更新该节点的所有邻节点
```python3
import collections
import heapq
class Solution(object):
    '''
    Dijkstra implementation using PQ, O(ElogE), as all edges are pushed into the PQ
    '''
    def networkDelayTime(self, times, N, K):
        graph = collections.defaultdict(list)
        for u, v, w in times:
            graph[u].append((v, w))
        # use distance to sort
        pq = [(0, K)]
        dist = {}
        while pq:
             # find the globally nearest node
            d, node = heapq.heappop(pq)
            print(d,node)
             # find the nearest unvisited node 
            if node in dist: continue
            # Remember to update dist
            dist[node] = d
            # update neighbors' distance to K
            for nei, d2 in graph[node]:
                if nei not in dist:
                    heapq.heappush(pq, (d+d2, nei))
        return max(dist.values()) if len(dist) == N else -1
```
