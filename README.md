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
Yes. These are exactly the kinds of topics you should have in your **Dynatrace L2/Production Support notes**. One important point: the Dynatrace UI changes between **Classic and current/latest Dynatrace**, so I’ll give you the concept plus the current workflow and note where older menu names may differ.

# Dynatrace Operations Notes

## 1. OneAgent Installation

### What is OneAgent?

**Dynatrace OneAgent** is the software agent installed on a monitored host. It collects telemetry from the host, processes, applications, services, and supported technologies and sends that information to Dynatrace.

Think:

```text
Server
   │
   └── Dynatrace OneAgent
            │
            ├── Host metrics
            ├── Process information
            ├── Application data
            ├── Service data
            └── Distributed tracing
                    │
                    ↓
              Dynatrace
```

Dynatrace currently provides OneAgent installation for platforms including **Windows, Linux and AIX**. ([Dynatrace Documentation][1])

---

# 2. OneAgent Installation — Windows Example

Typical workflow:

```text
Dynatrace
   ↓
Discovery & Coverage
   ↓
Install
   ↓
Install OneAgent
   ↓
Select Windows
   ↓
Select Monitoring Mode
   ↓
Download Installer
   ↓
Install on Server
   ↓
Check Deployment Status
   ↓
Restart monitored applications/processes
   ↓
Host appears in Dynatrace
```

The current Dynatrace documentation uses:

**Discovery & Coverage → Install → Install OneAgent**

and lets you choose the monitoring mode during installation. ([Dynatrace Documentation][2])

### Important prerequisite

The server must be able to communicate with the Dynatrace environment, and you generally need administrator privileges to install OneAgent. ([Dynatrace Documentation][1])

---

# 3. Monitoring Modes

You mentioned:

> Monitoring → Full Stack, Infra, Discovery

Correct. These are important.

### Full-Stack Monitoring

Provides the deepest level of application and infrastructure visibility.

Think:

```text
Host
 ↓
Processes
 ↓
Services
 ↓
Applications
 ↓
Requests
 ↓
Distributed traces
```

Use this when you need **application + infrastructure observability**.

---

### Infrastructure Monitoring

Focuses primarily on infrastructure/host-level visibility.

For example:

```text
CPU
Memory
Disk
Network
Host health
Processes
```

It is useful when you don't need the full application-level instrumentation.

---

### Discovery

Provides a lighter discovery-oriented view of your environment.

A simple way to remember:

```text
Full-Stack       → Deep application + infrastructure
Infrastructure   → Infrastructure focused
Discovery        → Discover what exists
```

Dynatrace supports these three OneAgent monitoring modes, and the mode can be changed after installation. ([Dynatrace Documentation][3])

---

# 4. OneAgent installed but not showing in Deployment Status

This is a **very realistic L2 troubleshooting scenario**.

First understand the distinction:

```text
Installer executed
       ↓
OneAgent installed
       ↓
OneAgent service running
       ↓
OneAgent communicates with Dynatrace
       ↓
Host appears in Dynatrace
```

If it isn't appearing, don't immediately assume the installation failed.

### Troubleshooting checklist

**1. Check OneAgent service**

On Windows:

```text
Services
   ↓
Dynatrace OneAgent
   ↓
Running?
```

You can also restart it.

Dynatrace documents the Windows service as **Dynatrace OneAgent**. ([Dynatrace Documentation][4])

**2. Check network connectivity**

Verify that the server can communicate with the Dynatrace environment/ActiveGate according to your architecture.

**3. Check installation logs**

Look for installation/OneAgent errors.

**4. Check proxy/firewall**

A proxy or firewall can prevent the agent from communicating with Dynatrace.

**5. Restart OneAgent**

For Windows, the service can be restarted from Services or command line.

```cmd
net stop "Dynatrace OneAgent"
net start "Dynatrace OneAgent"
```

Dynatrace also supports `oneagentctl` for configuration and restart operations. ([Dynatrace Documentation][4])

**6. Check Deployment Status again**

Then verify:

```text
Infrastructure & Operations
        ↓
Hosts
        ↓
Search hostname
```

---

# 5. Why do we restart application processes after OneAgent installation?

This is **very important**.

Installing OneAgent doesn't automatically mean every already-running application process is immediately instrumented.

Dynatrace states that processes running during installation need to be restarted for OneAgent monitoring/injection to take effect. ([Dynatrace Documentation][1])

Example:

