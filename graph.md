# Basic terminologies ->

1. **Graph**: A representation of connections between nodes or vertices. Types include undirected (no specific direction) and directed (with specific direction) graphs.

2. **Node/Vertex**: Represented by circles, these are the entities in a graph.

3. **Edge**: The connection between nodes. Can be undirected (bi-directional) or directed (one-way).

4. **Undirected Graph**: A graph where edges have no specific direction.

5. **Directed Graph**: A graph with edges having a specific direction.

6. **Cycle**: A path that starts and ends at the same node, leading to terms like undirected cyclic graph and directed cyclic graph.

7. **Path**: A sequence of vertices where each adjacent pair is connected by an edge.

8. **Degree**: In undirected graphs, it's the number of edges connected to a node. The total degree is twice the number of edges. In directed graphs, it's split into in-degree (incoming edges) and out-degree (outgoing edges).

9. **Edge Weight**: A numerical value assigned to edges, influencing the properties of the graph. If not specified, unit weight (1) is assumed.


# [BFS of graph](https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1)

## Approach ->
To traverse in the graph breath wise just create a queue and a visited array of size n (number of nodes in the adjacency list). Now push the first element of the graph i.e. 0 in the queue and mark it as visited. Now keep popping from the queue and check if the popped element has some nodes attached to it in the adjacency list and keep marking the visited nodes to avoid duplication. You will have your answer.

## Code ->
```cpp
class Solution {
public:
    // Function to return Breadth First Traversal of given graph.
    vector<int> bfsOfGraph(int V, vector<int> adj[]) {
        // Code here

        // Vector to store the result of Breadth-First Traversal
        vector<int> ans;

        // Queue for BFS traversal
        queue<int> q;

        // Array to mark visited nodes
        int visited[V] = {0};

        // Start BFS from node 0
        q.push(0);
        visited[0] = 1; // Mark the starting node as visited

        // Perform BFS traversal
        while (q.size()) {
            // Get the front element of the queue
            int curr = q.front();
            q.pop();

            // Add the current node to the result
            ans.push_back(curr);

            // Iterate through the adjacency list of the current node
            for (int i = 0; i < adj[curr].size(); i++) {
                // Check if the neighbor is not visited
                if (visited[adj[curr][i]] == 0) {
                    // Push the neighbor to the queue and mark it as visited
                    q.push(adj[curr][i]);
                    visited[adj[curr][i]] = 1;
                }
            }
        }

        // Return the result of BFS traversal
        return ans;
    }
};
```