# Dynatrace — Fundamentals

## 1. What is Dynatrace?

**Dynatrace is an Application Performance Monitoring (APM) and observability platform** used to monitor applications, infrastructure, user experience, services, databases, and the dependencies between them.

In simple words:

> **Dynatrace helps us understand whether an application is working properly, how well it is performing, and why a problem is happening.**

For example, suppose users report:

> "The tax-filing application is very slow."

Dynatrace can help us investigate:

**User → Application → Service → API → Database → Infrastructure**

and identify where the problem is occurring.

It can provide information such as:

* Response time
* Error rate
* Request rate
* Application health
* Service dependencies
* Database performance
* Infrastructure utilization
* User experience
* Distributed traces
* Logs/events
* Anomalies
* Root-cause relationships

---

# 2. Why do we need Dynatrace?

Modern applications are no longer a single server.

A production application might look like:

```text
User
 ↓
Web Application
 ↓
API Gateway
 ↓
Microservice A
 ↓
Microservice B
 ↓
Database
 ↓
External API
```

There could also be:

```text
AWS
 ├── EC2
 ├── Containers
 ├── Kubernetes
 ├── Load Balancer
 └── RDS
```

If something becomes slow, simply checking whether a server is "UP" isn't enough.

Dynatrace helps answer:

### What happened?

Example:

> API response time increased from 500 ms → 5 seconds.

### Where did it happen?

> Payment Service → Database query.

### Why did it happen?

> Database query latency increased because of a particular query/load condition.

### What was the impact?

> 18% of requests experienced elevated latency.

This is particularly useful for **production support, incident management, problem management and RCA**.

---

# 3. Monitoring vs Observability

These two terms are related but **not identical**.

### Monitoring

Monitoring generally means:

> **Watching predefined metrics/conditions and detecting when something goes wrong.**

Example:

```text
CPU > 90%
Memory > 85%
Disk > 90%
HTTP Errors > 5%
Response Time > 2 seconds
```

You configure a threshold:

```text
CPU > 90%
     ↓
Alert
     ↓
Support Team
```

Monitoring answers:

> **"Is something wrong?"**

---

### Observability

Observability goes deeper.

It is the ability to understand the **internal state and behavior of a system from its external outputs/data**.

The traditional three pillars are:

```text
        Observability
             │
    ┌────────┼────────┐
    ↓        ↓        ↓
 Metrics    Logs    Traces
```

For example:

**Metrics**

```text
API latency = 4.5 seconds
```

**Logs**

```text
Database connection timeout
```

**Trace**

```text
User Request
    ↓
API
    ↓
Service A
    ↓
Service B
    ↓
Database ← 4 sec delay
```

Observability helps answer:

> **"Why is something wrong?"**

### Easy way to remember

**Monitoring = Detect**

**Observability = Investigate + Understand**

Monitoring tells you:

> "The application is slow."

Observability helps you determine:

> "The application is slow because Service B is waiting on a database query."

---

# 4. Dynatrace vs Prometheus vs Grafana

These tools can work **together**, but they have different primary purposes.

| Tool           | Primary Purpose                 |
| -------------- | ------------------------------- |
| **Dynatrace**  | Full-stack observability/APM    |
| **Prometheus** | Metrics collection & monitoring |
| **Grafana**    | Visualization & dashboards      |

### Prometheus

Prometheus is primarily a **metrics monitoring and alerting system**.

It collects numerical time-series data such as:

```text
CPU usage
Memory
HTTP requests
Request latency
Container metrics
Kubernetes metrics
```

Example:

```text
http_requests_total = 125000
cpu_usage = 72%
```

Prometheus is particularly common in **cloud-native/Kubernetes environments**.

---

### Grafana

Grafana is primarily a **visualization and dashboarding platform**.

It can take data from sources such as:

```text
Prometheus
Loki
Elasticsearch
InfluxDB
CloudWatch
```

and turn it into dashboards:

```text
┌─────────────────────────────┐
│ CPU       ███████░░ 72%     │
│ Memory    ██████░░░ 61%     │
│ Requests  12,500/sec        │
│ Latency   320 ms            │
└─────────────────────────────┘
```

So:

> **Prometheus collects/stores metrics → Grafana visualizes them.**

---

# 5. Where Dynatrace fits

Think about the difference like this:

```text
                 PRODUCTION SYSTEM
                        │
       ┌────────────────┼────────────────┐
       ↓                ↓                ↓
   Prometheus         Dynatrace        Logs
       │                │
       ↓                ↓
    Metrics       Full-stack
       │          Observability
       ↓                │
    Grafana             ↓
   Dashboard      Root-cause analysis
```

Dynatrace provides a broader view across:

**User → Application → Services → Infrastructure → Database → Dependencies**

while Prometheus is heavily focused on **metrics**, and Grafana is primarily focused on **visualizing data**.

---

## 6. One practical incident example

Imagine users complain:

> **"The application is taking 10 seconds to load."**

### Prometheus

You might discover:

```text
CPU: 85%
Memory: 75%
API latency: 8 sec
```

Good for identifying that something abnormal is happening.

### Grafana

You visualize:

```text
CPU ────────────────╮
                    ╰── 85%

Latency ────────────╮
                    ╰── 8 sec
```

Good for dashboards and trends.

### Dynatrace

You can investigate the transaction:

```text
User Request
      ↓
Frontend
      ↓
API
      ↓
Service A
      ↓
Service B
      ↓
Database
      ↓
Slow Query
```

and correlate the dependencies to help identify the likely **root cause**.

---

# 7. Interview-friendly summary

