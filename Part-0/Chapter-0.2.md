# Chapter 0.2 — Processes and Threads

| Field                | Value                                                                         |
| -------------------- | ----------------------------------------------------------------------------- |
| **Part**             | Part 0 — Foundations                                                          |
| **Chapter**          | 0.2                                                                           |
| **Prerequisites**    | Chapter 0.1 — What is a Computer?                                             |
| **Difficulty**       | Foundational                                                                  |
| **Related Chapters** | 0.3 Memory, 0.5 Client–Server Architecture, Part I — The Journey of a Request |

---

# From Instructions to Execution

In the previous chapter, we looked at a computer in its simplest form—a machine capable of following instructions. Everything we interact with on a computer, whether it's a browser, a text editor, a database, or a game, is ultimately just a carefully organized collection of those instructions.

That naturally leads to another question.

**If software is simply a collection of instructions, where do those instructions live, and how do they actually begin running?**

It may seem like an obvious question, but the answer introduces one of the most fundamental concepts in software engineering. Every application you will ever build, deploy, or debug—whether it is a small command-line utility or a globally distributed system—relies on the mechanism we explore in this chapter.

Before we talk about processes or threads, however, we first need to understand something even more fundamental.

## What is a Program?

Imagine you've just downloaded Visual Studio Code onto your laptop.

The download completes, an icon appears on your desktop, and the application is now installed. Even though you've never opened it, something now exists on your computer that didn't exist a few minutes ago.

But what exactly was created?

Certainly not a running application. There are no windows on your screen, no memory allocated to VS Code, and no CPU actively executing its instructions. Instead, your storage device now contains a collection of files. Among these files are executable instructions, configuration files, icons, fonts, themes, and many other resources required for the application to function.

Together, these files make up what we call a **program**.

A program is simply software at rest. It is passive, waiting quietly on a storage device until someone—or something—asks the operating system to execute it. Until that moment arrives, it performs no work. It consumes no CPU cycles, allocates no memory, and interacts with nothing around it. It simply exists as a collection of instructions, ready to be brought to life.

This distinction is subtle, but it is one of the most important ideas in this chapter.

> *A program does not run. A program is run.*

---

## A Small Experiment

Take a moment and look around your computer.

You probably have applications like Chrome, Spotify, Slack, Docker Desktop, or VS Code installed. Even when your computer is turned off, those applications continue to exist. They occupy space on your SSD. You can copy them to another computer, delete them entirely, or back them up onto an external drive.

Despite existing physically on your storage device, they are incapable of doing anything on their own. They cannot respond to a mouse click, download a web page, or play music. They are nothing more than instructions waiting for the operating system to execute them.

Now double-click Chrome.

Something remarkable happens.

The files stored on your SSD remain exactly where they were. Nothing about the program itself has changed. Instead, the operating system creates a completely new execution environment and begins reading the program's instructions one by one.

That execution environment is called a **process**.

---

## From Program to Process

At first glance, it might seem as though launching an application simply means "starting the program." In reality, the operating system performs considerably more work than that.

Before the first instruction of the program can execute, the operating system needs to answer several important questions.

* Where in memory should this application run?
* How much memory should it receive initially?
* Which files is it allowed to access?
* Which user started the application?
* What security permissions should it have?
* How will the CPU know when to execute it?
* How can it be prevented from interfering with other running applications?

Only after these questions have been answered does execution begin.

The operating system packages all of this information together into a self-contained execution environment.

> *A process is what the operating system creates so that the program can actually execute.*

This is an important shift in perspective.

The program remains safely stored on disk.

The process is temporary.

The process is what brings the program to life.

---

## One Program, Many Processes

One of the easiest misconceptions to develop at this point is assuming that every program always has exactly one process.

That isn't true.

In fact, the same program can have many independent processes running simultaneously.

Imagine you have a Python script called `generate_report.py`.

You open two terminals and execute the same command.

```bash
python generate_report.py
```

Although both commands execute exactly the same program, the operating system creates **two completely independent processes**.

