---
title: SRE Metrics That Matter
description: Defining and measuring reliability indicators for modern systems
date: 2024-09-18
author: Prince Praveen
---

# SRE Metrics That Matter

Modern systems don’t fail because of lack of dashboards — they fail because teams measure the wrong things. CPU usage, memory graphs, and request counts are useful, but they don’t tell you if your users are actually experiencing reliability. This is where Site Reliability Engineering (SRE) changes the game. SRE is not about more metrics — it’s about meaningful metrics tied to user experience and business impact.

## 🎯 The Shift: From System Metrics to User-Centric Reliability

Traditional monitoring asks: Is the system up? Are servers healthy? SRE asks: Can users successfully complete actions? How fast is their experience? How often do we fail them? This shift introduces a powerful framework: SLI → SLO → Error Budget.

## 📏 Service Level Indicators (SLIs)

SLIs are quantitative measures of user experience. They answer: “What does reliability look like for this service?”

Common SLI categories include:
- Availability: Percentage of successful requests (e.g., successful_requests / total_requests)
- Latency: How fast responses are (e.g., p95 request duration)
- Throughput: Requests per second
- Correctness: Business success metrics (e.g., successful payments)

Example API SLI definition:

    service: checkout-api
    slis:
      availability:
        query: success_rate(http_requests_total)
        target: 99.9%
      latency:
        query: p95(http_request_duration_seconds)
        target: < 300ms
      correctness:
        query: successful_payments / total_payments
        target: 99.99%

## 🎯 Service Level Objectives (SLOs)

SLOs define the reliability target for each SLI. They answer: “How reliable is reliable enough?” For example: 99.9% availability over 30 days, or 95% of requests under 300ms. 100% reliability is unrealistic and expensive; trade-offs must be made between engineering effort and business value.

## 💥 Error Budgets

An error budget is the allowed failure. If your SLO is 99.9% uptime, then 0.1% failure is acceptable. Over 30 days, this translates to approximately 43 minutes of downtime.

Error budgets create alignment:
- If the budget is healthy → ship features faster
- If the budget is burning fast → prioritize reliability
- If the budget is exhausted → freeze risky deployments

Error budgets turn reliability into a governance mechanism rather than just a metric.

## 📡 The Four Golden Signals

Defined in Google’s Site Reliability Engineering practices, these signals help detect most production issues effectively:
- Latency: Time taken to serve a request (track p50, p95, p99)
- Traffic: Demand on your system (requests per second)
- Errors: Rate of failed requests (HTTP 5xx, exceptions)
- Saturation: Resource capacity (CPU, memory, queue depth)

## 🔥 Burn Rate Alerting

Burn rate measures how fast you are consuming your error budget.

Example: If your SLO is 99.9% over 30 days (0.1% error budget) and you see 2% errors in one hour, that is a critical burn rate scenario.

Multi-window alerting strategy:
- 5 minutes: High sensitivity to catch fast failures
- 1 hour: Detect sustained issues
- 6+ hours: Identify slow degradation

Example Prometheus alert:

    groups:
    - name: sre-burn-rate
      rules:
      - alert: HighBurnRate
        expr: error_rate_5m > (14.4 * error_budget)
        for: 2m
        labels:
          severity: critical

## ⚡ Latency Objectives by User Journey

Not all requests are equal. A homepage load is different from a checkout flow or a background job.

Define SLOs per journey:
- Login: < 200ms
- Checkout: < 300ms
- Search: < 500ms

This ensures you optimize what actually impacts user experience and business outcomes.

## 🧭 Designing Effective SLOs

Good SLOs are:
- User-centric (based on real interactions)
- Measurable (derived from observable data)
- Realistic (not perfection-driven)
- Actionable (drive decisions)

Avoid common anti-patterns:
- 100% uptime targets
- Measuring only infrastructure metrics
- Too many SLIs (signal dilution)
- No enforcement of error budgets

## 🚀 Practical Rollout Plan

Phase 1: Identify critical user journeys such as login, checkout, APIs, and core business flows.

Phase 2: Define SLIs using existing telemetry systems like Prometheus, OpenTelemetry, and logs.

Phase 3: Set initial SLOs conservatively (e.g., 99%) and iterate.

Phase 4: Implement error budget policies to guide release and reliability decisions.

Phase 5: Introduce burn rate alerts with multi-window strategies to avoid noise.

Phase 6: Align with business by mapping reliability metrics to revenue, conversion rates, and SLA penalties.

## 🔗 Tooling Ecosystem

Common tools used in SRE practices:
- Prometheus for metrics collection
- Grafana for visualization
- Jaeger for distributed tracing
- Loki for log aggregation

## 🧠 Final Thoughts

SRE metrics are not about visibility — they are about decision-making. If your metrics don’t help you decide whether to ship, scale, or fix reliability issues, they are just noise.

Focus on SLIs that reflect user experience, SLOs that enforce discipline, and error budgets that drive behavior. That’s how reliability becomes a feature, not an afterthought.

## ✍️ Key Takeaway

Measure what users feel. Define what “good” looks like. Enforce it with discipline. That’s what makes SRE metrics actually matter.