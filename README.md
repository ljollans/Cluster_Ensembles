# Cluster_Ensembles
A package for combining multiple partitions into a consolidated clustering. The combinatorial optimization problem of obtaining such a consensus clustering is reformulated in terms of approximation algorithms for graph or hyper-graph partitioning.

Installation
------------

Cluster_Ensembles is written in Python and in C. The original package was written for Python 2.7, but this version was configured to run on MacOS using Python 3.7.

An important prerequisite is having gpmetis installed.
This code was tested using version metis-5.1.0 downloaded from http://glaros.dtc.umn.edu/gkhome/metis/metis/download
Once the package is downloaded run
```
gunzip metis-5.x.y.tar.gz
tar -xvf metis-5.x.y.tar
```
then navigate into the metis directory and run
```
make config
make
```

this will build the package.
For Cluster_Ensembles to find gpmetis in my PATH I had to copy gpmetis from metis-5.1.0/build/Darwin-x86_64/programs to /usr/local/bin


Usage
-----

Say that you have an array of shape (M, N) where each row corresponds to a vector reporting the cluster label of each of the N samples comprising your dataset. It is possible that some of those samples have been left out of consideration from some of those M clusterings; in this case, the corresponding entry is tagged as NaN (```numpy.nan```). 

The few lines below illustrate how to submit consensus clustering analysis such an ```cluster_runs``` (M, N) array of cluster labels. 
A vector holding the consensus clustering identities for each of the N samples in your dataset, ```consensus_clustering_labels```, is returned.

Please note that those M vectors of clustering labels can correspond to partitions of the samples into distinct numbers of overall clusters. Cluster_Ensembles therefore offers the possibility of seeking a consensus clustering from the aggregation of a clustering of your dataset into, say, 10 groups, another clustering of a fraction of your samples into 5 clusters, yet another partition of your dataset into 20 clusters, etc. Those choices are entirely up to you. Pretty much all that is required for Cluster_Ensembles is an array of clustering vectors. 

```
>>> import numpy as np
>>> import Cluster_Ensembles as CE
>>> cluster_runs = np.random.randint(0, 50, (50, 15000))
>>> consensus_clustering_labels = CE.cluster_ensembles(cluster_runs, verbose = True, N_clusters_max = 50)
```

References
----------
* Giecold, G., Marco, E., Trippa, L. and Yuan, G.-C.,
"Robust Lineage Reconstruction from High-Dimensional Single-Cell Data". 
ArXiv preprint [q-bio.QM, stat.AP, stat.CO, stat.ML]: http://arxiv.org/abs/1601.02748
* Strehl, A. and Ghosh, J., "Cluster Ensembles - A Knowledge Reuse Framework
for Combining Multiple Partitions".
In: Journal of Machine Learning Research, 3, pp. 583-617. 2002
* Kernighan, B. W. and Lin, S., "An Efficient Heuristic Procedure 
for Partitioning Graphs". 
In: The Bell System Technical Journal, 49, 2, pp. 291-307. 1970
* Karypis, G. and Kumar, V., "A Fast and High Quality Multilevel Scheme 
for Partitioning Irregular Graphs".
In: SIAM Journal on Scientific Computing, 20, 1, pp. 359-392. 1998
* Karypis, G., Aggarwal, R., Kumar, V. and Shekhar, S., "Multilevel Hypergraph Partitioning: 
Applications in the VLSI Domain".
In: IEEE Transactions on Very Large Scale Integration (VLSI) Systems, 7, 1, pp. 69-79. 1999
