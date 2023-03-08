
# Job Arrays

Job arrays are used for running the same job many times with only slight differences between the jobs. For instance, let's say that you need to run 100 jobs, each with a ifferent seed value for the random number generator, or maybe you want to run the same analysis script on data for each of the 50 states in the US. Job arrays are the best choice for such cases.

Below is a sample submission script for running Python where there are 5 jobs in the array:

```
#!/bin/bash

#SBATCH --job-name=py-array        # give the job a name
#SBATCH --account=open             # specify the account
#SBATCH --partition=open           # specify the partition
#SBATCH --nodes=1                  # request a node
#SBATCH --ntasks=1                 # request a task / cpu
#SBATCH --mem=1G                   # request the memory required per node
#SBATCH --time=00:01:00            # set a limit on the total run time
#SBATCH --output=slurm-%A.%a.out   # stdout file
#SBATCH --error=slurm-%A.%a.err    # stderr file
#SBATCH --array=0-4		   # job array with index values 0, 1, 2, 3, 4

echo "My SLURM_ARRAY_JOB_ID is $SLURM_ARRAY_JOB_ID."
echo "My SLURM_ARRAY_TASK_ID is $SLURM_ARRAY_TASK_ID."
echo "Executing on the machine: " $(hostname)

module purge
module load anaconda3
conda activate py_env

python pyscript.py
```

In this example, the submission script will run five jobs. Each job will have a different value of SLURM_ARRAY_TASK_ID (i.e. 0, 1, 2, 3, 4). The value of SLURM_ARRAY_TASK_ID can be used to differentiate the jobs within the array. One can either pass SLURM_ARRAY_TASK_ID to the executable as a command-line parameter or reference it as an environment variable. Using the latter approach, the first few lines of a Python script (called pyscript.py above) might look like the following:

```
import os
idx = int(os.environ["SLURM_ARRAY_TASK_ID"])
parameters = [2.5, 5.0, 7.5, 10.0, 12.5]
myparam = parameters[idx]
# execute the rest of the script using myparam
```
For an R script:
```
idx <- as.numeric(Sys.getenv("SLURM_ARRAY_TASK_ID"))
parameters <- c(2.5, 5.0, 7.5, 10.0, 12.5)
myparam <- parameters[idx + 1]
# execute the rest of the script using myparam
```
For Matlab:
```
idx = uint16(str2num(getenv("SLURM_ARRAY_TASK_ID")))
parameters = [2.5, 5.0, 7.5, 10.0, 12.5]
myparam = parameters(idx + 1)
# execute the rest of the script using myparam
```
For Julia:
```
idx = parse(Int64, ENV["SLURM_ARRAY_TASK_ID"])
parameters = (2.5, 5.0, 7.5, 10.0, 12.5)
myparam = parameters[idx + 1]
# execute the rest of the script using myparam
```

Be sure to use the value of SLURM_ARRAY_TASK_ID to assign unique names to the output files for each job in the array, as a failure to do so will result in all the job using the same filename.

You can set the array numbers to different sets of numbers and ranges, for example:
```
#SBATCH --array=0,100,200,300,400,500
#SBATCH --array=0-24,42,56-99
#SBATCH --array=1-1000
```

However, the array numbers must be less than the maximum number of jobs allowed in the array.

