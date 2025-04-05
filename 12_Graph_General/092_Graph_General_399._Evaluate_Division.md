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

Now, let's start with the intuition of this problem. Our idea is to model this problem as a graph where each variable is a node and each equation is a weighted edge. The idea is that to compute something like `a / c`(a over c), we need to find a path from node `a` to node `c` and multiply the weights along that path. We can achieve this using graph traversal methods like DFS or BFS, and Union-Find approach. Let's start with DFS.

---

## **Solution 1: Graph DFS**

### **Java Code with Detailed Annotations**

```java
class Solution {
    // Method to calculate equations using DFS approach.
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        // Graph: Map each variable to its neighbors and the corresponding division factor.
        Map<String, Map<String, Double>> graph = new HashMap<>();
        
        // Build the graph from the equations.
        for (int i = 0; i < equations.size(); i++) {
            List<String> equation = equations.get(i);
            String dividend = equation.get(0);
            String divisor = equation.get(1);
            double value = values[i];
            
            // For dividend -> divisor, assign weight value.
            graph.putIfAbsent(dividend, new HashMap<>());
            graph.get(dividend).put(divisor, value);
            
            // For divisor -> dividend, assign weight 1 / value.
            graph.putIfAbsent(divisor, new HashMap<>());
            graph.get(divisor).put(dividend, 1 / value);
        }
        
        // Prepare the result array for queries.
        double[] results = new double[queries.size()];
        
        // Process each query using DFS.
        for (int i = 0; i < queries.size(); i++) {
            List<String> query = queries.get(i);
            String start = query.get(0);
            String end = query.get(1);
            
            // If either variable is not in the graph, answer is -1.0.
            if (!graph.containsKey(start) || !graph.containsKey(end)) {
                results[i] = -1.0;
            } else if (start.equals(end)) {
                // Division of same variable is always 1.
                results[i] = 1.0;
            } else {
                // Use DFS to find the product along the path from start to end.
                Set<String> visited = new HashSet<>();
                results[i] = dfs(start, end, 1, visited, graph);
            }
        }
        return results;
    }
    
    // DFS helper method.
    private double dfs(String current, String target, double product, Set<String> visited,
                       Map<String, Map<String, Double>> graph) {
        // Mark current node as visited.
        visited.add(current);
        
        // Get neighbors of the current variable.
        Map<String, Double> neighbors = graph.get(current);
        
        // If the target is a direct neighbor, return the product immediately.
        if (neighbors.containsKey(target)) {
            return product * neighbors.get(target);
        }
        
        // Otherwise, traverse each neighbor recursively.
        for (Map.Entry<String, Double> entry : neighbors.entrySet()) {
            String next = entry.getKey();
            if (visited.contains(next)) continue;
            // Recursively search from neighbor to target, accumulating the product.
            double result = dfs(next, target, product * entry.getValue(), visited, graph);
            // If a valid result is found, propagate it upward.
            if (result != -1.0) {
                return result;
            }
        }
        // If no path found, return -1.
        return -1.0;
    }
}
```

### **Analysis**

- **Time Complexity:**  
  - **Building the graph:** O(E) where E is the number of equations.
  - **DFS for each query:** In the worst case, it may traverse all nodes: O(V + E) per query.
  
- **Space Complexity:**  
  - O(V + E) for the graph.
  - O(V) for the recursion stack and visited set in the worst case.

---

## **Solution 2: Graph BFS**

### **Java Code with Detailed Annotations**

