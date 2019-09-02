# Explore

*Explore* is a **scientific workflow management tool** for MATLAB<sup>&reg;</sup> experiments. You can visualize the data provenance graph and when your code is changed, only the necessary functions are re-executed to provide an updated version of your data. This can save a lot of time, especially when experiments are constantly refined.

## Summary

*Explore* is a MATLAB<sup>&reg;</sup> class which implements automatic persistent memoization [1]. You can easily declare an experiment as a *directed acyclic graph* (DAG) where the nodes are functions and the edges represent variables that are produced and consumed by functions. 

During the first execution of the graph, variables are persisted to the disk which implies a longer graph execution time (due to variable loading and saving). However, for future executions, if the node and all the sub-functions called within the node remain unchanged, the results will simply be retrieved.

For data-intensive and compute-intensive tasks, one does not necessarily have access to computer clusters or does not necessarily have resources to integrate the experiments into a separated data pipeline tool. In this case, *Explore* is the right tool to persist automatically intermediate results to the disk.

![Example of an Explore graph plot](/fig/explore.png)

## Quick Start

1. Download this repository.
2. Add the `./test` and the `./src` folders to the MATLAB<sup>&reg;</sup> path.
3. Run and read the comments of the `./test/testExplore0.m` script.

## Tutorial

- **Basics**: Run and read the [`./test/testExplore0.m`](/test/testExplore0.m) script.
- **Confirmed**: Run and read the [`./test/testExplore1.m`](/test/testExplore1.m) script.
- **Advanced**: A complete description of the important concepts of *Explore* is available in the form of a Jupyter notebook ([`./example/notebook/tutorial1.ipynb`](/example/notebook/tutorial1.html)). You can either read and copy the command in your MATLAB<sup>&reg;</sup> editor or install Jupyter with a Matlab kernel.

## What is (not) *Explore* ?

*Explore* is **not**:

- a substitute to workflow schedulers like Apache Airflow 
- designed to parallelize tasks. Indeed, the hashes of the persisted data are computed using the function full paths as input. Therefore, running an unchanged graph from another location will trigger re-computations even if the code remains unchanged

*Explore* is a lightweight tool:

- to **avoid unnecessary re-computations**. This allows developers and scientists to iterate faster by automatically re-using results from previous runs.
- to **improve reproducibility in computational research**. Indeed, a majority of the 10 rules of reproducibility described in [2] can be totally or partially fulfilled:
  - **Keep track of data provenance** (rule 1): Assuming the input data of a graph is not modified between different runs, it is possible to keep track of how every result (intermediate or final) is produced.
  - **No manual data manipulation** (rule 2): The data is stored in the *Explore* root folder, separated from the scripts and functions. It should only be modified using *Explore* commands.
  - **Record intermediate results** (rule 5): This is one of the core concept of *Explore*. Moreover, automatic persistent memoization allows fault tolerancy
  - **Record random seeds** (rule 6): *Explore* keeps track of an history of runs including the random seeds.
  - **Store raw data behind plots** (rule 7): Plots can be generated by a specific class of nodes called `'leaf'`. These nodes should only be used to plot the data. Then, it is possible to view all the plots by re-running all the `'leaf'` nodes.
  - **Connect statements to results** (rule 9): *Explore* can be included in literate programming tools like Jupyter Notebooks [3]

## Author

This work is authored by Jonathan Ah Sue (<jonathan.ahsue@gmail.com>).

## References

This work is based on several contributions that have been slightly modified:

- Jan (2011). [DataHash](https://www.mathworks.com/matlabcentral/fileexchange/31272-datahash), MATLAB Central File Exchange.
- Ben Mitch (2002). [rgb.m](https://www.mathworks.com/matlabcentral/fileexchange/1805-rgb-m), MATLAB Central File Exchange.

A nice overview on the concepts used in this work as well as the motivations of such a tool can be found in the following research papers:

[1] Guo, Philip J., and Dawson Engler. "Using automatic persistent memoization to facilitate data analysis scripting." *Proceedings of the 2011 International Symposium on Software Testing and Analysis*. ACM, 2011.

[2] Sandve, Geir Kjetil, et al. "Ten simple rules for reproducible computational research." (2013): e1003285.

[3] Kluyver, Thomas, et al. "Jupyter Notebooks-a publishing format for reproducible computational workflows." *ELPUB*. 2016.

Thank you !

