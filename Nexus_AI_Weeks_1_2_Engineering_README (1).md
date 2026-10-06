# Nexus AI --- Weeks 1--2 Engineering Foundation

> **Purpose:** Build the secure, explainable backend foundation of Nexus
> AI while understanding **why every architectural and technology
> decision was made**.

Nexus AI is being developed as an AI infrastructure/control plane for
production AI applications and autonomous agents. The broader project
includes an AI/model gateway, model routing, token/cost optimization,
MCP, agents, workflows, evaluation, verification, evidence, policy,
observability, and a developer platform.

Weeks 1--2 are deliberately narrower: before Nexus can route models,
execute tools, run agents, or authorize actions, it needs a trustworthy
foundation for **HTTP APIs, configuration, persistence, identity,
authentication, authorization, and project isolation**.

------------------------------------------------------------------------

## 1. The Most Important Rule

Do **not** treat this README as a list of technologies to memorize.

For every decision in Nexus, you should eventually be able to answer:

1.  **What problem does it solve?**
2.  **Why does Nexus need it?**
3.  **What are its advantages?**
4.  **What are the realistic alternatives?**
5.  **Why did we choose this instead of those alternatives?**
6.  **What are the trade-offs and failure modes?**
7.  **When would we change the decision?**

The goal is not:

> "I used FastAPI because ChatGPT told me to."

The goal is:

> "I chose FastAPI because Nexus is an API-centric Python platform that
> will perform substantial I/O with databases, model providers and
> tools. FastAPI gives us strong request validation, OpenAPI support and
> async capabilities without imposing a large all-in-one framework. I
> understand the trade-offs against Django, Flask, Node/Nest and Go, and
> I know what conditions could make another choice better."

That is the standard for this project.

------------------------------------------------------------------------

# 2. Combined Goal of Weeks 1--2

## In simple language

Build the first secure version of Nexus where:

> **A real person can create an account, log in securely, create a
> project, and access only the resources they are allowed to access
> through a properly validated API backed by a real database.**

The complete flow should look like:

``` text
User / Client
     |
     | HTTP request
     v
FastAPI
     |
     | validate request
     v
Pydantic
     |
     | authenticate
     v
Identity / JWT
     |
     | authorize
     v
Authorization / Ownership
     |
     v
Service Layer
     |
     v
SQLAlchemy
     |
     v
PostgreSQL
```

Supporting components:

``` text
Environment Configuration
        |
        v
FastAPI application

Alembic
   |
   v
Database schema evolution

Argon2id
   |
   v
Password protection

Pytest
   |
   v
Automated verification
```

------------------------------------------------------------------------

# 3. What Weeks 1--2 Actually Teach You

By the end, you should understand:

### Backend fundamentals

-   HTTP
-   REST
-   JSON
-   HTTP methods
-   status codes
-   request/response lifecycle
-   headers
-   authentication headers
-   API versioning

### FastAPI

-   application creation
-   routers
-   path/query/body parameters
-   dependencies
-   validation
-   error handling
-   OpenAPI
-   synchronous vs asynchronous endpoints

### Data

-   relational databases
-   tables
-   rows
-   columns
-   primary keys
-   foreign keys
-   indexes
-   constraints
-   transactions
-   connection pooling
-   database sessions

### SQLAlchemy

-   engine
-   connection
-   session
-   ORM models
-   relationships
-   queries
-   transactions
-   commit/rollback

### Database evolution

-   migrations
-   Alembic revisions
-   upgrade/downgrade
-   migration safety

### Security

-   authentication
-   authorization
-   password hashing
-   Argon2id
-   JWT
-   OAuth2 concepts
-   OIDC concepts
-   sessions
-   API keys
-   RBAC
-   ownership
-   tenant/project isolation

### Engineering

-   modular monolith
-   configuration management
-   testing
-   logging
-   request IDs
-   health checks
-   failure handling

------------------------------------------------------------------------

# 4. The Architecture We Are Starting With

``` text
                         ┌─────────────────────┐
                         │      Client         │
                         │ Browser / CLI / SDK  │
                         └──────────┬──────────┘
                                    │
                                  HTTP
                                    │
                                    v
                         ┌─────────────────────┐
                         │      FastAPI        │
                         │   API Boundary      │
                         └──────────┬──────────┘
                                    │
                             Validation
                                    │
                                    v
                         ┌─────────────────────┐
                         │      Pydantic       │
                         │   Data Contracts    │
                         └──────────┬──────────┘
                                    │
                          Authentication
                                    │
                                    v
                         ┌─────────────────────┐
                         │ Identity / Auth     │
                         │ Argon2 + JWT        │
                         └──────────┬──────────┘
                                    │
                           Authorization
                                    │
                                    v
                         ┌─────────────────────┐
                         │ Service / Domain    │
                         │      Logic          │
                         └──────────┬──────────┘
                                    │
                                    v
                         ┌─────────────────────┐
                         │     SQLAlchemy      │
                         │ ORM + DB access     │
                         └──────────┬──────────┘
                                    │
                                    v
                         ┌─────────────────────┐
                         │    PostgreSQL       │
                         │ Persistent State    │
                         └─────────────────────┘

                 Alembic ─────> Schema migrations
                 Pytest  ─────> Automated tests
                 Config  ─────> Environment settings
                 Logging ─────> Diagnostics / observability
```

This is a **modular monolith**.

It is one deployable backend, but its internal domains should remain
separated.

------------------------------------------------------------------------

# 5. Decision Framework

For every technology below, use this exact structure:

### 1. What problem does it solve?

What would be difficult or impossible without it?

### 2. Why does Nexus need it?

Connect the tool to an actual Nexus requirement.

### 3. Advantages

What does this choice give us?

### 4. Alternatives

What other technologies or designs could solve the problem?

### 5. Why did we choose this?

What trade-off are we deliberately accepting?

### 6. When would we change?

What future condition would make another option better?

------------------------------------------------------------------------

# 6. Programming Language --- Python

## 6.1 What problem does Python solve?

Python is the implementation language for the Nexus backend.

It gives us the language runtime, standard library and ecosystem needed
to build the platform.

But the real architectural question is:

> Why Python for an AI infrastructure platform?

## 6.2 Why does Nexus need Python?

Nexus is not a generic CRUD application.

Later it will contain:

-   model integrations
-   model evaluation
-   benchmarking
-   token optimization
-   agents
-   MCP
-   verification
-   experimentation
-   AI-assisted analysis
-   potentially ML components

Python has an exceptionally strong AI/ML ecosystem.

Therefore the same language can support:

``` text
Backend API
    +
AI integration
    +
Evaluation
    +
Experiments
    +
ML tooling
```

That reduces friction between the platform layer and the AI layer.

## 6.3 Advantages

### AI ecosystem

Python has a very large ecosystem for AI and ML.

