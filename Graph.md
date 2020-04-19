# Graph

## LeetCode 743. 网络延迟时间
### :smiley: To find the time taken for all nodes to receive the signal sourcing from K
https://leetcode-cn.com/problems/network-delay-time/solution/dan-yuan-zui-duan-lu-po-su-de-dijkstra-dui-you-hua/

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
