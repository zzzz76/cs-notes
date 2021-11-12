# C-Through: part-time optics in data centers

- 研究背景
- 论文解决的主要问题是什么？你认为这个问题是否存在，其重要性如何？
- 点评该论文的创新性、创造性及技术理论深度等。
- 论文优点：2-4点，用bulletted points显示列出；并给出理由及评论
- 论文缺点：2-4点，用bulletted points显示列出；并给出理由及评论
- 论文评论写作模式请参考：How to Read a Paper, by S.Keshav
- 单独写一段：Potential future research suggested by the article or in your mind



### background

In traditional data centers networks, many use the electrical packet-switch technology, and it worked with the old tree-structure of ethernet. Of course it bring the problem that all the tree-structure will have, that is the traffic will concerntrate  from the leave nodes to the root nodes, the nodes and the link of root maybe overload with the increase data center network bandwith.



### motivation

Currently there two solution to solve the problem above. the Fat-tree and Bcube, they use more links and more switches to build a full bisection bandwidth, provided all-to-all communication in the electrical packet-switch data centers. 

But with the complex structure, both of the solutions are hard to construct and to expand, so to explore a new way to be an alternative, the article provide a hybrid packet/circuit switches data center network.



### Main work

The main work of the article is to explore how optical circuit switching technology could benefit a data center network. That is to find a new hybrid architecture to combind the advantages of the electrical packet-switch and the advantages of the optical switch, while their shortcomings could be overcome.

The design requirements of the hybrid architecture has several necessary components that can not be ignore, the traffic demand estimation and optical circuit configuration in control plane, and the dynamic traffic de-multiplexing in the data plane.

To achieve that, the paper provide a specific design called C-Through which can well work compare to other choices. And to evaluate the performance, the paper set up a small testbed to emulate the C-through prototype.



### Evaluation

Finally, the paper get the evaluation on a set of performance about system and application as follow:

- TCP can exploit dynamic bandwidth quickly.

- traffic control on servers does not bring significant overhead.

* buffering does not unfairly increase delay of small flows.

* Bulk transfer (VM migration)

* Loosely synchronized all-to-all communication (MapReduce)

* Tightly synchronized all-to-all communication (MPI-FFT) 



### Future directions

* The scaling property of hybrid data center networks

* Making applications circuit aware

* Power efficient data centers with optical circuits



