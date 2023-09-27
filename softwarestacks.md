
# Software Stacks

The software stack on Roar Collab (RC) provides many software modules to the entire user community. There are two software stacks available on RC.

- System software stack: contains software that is available to all users by default upon logging into the system without a need to load anything.
- Central software stack: contains software that is available to all users by default, but the software modules must be loaded in order to access them.


## Lmod

The central software stack uses [Lmod](https://lmod.readthedocs.io/en/latest/) to package the available software. Lmod is a useful tool for manager user software environments using environment modules that can be dynaically added or removed using module files. Lmod is hierarchical, so sub-modules can be nested under a module that is dependant upon. Lmod alters environment variables, most notably the `$PATH` variable, in order to make certain software packages reachable by the user environment.


### Useful Lmod Commands

| Command | Description |
| ---- | ---- |
| module avail | Lists all modules that are available to be loaded |
| module show \<module_name> | Shows the contents of a module |
| module spider \<module_name> | Searches the module space for a match |
| module load \<module_name> | Loads a module or multiple modules if given a space-delimited list of modules |
| module load \<module>/\<version> | Loads a module of a specific version |
| module unload \<module_name> | Unloads a module or multiple modules if given a space-delimited list of modules |
| module list | Lists all currently loaded modules |
| module purge | Unloads all currently loaded modules |
| module use \<path> | Adds an additional path to $MODULEPATH to expand module scope |
| module unuse \<path> | Removes a path from $MODULEPATH to restrict module scope |


## Central Software Stack

The central software stack is available to the user environment by default, so modules can be directly loaded with
```
$ module load <package>
```

To see the available software modules, use
```
$ module avail
```