```java
class Solution {
    // Method to calculate equations using BFS approach.
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        // Graph: Map each variable to its neighbors and the corresponding division factor.
        Map<String, Map<String, Double>> graph = new HashMap<>();
        
        // Build the graph.
        for (int i = 0; i < equations.size(); i++) {
            List<String> equation = equations.get(i);
            String dividend = equation.get(0);
            String divisor = equation.get(1);
            double value = values[i];
            
            graph.putIfAbsent(dividend, new HashMap<>());
            graph.get(dividend).put(divisor, value);
            
            graph.putIfAbsent(divisor, new HashMap<>());
            graph.get(divisor).put(dividend, 1 / value);
        }
        
        // Prepare results for each query.
        double[] results = new double[queries.size()];
        
        // Process each query using BFS.
        for (int i = 0; i < queries.size(); i++) {
            List<String> query = queries.get(i);
            String start = query.get(0);
            String end = query.get(1);
            
            if (!graph.containsKey(start) || !graph.containsKey(end)) {
                results[i] = -1.0;
            } else if (start.equals(end)) {
                results[i] = 1.0;
            } else {
                results[i] = bfs(start, end, graph);
            }
        }
        return results;
    }
    
    // BFS helper method.
    private double bfs(String start, String end, Map<String, Map<String, Double>> graph) {
        // Queue to hold the current variable and its cumulative product.
        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(start, 1.0));
        
        // Set to track visited variables.
        Set<String> visited = new HashSet<>();
        visited.add(start);
        
        while (!queue.isEmpty()) {
            Pair currentPair = queue.poll();
            String current = currentPair.variable;
            double product = currentPair.product;
            
            // If we find the target variable, return the product.
            if (current.equals(end)) {
                return product;
            }
            
            // Traverse the neighbors.
            for (Map.Entry<String, Double> entry : graph.get(current).entrySet()) {
                if (!visited.contains(entry.getKey())) {
                    visited.add(entry.getKey());
                    // Multiply the cumulative product by the edge weight.
                    queue.offer(new Pair(entry.getKey(), product * entry.getValue()));
                }
            }
        }
        // If no valid path is found, return -1.
        return -1.0;
    }
    
    // Helper class to store variable and the cumulative product.
    class Pair {
        String variable;
        double product;
        Pair(String variable, double product) {
            this.variable = variable;
            this.product = product;
        }
    }
}
```

### **Analysis**

- **Time Complexity:**  
  - **Graph Building:** O(E).
  - **BFS per query:** O(V + E) in the worst-case scenario per query.
  
- **Space Complexity:**  
  - O(V + E) for the graph.
  - O(V) for the queue and visited set.

---

## **Solution 3: Union-Find (with Ratio Information)**

This approach uses the Union-Find (Disjoint Set) data structure to group connected components and maintain the division ratios between each variable and its parent.

### **Java Code with Detailed Annotations**

```java
class Solution {
    // Map to store parent of each variable.
    private Map<String, String> parent;
    // Map to store the ratio between the variable and its parent.
    private Map<String, Double> ratio;
    
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        parent = new HashMap<>();
        ratio = new HashMap<>();
        
        // Initialize each variable: set parent to itself and ratio to 1.0.
        for (int i = 0; i < equations.size(); i++) {
            String var1 = equations.get(i).get(0);
            String var2 = equations.get(i).get(1);
            if (!parent.containsKey(var1)) {
                parent.put(var1, var1);
                ratio.put(var1, 1.0);
            }
            if (!parent.containsKey(var2)) {
                parent.put(var2, var2);
                ratio.put(var2, 1.0);
            }
            // Union the two variables with the given ratio.
            union(var1, var2, values[i]);
        }
        
        double[] results = new double[queries.size()];
        for (int i = 0; i < queries.size(); i++) {
            List<String> query = queries.get(i);
            String var1 = query.get(0);
            String var2 = query.get(1);
            // If one of the variables is not present, answer is -1.0.
            if (!parent.containsKey(var1) || !parent.containsKey(var2)) {
                results[i] = -1.0;
            } else {
                // Find roots for both variables.
                Pair p1 = find(var1);
                Pair p2 = find(var2);
                // If roots differ, they are not connected.
                if (!p1.parent.equals(p2.parent)) {
                    results[i] = -1.0;
                } else {
                    // Otherwise, the ratio between var1 and var2 is computed.
                    results[i] = p1.ratio / p2.ratio;
                }
            }
        }
        return results;
    }
    
    // Union two variables with the given value (ratio var1 / var2 = value).
    private void union(String var1, String var2, double value) {
        Pair p1 = find(var1);
        Pair p2 = find(var2);
        // Merge p1's parent to p2's parent.
        parent.put(p1.parent, p2.parent);
        // Adjust the ratio: p1.ratio * value gives the ratio for var1, so set parent ratio accordingly.
        ratio.put(p1.parent, p2.ratio * value / p1.ratio);
    }
    
    // Find operation with path compression.
    private Pair find(String var) {
        if (!parent.get(var).equals(var)) {
            Pair p = find(parent.get(var));
            // Path compression: update the parent.
            parent.put(var, p.parent);
            // Update ratio to be relative to the root.
            ratio.put(var, ratio.get(var) * p.ratio);
        }
        return new Pair(parent.get(var), ratio.get(var));
    }
    
    // Helper class to store a pair of parent and ratio.
    class Pair {
        String parent;
        double ratio;
        Pair(String parent, double ratio) {
            this.parent = parent;
            this.ratio = ratio;
        }
    }
}
```

