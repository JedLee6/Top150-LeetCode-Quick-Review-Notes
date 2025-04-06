## 092_Graph_General_399. Evaluate Division

You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` represent the equation `Ai / Bi = values[i]`. Each `Ai` or `Bi` is a string that represents a single variable.

> Standard Markdown doesn't have a built-in syntax for creating subscript text directly. However, you can achieve this using HTML's `<sub>` tag within your Markdown content. Here's how you would type "i" as a subscript in "Ai": `A<sub>i</sub>`.

You are also given some `queries`, where `queries[j] = [Cj, Dj]` represents the `jth` query where you must find the answer for `Cj / Dj = ?`.

Return *the answers to all queries*. If a single answer cannot be determined, return `-1.0`.

**Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Note:** The variables that do not occur in the list of equations are undefined, so the answer cannot be determined for them.

 

**Example 1:**

```
Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation: 
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? 
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
note: x is undefined => -1.0
```

**Example 2:**

```
Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]
```

**Example 3:**

```
Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]
```

 

**Constraints:**

- `1 <= equations.length <= 20`
- `equations[i].length == 2`
- `1 <= Ai.length, Bi.length <= 5`
- `values.length == equations.length`
- `0.0 < values[i] <= 20.0`
- `1 <= queries.length <= 20`
- `queries[i].length == 2`
- `1 <= Cj.length, Dj.length <= 5`
- `Ai, Bi, Cj, Dj` consist of lower case English letters and digits.



## Problem Recap

You’re given a set of equations like `"a / b = 2.0"`, and you need to answer queries like `"a / c = ?"` based on the given equations. This naturally forms a **graph problem** where each variable is a node and an equation represents a weighted edge (with the weight being the division result). If there is no connection between two variables, the answer is `-1.0`.

---

## **Intuition and Main Idea**

Now, let's start with the intuition of this problem. Our idea is to treat the given equations as a graph where each variable is a node and each equation is a weighted edge. The idea is that to compute something like `a / c`(a over c), we need to find a path from node `a` to node `c` and multiply the weights along that path. If a valid path exists, the product of these weights gives us the answer; otherwise, we return -1.0. We can achieve this using graph traversal methods like DFS or BFS, and Union-Find approach. Alright, let's start with DFS to see how it works.

---

## **Solution 1: Graph DFS**

```java
class Solution {  
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {  
        // In the code, we start by creating a graph using a HashMap where each key is a variable and the value is another HashMap representing neighbors and their weights. Graph structure is like "{A -> {B: 2.0}, B -> {A: 0.5, C: 3.0}, ...}", which means "a / b = 2."
        Map<String, Map<String, Double>> graph = new HashMap<>();
        // Next, we loop over each equation to build the graph with proper connections.
        for (int i = 0; i < equations.size(); i++) {  
            // Inside the loop, first, we get the current equation as a list of strings, and assign its first element to 'dividend' and its second element to 'divisor'.
            List<String> equation = equations.get(i);
            String dividend = equation.get(0);
            String divisor = equation.get(1);
            // Then, we get the corresponding value from the values array.
            double value = values[i];
            // Now, we call "graph.putIfAbsent(dividend, new HashMap<>())" to ensure that the dividend key exists in the graph.
            graph.putIfAbsent(dividend, new HashMap<>());
            // Next we access the dividend's inner map and put the divisor with the given weight into it.
            graph.get(dividend).put(divisor, value);
            // Similarly, we add the reverse edge by ensuring the divisor key exists and then adding the dividend with the reciprocal weight.
            graph.putIfAbsent(divisor, new HashMap<>());
            graph.get(divisor).put(dividend, 1 / value);
        }
        // After building the graph, we initialize a results array to hold the answer for each query.
        double[] results = new double[queries.size()];
        // Then, We loop over each query, extract the start and end variables, and then process the query accordingly.
        for (int i = 0; i < queries.size(); i++) {  
            // First, we get the current query from the list.
            List<String> query = queries.get(i);
            // And we extract the start and end variable for the query
            String start = query.get(0);
            String end = query.get(1);
            // Now, we handle the query with some checks. If the start or end variable is missing in our graph, we assign -1.0 to the result for this query.
            if (!graph.containsKey(start) || !graph.containsKey(end)) {  
                results[i] = -1.0;
            // Otherwise, if the start and end are the same, we know the result must be 1.0.
            } else if (start.equals(end)) {
                results[i] = 1.0;
            // Finally, if the two nodes exist, but they are different, we need to perform the DFS search.
            } else {
                // Use DFS to find the product along the path from start to end.
                // Before starting DFS, we create a HashSet to keep track of nodes visited during the search and prevent cycles.            
                Set<String> visited = new HashSet<>();
                // Then we call our 'dfs' helper method and pass the 'start' node, the 'end' target node, an initial product of 1 (since start/start = 1), the newly created 'visited' set, and the 'graph'.
                results[i] = dfs(start, end, 1, visited, graph);
            }
        }
        // Finally, after processing all the queries, we return the results array containing answers for all queries.
        return results;
    }
    