Both processes read instructions from the same program stored on disk, but from that moment onward they live entirely separate lives.

Each process receives its own:

* Process ID (PID)
* Virtual memory space
* Stack
* Heap
* CPU scheduling information
* Open file descriptors
* Network connections
* Security context

If one process crashes, the other continues running. If one process consumes large amounts of memory, it does not suddenly steal memory from the other. Even though they originate from the same program, the operating system treats them as completely independent execution environments.

### But why would the same program need multiple processes?

At first, creating multiple processes from the same program may seem unnecessary. If the program already exists, why not simply let one process do all the work?

In some situations, one process is perfectly sufficient. A simple calculator application, for example, may only ever need one process.

As applications become more sophisticated, however, running everything inside a single process becomes limiting.

Consider Google Chrome. You may have multiple browser windows open at the same time, perhaps even using different user profiles. Although every window originates from the same Chrome program installed on your computer, each runs as one or more independent processes. This separation improves both stability and security. If one renderer crashes or an extension misbehaves, the entire browser does not necessarily crash with it.

Multiple processes are also useful when several independent executions of the same program are required. Imagine a video encoding application converting several videos simultaneously. Rather than waiting for one conversion to finish before starting another, the operating system can create multiple processes, allowing each conversion to progress independently.

The important thing to notice is that **the program itself is never duplicated**. All of these processes execute the same instructions stored on disk. What differs is the execution environment created by the operating system.

> *One program can give rise to many processes, each with its own independent execution environment.*

---

## Why Processes Exist

Imagine a computer without processes.

Chrome, VS Code, Slack, Spotify, Docker Desktop, and even the operating system itself would all execute inside one giant shared memory space.

Now imagine Chrome accidentally writes over a section of memory currently being used by VS Code.

Or suppose a bug in Spotify overwrites data belonging to the operating system itself.

Very quickly, your computer would become unstable. One faulty application could bring down every other application running on the machine.

Processes prevent exactly this scenario.

By giving every application its own isolated execution environment, the operating system ensures that software can fail independently. One application crashing does not automatically crash everything else.

Isolation is one of the defining responsibilities of modern operating systems, and the process is the mechanism through which that isolation is achieved.

---

## The Limitation of Processes

Processes solve the problem of isolation remarkably well.

But software rarely stays simple.

Imagine opening your browser and navigating to a modern web application. Even before you begin interacting with it, a surprising number of things are happening simultaneously.

The browser is downloading HTML, CSS, JavaScript, images, and fonts from different servers. It is parsing and rendering the page while executing JavaScript. It continues listening for keyboard input, mouse movements, and network responses, all while ensuring that scrolling and animations remain smooth.

None of these activities are dependent on one another. A slow image download should not freeze the entire browser. A delayed network request should not prevent you from scrolling through the page.

The browser needs to perform many pieces of work at the same time.

One possible solution would be to create a new process for every task.

Technically, this would work.

However, every new process requires its own execution environment, memory space, operating system bookkeeping, scheduling information, and security context. Creating and managing processes is comparatively expensive.

Processes are also intentionally isolated from one another. If two processes need to exchange information, they cannot simply access each other's memory. Instead, they must communicate through mechanisms such as pipes, sockets, shared memory, or message queues—collectively known as **Inter-Process Communication (IPC)**.

For work that naturally belongs to the same application, this level of isolation becomes unnecessary overhead.

Naturally, engineers began asking a different question.

> **What if we could have multiple paths of execution inside the same process?**

That question gave rise to one of the most important abstractions in modern operating systems.

**Threads.**

---

## Threads: Multiple Paths, One Process

A **thread** is an independent path of execution within a process. While a process provides an isolated environment for an application to run, threads allow that application to divide its work into smaller, concurrent units. Instead of every task waiting for the previous one to finish, multiple threads can make progress independently while still belonging to the same process.

The important distinction is that a thread is **not** an independent application. It does not receive its own isolated execution environment. Instead, it lives inside an existing process and shares almost everything that process already owns.

