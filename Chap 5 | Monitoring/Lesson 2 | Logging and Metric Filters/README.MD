- Logging in AWS Lambda requires more intentional design compared to EC2 or container apps.
- Anything written to **standard output** automatically goes to **CloudWatch Logs**.
- CloudWatch is an umbrella service containing:
  - Logs
  - Metrics
  - Alarms
  - Dashboards
- When a Lambda runs, CloudWatch automatically creates:
  - A **log group** for the Lambda function
  - Multiple **log streams** — one for each execution environment/container
- This can make logs appear fragmented across many streams, making earlier debugging frustrating.
- Log entries include built-in metadata such as:
  - Timestamp
  - `START`, `END`, `REPORT` lines
  - Automatically generated **Request ID**
  - Lambda version executed
  - Duration, billed duration, memory used, memory allocated
  - Timeout messages (if execution exceeds configured time)
- Using a structured logging library is recommended because it provides:
  - Log levels (info, debug, warn, error)
  - Better searchability
  - Cleaner formatting

------

### Metric Filters

- Metric filters convert log patterns into CloudWatch metrics without writing custom metric code.
- Traditionally, developers used the **`PutMetricData` API** inside Lambda, which:
  - Increases execution time
  - Increases cost (because Lambda billing = duration x memory)
- Metric filters can reduce metric-related cost **by ~90%**, especially for high-throughput workloads.
- Best suited for:
  - High transaction rate workloads
  - Frequently invoked Lambda functions
- Typical metric log format examples:
  - `latency CreditCardServiceLatency 950`
  - `error PaymentFailure 1`
- Logging cost consideration: CloudWatch charges **$0.50 per GB of logs** stored.

------

### CloudWatch Logs Insights

- A query engine for searching and analyzing large log volumes.
- Solves the problem of searching across multiple log streams and Lambdas.
- Uses its own simple query language somewhat resembling SQL.
- Allows:
  - Pattern search
  - Time-filtered debugging
  - Aggregations
  - Visualization via histograms
- Billing model: **Pay-per-query-data-scanned** (no storage cost).
- Not ideal for large time ranges (e.g., "past year") due to cost and latency.
- Supports saving commonly used queries and embedding results into dashboards.