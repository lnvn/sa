# Solution Architect

> Defines and foresees multiple aspects of a business solution — system infrastructure, networking, security, compliance requirements, system operation, cost, and reliability.

As project size increases and teams become distributed globally, a Solution Architect (SA) ensures long-term sustainability and a solid foundation. SA addresses various solution needs while keeping the business context intact — specifying technology platforms, application components, data requirements, resource requirements, and critical non-functional requirements.

---

## Table of Contents

- [What is a Solution Architect?](#what-is-a-solution-architect)
- [Responsibilities](#responsibilities)
- [Types of Solution Architects](#types-of-solution-architects)
  - [Generalists](#generalists)
  - [Specialists](#specialists)
- [Architecture Quality Attributes](#architecture-quality-attributes)
  - [1. Scalability and Elasticity](#1-scalability-and-elasticity)

---

## What is a Solution Architect?

A Solution Architect bridges business needs with technology decisions. They help derive **maximum return on investment** by defining architectures that are scalable, reliable, secure, and maintainable.

> SA requires multiple skill sets — from technical leadership and domain expertise, to business analysis and project management.

---

## Responsibilities

| Area | Description |
|---|---|
| **Non-Functional Requirements** | Performance, scalability, throughput, availability, maintainability |
| **Disaster Recovery** | Business continuity planning and recovery strategies |
| **Security & Compliance** | Governance, regulatory compliance, and access control |
| **High Availability** | Fault-tolerant, resilient system design |
| **Scalability** | Horizontal and vertical scaling strategies |
| **Application Performance** | Optimization of runtime and resource usage |
| **Network & Latency** | Request/response latency and network architecture |

---

## Types of Solution Architects

SAs are categorized as **Generalists** or **Specialists**.

### Generalists

Generalists bring breadth across multiple technical domains.

| Role | Focus Areas |
|---|---|
| **Enterprise SA** | Organization strategy, business architecture |
| **Solution Architect** | Solution design, solution integration |
| **Technical Architect** | Software design, software development |
| **Cloud Architect** | Cloud strategy, cloud migration |
| **Architect Evangelist** | Platform adoption, technical content |

### Specialists

Specialists have deep expertise in a specific domain.

| Role | Focus Areas |
|---|---|
| **Infrastructure Architect** | IT infrastructure design, software standardization and patching |
| **Network Architect** | Network design, IT network strategy and latency |
| **Data Architect** | Data engineering, data science, and data intelligence |
| **Security Architect** | Cyber security, IT compliance |

---

## Architecture Quality Attributes

A well-designed solution must address these core quality attributes:

- **Scalability and Elasticity** — grow and shrink with demand
- **High Availability and Resiliency** — minimize downtime
- **Fault Tolerance and Redundancy** — handle failures gracefully
- **Disaster Recovery and Business Continuity** — recover quickly from incidents
- **Extensibility and Reusability** — design for change and reuse
- **Portability and Interoperability** — work across platforms and systems
- **Operational Excellence and Maintainability** — ease of operation and upkeep
- **Security and Compliance** — protect data and meet regulations
- **Cost Optimization** — balance performance with budget

---

### 1. Scalability and Elasticity

Scalability is a primary factor in solution design. It's not only about growing your system to handle increased load — it's also about **shrinking it to save cost** when demand decreases.

Quest: What if AWS ASG overscale ?
- can set cooldown period  -> does not overscale
- can set instance warm up -> does not count instance booting
- can reduce bootup time by using pre-baked AMIs -> reduce bootup time

### Some notice when scale:
#### Design
- stateless workload (session should be save on ElastiCache or dynamoDB), then every instane can serve traffic.
- pre-baked AMIs, then can reduce boot-up time.
- Multi AZ, then prevent disaster
- set min capacity > 0

#### Metric
- Use target tracking
- CPU target 60-70%, depend on your system can bootup and serve traffic fast
- Use ALB request count, if CPU lag, request count can reflect imidiately
- avoid metrics lag for example cloudwatch metrics can be delayed 1-2 minutes.

#### Launch
- Instance warmup = boot time, then instance booting up will not count
- Health check grace period, equal or greater than boot time, avoid health check fail and ASG kill instance
- Use schedule scaling or predictive scaling

#### Cost
- Mix ondemand vs spot instance
- Right size instance
- Enable scale in protection
- Countdown scale in longer that scale out, avoid re-scale

#### Pitfall
- DB is a bottle-neck, need some strategy like rds proxy ...
- Save session on local => loss session when change target
- Thrashing - scale high frequency
- do not monitor ASG activity