```text
10:00 → OneAgent installed
10:01 → Java application already running
10:02 → OneAgent is installed but Java process wasn't restarted
```

You may see host-level information such as:

```text
CPU
Memory
Disk
```

but application-level visibility can be limited.

After:

```text
Restart Java application
```

Dynatrace can instrument the process and provide deeper visibility.

---

# 6. Finding Hosts in Dynatrace

You mentioned the older:

> Search → Host

and newer:

> Infrastructure & Operations → Show all hosts

The exact navigation depends on the Dynatrace version/UI.

The important concept is:

```text
Infrastructure & Operations
        ↓
Hosts
        ↓
Search / Filter
        ↓
Host
```

Dynatrace's current documentation uses **Infrastructure & Operations → Hosts** to confirm a newly connected OneAgent host. ([Dynatrace Documentation][1])

---

# 7. Renaming a Host

There are two concepts you should distinguish:

### Actual OS hostname

This is the hostname configured in Windows/Linux.

### Dynatrace custom host name

Dynatrace allows you to override the name displayed in Dynatrace.

For example:

```text
Actual hostname:
WINPROD123

Dynatrace display name:
TAX-PROD-WEB-01
```

Using OneAgent CLI:

### Windows

```cmd
.\oneagentctl.exe --set-host-name=TAX-PROD-WEB-01
```

### Linux

```bash
./oneagentctl --set-host-name=TAX-PROD-WEB-01
```

Dynatrace notes that this changes the name displayed in Dynatrace; it does **not** change the actual OS hostname. A OneAgent restart is required for the change to take effect. ([Dynatrace Documentation][5])

---

# 8. Dynatrace Tagging

Tags are extremely important in enterprise environments.

They allow you to organize and filter:

```text
Hosts
Process Groups
Services
Applications
```

For example:

```text
Environment:Production
Application:Tax
Team:Tax-Platform
Criticality:High
Region:India
```

Then you can use these tags for:

* Searching
* Filtering
* Dashboards
* Alerting
* Maintenance windows
* Management zones
* Operational organization

Dynatrace supports both **manual and automatic tagging**. ([Dynatrace Documentation][6])

---

# 9. Manual vs Automatic Tagging

## Manual Tagging

You select an entity and manually assign a tag.

Example:

```text
Host: TAX-PROD-01

Tags:
Environment:Production
Application:Tax
```

Good for:

> Small/static environments.

---

## Automatic Tagging

Instead of manually tagging every host, you create a **rule**.

Example:

```text
IF

Host name contains "PROD"

THEN

Add tag:
Environment:Production
```

Another example:

```text
IF

Host property:
Environment = Production

THEN

Tag:
Environment:Production
```

This is much better for large dynamic environments.

Dynatrace specifically recommends automatic/rule-based tagging where environments are large or dynamic. ([Dynatrace Documentation][6])

---

# 10. How to Set Up Automatic Tagging

Current Dynatrace workflow:

```text
Settings
   ↓
Tags
   ↓
Automatically applied tags
   ↓
Create tag
   ↓
Enter Tag Name
   ↓
Add new rule
   ↓
Define condition
   ↓
Save
```

Example:

```text
Tag Name:
Environment

Rule:

Host name
contains
PROD

Value:
Production
```

Result:

```text
TAX-PROD-01 → Environment:Production
TAX-PROD-02 → Environment:Production
TAX-PROD-03 → Environment:Production
```

New matching entities receive the tag automatically. ([Dynatrace Documentation][6])

---

# 11. Process Group Monitoring

This is another **very important Dynatrace concept**.

A server can have many processes:

```text
Windows Server
│
├── Java
├── IIS
├── SQL Server
├── PowerShell
├── Windows Services
└── Other processes
```

Dynatrace groups related process instances into **Process Groups**.

Example:

```text
Process Group
    │
    ├── Java Instance 1
    ├── Java Instance 2
    └── Java Instance 3
```

This allows Dynatrace to analyze applications at the process/service level.

OneAgent automatically monitors detected process groups, particularly known technologies or significant processes. ([Dynatrace Documentation][7])

---

# 12. Why Process Group Monitoring matters

Imagine:

```text
Host: TAX-PROD-01

Process Groups:

Tax-Web
Tax-API
Tax-Database
```

If:

```text
Tax-API
   ↓
Response time ↑
   ↓
Error rate ↑
```

you can investigate the process group and its dependencies rather than just looking at the server's overall CPU.

---

# 13. Process Availability

