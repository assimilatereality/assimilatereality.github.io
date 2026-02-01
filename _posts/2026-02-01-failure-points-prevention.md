---
layout: post
title: "The Failure Points We Build Into Our Systems: A Field Guide to Preventing Tomorrow's Outages"
date: 2025-02-01
categories: [engineering-practices, system-reliability]
tags: [sre, system-design, failure-analysis, cynefin-framework, incident-prevention, architectural-review, chaos-engineering, proactive-engineering]
author: [Rodney Tigges]
excerpt: "Every system failure you'll experience next year already exists in your architecture today—embedded during design, implementation, or the last 'quick change.' A systematic approach to identifying and eliminating failure points before they manifest."
---

# The Failure Points We Build Into Our Systems: A Field Guide to Preventing Tomorrow's Outages

_**Executive Summary:** Every system failure you'll experience next year already exists in your architecture today—embedded during design, implementation, or the last "quick change." Drawing from real incidents including global medical system outages from single-line network errors and service account deletions causing cascading failures, this analysis presents a systematic approach to identifying and eliminating failure points before they manifest. By categorizing risks through the Cynefin framework and implementing structured identification processes, teams can transform from reactive firefighting to proactive resilience engineering._

## The Uncomfortable Truth About System Failures

Here's something I learned after twenty years managing infrastructure: **every incident you'll handle next year is already built into your systems today**. Not as active bugs waiting to trigger, but as architectural decisions, configuration patterns, and operational practices that create the conditions for future failures.

I've watched a single missing line in a copy-pasted network configuration bring down a global medical system—a change categorized as "low risk, no customer impact" that took hours to identify and propagate fixes for. I've seen service accounts deleted without coordination, causing outages across multiple systems. I've witnessed 39 code changes rolled out together, with hours wasted identifying which one caused the problem buried in the middle.

The pattern is consistent: these weren't mysterious failures that appeared from nowhere. They were predictable consequences of how we designed, implemented, and maintained our systems. The medical system went down because we had a single point of failure in our network configuration. The service account deletions caused outages because we lacked dependency mapping. The problematic code change was hard to identify because we bundled deployments without proper isolation.

Every one of these failures was preventable—not through better incident response, but through proactive identification and mitigation during design and implementation.

## The Architecture of Built-In Failures

When I say failures are "built in," I mean something specific: architectural decisions and operational practices create conditions where failures become inevitable given sufficient time and complexity. These aren't bugs in the traditional sense—they're properties of how systems are constructed and maintained.

### Design-Phase Failure Points

The most consequential failure points emerge during initial system design:

**Single Points of Failure (SPOFs)**

Every architect knows to avoid SPOFs, yet I've seen them embedded in production systems repeatedly. Sometimes it's obvious—a single database instance, a unique load balancer, a solitary authentication service. Other times it's subtle—redundant components that share a common dependency, backup systems that rely on the primary network path, "redundant" services running on the same physical infrastructure.

At one hospital system, we discovered our "redundant" authentication infrastructure actually had a SPOF: both authentication servers, while physically separate, ran on virtual machines hosted by the same hypervisor. When that hypervisor experienced issues, both authentication systems became unavailable simultaneously. The redundancy was illusory—we'd built in the failure during virtualization planning.

**Insufficient Capacity Buffers**

Systems designed to handle expected peak load with minimal buffer guarantee eventual capacity failures. I've watched engineers optimize resource utilization to 90% during normal operations, believing this demonstrated efficiency. Then traffic patterns shifted slightly, or a minor component degradation occurred, and suddenly we were at 120% capacity with nowhere to go.

The failure was built in when we optimized for cost over resilience, failing to model realistic growth patterns or account for degraded operation scenarios.

**Inadequate Failure Isolation**

When systems lack proper bulkheads between components, local failures propagate into global outages. I've seen a single problematic API endpoint consume all connection pool resources, bringing down an entire application tier. A slow database query blocked connection threads, cascading into complete service unavailability.

These architectural decisions—how to structure connection pools, which components share resources, what isolation mechanisms to implement—directly determine how failures propagate through systems.

### Implementation-Phase Failure Points

Even well-designed systems acquire failure points during implementation:

**Configuration Management Gaps**

That global medical system outage from a missing configuration line? It happened because our network configuration management lacked automated validation. Engineers could copy-paste configurations between environments without verification that all required lines were present.

The failure was built in when we prioritized speed of changes over validation rigor. We had no automated checks verifying configuration completeness, no templates that enforced required elements, no peer review requirement for network changes.

**Undocumented Dependencies**

The service account deletions that caused cascading outages happened because we had no comprehensive dependency mapping. Systems relied on service accounts, but those relationships weren't documented. When someone cleaned up "unused" accounts, they had no way to know what would break.

