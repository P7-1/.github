
# P7 Platform

### Building a more trusted way to get home services.

**A connected ecosystem for discovering, requesting, and managing home
services.**
:::

------------------------------------------------------------------------

## 🏠 Overview

**P7** is a home services marketplace designed to connect customers with
trusted service providers through a clear, convenient, and organized
digital experience.

The platform aims to make it easier to discover services, compare
providers, request assistance, agree on pricing, and follow a service
request from creation to completion. It is being developed with a focus
on **trust, usability, transparency, and reliable service delivery**.

This GitHub Organization contains the source code and technical
documentation for the P7 ecosystem. Each repository has a clearly
defined responsibility so that individual applications and services can
be developed, tested, maintained, and improved independently while
contributing to one connected product.

> **Our goal:** Build P7 incrementally, delivering complete, tested
> product features rather than isolated screens or disconnected APIs.

## 🎯 Purpose of This Organization

The P7 GitHub Organization is the central engineering workspace for the
project. It exists to:

-   **Separate responsibilities:** Keep the backend, mobile app,
    administration dashboard, public website, and documentation in
    dedicated repositories.
-   **Support independent development:** Allow each application or
    service to evolve without unnecessarily coupling it to the others.
-   **Maintain a shared product direction:** Use a central project board
    and consistent planning practices to coordinate work across
    repositories.
-   **Improve quality and maintainability:** Keep code organized,
    document technical decisions, and test features before considering
    them complete.
-   **Enable future collaboration:** Make it easier to onboard
    contributors and define clear ownership as the project grows.
-   **Build in vertical slices:** Complete a feature across backend,
    database, mobile integration, and testing before moving to the next
    major feature.

## 🧭 Organization Structure

P7 uses a **multi-repository architecture**. Each repository represents
a distinct responsibility within the product.

``` text
P7 Platform (GitHub Organization)
│
├── p7-backend         API, business logic, data access, and integrations
├── p7-mobile          Customer and service-provider mobile experience
├── p7-admin           Internal administration dashboard
├── p7-website         Public-facing website and marketing pages
├── p7-docs            Product, architecture, and development documentation
└── p7-shared-types    Shared TypeScript types and API contracts (when needed)
```

The repositories are separate, but they are **not separate products**.
They work together as parts of the P7 platform and are coordinated
through shared requirements, API contracts, documentation, and project
planning.

## 📦 Repository Guide

### 1. `p7-backend`

**Role:** The core API and business-logic layer of P7.

**Primary technology:** ASP.NET Core Web API, C#, Entity Framework Core,
and Microsoft SQL Server.

The backend provides the rules and services used by the client
applications. It is responsible for processing requests, validating
business operations, enforcing authorization, managing persisted data,
and exposing documented APIs.

**Expected responsibilities** - Authentication, account management, and
authorization. - Service categories and service information. - Customer
and service-provider profiles. - Provider services, availability, and
verification workflows. - Booking and service-request lifecycle. -
Pricing, offers, and negotiation rules. - Reviews, ratings, and other
marketplace workflows. - Notifications, payments, and external
integrations as they are introduced. - Database access, migrations,
validation, logging, and consistent error responses.

**Architecture direction** The backend is intended to follow Clean
Architecture principles, with clear separation between the API,
application use cases, domain model, persistence, and infrastructure
concerns.

**Repository boundary** The backend owns authoritative business rules
and persisted marketplace data. Frontend applications should call its
APIs rather than independently reimplementing business rules.

------------------------------------------------------------------------

### 2. `p7-mobile`

**Role:** The primary mobile application for customers and service
providers.

**Primary technology:** React Native, Expo, and TypeScript.

The mobile app is the main user-facing experience for discovering and
requesting home services. The initial product direction is **one
application with two modes**, rather than separate customer and provider
apps.

**Customer / Buying Mode** - Browse categories and available services. -
View service details and provider profiles. - Create a service request
and provide relevant details. - Share service location and preferred
timing. - Review prices or negotiate when applicable. - Follow booking
status and review completed services.

**Provider / Selling Mode** - Set up a provider profile and professional
details. - Manage offered services and availability. - Receive and
respond to service requests. - Review or negotiate proposed prices. -
Update job progress where supported. - View ratings and relevant account
information.

**Shared app responsibilities** - Authentication and account screens. -
Navigation and reusable mobile patterns. - Form validation, loading
states, empty states, and error handling. - Secure communication with
the backend API. - A consistent and accessible experience across
supported devices.

**Repository boundary** The mobile app owns mobile presentation and
client-side interaction. Business-critical decisions and final data
validation remain on the backend.

------------------------------------------------------------------------

### 3. `p7-admin`

**Role:** The internal administration interface for operating and
managing the platform.

**Primary technology:** Next.js, TypeScript, and Material UI (MUI).

