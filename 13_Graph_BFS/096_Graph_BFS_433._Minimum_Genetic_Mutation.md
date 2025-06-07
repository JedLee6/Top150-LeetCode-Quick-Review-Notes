## 096_Graph_BFS_433._Minimum_Genetic_Mutation

A gene string can be represented by an 8-character long string, with choices from 'A', 'C', 'G', and 'T'.

Suppose we need to investigate a mutation from a gene string startGene to a gene string endGene where one mutation is defined as one single character changed in the gene string.

For example, "AACCGGTT" --> "AACCGGTA" is one mutation.
There is also a gene bank bank that records all the valid gene mutations. A gene must be in bank to make it a valid gene string.

Given the two gene strings startGene and endGene and the gene bank bank, return the minimum number of mutations needed to mutate from startGene to endGene. If there is no such a mutation, return -1.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

 

Example 1:

Input: startGene = "AACCGGTT", endGene = "AACCGGTA", bank = ["AACCGGTA"]
Output: 1
Example 2:

Input: startGene = "AACCGGTT", endGene = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
Output: 2


Constraints:

0 <= bank.length <= 10
startGene.length == endGene.length == bank[i].length == 8
startGene, endGene, and bank[i] consist of only the characters ['A', 'C', 'G', 'T'].

> mutation  /mjuːˈteɪʃn/  N. a process in which the genetic material of a person, a plant or an animal changes in structure when it is passed on to children, etc., causing different physical characteristics to develop



## Initial Intuition

Looking at this problem, we immediately recognize it as a shortest path problem. Each gene string represents a node, and there's an edge between two genes if they differ by exactly one character. What we need to find is the shortest path from the startGene to the endGene, going through only valid mutations in the bank.

This is a classic shortest path problem, which makes us think of breadth-first search (BFS). BFS is perfect for this because it explores all possible mutations at the current "distance" before moving further away, ensuring we find the minimum number of mutations.

We can think of several solutions, from less optimal to more optimal:

## Solution Approaches (From Worst to Best)

1. **Brute Force DFS** - We could use depth-first search with backtracking to try all possible mutation paths, but this would be inefficient for finding the shortest path.

2. **Standard BFS** - A much better approach using breadth-first search to find the shortest path systematically.

3. **Bidirectional BFS** - An optimization of BFS that searches from both start and end simultaneously, potentially reducing the search space.

Let's go through each of these approaches in detail.

## Solution 1: Depth-First Search (DFS) - Conceptual (Less Optimal)

The main idea with a naive DFS would be to recursively try every possible mutation from the current gene.

- Start with `startGene`.
- For the current gene, generate all 1-character mutations.
- If a mutation is in the bank and hasn't been visited in the current path:
    - If it's the `endGene`, record the path length.
    - Otherwise, recursively call DFS from this new gene.
- We'd need to keep track of a global minimum path length found.

**Why it's less optimal:** DFS explores deeply. It might find a very long path to the `endGene` (or explore many dead ends) before finding the shortest one. To guarantee the shortest path, DFS would have to explore *all* possible paths, which can be combinatorially explosive. It also requires careful handling of visited states to avoid infinite loops, and managing path lengths can be more complex than BFS's level-order traversal. For shortest path problems in unweighted graphs, BFS is generally superior.

We won't code this one out fully because it's not the recommended approach, but it's useful to consider *why* BFS is preferred.

## Solution 2: Standard BFS

The main idea is to use breadth-first search to explore all possible valid mutations, level by level, until we reach the endGene or exhaust all possibilities.

We'll use a queue to manage the genes to visit and a set to keep track of visited genes to avoid redundant work and cycles. We start with `startGene` at 0 mutations. In each step (level of BFS), we take all genes reachable with the current number of mutations, generate all their valid one-character mutations. If a mutation is the `endGene`, we return the current mutation count. If it's a valid gene in the `bank` and not yet visited, we add it to the queue for the next level.

