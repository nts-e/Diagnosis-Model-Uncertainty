# Model‐Based Diagnosis with Uncertain Observations

Model-Based Diagnosis (MBD) is a computational approach to determine why a system under observation does not behave as expected. It uses diagnosis algorithms to find the faulty components that caused the unexpected behavior of the system.

The following project suggests a new method to run MBD with uncertain output observations.

The first step of the project was to establish an algorithm that computes the diagnoses and their probabilities, with a complexity which is better than the default BFS-based solution. The solution proposed is based on a DFS algorithm which I’ve built from scratch. The algorithm runs over the tree only once, and smartly stores the diagnoses per node. Each path from the root to a node is a diagnosis of a single output.

In the second step of the project I’ve created several improvements for the performance of that basic DFS algorithm.

Early Stopping is a mechanism that enables the DFS tree to return early once all outputs of the branch are satisfied (all output permutations were diagnosed).

Another mechanism, Pre-Diagnosis , works hand in hand with the Early Stopping mechanism. Pre-Diagnosis runs MBD with low cardinality (a short tree) prior to the full run. That short tree is then diagnosed to produce diagnoses with low cardinality. Only then the full run is being executed. During the full run, a node is being checked to see if its path is a superset of the pre-diagnosis. If it is, the diagnosis output is marked as found, much before the time it would have taken to the DFS tree to arrive to that node. Pre-Diagnoses increases the probability for Early Stopping to occur, which accelerates the run. The overheads of the additional checks that are required for in each node are controlled by Random Sampling. Only 10% of the nodes make the check of the pre-diagnosis.

The last improvement that contributed to the acceleration of the run is Ranking the gates of the DFS tree queue. Instead of feeding the algorithm with the gates sequentially, I consider the proximity of the gates to the system’s outputs. The higher the proximity to the outputs, the higher the rank. The tree is created such that the high ranked gates are accessed first.

The results show that these features make the algorithm run faster than the base DFS.

A full report is available upon demand.

----------------------

An example for a system in which the output observations are not certain, with 6 NAND gates, 5 inputs and 2 outputs:

![image](https://github.com/nts-e/Diagnosis-Model-Uncertainty/assets/107881111/2be913f3-f0b0-4fbd-901e-fcfa3adb08cf)

The figure  shows the observation inputs and outputs when all the components are healthy (white=0, yellow=1).
In a system with output observations that are certain (no noise), and given any other output observation than the true observation [1,1], we can determine that at least one of the NAND
gates is not healthy. Then, we will run MBD to find the diagnoses that will indicate the faulty gates.
For this project, that deals with a system with uncertain observations, we will be required to find the diagnoses for each of the 4 possible outputs ([0,0], [0,1], [1,0], [1,1]). Each scenario results another set of diagnoses. The probability of the diagnoses will be calculated per case.


