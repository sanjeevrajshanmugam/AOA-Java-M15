
# EX 5A 0/1 Knapsack Problem - Branch&Bound 

## AIM:
To Write a Java program to solve 0/1 Knapsack problem using Branch and Bound Approach.
You are heading a college entrepreneurship cell that can invest in up to N student‑startups.

## Algorithm
1. Input & Initialization:
Read number of startups N and budget B.
Store each startup’s cost (c[]) and profit (p[]).
Initialize a global variable best = 0 to track the maximum profit 
2.  Sorting by Efficiency:
Compute the profit-to-cost ratio (p[i]/c[i]) for each startup.
Sort startups in descending order of this ratio to improve bound estimation.
3.  Upper Bound Calculation (bound()):
For a given node (index idx), current weight cw, and current value cv:
Add profits of remaining startups until the budget is filled.
If the next startup doesn’t fit completely, add a fractional profit proportional to the remaining budget.
This provides a maximum possible (fractional) profit estimate for pruning.
4.   Depth-First Search (DFS) with Branch and Bound (dfs()):
If the current path exceeds the budget or bound ≤ current best → prune the branch.
Otherwise, recursively explore:
Including the current startup (if it fits).
Excluding the current startup.
Update best whenever a higher profit is found.
5.   Output:
After exploring all feasible combinations, print the maximum profit best. 

## Program:
```
Developed by: SANJEEV RAJ S
Register Number:212223220096
import java.util.*;

public class StartupShowcaseOptimizer {

    
    static int N, B;
    static int[] c, p;          
    static int best = 0;        

    static double bound(int idx, int cw, int cv) {
        if (cw >= B) return cv;                
        double val = cv;
        int rem = B - cw;

        while (idx < N && c[idx] <= rem) {      
            rem -= c[idx];
            val += p[idx];
            idx++;
        }
        if (idx < N) val += p[idx] * (rem / (double) c[idx]); 
        return val;
    }

   
    static void dfs(int idx, int cw, int cv) {
        if (idx == N) {                 
            best = Math.max(best, cv);
            return;
        }
        if (bound(idx, cw, cv) <= best) return; 

        
        if (cw + c[idx] <= B)
            dfs(idx + 1, cw + c[idx], cv + p[idx]);

        
        dfs(idx + 1, cw, cv);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        B = sc.nextInt();
        int[] cost = new int[N];
        int[] prof = new int[N];
        for (int i = 0; i < N; i++) cost[i] = sc.nextInt();
        for (int i = 0; i < N; i++) prof[i] = sc.nextInt();
        sc.close();

       
        Integer[] idx = new Integer[N];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, Comparator.comparingDouble(i -> -(double) prof[i] / cost[i]));

        c = new int[N];
        p = new int[N];
        for (int i = 0; i < N; i++) {
            c[i] = cost[idx[i]];
            p[i] = prof[idx[i]];
        }

        dfs(0, 0, 0);
        System.out.println(best);
    }
}
 

```

## Output:

<img width="508" height="234" alt="image" src="https://github.com/user-attachments/assets/457902e3-ee50-492b-88c7-4bf952af8c60" />


## Result:
The program successfully solved 0/1 Knapsack problem using branch & bound and output is verified. 


# EX 5B Topological Sort - Khan's Algorithm

## AIM:
To write a Java program to for given constraints.

A software development team is preparing for a product release. The release consists of multiple tasks, each dependent on other tasks being completed first. You are to determine a valid order in which all tasks can be completed. If it's not possible due to cyclic dependencies, output that the release cannot be scheduled.




## Algorithm
1. Input & Graph Construction:
Read number of tasks n and dependencies m.
Build an adjacency list where b → a means task a depends on b.
Maintain an indegree array to count how many prerequisites each task has
2. Initialize the Queue:
Add all tasks with indegree = 0 (no dependencies) to a queue — these can be executed first.
3. Process Tasks (Topological Sort):
While the queue is not empty:
Remove a task from the queue and add it to the final order list.
For each dependent task, decrease its indegree by 1.
If any dependent task’s indegree becomes 0, add it to the queue.
4.  Cycle Detection:
After processing, if the total tasks in the order list ≠ n,
→ a cycle exists (some tasks depend on each other),
→ output: “Release cannot be scheduled.”
5. Output:
If no cycle is detected, print the tasks in the valid topological order,
representing a feasible schedule of task execution.  

