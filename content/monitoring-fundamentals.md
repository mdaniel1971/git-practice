---
title: Monitoring Fundamentals for DevOps
author: Jane Smith
date: 2024-09-26
tags: [devops, monitoring, observability]
---

<Activity type="content" title="Introduction">

_Understanding the foundation of effective system monitoring_

<EngagementPrompt title="Did you know..?">

In 2023, a major e-commerce platform lost **$2.5 million** in just 4 hours due to a service outage that could have been prevented with proper monitoring.  
The incident wasnt detected for 45 minutes because their monitoring system had blind spots.  
This unit will teach you how to build comprehensive monitoring that catches issues before they impact users.

</EngagementPrompt>

# Why this unit matters

Monitoring is the eyes and ears of your DevOps infrastructure. Without it your flying blind.  
You will learn to implement monitoring systems that provide visibility into application health, performance, and security.  
As you work through this content consider how these monitoring principles apply to the module project you'll be building.

# Learning Objectives

1. Understand the difference between monitoring, observability and logging
1. Implement basic monitoring for applications and infrastructure.
1. Design effective alerts that reduce noise and focus on actionable issues.
1. Analyze monitoring data to identify and resolve performance bottlenecks

<EngagementPrompt title="Pause and think">

* **Consider your experience:** Have you ever experienced a system outage? What role did monitoring play?
* **Think about your current systems:** What monitoring gaps might exist in your organization?
* **Reflect on impact:** How could better monitoring improve your teams response time?

</EngagementPrompt>

</Activity>

<Flow title="The Three Pillars of Observability">

Modern system observability is built on three core pillars that work together to provide complete visibility.

# Metrics

Metrics are numerical measurements collected over time. They answer the question "what is happening?"  
Common metrics include CPU usage, memory consumption, request latency, and error rates.  
Tools like Prometheus and Grafana excel at collecting and visualizing metrics data.

## Key Metric Types
- **Counter:** A value that only increases (e.g. total requests)
- **Gauge:** A value that can go up or down (e.g. current memory usage)  
- **Histogram:** Distribution of values (e.g. request duration percentiles)

# Logs

Logs are timestamped records of discrete events. They answer "what happened and when?"  
Every application should log important events, errors, and state changes.  
Centralized logging systems like ELK stack (Elasticsearch, Logstash, Kibana) or Splunk make logs searchable and actionable.

<Highlight title="Log Best Practices">

- Use structured logging (JSON format)
- Include correlation IDs to track requests across services
- Set appropriate log levels (DEBUG, INFO, WARN, ERROR)
- Never log sensitive data like passwords or credit card numbers!

</Highlight>

# Traces

Distributed tracing shows the path of a request through your system. It answers "where is the bottleneck?"  
Tools like Jaeger and Zipkin visualize the complete journey of a request across microservices.  
This is essential for understanding complex distributed systems.

<Example title="Tracing in Action">

A user reports slow checkout times. Your trace shows:  
1. Frontend → API Gateway: 50ms
2. API Gateway → Auth Service: 200ms  
3. Auth Service → Database: **2000ms** ← Problem found!
4. Database → Auth Service: 50ms
5. Return to user: 100ms

The database query is the bottleneck, taking 2 seconds when it should take milliseconds.

</Example>

<EngagementPrompt title="Knowledge Check">

* What is the key difference between metrics and logs?
* Why is distributed tracing particularly important for microservices?
* How do the three pillars work together to provide complete observability

</EngagementPrompt>

</Flow>

<Flow title="Designing Effective Alerts">

![Alert Strategy](./m0.2.1-alert-strategy.png)

# The Alert Fatigue Problem

Alert fatigue occurs when teams receive so many alerts that they start ignoring them.  
This is dangerous - critical issues get lost in the noise.  
A well designed alerting strategy focuses on **actionable** alerts that require human intervention.

## Symptoms of Alert Fatigue
- Team members ignore or silence alerts without investigating
- Alerts are sent to channels nobody monitors
- 90% of alerts are false positives
- Real incidents are missed because of alert volume

# Alert Design Principles

## 1. Alert on Symptoms, Not Causes
**Bad:** "Disk usage is at 85%"  
**Good:** "API response time exceeded 500ms for 5 minutes"

Alert on what matters to users, not internal metrics that may or may not cause problems.

## 2. Make Alerts Actionable
Every alert should include:
- Clear description of the problem
- Impact on users or business
- Suggested remediation steps
- Link to runbook or documentation

