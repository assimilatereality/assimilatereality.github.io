---
layout: post
title: "Building Organizations That Learn from Chaos"
date: 2025-12-20
categories:
  - resilience
  - devops
  - reliability
  - systems-thinking
tags: [resilience-engineering, devops, sre, hro, systems-thinking, incident-management, observability]
---

# Building Organizations That Learn from Chaos

_**Executive Summary:** The strongest technical organizations aren't those that prevent all failures—they're the ones that learn fastest from them. This article explores how Resilience Engineering, DevOps practices, and High Reliability Organization theory combine to build teams and systems that genuinely improve through adversity. I examine the gap between how we think work happens versus how it actually happens, and show how service level objectives, error budgets, and blameless postmortems translate theory into practice. The key insight: incidents aren't anomalies to be eliminated but inherent properties of complex systems that smart organizations convert into competitive advantages._

---

I've explored how reality and fitness perception shape our troubleshooting approaches and looked at frameworks for applying them appropriately. But here's what I've learned over the years: none of that matters much if your organization doesn't know how to learn from the inevitable chaos. Today I want to talk about building resilient organizations, not just resilient systems.

## Asking the Wrong Question

Traditional approaches to system safety focus almost exclusively on preventing failures. Find the problem, fix it, prevent it from happening again. Repeat forever. Erik Hollnagel calls this "Safety-I" thinking.

But what if we're asking the wrong question?

Instead of "why do things go wrong?" what if we asked "how do things go right?" This shift—what Hollnagel calls "Safety-II"—recognizes something fundamental: success and failure come from the same source. They're both products of how people adapt to complex situations.

Think about it. When an engineer makes a judgment call during an incident and the system stays up, we call it good operations. When that same type of judgment leads to an outage, we call it an incident. But the cognitive process is identical. The difference is outcome, not approach.

Commercial aviation maintains an extraordinary safety record not because pilots never make mistakes—they do, constantly—but because the system has evolved to catch, correct, and learn from those mistakes before they cascade into disasters. The focus isn't on eliminating errors but on creating contexts where errors can be handled effectively.

This maps directly to what I've called fitness perception: success comes from effective adaptation, not perfect understanding. The most reliable systems aren't those where nothing goes wrong—they're those that respond effectively when things inevitably do.

## The Gap Between Plans and Reality

Here's something that frustrates me about many organizations: the persistent gap between "work-as-imagined" and "work-as-done." 

You know what I'm talking about. The operations team maintains secondary documentation, or worse, unwritten knowledge that leaves with personnel departures, because the official procedures don't reflect how the system actually behaves. Developers bypass the formal change approval process for "quick fixes" that somehow become permanent. System administrators know exactly which server to restart when a particular error appears, though no one quite understands why it works or what went wrong.

These gaps don't come from negligence. They emerge because complex systems are fundamentally... well, complex. No documentation, no matter how detailed, can capture all the emergent properties of systems interacting with their environments. No process can anticipate every scenario that arises in practice.

When organizations take a rigid reality-perception stance, this gap becomes a source of conflict. "Why can't people just follow the procedures?" But here's what I've learned: those undocumented workarounds and adaptations aren't violations—they're valuable signals. They show us where our formal models fail to match operational reality.

Organizations that understand this don't try to force work-as-done to conform to work-as-imagined. Instead, they study the adaptations engineers make and incorporate those insights into formal processes. When engineers create workarounds, they're not breaking the system—they're showing us where our understanding is incomplete.

## Four Capabilities That Actually Matter

Hollnagel identifies four capabilities essential for resilient systems: learning, anticipating, monitoring, and responding. But these aren't isolated skills—they're interconnected aspects of how organizations handle reality. And they map intriguingly to the perception frameworks I've been discussing.

**Learning from the past** demands deep reality perception. You have to trace causal paths through complex events, identifying not just what happened but why and how. This generates the mental models that inform future actions. But effective learning requires acknowledging limits—recognizing that even detailed post-incident analysis can't capture every contributing factor.

I've been in countless postmortems. The good ones accept that our understanding will always be partial. The bad ones create an illusion of complete understanding that makes the next incident more surprising.

**Anticipating future developments** requires both frameworks working together. Reality perception helps project current trends forward. Fitness perception helps prepare for unexpected variations. Effective organizations don't just prepare for predicted scenarios—they build adaptive capacity that can be deployed against unanticipated challenges.

**Monitoring present conditions** is where the frameworks meet. What you choose to monitor reflects your reality perception—what you believe matters. But how you interpret those signals often uses fitness perception heuristics—pattern recognition that helps you quickly identify deviations.

**Responding to situations** typically draws most heavily on fitness perception. When alarms fire at 3 AM, you don't have time to rebuild complete mental models. You rely on experience-based heuristics, pattern matching, and simplified interfaces that enable effective action despite uncertainty.

These four capabilities don't exist in isolation. They form a continuous learning cycle. Response generates experiences that inform learning. Learning shapes what you monitor and how you anticipate. Anticipation prepares more effective responses. When all four work in harmony, you get organizations that don't just withstand challenges but grow stronger through them.

## Making It Practical: DevOps and SRE