This answers:

> **"Is an important process actually running?"**

Example:

You have a critical Windows service:

```text
TaxApplicationService
```

You expect:

```text
TaxApplicationService = Running
```

If the process disappears/stops:

```text
Process unavailable
       ↓
Dynatrace alert
       ↓
Support team
       ↓
Incident
```

Dynatrace allows you to create process-availability monitoring rules. If no matching process exists, an alerting event can be generated. ([Dynatrace Documentation][8])

### Example rule

```text
Rule:
TaxApplicationService

Minimum matching processes:
1
```

If:

```text
Running processes = 1
```

→ OK

If:

```text
Running processes = 0
```

→ Alert

---

# 14. Process Group Availability

There is another useful scenario.

Suppose you have:

```text
Tax API

Instance 1
Instance 2
Instance 3
```

You might configure:

> Alert if fewer than **2 instances** are available.

So:

```text
3 → OK
2 → OK
1 → ALERT
0 → ALERT
```

Dynatrace supports availability alerting based either on a process becoming unavailable or the number of available processes falling below a configured threshold. ([Dynatrace Documentation][9])

---

# 15. Maintenance Windows

This is extremely important for production support.

Suppose the application team tells you:

> "We are deploying a new release from 1 AM to 3 AM."

During this period you may expect:

```text
Application restart
High CPU
High response time
Temporary errors
Services unavailable
```

You don't want normal maintenance activity generating unnecessary incidents/alerts.

Therefore:

> **Maintenance Window = predefined period during which planned maintenance is taking place.**

---

## Creating a Maintenance Window

Current Dynatrace UI:

```text
Settings
   ↓
Maintenance windows
   ↓
Monitoring, alerting, and availability
   ↓
Create maintenance window
```

You define:

```text
Name
Description
Planned / Unplanned
Start time
End time
Recurrence
Timezone
Scope
```

Dynatrace lets you choose what happens to problem detection during the window:

### Option 1 — Detect + Alert

Normal detection and alerting continue.

### Option 2 — Detect but don't Alert

Dynatrace detects the problem but suppresses notifications.

### Option 3 — Disable Problem Detection

Problems aren't detected during the maintenance window.

([Dynatrace Documentation][10])

---

# 16. Why Tags + Maintenance Windows are powerful

Suppose you have:

```text
100 Production servers
```

and your tax application servers have:

```text
Application:Tax
Environment:Production
```

You can create a maintenance window scoped to:

```text
Tag:
Application:Tax

AND

Environment:Production
```

Then you don't have to manually select 100 servers.

This is one reason **good tagging strategy is important in enterprise Dynatrace administration**. Maintenance windows can be scoped using entity tags and management zones. ([Dynatrace Documentation][10])

---

# 17. Metric Event / New Alert

A **metric event** is essentially a rule that monitors a metric and generates an alert when a defined condition is met.

Example:

```text
Metric:
CPU utilization

Condition:
> 90%

Duration:
5 minutes

Action:
Generate alert
```

Conceptually:

```text
CPU
 ↓
85%
 ↓
91%
 ↓
94%
 ↓
92%
 ↓
Threshold breached
 ↓
Metric Event
 ↓
Problem / Alert
```

Dynatrace's metric key events use incoming measurements of a metric and can evaluate them against static thresholds. ([Dynatrace Documentation][11])

---

# 18. Example: Create CPU Alert

Suppose you want:

> Alert when CPU utilization exceeds 90%.

Conceptually configure:

```text
Metric:
CPU utilization

Aggregation:
Average

Threshold:
90%

Condition:
Above threshold

Scope:
Production hosts
```

Then:

```text
CPU = 75% → No alert

CPU = 85% → No alert

CPU = 92% → Alert
```

In an enterprise environment, you would normally **scope the alert carefully** rather than applying every alert to every entity.

---

# 19. Important distinction: Alert vs Problem vs Event

This is worth putting in your notes.

```text
Metric
  ↓
Threshold/Anomaly detected
  ↓
Event
  ↓
Dynatrace correlation
  ↓
Problem
  ↓
Notification/Alert
```

A useful operational mental model is:

**Event** = something happened.

**Problem** = Dynatrace correlates one or more events into a broader issue.

**Alert/notification** = something that gets communicated to the responsible team according to configured alerting.

---

# Your Dynatrace L2 Cheat Sheet

