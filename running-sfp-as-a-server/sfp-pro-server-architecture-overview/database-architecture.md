---
icon: ring-diamond
---

# Database Architecture

sfp pro server had  the following requirements that shaped the need for using Supabase as foundational system

1. Handle authentication and authorization seamlessly for both users and applications
2. Provide real-time state management to replace our previous Git-based approach
3. Work equally well in both cloud and self-hosted environments
4. Scale independently for each organization's needs
5. Ensure complete data isolation between organizations

### Authentication Foundation

```mermaid
graph TD
    subgraph "Authentication Flow"
        SA[Supabase Auth]
        JWT[JWT Tokens]
        RLS[Row Level Security]
    end

    subgraph "Access Types"
        UI[User Interface]
        CI[CI/CD Systems]
        AP[Application Access]
    end

    UI --> SA
    CI --> SA
    AP --> SA

    SA --> JWT
    JWT --> RLS

    RLS --> Data[(Instance Data)]
```

Authentication in sfp pro server is built entirely on Supabase Auth, which provides several key advantages:

First, it offers built-in support for multiple authentication methods while maintaining a consistent security model. When users log in through OAuth providers (in FLXBL-managed instances) or through an organization's own authentication system (in self-hosted instances), Supabase Auth handles all the complexity of token management and session control.

Second, it provides a JWT-based authentication system that seamlessly integrates with both interactive users and automated systems. This means whether a request comes from a developer using the CLI, a CI/CD pipeline, or the Codev desktop application, the authentication flow remains consistent and secure.

### Instance Isolation

Each organization in sfp pro server receives its own dedicated Supabase instance. This architectural decision provides several benefits:

```mermaid
graph TD
    subgraph "FLXBL-Managed Environment"
        subgraph "Organization A Instance"
            A_Auth[Auth A]
            A_Data[Data A]
            A_RT[Realtime A]
        end
  
        subgraph "Organization B Instance"
            B_Auth[Auth B]
            B_Data[Data B]
            B_RT[Realtime B]
        end
    end

    subgraph "Global Services"
        OAuth[OAuth Handler]
    end

    OAuth -.-> A_Auth
    OAuth -.-> B_Auth

    A_Auth --> A_Data
    A_Auth --> A_RT
  
    B_Auth --> B_Data
    B_Auth --> B_RT
```

This isolation ensures that:

* Each organization's data remains completely separate
* Performance and scaling can be managed independently
* Security boundaries are enforced at the infrastructure level
* Organizations maintain control over their data governance

### Real-time State Management

One of the most significant improvements Supabase brings to sfp pro server is in state management. Previously, we stored state information in Git repositories, which led to several challenges:

* State updates required Git operations
* Real-time visibility was limited
* Concurrent updates were difficult to manage
* Performance was constrained by Git operations

Supabase's real-time capabilities transformed this approach:

```mermaid
sequenceDiagram
    participant Client
    participant Server
    participant Supabase
    participant Worker

    Client->>Server: Request Operation Status
    Server->>Supabase: Subscribe to Changes
  
    Worker->>Supabase: Update State
    Note over Supabase: Real-time Processing
    Supabase-->>Server: Instant Update
    Server-->>Client: Live Status
```

This real-time capability enables:

* Immediate visibility into operation status
* Live updates without polling
* Efficient resource state tracking
* Consistent state management across all components

### Self-hosting Capabilities

The ability to self-host Supabase instances was crucial for organizations that need to maintain their systems within their own infrastructure. Supabase's open-source nature and comprehensive deployment tooling make this possible while maintaining feature parity with cloud deployments.

When an organization chooses to self-host sfp pro server, they get:

* Complete control over their Supabase instance
* The ability to integrate with internal systems
* Custom backup and retention policies
* Direct access to their data and logs

