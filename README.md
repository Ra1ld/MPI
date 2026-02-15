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



# Prerequisites

The programs require an MPI implementation to be installed on our operating system
(e.g., OpenMPI or MPICH).

A Linux-based operating system is strongly recommended, as MPI environments
are most commonly deployed, tested, and used in Unix-like systems.
While MPI implementations are available on other platforms,
a Linux environment ensures the most predictable and consistent behavior.

For installation instructions, please refer to the end of this README.


## Compilation

To compile a C program using MPI, execute the following command:

```bash
mpicc <program_path> -o <output_executable>