You can think of a process as a house. The operating system constructs the house, provides an address and all the necessary utilities, and keeps it separate from neighboring houses. Threads are the people living inside that house. They share the same rooms, furniture, and utilities, yet each person can work on something different at the same time.

> *The process owns the resources. The threads simply make use of them.*

### What Do Threads Share?

Since threads belong to the same process, they share most of the resources that the operating system has already allocated.

This includes:

* Virtual memory
* Open files
* Network connections
* Security permissions
* Dynamically allocated objects
* Application state

However, every thread maintains its own execution stack, CPU register values, and program counter. This allows different threads to execute different parts of the program simultaneously while operating over the same shared data.

### Why Sharing Is Both a Strength and a Weakness

Sharing resources makes communication between threads extremely efficient. If one thread downloads product information from a server, another thread responsible for rendering the user interface can immediately use that data without involving the operating system.

Every engineering decision, however, comes with a trade-off.

Imagine two threads updating the same bank account balance at exactly the same time. One thread processes a salary deposit while another processes an online purchase. If both read and update the balance simultaneously, one update may unintentionally overwrite the other.

The problem is not that threads are flawed.

The problem is that **shared resources require coordination**.

As soon as multiple execution paths begin accessing the same data, software must answer an entirely new set of questions.

Who gets to modify the data first?

What happens if two threads arrive simultaneously?

How do we ensure the data remains consistent?

These questions introduce the field of **concurrency**, one of the most fascinating—and challenging—areas of software engineering. We will revisit it many times throughout this book.

### Threads in the Real World

You've already relied on threads countless times.

When you're downloading a file while continuing to scroll through a webpage, multiple threads are likely involved.

When your IDE indexes a project in the background while you continue writing code, threads are at work.

When Spotify continues playing music while you browse playlists, different parts of the application are progressing independently.

The goal is not always to make software faster.

Sometimes the goal is simply to prevent one piece of work from blocking everything else.

Responsiveness is just as important as raw performance.

### A Thought Before We Compare

At this point, it is easy to think that threads are simply "better" than processes.

They are lighter.

They are cheaper to create.

They communicate more easily.

So why not build every application using a single process and hundreds of threads?

Remember why processes were invented in the first place.

Isolation.

Processes protect applications from one another. Threads deliberately trade some of that isolation for efficiency.

Neither is universally better.

They solve different problems.

Modern software systems rely on both.

---

## A Comparison

We've now explored processes and threads independently. At this point, it's natural to compare them side by side. Rather than trying to memorize their differences, focus on the problem each one was designed to solve. Most of the distinctions below become obvious once that purpose is clear.

| Process                                      | Thread                                                |
| -------------------------------------------- | ----------------------------------------------------- |
| Represents an isolated execution environment | Represents an execution path within a process         |
| Owns its own virtual memory                  | Shares the process's virtual memory                   |
| Provides isolation from other processes      | Shares resources with sibling threads                 |
| More expensive to create                     | Much cheaper to create                                |
| Communication requires IPC                   | Communication happens naturally through shared memory |
| Failures are usually isolated                | Bugs can affect sibling threads                       |

---

## Bringing It All Together

Let's place what we've learned back into the bigger picture.

We began with a **program**—a passive collection of instructions stored on disk.

When the operating system decides to execute those instructions, it creates a **process**, providing everything the program needs to run safely and independently.

As applications become more sophisticated, a single execution path is no longer enough. The process therefore contains one or more **threads**, allowing different parts of the same application to make progress concurrently while sharing the same resources.

Notice how each concept exists because the previous one solved one problem while exposing another.

A program becomes a process.

A process contains one or more threads.

Together, these abstractions form the foundation upon which modern software systems are built.

---

## Questions to Ponder

* If every process receives its own memory, where does that memory come from?
* How does the operating system keep track of which memory belongs to which process?
* What happens when an application requests more memory than is physically available?
* Why do applications sometimes crash with an "Out of Memory" error?
* If multiple threads share memory, where is that shared memory actually stored?

The answers to these questions lead naturally to the next chapter.

**Memory.**
