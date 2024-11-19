# Heuristics for a capacitated facility location problem with a quadratic objective function
This project contains the code used in both M. Schmidt's completed master thesis and the article **The Balanced Facility Location Problem: Complexity and Heuristics** by M. Schmidt and B. Singh which is currently under review. A preprint of the article is available on [Optimization Online](https://optimization-online.org/2024/03/the-balanced-facility-location-problem-complexity-and-heuristics/). We provide the implementations of all the algorithms and heuristics discussed in the article as well as the the instances we use to test our heuristics on.

## Repository content
Please refer to the preprint for the terminology. The repository is structured as follows:
- ```data```: This folder contains the data for the four instances. For each instance, there is a file containing the travel probabilities $P_{ij}$ (e.g., ```instance_1_travel_dict.json.pbz2```) and a file containing the rest of the input data in a dataframe (e.g., ```instance_1_users_and_facs.csv```). For each instance, we also provide a corresponding instance where the capacity has been scaled to $C_j = \sum_{i \in I} U_i P_{ij}$ (```instance_1_suff_users_and_facs.csv```). 
To load the data, we provide the function ```load_an_instance```. An example of loading Instance 1 without sufficient capacity can be seen below. ```users``` is the set $I$ and ```facs``` is the set $J$. Note that in our use of the instances we scale the input capacities by a factor. If ```cap_factor``` is provided as an input to the function, then set it: to 1.0 for any of the sufficient capacity instances, to 1.5 for Instance 1 or Instance 2, and to 0.8 for Instance 3 or Instance 4.
```python
users_and_facs_df, travel_dict, users, facs = load_an_instance(1, False)
```
- ```heuristics_and_mips```: This folder contains the main part of the code. We briefly describe the purpose of each of the files:
	- ```BFLP_heuristics.py```: This file contains the code for all the BFLP heuristics. 
	- ```BFLP_MIP.py```: This file contains the code for running the BFLP MIP.
	- ```BUAP_heuristics.py```: This file contains all the BUAP heuristics and also implementations of those that are just needed for use within the BFLP heuristics; e.g., the optimization of `relaxation rounding` when used in the first version of `close greedy`.
	- ```BUAP_MIP.py```: This file contains the code for running the BUAP MIP and also for editing the model, as needed for `relaxation rouding` with the first version of `close greedy`.
	- ```results_heuristics.py```: This file contains functions for running all the heuristics across multiple budget factors and input parameters. For each BFLP heuristic, we provide a function for running the heuristics with multiple inputs (see the function name starting with `get_`) is provided that results in a JSON file. We also provide a function for converting these results into more easily readable Excel tables (see the function name starting with `write_`). Any results created here are saved in the `own_results` folder, which is created if it does not exist yet. For the BUAP, there is one function that runs all the heuristics on a single instance and the corresponding function which writes the results into a table.
	- ```utils.py```: This file contains a few subroutines that are useful for reading and writing data, and some subroutines that are used in multiple heuristics.
 	- ```main.py```: This file contains a concrete example of how to run the functions in the repository. The line `results = ` may be changed to run the relevant heuristic (with the appropriate data input existing).

## Example

We provide a short concrete example of how to our code via the ```main.py``` file. To run this file, make sure you have navigated to the repository folder in your terminal (see also the code requirements above. Then, you can run the file using ```python3 ./heuristics_and_mips/main.py```.
In this example, we run the open greedy algorithm on Instance 1 to test out how different values for the parameters $n_c$ and $d$ perform across different budgets. This is assuming that we have the results for the BFLP MIP for this instance saved in the file ```instance_1_BFLP_MIP.json``` in the own_results folder. 


## Requirements for running the code
Before cloning the code, make sure you have [git large file storage](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) installed. Note that the data files will not be properly downloaded if you just download the zip file.
Before running the code, make sure you have the below installed:
- python3
- numpy
- pandas
- geopy
- pyomo
- gurobi
