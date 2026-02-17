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
