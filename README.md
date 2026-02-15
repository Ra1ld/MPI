# Overview

This repository contains a set of parallel programs developed in C using the MPI library.
The first program demonstrates the use of blocking point-to-point communication (MPI_Send / MPI_Recv),
while the second focuses on collective communication primitives.

The main goal was to ensure both correctness and deterministic behavior despite the inherently non-deterministic nature of parallel process scheduling and communication.

The programs are structured so that each process maintains clear and isolated ownership of its memory,
avoiding unnecessary resource allocation across processes.
Globally shared memory is used only when explicitly justified by the overall program design.

Each process allocates and manages only the memory it strictly requires,
with explicit allocation and deallocation, ensuring zero memory leaks across all processes.

This approach enables efficient execution while establishing a solid foundation for scalability,
reducing future maintenance overhead for developers and preserving runtime efficiency for ALL users as the codebase grows.

All MPI operations are validated through explicit checking of MPI return codes (e.g., MPI_SUCCESS),
ensuring correct behavior under all execution conditions.
The programs are written with a deep understanding of the MPI execution model,
respecting its communication semantics and parallel nature.
The overall design follows the mindset of a parallel execution engineer,
rather than treating MPI as a simple library layered on top of sequential C code.



## Prerequisites

The programs require an MPI implementation to be installed on our operating system
(e.g., OpenMPI or MPICH).

A Linux-based operating system is strongly recommended, as MPI environments
are most commonly deployed, tested, and used in Unix-like systems.
While MPI implementations are available on other platforms,
a Linux environment ensures the most predictable and consistent behavior.



## Installation Instructions

The following instructions assume a Unix-based operating system.
Since each Unix-based system may differ, installation commands can vary across distributions.
However, the general installation process remains the same.

1. Update the system package index  
2. Install the GCC compiler (skip this step if already installed)  
3. Install an MPI implementation  
4. Verify the installation  

> **Note:**  
> MPI implementations are actively maintained and widely used on modern Linux systems.
> However, compatibility issues may arise depending on the specific combination of
> operating system version and MPI distribution.
> 
> If any issues arise, please refer to the accompanying notes document for
> environment-specific observations and troubleshooting details.

### Example: Ubuntu-based systems

```bash
# 1. Update the system package index
sudo apt update

# 2. Install the GCC compiler and build tools (skip if already installed)
sudo apt install gcc g++ build-essential

# 3. Install an MPI implementation (OpenMPI)
sudo apt install openmpi-bin openmpi-common libopenmpi-dev

# 4. Verify the installation
mpicc --version
```



## Compilation

To compile a C program using MPI, execute the following command:

```bash
mpicc <program_path> -o <output_executable>
```

## Execution

To execute an MPI-compiled C program, two commonly used commands are available:

1. mpirun
2. mpiexec

In modern MPI implementations, these commands often provide equivalent functionality.
However, mpiexec is the standardized MPI launcher and is considered the more portable option across different systems and MPI distributions.

In most local Linux environments, both commands can be used interchangeably as they are typically backed by the same MPI runtime launcher ( prterun ).

```bash
mpirun -np <number_of_processes> ./program
```

OR

```bash
mpiexec -np <number_of_processes> ./program
```