### Backend ecosystem

FastAPI, Pydantic, SQLAlchemy and pytest form a mature stack.

### Development speed

Python generally allows us to express backend logic with less
boilerplate.

### Research friendliness

Benchmarking and experimentation are natural in Python.

### Hiring / readability

Python is widely understood and relatively approachable.

## 6.4 Alternatives

### TypeScript / Node.js

Possible stack:

``` text
TypeScript
Node.js
NestJS / Express
Prisma
PostgreSQL
```

Advantages: - excellent web ecosystem - one language across
frontend/backend - strong type system - good async model

Why not primary choice? Nexus is strongly AI-oriented, and Python gives
a more direct path to the AI/ML ecosystem.

When could TypeScript win? If Nexus became predominantly a web platform
with a large TypeScript ecosystem and little Python-specific AI
processing.

### Go

Advantages: - high performance - excellent concurrency - strong
infrastructure ecosystem - simple deployment - efficient memory use

Why not now? The primary challenge in early Nexus is learning and
implementing AI infrastructure logic, not maximizing raw backend
throughput.

When could Go win? A highly performance-sensitive gateway, proxy or
infrastructure component could eventually be implemented in Go.

### Java

Advantages: - mature enterprise ecosystem - excellent tooling - strong
type system - very common in large enterprises

Why not now? More ceremony and less direct alignment with the AI
experimentation side of Nexus.

## 6.5 Decision

**Choose Python for the main Nexus platform.**

Important principle:

> Choosing Python does not mean every future Nexus component must use
> Python.

A future high-performance component can be written in Go or Rust if
there is evidence that it needs it.

------------------------------------------------------------------------

# 7. FastAPI

## 7.1 What problem does FastAPI solve?

FastAPI provides the HTTP API layer.

For example:

``` text
POST /api/v1/auth/register
POST /api/v1/auth/login
GET  /api/v1/projects
POST /api/v1/projects
GET  /api/v1/projects/{id}
```

It translates HTTP requests into application logic and application
results into HTTP responses.

## 7.2 Why does Nexus need it?

Nexus is a platform.

Other software needs to communicate with it.

Eventually:

``` text
Web Application
CLI
Python SDK
TypeScript SDK
Agent
MCP integration
Other services
```

will call Nexus APIs.

Therefore the API boundary is a core part of Nexus.

## 7.3 Advantages

### API-first design

FastAPI is particularly comfortable for building APIs.

### Request validation

Works closely with Pydantic.

### OpenAPI

API contracts and interactive documentation can be generated.

### Async support

Useful because Nexus will eventually perform lots of I/O:

``` text
database
LLM provider
MCP server
tool
webhook
external service
```

### Dependency injection

Useful for:

-   database sessions
-   current user
-   authorization
-   shared services

### Python compatibility

Works naturally with our Python AI stack.

## 7.4 Alternatives

### Flask

Advantages: - minimal - mature - flexible - enormous ecosystem

Why not? We would assemble more of the API-oriented functionality
ourselves.

When could Flask win? A very small service where maximum minimalism is
preferred.

### Django

Advantages: - batteries included - ORM - authentication - admin - mature
ecosystem

Why not? Nexus does not need a large all-in-one web framework for its
core architecture. We want explicit modules and domain boundaries.

When could Django win? If Nexus became primarily a traditional web
application with substantial built-in admin/content requirements.

### Node.js + Express

Advantages: - huge web ecosystem - excellent async I/O - TypeScript
option

Why not? Would move the main platform away from Python's AI ecosystem.

### Node.js + NestJS

Advantages: - structured architecture - dependency injection -
TypeScript - enterprise-friendly

Why not? Good technology, but Python is more aligned with Nexus's
AI/research direction.

### Go + Gin/Fiber

Advantages: - high performance - low overhead - strong concurrency

Why not? Raw performance is not our Week 1 bottleneck.

## 7.5 Decision

Use:

``` text
Python + FastAPI + Pydantic
```

because it gives a strong API layer without forcing unnecessary
framework complexity.

------------------------------------------------------------------------

# 8. Pydantic

## 8.1 What problem does it solve?

It defines and validates the structure of data.

Example:

``` text
ProjectCreateRequest
    name: string
    description: optional string
```

If a request does not satisfy the contract, it should not reach business
logic.

## 8.2 Why does Nexus need it?

Nexus will accept many structured inputs:

``` text
ModelRequest
ToolCall
AgentAction
Policy
Workflow
Experiment
Project
User
```

The API boundary must reject malformed data early.

This becomes even more important when Nexus starts accepting tool
arguments and agent actions.

## 8.3 Advantages

-   explicit contracts
-   type validation
-   automatic API schema generation
-   readable models
-   consistent validation

## 8.4 Alternatives

### Manual validation

Possible, but repetitive and error-prone.

### Marshmallow

Mature serialization/validation option.

Why not? Pydantic integrates more naturally with FastAPI.

### Dataclasses

Good Python data structures, but not a complete API validation solution.

### TypeScript types

Excellent in a TypeScript application, but not relevant if Python is the
backend language.

## 8.5 Decision

Pydantic is chosen because the FastAPI + Python architecture benefits
from strong explicit data contracts.

------------------------------------------------------------------------

# 9. PostgreSQL

## 9.1 What problem does it solve?

Persistent structured storage.

Without a database:

``` text
server stops
    ↓
memory disappears
    ↓
Nexus loses state
```

Nexus needs durable state.

## 9.2 Why does Nexus need it?

Nexus has highly relational data:

``` text
User
  ↓
Project
  ↓
Agent
  ↓
Run
  ↓
Tool Call
  ↓
Evidence
```

Other relationships will include:

``` text
Project → Models
Project → API credentials
Agent → Policies
Run → Events
Policy → Evidence requirements
```

This is control-plane state.

Consistency and relationships matter.

## 9.3 Advantages

### Relational model

Excellent for connected entities.

### Transactions

Multiple related changes can be handled atomically where appropriate.

### Constraints

Examples:

``` text
email UNIQUE
project.owner_id FOREIGN KEY users.id
```

### Indexes

Important for efficient lookups.

### Mature ecosystem

Large production ecosystem.

### Extensibility

PostgreSQL supports additional data types and extensions, so we do not
need to decide every future storage requirement today.

## 9.4 Alternatives

### MongoDB

Document-oriented.

Advantages: - flexible documents - easy schema evolution - natural
nested data

Why not primary Nexus database? Nexus's control plane is strongly
relational.

When could MongoDB be useful? A subsystem with genuinely
document-oriented data could use it, but don't introduce it merely
because it is popular.

### MySQL

Excellent relational database.

Why not? It is a viable alternative. PostgreSQL is selected because of
its strong feature set and excellent fit for this project.

There is no claim that MySQL is incapable of handling Nexus.

### SQLite

