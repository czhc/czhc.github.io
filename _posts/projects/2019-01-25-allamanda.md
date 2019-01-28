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

This project proposes a distributed, customizable software architecture pattern for desigining a software for service transactions, as well as an experimentative framework for optimization.

