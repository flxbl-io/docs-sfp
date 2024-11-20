---
icon: ring-diamond
---

# Introduction

|              | sfp-pro     | sfp (community) |
| ------------ | ----------- | --------------- |
| Availability | ✅           | ❌               |
| From         | December 24 |                 |



SFP Server is a backend service that provides essential functionality for managing Salesforce deployments, running background tasks, handling authentication, and storing deployment metadata. The server can be run as multiple isolated tenants, each serving different teams or purposes.

### Architecture Overview

The SFP Server consists of several key components:

* **API Server**: Core application server providing REST APIs
* **Worker Services**:
  * Critical Workers: High-priority tasks needing immediate processing
  * Normal Workers: Regular priority operations
  * Batch Workers: Background and non-time-critical tasks
* **Supporting Services**:
  * Redis: Message queue and task orchestration
  * Caddy: Reverse proxy and SSL termination
  * Document Store: Metadata and configuration storage



### Key Services and APIs

The SFP Server exposes several key APIs:

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

### Best Practices

1. **Tenant Isolation**:
   * Use separate tenants for different teams/purposes
   * Maintain independent configurations per tenant
2. **Worker Scaling**:
   * Scale critical workers based on time-sensitive operations
   * Adjust batch workers for background processing load
   * Monitor queue lengths to optimize worker counts
3. **Monitoring**:
   * Use status command for health checks
   * Monitor logs for specific services
   * Track task completion rates and failures
4. **Updates and Maintenance**:
   * Schedule updates during low-activity periods
   * Test changes in development tenant first
   * Keep regular configuration backups

> **Note**: For production deployments, ensure proper security configurations and monitoring are in place.

> **Warning**: The server requires specific environment configurations and prerequisites. Ensure all requirements are met before initialization.
