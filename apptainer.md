
# Apptainer / Singularity

Apptainer is a container platform designed for HPC use cases and is available on Roar and RC. Containers (or images) can either be pulled directly from a container repository or can be built from a definition file. A definition file or recipe file contains everything
required to build a container. Building containers requires root privileges, so containers are built on your laptop and can be deployed on Roar and RC.


## Container Basics

A container is a standard unit of software with two modes:

1. Idle: When idle, a container is a file that stores everything
an application (or collection of applications) requires to run
(code, runtime, system tools, system libraries and settings).
2. Running: When running, a container is a Linux process running on top of the host machine kernel with a user environment defined by the contents of the container file, not by the host OS.

A container is an abstraction at the application layer. Multiple containers can run on the same machine and share the host kernel with other containers, each running as isolated processes.


## Useful Apptainer Commands

| Command | Description |
| ---- | ---- |
| apptainer build <container> <definition> | Builds a container from a definition file |
| apptainer shell <container> | Runs a shell within a container |
| apptainer exec <container> <command> | Runs a command within a container |
| apptainer run <container> | Runs a container where a runscript is defined |
| apptainer pull <resource>://<container> | Pulls a container from a container registry |
| apptainer build --sandbox <sbox> <container> | Builds a sandbox from a container |
| apptainer build <container> <sbox> | Builds a container from a sandbox |

