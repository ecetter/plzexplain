
# Apptainer / Singularity

Apptainer is a secure container platform designed for HPC use cases and is available on Roar and RC. Containers (or images) can either be pulled directly from a container repository or can be built from a definition file. A definition file or recipe file contains everything required to build a container. Building containers requires root privileges, so containers are built on your laptop and can be deployed on Roar and RC.

Software is continuously growing in complexity which can make managing the required user environment and wrangling dependent software an intractable problem. Containers address this issue by storing the software and all of it dependencies (including a minimal operating system) in a single image file, eliminating the need to install additional packages or alter the runtime environment. This makes the software both shareable and portable while the output becomes reproducible.


## Container Basics

A container is a standard unit of software with two modes:

1. Idle: When idle, a container is a file that stores everything an application (or collection of applications) requires to run (code, runtime, system tools, system libraries and settings).
2. Running: When running, a container is a Linux process running on top of the host machine kernel with a user environment defined by the contents of the container file, not by the host OS.

A container is an abstraction at the application layer. Multiple containers can run on the same machine and share the host kernel with other containers, each running as isolated processes.

In a Slurm submission script, a container can be called serially using the following run line:
```
apptainer run <container> <args>
```

To use a container in parallel with MPI, the MPI library within the container must be compatible with the MPI implementation on the system, meaning that the MPI version on the system must generally be newer than the MPI version within the container. More details on using MPI with containers can be found on Apptainer's [Apptainer and MPI Applications](https://apptainer.org/docs/user/1.0/mpi.html) page. In a Slurm submission script, a container with MPI can be called using
```
srun apptainer exec <container> <command> <args>
```


## Container Benefits

Containers change the user space into a swappable component.
- Flexibility: Bring your own environment (BYOE) and bring your own software (BYOS)
- Reproducibility: Complete control over software versions
- Portability: Run a container on your laptop or on HPC systems
- Performance: Similar performance characteristics as native applications
- Compatibility: Open standard that is supported on all major Linux distributions


## Container Registries

Container images can be made publicly available, and containers for many use cases can be found at the following container registries:

- [Docker Hub](https://hub.docker.com/)
- [Singularity Hub](https://singularityhub.github.io/singularityhub-docs/)
- [Singularity Cloud Library](https://cloud.sylabs.io/library)
- [NVIDIA GPU Cloud](https://ngc.nvidia.com/catalog/containers)
- [Quay.io](https://quay.io/)
- [BioContainers](https://biocontainers.pro/)


## Useful Apptainer Commands

| Command | Description |
| ---- | ---- |
| apptainer build \<container> \<definition> | Builds a container from a definition file |
| apptainer shell \<container> | Runs a shell within a container |
| apptainer exec \<container> \<command> | Runs a command within a container |
| apptainer run \<container> | Runs a container where a runscript is defined |
| apptainer pull \<resource>://\<container> | Pulls a container from a container registry |
| apptainer build --sandbox \<sbox> \<container> | Builds a sandbox from a container |
| apptainer build \<container> \<sbox> | Builds a container from a sandbox |


## Building Container Images

Containers images can be made from scratch using a [definition file](https://apptainer.org/docs/user/latest/definition_files.html#definition-files), or recipe file, which is a text file that specifies the base image, the software to be installed, and other information. The documentation for the [apptainer build](https://apptainer.org/docs/user/main/cli/apptainer_build.html) command shows the full usage for the build command. Container images can also be bootstrapped from other images, found on Docker Hub for instance.