> **Dynatrace is an enterprise observability and application performance monitoring platform that provides visibility across applications, services, infrastructure, user experience, and dependencies. Monitoring primarily focuses on detecting predefined problems using metrics and alerts, whereas observability provides deeper context to understand why a problem occurred. Prometheus is primarily a metrics collection and monitoring system, while Grafana is primarily used for visualization and dashboards. Dynatrace provides a broader full-stack observability capability and can also work alongside tools such as Prometheus and Grafana.**

### Remember this:

**Dynatrace → Observe & investigate**

**Prometheus → Collect & monitor metrics**

**Grafana → Visualize data**

**Monitoring → "Something is wrong."**

**Observability → "Why is it wrong?"**

Yes — this is one of the **most important observability concepts** to understand before going deeper into Dynatrace.

# Logs vs Metrics vs Traces

Think of an application like a **car**:

* **Metrics** = dashboard gauges
* **Logs** = detailed event/history records
* **Traces** = the journey of one particular request

---

## 1. Metrics

**Metrics are numerical measurements collected over time.**

They tell us **what is happening** and help identify trends or abnormal behavior.

Examples:

```text
CPU Usage       = 82%
Memory Usage    = 71%
Request Rate    = 1,200 req/sec
Error Rate      = 3%
Response Time   = 850 ms
Disk Usage      = 91%
```

A metric generally has:

**Metric name + value + timestamp + dimensions/labels**

Example:

```text
http_requests_total{service="payment"} = 125000
```

### Metrics answer:

> **"How much?" / "How often?" / "Is something abnormal?"**

### Example

If your application normally has:

```text
Response time = 300 ms
```

and suddenly:

```text
Response time = 5,000 ms
```

the metric tells you that performance has degraded.

---

# 2. Logs

**Logs are timestamped records of events generated by applications, servers, or other components.**

Example:

```text
2026-09-26 04:10:21
INFO
User authentication successful
userId=12345
```

Another example:

```text
2026-09-26 04:11:05
ERROR
Database connection timeout
database=CustomerDB
timeout=30s
```

Logs usually contain much more **contextual/detail information** than metrics.

### Logs answer:

> **"What happened?"**

For example:

```text
ERROR: Payment service unable to connect to database.
```

That gives an engineer a much more specific clue than:

```text
Error Rate = 8%
```

---

# 3. Traces

A **trace represents the journey of a single request through a distributed application.**

This becomes extremely important with **microservices**.

Suppose a user opens your application:

```text
User
 ↓
Frontend
 ↓
API Gateway
 ↓
Authentication Service
 ↓
Tax Service
 ↓
Database
 ↓
Response
```

A trace can follow that individual request across all these components.

For example:

```text
Trace ID: ABC123

Frontend             100 ms
   ↓
API Gateway           50 ms
   ↓
Auth Service           80 ms
   ↓
Tax Service           4,500 ms
   ↓
Database               4,200 ms
```

Now we can see:

> The overall request was slow primarily because of the Tax Service/database portion.

### Traces answer:

> **"Where did this particular request spend its time?"**

---

# The easiest comparison

|                       | Metrics                 | Logs                  | Traces                      |
| --------------------- | ----------------------- | --------------------- | --------------------------- |
| **What?**             | Numerical measurements  | Event records         | Request journey             |
| **Example**           | CPU = 80%               | DB connection timeout | Request → API → DB          |
| **Main purpose**      | Detect trends/anomalies | Investigate events    | Find request bottlenecks    |
| **Scope**             | Aggregated              | Individual events     | Individual request          |
| **Question answered** | What's happening?       | What happened?        | Where/why did request slow? |

### Memory trick

> **Metrics = Numbers**
> **Logs = Events**
> **Traces = Journey**

---

# What is MELT?

**MELT** is an observability acronym:

> **M — Metrics**
> **E — Events**
> **L — Logs**
> **T — Traces**

So:

```text
                 MELT
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    Metrics     Events     Logs
                              │
                              ↓
                           Traces
```

However, you'll often hear the observability discussion framed around the **three pillars**:

> **Metrics + Logs + Traces**

MELT expands this by explicitly including **Events**.

---

# 4. What are Events?

An **event** represents something significant that happened in a system.

Examples:

```text
Deployment completed
Server restarted
Configuration changed
Kubernetes pod created
Database failover occurred
Security policy changed
```

Unlike metrics, which continuously measure something:

```text
CPU = 72%
CPU = 74%
CPU = 78%
CPU = 85%
```

an event records a specific occurrence:

```text
10:30 AM → Application deployment started
10:35 AM → Application deployment completed
```

Events are particularly useful when correlating:

**"What changed around the time the incident started?"**

---

# 5. Putting MELT together in a real incident

Imagine your production application suddenly becomes slow.

### Metrics

You see:

```text
Response Time
300 ms → 5 sec
```

**Something is wrong.**

↓

### Events

You discover:

```text
10:30 AM → New application deployment
10:32 AM → Response time increased
```

**Something changed.**

↓

### Traces

You inspect requests:

```text
API
 ↓
Tax Service
 ↓
Database
      ↑
   4.5 sec
```

**You locate the slow component.**

↓

### Logs

You find:

```text
ERROR: Database query timeout
```

**You get detailed evidence about the failure.**

---

## 🔥 This is the key concept for your Dynatrace notes

```text
              PRODUCTION INCIDENT
                     │
                     ↓
                  METRICS
             "Something is wrong"
                     │
                     ↓
                   EVENTS
             "What changed?"
                     │
                     ↓
                  TRACES
          "Where is the problem?"
                     │
                     ↓
                   LOGS
            "What exactly happened?"
```

Together, these signals give you **observability**.

And in Dynatrace, the real power comes from **correlating these signals with the application's topology and dependencies**, rather than looking at each data type in isolation.
