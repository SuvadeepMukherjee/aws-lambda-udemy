# Triggers

## What is a Trigger ? 

- A **Lambda trigger** is anything that *invokes* a Lambda function.

- A function can be invoked:

  - Manually in the console
  - Programmatically through SDKs
  - Through the AWS CLI
  - Through integration with other AWS services (the most powerful use case)

- A single Lambda can have **multiple triggers**, though usually only one is used for simplicity.

- ### **Event Source Mapping (ESM)**

  - Different from a basic trigger.
  - Lambda **polls** another service (like SQS or DynamoDB Streams) and invokes the function when new items appear.
  - Essentially: *“Lambda reads events from the source on your behalf.”*

- ### **Trigger Behavior Differences**

  - Different triggers have different **scaling** and **retry** behaviors.
  - Example: S3 → Lambda vs. SQS → Lambda behave differently under load.
  - Important in production because retry logic may affect downstream systems.

## Common Lambda Triggers /Event Source Mappings

- #### **CloudWatch Events (EventBridge) — Scheduled Jobs**

  - Used for **timer-based tasks** (cron-like).
  - Ideal for:
    - Database maintenance
    - Nightly data processing
    - Automations that run on schedule
  - Lambda runs automatically when the schedule fires.

- #### **API Gateway — REST APIs**

  - Used when building APIs.
  - Endpoint → API Gateway → Lambda.
  - Lambda processes request (DB calls, logic) → returns response back to clients.
  - Useful as a secure layer instead of exposing Lambda directly.

- #### **SNS (Simple Notification Service)**

  - Pub/Sub messaging.
  - Lambda can **subscribe** to an SNS topic.
  - When a message is published → all subscribers (including Lambda) are invoked.

- #### **SQS (Simple Queue Service)**

  - Queue-based messaging.
  - Integrated using **event source mapping**.
  - Lambda **polls** the queue and processes messages automatically.
  - Used for decoupling and buffering workloads.

- ####  **DynamoDB Streams**

  - Captures **record-level changes** (insert, update, delete).
  - Lambda can listen in near real-time.
  - Example use case:
    - New order added → Lambda aggregates stats → updates dashboards.

- #### **S3 Events**

  - Triggered on object creation, deletion, or update.
  - Very common pattern:
    - User uploads a file → Lambda processes it (resize image, extract metadata, parse invoice, etc.)
  - Lambda event payload includes **bucket name** and **object key**.