## 3. Use Appropriate Severity Levels
- **P1/Critical:** Service down, immediate action required
- **P2/High:** Degraded performance, action needed within hours
- **P3/Medium:** Issue detected, investigate during business hours
- **P4/Low:** Informational, no immediate action

## 4. Implement Alert Routing
Route alerts to the right team at the right time:
- Business hours vs. on-call rotation
- Team-specific alerts (database team vs. application team)
- Escalation paths for unacknowledged alerts

<Tip title="The 5-Minute Rule">

If an alert fires and resolves itself within 5 minutes, it probably shouldn't page someone.  
Use appropriate time windows and thresholds to reduce noise.  
For example: "CPU > 90% for 10 minutes" instead of "CPU > 90%"

</Tip>

<EngagementPrompt title="Knowledge Check">

* What is alert fatigue and why is it dangerous?
* Why should you alert on symptoms rather than causes?
* What information should every alert include to be actionable?

</EngagementPrompt>

</Flow>

<Activity title="Skills Application: Design a Monitoring Strategy">

You are the DevOps engineer for _HealthTrack_, a fitness tracking application with 500,000 users.  
The application consists of:
- React frontend
- Node.js API backend  
- PostgreSQL database
- Redis cache
- Running on AWS ECS containers

Your task is to design a comprehensive monitoring strategy.

# Instructions

1. Identify the key metrics to monitor for each component
2. Design 3-5 critical alerts with appropriate thresholds
3. Create a runbook template for responding to alerts
4. Document your monitoring architecture

# Solution Exemplar

## Key Metrics by Component

**Frontend (React)**
- Page load time (target: < 2s)
- JavaScript errors per session
- API call success rate

**API Backend (Node.js)**  
- Request rate (requests/second)
- Response time (p50, p95, p99)
- Error rate (4xx, 5xx responses)
- Active connections

**Database (PostgreSQL)**
- Query execution time
- Connection pool utilization  
- Slow query count (> 1s)
- Replication lag

**Cache (Redis)**
- Hit/miss ratio
- Memory usage
- Eviction rate

## Critical Alerts

1. **API Error Rate Alert**
   - Condition: Error rate > 5% for 5 minutes
   - Severity: P1
   - Action: Check application logs, recent deployments

2. **Database Connection Pool Alert**
   - Condition: Connection pool > 80% for 10 minutes  
   - Severity: P2
   - Action: Investigate slow queries, consider scaling

3. **Response Time Degradation**
   - Condition: p95 latency > 1000ms for 5 minutes
   - Severity: P2  
   - Action: Check downstream dependencies, database performance

</Activity>

<Activity type="quiz" title="Knowledge Check Quiz">
<Quiz>
  <Question type="multiple_choice" selectionMode="single" title="What is the primary purpose of **distributed tracing** in a microservices architecture?">
    <Option correct="false" helpText="This is the purpose of **metrics**, not tracing.">To measure CPU and memory usage across all services.</Option>
    <Option correct="true" helpText="Distributed tracing shows the complete path of a request through multiple services, helping identify bottlenecks.">To visualize the path and timing of requests as they flow through multiple services.</Option>
    <Option correct="false" helpText="Logs provide event details, but tracing shows the request flow.">To store detailed logs of every event in the system.</Option>
    <Option correct="false" helpText="Security monitoring is a separate concern from tracing.">To detect security vulnerabilities in the application code.</Option>
  </Question>

  <Question type="multiple_choice" selectionMode="single" title="Which metric type would you use to track the **total number of API requests** received?">
    <Option correct="true" helpText="A counter only increases over time, perfect for tracking cumulative totals.">Counter</Option>
    <Option correct="false" helpText="Gauges track values that can increase or decrease, like current memory usage.">Gauge</Option>
    <Option correct="false" helpText="Histograms show distributions of values, like response time percentiles.">Histogram</Option>
    <Option correct="false" helpText="Summaries are similar to histograms but calculated differently.">Summary</Option>
  </Question>
</Quiz>
</Activity>

<Activity type="content" title="Conclusion">

<Highlight title="Key takeaways">

Effective monitoring requires a balanced approach using metrics, logs, and traces.  
Well-designed alerts focus on actionable symptoms that require human intervention.  
The goal is visibility without noise - catching real issues while avoiding alert fatigue.

</Highlight>

</Activity>