# Quicluster
*Python based implementation of modified Kruskal's algorithm for finding clusters or forest of trees from graph with potential limitations on total tree arc weight and total node weights in clusters*

## Background
In Operations Research, while solving problems on large graphs long compute times may call for level of aggregation or creating subproblems in terms of clusters of nodes or set of minimum spanning trees. Several techniques are available to create such clusters however they may take significant time themselves or require specifying number of clusters. The motivation for this algorithm comes from limitations on the size of cluster, max weight of all nodes in cluster (if nodes have different weight) and max MST weight of arcs in cluster.

Simple example can be delivery routing where there is typically max capacity of vehicle and max drive time for drivers from DOT regulations. Such limitations may require several tours to be created for all deliveries. In such example, customers can be considered as nodes with their package size as node weight. All OD combinations can be considered as arcs with drive time as arc weight. The algorithm, with inputs for package weights and some MST estimate equivalent to max drive time, could then generate clusters of customers and corresponding minimum spanning tree. This minimum spanning tree could further be used as input to another routing heuristic to solve TSP.

## How it works
Create graph object by calling
```python
from graph import Graph

# v is list of vertices/nodes
# nodeweight is dictionary with node, weight as key, value
graph = Graph(v, nodeweights)
```

Add graph edges
```python
# for edge between (u,v) with weight w
graph.addEdge(u, v, w)
```

Create clusters with max total weight of nodes in cluster and max total weight of arcs in corresponding MST
```python
clusters = graph.getClusters(maxtreearcswt, maxtreenodeswt)
```

Output format
```python
# SAMPLE OUTPUT

{
	5: {
		'arcs':[(4, 5, 1), (5, 7, 1), (5, 8, 1)],
		'treearcweight': 3,
		'treenodeweight': 19,
	},
	3: {
		'arcs':[(2, 3, 2)],
		'treearcweight': 2,
		'treenodeweight': 14,
	},
	9: {
		'arcs':[(1, 9, 4)],
		'treearcweight': 4,
		'treenodeweight': 8,
	},
	6: {
		'arcs':[],
		'treearcweight': 0,
		'treenodeweight': 8,
	}
}

# Here 5, 3, 9, 6 are representative nodes of their corresponding clusters
# Cluster 6 is a single node cluster with no tree 
```

## Running time
Using disjoint set forest implementation with union by rank and path compression, the running time for this algortihm is no worse than *O(ElgV)*.