# Experiment No. 4

## Title

**Minimum Cost Spanning Tree Using Kruskal's and Prim's Algorithms**

## Aim

To implement Kruskal's Algorithm and Prim's Algorithm to find the Minimum Cost Spanning Tree (MST) of a given connected, undirected, weighted graph and analyze their time and space complexities.

## Program

#include <stdio.h>

#define MAX 20
#define INF 99999

struct Edge
{
    int src, dest, weight;
};

int parent[MAX];

struct Edge edges[MAX * MAX];
int graph[MAX][MAX];

int find(int i)
{
    while (parent[i] != i)
        i = parent[i];

    return i;
}

void unionSet(int i, int j)
{
    int a = find(i);
    int b = find(j);

    parent[a] = b;
}

void kruskal(int V, int E)
{
    int i, j, cost = 0;
    struct Edge temp;

    for (i = 0; i < E - 1; i++)
    {
        for (j = 0; j < E - i - 1; j++)
        {
            if (edges[j].weight > edges[j + 1].weight)
            {
                temp = edges[j];
                edges[j] = edges[j + 1];
                edges[j + 1] = temp;
            }
        }
    }

    for (i = 0; i < V; i++)
        parent[i] = i;

    printf("\nMinimum Spanning Tree using Kruskal's Algorithm:\n");
    printf("Edge\tWeight\n");

    int count = 0;

    for (i = 0; i < E && count < V - 1; i++)
    {
        int u = edges[i].src;
        int v = edges[i].dest;

        if (find(u) != find(v))
        {
            printf("%d - %d\t%d\n",
                   u, v, edges[i].weight);

            cost += edges[i].weight;
            unionSet(u, v);
            count++;
        }
    }

    if (count == V - 1)
        printf("Minimum cost of MST = %d\n", cost);
    else
        printf("MST does not exist. Graph is disconnected.\n");
}

void prim(int V)
{
    int selected[MAX] = {0};
    int cost = 0;
    int count, i, j;
    int min, u = -1, v = -1;

    selected[0] = 1;

    printf("\nMinimum Spanning Tree using Prim's Algorithm:\n");
    printf("Edge\tWeight\n");

    for (count = 0; count < V - 1; count++)
    {
        min = INF;
        u = -1;
        v = -1;

        for (i = 0; i < V; i++)
        {
            if (selected[i])
            {
                for (j = 0; j < V; j++)
                {
                    if (!selected[j] &&
                        graph[i][j] < min)
                    {
                        min = graph[i][j];
                        u = i;
                        v = j;
                    }
                }
            }
        }

        if (u == -1)
        {
            printf("MST does not exist. Graph is disconnected.\n");
            return;
        }

        printf("%d - %d\t%d\n", u, v, min);

        cost += min;
        selected[v] = 1;
    }

    printf("Minimum cost of MST = %d\n", cost);
}

int main()
{
    int V, E, i, j;

    printf("Enter number of vertices: ");
    scanf("%d", &V);

    printf("Enter number of edges: ");
    scanf("%d", &E);

    if (V < 1 || V > MAX ||
        E < 0 || E > MAX * MAX ||
        E < V - 1)
    {
        printf("Invalid graph size or edge count.\n");
        return 1;
    }

    for (i = 0; i < V; i++)
    {
        for (j = 0; j < V; j++)
        {
            graph[i][j] = (i == j) ? 0 : INF;
        }
    }

    printf("Enter edges (source destination weight):\n");

    for (i = 0; i < E; i++)
    {
        scanf("%d %d %d",
              &edges[i].src,
              &edges[i].dest,
              &edges[i].weight);

        int u = edges[i].src;
        int v = edges[i].dest;
        int w = edges[i].weight;

        if (u < 0 || u >= V ||
            v < 0 || v >= V ||
            u == v || w < 0 || w >= INF)
        {
            printf("Invalid edge input.\n");
            return 1;
        }

        graph[u][v] = w;
        graph[v][u] = w;
    }

    kruskal(V, E);
    prim(V);

    return 0;
}

**Note:** The order of the edges can differ between the two algorithms, but the minimum total cost is the same.

## Algorithm