### **Analysis**

- **Time Complexity:**  
  - **Initialization and Union operations:** Approximately O(E * α(V)) where α(V) is the inverse Ackermann function (almost constant).
  - **Query Processing:** Each find operation is nearly O(1) on average.
  
- **Space Complexity:**  
  - O(V) for both the `parent` and `ratio` maps.

---

## **Vlog Oral Script**

*Intro:*  
"Hey everyone, welcome back to my vlog where I break down LeetCode problems step-by-step. Today, we’re solving Problem 399: Evaluate Division. We need to find the result of division queries based on a set of given equations like `a / b = 2.0`."

*Intuition & Idea:*  
"My intuition was to model this problem as a graph where each variable is a node and each equation is a weighted edge. The idea is that to compute something like `a / c`, we need to find a path from node `a` to node `c` and multiply the weights along that path. We can achieve this using graph traversal methods like DFS or BFS, and there’s even a neat Union-Find approach to precompute relationships."

*Explaining the DFS Solution:*  
"In my first solution, I built an adjacency list to represent the graph. For every equation, I created two entries – one for the direct division and one for the inverse. Then, I used a DFS function to traverse from the start variable to the target, multiplying the values along the path. I carefully used a visited set to avoid cycles. As you see in the code, every line has a clear purpose: from initializing the graph, traversing each neighbor, and returning the cumulative product if a valid path exists."

*Time and Space Analysis:*  
"The DFS solution, in the worst case, might traverse all nodes and edges for each query, which gives us a time complexity of O(V+E) per query. The space complexity is driven by the graph and the recursion stack, leading to O(V+E) space usage."

*Explaining the BFS & Union-Find Solutions:*  
"I also explored a BFS approach which works similarly by using a queue to traverse level by level. In contrast, the Union-Find solution leverages disjoint sets to group variables and efficiently find conversion ratios. Both methods are efficient, with Union-Find being nearly constant for query operations after setup."

*Conclusion:*  
"To sum up, the key to solving Evaluate Division is recognizing the graph structure and choosing the right traversal or union method to compute the desired ratios. Each method has its trade-offs in terms of complexity, and understanding these helps you choose the best approach depending on the problem constraints."

"Thanks for watching, and I hope this detailed breakdown helps you in your coding journey. Don't forget to like and subscribe for more LeetCode problem walkthroughs!"



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
> contradiction n. If you describe an aspect of a situation as a contradiction, you mean that it is completely different from other aspects, and so makes the situation confused or difficult to understand.
>
> cumulative /ˈkjuːmjəleɪtɪv/ ADJ. If a series of events have a cumulative effect, each event makes the effect greater.