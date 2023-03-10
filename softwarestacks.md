
# Software Stacks

ICDS systems offer the entire user community availability to software at three software stack levels.

1. System software stack
2. Central software stack
3. RISE software stack

The system software stack contains software that is available to all users by default upon logging into the system without a need to load anything.

The central software stack contains software that is available to all user by default, but the software modules must be loaded in order to access them.

The RISE software stack is not available to users by default, but once the RISE software stack is loaded, users can access software modules normally.


## Lmod

Both the central and RISE software stacks use Lmod to package the available software. Lmod is a useful tool for manager user software environments using environment modules that can be dynaically added or removed using module files. Lmod is hierarchical, so sub-modules can be nested under a module that is dependant upon. Lmod alters environment variables, most notably the *$PATH* variable, in order to make certain software packages reachable by the user environment.

### Useful Lmod Commands

| Command | Description |
| ---- | ---- |
| module avail | Lists all modules that are available to be loaded |
| module show <module_name> | Shows the contents of a module |
| module spider <module_name> | Searches the module space for a match |
| module load <module_name> | Loads a module or multiple modules if given a space-delimited list of modules |
| module load <module>/<version> | Loads a module of a specific version |
| module unload <module_name> | Unloads a module or multiple modules if given a space-delimited list of modules |
| module list | Lists all currently loaded modules |
| module purge | Unloads all currently loaded modules |
| module use <path> | Adds an additional path to $MODULEPATH to expand module scope |
| module unuse <path> | Removes a path from $MODULEPATH to restrict module scope |


## Central Software Stack

The central software stack is available to the user environment by default, so modules can be directly loaded with
```
$ module load <package>
```

To see the available software modules, use
```
$ module avail
```


## RISE Software Stack

The supplemental RISE software stack is available for users to load and is more flexible and dynamic than the system and central software stacks. The RISE software stack is not available in the user environment by default, so it must be loaded into the *$MODULEPATH* variable. To load the RISE software stack on RC, use
```
$ module use /storage/icds/RISE/sw8/modules
```
On Roar, use
```
$ module use /gpfs/group/RISE/sw7/modules
```

It is recommended to add a *$RISESWST* variable to your bash configuration to store the location of the RISE software stack. In the *~/.bashrc* file on RC, add the following:
```
export $RISESWST='/storage/icds/RISE/sw8/modules'
```
On Roar, add the following to the *~/.bashrc* file:
```
export $RISESWST='/gpfs/group/RISE/sw7/modules'
```
