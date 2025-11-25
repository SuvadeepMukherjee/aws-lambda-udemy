# Maximum Duration and Timeouts

## Maximum Duration

- A Lambda function can run for **up to 15 minutes**.
- Before 2018, the limit used to be **5 minutes**, but it was increased to support longer jobs.

## Why the Limit Matters 

- Lambda is not meant for long-running computation like video encoding, training ML models, or massive ETL pipelines that exceed 15 minutes.
- If your task naturally exceeds 15 minutes, you should:
  - Break it into smaller units of work
  - Use a workflow engine like **Step Functions**
  - Or shift to a service designed for long-lived jobs (ECS, EC2, Batch)

## What Happens When Timeout is Reached 

- Lambda **immediately aborts** the execution.
- The entire execution environment is **torn down**.
- Any asynchronous or background work inside the runtime **dies instantly**.
- A **Timeout error** is logged to CloudWatch.

## Default Timeout

- The default timeout for new Lambda functions is **3 seconds**.
- You can adjust it up to 15 minutes in console, CLI, or IaC.

## Pro Tips

- **Longer Timeouts Are \*Not\* Automatically Better**
  - Increasing timeout blindly increases risk and cost.
  - Example:
    - Your SDK call hangs for 5 minutes due to misconfigured networking.
    - If your Lambda timeout is 15 minutes, you get billed for all 5 minutes.
  - Shorter timeouts act like *circuit breakers*:
    - They help catch dependency failures early.
    - They reduce surprise billing.

-  Infinite Loops Become Expensive Fast

  - Accidental loops or unawaited promises can run until the timeout.
  - If timeout is 15 minutes, each invocation becomes 15 minutes of billing.
  - Keeping a lower timeout helps catch those logical bugs early.

- Asynchronous Work inside the function dies on Timeout

  - If your code:

    ```js
     someAsyncOp().then(() => console.log("done"));
    
    ```

    and you **don’t `await` it**, then:

    - Lambda may hit timeout
    - Execution environment is destroyed
    - That “done” code will never run

    Timeouts kill:

    - Pending promises
    - Event loop callbacks
    - Child processes
    - Timers

- Lambda Spawns No Hidden "Seperate Thread"

  - Everything happens in the **same process**.
  - If timeout occurs, it all dies instantly.