## Program:
```
Program to implement Reverse a String
Developed by: SANJEEV RAJ S
Register Number:212223220096
import java.util.*;

public class prog {

    public static List<Integer> findTaskOrder(int n, int[][] dependencies) {
       
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }

        int[] indegree = new int[n];

       
        for (int[] dep : dependencies) {
            int a = dep[0];
            int b = dep[1];
            adj.get(b).add(a); 
            indegree[a]++;
        }

        
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0)
                queue.add(i);
        }

        List<Integer> order = new ArrayList<>();

      
        while (!queue.isEmpty()) {
            int task = queue.poll();
            order.add(task);

            for (int next : adj.get(task)) {
                indegree[next]--;
                if (indegree[next] == 0)
                    queue.add(next);
            }
        }

      
        if (order.size() != n)
            return null;

        return order;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(); 
        int m = sc.nextInt(); 
        int[][] dependencies = new int[m][2];
        for (int i = 0; i < m; i++) {
            dependencies[i][0] = sc.nextInt(); 
            dependencies[i][1] = sc.nextInt(); 
        }

        List<Integer> result = findTaskOrder(n, dependencies);

        if (result == null) {
            System.out.println("Release cannot be scheduled");
        } else {
            for (int task : result) {
                System.out.print(task + " ");
            }
        }
    }
}

```

## Output:

<img width="732" height="532" alt="image" src="https://github.com/user-attachments/assets/1fd0a1e3-31df-4c30-ae4c-30a7d4971aae" />


## Result:
The program successfully implemented and the expected output is verified.


# EX 5C Graph coloring

## AIM:
To write a Java program to for given constraints.
Problem Description:
In a hilly region, several radio towers are installed to provide communication services. However, due to signal interference, two adjacent towers (i.e., in communication range of each other) must not use the same frequency channel.

## Algorithm
1. Input & Graph Construction:
Read number of towers n, available channels m, and connections e.
Build an adjacency list representing connections between towers (edges).
2. Color Representation:
Create a color[] array where color[i] stores the assigned channel for tower i.
Initially, all towers are uncolored (0).
3. Recursive Backtracking (isColorable):
For each tower (node), try assigning channels (colors) from 1 to m.
Before assigning, check if the channel is safe using the isSafe() function.
4. Safety Check (isSafe):
Ensure no adjacent (connected) tower has the same channel.
If safe, assign the channel and recursively color the next tower.
If no valid channel exists, backtrack by resetting the color. 
5. Result:
If all towers can be assigned valid channels → print "YES".
Otherwise → print "NO" (conflict in channel assignment).  

## Program:
```
Program to implement Reverse a String
Developed by: SANJEEV RAJ S
Register Number:212223220096
import java.util.*;

public class RadioTowerChannelAssignment {

    
    public static boolean isSafe(List<List<Integer>> graph, int[] color, int node, int c) {
        for (int neighbour : graph.get(node)) {
            if (color[neighbour] == c) {
                return false; 
            }
        }
        return true;
    }

    
    public static boolean isColorable(List<List<Integer>> graph, int[] color, int node, int m, int n) {
        if (node == n) return true; 

       
        for (int c = 1; c <= m; c++) {
            if (isSafe(graph, color, node, c)) {
                color[node] = c; 
                if (isColorable(graph, color, node + 1, m, n))
                    return true;
                color[node] = 0; 
            }
        }
        return false; 
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(); 
        int m = sc.nextInt(); 
        int e = sc.nextInt();

        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++)
            graph.add(new ArrayList<>());

        for (int i = 0; i < e; i++) {
            int u = sc.nextInt();
            int v = sc.nextInt();
            graph.get(u).add(v);
            graph.get(v).add(u);
        }

        int[] color = new int[n];

        if (isColorable(graph, color, 0, m, n))
            System.out.println("YES");
        else
            System.out.println("NO");

        sc.close();
    }
}
 
```

## Output:
<img width="411" height="491" alt="image" src="https://github.com/user-attachments/assets/80a5bcb4-a0bb-44f6-a6e4-0c389e78284c" />



## Result:
The program successfully implemented and the expected output is verified.


# EX 5D Flower Planting.

## AIM:
To write a Java program to for given constraints.
You are given n gardens, labelled from 1 to n.

You also have a list called paths, where each element paths[i] = [xi, yi] represents a bidirectional road connectingthe  garden xi and garden yi.


## Algorithm
1. Input the Data
Read the number of gardens n and the number of paths m.
Read each of the m paths that connect two gardens and store them in an adjacency list (undirected graph). 
2. Build the Adjacency List
For each path (x, y), add y to the adjacency list of x and vice versa (convert to 0-based indexing).
3. Initialize Flower Assignment
Create an array ans[] of size n to store the flower type (1–4) for each garden.
Each garden will eventually have one flower type assigned.
4.  Assign Flowers Greedily
For each garden:
Create a boolean array used[5] to track which flower types are already used by its adjacent gardens.
For every neighbor, mark its flower type as used.
5. Output the Result
Print the flower type assigned to each garden in order.  

