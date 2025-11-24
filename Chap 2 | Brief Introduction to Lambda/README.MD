# Brief Introduction to Lambda

## What is AWS Lambda ? 

- AWS Lambda is a **serverless compute service** that lets you **run code without managing servers**.
- You write **functions**, and Lambda runs them **on demand**.
- **Pay per execution model** — billed only when your code runs.
- Supports many languages: **Python, Node.js, Java, C#, Go, Ruby**, etc.

## History & Evolution

- **Early 2000s – Traditional Data Centers**
  - Companies bought hardware, installed software, maintained servers, handled security and cooling.
  - Costly, slow provisioning, required forecasting hardware needs months ahead.
- **2006 – AWS EC2 (Elastic Compute Cloud)**
  - Provided virtual machines in the cloud.
  - No need to own physical servers — pay for compute time rented.
- **2014 – AWS Lambda**
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

---

## A Tour of the Lambda Console

### Accessing Lambda in AWS Console
- Open AWS Console → Use the **Services search box** → Select **Lambda**
- Make sure to **check your AWS Region** (top right).
- Lambda functions are **region-specific** — a function created in `us-east-1` won’t appear in `us-east-2`.

### Lambda Console Overview
- Default landing page shows the **Functions** section.
- If there are functions, they appear in **alphabetical order**.
- Use **Create function** button to start building functions.

## Dashboard Section
- Gives **high-level account-level metrics** for all Lambda functions.
- Shows:
  - Number of Lambda functions
  - **Code storage** used (max limit = **75 GB**)
  - **Concurrency limits**
    - Default account concurrency = **1000**
    - **Unreserved concurrency** = shared pool for all functions
- Useful metrics displayed:
  - **Errors**
  - **Success rate**
  - **Throttles**
  - **Invocations**
  - **Duration**
  - **Concurrent executions**
  - **Unreserved concurrent executions**

> Metrics are aggregated across **all functions in the region** — good for bird’s-eye monitoring.

## Applications Section
- Shows **pre-built serverless application templates**.
- Helps deploy sample apps using Lambda + other AWS services (API Gateway, DynamoDB, etc.)
- Example template: **Serverless API Backend** (Node.js runtime)
- This section simplifies setup, but **not used in manual learning or CDK projects**.

## Other Console Features (Left Menu)
### Code Signing
- Uses **AWS Signer** to validate and restrict code updates.
- Ensures only trusted sources can deploy to Lambda (security feature).

### Layers
- Allows sharing **common dependencies/libraries** across multiple Lambda functions.
- Helps avoid duplicating packages inside each Lambda bundle.

### Step Functions
- Workflow orchestration tool that integrates with Lambda.

---

## Creating our first function

### Navigation
- If the **left navigation pane** is missing, click the **three horizontal lines** (hamburger menu) to open it.
- Go to **Lambda → Functions → Create function** (top-right button).

## Ways to Create a Lambda Function
### 1. **Author from scratch** (most common)
- Gives a minimal starter file (Hello World).
- You implement everything manually.

### 2. **Use a blueprint**
- Pre-configured templates for common tasks.
- Example: **API Gateway + DynamoDB REST API** blueprint.
- Helpful for beginners, but for deep learning better start from scratch.

### 3. **Container image**
- Deploy Lambda using a Docker image.
- Uses **ECR (Elastic Container Registry)** as the image store.
- Useful when dependencies are large or need custom OS packages.

### Authoring a Lambda (Node.js)
### Basic input fields
- **Function name** (letters/numbers/hyphens/underscores)
- **Runtime** → choose **Node.js**
  - Common versions: Node.js 18.x / Node.js 20.x
- **Architecture** → choose **ARM64** (Graviton2)
  - cheaper & faster than x86

### Permissions (IAM Role)
- Lambda creates a **basic execution role** with CloudWatch logging permissions.
- You can:
  - Use default basic execution role (recommended for first time)
  - Select an **existing role**
  - Create a role from template policies (not recommended normally)

