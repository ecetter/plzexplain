
# R

R is a free software environment for statistical computing and graphics.


## Install R Packages

R manages some dependencies and versions through the CRAN-like repos. R packages can be installed from within the R console with the following command:
```
> install.packages( <package> )
```

After installation, packages can then be loaded using the following command in the R console:
```
> library( <package> )
```

It is recommended to review dependencies of any packages you would like to install because you may have to load additional software in your environment before launching the R console.


## R Package Installation Example

To install the ggplot2 R package, first search ggplot2 online to see if there are installation instructions. A quick search shows that ggplot2 is included in the tidyverse package and that the recommended installation instructions are the following:
```
# The easiest ways to get ggplot2 is to install the whole tidyverse:
> install.packages("tidyverse")

# Alternatively, install just ggplot2:
> install.packages("ggplot2")
```

Searching for install instructions will usually get you across the finish line!

Some R packages may require changes to the user environment before the package can be installed successfully within the R console. Typically, the user environment change is as simple as accessing a newer compiler version by loading a software module like *intel* with
```
$ module load intel
```

Sometimes, installing R packages may be a little more involved. To install the *units* R package, for example, an additional library must be downloaded and installed locally in order for the package to be installed properly. To install the *units* R package on Roar Collab (RC), perform the following commands in an interactive session on a compute node:
```
$ cd ~/scratch
$ wget https://downloads.unidata.ucar.edu/udunits/2.2.28/udunits-2.2.28.tar.gz
$ tar -xvf udunits-2.2.28.tar.gz
$ cd udunits-2.2.28
$ ./configure prefix=$HOME/.local
$ make
$ make install
$ export UDUNITS2_INCLUDE=$HOME/.local/include
$ export UDUNITS2_LIBS=$HOME/.local/lib
$ export LD_LIBRARY_PATH=$HOME/.local/lib:$LD_LIBRARY_PATH
$ module use /storage/icds/RISE/sw8/modules/
$ module load r/4.2.1
$ R
> install.packages("units")
> library(units)
```


## R Versions on Roar

R users should make sure that the version of R remains consistent. RC has several R versions available, and when a package is installed in one version, it is not always accessible when operating in another version. Always check the R version and remain consistent!

## Parallel Processing in R





