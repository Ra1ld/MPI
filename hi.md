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

The four processes are created after the user presses ENTER, which means
their creation happens during runtime.<br>
However, the number of processes is ``conceptually`` decided earlier, at the moment
the user writes the command.

<!------------------------------------------------------------------------------------------------------>


## What Does a Process Technically Do?

Each process executes a copy of the original `.c` code.

This means that **each process executes the entire code** written in the original
program.

At this point, a natural question arises:

> If each process executes the **entire code**, how can we define parallel
> computation as a model where each process executes only a *part* of the code,
> as implied by the goal of parallelism discussed earlier?

In order for this to be possible, **all processes participating in the solution
of a program must first have an identifying number**—in other words, a form of
*identity* that distinguishes each process from the others.

This allows us, when writing the original `.c` code, to decide **which parts of
the code will be executed by which process**.

> ⚠️ **Important**  
> The decision regarding *which process executes which part of the code* is
> **not** as simple as saying  
> “the first three lines of code will be executed by process 1, and the rest by
> process 2”.


Instead, the separation is based on a **conceptual partition of the problem**.
That is, a process (e.g., process 1) is responsible for solving a *specific part
of the problem*, and in order to do so, it may need to execute **different,
non-contiguous parts of the code** that are logically associated with that task.

These code segments may be scattered throughout the program, but they are
*conceptually bound* to the same process through the problem design.

This identifying number (or identity) of a process is called the **rank**.


<!------------------------------------------------------------------------------------------------------>

## Rank
Οταν κανουμε execute ενα προγραμμα με mpiexec, we specify the number of processes we want the OS/MPI to create. 

Upon their creation, each process is assigned a Unique ID (UID) more commonly called **Rank**
For example, lets assume the following command: 


Κατα την εκτελεση της εντολης θα δημιουργηθουν 4 διεργασιες, οπου η αριθμηση τους αρχιζει απο το 0
* Process 0 => 1η διεργασια
* Process 1 => 2η διεργασια
* Process 2 => 3η διεργασια
* Process 3 => 4η διεργφασια






> As a result, it is the programmer’s responsibility to decide which process
> will handle which subproblem, using appropriate conditional statements
> (e.g., `if` statements).

