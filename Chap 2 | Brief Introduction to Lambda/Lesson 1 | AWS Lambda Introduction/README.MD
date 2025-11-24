# AWS Lambda Introduction

## What is AWS Lambda ? 

- AWS Lambda is a **serverless compute service** that lets you **run code without managing servers**.
- You write **functions**, and Lambda runs them **on demand**.
- **Pay per execution model** — billed only when your code runs.
- Supports many languages: **Python, Node.js, Java, C#, Go, Ruby**, etc.

## History & Evolution

- Early 2000s – Traditional Data Centers
  - Companies bought hardware, installed software, maintained servers, handled security and cooling.
  - Costly, slow provisioning, required forecasting hardware needs months ahead.
- 2006 – AWS EC2 (Elastic Compute Cloud)
  - Provided virtual machines in the cloud.
  - No need to own physical servers — pay for compute time rented.
- 2014 – AWS Lambda
  - Next evolution: **compute without provisioning or managing servers**
  - **Pay per request**, scales automatically, optimized for event-driven systems.

## Why is Lambda Useful ? 

- **No servers to manage** — AWS handles infrastructure, patching, security.
- **Automatic scaling** — handles traffic spikes seamlessly (thousands of requests/sec).
- **Cost efficient** — no cost when idle, only charged per invocation.
- **High performance** — can increase memory for faster runs; Graviton CPUs = **20–30% faster & cheaper**.
- **Rich service integrations** — works naturally with **S3, DynamoDB, API Gateway, EventBridge**, etc.
- **Easy to use** — simple deployment, debugging, and monitoring.

## Common Use Cases

- REST **API backends** (with API Gateway)
- **Event processing** (e.g., S3 upload → trigger Lambda)
- **Scheduled tasks** (cron, database backups)
- Data pipelines / transformation

## Typical Lambda Workflow

1. **Create a function** (Console, CLI, IaC like CDK/CloudFormation).
2. **Write & upload code** (choose supported runtime).
3. **Invoke** via API, event trigger, or manual testing.

## Disadvantages/Tradeoffs

- **Less infrastructure control** — AWS handles hardware, so deeper tuning isn’t possible.
- **Debugging complex distributed systems can be harder**.
- **Cold starts** (slow startup on first execution or after idle time—covered later in course).

> *Summary tradeoff*: **Flexibility gained, control lost**.

