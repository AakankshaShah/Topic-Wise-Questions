

1. Queue 
```
Queue<Integer> q = new LinkedList<>();
Has poll & add methods
```
1. Vector
```
   int[] indegree = new int[n];
```
3. Adjacency list
 ```
       vector<vector<int>> temp(n);  
      
       List<List<Integer>> adjList = new ArrayList<>();


        for (int i = 0; i < n; i++) {
            adjList.add(new ArrayList<>());
        }

       temp[i].push_back(j);
       adjList.get(i).add(j);


```
4. vector<int> vis(n, 0);
```
     boolean[] visited = new boolean[n]
```
5.   private static final long mod = (int) 1e9 + 7;
6.  
 ```
Set<Integer> set = new HashSet<>();
        for (int num : nums2) {
            set.add(num);
        }
      set.contains(nums1[i])
```
7.
```
 int[][] result = new int[m][n];
        for (int[] row : result) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
```
8.
```
   PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
     pq.offer(new int[] { 0, 0, 0 });

```
9.
```
// memset(t, -1, sizeof(t));
        Arrays.fill(t, -1);
```
