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


# Key Concepts
