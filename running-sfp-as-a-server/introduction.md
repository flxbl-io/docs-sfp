---
icon: ring-diamond
---

# Introduction

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | December 24 |                 |



sfp-pro-server is a backend service that provides essential functionality for managing Salesforce deployments, running background tasks, handling authentication, and storing deployment metadata. The server can be run as multiple isolated tenants, each serving different teams or purposes.

### Architecture Overview

The sfp-pro-server Server consists of several key components:

* **API Server**: Core application server providing REST APIs
* **Worker Services**:
  * Critical Workers: High-priority tasks needing immediate processing
  * Normal Workers: Regular priority operations
  * Batch Workers: Background and non-time-critical tasks
* **Supporting Services**:
  * Redis: Message queue and task orchestration
  * Caddy: Reverse proxy and SSL termination



### Key Services and APIs

The sfp-pro-server Server exposes several key APIs:

#### Authentication

* OAuth callback handling
* Application token management
* Salesforce org authentication management

#### Task Management

* Submit and manage background tasks
* Support for immediate, scheduled, and recurring tasks
* Priority-based task routing
* Task status monitoring and cancellation

#### Document Storage

* Key-value store for configuration
* Collection-based document storage
* Version-controlled document management

#### Logging

* Operation-based log aggregation
* Log retention and cleanup