Advantages: - extremely simple - zero database server - excellent
local/testing option

Why not production foundation? Nexus is intended to support concurrent
production workloads and a multi-user control plane.

When could SQLite be useful? Local development, prototypes, isolated
tests.

### DynamoDB

Advantages: - managed cloud NoSQL - high scalability - predictable
access patterns

Why not? Adds cloud-specific design constraints and is unnecessary for
the early Nexus foundation.

## 9.5 Decision

Use PostgreSQL as the primary durable relational database.

Important future rule:

> PostgreSQL does not have to store every byte of Nexus.

Later we can have:

``` text
PostgreSQL → control-plane state
Object storage → large artifacts
Redis → cache / ephemeral coordination
Specialized stores → only when justified
```

------------------------------------------------------------------------

# 10. SQLAlchemy

## 10.1 What problem does it solve?

SQLAlchemy is the database toolkit/ORM layer between Python and the
relational database.

It lets us work with database state through Python abstractions while
retaining access to SQL-oriented functionality.

## 10.2 Why does Nexus need it?

We do not want every API endpoint directly constructing raw database
operations.

A cleaner boundary is:

``` text
API
 ↓
Service
 ↓
SQLAlchemy
 ↓
PostgreSQL
```

This keeps database access more organized.

## 10.3 Advantages

### ORM

Database entities can be mapped to Python classes.

### Core

SQLAlchemy also has a lower-level SQL expression system.

### Transactions

Provides structured transaction/session handling.

### Connection management

Provides engine/pooling mechanisms.

### Flexibility

We are not forced to choose between "ORM only" and "raw SQL only."

## 10.4 Alternatives

### Raw SQL

Advantages: - maximum SQL control - easy to see exactly what database
executes

Why not everything? A large application becomes repetitive if every
operation manually handles mapping, transactions and database
interaction.

When should we use it? For specialized or performance-critical queries
where direct SQL is clearer.

### Django ORM

Excellent, but tied to Django's ecosystem.

### Prisma

Very good TypeScript ORM.

Why not? Nexus backend is Python.

### Tortoise ORM

Possible Python alternative.

Why not? SQLAlchemy has a particularly mature ecosystem and strong
Core + ORM model.

## 10.5 Decision

Use SQLAlchemy because Nexus needs a mature relational abstraction while
retaining the ability to drop closer to SQL when necessary.

------------------------------------------------------------------------

# 11. SQLAlchemy Engine, Session and Connection --- DO NOT CONFUSE THEM

This is an important Week 2 concept.

## Engine

Think:

> "The database connectivity infrastructure."

It manages database connections and related configuration.

## Connection

Think:

> "A connection through which database commands can execute."

## Session

Think:

> "The unit through which the application works with ORM objects and
> transactions."

A simplified mental model:

``` text
PostgreSQL
    ↑
Connections
    ↑
Engine
    ↑
Session
    ↑
Application logic
```

Do not memorize this as an exact implementation diagram. Learn what each
abstraction is responsible for.

------------------------------------------------------------------------

# 12. Alembic

## 12.1 What problem does it solve?

The database schema changes over time.

Version 1:

``` text
users
projects
```

Version 2:

``` text
users
projects
agents
```

Version 3:

``` text
users
projects
agents
runs
```

We need controlled schema evolution.

## 12.2 Why does Nexus need it?

Without migrations, developers can manually change databases
differently.

Then:

``` text
Developer A → database structure A
Developer B → database structure B
Production → structure C
```

This becomes dangerous.

Alembic gives schema changes a versioned history.

## 12.3 Advantages

-   migration history
-   reproducibility
-   version control
-   upgrade/downgrade commands
-   SQLAlchemy integration
-   autogeneration support with important caveats

## 12.4 Alternatives

### Manual SQL scripts

Could work for a small project.

Why not? Harder to manage consistently as schema complexity grows.

### Django migrations

Good if using Django.

### Flyway

Mature migration tool.

Why not? SQLAlchemy + Alembic is a natural Python stack for this
project.

## 12.5 Important warning

Do not blindly trust autogenerated migrations.

Always inspect them.

Schema changes can have destructive consequences.

Especially dangerous:

``` text
DROP COLUMN
DROP TABLE
change nullable → non-null
large table rewrite
data transformation
```

Schema migration and data migration are not always the same problem.

## 12.6 Decision

Alembic is chosen because it fits SQLAlchemy/PostgreSQL and provides
version-controlled schema evolution.

------------------------------------------------------------------------

# 13. Password Hashing --- Argon2id

## 13.1 What problem does it solve?

We need to store a password verifier without storing the actual
password.

Never:

``` text
password = "mypassword"
```

Instead:

``` text
password
   ↓
Argon2id
   ↓
password hash
   ↓
database
```

## 13.2 Why does Nexus need it?

If the user database is compromised, plaintext passwords would expose
users immediately.

Passwords should use a password-specific adaptive hashing algorithm.

## 13.3 Advantages

Argon2id is designed for password hashing and is memory-hard.

It is deliberately expensive compared with fast general-purpose hashes.

## 13.4 Alternatives

### bcrypt

Strong and widely used.

Could absolutely be a valid choice.

### scrypt

Another memory-hard password hashing option.

### PBKDF2

Useful especially where compliance requirements dictate it.

### SHA-256

NOT appropriate by itself for password storage.

Why?

``` text
SHA-256 → intentionally fast
password hashing → should resist massive guessing attempts
```

### MD5

Not appropriate.

## 13.5 Why Argon2id?

For a new system, Argon2id is a strong modern default.

The exact parameters should be selected and benchmarked according to the
target environment and current security guidance rather than blindly
copying numbers.

## 13.6 When would we choose bcrypt instead?

If:

-   an organization's standard mandates it
-   existing infrastructure already uses it
-   compatibility requirements make it preferable

This is an engineering decision, not a religious argument.

------------------------------------------------------------------------

# 14. Authentication vs Authorization

This distinction is mandatory for Nexus.

## Authentication

Question:

> **Who are you?**

Example:

``` text
This request belongs to user 42.
```

## Authorization

Question:

> **What are you allowed to do?**

Example:

``` text
User 42 owns Project A.
User 42 cannot access Project B.
```

Therefore:

``` text
Authentication ≠ Authorization
```

Nexus must keep them separate.

------------------------------------------------------------------------

# 15. JWT

## 15.1 What problem does it solve?

After a user authenticates, subsequent API requests need a way to carry
authentication information.

A token can be presented with requests:

``` http
Authorization: Bearer <access-token>
```

The server validates it and identifies the caller.

## 15.2 Why does Nexus need it?

Nexus is API-centric and will eventually have:

``` text
Browser
CLI
Python SDK
TypeScript SDK
Agents
Services
MCP clients
```

Token-based authentication can be convenient for non-browser API
clients.

## 15.3 Advantages

