---
title: Implementing DAG in Python
author: Zefeng Zhu
date: 2020-05-17 15:30:00 +0800
categories: [Notes, Programming]
tags: [python, directed acyclic graph]
---

## Workflow Oriented

> Why are DAGs good for task dependencies? The 'G' in DAG is 'Graph'. A Graph is a collection of nodes and edges that connect the nodes. For our purposes, each node would be a task, and each edge would be a dependency. The 'D' in DAG stands for 'Directed'. This means that each edge has a direction associated with it. So we can interpret the edge (a,b) as meaning that b depends on a, whereas the edge (b,a) would mean a depends on b. The 'A' is 'Acyclic', meaning that there must not be any closed loops in the graph. This is important for dependencies, because if a loop were closed, then a task could ultimately depend on itself, and never be able to run. If your workflow can be described as a DAG, then it is impossible for your dependencies to cause a deadlock.

<script src="https://gist.github.com/NatureGeorge/f5ef97766a32bd0423a79d83e7400ad4.js"></script>

## Reference

1. <https://www.python.org/doc/essays/graphs/>
2. <https://ipython.org/ipython-doc/stable/parallel/dag_dependencies.html>
3. <https://github.com/ipython/ipython/blob/b78cec8e3c84dd2812615c2e6e52a9305b8d3ad6/examples/demo/dagdeps.py>
4. <https://github.com/ipython/ipyparallel/blob/ce5f0b8b4731d46842878faef87a82fe52843aef/examples/dagdeps.py>
5. <https://asynciojobs.readthedocs.io/en/latest/README.html>