The execution role controls what AWS resources Lambda can access (S3, DynamoDB, etc.).

### Advanced Settings
#### VPC
- You **don’t need a VPC** unless accessing private network resources (e.g., RDS).
- Leaving VPC **empty** means public network and faster cold starts.

#### Code Signing (optional)
- Ensures only trusted, verified code is deployed.

### After Function Creation
#### Function Overview section shows:
- **Triggers**: integrations like S3 / API Gateway / EventBridge / SQS, etc.
- **Layers**: additional shared dependencies
- **Destination**: where results go (e.g. SQS on failure)
- **ARN**: unique ID for the Lambda function

#### Code Source

```js
export const handler = async (event) => {
  // TODO implement
  const response = {
    statusCode: 200,
    body: JSON.stringify('Hello from Lambda!'),
  };
  return response;
};
```

### Key parts

- `handler` is the **entry point**
- Must match the **Handler config** on the console, e.g.:
  - Handler: `index.handler`
  - Means: file `index.js` and function `handler`

Common mistakes:

- Wrong file name (`app.js` vs `index.js`)
- Wrong export name
- Missing `event` & `context` params

## Additional Panels

- **Test** tab: invoke function with dummy data
- **Monitor** tab: logs, metrics, cold starts
- **Configuration** tab: environment vars, timeout, memory, concurrency, triggers, permissions
- **Versions & Aliases**: deploy multiple versions (blue/green)

---

## Invoking an AWS Lambda Function – Key Points

### Editing & Deploying Code
- You can modify Lambda code directly in the **AWS Console editor**, but this is **not recommended for production**.
- Typically develop using **Infrastructure as Code** (e.g., AWS CDK).
- After editing code → **click Deploy** to publish a new version of the function.
- Always **deploy before testing**, otherwise the old version will run.

## Creating & Using Test Events
- Go to **Test → Configure test event** to create a test payload.
- A Lambda function can have **up to 10 saved test events**.
- Events persist across browsers/devices.
- Useful when your Lambda integrates with services like **S3 / API Gateway / DynamoDB / EventBridge**, because **each passes different event formats**.
- Choose a template (e.g., **Hello World**), edit JSON, format it, and **Save**.

## Running the Test
- Click **Test** to invoke the Lambda with the saved event.
- Output shows:
  - Test event name
  - Response (null if nothing returned)
  - Logs (console output)
  - Execution report

## Invocation Metadata Shown in Logs
- **Request ID**: a unique UUID that identifies a single invocation (useful for tracking/debugging).
- **Duration**: actual compute time taken.
- **Billed duration**: billing rounding (now per millisecond instead of 100ms).
- **Memory configured** vs **memory used**.
- Example: 128MB configured, 50MB used.

## Cold Starts
- Appears in logs as **Init Duration**
- Happens when:
  - First invocation after deploy/update.
  - Scaling to new instances due to increased load.
- Lambda loads runtime + dependencies → overhead (e.g., 135ms for example here).
- Different languages/dependencies affect cold start time.
- Warm start means Lambda already has an execution environment ready.

![invocation-metadata](./assets/invocation-metadata.png)

## Event & Context Objects
### `event`
- Contains the **input data passed to Lambda**
- Example for API Gateway: user input, query params, body, headers.
- Can be logged: `console.log(event)`
- Access values like: `event.key1`

### `context`
- Provides info about the execution environment.
- Most important value: **remaining time before timeout**.
- Used to decide whether to continue or stop long-running logic.

## Important Limitation
- Lambda runtime duration is **1 second to 15 minutes max**.
- Long processes must be broken into smaller tasks or chained.



## Printing & Reading Event Data Example (Node.js)
```javascript
exports.handler = async (event, context) => {
  console.log(event); // whole event payload
  const value = event.key1;
  console.log(value);
};
```

---



## Looking at our functions Logs

