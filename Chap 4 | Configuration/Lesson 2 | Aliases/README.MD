# Aliases

### **AWS Lambda Aliases **

- An **alias** is a **named pointer** to a specific version of a Lambda function.
- Instead of referring to a version by number (`:1`, `:2`, etc.), you can refer to it using a **name** like `prod`, `beta`, or `test`.

### **Purpose of Aliases**

- Helps avoid manually tracking version numbers when routing traffic.
- Improves **change management**, especially for testing workflows and production rollout.
- Useful in deployment pipelines where stability and gradual rollout matter.

### **How Aliases Work**

- You can have:
  - **Version numbers** → immutable snapshots (`1`, `2`, `3`…)
  - **Aliases** → readable labels pointing to versions (ex: `prod → version 1`)
- Upload new versions without immediately changing which version receives traffic.
- Only update the alias after confirming the new version works as expected.

### **Example Workflow**

1. Upload code → Version `1` created → Alias `prod` points to version `1`.
2. Upload updated code → Version `2` created → Latest now points to `2`, but `prod` still points to `1`.
3. Test version `2` using manual tests, automated tests, or custom triggers.
4. If version `2` is good → Update `prod` alias to point to version `2`.

### **Alias Traffic Weights (Canary Deployments)**

- You can split traffic between versions by weight.
- Example:

```
prod alias:
- 90% traffic → version 1
- 10% traffic → version 2
```

- This allows gradual rollout and early failure detection.
- If errors increase → revert alias back to `100% version 1`.
- If stable → promote version `2` to receive **100% traffic**.

### **Why Aliases Matter**

- Enable **safe deployments**.
- Allow **canary releases**, **A/B testing**, and **controlled production rollouts**.
- Work closely with Lambda versions and are required for advanced deployment strategies.