```java
class Solution {
    public int minMutation(String startGene, String endGene, String[] bank) {
        //  In the code, we start by creating a HashSet variable, as we need to convert the bank to a HashSet for O(1) lookups, which will make it efficient to check if a mutation is valid
        Set<String> bankSet = new HashSet<>(Arrays.asList(bank));
        // Next, we check if the target gene isn't in the bank or not, we can't reach it with valid mutations. So we immediately return -1 to indicate it's impossible
        if (!bankSet.contains(endGene)) {
            return -1;
        }
        // We then create a character arrays containing 4 characters in a gene to generate all possible mutations
        char[] validChars = {'A', 'C', 'G', 'T'};
        // After that, we create a queue to perform our BFS, starting with the startGene
        Queue<String> queue = new LinkedList<>();
        queue.offer(startGene);
        // And we need to track genes we've already visited to avoid cycles and redundant processing
        Set<String> visited = new HashSet<>();
        visited.add(startGene);
        // Following that, we declare a variable to track how many mutations we've made so far
        int mutations = 0;
        // Now we begin our BFS with a while loop
        while (!queue.isEmpty()) {
            // For each iteration, we process all genes at the current mutation level together
            // Firstly, we get the static size of the queue before modifying it
            int levelSize = queue.size();
            // Secondly, we process all genes at the current level
            for (int i = 0; i < levelSize; i++) {
                // Inside the inner for-loop, we get the current gene we're examining
                String currentGene = queue.poll();
                // Next, If we've reached our target, return the current mutation count
                if (currentGene.equals(endGene)) {
                    return mutations;
                }
                // Or, we'll try changing each position in the current gene by transforming the currentGene string to character arrays at first
                char[] geneArray = currentGene.toCharArray();
                // Then we use a for-loop to iterate all characters in currentGene
                for (int j = 0; j < geneArray.length; j++) {
                    // For each iteration, we save the original character so we can restore it later
                    char originalChar = geneArray[j];
                    // Now we create a enhanced for-loop to try each possible character at the current position
                    for (char c : validChars) {
                        // For each possible character, we'll skip it if it's the same character (not a mutation)
                        if (c == originalChar) {
                            continue;
                        }
                        // Otherwise, we apply the mutation and get the mutated gene string
                        geneArray[j] = c;
                        String mutatedGene = new String(geneArray);
                        // After that, we check if this mutation is valid and not visited
                        if (bankSet.contains(mutatedGene) && !visited.contains(mutatedGene)) {
                            // If so, we add it to our queue for further exploration
                            queue.offer(mutatedGene);
                            // And we mark it as visited to avoid processing it again
                            visited.add(mutatedGene);
                        }
                    }
                    // Out of the enhanced for-loop, we need to restore the original character for the next iteration
                    geneArray[j] = originalChar;
                }
            }
            // After processing all genes at the current level, increment mutation count
            mutations++;
        }
        // If we've checked all possibilities without finding the endGene, we return -1, as there's no valid mutation path
        return -1;
    }
}
```

Alright, that's all of the code. Let's submit our code and check if our solution passes all the test cases. Yes, our code got accepted. Now, Let's analyze the time and space complexity.

### Time and Space Complexity Analysis:

**Time Complexity**: O(N * L * 4)

- N is the number of genes in the bank
- L is the length of each gene (which is 8 in this problem)
- For each gene, we try to mutate each position with 3 other characters (4 total minus the original)
- In worst case, we might examine all genes in the bank

**Space Complexity**: O(N)

- We store the bank in a HashSet: O(N)
- The queue and visited set can contain at most N genes
- Overall space complexity is O(N)

## Solution 3: Bidirectional BFS

The key insight is that instead of just searching from start to end, we can search from both directions simultaneously. This can significantly reduce the search space in many cases.

```java
class Solution {
    public int minMutation(String startGene, String endGene, String[] bank) {
        // First, we convert the bank to a HashSet for efficient lookups
        // This allows us to quickly check if a mutation is valid
        Set<String> bankSet = new HashSet<>(Arrays.asList(bank));
        
        // If the target gene isn't in the bank, it's impossible to reach
        // So we immediately return -1
        if (!bankSet.contains(endGene)) {
            return -1;
        }
        
        // These are the only valid characters for gene mutations
        char[] validChars = {'A', 'C', 'G', 'T'};
        
        // We'll use two sets for our bidirectional search
        // One starting from the startGene
        Set<String> beginSet = new HashSet<>();
        beginSet.add(startGene);
        
        // And one starting from the endGene
        Set<String> endSet = new HashSet<>();
        endSet.add(endGene);
        
        // We'll track all visited genes to avoid cycles
        Set<String> visited = new HashSet<>();
        
        // Initialize our mutation counter
        int mutations = 0;
        
        // Continue while we have genes to explore from both directions
        while (!beginSet.isEmpty() && !endSet.isEmpty()) {
            // For efficiency, we always expand the smaller set
            // This helps minimize the branching factor
            if (beginSet.size() > endSet.size()) {
                // Swap the sets if beginSet is larger
                Set<String> temp = beginSet;
                beginSet = endSet;
                endSet = temp;
            }
            
            // Create a new set for the next level of mutations
            Set<String> nextLevel = new HashSet<>();
            
            // Process all genes in the current level
            for (String gene : beginSet) {
                // Try mutating each position in the current gene
                char[] geneArray = gene.toCharArray();
                for (int i = 0; i < geneArray.length; i++) {
                    // Save the original character to restore it later
                    char originalChar = geneArray[i];
                    
                    // Try each possible mutation at this position
                    for (char c : validChars) {
                        // Skip if it's the same character (not a mutation)
                        if (c == originalChar) {
                            continue;
                        }
                        
                        // Apply the mutation
                        geneArray[i] = c;
                        String mutatedGene = new String(geneArray);
                        
                        // If the other direction has already reached this gene,
                        // we've found the shortest path
                        if (endSet.contains(mutatedGene)) {
                            return mutations + 1;
                        }
                        
                        // Otherwise, if it's a valid mutation and not visited
                        if (!visited.contains(mutatedGene) && bankSet.contains(mutatedGene)) {
                            // Add it to the next level
                            nextLevel.add(mutatedGene);
                            // Mark it as visited
                            visited.add(mutatedGene);
                        }
                    }
                    
                    // Restore the original character for the next iteration
                    geneArray[i] = originalChar;
                }
            }
            
            // Move to the next level
            beginSet = nextLevel;
            
            // Increment our mutation counter
            mutations++;
        }
        
        // If we've explored all possibilities without finding a path,
        // return -1 to indicate it's impossible
        return -1;
    }
}
```