## CloudWatch Logs for AWS Lambda – Summary

### What is CloudWatch?
- CloudWatch is an AWS monitoring and logging service.
- Provides metrics, monitoring, alerts, dashboards, and **log storage**.
- We mainly use **Logs → Log groups** to view Lambda logs.

## Log Groups & Log Streams
- Each Lambda function automatically creates a **Log Group**.
- Inside a log group, logs are stored in **Log Streams**.
- A log stream typically represents **one execution environment (container)**.
- Each time:
  - A new container is spun up **(new cold start)**, OR
  - You **deploy new code**
  → A **new log stream** gets created.
- Multiple log streams appear for scaling / multiple concurrent executions.

## Retention Policy (Important for cost)
- Default is **Never Expire**.
- CloudWatch Logs cost **$0.50 per GB stored**.
- For real applications, enable **log retention policy** or archive.

## Viewing Logs
- Navigate: **CloudWatch → Logs → Log Groups → Log Streams**
- Each log stream shows console output like Lambda test console.
- Use log streams to debug individual executions.

---

## 🧠 How AWS Lambda Actually Works (Internal Architecture Notes)

### **The Myth of “Serverless”**

- Despite the name, **Lambda is not literally server-less**.
- There *are servers*—AWS just handles all provisioning & lifecycle under the hood.
- The serverless feel is a **transparent orchestration layer** that hides infrastructure complexity.

------

## **What Happens When You Invoke a Lambda**

### 1. **User triggers Lambda**

- Either from AWS Console, API Gateway, or Infra-as-Code.
- AWS assigns an **ARN** (Amazon Resource Name) which uniquely identifies the Lambda.

### 2. **Request flows through a Load Balancer**

- AWS uses an internal **load balancer** to route requests to compute resources.

### 3. **Lambda Holding Pool**

- AWS maintains a **pool of pre-initialized resources** (EC2-like compute / containers).
- These machines are **not specific to you**; they’re shared across customers until needed.

------

## **Cold Start**

Triggered when no warm environment exists.
 Process:

1. AWS pulls a machine from holding pool.
2. Loads your Lambda code + runtime + dependencies + layers.
3. Registers this instance with the load balancer.
4. Executes your request.

⏳ **Cold start adds latency** — often 100ms to multiple seconds depending on runtime & dependencies.

------

## **Scaling (Bursts of Traffic)**

- Lambda spins up **more compute containers from holding pool**.
- Each new container must also cold start once.
- Scaling is automatic and transparent.

------

## **Warm State / Reuse**

- After cold start, the compute environment stays alive for **~30–60 minutes** (not officially guaranteed).
- Subsequent invocations of the same function run much faster.

### Warm environment retained:

- When function keeps getting periodic traffic.
- Results in very low latency (warm start).

------

## **When Environments Expire**

AWS automatically releases idle containers back into the pool when:

- No more requests for a while (approx 30–60 mins).
- Account scaling down.

Next invocation after that → **cold start again**.

------

## ⚡ Start-Type Summary

| Event Type             | Explanation                                                  |
| ---------------------- | ------------------------------------------------------------ |
| **Cold Start**         | No existing environment → machine pulled from pool → dependencies loaded → slowest |
| **Partial Cold Start** | Environment exists but some setup required (dependencies / subnet / extension loading) |
| **Warm Start**         | Reuse of existing environment → code runs immediately → fastest |

------

## **Main takeaway**

Lambda’s magic isn’t “no servers”—it’s **dynamic borrowing of compute resources** + intelligent reuse.

------

If you want, I can also create:

- A visual diagram in Markdown ASCII style
- A short Anki card version
- An explanation comparing Lambda vs ECS Fargate vs EC2 in interview format

Would you like an **interview-ready explanation** next? 🚀

![low-traffic](./assets/low-traffic.png)

![initial](./assets/initial.png)

![heavy-traffic](./assets/heavy-traffic.png)