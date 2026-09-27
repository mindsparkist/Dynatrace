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
Yes. This is the next important section for your Dynatrace notes. The key is to understand **why ActiveGate and Private Location exist**, rather than just memorizing the installation clicks.

# Dynatrace Synthetic Monitoring

## 1. What is Synthetic Monitoring?

**Synthetic Monitoring is proactive monitoring where Dynatrace automatically simulates user actions or sends requests to an application/API at scheduled intervals.**

Instead of waiting for a real user to report:

> "The application is down."

Dynatrace can continuously test it:

```text
Every 5 minutes
      ↓
Open application
      ↓
Login
      ↓
Navigate to page
      ↓
Perform action
      ↓
Check response
      ↓
Record result
```

It can measure things such as:

* Availability
* Response time
* Performance
* HTTP/API response
* Browser journey
* Errors
* DNS/network behavior

Dynatrace Synthetic supports HTTP monitors and browser monitors, among other synthetic monitoring capabilities. ([Dynatrace Documentation][1])

### Simple definition for your notes

> **Synthetic Monitoring = Simulating users or requests to proactively test application availability and performance.**

---

# 2. Real User Monitoring vs Synthetic Monitoring

This distinction is important.

### Real User Monitoring — RUM

You monitor **actual users**.

```text
Real User
    ↓
Application
    ↓
Dynatrace
```

Example:

> 5,000 real users experienced 3-second page load time.

---

### Synthetic Monitoring

Dynatrace creates **artificial/simulated requests**.

```text
Synthetic Monitor
       ↓
Application
       ↓
Dynatrace
```

Example:

> Every 5 minutes, Dynatrace logs into the application and checks whether the Tax Filing page works.

### Remember:

**RUM → What are real users experiencing?**

**Synthetic → Can the application work when we test it?**

---

# 3. What is ActiveGate?

**ActiveGate is a Dynatrace component that acts as an intermediary between your environment and the Dynatrace environment.**

Think of it as a **gateway/bridge**.

```text
Your Network
     │
     │
 ActiveGate
     │
     ↓
 Dynatrace
```

ActiveGate is useful when Dynatrace needs to communicate with resources that shouldn't or can't communicate directly with the Dynatrace environment.

Depending on its configuration, ActiveGate can provide different capabilities.

For Synthetic monitoring specifically:

> **A Synthetic-enabled ActiveGate executes Synthetic monitors from your own network.** ([Dynatrace Documentation][1])

---

# 4. What is a Private Location?

This is the most important part.

A **Private Location** is a Synthetic monitoring location inside **your own private/corporate network**.

For example:

```text
                    Dynatrace
                        │
                        │
                  ActiveGate
                        │
             ┌──────────┴──────────┐
             ↓                     ↓
       Internal App            Internal API
       10.x.x.x                 10.x.x.x
```

The ActiveGate executes the Synthetic test from inside your network.

Dynatrace defines a private location as a location in your private network infrastructure where one or more Synthetic-enabled ActiveGates execute monitors. ([Dynatrace Documentation][2])

---

# 5. Why do we need a Private Location?

This is the question you specifically asked:

> **"Why can't I just create Synthetic Monitoring directly?"**

Because your application may **not be accessible from the public internet**.

Imagine your company has:

```text
https://tax-application.company.local
```

or:

```text
10.20.30.40
```

Only employees inside the corporate network/VPN can access it.

A Dynatrace public Synthetic location on the internet cannot reach it.

```text
Public Synthetic Location
        ❌
        ↓
Internal Application
10.20.30.40
```

But:

```text
Corporate Network
       │
       ↓
Synthetic ActiveGate
       │
       ↓
Internal Application
10.20.30.40
       ✅
```

Therefore:

> **Private Synthetic Location allows Synthetic monitors to execute from inside your corporate network.**

Dynatrace explicitly states that private locations are used to monitor applications/endpoints inside corporate networks that are unavailable from the public internet. ([Dynatrace Documentation][3])

---

# 6. Your "Why Private Location?" Example

Suppose your company has:

```text
Tax Application
https://tax.internal.company.com
```

It is accessible only from:

```text
Corporate Network
VPN
Internal DNS
```

You create:

```text
Private Location
       ↓
Synthetic-enabled ActiveGate
       ↓
Browser Monitor
       ↓
https://tax.internal.company.com
```

The ActiveGate is physically/logically inside the network where the application is reachable.

Therefore:

```text
Private Location
       ↓
Can reach internal application
       ↓
Runs test
       ↓
Sends result to Dynatrace
```

---

# 7. What does the Synthetic test actually do?

Suppose you create a **Browser Monitor**:

```text
Tax Filing Application
```

You might configure:

```text
Step 1 → Open URL
Step 2 → Enter username
Step 3 → Enter password
Step 4 → Click Login
Step 5 → Open Tax Return
Step 6 → Verify page
```

Dynatrace runs this periodically.

Example:

```text
12:00 → PASS → 2.1 sec
12:05 → PASS → 2.3 sec
12:10 → PASS → 2.4 sec
12:15 → FAIL → Login timeout
```

Now the support team can investigate the problem **before a large number of real users report it**.

---

# 8. How to install ActiveGate for Synthetic Monitoring

There is an important distinction here:

> **A normal ActiveGate is not simply converted into a Synthetic-enabled ActiveGate.**

For a private Synthetic location, Dynatrace requires a **clean installation specifically for Synthetic monitoring**. A Synthetic-enabled ActiveGate is dedicated to Synthetic execution and doesn't perform the other normal ActiveGate functions. ([Dynatrace Documentation][2])

### High-level workflow

```text
Dynatrace
   ↓
ActiveGate setup
   ↓
Choose OS
   ↓
Choose purpose:
"Run synthetic monitors from a private location"
   ↓
Generate/download installer
   ↓
Install ActiveGate
   ↓
Synthetic module
   ↓
Create/assign Private Location
   ↓
Verify ActiveGate
   ↓
Create Synthetic Monitor
   ↓
Select Private Location
```

The current Classic documentation specifically instructs you to select:

> **Run synthetic monitors from a private location**

during the ActiveGate setup. ([Dynatrace Documentation][2])

---

# 9. ActiveGate Installation — Practical Example

Suppose you're installing on:

```text
Linux Server
10.10.10.50
```

### Step 1

In Dynatrace, go to the ActiveGate installation/setup area.

### Step 2

Select:

```text
Operating System
      ↓
Linux
```

### Step 3

Select the purpose:

```text
Run synthetic monitors from a private location
```

### Step 4

Generate/download the ActiveGate installer.

Dynatrace uses an appropriate token with the required installer-download permissions for the installation flow. ([Dynatrace Documentation][2])

### Step 5

Transfer the installer to the target Linux server.

### Step 6

