# Environment Variables

### **Lambda Environment Variables — Summary Notes**

- Environment variables are simple **key–value string pairs**.
- They allow modifying a Lambda function’s behavior **without changing the code**.

------

### **Why They’re Useful**

- They help manage different settings per environment (dev, test, prod).
- Common examples stored as environment variables:
  - **Stage identifier** (e.g., `DEV`, `STAGE`, `PROD`)
  - **API or service URLs**
  - **Database connection strings**
- Using them avoids hard-coding sensitive or environment-specific data.

------

### **Accessing Environment Variables in Code**

- Example (Python):

  ```
  import os
  db_connection_string = os.getenv("DATABASE_CONNECTION_STRING")
  ```

- Lambda automatically injects some built-in variables.

  - Most relevant: `AWS_REGION` (can be retrieved the same way).

------

### **Practical Use Case**

- Two environments: **dev** and **prod**, each with:
  - Its own Lambda function deployment
  - Its own database
- Both functions use **the same code**, but call different resources.
- Difference in behavior comes purely from environment variable configuration.

------

### **Key Benefit**

- Enables flexible, environment-aware code without rewriting or redeploying logic.