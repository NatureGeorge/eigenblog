---
layout: default
date: 2020-05-16
author: Zefeng Zhu
---

# Topological Network Parameters

## Connected components

> A component is a maximal connected subgraph.

In undirected networks, two nodes are connected if there is a path of edges between them. All nodes that are pairwise connected form a connected component. The number of connected components in a network is an indicator of the global connectivity of a network. A low number of connected components relates to strong network connectivity because many nodes are connected and form few connected components of large node size.

## Degree distributions

In undirected networks, the node degree of a node n is the number of edges linked to n (ref. 40). A self-loop of a node is counted like two edges for the node degree63. A node with a high degree is referred to as hub. The node degree distribution gives the number of nodes with degree k for k= 0,1, ... In directed networks, the in-degree of a node n is the number of incoming edges and the out-degree of a node is the number of outgoing edges. As in undirected networks, there are in-degree and out-degree distributions. A network is called scale-free if its degree distribution approximates a power law k−α with the degree exponent α (ref. 40). The topological role of network hubs depends on the α value. For α> 3 the hubs are not relevant, for 3 >α> 2 the hubs are organized in a hierarchy, and for α= 2 a hub-and-spoke model emerges, in which the largest hub is in contact with a large fraction of all nodes. For most biological networks, it has been observed that 2 <α< 3. Barabási and Albert used this network property to distinguish between random (as defined by Erdo ́ ́s and Rényi64) and scale-free network topologies40,65. There are also continued discussions about the observed power law and scale-freeness66,67.

## Neighborhood-related parameters

The *neighborhood* of a node n is the set of its neighbors. The *connectivity* kn is the size of the neighbor-hood of n (ref. 68). The *average number of neighbors* is an indicator for the average connectivity of the nodes in the network. A normalized version of this parameter is the *network density*. The density is a value between 0 and 1. It measures how densely the network is populated with edges (self-loops and duplicated edges are ignored). A network without edges and with solely isolated nodes has a density of 0. In contrast, the density of a clique, which is a set of nodes that are connected to each other, is 1. Another related parameter is the *network centralization*68. Networks whose topologies resemble a star have centralization close to 1, whereas more uniformly connected networks are characterized by centralization close to 0. The *network heterogeneity* reflects the tendency of a network to contain hub nodes68.

## The neighborhood connectivity

The neighborhood connectivity of a node n is the average connectivity of all neighbors of n (ref. 69). The neighborhood connectivity distributiongives the average of the neighborhood connectivities of all nodes n with k neighbors for k= 0,1, ... In directed networks, a node has the following three types of neighborhood connectivity: only in, the average out-connectivity of all in-neighbors of n; only out, the average in-connectivity of all out-neighbors of n; and in and out, the average connectivity of all neighbors of n (edge direction is ignored). On the basis of these three definitions, there are three neighborhood connectivity distributions: only in, only out and in and out. If the neighborhood connectivity distribution is a decreasing function in k, edges between low connected and highly connected nodes prevail in the network69.

## Shortest paths

The length of a path is the number of edges forming it. The length of the shortest path, the distance, between two nodes n and m is denoted by L(n,m). The shortest path length distribution gives the number of node pairs (n,m) with L(n,m) =k for k= 1,2, ... The shortest path length distribution may indicate small-world properties of a network70. The eccentricity of a node n is the maximum noninfinite length of a shortest path between n and another node in the network. The network diameter is the maximum node eccentric-ity. If a network is disconnected, it is clear by definition that the network diameter is the maximum of all diameters of its connected components. In contrast, the network radius is the minimum of the nonzero eccentricities of the nodes in the network. The average shortest path length, also known as the characteristic path length, indicates the expected distance between two connected nodes.

## Clustering coefficients

In undirected networks, the clustering coefficient Cn of a node n is defined as Cn= 2en / (kn (kn–1)), where kn is the number of neighbors of n and en the number of edges between all neighbors of n (refs. 40,70). In directed networks, the definition is slightly different: Cn=en / (kn (kn–1)). In both cases, the clustering coefficient constitutes a ratio N/M, where N is the number of edges between the neighbors of n, and M the maximum number of edges that could possibly exist between the neighbors of n. The clustering coefficient of a node is always a number between 0 and 1. The network clustering coefficient is the average of the clustering coefficients of all nodes in the network. The average clustering coefficient distribution gives the average of the clustering coefficients for all nodes n with k neighbors for k= 2, ... In particular, the average clustering coefficient distribution was used to identify a modular organization of metabolic networks71.

## Shared neighbors

P(n,m) is the number of interaction partners shared between the nodes n and m, that is, nodes that are neighbors of both n and m (ref. 38). The shared neighbors distribution gives the number of node pairs (n,m) with P(n,m) =k for k= 1,2, ...

## Topological coefficients

The topological coefficient Tn of a node n with kn neighbors is computed as follows: Tn= avg(J(n,m)) / kn (ref. 72). Here J(n,m) is defined for all nodes m that share at least one neighbor with n. The value J(n,m) is the number of neighbors shared between the nodes n and m, plus 1 if there is an edge between n and m. The topological coefficient is a relative measure for the extent to which a node shares neighbors with other nodes. The chart of the topological coefficients indicates the tendency of the nodes in the network to have shared neighbors.

## Stress centrality

The stress centrality of a node n is the number of shortest paths passing through n (refs. 73,74). The stress centrality distribution gives the number of nodes with stress s for different values of s.

## Betweenness centrality

The betweenness centrality Cb(n) of a node n is defined as follows73: Cb(n) =Σs≠n≠t (σst(n) / σst). Here s and t are nodes in the network different from n, σst denotes the number of shortest paths from s to t, and σst(n) is the number of shortest paths from s to t that n lies on. The betweenness value for each node n is normalized by dividing the number of node pairs excluding n: (N – 1)(N – 2) / 2, where N is the total number of nodes in the connected component that n belongs to. Thus, the betweenness centrality of each node is a number between 0 and 1. The betweenness centrality of a node reflects the amount of control that this node exerts over the interactions of other nodes in the network75.

## Closeness centrality

The closeness centrality Cc(n) of a node n is the reciprocal of the average shortest path length76. The closeness centrality of each node is a number between 0 and 1. Closeness centrality is a measure of how quickly information spreads from a given node to other reachable nodes in the network76.

