## **Provisioned Concurrency 

- Provisioned Concurrency keeps Lambda execution environments **pre-initialized** and ready, reducing latency and avoiding cold starts for the handler execution.
- It skips the **cold start phases**: downloading code, initializing runtime environment, and running `init` logic.
- Recommended for **latency-sensitive workloads**, especially when consistent low P99/P100 latency is required.

------

### **Auto Scaling & Configuration**

- Provisioned concurrency supports **auto-scaling based on usage**.
- You can configure:
  - A **base level** (example: always keep 10 provisioned instances).
  - A **maximum level** (example: scale up to 100 instances when traffic increases).
- Auto scaling can be triggered by:
  - Utilization thresholds (example: keep provisioned units at 70% of active usage).
  - **Scheduled scaling** for known peak hours.

------

### **Relation to Reserved Concurrency**

- Provisioned concurrency **subtracts** from reserved concurrency.
- It can never exceed the reserved concurrency value.
- Setting both values equal means:
  - **All invocations** use provisioned concurrency.
  - No overflow capacity exists — anything beyond the limit gets throttled.

------

### **Cost Considerations**

- Provisioned concurrency is **expensive**.
- Example estimate:
  - 512 MB memory
  - 1M requests/month
  - 5 provisioned units running 24/7
  - ≈ **$34/month**
- Should only be used when necessary, not by default.

------

### **Conceptual Notes**

- Provisioned concurrency blurs the line between **serverless** and **always-on compute (e.g., EC2)**.
- You get benefits like:
  - No infrastructure maintenance
  - Consistent latency
- But you pay for always-allocated resources, similar to virtual machines.

------

### **Example Behavior**

- Provisioned concurrency handles requests up to its configured limit.
- Reserved concurrency acts as an overflow buffer.
- Anything beyond both gets **throttled**.
- Matching provisioned concurrency with reserved concurrency removes the buffer and guarantees everything either:
  - Runs provisioned, or
  - Gets throttled when exceeding limits.

------

### **Best Practices / Pro Tips**

- **Set CloudWatch alarms for throttles** — early warning before production issues.
- Carefully plan concurrency needs based on traffic patterns and latency requirements.
- Ensure clients use **exponential backoff retry patterns** to avoi d retry storms.
- Increasing memory allocation can sometimes reduce concurrency needs by making executions faster — but has cost trade-offs.
- Use a **small baseline of provisioned concurrency** (example: 3–10 instances) for latency-sensitive applications to avoid cold starts during low-traffic periods.