## Program:
```

Program to implement Reverse a String
Developed by: SANJEEV RAJ S
Register Number:212223220096
import java.util.*;

public class GardenFlowerPlanner {

    public static int[] assignFlowers(int n, int[][] paths) {
        @SuppressWarnings("unchecked")
        List<Integer>[] adj = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<>();
        }

       
        for (int[] path : paths) {
            int x = path[0] - 1;
            int y = path[1] - 1;
            adj[x].add(y);
            adj[y].add(x);
        }

        int[] ans = new int[n]; 

        for (int i = 0; i < n; i++) {
            boolean[] used = new boolean[5];

            
            for (int neighbor : adj[i]) {
                if (ans[neighbor] != 0) {
                    used[ans[neighbor]] = true;
                }
            }

            
            for (int color = 1; color <= 4; color++) {
                if (!used[color]) {
                    ans[i] = color;
                    break;
                }
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt(); 
        int m = sc.nextInt(); 

        int[][] paths = new int[m][2];
        for (int i = 0; i < m; i++) {
            paths[i][0] = sc.nextInt();
            paths[i][1] = sc.nextInt();
        }

        int[] result = assignFlowers(n, paths);

        for (int flower : result) {
            System.out.print(flower + " ");
        }
        System.out.println();
        
        sc.close();
    }
}
  
```

## Output:

<img width="514" height="468" alt="image" src="https://github.com/user-attachments/assets/6405940e-8a92-43bd-9956-5d3527a15d63" />


## Result:
The program successfully implemented and the expected output is verified.


# EX 5E Minimum Spanning Tree -Boruvka's Algorithm

## AIM:
To write a Java program to for given constraints.
Boruvka's Algorithm - Minimum Spanning Tree

## Algorithm
1. Input the Graph
Read the number of vertices V and edges E.
Store all edges with their source (src), destination (dest), and weight (weight) in a list.
2. Initialize Components
Create a parent[] array where each vertex is its own parent (disjoint set initialization).
Set the number of connected components to V (each vertex starts as a separate component).
3. Repeat Until Only One Component Remains
Create an array cheapest[] to store the cheapest edge for each component.
4. Find the Cheapest Edge for Each Component
For every edge (u, v):
Find the component (set) of u and v using the find() operation.
If they belong to different components:
Update the cheapest edge for both components if this edge has a smaller weight.
5.  Add the Cheapest Edges to the MST
For each vertex i, if cheapest[i] is not null:
Find the components of its source and destination.
6. Once all vertices are connected (components = 1), print all selected edges and the total MST weight.  

## Program:
```
Program to implement Reverse a String
Developed by: SANJEEV RAJ S
Register Number:212223220096
import java.util.*;

public class BoruvkaMST {
    static int[] parent;

  
    static int find(int i) {
        if (parent[i] != i)
            parent[i] = find(parent[i]);
        return parent[i];
    }

   
    static void union(int x, int y) {
        int xset = find(x);
        int yset = find(y);
        parent[xset] = yset;
    }

    static int boruvkaMST(int V, List<Edge> edges) {
        parent = new int[V];
        for (int i = 0; i < V; i++) parent[i] = i;

        int components = V;
        int mstWeight = 0;

        
        while (components > 1) {
            Edge[] cheapest = new Edge[V];

            
            for (Edge e : edges) {
                int set1 = find(e.src);
                int set2 = find(e.dest);

                if (set1 == set2) continue; 

                if (cheapest[set1] == null || cheapest[set1].weight > e.weight)
                    cheapest[set1] = e;

                if (cheapest[set2] == null || cheapest[set2].weight > e.weight)
                    cheapest[set2] = e;
            }

      
            for (int i = 0; i < V; i++) {
                Edge e = cheapest[i];
                if (e != null) {
                    int set1 = find(e.src);
                    int set2 = find(e.dest);

                    if (set1 == set2) continue;

                
                    System.out.println("Edge: " + e.src + "-" + e.dest + " Weight: " + e.weight);
                    mstWeight += e.weight;
                    union(set1, set2);
                    components--;
                }
            }
        }

        return mstWeight;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        int V = sc.nextInt();
        int E = sc.nextInt(); 

        List<Edge> edges = new ArrayList<>();
        for (int i = 0; i < E; i++) {
            edges.add(new Edge(sc.nextInt(), sc.nextInt(), sc.nextInt()));
        }

        int totalWeight = boruvkaMST(V, edges);
        System.out.println("Total Weight of MST: " + totalWeight);

        sc.close();
    }
}

class Edge {
    int src, dest, weight;
    Edge(int s, int d, int w) {
        src = s;
        dest = d;
        weight = w;
    }
}
  

```

## Output:
<img width="663" height="497" alt="image" src="https://github.com/user-attachments/assets/3ef60c93-c172-4f4c-b4a0-42a4b4791c3f" />



## Result:
The program successfully implemented and the expected output is verified.
