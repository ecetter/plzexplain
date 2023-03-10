
# Anaconda

Anaconda is a very useful package manager on ICDS systems. Package managers simplify software package installation and manage dependency relationships while increasing both the repeatability and the portability of software. The user environment is modified by the package manager so the shell can access different software packages. Anaconda was originally created for Python, but it can package and distribute software for any language. It is usually very simple to create and manage new environments, install new packages, and import/export environments. Many packages are available for installation through Anaconda, and it enables retaining the environments in a silo to reduce cross-dependencies between different packages that may perturb environments.

Anaconda can be loaded from the central software stack on Roar and RC with the following:
```
$ module load anaconda3
```


## Installation Example

After loading the **anaconda3** module, environments can be created and packages can be installed within those environments. When using anaconda for the first time on Roar or RC, the **conda init bash** command must be used to initialize anaconda, then a new session must be started. In the new session, the command prompt will be prepended with **(base)** which denotes that the session is in the base anaconda environment.

To create an environment that contains both numpy and scipy
```
(base) $ conda create -n py_env
(base) $ conda activate py_env
(py_env) $ conda install numpy
(py_env) $ conda install scipy
```

Alternatively, the creation of an environemnt and package installation can be completed with a single line.
```
(base) $ conda create -n py_env numpy scipy
```


## Submission Script Usage

Slurm does not source the *~/.bashrc* file in your batch job, so anaconda fails to be properly initialized within Slurm submission scripts. This can be resolved by directly sourcing the *.bashrc* file in your job script before running any conda commands:
```
source ~/.bashrc
```

Alternatively, the environment can be activated using *source* instead of *conda*.
```
source activate <environment>
```

Another way to resolve this is to add the following shebang to the top of a slurm job script:
```
#!/usr/bin/env bash -l
```

Another option would be to put the following commands before activating the conda environment:
```
module load anaconda3
CONDAPATH=`which conda`
eval "$(${CONDAPATH} shell.bash hook)"
```

Yet another alternative is to use the RISE anaconda installation which sets up the environment correctly so that **conda activate** can be used.


## Useful Anaconda Commands

| Command | Description |
| ---- | ---- |
| conda create –n <env_name> | Creates a conda environment by name |
| conda create –p <env_path> | Creates a conda environment by location |
| conda env list | Lists all conda environments |
| conda env remove –n <env_name> | Removes a conda environment by name |
| conda activate <env_name> | Activates a conda environment by name |
| conda list | While in an active environment, lists all packages |
| conda deactivate | Deactivates the active conda environment |
| conda install <package> | While in an active environment, installs a package |
| conda search <package> | Uses conda to search for a package |
| conda env export > env_name.yml | Exports active environment to a file |
| conda env create –f env_name.yml | Loads environment from a file |

