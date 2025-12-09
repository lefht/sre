# Leyth's SRE Troubleshooting Playbook

> A systematic approach for tackling incidents and complex distributed system issues

---

## Table of Contents

- [Overview](#overview)
- [The Systematic Approach](#the-systematic-approach)
  - [1. Establish Your Baseline First](#1-establish-your-baseline-first)
  - [2. Follow the Request Path](#2-follow-the-request-path)
  - [3. Check Recent Changes](#3-check-recent-changes)
  - [4. Use the Scientific Method with Blast Radius Awareness](#4-use-the-scientific-method-with-blast-radius-awareness)
  - [5. Leverage Your Observability Stack](#5-leverage-your-observability-stack)
  - [6. Look for Patterns, Not Anomalies](#6-look-for-patterns-not-anomalies)
  - [7. Consider Cascading Failures](#7-consider-cascading-failures)
  - [8. Have Runbooks, Don't Be Enslaved by Them](#8-have-runbooks-dont-be-enslaved-by-them)
  - [9. Practice Chaos Engineering Principles](#9-practice-chaos-engineering-principles)
  - [10. Document the Incident Timeline](#10-document-the-incident-timeline)
  - [11. Know Your Kill Switches](#11-know-your-kill-switches)
- [Quick Reference](#quick-reference)
- [Contributing](#contributing)

---

## Overview

This guide provides a battle-tested framework for troubleshooting production incidents in distributed systems. Whether you're dealing with a quick incident or a complex multi-service failure, these principles will help you diagnose and resolve issues systematically.

---

## The Systematic Approach

### 1. 📊 Establish Your Baseline First

Before you start changing things, understand what's happening.

**Key Questions:**
- What's the expected behavior?
- What are the actual symptoms?
- What's the scope? (one user, one region, everything?)
- Get metrics, logs, and traces lined up

> ⚠️ **This prevents fixing the wrong problem**

---

### 2. 🔍 Follow the Request Path

Distributed systems fail in layers. Trace the flow through your entire stack and find where things diverge from expected behavior.

```
Client → Load Balancer → API Gateway → Service Mesh → Application → Database → Dependencies
```

**Tools:** Distributed tracing (Jaeger, Zipkin, Datadog APM)

---

### 3. 🕐 Check Recent Changes

In production systems, **most issues correlate with changes**.

**Review:**
- Recent deployments
- Config changes
- Infrastructure updates
- Upstream dependency updates

> 💡 **Your CI/CD pipeline and deployment logs are your timeline**

---

### 4. 🔬 Use the Scientific Method with Blast Radius Awareness

When testing hypotheses in production, always consider:

- ❓ What's the impact if I'm wrong?
- 🎯 Can I test this safely in a subset?
- ↩️ Do I have a quick rollback path?

**Best Practices:**
- Progressive rollouts
- Feature flags
- Canary deployments

---

### 5. 📡 Leverage Your Observability Stack

The three pillars work together to give you complete visibility:

| Pillar | Purpose | What It Shows |
|--------|---------|---------------|
| 📊 **Metrics** | Tell you **WHAT'S** broken | Latency, error rates, saturation |
| 📝 **Logs** | Tell you **WHY** it's broken | Error messages, stack traces |
| 🗺️ **Traces** | Tell you **WHERE** it's broken | Request flow, bottlenecks |

**Workflow:**
1. Start with metrics for the big picture
2. Drill into logs for specifics
3. Use traces to follow individual requests through the system

---

### 6. 🎯 Look for Patterns, Not Anomalies

Pattern recognition often reveals the root cause faster than chasing individual failures.

**Ask:**
- Is this affecting all requests or specific ones?
- Certain endpoints?
- Particular users?
- Geographic regions?

---

### 7. ⚠️ Consider Cascading Failures

In distributed systems, problems propagate. Sometimes you need to fix the downstream problem before the upstream symptoms resolve.

**Typical Cascade:**
```
Database slows down
    ↓
Services start timing out
    ↓
Connection pools get exhausted
    ↓
Circuit breakers trip
    ↓
Load shifts to other services
    ↓
New bottlenecks emerge
```

---

### 8. 📚 Have Runbooks, Don't Be Enslaved by Them

Good runbooks capture institutional knowledge, but complex systems fail in novel ways.

> 💡 **Use them as starting points, not scripts**

---

### 9. 🎲 Practice Chaos Engineering Principles

The best troubleshooting happens **before** production breaks.

**Proactive Practices:**
- Regular game days
- Failure injection
- Load testing
- Chaos experiments

> 🎯 **Build intuition for how your systems fail**

---

### 10. 📋 Document the Incident Timeline

During major incidents, maintain a timeline:

- 🔍 Observations
- 💭 Hypotheses tested
- ⚡ Actions taken

**Benefits:**
- Helps during post-mortems
- Trains others on your reasoning process
- Prevents repeated work

---

### 11. 🛑 Know Your Kill Switches

Sometimes the best troubleshooting move is damage control while you investigate properly.

**Options:**
- 🔄 Failover to backup region
- 📉 Drop non-critical traffic
- ⏸️ Disable problematic features
- 📈 Scale horizontally
- 🔧 Enable degraded mode

---

## Quick Reference

### Incident Response Checklist

- [ ] Establish baseline (what/scope/symptoms)
- [ ] Check monitoring dashboards
- [ ] Review recent changes/deployments
- [ ] Follow request path through system
- [ ] Check for patterns in affected requests
- [ ] Look for cascading failures
- [ ] Test hypotheses safely (blast radius)
- [ ] Document timeline of actions
- [ ] Consider kill switches if needed
- [ ] Verify fix and monitor

### Golden Signals to Monitor

1. **Latency** - How long it takes to service a request
2. **Traffic** - How much demand is on your system
3. **Errors** - Rate of requests that fail
4. **Saturation** - How "full" your service is

---
