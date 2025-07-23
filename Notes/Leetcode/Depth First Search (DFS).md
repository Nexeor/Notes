2025-01-08 17:49

Status: #Leaf

Tags: #ProgrammingConcepts #Trees #Graphs #Search #Algorithms

# Depth First Search (DFS)
Depth first search is a search algorithm applied to graphs, and by extension trees, to fully explore/traverse them. It starts at a selected node, typically the root or source, and explores as fast as possible down a single branch before backtracking. We prefer to explore child nodes rather than sibling nodes. 

![[Pasted image 20250108175419.png]]

The basic algorithm works in 5 steps:
1) Set the current node to the start node
2) Mark the current node as visited, then explore its neighbors
3) If the neighbor is unvisited, recursively iterate on that neighbor from step 2
4) When you reach a node who has no unvisited neighbors, backtrack up the recursion stack to a node that does have unvisited neighbors
5) Continue until all nodes are visited

DFS can be written with a **stack** to track the frontier nodes, or using **recursion** as described in the algorithm 

**Stack Implementation**
```python
def DFS(map, graph, start):
    visited = set()
    frontier = deque()
    
    visited.add(start)
    frontier.append(start)
    
    while len(frontier) != 0:
        cur = frontier.pop()
        
		for edge in graph[cur]:
			if edge not in visited:
				frontier.appendleft(edge)
        
        visited.add(cur)
    
    return score
```

**Recursion Implementation**
```python
def DFS(graph, vertex, visited):
    visited.add(vertex)
    for neighbor in graph[vertex]:
        if neighbor not in visited:
            DFS(graph, neighbor, ordering, visited)         
```
## References
