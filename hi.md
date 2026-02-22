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

> The four processes are created after the user presses ENTER, which means
> their creation happens during runtime.<br>
> However, the number of processes is ``conceptually`` decided earlier, at the moment
> the user writes the command.

Upon their creation, each process is assigned a Unique ID (UID) more commonly called **Rank**<br>
During the execution of the command above, 4 processes will be created, and
their numbering will start from 0:
* Process 0 → 1st process (rank 0)

* Process 1 → 2nd process (rank 1)

* Process 2 → 3rd process (rank 2)

* Process 3 → 4th process (rank 3)

<!------------------------------------------------------------------------------------------------------>


## What Does a Process Technically Do?

Each process executes a copy of the original `.c` code.

This means that **each process executes the entire code** written in the original
program.

<img width="1536" height="1024" alt="ChatGPT Image Feb 22, 2026, 01_36_42 PM" src="https://github.com/user-attachments/assets/2d9a1713-4952-40c3-8d38-437f735a0e31" />


At this point, a natural question arises:

> If each process executes the **entire code**, how can we define parallel
> computation as a model where each process executes only a *part* of the code,
> as implied by the goal of parallelism discussed earlier?

In order for this to be possible, we make use of the **rank (UID)** of each
process. The rank is accessible inside the program as a special integer value
and allows us to control execution flow.

By checking the rank value, we can ensure that **specific sections of the code
are executed only by specific processes**. This is typically achieved by placing
parts of the code inside conditional statements (`if` statements) that evaluate
the process rank.

For example, assume that we are the **first process** in a program consisting
of five processes (our rank is `0`). If we encounter an `if` statement whose
code block is intended to be executed only by process `0`, then **we will execute
it**. Otherwise, if we are any other process (rank different from `0`), we will
**skip it**.

```c
// If the rank is 0, this block will be executed
if (curr_rank == 0)
{
    ...
}
else
{
    // Processes with ranks 1, 2, 3, 4 will execute this block
    ...
}
```

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

> As a result, it is the programmer’s responsibility to decide which process
> will handle which subproblem, using appropriate conditional statements
> (e.g., `if` statements).


<!------------------------------------------------------------------------------------------------------>

## Rank
Οταν κανουμε execute ενα προγραμμα με mpiexec, we specify the number of processes we want the OS/MPI to create. 

Upon their creation, each process is assigned a Unique ID (UID) more commonly called **Rank**
For example, lets assume the following command: 

```bash
mpiexec -np 4 ./the_program
```

Κατα την εκτελεση της εντολης θα δημιουργηθουν 4 διεργασιες, οπου η αριθμηση τους αρχιζει απο το 0
* Process 0 => 1η διεργασια
* Process 1 => 2η διεργασια
* Process 2 => 3η διεργασια
* Process 3 => 4η διεργφασια




<!------------------------------------------------------------------------------------------------------>


## Intra-Communicators

A **communicator** is a special object that allows processes (that belong to it)
to **communicate with one another**.  
Without communicators, processes would simply exist as isolated entities, unable
to exchange data or coordinate their actions.

In other words, communicators are what transform independent processes into a
**functional parallel program**.

> **Why Is Communication Important?**<br>
>
> Without communication, we could still divide a program so that each process
> computes **a part of the solution**. However, since **each process operates
> locally with its own private memory** (processes do *not* have direct access to
> each other’s data), the partial results must eventually be **shared**.
> 
> This sharing of results is only possible through **inter-process communication**.

### A Simple Example
Assume we have **two processes** that need to compute the sum:

$$
1 + 2 + 3 + 4
$$

We could assign:
- Process with `rank = 0` → computes `1 + 2 = 3`
- Process with `rank = 1` → computes `3 + 4 = 7`

At this point, each process has computed **only a partial result**.
In order to compute the **final sum**, Process `1` must **send** its result (`7`) to process `0`.
Only then can process `0` compute the final result: 

$$
3 + 7 = 10
$$

This exchange of data is **communication**, and it is essential.

Without a communicator:
- Processes would still execute code
- Each process would still compute its local result
- But **no process would be able to share its data**
- The program would never reach a meaningful final solution

For this reason, **every process must belong to a communicator** in order to be
functional and to fulfill the purpose of parallelism.

Parallel computation achieves its full potential **only when communication
between processes is possible**, and communicators are the mechanism that makes
this possible.


<!------------------------------------------------------------------------------------------------------>

## MPI_COMM_WORLD

`MPI_COMM_WORLD` stands for **MPI – Communicator – World**.

It is the **most important communicator** in an MPI program because, during the
creation of the processes (that is, when the user executes the program using
the `mpiexec` command), **all processes initially belong to this communicator**.

Because of this, processes are able to **communicate with one another from the
very beginning**, without the programmer having to explicitly create or configure
anything. The `MPI_COMM_WORLD` communicator is created **automatically**, and
all processes are **automatically placed inside it**.

In other words, `MPI_COMM_WORLD` is the **default communicator** of an MPI
program, and it includes **every process** that participates in the execution
from the very beginning.

For example, when the user presses **ENTER** after issuing the following command:

```bash
mpiexec -np 10 ./the_program
```

**10 processes** will be created (with ranks `0` through `9`), and **all of them
will automatically belong to the `MPI_COMM_WORLD` communicator.**

As a result, the processes will be able to **communicate with one another from
the very beginning**, without the programmer having to explicitly create or set
up any additional communication mechanism.

