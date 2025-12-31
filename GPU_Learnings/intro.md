# GPUs: An Introduction

A GPU (Graphics Processing Unit) is often described as a “parallel processor,” which is true in the same way calling the ocean “wet” is true. It’s accurate, but it hides the interesting parts.

This document is meant to build a *mental model* of what GPUs actually are, how they think, and why they exist - not as magical throughput machines, but as a very opinionated answer to a specific class of problems.

## Why GPUs Exist At All

CPUs are built to be excellent at:
- Low-latency execution
- Complex control flow
- Running *one* thread very well

GPUs were born from a different pain point: **doing the same math over a lot of data**.  
Pixels, vertices, fragments, rays, particles - the world of graphics is embarrassingly parallel.

The GPU’s core idea is simple:
> If the same instruction needs to be applied to thousands of data elements, stop pretending they’re different.

This single idea reshapes everything: the execution model, memory hierarchy, scheduling, and even how programmers are expected to think.

## Throughput Over Latency

A CPU asks: *“How fast can I finish this instruction?”*  
A GPU asks: *“How many instructions can I finish per second if I never stop?”*

GPUs willingly tolerate:
- High memory latency
- Simple cores
- Reduced control flexibility

In exchange, they gain:
- Massive parallelism
- Hardware scheduling of thousands of threads

Latency is hidden, not eliminated - usually by swapping in another group of threads while the current one waits.

## Threads That Aren’t Really Threads

GPU “threads” are not CPU threads with smaller egos lol.

They are:
- Lightweight
- Created in bulk
- Expected to run *the same instruction stream*

Threads are grouped (warps / wavefronts), and **lockstep execution** is the default.  
This means divergence - when threads in a group take different branches - is not free. It is tolerated, serialized, and quietly punished.

This single constraint explains *a shocking amount* of GPU behavior.


## Memory Is the Real Boss

Most GPU programs are not compute-bound. They are **memory-bound**.

GPUs compensate with:
- Extremely high bandwidth
- Explicit programmer-managed memories
- Coalescing rules that reward regular access patterns

If CPUs are about clever caching, GPUs are about **obedience**:
> Access memory the way the hardware wants, or suffer politely.

## GPUs Are Not General-Purpose (And That’s the Point)

Modern GPUs run neural networks, physics simulations, and databases - but they still carry the DNA of graphics.

They shine when:
- Work is uniform
- Data is plentiful
- Control flow is boring

They struggle when:
- Branches diverge wildly
- Work is irregular
- Tasks depend heavily on each other