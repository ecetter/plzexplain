
# Anaconda

[Anaconda](https://docs.anaconda.com/index.html) is a very useful package manager that is available on ICDS systems. Package managers simplify software package installation and manage dependency relationships while increasing both the repeatability and the portability of software. The user environment is modified by the package manager so the shell can access different software packages. Anaconda was originally created for Python, but it can package and distribute software for any language. It is usually very simple to create and manage new environments, install new packages, and import/export environments. Many packages are available for installation through Anaconda, and it enables retaining the environments in a silo to reduce cross-dependencies between different packages that may perturb environments.

Anaconda can be loaded from the software stack on Roar Collab (RC) with the following:
```
$ module load anaconda
```


## Installation Example

After loading the **anaconda** module, environments can be created and packages can be installed within those environments. When using the **anaconda** module for the first time on RC, the `conda init bash` command must be used to initialize anaconda, then a new session must be started for the change to take effect. In the new session, the command prompt will be prepended with `(base)` which denotes that the session is in the base anaconda environment.

To create an environment that contains both numpy and scipy, for example, run the following commands:
```
(base) $ conda create -n py_env
(base) $ conda activate py_env
(py_env) $ conda install numpy
(py_env) $ conda install scipy
```

Note that after the environment is entered, the leading item in the prompt changes to reflect the current environment.

Alternatively, the creation of an environemnt and package installation can be completed with a single line.
```
(base) $ conda create -n py_env numpy scipy
```

For more detailed information on usage, check out the [Anaconda documentation](https://docs.conda.io/projects/conda/en/latest/index.html).


## Submission Script Usage

Slurm does not automatically source the `~/.bashrc` file in your batch job, so anaconda fails to be properly initialized within Slurm job submission scripts. Fortunately, the **anaconda** module intitializes the software so that the `conda` command is automatically available within the Slurm job submission script. If using a different anaconda installation, this issue can be resolved by directly sourcing the `~/.bashrc` file in your job script before running any conda commands:
```
source ~/.bashrc
```

Alternatively, the environment can be activated using `source` instead of `conda`.
```
source activate <environment>
```

Another way to resolve this is to add the following shebang to the top of a slurm job script:
```
#!/usr/bin/env bash -l
```

Yet another option would be to put the following commands before activating the conda environment:
```
module load <custom anaconda module>
CONDAPATH=`which conda`
eval "$(${CONDAPATH} shell.bash hook)"
```

To reiterate, the **anaconda** module available on RC is configured such that the `conda` command is automatically available within a Slurm job submission sript. The above options are only necessary for other anaconda installations.


## Using Conda Environments in Interactive Apps


### Jupyter Server

To access a conda environment within a Jupyter Server session, the *ipykernel* package must be installed within the environment. To do so, enter the environment and run the following commands:
```
(base) $ conda activate <environment>
(<environment>) $ conda install -y ipykernel
(<environment>) $ ipython kernel install --user --name=<environment>
```

After the *ipykernel* package is successfully installed within this environment, a Jupyter Server session can be launched via the Portal. When submitting the form to launch the session, under the *Conda environment type* field, select the *Use custom text field* option from the dropdown menu. Then enter the following into the *Environment Setup* text field:
```
module load anaconda
```

After launching and entering the session, the environment is displayed in the kernel list.


### RStudio

To launch an RStudio Interactive App session, RStudio must have access to an installation of R. R can either be installed within the conda environment itself, or it can be loaded from the software stack. Typically, R will be installed by default when installing R packages within a conda environment; therefore, it is recommended when using conda environments within RStudio to simply utilize the environment's own R installation. To create an environment containing an R installation, run the following command:
```
(base) $ conda create -y -n <environment> r
```
Alternatively, R can simply be added to an existing environment by entering that environment and installing using the following command:
```
(<environment>) $ conda install r <plus any additional R packages>
```
R packages can installed directly via Anaconda within the environment as well. R packages available in Anaconda are usually named `r-<package name>` such as `r-plot3d`, `r-spatial`, or `r-ggplot`.

After R and any necessary R packages are installed within the environment, an RStudio session can be launched via the Portal. When submitting the form to launch the session, under the *Environment type* field, select the *Use custom text field* option from the dropdown menu. Then enter the following into the *Environment Setup* text field:
```
module load anaconda
conda activate <environment>
export CONDAENVLIB=~/.conda/envs/<environment>/lib
export LD_LIBRARY_PATH=$CONDAENVLIB:$LD_LIBRARY_PATH
```

Please note that the default location of conda environments is in `~/.conda/envs`, which is why the `CONDAENVLIB` variable is being set to `~/.conda/envs/<environment>/lib`. If the environment is instead installed a non-default location, then the `CONDAENVLIB` variable should be set accordingly. The two `export` commands in the block above are required because RStudio often has an issue loading some libraries while accessing the conda envrionment's R installation. Explicitly adding the conda environment's *lib* directory to the `LD_LIBRARY_PATH` variable seems to clear this issue up.


## Useful Anaconda Commands

| Command | Description |
| ---- | ---- |
| conda create –n \<env_name> | Creates a conda environment by name |
| conda create –p \<env_path> | Creates a conda environment by location |
| conda env list | Lists all conda environments |
| conda env remove –n \<env_name> | Removes a conda environment by name |
| conda activate \<env_name> | Activates a conda environment by name |
| conda list | While in an active environment, lists all packages |
| conda deactivate | Deactivates the active conda environment |
| conda install \<package> | While in an active environment, installs a package |
| conda search \<package> | Uses conda to search for a package |
| conda env export > env_name.yml | Exports active environment to a file |
| conda env create –f env_name.yml | Loads environment from a file |