This failure point was built in when we allowed systems to be implemented without documenting their external dependencies. We had no requirement that new services register their authentication dependencies, no automated discovery of service account usage, no governance around service account lifecycle management.

**Insufficient Testing Coverage**

That code change buried in 39 simultaneous deployments was hard to isolate because our testing and deployment practices made it impossible to identify causation. We bundled changes for efficiency without considering how this would complicate troubleshooting when issues emerged.

The failure was built in when we optimized for deployment velocity over diagnostic capability. We didn't require isolation of changes, didn't mandate feature flags for gradual rollout, didn't implement sufficient monitoring to correlate changes with behavior.

### Operational-Phase Failure Points

Ongoing operations continuously introduce new failure points:

**Technical Debt Without Documentation**

Every quick fix, temporary workaround, and "we'll clean this up later" decision creates future failure points. I've inherited systems held together by dozens of undocumented compensating controls—scripts that run on someone's laptop, manual processes that must execute in precise sequence, configuration quirks that can't be changed without breaking dependent systems.

These failure points accumulate gradually. Each one seems minor at the time, but collectively they create systems so fragile that any change becomes dangerous.

**Knowledge Concentration**

When only one or two people understand critical system components, you've built in a knowledge-based single point of failure. I've watched teams struggle through incidents because the person who understood a particular system was unavailable—on vacation, in a meeting, or having left the company.

This failure point is built in through poor documentation practices, inadequate knowledge transfer, and allowing systems to remain "tribal knowledge" rather than shared understanding.

## The Cynefin Lens: Understanding Failure Complexity

Dave Snowden's Cynefin framework provides a useful lens for understanding failure points and determining appropriate mitigation strategies. Different types of failure points require different approaches to identification and prevention.

### Simple/Obvious Domain Failures

Some failure points are straightforward and easily identifiable:

**Characteristics:**
- Clear cause-effect relationships
- Well-understood failure modes
- Standard mitigation patterns exist
- Can be detected through checklists and automated scans

**Examples:**
- Single instance of a critical database
- Hardcoded credentials in application code
- Network paths without redundancy
- Missing backups for critical data

**Mitigation Approach:**

For simple domain failure points, systematic identification through checklists and automated scanning works well. These are the low-hanging fruit—failures that are obvious once you look for them, easily addressed through standard architectural patterns.

I've used automated infrastructure scanning tools that identify single points of failure, check for redundancy in critical paths, verify backup configurations, and flag common anti-patterns. When a scan identifies a database without replication or a network segment with single-path connectivity, the mitigation is straightforward: implement established redundancy patterns.

### Complicated Domain Failures

Other failure points require expert analysis to identify:

**Characteristics:**
- Cause-effect relationships exist but require expertise to discern
- Multiple viable solutions rather than one right answer
- Failures emerge from component interactions
- Expert knowledge and specialized tools needed for identification

**Examples:**
- Shared resource contention between seemingly independent systems
- Subtle race conditions in distributed transactions
- Capacity limitations that emerge only under specific load patterns
- Configuration interactions that create unexpected failure modes

**Mitigation Approach:**

Complicated domain failure points demand expert analysis using specialized tools. I've seen distributed tracing reveal unexpected dependencies between services, load testing expose capacity limits that wouldn't surface in production for months, and performance profiling identify resource contention invisible to standard monitoring.

At one organization, we discovered a complicated failure point through methodical analysis: our caching layer and database connection pool sizing were independently optimized, but their interaction created a failure mode where cache misses could exhaust database connections. Neither component had an obvious problem—the failure point emerged from their relationship.

### Complex Domain Failures

The most challenging failure points involve emergent properties:

**Characteristics:**
- Cause-effect relationships only visible in retrospect
- Failures emerge from non-linear system interactions
- No clear relationship between component reliability and system reliability
- Pattern detection rather than causal analysis required

**Examples:**
- Cascading failures triggered by combinations of minor degradations
- Emergent capacity constraints from interaction of multiple components
- Failure modes that only manifest under specific combinations of conditions
- System behaviors that change as load patterns evolve

**Mitigation Approach:**

Complex domain failure points can't be identified through analysis alone—they require experimentation and observation. This is where chaos engineering becomes essential.

Netflix's approach to chaos engineering exemplifies this: rather than trying to predict all possible failure combinations, they deliberately inject failures in production and observe how systems respond. This reveals emergent failure points that analysis couldn't identify—the ways that real systems fail under actual operating conditions.

I've implemented controlled chaos experiments that revealed failure points invisible to static analysis: how connection pool exhaustion in one service triggers cascading timeouts in dependent services, how specific combinations of component degradations lead to complete system unavailability, how systems that work perfectly in isolation fail when integrated.

