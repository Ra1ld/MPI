

# Example MPI Programs



## Overview

This repository contains a set of parallel programs developed in C using the MPI library,
demonstrating two communication paradigms:

- **Blocking point-to-point communication** (`MPI_Send` / `MPI_Recv`)
- **Collective communication primitives**

The primary goal was to ensure deterministic behavior, despite the inherently
non-deterministic nature of parallel process scheduling and communication,
while maintaining efficient execution.

The programs are structured so that each process maintains clear and isolated
ownership of its memory, avoiding unnecessary resource allocation across processes.

> Globally shared memory is used only when explicitly justified by the overall
> program design.

This approach enables efficient execution while establishing a solid foundation
for scalability, reducing future maintenance overhead for developers and
preserving runtime efficiency for **all users** as the codebase grows.

All MPI operations are validated through explicit checking of MPI return codes
(e.g., `MPI_SUCCESS`), ensuring correct behavior under all execution conditions.


The programs are written with a deep understanding of the MPI execution model,
respecting its communication semantics and parallel nature.
Despite MPI being particularly unforgiving with regard to memory management,
this implementation avoids common pitfalls through careful allocation,
deallocation (and a lot of discipline).

> **Design mindset:**  
> The overall design follows the mindset of a parallel execution engineer,
> rather than treating MPI as a simple library layered on top of sequential C code.



## Prerequisites

The programs require an MPI implementation to be installed on our operating system
(e.g., OpenMPI or MPICH).

A Linux-based operating system is strongly recommended, as MPI environments
are most commonly deployed, tested, and used in Unix-like systems.
While MPI implementations are available on other platforms,
a Linux environment ensures the most predictable and consistent behavior.

> It is assumed that the user is comfortable working in a terminal-based environment and has basic familiarity with command-line tools.

## MPI Installation Instructions

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
sudo apt install -y build-essential


# 3. Install an MPI implementation (OpenMPI)
sudo apt install -y openmpi-bin openmpi-common libopenmpi-dev
```

```bash
# 4. Verify if the C compiler wrapper is installed
mpicc --version
```
<img width="833" height="285" alt="image" src="https://github.com/user-attachments/assets/838bd759-0083-4ab6-80e0-0bc5d9aee914" />

```bash
#5. Verify if the MPI runner is installed
mpiexec --version
```
<img width="927" height="649" alt="image" src="https://github.com/user-attachments/assets/a8c5f337-e60c-4aaa-acf9-851ff82e04f2" />

If the commands above produce output similar to the examples shown, the MPI environment has been successfully installed.

It is strongly recommended to validate the installation by compiling and executing a simple MPI program.
The following sections describe the standard compilation and execution workflow.

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
> The ./ prefix assumes that the executable is located in the current working directory.

If the executable resides in a different location, the full path must be provided:

```bash
mpiexec -np <number_of_processes> <program_path>
```

## Problem Scope 

The scope of this project is the study and implementation of deterministic parallel computations over large discrete input domains, using message-passing paradigms and explicit process coordination

Formally, we consider a finite sequence of natural numbers:

$$
X = \{x_1, x_2, \dots, x_N\}, \quad x_i \in \mathbb{N}
$$

> The sequence length N is user-defined at runtime


distributed across a set of 𝑃 independent processes:


$$
\mathcal{P} = \{p_0, p_1, \dots, p_{P-1}\}
$$

> The number of processes P is specified at execution time via `mpiexec`

Each process \(p_k\) is assigned a disjoint subset \(X_k \subset X\), such that:

$$
\bigcup_{k=0}^{P-1} X_k = X
\quad \text{and} \quad
X_i \cap X_j = \emptyset \;\; \forall i \neq j
$$


The computational objective is to evaluate a set of aggregation functions over the global input domain, including:

* additive reductions(for blocking point-to-point):

$$
S = \sum_{i=1}^{N} x_i
$$

* multiplicative transformations (collective communication):

$$
F = \prod_{i=1}^{N} f(x_i)
$$


Rather than relying on shared-memory abstractions, the system adopts a pure message-passing model, where all global results are constructed exclusively through explicit inter-process communication.

## Computational Model

The programs follow a Single Program, Multiple Data (SPMD) execution model, implemented using MPI. Each process executes the same binary but operates on a different partition of the input space.

Two complementary communication strategies are explored:

* Blocking point-to-point communication
* Collective communication primitives



In both cases, the global computation can be abstracted as a parallel reduction tree:

$$
R = \mathcal{R}(r_0, r_1, \dots, r_{P-1})
$$

where each \(r_k\) is a locally computed result derived solely from \(X_k\),
and \(\mathcal{R}\) is a deterministic reduction operator.









The programs can be compiled and executed locally by following the steps described above.
They are intended for experimentation and inspection, allowing the user to observe and analyze their behavior under different execution scenarios.

Contributions, improvements, and discussions are welcome.
If you have suggestions or would like to extend the programs in any way, feel free to contribute.

## Additional Resources 

For troubleshooting notes and deeper explanations of MPI concepts, please refer to the accompanying documentation:

* NOTES.md — Environment-specific notes and troubleshooting
* MPI_TUTORIAL.md — Step-by-step explanation of MPI fundamentals and core technical concepts


## Screenshots 

### Menu 
<img width="292" height="236" alt="image" src="https://github.com/user-attachments/assets/fb9ff082-d145-4172-ae18-ca253fc5a5fa" />

### Blocking oper.C
<img width="312" height="462" alt="image" src="https://github.com/user-attachments/assets/79e49294-7c29-41d0-a04e-c1a5e5f16773" />

### Collective oper.C
<img width="413" height="756" alt="image" src="https://github.com/user-attachments/assets/61f9fd84-0099-40d1-abee-3d97ddef6dda" />


