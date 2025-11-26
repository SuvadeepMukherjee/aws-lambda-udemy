# Unreserved and Reserved Concurrency 

### **Unreserved Concurrency**

- Default concurrency mode for Lambda functions.
- All functions in an AWS account *per region* share the same concurrency pool.
- Default concurrency limit: **1000 concurrent executions**.
- If one function consumes all concurrency, **other functions get throttled**.
- Throttled requests return a **"Rate Exceeded"** error.
- The 1000 limit is a **soft limit** meant to prevent runaway/infinite invocation loops.
- You can request a higher limit via AWS Support (values like **10,000+** are possible).

### **Reserved Concurrency**

- Must be manually configured per function.
- Guarantees a fixed number of concurrent executions *for that specific function*.
- Reserved capacity is **removed from the unreserved pool**.
  - Example: If default is 1000 and Function A reserves 200 → remaining unreserved = **800**.
- Even if the reserved function has **zero traffic**, its reserved capacity remains blocked from others.
- Acts as a **hard limit**:
  - If reserved concurrency = 100, invocation 101 will be throttled.
  - It **cannot borrow** from the unreserved pool.

### **Why/How to Use Reserved Concurrency**

- Useful for **isolation** of critical workloads (e.g., production vs dev/test Lambda).
- Can be used to:
  - **Slow down processing** (rate limiting).
    - Example: SQS + Lambda → prevent flooding downstream services like databases.
    - Setting reserved concurrency to something like **5–10** ensures controlled processing.
  - **Guarantee high throughput** for important workloads.
    - Example: Setting reserved concurrency = **1000** ensures scaling capacity for that function.