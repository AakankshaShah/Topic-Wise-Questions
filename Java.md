

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
5.