### Chaotic Domain Failures

During active incidents, we're often in the chaotic domain:

**Characteristics:**
- No discernible patterns in the moment
- Multiple simultaneous failures obscuring root causes
- Traditional diagnostic approaches ineffective
- Need for immediate stabilization before analysis

**Mitigation Approach:**

While we can't "plan" for chaotic domain failures, we can prepare for them through proper incident management practices. This means establishing clear command structures, defining decision-making authority, creating communication channels, and having predefined stabilization patterns.

I've been in incidents where management and executives jumped into the technical troubleshooting, hijacking the direction of investigation. This happens because we failed to establish clear roles during calm periods—we built in the organizational failure point by not defining who has authority during incidents.

## A Systematic Approach to Failure Point Identification

Based on these observations and the Cynefin framework, here's a systematic approach to identifying failure points before they cause incidents:

### 1. Architectural Review: Simple Domain Identification

**Process:**
- Automated scanning for obvious SPOFs
- Checklist-based architecture reviews
- Dependency mapping to identify critical paths
- Capacity planning with realistic buffers

**Concrete Implementation:**

I've run monthly "SPOF hunts" where teams systematically examine infrastructure diagrams, identify single-instance components, document dependencies on shared resources, and verify redundancy claims are real (not illusory like that authentication system).

This catches the straightforward failures: the database without replication, the load balancer without a pair, the API gateway that's a single point of failure.

### 2. Expert Analysis: Complicated Domain Investigation

**Process:**
- Performance profiling under realistic load
- Distributed tracing to reveal subtle dependencies
- Resource contention analysis
- Interaction pattern modeling

**Concrete Implementation:**

Quarterly, we run comprehensive load tests that stress systems beyond expected capacity, use distributed tracing to map actual (not documented) dependencies, profile resource utilization patterns, and identify interaction effects between components.

This reveals the failures that require expertise: the cache-database interaction problem, the connection pool sizing that's optimal individually but problematic collectively, the timeout configurations that cascade under stress.

### 3. Chaos Engineering: Complex Domain Discovery

**Process:**
- Controlled failure injection in production
- Observation of emergent system behaviors
- Documentation of unexpected failure modes
- Iterative improvement of resilience

**Concrete Implementation:**

I've helped implement weekly chaos experiments: random instance termination to verify recovery mechanisms, network latency injection to test timeout handling, dependency unavailability simulation, and resource constraint introduction.

The key is starting small—failing a single instance during business hours when teams are ready to respond—and gradually expanding scope as confidence builds. Each experiment reveals failure points that static analysis missed.

### 4. Incident Learning: Converting Chaos to Knowledge

**Process:**
- Blameless postmortems that identify systemic issues
- Pattern analysis across multiple incidents
- Root cause tracking to architectural decisions
- Action item enforcement with executive support

**Concrete Implementation:**

After every incident, we ask not just "what broke?" but "when did we build in this failure point?" If a service account deletion caused an outage, when did we allow that dependency to be created without documentation? If a configuration error propagated, when did we decide not to implement validation?

This transforms incidents from isolated events into learning opportunities that prevent entire classes of future failures.

## The Documentation Challenge

One of the most common failure points I've observed is inadequate documentation of system dependencies, configuration requirements, and operational procedures. This creates a particular type of fragility: systems work fine as long as specific people with specific knowledge remain available, but fail when that knowledge isn't accessible.

### What Needs Documentation

Not everything requires equal documentation depth. I focus on:

**Critical Dependencies:**
- Service accounts and their usage
- External system dependencies
- Required network paths and firewall rules
- Database connection requirements
- Shared resource dependencies

**Configuration Requirements:**
- Non-obvious configuration relationships
- Order-dependent settings
- Environment-specific variations
- Consequences of configuration changes

**Operational Knowledge:**
- Deployment procedures and their risks
- Common failure modes and their symptoms
- Troubleshooting approaches for known issues
- Escalation paths and decision authorities

### Documentation That Scales

Traditional documentation fails because it becomes outdated immediately. I've moved toward documentation that's:

**Self-Updating Where Possible:**
- Infrastructure-as-code that documents itself
- Automated dependency discovery
- Configuration management that enforces relationships
- Monitoring that reveals actual system behavior

**Living Documentation:**
- Architecture decision records (ADRs) explaining why choices were made
- Runbooks that evolve through incident learnings
- Postmortem databases that capture pattern knowledge
- Regular review cycles to verify accuracy

## The Political Challenge: Getting Organizations to Care

