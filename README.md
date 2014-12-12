#Market Survey: Community Detection

## Good Data Sets
- Data which may not have Ground Truth
	- Stanford's InfoLab - https://snap.stanford.edu/data
- Data with Ground Truth
	- Stanford's InfoLab - https://snap.stanford.edu/data
- Synthetic Data Generation
	- Generated Graph by splitting into 4 groups [11]
	- Create graph which replicates properties of graphs--Kronecker Method [Lescovec et al. 2010] 
	- Need to replicate realworld properties: heavy tails for in and out-degree distribution, heavy tails for eigenvalues and eigenvectors, small diameters and desnificaiton and shrinking diameters over time.
                
## Methods of Community Detection
- __Node Based__
	- Given na individual node, identify the communities that the node belongs to
- __Component Based__
	- Given a component(community), discover overlapping sub-communities within it
- __Nework Based__
	- Given a community (nodes & edges), discover overlapping sub-communities within it

## Use Cases
-  Nodes belonging to a community are more than likely to have other properties in common [10].
-  Enables data triage through node relationships
-  May identify hierarchies (communities within communities)
-  Finding partitions in nodes to promote efficiencies in distributing computation 
-  Enables centrality measures within a community
-  Enables the identification of key edges used to link communities

## Evaluation Metrics
-  Overview of using ground truth [8]
-  These metrics were collectively suggested in the paper [14]
-  Good paper outlining potential issues with validating community detection, 2013 [22]
-  Classes of metrics: Internal Connectivity, Combo of internal and external connectivity
-  Goodness scores (as second order testing bc increases in community scores increases goodness scores): Separability, Density, Cohesiveness, Cluster Coefficient
-  For overlapping communities, we will want to use only internal and external metric values.  Combination metrics and modularity scores will result in misleading values that should be inconsistent.



| Metric Name | Connectivity Type | Ground Truth Required? | Details |
| :---------- | :---------------- | :----------------------| :------- |
| Edge/Link Prediction | Comparative Scoring Funciton | No | Quality of division is based on ability to predict an edge after removing it |
| Modularity [11] | Comparative Scoring Function |No | Tests a given division of a network against the random division. Scale below which modularity cannot identify communities bc random graphs have high-modularity subsets. Does not work well under nodeswap perturbation
| F1 Score | Comparative Scoring Function | Yes | Determination of precision and recall.  Compares membership output of an algorithm to ground truth and returns the ratio of true and false assignments |
| Omega Index | Comparative Scoring Function | Yes |  Compares non-disjoing clustering results.  The omega index considers the number of clusters in which a pair of objects appears together.|
| Normalized Mutual Information [10] | Comparative Scoring Function | Yes | Used to quantify matching of "true" graphs particularly in synthetic graphs, it quantifies teh degree of overlap between graphs.|


##Metrics

