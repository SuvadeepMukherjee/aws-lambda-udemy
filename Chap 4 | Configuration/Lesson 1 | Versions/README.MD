# Lambda Versions

- Versions help manage **changes to Lambda functions over time**, including code and configuration updates.
- They allow multiple, independent copies of a Lambda function to exist simultaneously.
- Useful for **controlled rollouts**, testing, and safe change management.

------

### **How Versions Work**

- A version is a **snapshot** of the Lambda's code and settings at the moment it is published.
- Once published:
  - Code and settings are **locked** and cannot be changed.
  - Updates require publishing a **new version**.
- Version numbers are **incremental** starting from `1`.

------

### **Why Versions Matter**

- Enable **quick rollbacks**:
  - If a new deployment breaks something, you can immediately revert to a previous working version.
- Required for some advanced features, e.g., **Provisioned Concurrency**.
- Improve reliability and minimize deployment risk.

------

### **Using `$LATEST`**

- `$LATEST` is a special auto-updating pointer to the most recently deployed code/state.
- You can choose to:
  - Use `$LATEST` for rapid development.
  - Use locked versions for stability in production.
- Most simple applications start by relying only on `$LATEST`.

------

### **ARN Example**

```
arn:aws:lambda:<region>:<account-id>:function:<function-name>:<version-number>
```

- Adding `:<version>` targets a specific version (e.g., `:1`).
- Omitting the version uses `$LATEST`.

------

### **Versioning Lifecycle Example**

1. Upload first code → becomes **Version 1** → `$LATEST` points to it.
2. Update code and deploy → becomes **Version 2** → `$LATEST` now points to Version 2.
3. Deploy again → becomes **Version 3** → `$LATEST` now points to Version 3.