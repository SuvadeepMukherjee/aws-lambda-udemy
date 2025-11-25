# The Anatomy of a Function

## What happens during a Lambda function execution ?

Lambda doesn’t just “run your code”; it performs several behind-the-scenes steps.

A function execution involves **four steps**, in order:

1. **Code download**
   - Lambda fetches your ZIP/JAR/executable from S3.
   - Happens when the function has no existing container.
   - More dependencies → larger package → longer download time.
2. **Start the execution environment**
   - Lambda boots a runtime environment for the language (Node.js, Python, Java, etc.).
   - Some languages start slower (Java, C#).
   - Others start fast (Node.js, Python).
   - Language choice affects startup latency.
3. **Run initialization code**
   - Everything **outside** the handler runs here.
   - Example: imports, library loading, environment variable extraction, DB clients, SDK setup.
   - Heavy imports slow this step.
   - Avoid unused dependencies and avoid “import *” patterns.
4. **Execute the handler**
   - The business logic inside the handler finally runs.
   - Returns result to caller.

## Cold Start Explained

- Cold starts occur because Lambda creates containers on-demand.

  Cold starts matter most during:

  - First invocation,
  - Long idle periods,
  - Deploying a new version,
  - Sudden traffic spikes where new containers must be created.

## Types of Starts (Full Cold Start , Partial Cold Start,Warm Start)

- Full Cold Start 

  - Happens when **no container exists**.
    - Lambda must:
      1. Download code
      2. Start execution environment
      3. Run initialization code
      4. Run handler
    - Highest latency.
    - Can be several seconds.
    - In rare cases (especially Java), can reach **up to 1 minute**.

- Partial Cold Start

  - Happens when **Lambda kept the container but discarded the initialized runtime**.

    - Code is already downloaded.

      Execution environment still exists.

      Lambda must:

      1. Run initialization code
      2. Run handler

      Faster than a full cold start, slower than a warm start.

      Import size heavily influences this stage.

- Warm Start
  - Happens when **the entire container (including initialization) is already alive**.
    - No downloading
    - No environment startup
    - No initialization
    - Just runs the handler immediately.
    - Lowest latency.
  - Warm starts mimic the experience of running on EC2: always warm, always ready.

## Scaling & Cold Starts

- Lambda can create **500 new execution environments per minute**.
- If traffic spikes suddenly (e.g., 5,000 requests at once), scaling may lag.
- During scale-up, new containers → cold starts → temporary throttling is possible.

## Strategies to Reduce Cold Starts

Minimize library dependencies; keep ZIP/JAR small.

Avoid wildcard imports; import only what you need.

Increase memory to get more CPU (helps initialization run faster).

Use **Provisioned Concurrency**:

- Keeps containers pre-initialized and always warm.
- Eliminates cold starts but increases cost.
- Best for production APIs requiring consistent low latency.

