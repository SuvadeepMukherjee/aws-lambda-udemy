# Looking at our Functions Logs

## What is CloudWatch?

- CloudWatch is an AWS monitoring and logging service.
- Provides metrics, monitoring, alerts, dashboards, and **log storage**.
- We mainly use **Logs → Log groups** to view Lambda logs.

## Log Groups & Log Streams

- Each Lambda function automatically creates a **Log Group**.
- Inside a log group, logs are stored in **Log Streams**.
- A log stream typically represents **one execution environment (container)**.
- Each time:
  - A new container is spun up **(new cold start)**, OR
  - You **deploy new code** → A **new log stream** gets created.
- Multiple log streams appear for scaling / multiple concurrent executions.

## Retention Policy (Important for cost)

- Default is **Never Expire**.
- CloudWatch Logs cost **$0.50 per GB stored**.
- For real applications, enable **log retention policy** or archive.

## Viewing Logs

- Navigate: **CloudWatch → Logs → Log Groups → Log Streams**
- Each log stream shows console output like Lambda test console.
- Use log streams to debug individual executions.