---
title: Chapter 5 - Network Layer (Control Plane)
date: 2026-08-11
tags:
  - CSE4411
  - networking
  - chapter5
---

# Chapter 5: Network Layer - Control Plane

> [!info] Introduction
> The Network Layer is responsible for moving data from a sender to a receiver across the Internet. This chapter focuses on the **Control Plane**, which is the logic that decides *how* packets are routed.

## 1. Data Plane vs. Control Plane
To understand the Control Plane, it helps to distinguish it from the Data Plane:

- **Data Plane (The Muscle):** This is a local, per-router function. When a packet arrives at a router's input link, the data plane is responsible for forwarding that packet to the correct output link. 
  - *Analogy:* A mail sorting worker who looks at a zip code and throws the letter into the corresponding bin.
- **Control Plane (The Brain):** This is network-wide logic. It determines the actual end-to-end path a packet will take from its source to its destination.
  - *Analogy:* The central routing system that plans the mail truck routes across the country.

## 2. Two Approaches to the Control Plane
How do routers know which way to send packets? There are two main approaches to structuring the control plane:

1. **Per-Router Control (Traditional):** 
   - Every single router has a routing algorithm built into it. 
   - The routers all talk to each other, sharing information to build a map of the network and compute their own individual forwarding tables.
2. **Logically Centralized Control (SDN - Software Defined Networking):** 
   - A separate, centralized remote server (a "controller") computes the best paths for the entire network.
   - It then directly installs the forwarding tables into all the routers. The routers simply blindly follow the controller's instructions.
