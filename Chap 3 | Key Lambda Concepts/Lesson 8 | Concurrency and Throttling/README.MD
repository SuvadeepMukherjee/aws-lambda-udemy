# Concurrency and Throttling

## Concurrency 

- Concurrency in AWS Lambda refers to how many requests are being handled at the same time.
- Each request that arrives while another is still being processed creates a new execution environment (container).
- Lambda *seems* infinitely scalable, but that’s only partly true—there are limits.
- AWS accounts have a **default concurrency limit of 1000 per region**.
- All Lambda functions in that account & region share this 1000 pool.
- If a function uses all available concurrency, other Lambda functions will start failing.
- **Throttling** happens when Lambda can’t process a request because concurrency is exhausted.
- When throttled, Lambda throws a `"Rate Exceeded"` error back to the caller (API, SNS, SDK, etc.).

## Three Types of Concurrency in Lambda

1. **Unreserved Concurrency**
   - Default mode.
   - All functions share the same concurrency pool.
2. **Reserved Concurrency**
   - You allocate fixed concurrency to a specific Lambda.
   - Guarantees capacity for that function.
   - But it also caps it—if it hits its reserved limit, it gets throttled even if unused concurrency exists elsewhere.
3. **Provisioned Concurrency**
   - Always-on warm execution environments.
   - Avoids cold starts.
   - Best for latency-critical workloads.
   - Costs extra.