Run the installation commands provided by Dynatrace.

For example, the current documentation uses an installer with Synthetic enabled; exact commands should always be copied from the Dynatrace UI because installer/version requirements change. ([Dynatrace Documentation][2])

### Step 7

Verify:

```text
Deployment Status
       ↓
ActiveGate
       ↓
Healthy / Connected
```

---

# 10. Important: Synthetic ActiveGate is different from normal ActiveGate

This is a very good interview point.

Normal ActiveGate:

```text
ActiveGate
 ├── Gateway functions
 ├── Extensions
 ├── Remote monitoring
 └── Other capabilities
```

Synthetic-enabled ActiveGate:

```text
Synthetic ActiveGate
        ↓
Synthetic Engine
        ↓
HTTP monitors
Browser monitors
NAM monitors
```

Dynatrace says a Synthetic-enabled ActiveGate is used exclusively to execute Synthetic monitors and disables other ActiveGate features. ([Dynatrace Documentation][1])

---

# 11. Why does Synthetic ActiveGate need a browser?

For a **browser monitor**, Dynatrace needs an actual browser engine to simulate a user's browser interaction.

Conceptually:

```text
Synthetic Monitor
       ↓
Browser Engine
       ↓
Open website
       ↓
Click
       ↓
Type
       ↓
Navigate
       ↓
Validate
```

Current Dynatrace Synthetic-enabled ActiveGate installations use **Chrome for Testing** on supported platforms/versions; exact OS/browser requirements vary by ActiveGate version. ([Dynatrace Documentation][4])

That's why Synthetic ActiveGate has higher resource requirements than a normal ActiveGate.

---

# 12. Now your question about the Dynatrace UI

You mentioned:

> **Search → Synthetic Monitoring (Classic) → setup Synthetic Monitoring from private location**

The important thing isn't the word **Classic**.

The important architecture is:

```text
Synthetic Monitoring
        ↓
Where should the test execute?
        ↓
Public Location OR Private Location
```

### Public location

```text
Dynatrace Public Synthetic Location
             ↓
        Your website
```

Useful for publicly accessible applications.

### Private location

```text
Your Corporate Network
        ↓
Synthetic ActiveGate
        ↓
Your Internal Application
```

Useful for internal applications.

---

# 13. Why select Private Location when creating the monitor?

Suppose:

```text
Application:
https://tax.company.com
```

If it's publicly accessible, you could run:

```text
New York
London
Singapore
Mumbai
```

from Dynatrace public locations.

But suppose:

```text
Application:
https://tax-internal.company.local
```

Only the corporate network can resolve/reach it.

Then:

```text
Public Location
       ↓
       ❌ Cannot reach

Private Location
       ↓
Synthetic ActiveGate
       ↓
       ✅ Internal application
```

So when creating the Synthetic monitor, you select your **Private Location**.

---

# 14. Multiple ActiveGates in one Private Location

You can have:

```text
Private Location: Hyderabad DC

        ┌──────────────┐
        ↓              ↓
   ActiveGate 1    ActiveGate 2
        │              │
        └──────┬───────┘
               ↓
       Synthetic Monitors
```

This is useful for **capacity, load distribution and resilience/failover**.

Dynatrace recommends at least two ActiveGates for a private location in current documentation, and a single ActiveGate can only be assigned to one private location. ([Dynatrace Documentation][3])

---

# 15. Complete Architecture — Remember This

This is probably the most useful diagram for your notes:

```text
                    DYNATRACE
                       │
                       │
                Synthetic Monitoring
                       │
             ┌─────────┴─────────┐
             │                   │
       Public Location      Private Location
             │                   │
             │             Synthetic ActiveGate
             │                   │
             │             Corporate Network
             │                   │
             │            ┌──────┴───────┐
             │            ↓              ↓
             │       Internal Web     Internal API
             │
             ↓
       Public Website
```

### The core relationship

> **Synthetic Monitoring = What we want to test**

> **Location = Where we want to run the test**

> **ActiveGate = The component that executes private Synthetic tests from our network**

> **Private Location = A logical/geographical location inside our network containing Synthetic-enabled ActiveGate(s)**

---

## 🔥 Interview answer: "Why do we need a Private Location?"

> **A private Synthetic location is required when the application or endpoint cannot be reached from Dynatrace's public Synthetic locations, such as an internal corporate application, private API, or intranet application. A Synthetic-enabled ActiveGate is deployed inside the corporate network and executes the Synthetic monitor from that network, allowing Dynatrace to test internal applications for availability and performance.** ([Dynatrace Documentation][1])

### One-line memory trick

**Public application → Public Synthetic Location**

**Private/internal application → Private Synthetic Location + Synthetic ActiveGate**

That distinction is the key to understanding the whole setup.

[1]: https://docs.dynatrace.com/docs/ingest-from/dynatrace-activegate/capabilities/synthetic-purpose?utm_source=chatgpt.com "Execute synthetic monitors from private locations — Dynatrace Docs"
[2]: https://docs.dynatrace.com/docs/observe/digital-experience/synthetic-monitoring/private-synthetic-locations/create-a-private-synthetic-location?utm_source=chatgpt.com "Create a private Synthetic location in Classic — Dynatrace Docs"
[3]: https://docs.dynatrace.com/docs/observe/digital-experience/synthetic/synthetic-app/private-locations?utm_source=chatgpt.com "Private Synthetic locations — Dynatrace Docs"
[4]: https://docs.dynatrace.com/docs/observe/digital-experience/synthetic-monitoring/private-synthetic-locations/system-and-hardware-requirements-for-private-synthetic?utm_source=chatgpt.com "Requirements for private Synthetic locations in Classic — Dynatrace Docs"

Yes — this is an important distinction in Dynatrace. Think of **RUM = actual people** and **Synthetic = simulated people**.

