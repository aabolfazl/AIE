+++
date = '2024-11-21T00:24:46+03:00'
draft = false
title = 'Kdht: A Kademlia Distributed Hash Table in Rust - Introduction'
tags = ["kdht", "rust", "distributed_systems", "dht"]
showTags= true
+++


## Introduction

# DHT Kernelspace Routing(C) and Userspace Storage(Rust)

## Introduction

This project implements a **Distributed Hash Table (DHT)** using a split architecture: **routing logic** in the kernel space and **storage functionality** in the userspace. This design allows efficient packet routing directly in the kernel while leveraging Rust in userspace for safe, flexible data management.

## Concept

Using a **Kademlia-based DHT**, the kernel module manages node discovery, routing, and packet forwarding based on an XOR distance metric, allowing efficient data lookup across distributed nodes. The kernel handles the DHT’s routing independently, guiding requests through the network to locate data on other nodes when necessary.

If a request arrives for data stored locally, the kernel module calls on a Rust-based userspace application to manage the data storage and retrieval. This separation allows clients to seamlessly locate data within the DHT without implementing their own DHT logic.

## Key Components

1. **Kernel space (Routing)**: The kernel module intercepts packets, manages the DHT routing table, and uses XOR-based lookups to route requests across the network.
2. **Userspace (Storage)**: The Rust application in userspace handles data storage for locally-responsible keys, supporting the kernel's routing layer without interfering with routing logic.

## Benefits

- **High Performance**: Kernel-level routing minimizes latency in DHT lookups.
- **Modularity**: Decoupling routing and storage allows each layer to scale or improve independently.
- **Safety**: Rust in userspace ensures safe, concurrent data handling for reliable storage management.

This project provides a robust, modular DHT implementation with efficient routing and safe, manageable storage, suited for distributed data access without centralization.

# Roadmap for Implementing
This project will start with a full userspace implementation to establish and test core functionality. Once stable, the routing component will be transitioned to the kernel for improved performance.

### Phase 1: Userspace Implementation
- **Routing Module:** Implement Rust's core DHT routing logic, including node communication and efficient key-based lookups.
- **Storage Module:** Build a storage layer to manage data, integrated with the routing module.

### Phase 2: Kernel Transition for Routing
- **Kernel Routing Module:** Move the routing logic to a Linux kernel module, allowing for low-latency packet handling.
- **Kernel-Userspace Integration:** Connect the kernel routing module with the userspace storage layer to complete a hybrid DHT system.

Stay tuned for upcoming posts where I'll share my learning journey and dive into specific implementation details. You can find the source code on [GitHub](https://github.com/aabolfazl/kdht).

Feel free to reach out if you have any questions or want to discuss DHT implementations!

Thank you for reading!