Resilience Engineering provides the theoretical foundation, but the DevOps movement and Google's Site Reliability Engineering approach offer concrete practices. These aren't just technical methodologies—they're cultural frameworks that bridge the gap between development and operations, between work-as-imagined and work-as-done.

### Service Level Objectives: Drawing the Line

Service Level Objectives transform subjective assessments into measurable targets. When an API team commits to 200ms response time for 99.9% of requests, they're creating a clear boundary between acceptable and unacceptable performance.

Here's why this matters: that boundary serves as a shared reference point for both perception approaches. Within the acceptable range, teams can use quick fixes and heuristics, confident the system remains within desired parameters. When metrics approach or cross boundaries, it signals the need for deeper investigation—a shift from fitness to reality perception.

This objective trigger helps organizations balance quick remediation with thorough understanding. I've seen too many teams oscillate between firefighting everything and investigating nothing. SLOs provide the criteria for deciding which approach fits.

### Error Budgets: Permission to Break Things

Error budgets extend this concept by acknowledging that perfection is neither possible nor economically rational. A 99.9% uptime target allows about 43 minutes of downtime monthly. Teams can "spend" this budget on planned improvements or unplanned incidents.

This creates a natural balancing mechanism. I've worked in environments where fear of any downtime paralyzed development. Nothing changed because change was too risky. Error budgets flip that script: you have explicit permission to break things within defined limits. This enables innovation while maintaining stability.

### Infrastructure as Code: Making Mental Models Tangible

Infrastructure as Code transforms tacit knowledge of system configuration into explicit, version-controlled artifacts. This practice doesn't just improve consistency—it makes work-as-imagined transparent and executable.

When configuration exists as code rather than manual procedures, the gap between intention and implementation narrows. Changes become visible, testable, and reversible. This supports both perception frameworks while reducing cognitive load on operators.

### Blameless Postmortems: Creating Safe Spaces for Truth

The DevOps concept of blameless postmortems creates safe spaces for reality perception after incidents. By focusing on systemic factors rather than individual actions, these structured analyses enable honest exploration without triggering defensive responses.

I cannot overstate how important this is. In environments where incidents lead to blame, engineers learn to hide problems, minimize impact, and cover tracks. Blameless postmortems allow engineers to describe what they observed, what they did, and why it seemed reasonable at the time, without fear. It cannot be blameless in name only; it has to be real and believed.

This psychological safety is essential for bridging from fitness perception (how operators responded in the moment) to reality perception (understanding the broader context that made those responses necessary).

## Learning from Organizations That Get It Right

Some organizations—nuclear power plants, aircraft carriers, air traffic control centers—maintain extraordinary safety records despite operating complex, high-risk systems. High Reliability Organizations implement cultural principles that enable consistent performance.

What distinguishes HROs isn't flawless execution. It's a distinctive approach to failure, simplification, operations, resilience, and expertise. They're simultaneously more concerned with failure and more resilient to it.

### Preoccupation with Failure

HROs treat near-misses not as successes ("nothing bad happened") but as failures that happened to be caught. When a nuclear plant operator reports an unusual vibration that turns out to be innocuous, HROs don't dismiss this as a false alarm—they celebrate it as vigilance.

This attitude creates early warning systems. Problems get detected when they're small and manageable. Reward people for raising concerns, even when those concerns turn out to be nothing, rather than just nodding and moving on. Because the one time it's something, that early warning is invaluable.

### Reluctance to Simplify

Most organizations value simple, clear explanations. HROs intentionally cultivate diverse perspectives and complex models. They understand that simplification, while cognitively satisfying, often masks the subtle interactions that lead to failure.

When investigating incidents, they deliberately seek alternative explanations, encouraging minority viewpoints that might reveal overlooked factors. This doesn't paralyze action—it's balanced by sensitivity to actual operations.

### Sensitivity to Operations

HROs maintain awareness of the actual state of systems rather than relying on assumptions or plans. They create communication channels that bring real-time information from frontline operators to decision-makers.

This operational awareness helps detect and correct small problems before they escalate. I've seen organizations where executives made decisions based on how they thought systems worked, not how systems actually worked. Those organizations had more incidents.

### Commitment to Resilience

HROs don't just plan for expected contingencies. They build general adaptive capacity that can be deployed against novel challenges. This includes redundant systems, cross-trained personnel, and regular practice responding to simulated emergencies. EVERY company I've worked for has built such systems and environment when income was good, only to fail to plan for economic downturns and toss them under a 'do more with less' philosophy. As a result, they were not there when most needed to excel in a more competitive environment.

### Deference to Expertise

Perhaps most remarkable: HROs shift decision authority to those with the most relevant knowledge regardless of hierarchy. During normal operations, traditional command structures function. But when anomalies arise, authority migrates to whoever has the expertise most relevant to the challenge.

A junior technician who understands a specific subsystem may temporarily lead response efforts, with senior leaders supporting rather than directing. This requires genuine organizational humility—admitting that rank doesn't equal relevant expertise in every situation.

## Incidents: Built Into the System

Here's something I've come to understand: incidents aren't anomalies but inherent properties of complex systems. They're effectively "built in" during design, implementation, and modification—not through negligence but through the fundamental constraints of complex engineering.