<img width="434" height="184" alt="ΣΚΑΤΑ" src="https://github.com/user-attachments/assets/2f7f349d-9d8b-4dad-a4ae-165ef9e85174" />

<!------------------------------------------------------------------------------------------------------>

### Is MPI_COMM_WORLD the only intra-communicator?

The short answer is **no**.

In an MPI program, we can create **as many communicators as we want**. However,
unlike `MPI_COMM_WORLD`, these additional communicators **must be explicitly
created by the programmer**. They are *not* created automatically at program
startup like `MPI_COMM_WORLD`.

The reason for this is simple:  
any communicator beyond `MPI_COMM_WORLD` represents a **design decision made by
the programmer**.

If a programmer chooses to create additional communicators, it usually means
that they want **fine-grained control** over:
- which processes are allowed to communicate with each other,
- and how the overall process layout is structured.

This kind of logical organization **cannot be inferred automatically** by the
system, nor should it be. It depends entirely on the **semantics of the problem**
being solved and the **intent of the program design**.

Therefore, while `MPI_COMM_WORLD` is provided automatically as a default
communicator, all other communicators are the **explicit responsibility of the
programmer**.

### Communicator Membership and Rank Re-Mapping

Let us assume that we create a new communicator called `comm1`, and we include
inside it **some** of the processes that originally belong to `MPI_COMM_WORLD`.


<img width="446" height="361" alt="image" src="https://github.com/user-attachments/assets/d403f29c-11ce-429a-9597-36bde425aeef" />

From the image above, we can observe two important things:

1. The selected processes now **belong to both** `MPI_COMM_WORLD` **and** `comm1`.
2. Those same processes **receive new ranks** inside `comm1`.

### 1) Processes Are Not “Moved” Between Communicators

A process is **not transferred** from one communicator to another, because a
process is not a “unique entity” relative to communicators.

Processes are technological objects created by the MPI runtime environment, and
the *same process* can participate in **multiple communicators** at the same
time. Each communicator simply defines a **communication context** in which a
subset of processes is allowed to communicate.

So, creating `comm1` does not “remove” processes from `MPI_COMM_WORLD`.  
It simply creates an additional communication space where those same processes
can communicate under a different organization.

---

### 2) Why Do Ranks Change Inside a New Communicator?

This happens because a process receives a **rank relative to the communicator**
it belongs to.

In other words, the “identity” of a process (its rank / UID) is **not globally
rooted** on the process itself across the whole MPI program. Instead, it is
defined **within the scope of a communicator**.

This is intentional, because the purpose of a rank is not to uniquely identify a
process across the entire system, but to distinguish processes **only with
respect to communication inside a specific communicator**.

That is why a process may have:
- one rank inside `MPI_COMM_WORLD`
- and a different rank inside `comm1`

Both ranks are “correct”, because they are valid only within their respective
communication context.

<!------------------------------------------------------------------------------------------------------>

## Groups

A **group** is a set of grouped processes *within* a communicator (either inside
`MPI_COMM_WORLD` or inside a communicator that we create).

From the available processes inside a communicator, we can create **multiple
groups**.

For example, in an MPI program where we have created **10 processes** in
`MPI_COMM_WORLD`, we can create two groups: one containing the first five
processes, and another containing the remaining five.

<img width="500" height="303" alt="image" src="https://github.com/user-attachments/assets/04582ccd-6c95-474a-8150-e69184c5e4b6" />

> We can also create a third group that contains the processes of the first
> group or of the second group.

Groups (and the processes inside them) are only useful when they belong to a
**communicator**. A group that does not belong to any communicator simply
contains grouped processes, which are practically **useless**, because there is
no communication context in which they can operate.

---


### How Do We Declare a Group?

In order to create a group, we must first declare variables that will hold
**special information** (not normal values) related to groups and their ranks.
Those variables are bascily our accsess to the group itself.

> Groups also have their own rank ordering inside a communicator.
> We can declare multiple groups, just like we do with normal variables.

<img width="337" height="28" alt="Picture" src="https://github.com/user-attachments/assets/11027897-d5c4-41ed-8a5b-5428a678e37e" />

What the group variables actually contain internally is complex and is handled
by the implementation (compiler / MPI runtime). They may store pointers, integer
values, internal structures, etc. Therefore, the programmer cannot know what
exactly these variables contain at any given moment.

Because of this, group management must be performed **entirely through MPI
functions**, and not by direct manipulation.

After that, the actual creation of the groups is done using the function `MPI_Group_incl`:

<img width="395" height="35" alt="Picture" src="https://github.com/user-attachments/assets/8bfb6520-1997-4780-99f6-84026d20dcbb" />

## Inter-Communicators

So far, we have discussed **communicators where all participating processes
belong to the same communication space** (for example `MPI_COMM_WORLD` or
user-defined communicators like `comm1`). These are called **intra-communicators**.

An **inter-communicator** is conceptually different.

An **inter-communicator** is a special type of communicator that allows
**communication between two distinct groups of processes**, rather than within
a single group.

In other words:
- In an **intra-communicator**, processes communicate *with processes in the same group*
- In an **inter-communicator**, processes communicate *with processes in another group*

---

### Why Do Inter-Communicators Exist?

There are cases where a parallel program is **naturally split into two separate
teams of processes**, each with a different role.

For example:
- One group may act as **workers**
- Another group may act as **controllers / coordinators**
- Or two independent simulations may need to **exchange data occasionally**

In such cases, we do **not** want all processes to freely communicate with
everyone else. Instead, we want **structured communication between groups**.

This is exactly what inter-communicators provide.