    // Now, let's implement the 'dfs' helper method which performs the actual path search from the current variable to the target variable.
    private double dfs(String current, String target, double product, Set<String> visited, Map<String, Map<String, Double>> graph) {  
        // The very first step inside dfs is to mark the 'current' node as visited by adding it to the 'visited'to prevents infinite cycles in graphs.
        visited.add(current);
        // Then we retrieve all the adjacent variables (neighbors) from the graph.
        Map<String, Double> neighbors = graph.get(current);
        // Now, if the target variable is found among the neighbors, we immediately return the current product multiplied by the edge weight.
        if (neighbors.containsKey(target)) {  
            return product * neighbors.get(target);
        }
        // Otherwise, if the target is not a direct neighbor, we iterate through each neighbor to continue the DFS.
        for (Map.Entry<String, Double> entry : neighbors.entrySet()) { 
            // Inside the loop, we get the neighbor node's key at first.
            String next = entry.getKey();
            // Next, we check if we have already visited this node during this DFS path. If so, we skip this loop iteration and move to the next neighbor to avoid cycles.
            if (visited.contains(next)) continue;
            // Otherwise, we call dfs recursively on the neighbor and update the product by multiplying the current product with the edge weight.
            double result = dfs(next, target, product * entry.getValue(), visited, graph);
            // After the recursive call returns, we check if it found a valid path, which means the result isn't -1.0. If so, we return the result immediately.      
            if (result != -1.0) {  
                return result;
            }
        }
        // Finally, after iterating through all neighbors, if no valid path exists from the current variable to the target, we return -1.0.
        return -1.0;
    }
}
```

Okay, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's discuss the time and space complexity.

### **Complexity Analysis**

We'll use 'E' for the number of initial equations, 'V' for the number of unique variables (which become nodes in our graph), and 'Q' for the number of queries.

- **Time Complexity is consist of 2 factors:**  
  
  - **Graph Building:** We iterate through each of the 'E' equations once. Inside the loop, we perform map operations like `putIfAbsent`, `get` and `put`. HashMap operations take O(1) time on average. So, building the graph takes **O(E)** time.
  - **Query Processing (DFS):** For each query (there are 'Q' of them), we may perform a Depth-First Search. In the worst case, a DFS might visit every node ('V') and traverse every edge in its connected component. The number of edges in our graph is actually 2*E (because we add both forward and backward edges). So, a single DFS takes O(V + 2E) time, which is simply **O(V + E)**. Since we do this for 'Q' queries, the total time for query processing is **O(Q \* (V + E))**.
  - **Overall Time:** According to the two factors above, the total time complexity is dominated by the graph building and the query processing, which is **O(E + Q * (V + E))**.
  
- **Space Complexity is also made of 2 factors:**  
  
  - **Graph Storage:** We store the graph in our nested HashMap. In the worst case, we might have 'V' nodes. The total number of edges stored is 2E (or just simple E). The space needed for the adjacency list representation is proportional to the number of nodes plus the number of edges. So, the graph takes **O(V + E)** space.
  
      **DFS Auxiliary Space:** During a single DFS call, we use a `visited` set, which can store up to 'V' nodes in the worst case. Additionally, the recursion call stack depth can also go up to 'V' in the worst case (e.g., a long chain). So, the space needed for a single query's execution is **O(V)**.
  
      **Overall Space:** According to the two factors above, the dominant factor is the graph storage. Therefore, the total space complexity is **O(V + E)**.

---

## **Solution 2: Graph BFS**

```java
class Solution {
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) { 
        // Here, we define the main method that will take a list of equations, their corresponding values, and a list of queries.
        // Graph: Map each variable to its neighbors and the corresponding division factor.
        Map<String, Map<String, Double>> graph = new HashMap<>(); 
        // We initialize a HashMap called 'graph' that will store each variable as a key, and its value will be another map of connected variables (neighbors) along with their division factors.
        // Build the graph.
        for (int i = 0; i < equations.size(); i++) { 
            // We loop over each equation provided.
            List<String> equation = equations.get(i); 
            // We extract the current equation from the list.
            String dividend = equation.get(0); 
            // The first element in the equation list is treated as the dividend.
            String divisor = equation.get(1); 
            // The second element is the divisor.
            double value = values[i]; 
            // The corresponding division value for this equation is taken from the values array.
            graph.putIfAbsent(dividend, new HashMap<>()); 
            // We ensure that the dividend is a key in our graph by adding it if it isn’t already present.
            graph.get(dividend).put(divisor, value); 
            // We then connect the dividend to the divisor with the given value as the edge weight.
            graph.putIfAbsent(divisor, new HashMap<>()); 
            // Similarly, we ensure that the divisor is also present in the graph.
            graph.get(divisor).put(dividend, 1 / value); 
            // We add the reverse connection from the divisor back to the dividend using the reciprocal of the value.
        }
        // Prepare results for each query.
        double[] results = new double[queries.size()]; 
        // We initialize an array to store the results for each query.
        // Process each query using BFS.
        for (int i = 0; i < queries.size(); i++) { 
            // We iterate over each query.
            List<String> query = queries.get(i); 
            // Extract the current query.
            String start = query.get(0); 
            // The first element in the query is the starting variable.
            String end = query.get(1); 
            // The second element is the target variable we want to reach.
            
            if (!graph.containsKey(start) || !graph.containsKey(end)) { 
                // If either the start or end variable is not present in the graph,
                results[i] = -1.0; 
                // we set the result for this query to -1.0 as the division cannot be computed.
            } else if (start.equals(end)) { 
                // If the start and end variables are the same,
                results[i] = 1.0; 
                // the division of any number by itself is 1.0.
            } else {
                results[i] = bfs(start, end, graph); 
                // Otherwise, we perform a BFS search to find a path between the start and end variables, and store the resulting product.
            }
        }
        return results; 
        // Finally, we return the array of results for all queries.
    }
    
    // BFS helper method.
    private double bfs(String start, String end, Map<String, Map<String, Double>> graph) { 
        // This helper method performs a breadth-first search starting from the 'start' variable to find the 'end' variable in the graph.
        
        // Queue to hold the current variable and its cumulative product.
        Queue<Pair> queue = new LinkedList<>(); 
        // We initialize a queue that will help us explore the graph level by level.
        queue.offer(new Pair(start, 1.0)); 
        // We add the starting variable to the queue with an initial product of 1.0.
        // Set to track visited variables.
        Set<String> visited = new HashSet<>(); 
        // We initialize a set to keep track of visited nodes to avoid cycles.
        visited.add(start); 
        // We mark the starting variable as visited.
        
        while (!queue.isEmpty()) { 
            // We continue processing until there are no more nodes in the queue.
            Pair currentPair = queue.poll(); 
            // We remove the front element of the queue, which contains the current variable and the cumulative product so far.
            String current = currentPair.variable; 
            // 'current' holds the current variable.
            double product = currentPair.product; 
            // 'product' holds the cumulative multiplication result along the path taken.
            
            // If we find the target variable, return the product.
            if (current.equals(end)) { 
                // If the current variable is the target,
                return product; 
                // we return the cumulative product as the result of the query.
            }
            // Traverse the neighbors.
            for (Map.Entry<String, Double> entry : graph.get(current).entrySet()) { 
                // We iterate over all the neighbors of the current variable.
                if (!visited.contains(entry.getKey())) { 
                    // If a neighbor has not been visited yet,
                    visited.add(entry.getKey()); 
                    // we mark it as visited.
                    // Multiply the cumulative product by the edge weight.
                    queue.offer(new Pair(entry.getKey(), product * entry.getValue())); 
                    // We then add the neighbor to the queue with the updated product (the product so far multiplied by the weight of the edge connecting to this neighbor).
                }
            }
        }
        // If no valid path is found, return -1.
        return -1.0; 
        // When the BFS completes without finding the target, we return -1.0 to indicate that there is no connection between the variables.
    }
    
    // Helper class to store variable and the cumulative product.
    class Pair { 
        // This helper class is used to store a variable along with its current cumulative product during the BFS traversal.
        String variable; 
        // The 'variable' field stores the variable name.
        double product; 
        // The 'product' field stores the cumulative multiplication product up to this point.
        Pair(String variable, double product) { 
            // Constructor for initializing the Pair with a variable and its product.
            this.variable = variable; 
            // Assign the variable.
            this.product = product; 
            // Assign the product.
        }
    }
}
```

### **Analysis**

- **Time Complexity:**
    - **Graph Building:** This part is identical to the DFS solution. We iterate through 'E' equations, and map operations are O(1) on average. So, building the graph still takes **O(E)** time.
    - **Query Processing (BFS):** For each of the 'Q' queries, we perform a BFS. Standard BFS on a graph visits each node ('V') and each edge (E' = 2*E) at most once. Inside the BFS loop, queue operations (`offer`, `poll`) and set operations (`add`, `contains`) take roughly O(1) time on average for LinkedLists and HashSets. Therefore, a single BFS search takes **O(V + E')** or **O(V + E)** time. Since we do this for 'Q' queries, the total time for query processing is **O(Q \* (V + E))**.
    - **Overall Time:** Combining graph building and query processing, the total time complexity is **O(E + Q \* (V + E))**. Notice this is asymptotically the same as the DFS solution!
- **Space Complexity:**
    - **Graph Storage:** Again, same as DFS. Storing the adjacency list in the nested HashMap takes space proportional to the number of nodes plus edges: **O(V + E')** or **O(V + E)**.
    - **BFS Auxiliary Space:** During a single BFS run, we need space for:
        - The `visited` set: In the worst case, this can store up to 'V' nodes, taking **O(V)** space.
        - The `queue`: In the worst case (e.g., a graph where one node connects to many others), the queue might hold up to O(V) `Pair` objects simultaneously. So, the queue takes **O(V)** space.
        - The auxiliary space for BFS execution is therefore **O(V)**.
    - **Overall Space:** The graph storage (O(V + E)) is typically the dominant factor. So, the overall space complexity remains **O(V + E)**, which is also asymptotically the same as the DFS solution.

---

## **Solution 3: Union-Find (with Ratio Information)**

The main idea is to treat each variable as a node in a disjoint-set structure where the ratio between variables is maintained. By unioning two variables with a given ratio, we can later quickly determine if two variables are connected, and if so, compute the division result by comparing their ratios relative to their ultimate parents. This strategy helps us efficiently answer multiple queries about variable relationships. 

### **Java Code with Detailed Annotations**

```java
class Solution {
    // Map to store parent of each variable.
    private Map<String, String> parent; // Here, we declare a map to track each variable's parent in the disjoint set.
    // Map to store the ratio between the variable and its parent.
    private Map<String, Double> ratio; // And here, we declare a map to maintain the multiplication ratio from a variable to its parent.
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) { 
        // This public method calculates the results for the division queries given the equations and corresponding values.
        parent = new HashMap<>(); // We initialize the parent map.
        ratio = new HashMap<>();  // We initialize the ratio map.
        // Initialize each variable: set parent to itself and ratio to 1.0.
        for (int i = 0; i < equations.size(); i++) { 
            // We iterate through each equation in the list, and for each equation we process both variables.
            String var1 = equations.get(i).get(0); 
            // We extract the first variable from the equation, and
            String var2 = equations.get(i).get(1); 
            // we extract the second variable from the equation, and
            if (!parent.containsKey(var1)) { 
                // if the first variable is not in the parent map, then
                parent.put(var1, var1); 
                // we initialize it by setting its own parent to itself, and
                ratio.put(var1, 1.0); 
                // we set its ratio relative to itself as 1.0.
            }
            if (!parent.containsKey(var2)) { 
                // Similarly, if the second variable is not present in the parent map, then
                parent.put(var2, var2); 
                // we initialize it by setting its parent to itself, and
                ratio.put(var2, 1.0); 
                // we set its ratio relative to itself as 1.0.
            }
            // Union the two variables with the given ratio.
            union(var1, var2, values[i]); 
            // We then union these two variables using the provided ratio value, which represents var1/var2.
        }
        double[] results = new double[queries.size()]; 
        // We prepare an array to store the results of each query.
        for (int i = 0; i < queries.size(); i++) { 
            // Next, we iterate through each query in the queries list.
            List<String> query = queries.get(i); 
            // We extract the current query, and
            String var1 = query.get(0); 
            // we get the first variable of the query, and
            String var2 = query.get(1); 
            // we get the second variable of the query.
            // If one of the variables is not present, answer is -1.0.
            if (!parent.containsKey(var1) || !parent.containsKey(var2)) { 
                // If either variable does not exist in our parent map, then
                results[i] = -1.0; 
                // we assign -1.0 as the result for that query.
            } else {
                // Otherwise, we proceed to find the roots of both variables.
                Pair p1 = find(var1); 
                // We find the ultimate parent of the first variable along with its accumulated ratio, and
                Pair p2 = find(var2); 
                // we do the same for the second variable.
                // If roots differ, they are not connected.
                if (!p1.parent.equals(p2.parent)) { 
                    // If the root parents of var1 and var2 are different, then
                    results[i] = -1.0; 
                    // the variables are not connected, so we set the result to -1.0.
                } else {
                    // Otherwise, the ratio between var1 and var2 is computed.
                    results[i] = p1.ratio / p2.ratio; 
                    // We compute the result by dividing the ratio of var1 by the ratio of var2.
                }
            }
        }
        return results; 
        // Finally, we return the array of computed results for all the queries.
    }
    // Union two variables with the given value (ratio var1 / var2 = value).
    private void union(String var1, String var2, double value) { 
        // This method unites the sets containing var1 and var2, considering the ratio between them.
        Pair p1 = find(var1); 
        // We find the root and accumulated ratio for var1, and
        Pair p2 = find(var2); 
        // we find the root and accumulated ratio for var2.
        // Merge p1's parent to p2's parent.
        parent.put(p1.parent, p2.parent); 
        // We merge the set by setting the parent of p1's root to be p2's root, and
        // Adjust the ratio: p1.ratio * value gives the ratio for var1, so set parent ratio accordingly.
        ratio.put(p1.parent, p2.ratio * value / p1.ratio); 
        // We update the ratio for p1's root to reflect the ratio relationship between var1 and var2.
    }
    // Find operation with path compression.
    private Pair find(String var) { 
        // This method finds the root of the variable 'var' and compresses the path for efficiency.
        if (!parent.get(var).equals(var)) { 
            // If the current variable is not its own parent, then
            Pair p = find(parent.get(var)); 
            // we recursively call find to reach the root of the variable, and
            // Path compression: update the parent.
            parent.put(var, p.parent); 
            // we update the parent of var to its root, and
            // Update ratio to be relative to the root.
            ratio.put(var, ratio.get(var) * p.ratio); 
            // we update the ratio of var by multiplying its current ratio with the ratio of its parent.
        }
        return new Pair(parent.get(var), ratio.get(var)); 
        // Finally, we return a new Pair containing the root parent and the updated ratio for var.
    }
    // Helper class to store a pair of parent and ratio.
    class Pair { 
        // This helper class stores the information about a variable's ultimate parent and its ratio relative to that parent.
        String parent; // It has a field for the parent variable, and
        double ratio; // it has a field for the ratio from the variable to its parent.
        Pair(String parent, double ratio) { 
            // The constructor initializes the Pair with a given parent and ratio.
            this.parent = parent; 
            // It assigns the parent, and
            this.ratio = ratio; 
            // it assigns the ratio.
        }
    }
}
```

### **Analysis**

**Time Complexity:**

1. **Building the Structure (Union Loop):** We loop through 'E' equations. Inside, we might initialize variables (O(1) map access on average) and call `union`. The `union` calls `find` twice. Now, the magic of Union-Find with *path compression* (and often union by rank/size, though not explicitly used here, path compression dominates) makes the amortized time complexity of `find` (and thus `union`) *almost* constant. It's technically O(α(V)), where α is the super slow-growing **Inverse Ackermann function**. For any practical value of V you'll ever encounter, α(V) is effectively a very small constant (like 4 or 5). So, building the structure takes roughly **O(E \* α(V))** time.
2. **Query Processing:** We loop through 'Q' queries. Inside, we do a couple of O(1) map checks and then make two `find` calls. Each `find` is O(α(V)) amortized time. So, processing queries takes **O(Q \* α(V))** time.
3. **Overall Time:** Combining these, the total time complexity is **O((E + Q) \* α(V))**. This is considered *nearly linear* time, which is typically much faster than the O(E + Q * (V + E)) complexity we saw with the graph traversal methods, especially if V or E is large relative to Q.

**Space Complexity:**

1. **Data Structures:** We store the `parent` map and the `ratio` map. Both maps store an entry for each unique variable 'V'. This takes **O(V)** space.
2. **Recursion Stack:** The `find` method is recursive. In the worst-case theoretical scenario without path compression fully applied yet, the stack could go deep. However, path compression keeps the effective tree depth very shallow (related to α(V)). We can still bound the worst-case stack space as O(V), although practically it's much less.
3. **Overall Space:** The dominant space cost comes from the `parent` and `ratio` maps. Therefore, the overall space complexity is **O(V)**. This is generally better than the O(V + E) needed for storing an explicit graph, especially if there are many equations (edges).

---

## Terms explain

### evaluate

In mathematics and computer science, **"evaluate" generally means to calculate or determine the numerical value of an expression.**

When applied to the problem "Evaluate Division", it means you need to **compute the result** (find the numerical answer) for the division queries (`Cj / Dj`) based on the information provided in the `equations` and `values`.

### expression

- **Definition:** An expression is a combination of numbers, variables (symbols representing values, like x or y), operators (+, -, *, /), and grouping symbols (like parentheses) that represents a single value. Think of it as a mathematical "phrase".
- **Key Characteristic:** It does **not** contain an equals sign (`=`) asserting equality between two things. It simply stands for a value, which might be known or unknown depending on the variables.
- **Purpose:** To represent or compute a value.
- **Examples:**
    - `5` (A simple number is an expression)
    - `x` (A variable is an expression)
    - `3 + 7` (Represents the value 10)
    - `2 * width` (Represents a value that depends on the variable `width`)
    - `x^2 + 2x - 1` (A polynomial expression; its value depends on `x`)
    - `(a + b) / 2` (Represents the average of `a` and `b`)
    - `sin(theta) - 1` (Involves a function; its value depends on `theta`)

### equation

- **Definition:** An equation is a mathematical statement asserting that two **expressions** are equal. Think of it as a mathematical "sentence".  
- **Key Characteristic:** It **must** contain an equals sign (`=`). It has a left-hand side (LHS) expression and a right-hand side (RHS) expression.  
- **Purpose:** To state a relationship of equality between the LHS and RHS. Often, we need to "solve" an equation, which means finding the value(s) of the variable(s) that make the statement true.  
- **Examples:**
    - `x = 5` (States that the value represented by `x` is equal to the value `5`)
    - `3 + 7 = 10` (States that the expression `3 + 7` is equal to the expression `10`)
    - `2 * width = 12` (States that two times the `width` is equal to `12`. We could solve this to find `width = 6`)
    - `x^2 + 2x - 1 = 0` (A quadratic equation. We might solve it to find the values of `x` that make the expression `x^2 + 2x - 1` equal to `0`)
    - `Area = length * width` (A formula stating the relationship between area, length, and width)
    - `sin^2(theta) + cos^2(theta) = 1` (This is an identity – an equation that is true for all valid values of the variable `theta`)

### inequation / inequality

An **inequality** (or **inequation**) is a mathematical statement that asserts an order relationship between two expressions. Unlike an equation, which states that two expressions are equal (using the equals sign `=`), an inequality states that one expression is:  

- **Less than** (`<`) another
- **Greater than** (`>`) another
- **Less than or equal to** (`≤` or `<=`) another (at most)
- **Greater than or equal to** (`≥` or `>=`) another (at least)
- **Not equal to** (`≠` or `<>`) another

**Examples:**

1. **Numerical Inequalities (comparing numbers):**
    - `8 > 10` (8 is greater than 10 - False)
    - `6 ≤ 6` (6 is less than or equal to 6 - True)
    - `9 ≠ 3` (9 is not equal to 3 - True)
2. **Algebraic Inequalities (involving variables):**
    - `x + 3 > 7` (This states that the expression `x + 3` must have a value greater than 7. Solving it gives `x > 4`.)
    - `2y - 5 ≤ 1` (This states that `2y - 5` must have a value less than or equal to 1. Solving it gives `y ≤ 3`.)
    - `a^2 ≥ 0` (This states that the square of any real number `a` is greater than or equal to 0, which is always true for real numbers.)
    - `m ≠ 0` (This states that the variable `m` cannot be equal to zero.)
    - `-10 ≤ temperature < 25` (This is a *compound inequality*, stating that `temperature` is greater than or equal to -10 AND less than 25.)

> inequation /ɪnɪˈkweɪʒn/	inequality /ˌɪnɪˈkwɑːləti/
>
> absent /ˈæbsənt/ ADJ. If someone or something is absent from a place or situation where they should be or where they usually are, they are not there.
>
> reciprocal /rɪˈsɪprəkəl/ N. In mathematics, the **reciprocal** of a number is simply **1 divided by that number**. In other words, for any non-zero number `x`, the reciprocal of `x` is `1/x`.
>
> product N. In mathematics, the **product** is the result of multiplying two or more numbers or expressions together. It is the outcome you get after applying the multiplication operation. The product of 5 and 6 is **30**.
>
> contradiction n. If you describe an aspect of a situation as a contradiction, you mean that it is completely different from other aspects, and so makes the situation confused or difficult to understand.
>
> cumulative /ˈkjuːmjəleɪtɪv/ ADJ. If a series of events have a cumulative effect, each event makes the effect greater.