Think about a system's lifecycle. During initial design, architectural decisions necessarily introduce constraints and potential failure modes. Choose microservices? You've introduced network dependencies and potential consistency challenges. Prefer monoliths? You've created tight coupling that may make partial failures impossible. Every technical choice closes some failure modes while opening others.

Implementation adds another layer. The languages chosen, libraries incorporated, coding patterns adopted—all bring their own failure characteristics. These aren't bugs to be eliminated but properties to be understood and managed.

Then comes deployment, where systems meet their operating environment. New interactions emerge—with users, with other systems, with varying load patterns. Each new interaction creates possibilities for unexpected behavior.

Maintenance compounds this. Every change, even improvements, introduces the potential for new incidents while resolving old ones.

This perspective suggests a fundamental shift. Rather than viewing incidents as preventable anomalies, recognize them as inevitable expressions of complexity. The question isn't whether a system will experience incidents but which incidents it will experience and how it will respond.

This aligns with the perception frameworks I've been discussing. Reality perception helps understand specific incident mechanisms. Fitness perception helps develop effective responses. Both are necessary, but neither is sufficient alone.

## The Knowledge Transfer Problem

Complex systems outlive their creators. Engineers design, build, and modify systems, then move to other projects or organizations. How do we ensure that hard-won insights about system behavior transfer to those who inherit operational responsibility?

Traditional approaches focus on documentation. But documentation alone proves insufficient for several reasons:

First, it struggles to capture tacit knowledge—the intuitive understanding that experienced operators develop through years of interaction. You can document the steps to resolve an issue, but not the subtle cues that experienced engineers use to detect problems early. Tacit knowledge is why Michelin star chefs can write a cookbook and not worry about competition. Tacit knowledge is impossible to properly document, rather it takes time to learn under expert tutelage.

Second, documentation diverges from reality as systems evolve. When documentation updates require extra effort beyond making changes, they often don't happen.

Third, static documentation can't reflect the dynamic nature of complex systems—how behavior changes under different conditions, how components interact in unexpected ways.

More effective knowledge transfer requires mechanisms that communicate mental models, not just procedures:

**Extended transitions** allow overlap between departing and arriving operators, creating space for experiential learning. Rather than just handing over documentation, departing operators demonstrate their approach to common scenarios, explaining not just what they do but why.

**Scenario training** simulates common incidents, allowing new operators to develop pattern recognition before encountering real emergencies. These simulations should incorporate decision-making processes—how to prioritize, when to escalate, how to communicate.

**System visualization** creates interfaces that make internal states visible and interpretable. Well-designed dashboards don't just display metrics but reveal relationships between components, helping operators build accurate mental models.

**Failure injection** deliberately introduces controlled failures to build operator experience. Tools like Chaos Monkey help teams develop resilience to common failure modes.

But beyond these approaches, effective knowledge transfer requires organizational structures that value operational expertise and create channels for feeding operational insights back into system design. When operators discover new failure modes or develop new remediation strategies, these learnings should influence future evolution.

## Bringing It All Together

As I've explored Resilience Engineering, DevOps/SRE, and HRO theory, common threads emerge. All three recognize the importance of balancing theoretical understanding with practical response. All acknowledge that success in complex environments requires both systematic rigor and adaptive flexibility. All value the knowledge that emerges from operations as much as the knowledge that informs design.

These commonalities point toward an integrated approach that transcends traditional divisions. Organizations that embody this integration share several characteristics:

They create clear standards for when fitness-based approaches suffice and when deeper investigation is warranted—through explicit triggers like SLOs or implicit cultural norms.

They build observability that reveals system reality, making internal states visible and interpretable through both technical implementation and organizational channels.

They cultivate mindsets that value both quick response and deeper understanding, avoiding false dichotomies between "firefighters" and "investigators."

They develop practices that bridge immediate resolution and longer-term learning, creating virtuous cycles where each incident strengthens rather than depletes organizational capacity.

They acknowledge incidents as inherent system properties, not external disruptions. This shifts focus from blame to understanding, from prevention to preparation.

They create knowledge transfer mechanisms that bridge between system creators and operators, ensuring operational insights inform future design and design intentions remain visible during operations.

## Living Practice, Not Fixed Methodology

The most successful organizations don't implement these frameworks as rigid methodologies. They embody them as living practices—continuously evolving approaches that reflect their specific challenges.

This requires acknowledging the tension between standardization and adaptation, between documented processes and emergent responses. Rather than trying to eliminate this tension, effective organizations harness it as a source of resilience.

In my next article, I'll explore systems thinking and complexity theory—frameworks that help explain why pure reality perception approaches sometimes fail in complex system operations. But before that, consider: How effectively does your organization transfer mental models between system creators and operators? What mechanisms could you implement to better acknowledge and prepare for the incidents built into your systems by design? How might you evolve from viewing incidents as preventable anomalies to seeing them as opportunities for system strengthening?

Through this evolution in thinking, we don't just build more reliable systems—we create organizations capable of learning, adapting, and growing stronger through the challenges they inevitably face.
