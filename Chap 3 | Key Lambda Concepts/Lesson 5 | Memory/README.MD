# Memory

## Lambda Memory

- Memory defines how much RAM your Lambda function has at runtime.
- It matters for tasks that rely on heavy computation or big dependencies.
- The memory range is **128 MB to 10,240 MB**, higher than older limits.
- Higher memory = higher cost, since pricing depends on memory allocation.
- Memory is the **strongest performance lever** because:
  - Increasing memory upgrades the underlying instance type.
  - More memory gives more CPU and more network throughput.

## **Why people misconfigure memory**

- Many developers raise memory to “fix” latency without investigating root causes.
- Throwing more memory at a problem might hide underlying issues that memory won’t resolve.

## **Pro Tips**

- Increasing memory reduces cold start time because:
  - More memory → more CPU → faster initialization.
- Cold start differences exist by language:
  - Faster: Node.js and Python.
  - Slower: Java and C#.
- Higher memory helps **networking speed**, especially when Lambda runs inside a VPC (due to connection overhead).
- Memory increases **do not give linear performance gains**:
  - Doubling memory from 128 MB to 256 MB does **not** cut execution time in half.
  - Benefits plateau after a point — more memory may give almost no improvement.

## **How to choose a memory setting**

- Start on the low end (example: **256 MB**).
- Deploy and observe CloudWatch metrics:
  - Use the built-in Lambda metric template that shows the **percentage of memory used**.
  - Avoid setting too low (causes slowdowns).
  - Avoid setting too high (wastes money).
- The aim is to find the “sweet spot,” not to max out memory blindly.

### **Tooling**

- There is a tool called **Lambda Power Tuning**.
- It benchmarks your function across all memory settings.
- It visualizes cost vs performance, helping you choose the **most cost-effective** configuration.