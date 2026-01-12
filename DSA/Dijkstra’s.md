1. Initialize distance[] = ∞ for all vertices
2. Set distance[source] = 0
3. Create a min-priority queue (min-heap) to pick the vertex with the smallest distance

4. While the priority queue is not empty:
       a. Extract the vertex u with the smallest distance value form pq
       b. For each neighbor v of u:
             - Calculate newDist = distance[u] + weight(u, v)
             - If newDist < distance[v]:
                    distance[v] = newDist
                    Push (v, distance[v]) into the priority queue
5. When the queue is empty, distance[] holds the shortest distances from source


**Q. Find Minimum Time to Reach Last Room I**


```java
class Solution {
    public int minTimeToReach(int[][] moveTime) {
        int n=moveTime.length, m=moveTime[0].length;

        int [][]distance = new int[n][m];
        for(int []d : distance){
            Arrays.fill(d, Integer.MAX_VALUE);
        }
        PriorityQueue<int[]>pq = new PriorityQueue<>(Comparator.comparing(a->a[0]));
        pq.offer(new int []{0,0,0});
        distance[0][0]=0;
        int[][] vis = {{1,0}, {-1,0},{0,1},{0,-1}};
        while(!pq.isEmpty()){
            int cur[] = pq.poll();
            int t=cur[0], r=cur[1],c=cur[2];
            if(t>distance[r][c]){
                continue;
            }
            if(r==n-1 && c==m-1){
                return t;
            }
            for(int []v:vis){
                int nr = r + v[0], nc = c + v[1];
                if (nr < 0 || nc < 0 || nr >= n || nc >= m) continue;
                int nt = Math.max(t, moveTime[nr][nc])+1;
                if (nt < distance[nr][nc]) {
                    distance[nr][nc] = nt;
                    pq.offer(new int[]{nt, nr, nc});
                }
            }
        }
        return -1;
    }
}
```