### 1. Kruskal's Algorithm

1. Start the program.
2. Read the number of vertices and edges.
3. Read the source, destination, and weight of every edge.
4. Sort all edges in ascending order of their weights.
5. Initialize each vertex as a separate set.
6. Select the edge with the minimum weight.
7. Check whether its endpoints belong to different sets.
8. If they belong to different sets, include the edge in the MST and combine the sets.
9. If the edge creates a cycle, discard it.
10. Repeat until the MST contains `V - 1` edges or all edges have been considered.
11. Display the MST edges and their total cost.
12. Stop the program.

### 2. Prim's Algorithm

1. Start the program.
2. Read the number of vertices and edges.
3. Read the weighted edges and construct the adjacency matrix.
4. Start with vertex 0 and mark it as selected.
5. Find the minimum-weight edge connecting a selected vertex to an unselected vertex.
6. Add the selected edge to the MST.
7. Mark the newly connected vertex as selected.
8. Repeat until all vertices are selected or no connecting edge exists.
9. Display the MST edges and their total cost.
10. Stop the program.

## Time Complexity

### Kruskal's Algorithm

| Operation                       | Time Complexity                              |
| ------------------------------- | -------------------------------------------- |
| Sorting edges using bubble sort | O(E²)                                        |
| Finding and combining sets      | O(E × V) upper bound for this implementation |
| Overall Time Complexity         | **O(E² + E × V)**                            |

The program uses bubble sort to arrange edges in ascending order. Therefore, the sorting operation takes O(E²) time in the worst case.

### Prim's Algorithm

| Operation                             | Time Complexity     |
| ------------------------------------- | ------------------- |
| Initializing the selected array       | O(V)                |
| Searching for the minimum-weight edge | O(V²) per selection |
| Selecting edges for the MST           | O(V³) overall       |
| Overall Time Complexity               | **O(V³)**           |

The implementation uses an adjacency matrix and searches for the minimum-weight connecting edge in each iteration. Since the search takes O(V²) time and is repeated V − 1 times, the overall time complexity is O(V³).

## Space Complexity

### Kruskal's Algorithm

The program stores the edges in an edge array and uses a parent array for disjoint-set operations.

* Edge array: O(E)
* Parent array: O(V)

Therefore:

**Space Complexity = O(E + V)**

### Prim's Algorithm

The program uses an adjacency matrix and a selected array.

* Adjacency matrix: O(V²)
* Selected array: O(V)

Therefore:

**Space Complexity = O(V²)**

## Applications

1. Designing minimum-cost computer networks.
2. Planning electrical power distribution networks.
3. Designing road and railway networks.
4. Connecting communication networks.
5. Planning water supply and pipeline systems.
6. Optimizing connections between multiple locations.

## Advantages

* Both algorithms find a Minimum Spanning Tree for a connected, undirected, weighted graph.
* Kruskal's Algorithm selects edges in ascending order of weight.
* Prim's Algorithm expands the MST by adding the minimum-weight connecting edge.
* Both algorithms help minimize the total cost of connecting all vertices.
* Their results can be compared to understand different greedy approaches.

## Limitations

* Kruskal's implementation uses bubble sort, which is inefficient for large edge sets.
* Prim's implementation uses an adjacency matrix and repeated minimum-edge searches.
* The graph must be undirected and connected to have a spanning tree.
* The program supports a maximum of 20 vertices.
* This implementation assumes nonnegative edge weights.

## Conclusion

The Minimum Cost Spanning Tree problem is solved using **Kruskal's Algorithm and Prim's Algorithm**, both based on the Greedy Method.

Kruskal's Algorithm sorts the edges and selects those that do not create cycles, while Prim's Algorithm repeatedly selects the minimum-weight edge connecting the current tree to an unselected vertex.

For the given sample graph, both algorithms produce a Minimum Spanning Tree with a total cost of **19**.

The experiment demonstrates how two different greedy techniques can solve the Minimum Spanning Tree problem.
output image:<img width="1000" height="620" alt="kruskalprim" src="https://github.com/user-attachments/assets/03801686-a1ef-49ef-a193-09d0dd758f54" />