Technical solutions for identifying failure points are relatively straightforward. The harder challenge is organizational—getting leadership to prioritize failure point elimination over feature velocity, allocate resources for resilience engineering, and accept that prevention is more valuable than rapid incident response.

### Making the Business Case

I've found several approaches effective:

**Quantify the Cost of Incidents:**

Calculate actual cost of downtime—not just lost revenue, but engineering time responding, customer support costs, reputation damage, and opportunity costs. When leadership sees that a two-hour outage costs $500K, investing in prevention becomes rational.

**Demonstrate Competitive Risk:**

In healthcare, I could point to competitors who maintained availability during high-traffic periods while we experienced outages. In financial services, regulatory scrutiny of availability becomes a forcing function. Find your organization's pressure points.

**Start Small and Prove Value:**

Rather than proposing comprehensive resilience engineering programs, identify one high-visibility failure point, eliminate it, and publicize the success. Build momentum through demonstrated value.

**Connect to Strategic Initiatives:**

When the organization prioritizes growth, frame resilience as enabling that growth. When security becomes a focus, show how failure points create security vulnerabilities. Find alignment between resilience and existing priorities.

### Organizational Structures That Help

Some organizational patterns make proactive failure point identification more likely:

**Dedicated Reliability Engineering:**

Having a team explicitly responsible for system reliability—distinct from feature development—creates organizational focus on resilience.

**Architecture Review Boards:**

Mandatory review of designs before implementation catches failure points early. The key is giving these boards actual authority to require changes.

**Chaos Engineering Programs:**

Institutionalizing regular chaos experiments normalizes thinking about failures and creates systematic discovery of failure points.

**Error Budget Frameworks:**

Using SRE-style error budgets forces explicit trade-offs between feature velocity and reliability, making failure point elimination part of regular prioritization.

## From Reactive to Proactive: The Cultural Shift

The ultimate goal is cultural transformation—shifting from viewing failures as inevitable surprises to recognizing them as consequences of our design, implementation, and operational choices.

### What This Culture Looks Like

In organizations that have made this shift, I observe:

**Design Reviews Focus on Failure:**

Rather than just reviewing how systems work when functioning, teams systematically explore how they'll fail. What happens when this database becomes unavailable? How does the system behave at 200% expected capacity? What occurs if this API endpoint becomes slow?

**Implementation Includes Failure Testing:**

Before declaring features "complete," teams validate failure handling. They inject faults, simulate degraded dependencies, and verify graceful degradation. Failure testing is as routine as functional testing.

**Operations Includes Failure Experiments:**

Regular chaos engineering experiments are scheduled maintenance windows, not special events. Teams proactively discover failure points rather than waiting for incidents to reveal them.

**Incidents Lead to Architectural Changes:**

Postmortems don't just produce action items like "add monitoring" or "update runbook." They trace failures back to design decisions and result in architectural improvements that eliminate entire classes of failures.

### Getting Started

If your organization isn't there yet, start where you are:

**Pick One System:**

Choose a critical system and systematically identify its failure points. Document SPOFs, map dependencies, test failure modes, and eliminate identified risks.

**Run One Chaos Experiment:**

Select a low-risk failure to inject—terminate a single instance of a horizontally scaled service during business hours with the team ready to respond. Learn from what happens.

**Document One System Comprehensively:**

Choose a system that's currently tribal knowledge and create thorough documentation—dependencies, configuration requirements, operational procedures, known failure modes.

**Fix One Organizational Practice:**

If service accounts are being deleted without coordination, implement a governance process. If changes are bundled making problems hard to isolate, require deployment isolation.

Small wins build momentum. Each success makes the next improvement easier.

## Conclusion: The Failures We Choose

Every system will fail. That's inevitable given sufficient complexity and time. But we choose which failures our systems experience through our design decisions, implementation practices, and operational disciplines.

The medical system didn't have to go down from a missing configuration line—we could have implemented automated validation. Service accounts didn't have to cause cascading outages—we could have mapped dependencies. The problematic code change didn't have to be so hard to isolate—we could have deployed with better isolation.

These failures weren't mysteries. They were choices—often unconscious, sometimes deliberate ("we'll fix that later"), but always choices. We built them into our systems.

The opportunity is to make different choices. To systematically identify failure points during design rather than discovering them during incidents. To implement validation that catches configuration errors. To document dependencies that prevent cascade failures. To deploy changes with isolation that simplifies troubleshooting.

This requires shifting from reactive incident response to proactive failure point elimination. It requires organizational commitment to resilience engineering. It requires accepting that prevention, while less visible than heroic incident response, is more valuable.

The failure points are already in your systems. The question is whether you'll find them through systematic identification or wait for them to find you through incidents.

What failure point will you eliminate today?
