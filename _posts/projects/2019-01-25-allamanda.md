---
title: "Project Allamanda"
description: "Distributed State Machines as a Web Application architecture"
categories: [projects]
tags: [step-functions, workflow-orchestration, finite-state-machines]
last_modified_at: "2019-01-29"

---

# {{ page.title }} 

{{page.description}}

---

Most transactional processes can be modelled as finite state machines (FSMs). A ride hailing service has a fixed pickup and drop-off, and a single point of cost calculation and payment transfer. Recent changes to consumer behavior has allowed customizations in cart dimensions (size, cost, etc.) and state transitions (pooling, fare splitting). 

States and transitions are less finite in service marketplace transactions. A state machine has multiple and differing sets of "completion", and has multiple participants in navigating a state machine, varying across one-in-a-lifetime (OLT), recurring, and primary-sub transaction state machines.

This project proposes a service fulfilment software architected as a strategy-patterned, distrbuted state machine, its implementations and methods of optimization.


## Automata Theory

Automata Theory

> Logic of computation with respect to simple machines, ... to understand how 
> machines compute functions and solve problems, ... what it means for a 
> function to be defined as computable



State Machines

> Mathematical model of computation. 
> A machine can have different states but at a given time is only fulfilling one of them

Finite vs Infinite State Machines
 
* e.g of Infinite SMs: Turing machine
* e.g of Finite SMs: Mealy machine, Moore machine

Mealy machine  output values are determined by both current state and current inputs vs Moore machine, output values are deteremined only by current state. Mealy machines have fewer states output on arcs is `n^2` than `n`. Moore machines outputs cange at clock edges, one cycle after. Mealy machines react faster to inputs.

Given variables Q set of states, O output, Σ input,  δ transition f(x) and λ output function
* Mealy  λ: Q x Σ -> O
* Moore λ: Q -> O 

E.g.
Mealy: watch with timer, vending machines, traffic light
Moore: bus routes (?) technically, 


### DAG vs FSMs

### Strategy Pattern 

## Cost Analysis



## References

1. [Automata Theory](https://cs.stanford.edu/people/eroberts/courses/soco/projects/2004-05/automata-theory/basics.html)
1. https://aws.amazon.com/step-functions/pricing/
1. [Turing Machine code architecture example](http://legoofdoom.blogspot.com/2009/01/software-architecture-of-turing-machine.html)
1. [Using finite state machines to design software](https://www.edn.com/Pdf/ViewPdf?contentItemId=4026990)
 