| Metric Name | Connectivity Type | Details |
| :---------- | :---------------- | :----------------------| :------- |
| FOMD | Internal | Fraction over Median Degree determines the number of nodes that have an internal degree greater than the median degree of all nodes in a group |
| Internal Density | Internal | Density is defined by the number of edges (m<sub>s</sub>) in subset S divided by the total number of possible edges between all nodes (n<sub>s</sub>(n<sub>s</sub>-1)/2).  The "2" is there to cancle out duplicated edges.  Internal Density = m<sub>s</sub>/(n<sub>s</sub>(n<sub>s</sub>-1)/2)| 
| Edges Inside | Internal | Somewhat useless by itself since it isn't related to any other attributes of subset S.  The total number of edges (m<sub>s</sub>) present in subset S.  Edges Inside = m<sub>s</sub> |
| Average degree | Internal | The average interal degree across all nodes (n<sub>s</sub>) in subset S.  Average Degree = 2m<sub>s</sub>/n<sub>s</sub>
| Fraction over median degree (FOMD) | Internal | Determines the number of nodes that have an internal degree greater than the median degree of nodes in Subset S. |
| Triangle Participation Ratio (TPR) | Internal | The best mesure for density, cohesiveness, and clustering within the goodness scales.  Robust under random and expand perturbations.  A measure of cohesion.  The fraction of nodes in S that belong to a triad. TPR = (# of nodes belonging to a triad)/n |
| Expansion (External Degree) | External | This measure of separability gives the average of number of external connections (c<sub>s</sub>) per node (n<sub>s</sub>) in subset S has with graph G.  It can be thought of "External Degree".  Expansion = c<sub>s</sub>/(n<sub>s</sub>(n-n<sub>s</sub>)).
| Cut Ratio (External Density) | External | This metric is a measure of sperarability and can be thought of as "External Density".  It is the fraction of external edges (c<sub>s</sub>) of subset S out of the total number of possible edges in graph G. |
| Conductance [6,8] | Combo (Int. & Ext.) | Ratio of edges inside the cluster to the number of edges leaving the cluster (captures surface area to volume).  Discriminative Algorithm.  Measures best in separability (goodness scale); measures well-separated non-overlapping communities.  Robust under nodeswap and shrink perturbation.  Community like sets of nodes have lower conductance.
| Normalized Cut | Combo (Int. & Ext.) | Represents how well subset S is separated from graph G.  It sums up the fraction of external edges over all edges in subset S (conductance) with the fraction of external edges over all non-community edges. |
| Maximum Out Degree Fraction  (Maximum ODF) | Combo (Int. & Ext.) | This metric first finds the fraction of external conections to internal connections for each node (n<sub>s</sub>) in S.  It then returns the fraction with the highest value.  For example, S contain 5 nodes all connected to themselves.  There is one node in S that has 7 external connections.  The Max ODF = 7/4.  |
| Average ODF | Combo (Int. & Ext.) | The sum of the individual fraction of edges outside of the community over the total connections of a node in subset S.  It is then divided by the total number of nodes (n<sub>s</sub>) in subset S. |
| Flake-ODF | Combo (Int. & Ext.) | This is a fraction of the number of nodes that have fewer internal connections than external connections (***) to the number of nodes (n<sub>s</sub>) in subset S. |
| Density | Internal | No | Members of a large community are more likely not to be connected to each other.  Two nodes in the same community of size k connected to each other has a probability of [Omega (1/sq.rt(k))]--Arora et al. 2012.  This creates a low-density, large graph.|
      
##Algorithms      
        
               
####Algorithm Classes[7]
        
-  __Divisive__: Focus on edges and verticies that exist between communities. This class tends to be more repeatable, traditional and computationally expensive
-  __Model Based__: Considers an underlying model of statistical nature that can generate the division of the network.  Time complexity often is derived through testing, and is not explicity characterized.
- __Quality Optimization__: Usually maximizes the delta from some score such as modularity
- __Vertex Clustering__: Embeds the Graph into vector space in order to use conventional data clustering methods such as k-means
- __Cohesive Subgraph Discovery__:  This class finds communities of a particular predefined structure within the graph. For example, the structure could be a clique, n-clique, k-core, LS set, lambda set
       
####Algorithm Attributes

- __R__: Requires Number of Communities
- __R+__: Requires # of communities, but is able to guess if not provided
- __D__: Deterministic
- __G__: Global
- __L__: Local
- __H__: Heirarchical
- __A__: Agglomerative
- __O__: Overlapping
- __Q__: Distributable 
- __S__: Requires community size
      
      
####Graph Input Types
- __w__ = "weighted"
- __-w__ = "unweighted"
- __d__ = "directed" 
- __-d__ = "non-directed"
        

####Framework Descriptions
| Name | Author(s) | Homepage | Comments | 
| :----| :-------- | :------- | :------- |
| Snap || http://snap.stanford.edu ||
| igraph ||http://igraph.sourceforge.net/index.html| igraph is a free software package for creating and manipulating undirected and directed graphs. It includes implementations for classic graph theory problems. igraph has four implementations: R, C/C++, Python, and Ruby |
| NetworkX |||
| MatLab |||

####Algorithm Table

|Algorithm Name/Year| Class | Time/Space Complexity (n = # of vertices, m = # of edges) | Algo Attributes | Implementations | Input Graph Type | Comments |
| :---------------- | :---- | :---------------------| :-------------- | :-------------- | :--------------- | :------- |
|Girvan-Newman (__Shortest-path betweenness__), 2004  [11] | Divisive | &Omicron;(nm<sup>2</sup>): Each iteration uses a tree structure to calculate edge betweeness of a graph in O(mn). Do this m times, once for each edge| D,H,G | Mathematica | w,-d | Iteratively removes links with the highest betweenness score (a score that measures the number of shortest paths that pass through an edge) |
| Girvan-Newman (__random-walk betweenness__), 2004 [11][16] | Divisive | &Omicron;((n+m)mn<sup>2</sup>) | ||| Uses random traversal through a graph |
| Girvan-Newman (__current-flow betweenness__), 2004 [11][16]| Divisive | &Omicron;((n+m)mn<sup>2</sup>) | ||| Uses a resistor network concept |
| Infomaps[13] | Model Based/Information Theory/ Random Walk | &Omicron;((n<sup>2</sup>)Log(n)) | snap, igraph | |w,d | Attempts to find partition that yields the minimum description length of an infinite random walk on the network. It determines community and network structure by analyzing the flow of information, proxied through random walk calculations, among various groups of nodes.
| Fast Newman [12] | Quality Optimization | O((m + n)n) | A,H | | Snap | w,-d | Optimize Q over all divisions using agglomerative hierarchical clustering | 
| Clauset-Newman-Moore [9] | Model Based, Quality Optimization | O(md log n) where d is depth of dendrogram | ||Snap, igraph | Improves upon Fast Newman, remains based oon the greedy optimization of modularity.  A modularity of >0.3 is a good indicator of a significant community structure |
| BIGCLAM (Cluster Affilitation Model for Big Networks)[14] | Model Based | Unclear | O,H,G,Q,R+ | Snap | -w,-d | Partitioning that most likely produces the edges of a given graph, given the notion that nodes in the same community are more likely to share an edge. It captures the probability that a pair of nodes are connected as a funciton of that shared membership. http://www.youtube.com/watch?v=htWQWN1xAZQVideo Lecture</a> |
|Label Propogation, 2007 [17] | Vertex Clustering | |A|| | Label Propagation uses the structure of the graph to cluster vertices.  Nodes are iteratively labeled with the label that most of its neighbors holds.  Densely connected groups form a consensus with a unique label. </td>
| Fast Unfolding, 2008 [19] | Quality Optimization | H | igraph | |w,-d | An iterative algorithm that calculates modularity based on nearest neighbros, joining nodes if the values are positive, and then builds a new network whose nodes are now communities based on step one. This it repeated until maximum modularity is achieved.
| WalkTrap, 2005 [18] | Vertex Clustering, Quality Optimization | O(mn<sup>2</sup>) | A | igraph | w,-d | Defines a similarity metric between nodes based on distance to all other nodes from a random walk. Merges nodes in an agglomarative fashion that minimizes distance from other nodes in the community. Then picks partition that has highest modularity.
| CESNA (Communities from Edge Structure and Node Attributes) [26] | Subgraph Discovery | 1 million nodes in 10 hours | Q,R+ | Snap | -w,-d |                   Detects overlapping communities in networks with node attributes.  Assumes that nodes that belong ot the same communities are likely to be connected to each other, communities can overlap, the more common communities a set of nodes share the more likely they are to be connected, and nodes in the same community are likely to share common attributes
| Radicchi et al [15] | Model Based, Divisive | O(m<sup>2</sup>) on large systems | None | |-d  | A local algorithm that returns similar results to Girvan Newman betweenness but reduces computational cost by using a local quantitiy--the number of loops of a given length that contains an edge--to choose which edge to remove. |
| Leading Eigenvector, 2006 [20]| | || igraph || 
| Latent Space Models [21]| |||||
| CoDA (Communities through directed affiliations)[23] | Model Based | Unclear | O,H,G,Q,R+ | snap | -w,d | Extension of BIGCLAM that incorporated directedness to discover community type. It detects overlapping communities as wel as cohesively and 2-mode communities, whether connected or hierarically nested.
| Clique Clique Percolation [27]| Cohesive Subgraph Discovery | G,D,S | CFinder | |-w,-d | Begins by identifying all cliques of size k in a network.  All cliques are collapsed into single vertex.  Vertices are connected if cliques that they are representing share k-1 members—connected components identify which cliques compose a community. Input value of k must be chosen and typically is between 3 and 6.</td>
| CONGA (Cluster-Overlapping Newman Girvan Algorithm), 2007 [28] | Divisive | O(m<sup>3</sup>) | D,G,O | None | -w,-d | CONGA, like Girvan Newman, relies on calculation of betweenness to determine splits, but splits on vertices, rather than edges. The algorithm specifies when and how to split vertices allowing for overlapping membership |
| CONGO (CONGA Optimized), 2008 [32] | Divisive | O(n log n) Sparse Networks | D,G,O | None | -w,-d | Expands upon the CONGA algorithm by achieving a local form of betweenness |
|Fuzzy Overlapping Group (FOG) [29] | Divisive | O(L<sup>3</sup>) | O | ORA  | | A combination of a stochastic model and maximum likelihood method group detection algorithm for inferring fuzzy overlapping groups. Groups are built from links which allows for multiple memberships varying levels of association from entities to groups. |
| Bipartite Stochastic Block Model (biSBM), 2014 [30] | Model Based | O(N<sub>a</sub>K<sub>a</sub>(K<sub>a</sub> + k)) + O(N<sub>b</sub>K<sub>b</sub>(K<sub>b</sub> + k)) | Matlab || w | The degree corrected Bipartite Stochastic Block Model uses probability to search for maximum likelihood partition of a network into communities.  It models a network's observed degree sequence before finding community structure.  It divdes verticies into groups of like types.  These calculations are done iteratively to continually propose movements of vertices that increases the likelihood function. In ref to run time, k is avg degree of each node, N_a and N_b or number of vertices of type a and b, K_a and K_b are number of communities of type a and b</td>
| Spectral Clustering [31] | Model Based | Determined by Eigenvalue Computation | L,Q,R || w,d | Spectral clustering algorithms rely on the calculation of eigenvalues of a similarity matrix to find optimal partitions, give a predetermined number of partitions.  It relaxes the complex problem of minimizing cut ratio over every possible k-partition to find the k-smallest eigenvalues and related eigenvectors of the Laplacian of the graph.
| RolX  | Role identification/discovery | Q | SNAP || -w,-d | Generates list of roles as "profiles" of local/global node metrics, and assigns nodes a linear combination of these roles. 



        
##Notable Papers
| Topic | Title | Comments |
| :---- | :---- | :------- |
| Algorithm Comparisons | Community Detection Algorithms: A Comparative Analysis [3]
| General Overview | Communities in networks [4] |
| General Overview | Community Detection in Graphs (2010) [5] |
| General Overview | Community Detection in Social Media [7] | 
| Algorithm Comparisons | Empirical Comparison of Algorithms for Network Community Detection [6] |
| Algorithm Evalutation | Defining and Evaluating Network Communities based on Ground-truth[8]|
| Centrality Measures / Community Detection | Using Centrality Measures to Identify Key Members of an Innovation Collaboration Network [24] |
| Overlapping Community Detection | Overlapping community detection in networks: the state-of-the-art and comparative study [25]|
        
        

                
##References
1. Michelle Girvan and M. E. J. Newman, 2001 http://arxiv.org/pdf/cond-mat/0112110.pdf, Community structure in social and biological networks
2. M E J Newman, 2001  href="http://www-personal.umich.edu/~mejn/papers/016132.pdf, Scientific collaboration networks. II. Shortest paths, weighted networks, and centrality
3. Andrea Lancichinetti and Santo Fortunato, 2010 http://arxiv.org/pdf/0908.1062v2.pdfCommunity detection algorithms: a comparative analysis
4. Mason A. Porter, Jukka-Pekka Onnela, and Peter J. Mucha, 2009 http://www.ams.org/notices/200909/rtx090901082p.pdfCommunities in Networks
5. Santo Fortunato, http://arxiv.org/pdf/0906.0612v2.pdfCommunity detection in graphs
6. Jure Leskovec, Kevin J. Lang, Michael W. Mahoney, http://cs.stanford.edu/people/jure/pubs/communities-www10.pdfEmpirical Comparison of Algorithms for Network Community Detection
7. Symeon Papadopoulos, Yiannis Kompatsiaris, Athena Vakali, Ploutarchos Spyridonos, http://static.springer.com/sgw/documents/1387631/application/pdf/10-3.pdfCommunity detection in Social Media Performance and application considerations
8. Jaewon Yang and Jure Leskovec, http://cs.stanford.edu/people/jure/pubs/comscore-icdm12.pdfDefining and Evaluating Network Communities based on Ground-truth
9.  Aaron Clauset, M. E. J. Newman, Cristopher Moore, http://arxiv.org/pdf/cond-mat/0408187.pdfFinding community structure in very large networks
10. Leon Danon, Albert D ́iaz-Guilera, Jordi Duch, and Alex Arenas http://arxiv.org/pdf/cond-mat/0505245.pdfComparing community structure identification
11. M E J Newman, and M. Girvan, 2004,  http://arxiv.org/pdf/condmat/0308217.pdfFinding and evaluating community structure in networks
12. M E J Newman, http://arxiv.org/pdf/cond-mat/0309508.pdfFast algorithm for detecting community structure in networks
13. Martin Rosvall and Carl T. Bergstromhttp://www.mapequation.org/assets/publications/RosvallBergstromPNAS2008Full.pdfMaps of random walks on complex networks reveal community structure
14. Jaewon Yang and Jure Leskovec http://infolab.stanford.edu/~crucis/pubs/paper-nmfagm.pdfOverlapping Community Detection at Scale: A Nonnegative Matrix Factorization Approach
15. Filippo Radicchi, Claudio Castellano, Federico Cecconi, Vittorio Loreto, and Domenico Parisi http://www.pnas.org/content/101/9/2658.full.pdf+htmlDefining and identifying communities in networks
16. M.E.J. Newman, 2004, http://arxiv.org/pdf/condmat/0309045.pdfA measure of betweenness centrality based on random walks
17. Usha Nandini Raghavan, Reka Albert, Soundar Kumara, 2007, http://arxiv.org/pdf/0709.2938v1.pdfNear linear time algorithm to detect community structures in large-scale networks
18. Pascal Pons and Matthieu Latapy, 2007 http://arxiv.org/pdf/physics/0512106v1.pdfComputing communities in large networks using random walks
19. Vincent D. Blondel1; Jean-Loup Guillaume, Renaud Lambiotte, and Etienne Lefebvre, http://arxiv.org/pdf/0803.0476v2.pdfFast unfolding of communities in large networks
20. M E J Newman, 2006 http://arxiv.org/pdf/physics/0605087v3.pdfFinding community structure in networks using the eigenvectors of matrices
21. Lei Tang, Huan Liu, Community Detetection and Mining in Social Media<br>
22. Conrad Lee and Pádraig Cunningham, 2013http://comnet.oxfordjournals.org/content/2/1/19.full.pdf+htmlCommunity detection: effective on large social networks
23. Detecting Cohesive and 2-mode Communities in Directed and Undirected Networks     by J. Yang, J. McAuley, J. Leskovec.     ACM International Conference on Web Search and Data Mining (WSDM), 2014. <br> 
24. John Cardente, http://dsgeek.com/docs/jcardente_cs224w_projreport.pdfUsing Centrality Measures to Identify Key Members of an Innovation Collaboration Network 
25. Jierui Xie, Stephen Kelley, Boleslaw K. Szymanski, http://arxiv.org/abs/1110.5813Overlapping community detection in networks: the state-of-the-art and comparative study
26. Jaewon Yang, Julian McAuley, Jure Leskovec, Community Detection in Networks with Node Attributes
27. Gergely Palla, Imre Derenyi, Illes Farkas, and Tamas Vicsek, Uncovering the overlapping community structure of complex networks in nature and society.
28. Steve Gregory, An algorithm to Find Overlapping Community Structure in Networks.
29. George David and Kathleen Carley, Clearing the FOG: Fuzzy, Overlapping Groups in Social Networks. 
30. Daniel Larremore, Aaron Clauset, and Abigail Jacobs, Efficiently inferring community structure in bipartite networks.</a><<br>
31. Ulrike von Luxburg, A Tutorial On Spectral Clustering.
32. Steve Gregory, http://www.cs.bris.ac.uk/publications/Papers/2000885.pdfA Fast Algorithm to Find Overlapping Communities in Networks</a>, 2008