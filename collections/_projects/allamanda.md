---
title: "Project Allamanda"
description: "Marketplace Web Applications using Finite-State Machines"
collection: projects
categories: [projects]
tags: [step-functions, workflow-orchestration, finite-state-machines]
last_modified_at: "2019-01-29"
layout: default

---

# {{ page.title }} 

{{page.description}}

---

Most transactional processes can be modelled as finite state machines (FSMs). A ride hailing service has a fixed pickup and drop-off, and a single point of cost calculation and payment transfer. Recent changes to consumer behavior has allowed customizations in cart dimensions (size, cost, etc.) and state transitions (pooling, fare splitting). 

States and transitions are less finite in service marketplace transactions. A state machine has multiple and differing sets of "completion", and has multiple participants in navigating a state machine, varying across one-in-a-lifetime (OLT), recurring, and primary-sub transaction state machines.

This project proposes a service fulfilment software architected as a strategy-patterned, distrbuted state machine, its implementations and methods of optimization.