The admin dashboard is intended to give authorized administrators the
tools needed to manage platform data and oversee marketplace operations.

**Expected responsibilities** - Administrative authentication and
access-controlled navigation. - Managing service categories and service
listings. - Reviewing and managing provider information and verification
status. - Viewing and managing customers and service requests where
authorized. - Supporting operational workflows and issue
investigation. - Displaying useful operational summaries and metrics as
those capabilities are implemented.

**Repository boundary** The dashboard is an administrative client, not a
separate source of business rules. It consumes authorized backend APIs,
and sensitive operations must be protected by backend authorization.

------------------------------------------------------------------------

### 4. `p7-website`

**Role:** The public-facing website and marketing presence for P7.

**Primary technology:** Next.js and TypeScript.

The website communicates what P7 does, who it serves, and how customers
or service providers can get involved. It is distinct from the
authenticated mobile experience and internal admin dashboard.

**Expected responsibilities** - Explain the P7 service and value
proposition. - Present supported service categories and key benefits. -
Provide contact, support, or registration entry points as appropriate. -
Publish public information and future marketing content. - Support
responsive layouts, accessibility, performance, and search-engine
discoverability.

**Repository boundary** The website focuses on public content and
acquisition. Private account operations and marketplace workflows should
be handled through the appropriate authenticated application and backend
APIs.

------------------------------------------------------------------------

### 5. `p7-docs`

**Role:** The central documentation repository for product and
engineering knowledge.

This repository preserves important decisions and gives contributors a
reliable place to understand what is being built, why it is being built,
and how the system is expected to work.

**Expected contents** - Product vision, goals, scope, and
requirements. - User journeys and user stories. - Architecture overview
and Architecture Decision Records (ADRs). - Domain concepts and business
rules. - API conventions and links to API specifications. - Database and
integration design notes. - Feature specifications and acceptance
criteria. - Sprint plans, implementation notes, and release
checklists. - Local development and contribution guides.

**Repository boundary** Documentation should describe the agreed
direction and current implementation accurately. Proposed or future
capabilities should be clearly distinguished from completed
functionality.

------------------------------------------------------------------------

### 6. `p7-shared-types` *(Optional --- introduce when justified)*

**Role:** Reusable TypeScript types and API contracts shared by frontend
repositories.

This repository may contain stable, framework-independent types that are
genuinely useful across the mobile app, admin dashboard, and website.

**Potential contents** - API request and response types. - Shared
identifiers and pagination contracts. - Common enums and status
representations. - Versioned contracts where cross-application
coordination requires them.

**Important boundary** This repository should not become a home for all
shared code by default. Do not place React components, platform-specific
UI, backend C# models, secrets, or business logic here. Introduce it
when real duplication or a clear contract-sharing need exists;
otherwise, keep the initial setup simpler.

## 🔗 How the Repositories Work Together

At a high level, the client applications communicate with the backend
through documented APIs:

``` text
                 ┌──────────────────┐
                 │     p7-mobile    │
                 │ Customer/Provider│
                 └────────┬─────────┘
                          │
                          │ HTTPS / API
                          ▼
┌─────────────────┐  ┌──────────────────┐  ┌─────────────────┐
│    p7-admin     │─▶│   p7-backend     │─▶│   SQL Server    │
│ Admin Dashboard │  │ Business Rules   │  │ Persistent Data │
└─────────────────┘  └────────┬─────────┘  └─────────────────┘
                              ▲
                              │
                    ┌─────────┴────────┐
                    │    p7-website    │
                    │ Public Features  │
                    └──────────────────┘

             p7-docs describes the ecosystem
```

This diagram represents the intended high-level relationship, not a
claim that every website feature will require backend access. Public
pages may be static, while interactive features can use the backend when
needed.

## 🧱 Engineering Principles

### 1. Complete features end to end

P7 follows a **vertical-slice workflow**. Work is organized around
user-visible capabilities rather than completing every backend task
first and postponing all mobile work until later.

A feature is considered complete only when the relevant parts are
integrated and verified:

-   Backend behavior and business rules.
-   Database schema and persistence, where required.
-   API contracts and authorization.
-   Mobile or web integration.
-   Validation, loading states, and error handling.
-   Appropriate automated tests and end-to-end verification.
-   Updated documentation.

### 2. Keep responsibilities explicit

Each repository should have a clear purpose. Avoid duplicating business
logic across applications or introducing cross-repository dependencies
without a concrete need.

### 3. Treat the backend as the source of truth

Client-side validation improves usability, but the backend must validate
inputs, enforce permissions, and protect business-critical operations.

### 4. Build security in from the beginning

-   Never commit passwords, API keys, signing keys, or production
    connection strings.
-   Use environment-specific configuration and appropriate secret
    storage.