-   standard token ecosystem
-   works naturally with APIs
-   can carry claims
-   can be validated without necessarily looking up a server-side
    session for every request
-   convenient for SDK/CLI/service-style clients

## 15.4 Alternatives

### Server-side sessions

The server stores session state and the browser carries a session
cookie.

### OAuth 2.0

An authorization framework, especially relevant when delegating access.

### OpenID Connect

An identity layer built on OAuth 2.0.

### API keys

Useful for applications/services, not ideal as the primary human
authentication mechanism.

### mTLS / service credentials

Useful for service-to-service trust in some environments.

## 15.5 Why JWT for the initial implementation?

Because it makes the API authentication flow explicit and works
naturally with an API-first architecture.

But:

> JWT is not automatically better than sessions.

It is a trade-off.

## 15.6 JWT disadvantages

This is important.

### Revocation can be harder

If a token remains valid until expiration, simply changing server-side
state may not immediately invalidate it unless the architecture supports
revocation/introspection mechanisms.

### Token leakage is dangerous

Whoever possesses a valid bearer token may be able to use it.

### Claims can become stale

Authorization changes may not automatically invalidate an already-issued
token unless the system checks current authorization state or uses
short-lived tokens.

### More complexity is not automatically better

Do not put too much information into JWTs.

## 15.7 When would we reconsider?

If Nexus becomes heavily browser-centric, secure server-side sessions
may be preferable.

If enterprise identity becomes important, OIDC/SSO becomes important.

If service-to-service authentication dominates, service credentials,
OAuth-based service identity, mTLS or another mechanism may be
appropriate.

------------------------------------------------------------------------

# 16. Sessions

## 16.1 What problem do sessions solve?

A session lets the server maintain authenticated state.

Simplified:

``` text
User logs in
     ↓
Server creates session
     ↓
Browser receives session cookie
     ↓
Browser sends cookie
     ↓
Server identifies session
     ↓
Server identifies user
```

## 16.2 Why could sessions be good for Nexus?

Especially for the web dashboard.

Advantages:

-   natural browser model
-   secure HttpOnly/SameSite cookies
-   server-controlled session lifecycle
-   easy revocation
-   server can immediately change session state

## 16.3 Why didn't we choose sessions as the initial universal mechanism?

Because Nexus is not only a browser application.

It will eventually serve:

``` text
SDKs
CLI
agents
services
MCP clients
automation
```

Token-based API credentials can be more natural for those clients.

## 16.4 Important conclusion

We are NOT saying:

> Sessions are bad.

We are saying:

> Sessions and token-based authentication serve different client models.

Nexus could eventually support:

``` text
Browser
  → secure session cookie

CLI / SDK
  → access token / credential

Service / Agent
  → service identity / scoped credential
```

------------------------------------------------------------------------

# 17. Google Sign-In / OIDC

## 17.1 What problem does Google Sign-In solve?

It lets Google authenticate the user instead of Nexus managing the
user's password directly.

Conceptually:

``` text
User
 ↓
Google
 ↓
Google authenticates user
 ↓
Nexus receives identity information
 ↓
Nexus maps identity to a Nexus account
```

## 17.2 Why could it be useful?

Advantages:

-   users don't need another password
-   Google handles its own authentication security
-   convenient UX
-   supports an identity-provider model

## 17.3 Why not make Google the only login method?

Because Nexus is an infrastructure platform.

Different users may use:

``` text
Google
Microsoft
GitHub
Okta
other enterprise identity providers
```

A production architecture should not make the entire identity model
dependent on one provider.

## 17.4 Why not use it in Week 2 instead?

Because the learning objective is to understand:

``` text
credentials
password hashing
authentication
tokens
authorization
```

If we outsource all authentication to Google immediately, you may build
a working login while understanding less about the underlying concepts.

## 17.5 What should happen later?

Potential Nexus architecture:

``` text
                    Nexus Identity
                          |
            +-------------+-------------+
            |             |             |
      Email/password   Google OIDC   Enterprise OIDC
            |             |             |
            +-------------+-------------+
                          |
                    Nexus user
                          |
                    Authorization
```

This is more flexible.

------------------------------------------------------------------------

# 18. OAuth 2.0 vs OIDC --- Do Not Mix Them Up

## OAuth 2.0

Primarily an authorization framework.

Think:

> "Can this client access this resource?"

## OpenID Connect

An identity layer built on top of OAuth 2.0.

Think:

> "Who is this user?"

So:

``` text
OAuth 2.0 → authorization
OIDC      → authentication/identity
```

Google Sign-In commonly uses OIDC concepts.

------------------------------------------------------------------------

# 19. API Keys

## 19.1 What problem do they solve?

They provide credentials for applications or services.

Example:

``` text
Python application
      ↓
Nexus API key
      ↓
Nexus
```

## 19.2 Why will Nexus need them?

Later Nexus will be called by:

-   CI/CD
-   backend services
-   scripts
-   SDKs
-   agents
-   automation

Those are not necessarily human browser sessions.

## 19.3 Advantages

-   simple
-   easy to use
-   good for service integration
-   can be scoped
-   easy to rotate

## 19.4 Alternatives

-   OAuth access tokens
-   service accounts
-   workload identity
-   mTLS
-   signed requests

## 19.5 Why not use API keys for everything?

Because API keys are not a replacement for human identity.

We need to know:

``` text
Human identity
Service identity
Agent identity
```

and authorize them appropriately.

For Nexus, API keys should be treated as **credentials**, not as the
entire identity architecture.

------------------------------------------------------------------------

# 20. RBAC

## 20.1 What problem does RBAC solve?

Authentication tells us:

> "Who are you?"

RBAC helps answer:

> "What are you allowed to do?"

Example:

``` text
Admin
Editor
Viewer
```

## 20.2 Why does Nexus need it?

Eventually projects/workspaces will contain sensitive:

-   model credentials
-   prompts
-   evaluations
-   agent configurations
-   policies
-   evidence
-   production actions

Not every user should have unrestricted access.

## 20.3 Alternatives

### ACLs

Explicit permissions assigned to users/resources.

### ABAC

Attribute-Based Access Control.

Example:

``` text
user.department == "security"
AND
resource.environment == "staging"
```

### ReBAC

Relationship-Based Access Control.

Example:

``` text
user is owner of project
```

## 20.4 Why start with RBAC + ownership?

It is understandable and sufficient for the initial foundation.

But later Nexus may need:

``` text
RBAC
+
resource ownership
+
scoped permissions
+
policy rules
+
attributes
```

Do not prematurely build a massive authorization engine.

------------------------------------------------------------------------

# 21. Project Ownership and Tenant Isolation

This is one of the most important Week 2 concepts.

Suppose:

``` text
User A → Project A
User B → Project B
```

User A must NOT be able to:

``` text
GET /projects/project-B
```

