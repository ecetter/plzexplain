
# Parallel Processing

Multi-core computing architectures are capabile of running many computations concurrently if the software is able to distribute the work to the available processor cores. By crafting software that can exploit multiple available cores, typically the computation time can be reduced. The data and tasks can be divided among the computational units (i.e. cores) and then recombined after the computations occur. The specifics as to how the data and tasks are divided up among the computational units is dependent upon the specific computational problem and its computational reducibility.

The first step to parallelizing software is to establish the computational problem with flowcharts and diagrams representing the datasets and the computations to be performed. Once the computational problem is established, it can then be broken down into *units of computation*, which include chunks of data and the computational processes to be done on those chunks, so as to reduce the amount of interdependent relationships between the units of computation. If the computational problem cannot be broken down into separate, independent units of computation, then it is considered computationally irreducible. Computationally irreducible problems can not take advantage of parallelization. Without first properly defining the computational problem, it is difficult to understand if the problem will benefit from parallelization, and it is even more difficult to parallelize. While parallelizing, it is highly encouraged to create a smaller, but representative, subset of the data that can be used to benchmark the speedup gained through parallelization. Benchmarking is the process of timing different runs of the job using different numbers of cores to evaluate how the compute time changes as the number of cores grows. Only a portion of the computational job can be run in parallel, and this serial portion of the computational job will not benefit from the use of additional cores. If $f$ represents the proportion of computation time that must be performed serially, as the number of cores ($P$) is increased, then the computation time ($T$) follows the relationship known as Amdahl's Law:

$$ T \approx f + \frac{1-f}{P} $$

Utilizing additional cores will reduce the compute time of the parallel portion of the job, but will do nothing to speed up the portion that must be performed serially. It is important to recognize, however, that making more cores available to a computational job will not improve the performance unless the software has been configured to utilize the additional cores. 

In addition to the time taken to parallelize the software, there will also be some additional overhead time cost to setting up parallel processes and aggregating the information after the computations occur. Also, there will be additional overhead in time if the units of computation must share data as the data will have to be transferred, and some time may be required to conduct these hand-offs of data. These phenomena will add additional compute time to the estimate provided by Amdahl's Law.


## Some Key Terms in Parallel Computing

| Term | Definition |
| ---- | ---- |
| Node | A single computer consisting of a motherboard, memory, and processor(s). It requires more effort to distribute computational work among multiple nodes than to distribute work among cores on a single node. |
| Processor or Socket | The physical chip containing one or, usually, multiple cores. |
| Core | The smallest computational unit of the processor, capable of running a single computational task. To avoid confusion, this will be often referred to as a *processor core* in these materials. |
| Multi-Core Processor | A physical processor within which many cores are embedded. |
| Hardware Thread or Logical Core | A computational unit within a core that could allow the core to do multiple things at once, known as hyperthreading. |
| Worker | An independent process that conducts the computation. |
| Compute Time or Wall Time | The total time the job takes to run to completion. |
| Core-Hours | The product of the number of cores a job uses to run times the number of hours it takes to run. For example, a 4-core job that takes 2 hours to run requires 8 core-hours. |
| Speedup Ratio | For a parallel job, the serial execution time divided by the parallel execution time. |
| Parallel Efficiency | For a parallel job, the speedup ratio divided by the number of cores used. |


## Scaling Analysis

Selecting the optimal number of nodes and tasks/cores is done by performing scaling analysis. Again, it is recommended to generate a minimal representative dataset to perform scaling analysis. Only enough data has to be used to produce trends so the test cases can be compared against one another, and the job just needs to run long enough such that the one-time start-up operations can be ignored. If necessary, add timing statements to the software so that only the relevant sections are measured.

The total time to completion of a job consists of the amount of time the job is queued plus the amount of time it takes the job to run to completion once placed. The queue time is minimized when the bare minimum amount of resources are requested, and the queue time grows as the amount of requested resources grow. The run time of the job is minimized when all of the computational resources available to the job are efficiently utilized. The total time to completion, therefore, is minimized when the resources requested closely match the amount of computational resources that can be efficiently utilized by the job. The process of scaling analysis involves scaling the number of cores used, and perhaps the number of nodes for larger number of cores, and timing the result. After gathering the run times of the various core configurations, the both the speedup ratio and the parallel efficiency can be calculated for the various runs to show the trends related to the core configurations. The best decision for the core configuration to run the full computational job is to choose the smallest set of resources that gives a reasonable speedup over the baseline case. Selecting the smallest set of resources will minimize queue time while also reducing the compute time.