### Time and Space Complexity Analysis:

**Time Complexity**: O(N * L * 4)
- In the worst case, it's the same as standard BFS
- However, in practice, bidirectional BFS often explores significantly fewer nodes
- If the branching factor is b and the distance is d, standard BFS explores O(b^d) nodes
- Bidirectional BFS explores approximately O(2 * b^(d/2)), which is much smaller

**Space Complexity**: O(N)
- We store the bank in a HashSet: O(N)
- The two search sets and visited set can contain at most N genes
- Overall space complexity is O(N)

## Solution 4: Optimized BFS with Character Substitution

Instead of generating all mutations by trying each character at each position, we can directly check which genes in the bank are one mutation away.

```java
class Solution {
    public int minMutation(String startGene, String endGene, String[] bank) {
        // Convert the bank to a HashSet for O(1) lookups
        // This will make checking for valid mutations efficient
        Set<String> bankSet = new HashSet<>(Arrays.asList(bank));
        
        // If the target gene isn't in the bank, we can't reach it
        if (!bankSet.contains(endGene)) {
            return -1;
        }
        
        // Initialize our BFS queue with the starting gene
        Queue<String> queue = new LinkedList<>();
        queue.offer(startGene);
        
        // Track genes we've already processed to avoid cycles
        Set<String> visited = new HashSet<>();
        visited.add(startGene);
        
        // Initialize mutation counter
        int mutations = 0;
        
        // Begin our BFS traversal
        while (!queue.isEmpty()) {
            // Process all genes at the current mutation level
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                // Get the current gene
                String currentGene = queue.poll();
                
                // If we've reached our target, return the current mutation count
                if (currentGene.equals(endGene)) {
                    return mutations;
                }
                
                // Instead of trying all character combinations,
                // we'll check each gene in the bank to see if it's one mutation away
                for (String nextGene : bankSet) {
                    // Skip if we've already visited this gene
                    if (visited.contains(nextGene)) {
                        continue;
                    }
                    
                    // Check if this gene is exactly one mutation away
                    if (isOneMutation(currentGene, nextGene)) {
                        // Add it to our queue for processing
                        queue.offer(nextGene);
                        // Mark it as visited
                        visited.add(nextGene);
                    }
                }
            }
            
            // Increment mutation count after processing all genes at current level
            mutations++;
        }
        
        // If we've exhausted all possibilities without finding endGene,
        // return -1 to indicate it's impossible
        return -1;
    }
    
    // Helper method to check if two genes differ by exactly one character
    private boolean isOneMutation(String gene1, String gene2) {
        // Count the number of differing characters
        int differences = 0;
        
        // Compare each position in the two genes
        for (int i = 0; i < gene1.length(); i++) {
            // If characters at this position differ, increment our counter
            if (gene1.charAt(i) != gene2.charAt(i)) {
                differences++;
            }
            
            // If we've found more than one difference, return false immediately
            if (differences > 1) {
                return false;
            }
        }
        
        // Return true if there's exactly one difference
        return differences == 1;
    }
}
```

### Time and Space Complexity Analysis:

**Time Complexity**: O(N² * L)
- We potentially check every gene in the bank against every other gene
- For each pair, we compare up to L characters (gene length)
- This gives us O(N² * L) in the worst case
- However, the pruning from the visited set often makes this faster in practice than the theoretical worst case

**Space Complexity**: O(N)
- We store the bank in a HashSet: O(N)
- The queue and visited set can contain at most N genes
- Overall space complexity is O(N)