just because they know its ID.

The server must check ownership/authorization.

Conceptually:

``` text
request
  ↓
authenticate user
  ↓
load project
  ↓
check project.owner_id == current_user.id
  ↓
allow / deny
```

This is not optional.

It is part of the security architecture.

------------------------------------------------------------------------

# 22. UUID vs Integer IDs

This is another design decision you should understand.

## Integer ID

``` text
1
2
3
4
```

Advantages: - simple - compact - efficient - human-readable

Disadvantages: - sequential IDs can expose resource counts - easier to
guess - less convenient for distributed ID generation in some
architectures

## UUID

``` text
3fa85f64-5717-4562-b3fc-2c963f66afa6
```

Advantages: - extremely low collision probability - harder to guess -
useful in distributed systems - doesn't expose simple sequential counts

Disadvantages: - larger - less human-friendly - indexes can be larger

## Nexus decision

UUIDs are a reasonable fit because Nexus is intended to become a
distributed AI platform with many resource types.

But don't claim:

> "UUID automatically makes authorization secure."

It does not.

Authorization must still be enforced.

------------------------------------------------------------------------

# 23. Modular Monolith

## 23.1 What problem does it solve?

We need an architecture that is organized without creating unnecessary
distributed complexity.

A bad monolith:

``` text
everything.py
```

A premature microservice architecture:

``` text
auth-service
project-service
model-service
routing-service
agent-service
policy-service
verification-service
...
```

with network calls everywhere.

A modular monolith:

``` text
backend
 ├── auth
 ├── projects
 ├── users
 ├── models
 ├── agents
 ├── policy
 └── verification
```

One deployment, clear internal boundaries.

## 23.2 Why Nexus needs it?

Nexus has many future domains.

We need boundaries now.

But we do not yet have enough scale to justify distributed deployment
everywhere.

## 23.3 Advantages

-   simpler local development
-   simpler deployment
-   easier debugging
-   fewer network failure modes
-   clear domain boundaries
-   future service extraction remains possible

## 23.4 Alternatives

### Giant monolith

Easy initially, but boundaries become messy.

### Microservices

Strong isolation and independent deployment, but introduces
distributed-system complexity.

### Serverless

Good for event-driven functions, but not the natural first architecture
for the core control plane.

## 23.5 Why modular monolith?

Because:

> **Complexity should be earned.**

We should introduce distributed infrastructure because a requirement
demands it, not because a diagram looks more impressive.

------------------------------------------------------------------------

# 24. Configuration and Environment Variables

## 24.1 Problem

Different environments require different settings:

``` text
development
testing
staging
production
```

Examples:

``` text
DATABASE_URL
JWT_SECRET
ENVIRONMENT
LOG_LEVEL
```

## 24.2 Why Nexus needs it?

Configuration should not be hardcoded into application logic.

Bad:

``` text
DATABASE_PASSWORD = "real-password"
```

Better:

``` text
DATABASE_URL = environment configuration
```

## 24.3 Alternatives

### Configuration files

Useful for non-secret configuration.

### Environment variables

Simple and deployment-friendly.

### Secret managers

Better for production secrets:

``` text
Vault
cloud secret managers
Kubernetes secrets integrations
```

## 24.4 Why start with environment variables?

Week 1--2 should establish the configuration boundary.

Production secret-management infrastructure can be introduced later.

The principle is:

> Do not hardcode secrets now, but do not build a giant
> secret-management platform before Nexus needs one.

------------------------------------------------------------------------

# 25. Dependency Injection

## What problem does it solve?

A route should not have to manually construct every dependency.

For example:

``` text
endpoint
  ↓
database session
current user
authorization service
```

FastAPI dependencies allow these concerns to be injected.

## Why Nexus needs it?

It helps separate:

``` text
API layer
authentication
database
services
```

and makes testing easier.

## Alternatives

### Global variables

Simple but dangerous and difficult to test.

### Manual dependency construction

Possible, but repetitive.

### Dependency injection frameworks

Possible, but additional complexity.

## Why FastAPI dependency injection?

It is already part of the framework and fits the API architecture
without introducing another container.

------------------------------------------------------------------------

# 26. HTTP and REST Fundamentals

Before implementing the endpoints, understand the protocol.

## GET

Read something.

``` text
GET /projects
GET /projects/{id}
```

## POST

Create something or initiate an operation.

``` text
POST /projects
POST /auth/login
```

## PUT

Replace/update a resource representation.

## PATCH

Partially update.

## DELETE

Delete.

------------------------------------------------------------------------

# 27. HTTP Status Codes

You should understand at least:

``` text
200 OK
201 Created
204 No Content

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Validation Error

429 Too Many Requests

500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

Important distinction:

### 401

Authentication missing/invalid.

Think:

> "I don't know who you are."

### 403

Authenticated but not permitted.

Think:

> "I know who you are, but you can't do this."

This distinction becomes extremely important for Nexus authorization.

------------------------------------------------------------------------

# 28. Error Handling

Do not let every endpoint invent its own error format.

Prefer a consistent API error model.

For example:

``` json
{
  "error": {
    "code": "PROJECT_NOT_FOUND",
    "message": "Project does not exist",
    "request_id": "..."
  }
}
```

Later Nexus can standardize errors across:

``` text
model gateway
MCP gateway
agents
policy
verification
```

------------------------------------------------------------------------

# 29. Request IDs

Every request should eventually have a request identifier.

Example:

``` text
request_id = req_123
```

Why?

If a user says:

> "My request failed."

you need to find:

``` text
request
 ↓
logs
 ↓
database operation
 ↓
downstream call
```

Later, request IDs become part of distributed tracing.

------------------------------------------------------------------------

# 30. Structured Logging

Bad:

``` text
something went wrong
```

Better:

``` json
{
  "request_id": "req_123",
  "user_id": "...",
  "route": "/projects",
  "status": 403,
  "event": "authorization_denied"
}
```

Structured logs are machine-readable.

This will become especially valuable when Nexus gains:

``` text
agents
tools
models
verification
policy
replay
```

------------------------------------------------------------------------

# 31. Health Check

Implement:

``` text
GET /health
```

It should tell us whether the service is alive.

Do not confuse:

### Liveness

> Is the application process alive?

### Readiness

> Is the application ready to serve traffic?

Later:

``` text
/health
/readiness
```

can have different meanings.

------------------------------------------------------------------------

# 32. Database Transactions

This is a critical concept.

Suppose Nexus needs to:

``` text
create project
record audit event
update some related state
```

If the first two succeed and the third fails, we may not want a
half-finished operation.

Transactions give us a unit of atomic work.

Conceptually:

``` text
BEGIN
  operation A
  operation B
  operation C
