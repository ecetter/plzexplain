
# Slurm

On Roar Collab (RC), users can run jobs by submitting scripts to the Slurm job scheduler. A Slurm script must do three things:

1. prescribe the resource requirements for the job
2. set the job's environment
3. specify the work to be carried out in the form of shell commands

The portion of the job that prescribes the resource requirements consists of the scheduler directives. Scheduler directives in Slurm submission scripts are denoted by lines starting with the `#SBATCH` keyword. The rest of the script, which both sets the environment and specifies the work to be done, consists of bash commands. The very first line of the submission script, `#!/bin/bash`, is called a *shebang* and specifies to the command line environment to interpret the commands as bash commands.

Below is a sample Slurm script for running a Python task using a Conda environment

```
#!/bin/bash

#SBATCH --job-name=apythonjob      # give the job a name
#SBATCH --account=open		   # specify the account
#SBATCH --partition=open	   # specify the partition
#SBATCH --nodes=1		   # request a node
#SBATCH --ntasks=1		   # request a task / cpu
#SBATCH --mem=1G		   # request the memory required per node
#SBATCH --time=00:01:00		   # set a limit on the total run time

module purge
module load anaconda3
source activate py_env

python pyscript.py
```

In this sample submission script, the scheduler directives request a single node with a single *task*. Slurm is a task-based scheduler, and a task is equivalent to a processor core unless otherwise specified in the submission script. The scheduler directives then request 1 GB of memory per node for a maximum of 1 minute of runtime. The memory can be specified in KB, MB, GB, or TB by using a suffix of K, M, G, or T, respectively. If no suffix is used, the default is MB. The next block of the submission script sets the environment for the job by loading the **anaconda** module and then activating the **py_env** conda environment. Lastly, the work to be done is specified, which is the execution of a Python script.

The scheduler directives should be populated with resource requests that are adequate to complete the job but should minimal enough that the job can be placed somewhat quickly by the scheduler. The total time to completion of a job consists of the amount of time the job is queued plus the amount of time it takes the job to run to completion once placed. The queue time is minimized when the bare minimum amount of resources are requested, and the queue time grows as the amount of requested resources grow. The run time of the job is minimized when all of the computational resources available to the job are efficiently utilized. The total time to completion, therefore, is minimized when the resources requested closely match the amount of computational resources that can be efficiently utilized by the job. During the development of the computational job, it is best to keep track of an estimate of the computational resources used by the job. Add about a 20% margin on top of the best estimate of the job's resource usage in order to produce a job's resource requests used in the scheduler directives.