```text
ONEAGENT
   ↓
Installed on Host
   ↓
Monitoring Mode
   ├── Full-Stack
   ├── Infrastructure
   └── Discovery
   ↓
Host appears in Dynatrace
   ↓
Processes detected
   ↓
Process Groups
   ↓
Services / Applications
   ↓
Metrics + Logs + Traces + Events
   ↓
Problems / Alerts
```

### Operational commands/concepts to remember

| Topic                    | Remember                                      |
| ------------------------ | --------------------------------------------- |
| **OneAgent**             | Agent installed on monitored host             |
| **Full-Stack**           | Deep application + infrastructure monitoring  |
| **Infrastructure**       | Infrastructure-focused monitoring             |
| **Discovery**            | Lightweight environment discovery             |
| **Deployment Status**    | Check OneAgent connection/deployment          |
| **Host**                 | Physical/VM/cloud machine being monitored     |
| **Process Group**        | Logical grouping of related process instances |
| **Process Availability** | Is a required process running?                |
| **Manual Tag**           | Human assigns tag                             |
| **Automatic Tag**        | Rule assigns tag                              |
| **Maintenance Window**   | Planned period controlling detection/alerting |
| **Metric Event**         | Metric condition that can generate an event   |
| **Problem**              | Correlated issue derived from events          |

**One operational correction to your note:** don't make "restart OneAgent if it isn't showing in Deployment Status" the only troubleshooting step. A better L2 sequence is **service status → connectivity/proxy/firewall → OneAgent logs → OneAgent restart → Deployment Status → Hosts**, because a restart won't fix a network or configuration problem. Dynatrace's installation/troubleshooting guidance also emphasizes connectivity and process restart requirements. ([Dynatrace Documentation][12])

[1]: https://docs.dynatrace.com/docs/ingest-from/dynatrace-oneagent/installation-and-operation?utm_source=chatgpt.com "Install OneAgent on a server — Dynatrace Docs"
[2]: https://docs.dynatrace.com/docs/ingest-from/dynatrace-oneagent/installation-and-operation/windows/installation/install-oneagent-on-windows?trk=article-ssr-frontend-pulse_little-text-block&utm_source=chatgpt.com "Install OneAgent on Windows — Dynatrace Docs"
[3]: https://docs.dynatrace.com/docs/ingest-from/dynatrace-oneagent/installation-and-operation/windows/installation/customize-oneagent-installation-on-windows?utm_source=chatgpt.com "Customize OneAgent installation on Windows — Dynatrace Docs"
[4]: https://docs.dynatrace.com/docs/ingest-from/dynatrace-oneagent/installation-and-operation/windows/operation/stop-restart-oneagent-on-windows?utm_source=chatgpt.com "Stop/restart OneAgent on Windows — Dynatrace Docs"
[5]: https://docs.dynatrace.com/docs/ingest-from/dynatrace-oneagent/oneagent-configuration-via-command-line-interface?utm_source=chatgpt.com "OneAgent configuration via command-line interface — Dynatrace Docs"
[6]: https://docs.dynatrace.com/docs/manage/tags-and-metadata/setup/how-to-define-tags?utm_source=chatgpt.com "Define and apply tags — Dynatrace Docs"
[7]: https://docs.dynatrace.com/docs/observe/infrastructure-observability/process-groups/configuration/pg-monitoring?utm_source=chatgpt.com "Process deep monitoring — Dynatrace Docs"
[8]: https://docs.dynatrace.com/docs/observe/infrastructure-observability/hosts/monitoring/process-availability?utm_source=chatgpt.com "Process availability — Dynatrace Docs"
[9]: https://docs.dynatrace.com/docs/observe/infrastructure-observability/process-groups/monitoring/process-group-availability-monitoring-and-alerting?utm_source=chatgpt.com "Process group availability monitoring and alerting — Dynatrace Docs"
[10]: https://docs.dynatrace.com/docs/analyze-explore-automate/notifications-and-alerting/maintenance-windows/define-maintenance-window?utm_source=chatgpt.com "How to define a maintenance window — Dynatrace Docs"
[11]: https://docs.dynatrace.com/docs/dynatrace-intelligence/anomaly-detection/metric-events/metric-key-events?utm_source=chatgpt.com "Metric key events — Dynatrace Docs"
[12]: https://docs.dynatrace.com/docs/ingest-from/dynatrace-oneagent/oneagent-troubleshooting/troubleshoot-oneagent-installation?utm_source=chatgpt.com "Troubleshooting OneAgent installation — Dynatrace Docs"