COMMIT
```

If something fails:

``` text
ROLLBACK
```

Do not assume every operation should be one giant transaction.
Transaction boundaries are design decisions.

------------------------------------------------------------------------

# 33. Connection Pooling

Opening a new database connection for every request is inefficient.

A pool keeps reusable database connections available.

Conceptually:

``` text
Application
   |
   +--> connection
   +--> connection
   +--> connection
   +--> connection
          |
       PostgreSQL
```

SQLAlchemy provides engine/connection infrastructure for this.

Later you must understand:

-   pool size
-   overflow
-   connection timeout
-   idle connections
-   database limits

Do not blindly increase pool size.

------------------------------------------------------------------------

# 34. Authentication Flow

Initial learning implementation:

``` text
                 REGISTER
                    |
                    v
              Validate input
                    |
                    v
             Check email
                    |
                    v
              Argon2id hash
                    |
                    v
             Store user
```

Login:

``` text
                 LOGIN
                    |
                    v
             Validate input
                    |
                    v
             Find user
                    |
                    v
       Verify Argon2id password
                    |
              +-----+-----+
              |           |
            FAIL         PASS
              |           |
           401 error      v
                     Issue access token
                           |
                           v
                        Client
```

Protected request:

``` text
Client
  |
  | Authorization: Bearer token
  v
FastAPI
  |
  v
Validate token
  |
  v
Identify user
  |
  v
Authorization
  |
  v
Business operation
```

------------------------------------------------------------------------

# 35. Authorization Flow

Suppose:

``` text
User A
Project A

User B
Project B
```

User A asks:

``` text
GET /projects/B
```

Flow:

``` text
Request
   ↓
Token valid?
   ↓ YES
User = A
   ↓
Project B exists?
   ↓ YES
Does A have permission?
   ↓ NO
403 Forbidden
```

This is the security boundary.

------------------------------------------------------------------------

# 36. Project CRUD

Week 2 should produce:

``` text
POST   /projects
GET    /projects
GET    /projects/{id}
PATCH  /projects/{id}
DELETE /projects/{id}
```

Every operation must respect authorization.

Do not only test:

> "Can I create a project?"

Test:

> "Can User A access User B's project?"

That is a much more meaningful security test.

------------------------------------------------------------------------

# 37. Testing Strategy

Testing should happen at multiple levels.

## Unit tests

Test individual functions.

Examples:

``` text
password hashing
token creation
authorization helper
validation logic
```

## API tests

Test HTTP behavior.

Examples:

``` text
POST /register
POST /login
GET /projects
```

## Integration tests

Test:

``` text
FastAPI
   ↓
SQLAlchemy
   ↓
PostgreSQL
```

## Security tests

Critical cases:

``` text
invalid password
expired token
missing token
malformed token
wrong project owner
cross-user access
duplicate email
```

------------------------------------------------------------------------

# 38. Failure Cases You MUST Test

Do not define Week 2 as complete just because the happy path works.

## Authentication failures

-   wrong password
-   nonexistent user
-   malformed token
-   expired token
-   missing token
-   disabled user

## Authorization failures

-   user accesses another user's project
-   user tries to modify another user's project
-   user tries to delete another user's project

## Database failures

-   database unavailable
-   connection timeout
-   transaction failure
-   constraint violation

## API failures

-   malformed JSON
-   missing required field
-   invalid field type
-   duplicate resource
-   resource not found

## Security failures

-   plaintext password accidentally logged
-   secrets committed to Git
-   token exposed in logs
-   unauthorized resource access

------------------------------------------------------------------------

# 39. What NOT to Do

## Do not

-   hardcode secrets
-   store plaintext passwords
-   trust client-supplied owner IDs
-   assume UUIDs provide authorization
-   return internal stack traces to clients
-   put business logic entirely inside route handlers
-   create microservices just for appearance
-   add Redis because "every backend needs Redis"
-   add Kafka because "Nexus is distributed"
-   add Kubernetes because it looks impressive
-   add Google login before understanding identity fundamentals
-   copy a giant AI-generated backend without understanding it

------------------------------------------------------------------------

# 40. Recommended Repository Structure

A reasonable Week 1--2 structure:

``` text
nexus-ai/
│
├── app/
│   ├── main.py
│   │
│   ├── api/
│   │   ├── deps.py
│   │   ├── router.py
│   │   └── v1/
│   │       ├── auth.py
│   │       └── projects.py
│   │
│   ├── core/
│   │   ├── config.py
│   │   ├── security.py
│   │   └── logging.py
│   │
│   ├── db/
│   │   ├── base.py
│   │   ├── session.py
│   │   └── models/
│   │
│   ├── models/
│   │   ├── user.py
│   │   └── project.py
│   │
│   ├── schemas/
│   │   ├── auth.py
│   │   └── project.py
│   │
│   ├── services/
│   │   ├── auth_service.py
│   │   └── project_service.py
│   │
│   └── repositories/
│       ├── user_repository.py
│       └── project_repository.py
│
├── alembic/
│   └── versions/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── api/
│
├── .env.example
├── .gitignore
├── alembic.ini
├── pyproject.toml
└── README.md
```

The exact structure can evolve. Do not treat folder structure as sacred.

The important part is **separation of responsibility**.

------------------------------------------------------------------------

# 41. Separation of Responsibility

Avoid this:

``` text
route
 ├── validate
 ├── authenticate
 ├── query database
 ├── perform business logic
 ├── send email
 ├── format response
 └── log everything
```

Prefer:

``` text
Router
  ↓
Schema validation
  ↓
Dependency/authentication
  ↓
Service
  ↓
Repository / DB
  ↓