-   Enforce authorization on protected backend operations.
-   Validate and sanitize incoming data.
-   Keep dependencies and access permissions under review.

### 5. Test before declaring completion

A screen rendering successfully or an endpoint returning `200 OK` does
not, by itself, prove that a feature works. Verify the complete user
journey and its expected data changes.

### 6. Prefer simplicity over premature complexity

Add infrastructure, packages, and services when there is a demonstrated
need. Keep early development understandable and maintainable while
preserving room to grow.

## 🗺️ Product Development Approach

The product is expected to evolve through incremental milestones. The
exact scope and order may change as requirements are validated.

1.  **Foundation:** Repository setup, backend foundation, configuration,
    logging, health checks, and development documentation.
2.  **Authentication & Onboarding:** Registration, sign-in, account
    verification, and account recovery.
3.  **Customer Profile:** Personal information and account management.
4.  **Service Discovery:** Categories, services, and service details.
5.  **Provider Discovery & Onboarding:** Provider profiles, offered
    services, and verification workflows.
6.  **Booking Lifecycle:** Service requests, provider responses, and
    booking status.
7.  **Pricing & Negotiation:** Indicative pricing for simple services
    and quote-based pricing for more complex work.
8.  **Tracking & Reviews:** Service progress, completion, and customer
    feedback.
9.  **Payments & Notifications:** Introduced after the relevant
    workflows and integration requirements are validated.
10. **Support & Operations:** Support workflows, administration
    capabilities, and operational improvements.

The guiding rule is to finish and test one major feature before starting
another major feature. Related tasks may span repositories, but they
should be coordinated as one feature in the central project board.

## 📋 Planning and Work Tracking

The GitHub Organization uses a central project board named **P7 Product
Development** to coordinate work across repositories.

Issues should be created in the repository responsible for the
implementation and then added to the central project. Cross-repository
features should have a clearly defined parent issue or tracking issue
with linked implementation tasks.

Recommended workflow:

`Backlog → Ready → In Progress → In Review → Testing → Done`

A feature should not be marked **Done** until its acceptance criteria
are met and its relevant end-to-end flow has been verified.

## 🤝 Contribution Workflow

The contribution process is intended to stay lightweight and consistent:

1.  Review the relevant requirement or issue.
2.  Create a focused branch associated with the issue.
3.  Implement the change within the repository's responsibility.
4.  Run relevant builds, checks, and tests.
5.  Open a pull request describing the change and how it was verified.
6.  Address review feedback and confirm required checks.
7.  Merge only when the change meets the agreed acceptance criteria.
8.  Update documentation when behavior, contracts, or architecture
    decisions change.

Branch names can follow conventions such as:

-   `feature/issue-summary`
-   `fix/issue-summary`
-   `docs/issue-summary`
-   `chore/issue-summary`

## 🔐 Security and Repository Access

Repositories should remain private while the product and business
requirements are being developed, unless a deliberate decision is made
to open specific parts of the project.

-   Grant access only to people who need it.
-   Prefer the minimum permissions necessary.
-   Protect the default branch and require review or checks when the
    team and tooling are ready.
-   Never store real credentials or private customer data in source
    control.
-   Report security issues privately rather than publishing sensitive
    exploit details in public issues.

## 🧰 Technology Overview

  -----------------------------------------------------------------------
  Area                                Planned technology
  ----------------------------------- -----------------------------------
  Backend API                         ASP.NET Core Web API / C#

  Backend architecture                Clean Architecture; use-case
                                      separation

  Database                            Microsoft SQL Server

  ORM and migrations                  Entity Framework Core

  Mobile                              React Native + Expo + TypeScript

  Admin dashboard                     Next.js + TypeScript + Material UI

  Public website                      Next.js + TypeScript

  API documentation                   OpenAPI / Swagger

  Source control and planning         GitHub repositories, Issues,
                                      Projects, and Pull Requests
  -----------------------------------------------------------------------

Technology choices may evolve through documented decisions. This table
describes the intended stack, not a guarantee that every listed
capability is already implemented.

## 📌 Current Status

P7 is under active development. Repository responsibilities and the
engineering workflow are being established so that the platform can be
delivered in small, testable increments.

Not every capability described in this document is necessarily
implemented. Feature availability should be confirmed through the
relevant repository, issues, documentation, and release notes.

## 📄 Documentation and Next Steps

For detailed product and engineering information, see the `p7-docs`
repository once its organization URL is configured.

Recommended next steps:

-   Establish the repositories and their initial READMEs.
-   Confirm repository visibility and access permissions.
-   Create the central **P7 Product Development** project board.
-   Add initial milestones and feature issues.
-   Document API conventions and cross-repository dependencies.
-   Start with the foundation review, then implement the first complete
    user-facing feature.

------------------------------------------------------------------------

**P7 Platform**\
*Trust. Clarity. Better home services.*

