# Virtual Private Cloud(VPC) Integration

### **VPCs & Lambda — Summary Notes**

- VPC stands for **Virtual Private Cloud**.
- VPCs create **isolated private networks** inside AWS.
- They allow fine-grained control of **network access (inbound/outbound)**.
- Used to protect sensitive data like **personal info, banking, credentials**, etc.

------

### **Subnet Types**

- **Public subnet**
  - Has direct access to the **public internet**.
  - Internet routing depends on proper **route table configuration**.
- **Private subnet**
  - **No direct public internet access**.
  - Used for sensitive resources like **databases**.

------

### **Why VPCs Matter**

- Essential for **security, compliance**, and preventing unwanted public exposure.
- Common private-subnet resources: **RDS (Postgres/MySQL), Redis, Elasticsearch, EC2 backend servers**.

------

### **Lambda + VPC**

- When creating a Lambda, you can choose whether to attach it to a **VPC**.
- Only required when the function needs to access **private resources** (ex: RDS, ElastiCache, internal services).
- A Lambda **NOT in a VPC** can still access public AWS services normally.

------

### **ENIs (Elastic Network Interfaces)**

- When Lambda runs inside a VPC, AWS creates **ENIs** (network adapters) in selected subnets.
- Historically caused **slow cold starts** (sometimes up to 15 seconds).
- In **2019**, AWS introduced **Hyperplane** networking — eliminating the cold start penalty.

------

### **Internet Access from Private Subnets**

- Lambda in a private subnet **cannot access the internet by default**.
- To enable external calls (like APIs, 3rd-party services), use:
  - **NAT Gateway** in a **public subnet**
  - Route table rule: private subnet → NAT Gateway → Internet Gateway
- **Internet Gateway** provides public internet connectivity for outbound traffic.

------

### **VPC Endpoints**

- Used to access AWS services **privately**, without going through the public internet.
- Reduce latency and improve security.
- Supported services include: **S3 and DynamoDB**.
- With an endpoint:
  - Lambda → VPC endpoint → Service (no internet exposure)

------

### **Connection Flow Examples**

1. **Private Lambda accessing database**
   - Lambda in private subnet → DB in same private subnet → works directly.
2. **Lambda calling external API**
   - Lambda → NAT Gateway → Internet Gateway → external service
3. **Lambda calling S3 (with endpoint)**
   - Lambda → VPC endpoint → S3 (no internet)