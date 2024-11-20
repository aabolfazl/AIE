+++
date = '2024-11-20T14:33:19+03:00'
draft = false
title = 'Vortex: A Load-Balancing in C++ using IO_URING, Introduction'
tags = ["Vortex", "C++", "Linux Networking", "Concurrency", "Load Balancer"]
showTags= true
+++

## Introduction

Vortex is a learning-focused project that explores **Linux Networking** and **Concurrency** in C++. The goal of the project is to build a proficient **load-balancing web server** to understand the complexities of networking at the OS level. 

In this posts series, I’ll walk you through the architecture of Vortex, how it handles load balancing, and some of the key challenges I faced while implementing the solution in C++ and Linux Networking.

## Project Structure

After a lot of research and reading Envoy Proxy [Github](https://github.com/envoyproxy/envoy) source code and documentation I decided to implement a similar architecture to Envoy Proxy. also I was inspired by Nginx[Github](https://github.com/nginx/nginx) design to implement a more efficient and scalable system using their event loop model. 

The architecture consists of three main components that contain the core business logic:

#### 1. Listener Component
Responsible for accepting new connections based on configuration. It listens on specified ports and forwards accepted connections to the Balancer component.

#### 2. Balancer Component (Cluster of Backends)
Manages connections to upstream servers by:
- Maintaining a pool of connections to upstream servers
- Observing server status from the Health Checker
- Implementing routing logic based on configuration and server health
- Making decisions about upstream server selection

#### 3. Health Checker Component
Operates independently to:
- Monitor upstream server health based on configuration
- Maintain its own socket connections to upstream servers
- Continuously check upstream server status
- Notify Balancer of any status changes

Additionally, we have shared utility components that support the main components:

#### Shared Components
- EventLoop (epoll, io_uring)
- Buffer Pool (memory management)
- Configuration Manager (reading and updating configuration)
- Other utilities (logging, metrics, etc.)

### Architecture Benefits

This modular design offers several advantages:
1. **Flexible Listener-Balancer Mapping**: You can have multiple listeners sending traffic to a single balancer cluster, or a single listener distributing to multiple balancer clusters
2. **Independent Health Checking**: The health checker operates outside the cluster, allowing shared server monitoring across clusters
3. **Scalable Component Structure**: Each component can be scaled independently based on needs

## Conclusion

Vortex is a learning-focused project that explores the implementation of a Layer 4 load balancer using modern C++ and io_uring. By studying and drawing inspiration from production-grade systems like Envoy and Nginx, this project aims to understand the fundamentals of:
- Network programming at the transport layer (TCP/UDP)
- Modern C++ features and their practical applications
- Event-driven I/O using io_uring
- Process-per-core architecture patterns

While this is primarily a learning exercise and remains under development 🏗️, it serves as a practical way to understand how load balancers work at a fundamental level. Through this project, I'm gaining hands-on experience with systems programming concepts and modern Linux networking features.

Stay tuned for upcoming posts where I'll share my learning journey and dive into specific implementation details. You can find the source code on [GitHub](https://github.com/aabolfazl/Vortex).

Feel free to reach out if you have any questions or want to discuss load balancer implementations!

Thank you for reading!