![Image](https://images.openai.com/static-rsc-4/fGEoThl9oidQNedVKs9_dpn6ZpJ_coz3qjLeGHnQRJ6FHthnMxbfHjk743f8OxuAzLZzpp6Fqrjwj5mPDUjFmc0A-ygtUjJVOjikkQDspvhrRBbN6rs1Q-gMpLTqxVvOUlIJWiA_LEHAv7eiZGc_i3GOK0QZ2B8bBlL597VO9_tCbIbW2gq7WXXU1UwWvYr8?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/LT6cOC-fjIYr9vFzeqkXtqBWj7uodV_G_JkKnmELzVq7fRqdY5uYKGcA_wPK0TWdzxEEp3YXRzdLVmOz4xEqr0jj3sGMiUDKFv_cvYUDivzhUgPWMRRS1gpN_JcL99X_nsi2yuOvHofdekQ5HfrzTaU3Sk6IXbWroZE13vY2DX5ahac5wgeMh2FZJcz6wbeO?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/UBniY6cvhVELqMeSxoD-ab9pMUILYIpkq2gNFJHoe4Ud8CDdlOr_2GakXYqIdp9RWVEDOtmiC4naL-y35aPFj2mMOTi2bU7-kRZjN3Tz7LQ_CRQDvg0xUwZ-6gfCCuHTUe89N7wOl10zQppdw1cVp1fjygNwh_3xdQkp1BmmJ5Yp-fb4HozZUcbUGOA5yOg0?purpose=fullsize)

## Real User Monitoring vs Synthetic User

|                                                 | **Real User Monitoring (RUM)**                    | **Synthetic Monitoring**                            |
| ----------------------------------------------- | ------------------------------------------------- | --------------------------------------------------- |
| **Who generates traffic?**                      | Actual users                                      | Automated/simulated users                           |
| **When does it run?**                           | When users actually use the application           | On a predefined schedule                            |
| **Purpose**                                     | Understand actual user experience                 | Proactively test availability/performance           |
| **Requires real users?**                        | Yes                                               | No                                                  |
| **Example**                                     | Customer logs in and completes a tax return       | Dynatrace automatically tests login every 5 minutes |
| **Can detect issues when nobody is using app?** | ❌ Not necessarily                                 | ✅ Yes                                               |
| **Geographic testing**                          | Shows where real users are                        | You choose public/private locations                 |
| **Private/internal application**                | Can monitor real users accessing it               | Can use Private Location + Synthetic ActiveGate     |
| **Best question answered**                      | "How are our users experiencing the application?" | "Is our application working before users complain?" |

Dynatrace describes RUM as capturing and analyzing **actual end-user interactions**, while Synthetic Monitoring uses automated scripted tests to simulate user behavior and proactively detect availability/performance issues. ([Dynatrace Documentation][1])

---

## 1. Real User Monitoring — RUM

Imagine your company's tax application:

```text
Real Customer
     ↓
Opens Tax Application
     ↓
Login
     ↓
Fills Form
     ↓
Submits Return
     ↓
Backend/API/Database
```

Dynatrace observes what actually happened during that user's session.

It can help you understand things such as:

* Page load performance
* User actions
* Errors
* Application responsiveness
* Geographic/user impact
* Frontend performance
* Backend performance

Dynatrace RUM creates user sessions representing actual visits to web/mobile applications. ([Dynatrace Documentation][2])

### Example

Suppose 10,000 customers use your application.

You discover:

```text
Mumbai users       → 2.1 sec
Delhi users        → 2.3 sec
Bangalore users    → 2.0 sec
Hyderabad users    → 8.7 sec
```

RUM can help you identify that **actual users in a particular segment/location are experiencing slower performance**.

---

# 2. Synthetic Monitoring

Now imagine **nobody is using your application at 3 AM**.

You still want to know:

> "Can a user log in and complete the important workflow?"

That's where Synthetic Monitoring comes in.

Dynatrace can simulate the journey:

```text
Synthetic User
      ↓
Open Website
      ↓
Login
      ↓
Navigate to Tax Return
      ↓
Enter Details
      ↓
Submit
      ↓
Verify Response
```

It can run this automatically at configured intervals.

Dynatrace browser monitors simulate user interactions, while HTTP monitors can test websites and API endpoints. ([Dynatrace Documentation][3])

---

# The easiest example

Imagine an online banking application.

### RUM

A **real customer** does:

```text
Customer
   ↓
Login
   ↓
Check Balance
   ↓
Transfer ₹10,000
```

Dynatrace records what actually happened.

---

### Synthetic

Dynatrace creates a **simulated customer**:

```text
Synthetic Test
      ↓
Open Banking Website
      ↓
Login
      ↓
Check Balance
      ↓
Transfer Test / Validate Workflow
```

This happens according to the monitoring schedule.

---

# Why do we need both?

This is the really important production-support concept.

### Synthetic tells you:

> **"Something is broken before/when users encounter it."**

### RUM tells you:

> **"Here is what actual users are experiencing."**

So you can combine them:

```text
             APPLICATION
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
      RUM                Synthetic
        ↓                   ↓
Actual Users          Simulated Users
        ↓                   ↓
"What happened?"      "Does it work?"
        │                   │
        └─────────┬─────────┘
                  ↓
             Dynatrace
                  ↓
        Investigation / Alert
```

Dynatrace explicitly positions the two capabilities as complementary: RUM provides visibility into actual end-user experience, while Synthetic provides proactive testing. ([Dynatrace Documentation][1])

---

# Production Support Example

Suppose at **10:00 AM**:

### Synthetic detects

```text
Login Synthetic Monitor
        ↓
FAILED
        ↓
HTTP 500
```

This can alert the support team even if real-user traffic hasn't yet exposed the issue.

Then you check RUM:

```text
RUM
 ↓
3,200 real user sessions affected
 ↓
Login errors increased
 ↓
Users unable to access application
```

Then you investigate deeper:

```text
Synthetic
   ↓
Login failing

RUM
   ↓
Users affected

Service
   ↓
Authentication service

Trace
   ↓
Database call slow

Log
   ↓
DB connection timeout
```

That's where Dynatrace becomes powerful for **L2/production support**.

---

## One-line interview answer

> **Real User Monitoring monitors the experience of actual users interacting with an application, whereas Synthetic Monitoring proactively uses automated tests to simulate user interactions and verify application availability and performance, even when no real users are present.** ([Dynatrace Documentation][1])

### Easy memory trick 🧠

**RUM = REAL users**

**Synthetic = SIMULATED users**

And:

> **RUM tells you what users experienced. Synthetic tells you whether the experience can be successfully reproduced proactively.**

[1]: https://docs.dynatrace.com/docs/license/capabilities/real-user-synthetic-monitoring?utm_source=chatgpt.com "Real User and Synthetic Monitoring overview (DPS) — Dynatrace Docs"
[2]: https://docs.dynatrace.com/docs/license/capabilities/real-user-synthetic-monitoring/real-user-monitoring?utm_source=chatgpt.com "Understand and manage consumption for Real User Monitoring (DPS) — Dynatrace Docs"
[3]: https://docs.dynatrace.com/docs/license/capabilities/real-user-synthetic-monitoring/browser-monitor-clickpath?utm_source=chatgpt.com "Understand and manage consumption for Browser Monitor or Clickpath (DPS) — Dynatrace Docs"

Yes. In Dynatrace, **Network Availability Monitoring (NAM)** is a type of Synthetic Monitoring used when you want to check whether a **host, device, service, port, or DNS name is reachable over the network**, rather than testing a full webpage or user journey. ([Dynatrace Documentation][1])

## Network Availability Monitor — simple explanation

Think of it as:

> **“Can I reach this system over the network?”**

For example, suppose you have:

```text
Application Server
10.20.30.40
```

You don't necessarily need to open a browser and test the application.

You may simply want to know:

```text
Is 10.20.30.40 reachable?
Is port 443 open?
Can DNS resolve app.company.local?
```

That's where **NAM** comes in.

### Three types

| Type     | What it checks                         | Example                           |
| -------- | -------------------------------------- | --------------------------------- |
| **ICMP** | Network reachability using ping        | Can I ping `10.20.30.40`?         |
| **TCP**  | Whether a TCP port accepts connections | Is port `443` open?               |
| **DNS**  | Whether hostname resolves to an IP     | Does `app.company.local` resolve? |

Dynatrace currently supports these three NAM protocols: ICMP, TCP and DNS. ([Dynatrace Documentation][1])

---

## 1. ICMP Monitor

Basically a **ping test**.

```text
Synthetic Monitor
       ↓
   ICMP / Ping
       ↓
10.20.30.40
       ↓
   Response?
```

Example:

```text
10.20.30.40 → Reply
```

✅ Host/network reachable

But:

```text
10.20.30.40 → Timeout
```

❌ Network connectivity problem or ICMP blocked

It can also evaluate connection quality, not just whether a response exists. ([Dynatrace Documentation][1])

---

## 2. TCP Monitor

This is extremely useful for production support.

Suppose your application uses:

```text
Application Server
      ↓
TCP 443
```

NAM checks whether a TCP connection can be established to that port.

```text
Synthetic
   ↓
TCP connection
   ↓
10.20.30.40:443
   ↓
Connection accepted?
```

If port 443 isn't accepting connections:

```text
❌ TCP connection failed
```

This can indicate things such as:

* Service isn't listening
* Firewall/network issue
* Server unavailable
* Port blocked
* Application/service stopped

Dynatrace describes TCP NAM as validating that a port is open and accepts TCP connections. ([Dynatrace Documentation][1])

---

## 3. DNS Monitor

This checks **name resolution**.

For example:

```text
app.company.local
        ↓
      DNS
        ↓
10.20.30.40
```

If DNS cannot resolve the hostname:

```text
app.company.local
        ↓
     ❌ DNS failure
```

The application might actually be running perfectly, but users could still be unable to access it because the hostname doesn't resolve.

---

# NAM vs HTTP Monitor vs Browser Monitor

This is an important distinction for your Dynatrace notes.

| Monitor             | Main question                                   |
| ------------------- | ----------------------------------------------- |
| **ICMP NAM**        | Can I reach the host?                           |
| **TCP NAM**         | Can I connect to this port?                     |
| **DNS NAM**         | Can I resolve this hostname?                    |
| **HTTP Monitor**    | Does this HTTP/API endpoint respond correctly?  |
| **Browser Monitor** | Can a simulated user complete this web journey? |

For example:

```text
                  Application
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
        ICMP          TCP          DNS
          │            │            │
      Host alive?   Port open?   Name resolves?
          
                       ↓
                 HTTP Monitor
                       │
                 API/URL works?
                       ↓
                Browser Monitor
                       │
              User journey works?
```

HTTP monitors can run from both public and private Synthetic locations, whereas **NAM is supported only on private Synthetic locations**. ([Dynatrace Documentation][2])

---

# Why is NAM useful for your Private Location project?

This connects directly to what you've been building.

Imagine your company has:

```text
Corporate Network

10.20.30.10   DNS Server
10.20.30.20   Web Server
10.20.30.30   API Server
10.20.30.40   Database
```

A public Synthetic location on the internet cannot necessarily reach these private addresses.

So:

```text
Dynatrace
    ↓
Private Synthetic Location
    ↓
Synthetic-enabled ActiveGate
    ↓
Corporate Network
    ↓
10.20.30.x
```

Then you can create NAM tests such as:

```text
ICMP → 10.20.30.20
TCP  → 10.20.30.30:443
DNS  → api.company.local
```

NAM monitors are specifically designed for cases where an HTTP/HTTPS endpoint isn't available and are supported only on private locations. ([Dynatrace Documentation][1])

---

## Production-support example

Suppose users report:

> "The internal application isn't opening."

You could investigate layer by layer:

```text
1️⃣ DNS
   ↓
   api.company.local resolves?
   
2️⃣ ICMP
   ↓
   Server reachable?
   
3️⃣ TCP
   ↓
   Port 443 accepting connections?
   
4️⃣ HTTP
   ↓
   API responding?
   
5️⃣ Browser
   ↓
   Can user complete the workflow?
```

This gives you a nice troubleshooting hierarchy:

**DNS → Network → Port → HTTP/API → Application/User Journey**

### Interview-ready answer

> **Network Availability Monitoring in Dynatrace is a Synthetic Monitoring capability used to proactively verify network-level availability of hosts and services. It supports ICMP, TCP, and DNS checks. For example, ICMP can verify host reachability, TCP can verify whether a specific port accepts connections, and DNS can verify hostname resolution. NAM is particularly useful for private infrastructure and operates through private Synthetic locations.** ([Dynatrace Documentation][1])

[1]: https://docs.dynatrace.com/docs/observe/digital-experience/synthetic-monitoring/network-availability-monitors/network-availability-monitoring?utm_source=chatgpt.com "Network availability monitoring in Classic — Dynatrace Docs"
[2]: https://docs.dynatrace.com/docs/observe/digital-experience/synthetic-monitoring/general-information/types-of-synthetic-monitors?utm_source=chatgpt.com "Types of synthetic monitors in Classic — Dynatrace Docs"

Yes. The flow you wrote is essentially about **setting up RUM → identifying your frontend/application → viewing the collected data → querying the data → putting the result on a dashboard**.

One terminology correction first: **“Dynatrace Data Exporter” and “Data Explorer” are different things.** If your step is **Write Query → Pin to Dashboard**, you are most likely referring to **Data Explorer** or, in the newer Dynatrace experience, **DQL/Notebooks**. Data Explorer is specifically designed to query/visualize metrics and pin visualizations to dashboards. ([Dynatrace Documentation][1])

## 1. What is RUM?

**RUM = Real User Monitoring.**

RUM collects information about how **real users interact with your web/mobile application**.

For a web application:

```text
Real User
   ↓
Opens Website
   ↓
Login
   ↓
Clicks / Navigates
   ↓
API Calls
   ↓
Backend Services
   ↓
Database
```

Dynatrace can capture information about the user's experience, such as:

* User sessions
* Page views / views
* User actions
* JavaScript errors
* Request errors
* Page-load performance
* Core Web Vitals
* Browser/device information
* Geographic information
* Frontend-to-backend relationships

Dynatrace's current RUM model includes **user events and user sessions**, while Experience Vitals provides frontend-level performance and health information. ([Dynatrace Documentation][2])

---

# 2. Your flow: Application Detection

You wrote:

> RUM → Settings → Applications Detection → Add Item → Add URL → Application and Observability → Frontend → Show Data

The important concept here is **Application/Frontend Detection**.

Dynatrace needs to know:

> **“Which application should this RUM data belong to?”**

For example, suppose your company has:

```text
https://tax.company.com
https://hr.company.com
https://portal.company.com
```

You don't want all RUM traffic to appear as one generic application.

You can create detection rules so Dynatrace maps traffic to the appropriate frontend/application.

The application detection configuration defines rules for grouping RUM monitoring data into distinct applications. ([Dynatrace Documentation][3])

### Example

You create a rule:

```text
URL contains:
tax.company.com

        ↓

Application:
Tax Portal
```

Then:

```text
Real User
    ↓
https://tax.company.com/login
    ↓
Dynatrace RUM
    ↓
Tax Portal frontend
```

So **Application Detection = telling Dynatrace how to classify the captured RUM traffic.**

---

# 3. Frontend

This is another important term.

In Dynatrace, a **frontend** represents the client-side application users interact with.

For example:

```text
User
 ↓
Browser
 ↓
Tax Portal Frontend
 ↓
API
 ↓
Backend Service
 ↓
Database
```

The frontend is where RUM starts observing the user's experience.

Current Dynatrace uses **Experience Vitals** as a major entry point for frontend monitoring. It provides an overview of monitored frontends and their performance/health information. ([Dynatrace Documentation][4])

---

# 4. "Show Data"

Once RUM is correctly configured and users are generating traffic, Dynatrace starts receiving RUM data.

For example:

```text
Frontend: Tax Portal

Active Users       1,250
Sessions            1,480
Error Rate          2.1%
Page Load           2.4 sec
LCP                  2.1 sec
INP                  180 ms
```

You can drill into the frontend and investigate:

```text
Frontend
   ↓
Performance
   ↓
User Sessions
   ↓
User Actions
   ↓
Errors
   ↓
Backend services
   ↓
Distributed traces
```

Dynatrace also supports frontend-to-backend linking, allowing you to move from a RUM issue into backend traces/services for investigation. ([Dynatrace Documentation][5])

---

# 5. Where does "Trigger now" fit?

If you're referring to **Synthetic Monitoring**, don't mix this with RUM.

### RUM

```text
Real user generates traffic
        ↓
Dynatrace captures it
```

### Synthetic

```text
Dynatrace triggers a test
        ↓
Synthetic user/test runs
        ↓
Result is collected
```

So if you see a **Trigger now / execute now** type option while working with a Synthetic monitor, the purpose is generally to **run the synthetic test immediately rather than waiting for its normal schedule**.

For example:

```text
Synthetic Monitor
Schedule: Every 5 minutes

Normal:
10:00 → Run
10:05 → Run
10:10 → Run

Trigger Now:
10:02 → Run immediately
```

This is particularly useful when you're troubleshooting a failed monitor and want to verify whether the problem still occurs.

---

# 6. Data Explorer — "Write Query"

Now we get to the second part of your notes.

Suppose you've collected RUM data and want to answer:

> "How many users are accessing my application?"

or:

> "What's the error rate?"

or:

> "Which pages have the most traffic?"

You need to **query the data**.

That's where tools such as **Data Explorer** and **DQL** come in.

### Data Explorer

Data Explorer lets you select metrics, apply filters, split dimensions, choose visualizations, and create charts. ([Dynatrace Documentation][1])

For example:

```text
Metric
 ↓
User actions
 ↓
Filter
 ↓
Tax Portal
 ↓
Visualization
 ↓
Line chart
```

---

# 7. Example: RUM query

With newer Dynatrace/DQL workflows, you can query RUM user events directly.

For example, conceptually:

```text
fetch user.events
| filter frontend.name == "Tax Portal"
| summarize count()
```

This asks:

> **How many user events were captured for the Tax Portal frontend?**

Dynatrace documents DQL specifically for analyzing RUM user behavior, including clicks, navigations, sessions, and custom properties. ([Dynatrace Documentation][6])

You can also create queries around:

```text
Users
Sessions
Page views
Clicks
Errors
Navigation
User actions
Performance
```

---

# 8. Pin to Dashboard

Once you've created a useful visualization:

```text
Query
  ↓
Result
  ↓
Visualization
  ↓
Pin to Dashboard
```

Now your support/operations dashboard can contain things like:

```text
┌─────────────────────────────────┐
│       Production Dashboard      │
├─────────────────────────────────┤
│ Active Users        1,250       │
│ Sessions            1,480       │
│ Error Rate           2.1%       │
│ Page Load            2.4 sec    │
│                                 │
│ User Traffic ───────────────    │
│ Error Trend  ───────────────    │
│ Top Pages    ───────────────    │
└─────────────────────────────────┘
```

Data Explorer specifically supports **Pin to dashboard** after you've built the query and visualization. ([Dynatrace Documentation][1])

---

# 9. Your complete learning flow

I would rewrite your notes like this:

```text
                    RUM
                     │
                     ↓
        Monitor Real User Experience
                     │
                     ↓
           Identify Frontend/App
                     │
                     ↓
          Application Detection
                     │
                     ↓
          URL / Detection Rules
                     │
                     ↓
              Frontend
                     │
                     ↓
             RUM Data Captured
                     │
                     ↓
              Show / Analyze
                     │
            ┌────────┴────────┐
            ↓                 ↓
       Data Explorer          DQL
            ↓                 ↓
       Write Query       Query RUM Data
            │                 │
            └────────┬────────┘
                     ↓
               Visualization
                     ↓
              Pin to Dashboard
```

### In one sentence:

> **RUM captures real-user activity → Application Detection maps that activity to the correct frontend → you analyze the captured data using Data Explorer/DQL → create visualizations → pin useful views to a dashboard.**

### And don't mix these three concepts:

**RUM** → collects/observes real-user experience.

**Data Explorer / DQL** → analyzes the collected data.

**Dashboard** → presents the important results for continuous monitoring.

That's a very useful distinction for an **L2/Production Support** interview.

[1]: https://docs.dynatrace.com/docs/analyze-explore-automate/explorer?utm_source=chatgpt.com "Data Explorer — Dynatrace Docs"
[2]: https://docs.dynatrace.com/docs/semantic-dictionary/model/rum?utm_source=chatgpt.com "Real User Monitoring — Dynatrace Docs"
[3]: https://docs.dynatrace.com/docs/dynatrace-api/environment-api/settings/schemas/builtin-rum-web-app-detection?utm_source=chatgpt.com "Settings API - Application detection schema table — Dynatrace Docs"
[4]: https://docs.dynatrace.com/docs/observe/digital-experience/rum/experience-vitals?utm_source=chatgpt.com "Experience Vitals — Dynatrace Docs"
[5]: https://docs.dynatrace.com/docs/observe/digital-experience/rum/analyze-and-alert/analyze-frontend-to-backend?utm_source=chatgpt.com "Analyze performance from frontend to backend — Dynatrace Docs"
[6]: https://docs.dynatrace.com/docs/observe/digital-experience/rum/analyze-and-alert/rum-dql-user-behavior?utm_source=chatgpt.com "Analyze user behavior with DQL — Dynatrace Docs"

Your notes are covering several **advanced but very useful Dynatrace topics**. The key is to understand how they connect rather than memorizing menu paths.

> **RUM collects user experience → DQL analyzes data → Synthetic/API monitors proactively test systems → Segments organize/filter environments → Tags enrich entities so you can group and operate them.**

---

# 1. Agentless RUM

### What is Agentless RUM?

Normally, Dynatrace can use **OneAgent/browser auto-injection** to instrument an application.

**Agentless RUM** means you manually add the Dynatrace RUM JavaScript snippet to the web application instead of relying on OneAgent to inject it.

```text
Normal / Auto-injected RUM

Web Server
    ↓
OneAgent
    ↓
Inject RUM JavaScript
    ↓
Browser
    ↓
RUM data
```

Agentless:

```text
Web Application
    ↓
Developer manually adds
Dynatrace RUM JavaScript
    ↓
Browser
    ↓
Dynatrace
```

Dynatrace's current agentless setup provides a JavaScript tag that you copy into the application. The RUM JavaScript then sends RUM beacons containing the captured data. ([Dynatrace Documentation][1])

### Why use Agentless RUM?

Useful when:

* You can't install OneAgent.
* The application is hosted somewhere you don't control.
* You want explicit control over the RUM JavaScript.
* You have a frontend application where browser-side instrumentation is easier than server-side instrumentation.

---

# 2. How to add Agentless RUM

The exact UI wording can change, but the current Dynatrace workflow is essentially:

```text
RUM / Frontend
      ↓
Create / Add web application
      ↓
Choose Agentless monitoring
      ↓
Configure application
      ↓
Configure privacy / interactions
      ↓
Copy JavaScript tag
      ↓
Add tag to web application
      ↓
Deploy application
      ↓
Real users generate traffic
      ↓
Dynatrace receives RUM data
```

Current Dynatrace documentation specifically has an **Agentless RUM setup** flow and provides the JavaScript tag to copy. You can also enable user interactions and configure end-user privacy settings during setup. ([Dynatrace Documentation][1])

### Example

Suppose your application is:

```text
https://tax.company.com
```

You add the Dynatrace RUM JavaScript to the frontend:

```html
<script>
    /* Dynatrace RUM JavaScript */
</script>
```

Then:

```text
User opens tax.company.com
        ↓
RUM JavaScript executes
        ↓
Page/view/action information captured
        ↓
RUM beacon sent
        ↓
Dynatrace
```

---

# 3. What should we monitor with RUM?

Don't think of RUM as simply:

> "Is the website UP?"

That's more Synthetic Monitoring.

RUM answers:

> **"How are actual users experiencing my application?"**

Monitor things such as:

### User experience

* Page/view performance
* User actions
* Navigation
* Session information
* User interactions

### Frontend health

* JavaScript errors
* Failed requests
* Application errors
* Performance degradation

### Performance

* Load time
* Core Web Vitals
* Response/request performance
* Frontend-to-backend performance

### User impact

For example:

```text
Application: Tax Portal

Users             12,500
Sessions           9,800
Error rate           3.2%
Slow sessions       1,100
JS errors             450
```

Then you can drill down:

```text
User
 ↓
Session
 ↓
User Action
 ↓
Frontend Request
 ↓
Backend Service
 ↓
Distributed Trace
```

That's particularly useful for your L2 support work because you can go from **user impact → technical root-cause investigation**.

---

# 4. Agentless RUM vs Synthetic

This distinction is important.

| Agentless RUM                | Synthetic                   |
| ---------------------------- | --------------------------- |
| Real users                   | Simulated users             |
| Passive observation          | Proactive testing           |
| User actually visits         | Dynatrace initiates test    |
| Captures real experience     | Validates expected behavior |
| Requires user traffic        | Works even with zero users  |
| "What did users experience?" | "Does it work?"             |

Example:

```text
RUM
Customer → Login → Error
             ↓
       Dynatrace records it


Synthetic
Dynatrace → Login test → Error
                    ↓
             Alert/support
```

---

# 5. DQL — what you're learning

Your list:

> `fetch, filter, filterOut, fields, fieldName, limit, sort, count, countDistinct, collectDistinct, countIf, endsWith, timeseries, fieldsAdd`

These are **DQL building blocks**.

Think of DQL as:

> **SQL-like querying for Dynatrace observability data.**

Dynatrace Grail stores observability data, and DQL is used to explore and analyze it. Notebooks and Dashboards can directly use DQL queries. ([Dynatrace Documentation][2])

---

# 6. Notebook → New Notebook

A typical learning workflow:

```text
Dynatrace
   ↓
Notebooks
   ↓
New Notebook
   ↓
Add section
   ↓
DQL
   ↓
Write query
   ↓
Run
   ↓
Visualize
   ↓
Save / Share / Dashboard
```

A Notebook is excellent for **investigation and analysis**.

For example:

> "Show me all errors from production in the last 2 hours."

---

# 7. Your DQL commands

Let's make your list easy to remember.

### `fetch`

**Get data.**

```dql
fetch logs
```

Meaning:

> Give me log records.

---

### `filter`

**Keep matching records.**

```dql
fetch logs
| filter loglevel == "ERROR"
```

Meaning:

> Only show ERROR logs.

---

### `filterOut`

Conceptually:

> Remove records matching a condition.

Useful when you want to exclude noise.

---

### `fields`

**Choose which columns you want to see.**

```dql
fetch logs
| fields timestamp, loglevel, content
```

Instead of displaying every available field.

---

### Field name

A **field** is basically a piece of information in a record.

Example:

```text
timestamp
loglevel
content
host.name
service.name
```

Think:

```text
Record
 ├── timestamp
 ├── loglevel
 ├── service.name
 └── content
```

---

### `limit`

Restrict number of records.

```dql
fetch logs
| limit 20
```

> Give me only 20 records.

---

### `sort`

Sort your result.

```dql
fetch logs
| sort timestamp desc
```

Newest first.

---

# 8. `count()`

Counts records.

```dql
fetch logs
| summarize count()
```

Example result:

```text
count = 15,420
```

Meaning:

> 15,420 matching log records.

---

# 9. `countDistinct()`

Counts **unique values**.

Imagine:

```text
user
----
Shuv
Rahul
Shuv
Amit
Rahul
```

Normal count:

```text
5
```

Distinct users:

```text
3
```

So:

> `countDistinct()` = How many unique values?

---

# 10. `collectDistinct()`

Instead of counting unique values, collect the unique values.

Example concept:

```text
service.name

TaxAPI
TaxAPI
AuthService
PaymentService
AuthService
```

`collectDistinct()` gives you something conceptually like:

```text
TaxAPI
AuthService
PaymentService
```

Useful when you want to **see the unique values**, not just count them.

---

# 11. `countIf()`

Counts only records satisfying a condition.

For example:

```text
Total requests = 10,000
Errors = 250
```

You can use a conditional count to calculate the number of errors.

Conceptually:

```dql
countIf(status == "ERROR")
```

Think:

> `count()` = count everything matching the query
> `countIf()` = count only records satisfying this condition

---

# 12. `endsWith()`

Checks whether a string ends with a particular value.

Example:

```text
server-prod-01
server-prod-02
server-dev-01
```

You might filter names ending in:

```text
"-01"
```

Conceptually:

```dql
filter endsWith(host.name, "-01")
```

---

# 13. `fieldsAdd`

This is very useful.

It allows you to **create/add a calculated field**.

Conceptually:

```dql
| fieldsAdd environment = "PROD"
```

Or derive a value from another field.

For example:

```text
Original:
host.name = web-prod-01

New:
environment = PROD
```

You can then use the new field for analysis.

---

# 14. `timeseries`

This is for **time-based analysis**.

Instead of:

```text
Error count = 500
```

you want:

```text
10:00 → 20 errors
10:05 → 35 errors
10:10 → 80 errors
10:15 → 150 errors
10:20 → 215 errors
```

Then you can visualize the trend.

Dynatrace's Synthetic documentation itself uses `timeseries` to analyze monitor availability over time. ([Dynatrace Documentation][3])

---

# 15. DQL → Visualization → Dashboard

Your workflow is correct:

```text
DQL
 ↓
Run Query
 ↓
Result
 ↓
Visual
 ↓
Choose chart
 ↓
Configure visualization
 ↓
Save
 ↓
Add / Pin to Dashboard
```

For example:

### Query

```text
Error count over time
```

### Visualization

```text
Line chart
```

### Dashboard

```text
┌──────────────────────────────┐
│ Production Application       │
├──────────────────────────────┤
│ Users              12,500    │
│ Error Rate             2.1%  │
│                              │
│ Error Trend                 │
│       ╱───────              │
│  ────╯                      │
│                              │
│ Top Error Services           │
│ Auth        125              │
│ Tax API      89              │
└──────────────────────────────┘
```

Current Dynatrace Dashboards allow a DQL tile to be added and then configured through **Data** and **Visual** tabs; dashboard-level and tile-level segments can also be applied. ([Dynatrace Documentation][4])

---

# 16. API-Based Monitor Creation

Now we move into automation.

You can create Synthetic monitors through the Dynatrace UI:

```text
Synthetic
 ↓
New monitor
 ↓
HTTP
 ↓
Configure
 ↓
Save
```

But imagine you have:

```text
100 APIs
```

and need:

```text
100 Synthetic monitors
```

Manually creating them isn't ideal.

Instead:

```text
Automation / Script
        ↓
Dynatrace API
        ↓
Create monitor
        ↓
Monitor created
```

Dynatrace provides a Synthetic Monitor API for creating monitors, including HTTP and browser monitors. The API requires appropriate authentication, such as the `ExternalSyntheticIntegration` scope for the documented POST operation. ([Dynatrace Documentation][5])

---

# 17. Why API-based monitor creation?

### 1. Automation

Create monitors automatically.

### 2. Scale

Instead of:

```text
100 APIs → 100 manual configurations
```

you can generate configurations programmatically.

### 3. DevOps integration

For example:

```text
New application deployed
        ↓
CI/CD pipeline
        ↓
API call
        ↓
Create Synthetic monitor
```

### 4. Standardization

Every monitor can follow the same:

```text
Name
Frequency
Location
Authentication
Tags
Thresholds
```

### 5. Infrastructure-as-code / automation

You can keep monitor configuration in source control and automate deployment.

---

# 18. How to implement API-based Synthetic Monitor

Conceptually:

```text
1. Create Dynatrace API token
             ↓
2. Give required permissions
             ↓
3. Prepare monitor JSON
             ↓
4. POST to Dynatrace Synthetic API
             ↓
5. Dynatrace creates monitor
             ↓
6. Monitor executes
             ↓
7. Query results / metrics
```

Example architecture:

```text
Git Repository
      ↓
Python / PowerShell / Terraform / CI pipeline
      ↓
Dynatrace API
      ↓
HTTP Synthetic Monitor
      ↓
Private/Public Location
      ↓
API endpoint
```

Dynatrace also provides APIs to retrieve monitor configuration and execution results. ([Dynatrace Documentation][6])

---

# 19. Environment Segmentation

This is another important concept.

Imagine your company has:

```text
Production
 ├── Tax
 ├── HR
 ├── Finance

UAT
 ├── Tax
 ├── HR

Development
 ├── Tax
 ├── HR
```

You don't want everyone looking at everything.

**Segments** allow you to logically filter observability data.

For example:

```text
Segment: Production

Environment = PROD
```

Then:

```text
Dashboard
    ↓
Production segment
    ↓
Only production-related data
```

Dynatrace describes segments as a way to logically structure and filter observability data across applications, infrastructure, logs, metrics, events and other data types. ([Dynatrace Documentation][7])

### Important current-Dynatrace distinction

If you're learning older Dynatrace:

```text
Management Zones
```

was a major concept.

In **Latest Dynatrace**, **Segments** are used for data segmentation/filtering, while access control is handled separately through permissions/data access concepts. Dynatrace's current documentation explicitly describes segments as replacing the data-filtering role of management zones in the newer model. ([Dynatrace Documentation][8])

---

# 20. Your Auto-tagging requirement

This is the most interesting part of your question:

> **"We want to get the server name out of the URL and put it as a tag."**

There is an important architectural point here.

Suppose your URL is:

```text
https://server123.company.com/api/login
```

You want:

```text
ServerName = server123
```

and ultimately:

```text
ServerName:server123
```

### Don't immediately create an automatic-tagging rule based on the URL.

Why?

Dynatrace's **automatic tagging** works primarily from properties of the entity being tagged — for example host name, IP, process-group properties, service properties, etc. It can also use regex conditions. ([Dynatrace Documentation][9])

If the **server name exists only inside a request URL**, the cleaner approach is generally:

```text
URL
 ↓
Extract value
 ↓
Request Attribute
 ↓
Use attribute for analysis/naming/enrichment
 ↓
If needed, use appropriate tagging/enrichment mechanism
```

Dynatrace supports creating request attributes from web-request data and then processing the captured value, including extraction using delimiters or regular expressions. ([Dynatrace Documentation][10])

---

# 21. Example: extracting server name from URL

Suppose requests look like:

```text
https://server01.company.com/api/login
https://server02.company.com/api/login
https://server03.company.com/api/login
```

You want:

```text
server01
server02
server03
```

Conceptually:

```text
WEBREQUEST_URL
       ↓
Extract hostname
       ↓
server01
       ↓
Request Attribute
       ↓
server_name = server01
```

You can then use that information for:

```text
Filtering
Grouping
Request naming
Analysis
Metrics
Dashboards
```

Dynatrace's request-attribute processing supports extraction and regex-based post-processing. ([Dynatrace Documentation][10])

---

# 22. What if you specifically need a TAG?

Then separate the problem into two parts.

### Part A — extract

```text
URL
 ↓
Request Attribute
 ↓
server_name
```

### Part B — enrichment/tagging

```text
server_name
 ↓
appropriate entity/data enrichment
 ↓
ServerName:server01
```

Don't confuse:

```text
Request Attribute
```

with:

```text
Entity Tag
```

They are different concepts.

### Request Attribute

Attached to/requested from request data and useful for request-level analysis.

### Entity Tag

Attached to an entity such as:

```text
Host
Service
Process
Process Group
Application
```

and useful for grouping, filtering, alert routing, maintenance, etc.

Dynatrace's automatic tagging documentation lists supported entity properties and allows regex-based conditions, but the property must be available on the entity being evaluated. ([Dynatrace Documentation][9])

---

# 23. A better architecture for your use case

If your real requirement is:

> "We have URLs containing application/server information and we want to segment our monitoring based on that information."

I'd structure it like this:

```text
                 Incoming Request
                       │
                       ↓
              https://server01...
                       │
                       ↓
               Request Attribute
                 server_name
                       │
              ┌────────┴─────────┐
              ↓                  ↓
        Request analysis       DQL
              ↓                  ↓
        Request naming       Dashboard
              │
              ↓
       Environment filtering
              │
              ↓
            Segment
```

If the requirement is instead:

> "We already have server names as host properties and just want Dynatrace to automatically tag the hosts."

Then **Automatic Tags** is the correct solution:

```text
Settings
 ↓
Tags
 ↓
Automatically applied tags
 ↓
Create tag
 ↓
Add rule
 ↓
Rule applies to → Hosts
 ↓
Condition → Host name / detected host name
 ↓
Regex/contains/etc.
 ↓
Preview
 ↓
Create rule
 ↓
Save
```

Dynatrace's automatic-tagging workflow supports creating rules, selecting the entity type, adding conditions, previewing matching entities, and saving the rule. ([Dynatrace Documentation][9])

---

# 24. Your whole Dynatrace learning map

You are actually building a pretty good L2/observability skill set:

```text
                    DYNATRACE
                        │
        ┌───────────────┼────────────────┐
        ↓               ↓                ↓
       RUM          Synthetic          DQL
        │               │                │
   Real Users      Synthetic Users    Analyze Data
        │               │                │
   Frontend        HTTP/Browser/NAM    Notebook
        │               │                │
 Agentless/OneAgent  Public/Private   Visualization
        │               │                │
        └───────────────┼────────────────┘
                        ↓
                   DASHBOARDS
                        │
                        ↓
                ENVIRONMENT SEGMENT
                        │
                        ↓
                   TAGGING
                        │
          ┌─────────────┴──────────────┐
          ↓                            ↓
 Automatic Tags                 Request Attributes
          │                            │
 Entity classification          Request-level context
          │                            │
          └─────────────┬──────────────┘
                        ↓
                L2 INVESTIGATION
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
       Detect        Analyze        Act
          │             │             │
        RUM/Davis      DQL         Incident/Alert
        Synthetic      Traces      Remediation
```

### The interview-level mental model

**RUM** → *What are real users experiencing?*

**Synthetic** → *Can I proactively verify that the application works?*

**DQL** → *What does the observability data tell me?*

**Notebook** → *Where do I investigate and document analysis?*

**Dashboard** → *How do I continuously visualize important information?*

**API monitor creation** → *How do I automate monitoring at scale?*

**Segments** → *How do I logically filter my environment/data?*

**Auto-tagging** → *How do I automatically classify entities?*

**Request attributes** → *How do I extract additional context from requests?*

And for your specific **URL → server name** requirement, remember:

> **URL extraction → Request Attribute is usually the first step; Automatic Tagging is a separate entity-classification mechanism.** Don't treat them as the same feature. ([Dynatrace Documentation][9])

[1]: https://docs.dynatrace.com/docs/observe/digital-experience/rum/web-frontends/initial-setup/set-up-agentless-monitoring?utm_source=chatgpt.com "Set up agentless RUM — Dynatrace Docs"
[2]: https://docs.dynatrace.com/docs/manage/segments/concepts/segments-concepts-queries?utm_source=chatgpt.com "Segments in DQL queries — Dynatrace Docs"
[3]: https://docs.dynatrace.com/docs/observe/digital-experience/synthetic/synthetic-metrics/http-monitor-metrics?utm_source=chatgpt.com "HTTP monitor metrics in Synthetic Monitoring — Dynatrace Docs"
[4]: https://docs.dynatrace.com/docs/analyze-explore-automate/dashboards-and-notebooks/dashboards-new/components/dashboard-component-data?utm_source=chatgpt.com "Add data to a dashboard — Dynatrace Docs"
[5]: https://docs.dynatrace.com/docs/dynatrace-api/environment-api/synthetic/synthetic-monitors/post-a-monitor?utm_source=chatgpt.com "Synthetic monitors API - POST a monitor — Dynatrace Docs"
[6]: https://docs.dynatrace.com/docs/dynatrace-api/environment-api/synthetic/synthetic-monitors/get-a-monitor?utm_source=chatgpt.com "Synthetic monitors API - GET a monitor — Dynatrace Docs"
[7]: https://docs.dynatrace.com/docs/manage/segments?utm_source=chatgpt.com "Segments — Dynatrace Docs"
[8]: https://docs.dynatrace.com/docs/manage/segments/upgrade-guide-segments?utm_source=chatgpt.com "Upgrade from management zones to segments — Dynatrace Docs"
[9]: https://docs.dynatrace.com/docs/manage/tags-and-metadata/setup/how-to-define-tags?utm_source=chatgpt.com "Define and apply tags — Dynatrace Docs"
[10]: https://docs.dynatrace.com/docs/observe/applications-and-microservices/services/request-attributes/capture-request-attributes-based-on-web-request-data?utm_source=chatgpt.com "Capture request attributes based on web request data — Dynatrace Docs"

