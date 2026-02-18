# MPI Tutorial

## What Is a Process?

A process is an entity created by the CPU in order to execute code.<br>
For the sake of simplicity, think of a Process as a single CPU Core that will handle the execution of a Code.

<!------------------------------------------------------------------------------------------------------>

## Processes in relation to the "real world"
Before discussing what a process actually does, let us first establish some
basic concepts to avoid confusion when reasoning about MPI.

A task (the main problem to be solved), in the real world, 
can be divided into smaller solvable tasks (subproblems), so that ``multiple
participants`` can work on those subproblems. 

* In the real world, ``those participants`` would be **humans**.
* In parallel computing, ``those participants`` are called **processes**.

In the real world, each ``HUMAN`` will be assigned one of those subproblems.<br>
In Parallel computing, each ``PROCESSS`` will be assigned one of thoose subproblems.

<!------------------------------------------------------------------------------------------------------>

## What's the Goal?

In the context of parallel programming, the ultimate goal is for each process to
handle a smaller task derived from the original problem, allowing the overall
problem to be solved faster than if a single CPU core attempted to solve
everything on its own.

For example:

> A subproblem in parallel computing could be as simple as dividing a basic
> summation of six numbers across three different processes (typically CPU
> cores).  
> Each process would add two numbers, so instead of waiting for a single CPU
> core to perform all additions sequentially, the additions are distributed
> across multiple cores, allowing all of them to compute their partial results
> **simultaneously**.


<!------------------------------------------------------------------------------------------------------>



## How Are Processes Created?

Processes are created at runtime by the ``MPI runtime environment`` after executing
the `mpirun` or `mpiexec` command. However, the number of processes to be created
is specified by the user **before** executing the command.

This means that the user decides how many processes will be created, while the
actual creation of those processes takes place only after the command is issued
and just before the program starts running (i.e., during runtime).

For example, the command below will create 4 processes:

```bash
mpiexec -np 4 ./the_program
```

``ΕΔΩ ΓΡΑΦΤΟ ΛΙΓΟ ΚΑΛΥΤΕΡΑ``
The following Command will create 4 processes starting from 0:
* Process 0
* Process 1
* Process 2 
* Process 3 

<!------------------------------------------------------------------------------------------------------>



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

