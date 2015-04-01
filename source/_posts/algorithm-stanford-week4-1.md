title: "Algorithm-Stanford-Week4"
date: 2015-02-01 17:41:04
categories: 
- Algorithm
tags:
- algorithm
- data structure
- coursera
---

BFS breadth-first search:
    core concepts : layer and queue

application : shortest path

DFS deepth-first search 
    core concepts: stack.
         
explore aggressively, only trackback when necessary. 
-- also computes a topological ordering of a directed xxx graph.
-- and strongly connected components of directed graphs.

application : Topological sort  
tips: after recursive, the statement N--  is the magic key.

SCC：

SCC analysis:
observation: 
--meta-node: SCC

--induce the original 

SCC meas strongly connected component. In an SCC, you can start from any node and get to any other nodes you want. 

How to find SCCs in a graph? lecturer gave a solution called Karasu... Algorithm.

the solution contains 3 steps.
first : reverse the Graph entirely.
second: run DFS loop on the reversed Graph in an arbitary order.
third: run DFS loop on the original Graph in the order of finishing times, in descending order.

what is the finnishing time? For example, when the node cannot call DFS recursively, the node is finished, which means that all nodes near it have been explored. Then we assign a "finishing time" to this node. (from 1 to N).

then we would get an order to make the second pass on the orignal Graph.

so why this solution gonna work?
proof:  the SCC could be seen as a "meta-node". because all nodes in a SCC are connected to eachother. so if there is an arc which connect a SCC node to other component's node, the other nodes in this SCC are also connected to the outer node.
so if we abstract the SCC as a single node, we convert the original Graph to a new acyclic graph. It is easy to prove that. If we still have a circle in the Graph, then these SCC(meta-node) are actually in one SCC. That is a contradiction.

now, we reverse the new meta-Graph and we run DFS loop on it. We could expect that, wherever we start the search loop. for example, we have a meta-graph like the one in below picture.

{% asset_img scc-example-image.png %}

where f1, f2, f3, f4 means the max finish time in the SCC(meta-node), respectively. We could expecte that wherever we start, the f4 would be smaller than f2 and f3, the f1 would be the biggest, f2 could be bigger or smaller than f3, depending on where we start the loop. Why? Because the "sink" meta-node(C4) would finsh the recursive first, then C2 or C3 and C1 is the last. That is determined by the recursive call order, which is FILO(call stack !!).  

Now, let's start our final step to get what we want, SCCs. We start DFS loop in original Graph, from the biggest "finish time" to 1.
As we can see, in the orignal Graph, the C1, could only be a sink node! So we start from here, and in one iteration, we would never go out of the SCC, because it is a "sink" meta-node. Similarly, we would then find out C2, C3 and C4.  QED.