If the above sample submission script was saved as **pyjob.slurm**, it would be submitted to the Slurm scheduler with the [sbatch](https://slurm.schedmd.com/sbatch.html) command.
```
$ sbatch pyjob.slurm
```

The job should be submitted to the scheduler from a submit node on RC. The scheduler will keep the job in the job queue until the job gains sufficient priority to run on a compute node. Depending on the nature of the job and the availability of computational resources, the queue time will vary between seconds to many days. To check the status of queued and running jobs, use the [squeue](https://slurm.schedmd.com/squeue.html) command:
```
$ squeue -u <userid>
```


## Useful Slurm Commands

### Gathering System Information

| Command | Description |
| ---- | ---- |
| check_storage_quotas | compares your usage of disk space against the storage quotas to see the remaining storage space |
| cat /etc/os-release | provides information about the operating system |
| lscpu | provides information about the processors on a particular node |
| sinfo | shows how RC's nodes are being used |
| top | displays processor activity on a particular node (press 'q' to quit) |
| top -u <userid> | displays your own processor activity on a particular node (press 'q' to quit) |
| sprio | shows how job priority is designated |

### Submitting Jobs

| Command | Description |
| ---- | ---- |
| sbatch <submission-script> | submits your job to the scheduler |
| salloc | requests an interactive job on compute nodes |

### Monitoring Jobs

| Command | Description |
| ---- | ---- |
| squeue | displays all jobs that are running or are queued |
| squeue -u <userid> | displays jobs under "userid" that are running or are queued |
| squeue -j <jobid> | displays information about a specific job | 
| scontrol show jobid <jobid> | displays details information about a specific job |
| scancel <jobid> | cancels a job |


## Interactive Allocations Using salloc

The submit nodes of RC are designed to handle very simple computational tasks such as connections, file editing, and submitting jobs. Performing intensive computations on submit nodes will adversely impact other users' ability to interact with the system. For this reason, users that want to perform computations interactively should do so on compute nodes using the [salloc](https://slurm.schedmd.com/salloc.html) command. To work interactively on a compute node with a single processor core for half an hour, use the following command:
```
$ salloc --nodes=1 --ntasks=1 --mem=1G --time=00:30:00
```

The above command submits a request to the scheduler to queue an interactive job, and when the scheduler is able to place the request, the prompt will return. The hostname in the prompt will change from the previous submit node name to a compute node. Now on a compute node, intensive computational tasks can be performed interactively. This session will be terminated either when the time limit is reached or when the **exit** command is entered. After the interactive session completes, the session will return to the previous submit node.


## Multithreading

Some software like the linear algebra routines in NumPy and Matlab are able to use multiple processor cores via libraries that have been written using shared-memory parallel programming models, like OpenMP. OpenMP programs, for instance, run as multiple "threads" on a single node with each thread using one processor core. Below is a sample Slurm submission script for a multithreaded job:

```
#!/bin/bash

#SBATCH --job-name=matlab_mthread  # give the job a name
#SBATCH --account=open		   # specify the account
#SBATCH --partition=open	   # specify the partition
#SBATCH --nodes=1		   # request a node
#SBATCH --ntasks=1		   # request a task
#SBATCH --cpus-per-task=4	   # processor cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=1G	   # request the memory per processor core
#SBATCH --time=00:10:00		   # set a limit on the total run time

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

module purge
module load matlab

matlab -nodisplay -nosplash -r mthread_parfor
```

For a multithreaded single-node job, make sure that the product of ntasks and cpus-per-task is equal to or less than the number of processor cores on a node. Only codes that have been explicitly written to use multiple threads will be able to take advantage of multiple processor cores. Using a value of cpus-per-task greater than 1 for a code that has not been parallelized will not improve its performance and will instead waste resources.


## Parallel Jobs Using MPI

Many scientific programs use a form of distributed-memory parallelism based on MPI (Message Passing Interface). These programs are able to use multiple processor cores on multiple nodes simultaneously. The sample submission script below uses 12 processor cores on each of 3 nodes:

```
#!/bin/bash

#SBATCH --job-name=multinode       # give the job a name
#SBATCH --account=open             # specify the account
#SBATCH --partition=open           # specify the partition
#SBATCH --nodes=3                  # request multiple nodes
#SBATCH --ntasks-per-node=12       # request multiple tasks per node
#SBATCH --mem-per-cpu=1G           # request the memory	per processor core
#SBATCH --time=00:10:00            # set a limit on the total run time

module purge
module load openmpi

srun mympiprog <args>
```

MPI jobs are composed of multiple processes running across one or more nodes. The processes coordinate through point-to-point and collective operations. Slurm recommends using the **srun** command instead of using **mpirun** or **mpiexec**. Only programs that have been explicitly written to run in parallel can take advantage of multiple processor cores on multiple nodes. Using a value of ntasks greater than 1 for a program that has not been parallelized will not improve its performance and will instead waste resources.


## Multinode, Multithreaded Jobs

Some software combines multithreading with the multinode parallelism using a hybrid OpenMP/MPI approach. Below is a sample submission script for this case:

```
#!/bin/bash

#SBATCH --job-name=multinode       # give the job a name
#SBATCH --account=open             # specify the account
#SBATCH --partition=open           # specify the partition
#SBATCH --nodes=3                  # request multiple nodes
#SBATCH --ntasks-per-node=12       # request multiple tasks per	node
#SBATCH --cpus-per-task=4	   # processor cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=1G           # request the memory per processor core
#SBATCH --time=00:10:00            # set a limit on the total run time

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

module purge
module load openmpi

srun mympiprog <args>
```

For a OpenMP/MPI program, the above submission script would produce 12 MPI processes per node. When an OpenMP parallel directive is encountered, each process would execute the work using 4 processor cores. 


## GPU Jobs

GPUs are available on RC. To use GPUs, add a scheduler directive with the **--gres** option:

```
#!/bin/bash

#SBATCH --job-name=apythonjob      # give the job a name
#SBATCH --account=open             # specify the account
#SBATCH --partition=open           # specify the partition
#SBATCH --nodes=1                  # request a node
#SBATCH --ntasks=1                 # request a task / cpu
#SBATCH --mem=1G                   # request the memory required per node
#SBATCH --gres=gpu:1		   # request a gpu
#SBATCH --time=00:01:00            # set a limit on the total run time

module purge
module load anaconda3
source activate py_env

python pyscript.py
```

Only software that has been explicitly written to run on GPUs can take advantage of GPUs. Adding the **--gres** option to a Slurm script for a CPU-only program will not speed up the execution time and will just waste resources and increase the queue time. Furthermore, some codes are only written to use a single GPU, so avoid requesting multiple GPUs unless the program can use them.


## Job Dependencies

Job dependencies in Slurm allow multiple jobs to be run in sequence. This is useful for jobs that need to run for much longer than the time limit or when a second job can only be started after the first job is finished because of a dependency between the two.