Response schema
```

This makes the system easier to test and evolve.

------------------------------------------------------------------------

# 42. Week 1 Definition of Done

You are NOT done because FastAPI starts.

You are done when you understand and can demonstrate:

### API

-   FastAPI app
-   routing
-   request/response
-   HTTP methods
-   status codes
-   API versioning
-   validation
-   errors

### Configuration

-   environment configuration
-   no hardcoded secrets

### Reliability

-   health endpoint
-   request IDs
-   structured logging
-   consistent errors

### Architecture

-   modular structure
-   separation of API/service/database concerns

### Understanding

You can explain why:

``` text
Python
FastAPI
Pydantic
modular monolith
```

were chosen.

------------------------------------------------------------------------

# 43. Week 2 Definition of Done

You are done when:

### Database

-   PostgreSQL running
-   SQLAlchemy connected
-   Alembic migrations working
-   User table
-   Project table
-   primary keys
-   foreign keys
-   indexes where justified
-   constraints
-   transactions understood

### Authentication

-   registration
-   secure password hashing
-   login
-   access token
-   protected endpoints

### Authorization

-   current-user dependency
-   project ownership
-   cross-user access blocked
-   401 vs 403 understood

### Project management

-   create
-   list own projects
-   retrieve
-   update
-   delete

### Testing

-   unit tests
-   API tests
-   database integration tests
-   authentication failure tests
-   authorization failure tests

### Understanding

You can explain why:

``` text
PostgreSQL
SQLAlchemy
Alembic
Argon2id
JWT
RBAC
ownership checks
```

were chosen.

------------------------------------------------------------------------

# 44. The Five Questions --- Master Table

  --------------------------------------------------------------------------------------------
  Technology /   Problem          Why Nexus                  Main             Why We Choose It
  Decision                                                   Alternatives     
  -------------- ---------------- -------------------------- ---------------- ----------------
  Python         Backend          AI + backend ecosystem     TypeScript, Go,  Best fit for
                 implementation                              Java             AI-heavy
                                                                              platform

  FastAPI        HTTP API         API-first architecture     Django, Flask,   Lightweight,
                                                             NestJS, Go       validation,
                                                                              OpenAPI, async

  Pydantic       Data validation  Strong API/tool contracts  Manual,          Natural FastAPI
                                                             Marshmallow      integration

  PostgreSQL     Durable          Control-plane              MongoDB, MySQL,  Consistency +
                 relational state relationships              SQLite, DynamoDB relationships

  SQLAlchemy     DB abstraction   Python ↔ PostgreSQL        Raw SQL, Django  Mature +
                                                             ORM, Prisma      flexible
                                                                              ORM/Core

  Alembic        Schema evolution Versioned DB changes       SQL scripts,     Natural
                                                             Flyway           SQLAlchemy
                                                                              pairing

  Argon2id       Password         Secure password storage    bcrypt, scrypt,  Modern
                 protection                                  PBKDF2           memory-hard
                                                                              option

  JWT            API              API/SDK/service-friendly   Sessions, opaque Fits initial
                 authentication                              tokens           API-first
                 state                                                        foundation

  Sessions       Alternative auth Browser-friendly           JWT              Useful later for
                 state                                                        web UI

  Google/OIDC    External         Convenience + SSO          Password auth,   Good future
                 identity                                    other IdPs       addition, not
                                                                              sole identity

  API keys       Machine          SDK/CLI/service access     OAuth, service   Simple scoped
                 credentials                                 identity         integration
                                                                              credential

  RBAC           Permission       Multi-user projects        ACL, ABAC, ReBAC Understandable
                 control                                                      initial model

  Modular        Architecture     Many domains without       Microservices,   Earn complexity
  monolith                        distributed complexity     serverless       later

  Env config     Runtime          Different environments     Config files,    Simple
                 configuration                               secret managers  foundation;
                                                                              production can
                                                                              mature

  Pytest         Automated        Prevent regressions        unittest         Strong Python
                 testing                                                      testing
                                                                              ecosystem
  --------------------------------------------------------------------------------------------

------------------------------------------------------------------------

# 45. The Questions You Should Ask Yourself Before Every New Technology

Before adding anything to Nexus:

### Question 1

**What problem am I actually solving?**

### Question 2

**Is this problem currently real?**

### Question 3

**Can an existing component solve it?**

### Question 4

**What are the alternatives?**

### Question 5

**What complexity am I introducing?**

### Question 6

**How will I test it?**

### Question 7

**How will I observe it?**

### Question 8

**How will it fail?**

### Question 9

**How will I explain the decision in an interview?**

### Question 10

**What evidence would make me change my mind?**

That last question is particularly important.

Good engineers do not say:

> "PostgreSQL is always better."

They say:

> "Given our current requirements, PostgreSQL is the better trade-off.
> If the workload or requirements change, I would reevaluate."

------------------------------------------------------------------------

# 46. Interview Questions You MUST Be Able to Answer

## Basic

1.  What is an API?
2.  What is HTTP?
3.  Difference between GET and POST?
4.  What is REST?
5.  What is FastAPI?
6.  What is Pydantic?
7.  What is PostgreSQL?
8.  What is SQLAlchemy?
9.  What is an ORM?
10. What is a migration?

## Database

11. What is a primary key?
12. What is a foreign key?
13. Why use indexes?
14. What is a transaction?
15. What is a connection pool?
16. What is normalization?
17. When would you use JSON/JSONB?
18. Why PostgreSQL instead of MongoDB?
19. ORM vs raw SQL?
20. What happens if a transaction fails?

## Authentication

21. Authentication vs authorization?
22. How does registration work?
23. Why hash passwords?
24. Why not SHA-256?
25. What is Argon2id?
26. What is JWT?
27. What is inside a JWT?
28. Why use expiration?
29. What happens if a token is stolen?
30. JWT vs sessions?

## Identity

31. What is OAuth 2.0?
32. What is OpenID Connect?
33. Why Google Sign-In?
34. Why not Google Sign-In as the only authentication mechanism?
35. What is an API key?
36. When should an application use an API key?
37. Human identity vs service identity?

## Authorization

38. What is RBAC?
39. What is ownership-based authorization?
40. Why isn't knowing a UUID enough?
41. What is 401?
42. What is 403?
43. How do you prevent User A accessing User B's project?

## Architecture

44. Why modular monolith?
45. Why not microservices?
46. When would you split a service?
47. What belongs in a router?
48. What belongs in a service?
49. What belongs in the database layer?
50. Why separate these layers?

## Reliability

51. Why request IDs?
52. Why structured logs?
53. What is a health check?
54. Liveness vs readiness?
55. What happens if PostgreSQL goes down?
56. How should the API respond to unexpected exceptions?

------------------------------------------------------------------------

# 47. Difficult Interview Questions

These are the questions that distinguish understanding from
memorization.

## Q1. Why FastAPI instead of Django?

You should discuss:

-   API-first requirement
-   validation
-   OpenAPI
-   async
-   framework scope
-   ecosystem
-   trade-offs

## Q2. Why PostgreSQL instead of MongoDB?

You should discuss:

-   relational data
-   foreign keys
-   transactions
-   consistency
-   query patterns
-   schema flexibility
-   cases where MongoDB would actually be better

## Q3. Why SQLAlchemy instead of raw SQL?

You should discuss:

-   abstraction
-   productivity
-   relationships
-   transaction handling
-   maintainability
-   ability to use SQL when necessary

## Q4. Why JWT instead of sessions?

You should NOT say:

> JWT is more secure.

Instead discuss:

-   browser vs API clients
-   server-side state
-   revocation
-   token leakage
-   scalability
-   SDK/CLI/service clients

## Q5. Why not Google login?

Answer:

> Google Sign-In and JWT are not mutually exclusive. Google/OIDC handles
> external identity; JWT/session/access-token mechanisms handle
> authenticated access. For Week 2, we implement fundamental
> authentication ourselves so the identity/authentication/authorization
> concepts are understood. Later Nexus can support OIDC providers such
> as Google and enterprise identity systems.

## Q6. What happens if an attacker steals a JWT?

You should discuss:

-   bearer-token nature
-   short expiration
-   secure storage
-   HTTPS
-   refresh/revocation strategy
-   token scope
-   logging prevention
-   rotation where appropriate

## Q7. Why does UUID not solve authorization?

Because:

``` text
random ID ≠ permission
```

Authorization must explicitly check whether the caller has access.

------------------------------------------------------------------------

# 48. The Final Mental Model

At the end of Week 2, think about Nexus like this:

``` text
                    NEXUS
                      |
              ┌───────┴────────┐
              |                |
          Identity          Resources
              |                |
        Who are you?       What exists?
              |                |
      Authentication      Projects
              |            Users
       ┌──────┼──────┐     etc.
       |      |      |
   Password  OIDC  Service
       |      |    Identity
       └──────┼──────┘
              |
        Authorization
              |
       What may you do?
              |
        ┌─────┴─────┐
        |           |
       RBAC      Ownership
        |           |
        └─────┬─────┘
              |
         Business Logic
              |
           Database
              |
         PostgreSQL
