# MPI Tutorial

## Processes

A task (the main problem to be solved), both in the real world and in technology,
can be divided into smaller solvable tasks (subproblems), so that ``multiple
participants`` can work on those subproblems, each participant being assigned
one of them.

In parallel computing, ``those participants`` are called **processes**.



## How Are Processes Created?

Processes are created at runtime by the MPI runtime environment and are specified
by the user through the `mpirun` or `mpiexec` command, by providing the number of
processes to be created.

For example:

```bash
mpiexec -np 4 ./the_program
```
> The above command will create 4 processes




## What a technicaly process does 
Εach process executes a copy of the original `.c` code.

This means that **each process executes the entire code** written in the original
program.
At this point, a natural question arises:

> If each process executes the **entire code**, how can we define parallel
> computation (as a model) where each process executes only a *part* of the code?

This is where **Rank** comes in.

## Rank
Οταν κανουμε execute ενα προγραμμα με mpiexec, we specify the number of processes we want the OS/MPI to create. 

Upon their creation, each process is assigned a Unique ID (UID) more commonly called **Rank**
For example, lets assume the following command: 


The following Command will create 4 processes starting from 0:
* Process 0
* Process 1
* Process 2 
* Process 3 






> As a result, it is the programmer’s responsibility to decide which process
> will handle which subproblem, using appropriate conditional statements
> (e.g., `if` statements).

