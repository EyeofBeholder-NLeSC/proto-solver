# proto-solver
A prototype of applying the solver to QBAF that contains support links and nodes of topics and products in a single graph.

## Installation and usage
1. Clone the repo to anywhere on your local machine
```
git clone https://github.com/EyeofBeholder-NLeSC/proto-solver.git
```
2. We recommend to create a new virtual environment and install everything there. To do that and activate it:
```
python -m venv venv
source ./venv/bin/activate
```
3. Install dependencies for running the notebook:
```
pip install -r requirements
```
4. Start jupyter and run the notebook in your web browser:
```
jupyter notebook
```

## Optimization of the performance of solving algorithm
The newly developed `reasoner` module (https://github.com/EyeofBeholder-NLeSC/orange3-argument/tree/dev_graph_solver/orangearg/argument/reasoner) is applied. Compared to that, there is one change in the code, which is to optimize the computational performance on the specific structure of the input graph, which is more like a star graph but with directions. In our target case of QBAF, the parent vectors of all leaf nodes (nodes of chunks) are zero vectors, which means that their strength will never be updated after aggregation and influence computation. This becomes the major bottleneck.

To avoid computing that, a parameter called `update_last` is added to the `Model.compute_delta` function. By indicating that, the algorithm will only compute the delta for the last number of nodes. We tested the performance with the given dataset, for solving a single graph, the optimized version of the model is 46 times faster than the original one, resulting in 1m40s of iterating over all reviews for removing the from the chunk file.

![image](https://github.com/EyeofBeholder-NLeSC/proto-solver/assets/92043159/ee51c508-2287-4d4b-a5bd-6067e431e0f2)
![image](https://github.com/EyeofBeholder-NLeSC/proto-solver/assets/92043159/0455c912-9fb4-47de-b3d6-1356985475f4)

## Significance of the output
By looking at the output, it can be observed that the impact of a single review is very small, and it comes to the problem of applying this result as one of the features in the next step of classification. But still, the difference in the impact of different reviews can be observed. 

![image](https://github.com/EyeofBeholder-NLeSC/proto-solver/assets/92043159/d11849f9-f57a-4562-bad8-6c219c452aff)