```

This foundation will later become:

``` text
Identity
   ↓
Policy
   ↓
Model Gateway
   ↓
MCP Gateway
   ↓
Agent Runtime
   ↓
Workflow Engine
   ↓
Verification
   ↓
Evidence
   ↓
Authorization
   ↓
Execution
   ↓
Observability / Replay
```

So Weeks 1--2 are not "just authentication and CRUD."

They establish the **trust boundary and control-plane foundation** that
the rest of Nexus will depend on.

------------------------------------------------------------------------

# 49. Engineering Journal Requirement

Create:

``` text
NEXUS_ENGINEERING_JOURNAL.md
```

For every major decision, record:

``` markdown
## Decision: PostgreSQL

### Problem
What problem were we solving?

### Requirements
What did Nexus need?

### Alternatives
What alternatives did we consider?

### Why PostgreSQL?
Why did it win?

### Trade-offs
What are we giving up?

### Failure modes
How can this fail?

### How we test it
What tests prove it works?

### What would make us change?
What future evidence would invalidate this decision?

### What I learned
...

### Interview answer
...
```

This will become one of the most valuable files in the entire project.

------------------------------------------------------------------------

# 50. The AI-Assisted Development Rule

When using Claude/ChatGPT/Copilot to implement Nexus, do NOT say:

> "Build Week 2."

Instead work feature by feature.

Example:

``` text
Teach me how password hashing works.
Quiz me.
Ask me to design the flow.
Review my design.
Then help me implement it.
Explain the important code.
Give me tests.
Give me failure cases.
Ask me interview questions.
```

For each subsystem:

``` text
LEARN
  ↓
EXPLAIN BACK
  ↓
DESIGN
  ↓
IMPLEMENT
  ↓
TEST
  ↓
BREAK
  ↓
FIX
  ↓
MEASURE
  ↓
DOCUMENT
  ↓
INTERVIEW
```

Never:

``` text
PROMPT
  ↓
200 lines of AI code
  ↓
COPY
  ↓
RUN
  ↓
NEXT
```

------------------------------------------------------------------------

# 51. Week 1--2 Final Demo

You should eventually be able to demonstrate this without explaining
every line of code:

``` text
1. Start Nexus
2. Health check succeeds
3. Register User A
4. Register User B
5. Login as User A
6. Receive authentication credential
7. Create Project A
8. Login as User B
9. Create Project B
10. User A can access Project A
11. User A cannot access Project B
12. Invalid credentials fail
13. Missing authentication fails
14. Invalid input fails validation
15. Database state survives restart
16. Schema changes are handled through Alembic
17. Tests pass
18. Logs contain request IDs
```

The most important demonstration is:

``` text
User A
   ↓
tries to access
   ↓
User B's project
   ↓
DENIED
```

That proves you understand that authentication alone is not enough.

------------------------------------------------------------------------

# 52. Final Definition of Done

Weeks 1--2 are complete only when you can honestly say:

> "I can explain the full lifecycle of a Nexus API request from HTTP
> entry to database persistence and response. I understand why we
> selected Python, FastAPI, Pydantic, PostgreSQL, SQLAlchemy, Alembic,
> Argon2id and JWT. I understand the alternatives and their trade-offs.
> I understand authentication versus authorization, JWT versus sessions,
> Google/OIDC versus local authentication, and API keys versus human
> identity. I can explain project ownership and prevent cross-user
> access. I can test both successful and failure paths. I understand the
> major security risks and can explain what would make me reconsider an
> architectural decision."

If you cannot explain one of those, **that topic is not finished yet.**

------------------------------------------------------------------------

# 53. Official Documentation to Study

Use official documentation as the primary technical reference rather
than random tutorials.

-   FastAPI: https://fastapi.tiangolo.com/
-   Pydantic: https://docs.pydantic.dev/
-   PostgreSQL: https://www.postgresql.org/docs/
-   SQLAlchemy 2.x: https://docs.sqlalchemy.org/en/20/
-   Alembic: https://alembic.sqlalchemy.org/
-   OWASP Password Storage:
    https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html
-   OWASP Session Management:
    https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
-   OpenID Connect: https://openid.net/developers/how-connect-works/

------------------------------------------------------------------------

# 54. Nexus Decision Philosophy

The most important architectural rule coming out of Weeks 1--2 is:

> **Choose technology because of requirements and measurable trade-offs,
> not because it is fashionable.**

Therefore:

``` text
Need relational consistency?
        → PostgreSQL

Need API boundary?
        → FastAPI

Need explicit validation?
        → Pydantic

Need Python DB abstraction?
        → SQLAlchemy

Need schema evolution?
        → Alembic

Need secure password storage?
        → Argon2id

Need initial API authentication?
        → JWT/access-token approach

Need browser session behavior?
        → Sessions are a valid option

Need external identity?
        → OIDC

Need machine credentials?
        → API keys / service identity

Need permissions?
        → RBAC + ownership initially

Need more sophisticated policy?
        → Add it when Nexus requirements justify it
```

That is the engineering mindset we want to carry into every later Nexus
subsystem.

------------------------------------------------------------------------

## Current Nexus Week 1--2 Stack

``` text
Language
└── Python

API
└── FastAPI

Validation
└── Pydantic

Database
└── PostgreSQL

Database Toolkit
└── SQLAlchemy 2.x

Migrations
└── Alembic

Password Hashing
└── Argon2id

Initial API Authentication
└── JWT / access-token approach

Authorization
└── Ownership + initial RBAC

Architecture
└── Modular Monolith

Configuration
└── Environment-based configuration

Testing
└── pytest

Development
└── Git + GitHub
```

This is the **starting architecture**, not a promise that every
component will remain unchanged forever.

The ability to say **"I chose this, I understand what I gave up, and I
know what evidence would make me change it"** is itself one of the
engineering skills Nexus is meant to teach.
