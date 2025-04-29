# **1. Introduction**

## **1.1 Background and Motivation for RBAC in Distributed Systems**

Modern distributed systems power large-scale applications across industries like finance, healthcare, and cloud computing, ensuring scalability, high availability, and efficient data processing. However, as these architectures grow in complexity, security concerns such as unauthorized access, privilege escalation, and inefficient permission management become critical. **Role-Based Access Control (RBAC)** addresses these challenges by assigning permissions to roles rather than individual users, enabling scalable and maintainable security policies. This model enhances administrative efficiency, minimizes human errors, and enforces the principle of least privilege, reducing security risks.

The **NIST-standardized RBAC model** includes core, hierarchical, constrained, and symmetric components, making it widely adopted in enterprise systems requiring granular access control. However, implementing RBAC in distributed environments, such as cloud-native applications and real-time data streaming platforms, presents challenges like **multi-cluster security, dynamic authorization, and cross-platform integration**. Traditional security models often fail to meet these demands, necessitating a **scalable RBAC framework** tailored for distributed architectures like Apache Kafka.

## **1.2 Kafka Security Challenges in Enterprise Environments**

Apache Kafka has become a **de facto standard for real-time data streaming**, enabling organizations to process and transmit vast amounts of data efficiently. Its **pub-sub architecture, scalability, and fault tolerance** make it an ideal choice for event-driven systems. However, despite its architectural strengths, Kafka's built-in security mechanisms remain **a point of concern for enterprises that require fine-grained access control**.

Kafka’s native security primarily relies on **Access Control Lists (ACLs)**, which, while effective for basic permission management, become difficult to maintain in large-scale environments. Several **security pitfalls** arise when configuring Kafka ACLs, including:

1. **Default Configuration Issues** – Many organizations leave Kafka’s default security settings unchanged, inadvertently exposing clusters to unauthorized access.
2. **Misconfigured ACLs** – Due to Kafka’s reliance on topic-based permissions, misconfiguring ACLs can either over-restrict or over-permit access.
3. **Lack of Granular Role Management** – Kafka ACLs do not inherently support a hierarchical role structure, making it challenging to enforce **role-based authorization** across multiple teams and services.
4. **Multi-Tenancy Risks** – In organizations with **multi-cluster Kafka deployments**, ensuring **consistent security policies** across clusters becomes increasingly difficult.
5. **No support for dynamic roles** - Once a access list is set and the system starts there's no way to change the the permissions or add new roles without stopping the system resulting in down time.
6. **Weak Auditing & Monitoring** – Kafka does not offer **built-in visibility into access patterns**, making security monitoring dependent on external logging and SIEM solutions.

These security limitations have prompted organizations to explore **RBAC as a more structured and scalable solution** for managing Kafka access. Unlike ACLs, RBAC provides **centralized role management**, **dynamic permission assignment**, and **hierarchical role structures**, making it a more efficient model for enterprises with **complex organizational structures** ([MeshIQ, 2023](https://www.meshiq.com/common-kafka-security-pitfalls-and-how-to-avoid-them/?utm_source=chatgpt.com)).

## **1.3 Research Objectives and Contributions**

This research paper aims to investigate the feasibility, implementation challenges, and advantages of **integrating RBAC into Kafka ecosystems**. Specifically, the study is designed to:

- **Analyze the security limitations of Kafka’s native ACL-based model**, identifying key challenges enterprises face when scaling access management.
- **Compare various RBAC implementations**, including those offered by cloud providers such as Confluent’s Kafka RBAC model, as well as custom RBAC implementations.
- **Design a reference architecture** for implementing RBAC in Kafka-based distributed systems, ensuring security, performance, and maintainability.
- **Implement and benchmark custom RBAC solutions** within Kafka environments, evaluating factors such as latency, scalability, and security resilience.
- **Provide best practices for enterprises** looking to transition from ACL-based security to a **comprehensive RBAC framework**.

Through this research, we seek to **bridge the gap between traditional ACL-based security** in Kafka and modern **RBAC-driven authorization frameworks**, ultimately contributing to **improved security and access control for real-time data streaming platforms**.

## **1.4 Paper Organization**

To provide a structured and comprehensive analysis, this paper is organized into the following sections:

- **Section 2: Fundamentals of Role-Based Access Control** – Covers the core principles of RBAC, including **role hierarchies, constraints, and permission inheritance**, as well as existing RBAC standards.
- **Section 3: Kafka’s Security Model and Limitations** – Examines **Kafka’s built-in authentication, authorization, and encryption mechanisms**, with a focus on their limitations in enterprise settings.
- **Section 4: RBAC Implementation Strategies for Kafka** – Reviews various RBAC models suitable for Kafka, including **Confluent’s RBAC approach, cloud-based solutions, and custom implementations**.
- **Section 5: Experimental Design & Methodology** – Details the **experimental setup, datasets, performance benchmarks, and security evaluation** criteria used in this research.
- **Section 6: Performance Analysis of RBAC in Kafka** – Presents **benchmark results**, analyzing the trade-offs between **security enforcement, system latency, and scalability**.
- **Section 7: Implementation Challenges & Operational Feasibility** – Discusses **common challenges in adopting RBAC for Kafka**, such as **role explosion, dynamic authorization needs, and multi-cluster synchronization**.
- **Section 8: Multi-Cluster RBAC Consistency Challenges** – Explores issues related to **cross-cluster role synchronization** and proposes strategies for **ensuring policy consistency**.
- **Section 9: Role Lifecycle Management in RBAC Systems** – Investigates best practices for **creating, modifying, and retiring roles in an RBAC-driven Kafka ecosystem**.
- **Section 10: Dynamic Authorization Frameworks** – Explores **policy-based authorization models (e.g., ABAC and PBAC)** and their potential integration with Kafka’s RBAC framework.
- **Section 11: Real-World Case Study of Kafka RBAC Implementation** – Provides **a practical case study** of an enterprise that has successfully **integrated RBAC into Kafka**, detailing lessons learned and security improvements.
- **Section 12: Research Findings & Security Implications** – Summarizes key insights from the study and discusses **real-world implications for enterprise Kafka deployments**.
- **Section 13: Future Research Directions** – Highlights potential areas for future work, including **AI-driven access control, self-adaptive security policies, and decentralized identity management** for Kafka environments.    
- **Section 14: Conclusion** – Concludes the paper by reflecting on the **importance of RBAC in Kafka security**, summarizing contributions, and suggesting best practices for adoption.

--- 
# 2. Role-Based Access Control Fundamentals
## 2.1 Core RBAC Principles and Components

Role-Based Access Control (RBAC) represents a policy-neutral access control mechanism defined around roles and privileges. In RBAC, access decisions are determined by the roles that individual users have as part of an organization. Users acquire access rights by being assigned particular roles, rather than having access rights assigned directly to them.

The fundamental components of RBAC include:
- **Users**: Entities that need to access resources within a system. In the Kafka context, these include client applications, services, and human operators that interact with the Kafka cluster.
- **Roles**: Named job functions or titles which define an authority level. Roles are typically created based on job responsibilities and qualifications. For instance, in a Kafka environment, typical roles might include "Admin," "Producer," "Consumer," or more specific designations like "PaymentProcessor" or "AuditLogger."
- **Permissions**: Approvals of a particular mode of access to one or more resources. In Kafka, these correspond to operations like READ, WRITE, CREATE, DELETE, ALTER, and DESCRIBE on resources such as topics, consumer groups, and cluster configurations.
- **Role Assignments**: The mapping of users to roles, establishing which users possess which capabilities. A user may be assigned multiple roles, and a role may have multiple users assigned to it.
- **Role Hierarchies**: In advanced implementations, roles may be structured hierarchically where senior roles inherit permissions from subordinate roles. For example, a "KafkaAdmin" role might inherit all permissions from a "KafkaMonitor" role.

The primary benefit of RBAC stems from the abstraction layer it creates between users and permissions. This indirection allows security administrators to manage users and privileges independently, significantly simplifying access management in large systems.

## 2.2 RBAC Reference Models (NIST Standards)

The National Institute of Standards and Technology (NIST) has defined a standard RBAC model (INCITS 359-2012) that provides a formal reference framework. This standard describes four models of increasing capability:

**Core RBAC**: Defines the minimum set of RBAC features including user-role assignment, permission-role assignment, and session management. This basic model enforces that permissions can only be exercised through roles.

**Hierarchical RBAC**: Extends Core RBAC with role hierarchies, where senior roles inherit permissions from junior roles. Two variants exist:
- *Limited Hierarchical RBAC*: Restricts hierarchies to simple tree structures
- *General Hierarchical RBAC*: Allows arbitrary partial orders, including multiple inheritance

**Constrained RBAC**: Adds separation of duty constraints:
- *Static Separation of Duty (SSD)*: Constraints on role assignments, preventing users from being assigned conflicting roles
- *Dynamic Separation of Duty (DSD)*: Constraints on role activation, restricting which roles can be simultaneously active in a user's session

**Symmetric RBAC**: Introduces permission-role review features, enabling administrators to determine which roles grant specific permissions and to revoke permissions from roles.

In Kafka RBAC implementations, most commercial solutions implement Core RBAC with some elements of Hierarchical RBAC, while Constrained RBAC features are less commonly implemented but critically important for regulatory compliance scenarios.

## 2.3 Evolution from ACLs to RBAC Models

Access Control Lists (ACLs) represent the foundation of Kafka's native security model, providing a simple mapping between principals (users or services) and the operations they can perform on specific resources. While effective for basic security requirements, ACLs face significant limitations as deployment complexity increases:

- **Scalability Challenges**: As organizations scale their Kafka deployments, the number of required ACL rules grows exponentially. Consider a scenario with 50 applications accessing 100 topics—managing individual ACLs for each application-topic pair would require thousands of rules.
- **Administrative Complexity**: ACLs require explicit definition for each resource-principal-permission combination. Without abstraction mechanisms, administrative overhead becomes prohibitive as clusters grow.
- **Limited Abstraction**: Kafka's native ACLs lack grouping mechanisms, making it difficult to express organizational concepts like teams, departments, or functional roles.
- **Operational Rigidity**: Changes to access patterns often require modifications to numerous ACL rules, increasing the risk of misconfiguration and security gaps.

RBAC emerged as a solution to these limitations, offering several key advantages:
- **Simplified Administration**: By grouping permissions into roles and assigning roles to users, RBAC dramatically reduces the number of administrative operations required to manage access.
- **Alignment with Organizational Structures**: Roles typically correspond to job functions, making it easier to align access controls with organizational responsibilities and business functions.
- **Reduced Complexity**: The number of relationships managed in RBAC (users to roles, and roles to permissions) is typically orders of magnitude smaller than the equivalent ACL-based system.
- **Enhanced Auditability**: RBAC simplifies access review processes, making it easier to certify that users have appropriate access levels for compliance requirements.
- **Principle of Least Privilege**: RBAC facilitates implementing least privilege access by allowing fine-grained role definitions that grant only necessary permissions.

The evolution from ACLs to RBAC in Kafka environments represents a natural progression as organizations move from development-focused deployments to enterprise-grade production systems with strict security and compliance requirements. This transition is particularly evident in regulated industries like finance, healthcare, and telecommunications, where granular access control with clear audit trails is mandated by regulatory frameworks.

---
# **3. Kafka Security Architecture**

## **3.1 Native Kafka Security Model and Limitations**

### **3.1.1 Authentication in Kafka**

Kafka supports multiple authentication mechanisms:
- **SASL**: Includes PLAIN (basic username/password), SCRAM (secure password storage), GSSAPI (Kerberos), and OAUTHBEARER (OAuth 2.0 tokens).
- **SSL/TLS Client Authentication**: Uses X.509 certificates for secure client verification.
- **Delegation Tokens**: Short-lived credentials that reduce re-authentication needs.
### **3.1.2 Kafka Authorization Framework**

Kafka’s authorization system uses ACLs to define **who can access what**, with permissions stored in **ZooKeeper** (or **KRaft** in newer versions).
- **Resources**: Topics, Consumer Groups, Clusters, etc.
- **Operations**: READ, WRITE, CREATE, DELETE, ALTER, etc.
- **Permission Types**: ALLOW (grants access) and DENY (blocks access).
- **Resource Patterns**: LITERAL (exact match), PREFIXED (grouped resources), WILDCARD (all resources of a type).
### **3.1.3 Limitations of Kafka’s Native Security**

Kafka's built-in security has key challenges:
- **Coarse-grained permissions** (no fine control over partitions or message content).
- **No built-in role abstraction**, leading to admin overhead.
- **Limited audit logging**, making compliance difficult.
- **Static ACLs**, requiring manual changes.
- **Complex multi-tenancy** setups.
- **No external policy system integration** for enterprise access control.

## **3.2 Authorization Control Points in Kafka Ecosystem**

Kafka’s authorization model defines **who can perform what actions on which resources**. However, managing access at scale requires multiple **control points** across the Kafka ecosystem.
### **3.2.1 How Kafka Brokers Handle Authorization**

The **authorization process** within a Kafka broker follows these steps:
1. **Client Authentication** → The client establishes a connection and authenticates via **SASL or TLS**.
2. **Principal Extraction** → The broker identifies the client’s identity (username, service account, or certificate).
3. **Authorization Check** → The broker determines:
    - **Resource type** (topic, consumer group, transactional ID, etc.).
    - **Requested action** (READ, WRITE, CREATE, DELETE, ALTER, etc.).
    - **Applicable ACL or external policy rule**.
4. **Decision Enforcement** → The broker **allows or denies** access based on authorization rules.

**Key Limitation:** Kafka’s default ACL-based authorizer does not support **role-based access (RBAC)**, **dynamic policies**, or **external identity providers**.
### **3.2.2 Key Authorization Control Points in Kafka**

#### 1. **Broker-Level Authorization** (Server-Side Security)

Kafka brokers enforce authorization at multiple layers:
- **ACL-Based Authorizer (`AclAuthorizer`)**
    - Kafka’s default security mechanism, defining ACLs at the broker level.
    - ACL rules are stored in **ZooKeeper (pre-KRaft) or metadata (post-KRaft)**.
    - Simple but **Difficult to manage at scale** (no roles or policy logic).
- **Custom Authorizers (`Authorizer` Interface)**
    - Kafka allows plugging in external **RBAC or ABAC** systems via the `Authorizer` API.
    - Example: **OPA (Open Policy Agent), Keycloak, or LDAP-based authorizers**.
    - **Fine-grained control** but Requires **external policy management**.
- **Principal Rewriting**
    - Allows transforming a client identity **before** enforcing authorization rules.
    - Useful for mapping **OAuth tokens → Kafka principals** in custom security setups.

#### **2. Client-Side Authorization** (Application-Level Security)

Applications interacting with Kafka can enforce **additional authorization layers** before sending requests:
- **Producer/Consumer Interceptors**
    - Custom client-side logic to validate **message-level access**.
    - Example: A consumer should only **receive messages with a specific data classification** (e.g., `security_level=high`).
- **Admin Client Security Policies**
    - Enforce additional checks before executing administrative actions (e.g., topic deletion).
#### **3️. External Security Layers** (Proxies and Middleware)

Many organizations enhance Kafka security by **deploying proxies or middleware**:
- **API Gateway / Sidecar Proxy**
    - Enforces authentication & authorization **before** Kafka receives requests.
    - Example: Envoy, Nginx, or Istio validating OAuth tokens.
    - Works well for **multi-cloud and Zero Trust** architectures.
- **Kafka Security Gateway (e.g., Confluent RBAC)**
    - Implements **centralized role-based access** across multiple clusters.
    - Example: Confluent’s **RBAC model**, where **teams manage their own permissions**.
## **3.3 Challenges in Securing Kafka Deployments**

While Kafka provides basic security mechanisms, real-world enterprise environments introduce unique challenges that require additional security layers.
### **3.3.1 Common Security Risks in Kafka Deployments**

1. **Unauthorized Access**
    - Weak authentication mechanisms (e.g., default credentials, lack of strong identity verification).
    - Lack of **fine-grained authorization** (ACLs only control high-level resource access).
2. **Data Leakage and Breaches**
    - Sensitive data (PII, financial transactions) may be exposed due to misconfigured permissions.
    - **Lack of encryption** in some data flows (Kafka supports TLS, but at-rest encryption is not native).
3. **Privilege Escalation and Lateral Movement**
    - Overly broad permissions (users with `ALL` access to topics).
    - Lack of **multi-layered security** (e.g., network segmentation, role-based access).
4. **Compliance and Auditability Challenges**
    - **Minimal audit logging** makes tracking unauthorized access difficult.
    - Enterprises often need **centralized logging** and real-time access monitoring.
5. **Security in Multi-Tenant Environments**
    - Different teams, applications, or business units sharing Kafka need strict **tenant isolation**.
    - ACL-based security does not scale well for large organizations with dynamic access needs.
6. **Dynamic Workloads and Cloud-Native Security Gaps**
    - **Dynamic Kubernetes-based Kafka clusters** need automated security policy enforcement.
    - **Short-lived workloads (serverless, CI/CD environments)** require flexible, ephemeral access controls.

### **3.3.2 Gaps in Kafka’s Native Security Model**

|**Enterprise Security Requirement**|**Kafka Native Security**|**Limitations**|
|---|---|---|
|**Identity Federation (SSO, IAM)**|SASL/OAUTHBEARER|No native OpenID Connect (OIDC) or enterprise IAM integration.|
|**Role-Based or Attribute-Based Access Control (RBAC/ABAC)**|ACLs per principal|ACLs do not support roles, groups, or fine-grained policies.|
|**Dynamic Access Control**|Static ACLs|Changes require manual updates; no real-time policy evaluation.|
|**Data Encryption at Rest**|None|Requires external tools like Kafka Connect with encryption plugins.|
|**Comprehensive Audit Logging**|Minimal Kafka logs|No built-in, detailed security event tracking.|
|**Centralized Policy Management**|Per-cluster ACLs|No unified multi-cluster policy framework.|


---

# **4. RBAC Implementation Approaches for Kafka**

Kafka does not provide a built-in role-based access control (RBAC) system. Instead, organizations must enforce access control using **Kafka’s ACL mechanism, custom authorization plugins, or external IAM-based authorization**. This section explains these three approaches, detailing how each one works from an **authorization request flow perspective**.

## **4.1 Kafka Native ACL-Based RBAC**

### **Overview**

Kafka’s built-in Access Control Lists (ACLs) provide a basic method for controlling who can **read, write, or manage Kafka topics and consumer groups**. This approach is useful when **RBAC requirements are simple** and **Kafka is managed internally** without an external IAM system.

![diagram](/myFirstVault/images/diagram-export-3-28-2025-5_40_05-AM.png)
### **How It Works**

1. **User Authentication**
	- The client (producer, consumer, or admin) connects to a Kafka broker.
	- Kafka **verifies the client’s identity** using one of the authentication mechanisms:
	    - **SSL/TLS Certificates** → Authenticates using X.509 certificates.
	    - **SASL (PLAIN, SCRAM, Kerberos, OAuth)** → Authenticates based on user credentials or tokens.
	- If authentication **fails**, the connection is **rejected immediately**.
2. **Request Initialization**
	- After authentication, the Kafka client sends a request to perform an action, such as:
	    - Producing messages to a topic.
	    - Consuming messages from a topic.
	    - Creating or deleting topics.
	    - Describing or altering configurations.
	- The Kafka broker extracts the following details from the request:
	    - **Principal (user or service account name).**
	    - **Requested operation (READ, WRITE, DELETE, etc.).**
	    - **Target resource (Topic, Consumer Group, Cluster, etc.).**
3. **ACL Verification**
	- The Kafka broker **checks the configured ACLs** to see if the user has the required permissions.
	- ACLs are stored in:
	    - **ZooKeeper** (in legacy setups).
	    - **Kafka’s metadata store** (in newer KRaft-based clusters).
	- The broker performs a **lookup** to find an ACL entry matching:
	    - **Principal** (e.g., `User:alice`).
	    - **Operation** (e.g., `WRITE`).
	    - **Resource Type** (e.g., `Topic`).
4. **Access Decision**
	- If a **matching ACL entry** explicitly **grants** permission → **Access is allowed**.
	- If no ACL entry is found or if a **deny rule** is set → **Access is denied**.
	- The broker logs the **authorization decision** for auditing purposes.
5. **Policy Enforcement & Action Execution**
	- If authorization **succeeds**, Kafka **executes the request** (e.g., message production, consumption, topic creation).
	- If authorization **fails**, Kafka **rejects the request**, sending an error response to the client.
### **Strengths & Limitations**
✔️ **Simple to configure** for small-scale deployments.  
✔️ **No external dependencies**—works natively within Kafka.  
❌ **No dynamic policy updates**—ACLs must be manually updated when roles change.  
❌ **Lacks role hierarchy**—permissions are assigned per user, not based on roles.

## **4.2 Custom Authorization Plugin (Pluggable Kafka Authorizer)**

### **Overview**

For organizations requiring **fine-grained control** over access permissions, Kafka allows **custom authorization plugins** that evaluate requests dynamically based on **role-based policies stored in an external database**. This approach enables **centralized policy management and hierarchical role structures**.
![diagram](/myFirstVault/images/diagram-export-3-28-2025-5_40_22-AM.png)
### **How It Works**

1. **User Authentication**
	- The client **authenticates** using **SSL/TLS, SASL (SCRAM, OAuth, Kerberos), or Delegation Tokens**.
	- Kafka **extracts the authenticated principal** (user/service identity) and forwards it to and SASLCallbackHandler plugin which will query the credentials from the db and verifies the request
2. **Authorization Request Sent to Custom Authorizer**
	- Kafka **forwards the access request** (user, resource, and operation) to the **custom authorizer plugin** implemented via the `kafka.security.authorizer.Authorizer` interface.
3. **Policy Lookup & Decision Logic**
	- The **custom authorizer queries an external system** (e.g., a database, LDAP, policy engine) to determine:
	    - **User roles** → Assigned roles for the authenticated principal.
	    - **Allowed permissions** → What actions (READ, WRITE, DELETE, etc.) the role grants.
	    - **Resource-level policies** → Whether the requested operation is allowed on the specific Kafka topic, consumer group, or cluster.
4. **Authorization Decision**
	- If the user’s **role has permission**, the **custom authorizer grants access**.
	- If the user **lacks permission**, the **request is denied** and Kafka returns an authorization error.
5. **Policy Management & Updates**
	- **Administrators update access policies dynamically** in the external system (e.g., a database or policy engine).
	- Kafka brokers automatically **fetch the latest policy changes** without requiring a restart.
### **Strengths & Limitations**
✔️ **Supports hierarchical roles and dynamic updates**.  
✔️ **Fine-grained control** over permissions.  
❌ **Requires external database setup** for policy storage.  
❌ **More complex than Kafka ACLs**—requires custom development and maintenance.
## **4.3 External IAM-Based RBAC (Keycloak + OPA)**

### **Overview**
For enterprises requiring **centralized IAM integration**, Kafka authorization can be managed **outside Kafka** using **Keycloak (Identity & Access Management) and Open Policy Agent (OPA) for policy enforcement**. This method enables **attribute-based access control (ABAC)** and **policy-driven RBAC**.
![diagram](/myFirstVault/images/diagram-export-3-28-2025-5_45_00-AM.png)
### **How It Works**

1.  **User Authentication via Keycloak**
    - The client first authenticates with **Keycloak**, using credentials (username/password, OAuth2, OpenID Connect, etc.).
    - If authentication is successful, **Keycloak issues a JWT (JSON Web Token)** containing user identity, roles, and attributes.
2. **Client Connects to Kafka with Token**
    - The client presents the **JWT token** to Kafka for authentication via the **SASL/OAUTHBEARER mechanism**.
    - Kafka extracts the **user identity and roles** from the token.
3.  **Authorization via OPA (Open Policy Agent)**
    - Kafka forwards the authorization request (user identity, resource, and action) to **OPA**, which acts as an external Policy Decision Point (PDP).
    - OPA evaluates the request against **predefined policies** stored in **Rego** (OPA’s policy language).
    - These policies define **role-based access control (RBAC)** rules, such as:
        - Which users/roles can produce or consume from specific topics.
        - Context-based conditions (e.g., time-based or IP-based restrictions).
4.  **Access Decision & Response**
    - OPA **returns a decision** (Allow/Deny) to Kafka.
    - If allowed, the client proceeds with the requested action; otherwise, Kafka denies access.
5. **Dynamic Policy Management**
    - Administrators update **RBAC policies in OPA** dynamically, without modifying Kafka configurations.
    - Policies are **evaluated in real-time**, enabling **centralized, fine-grained access control**.
### **Strengths & Limitations**
✔️ **Centralized IAM integration**—works across multiple services, not just Kafka.  
✔️ **Highly flexible**—policies can be modified dynamically.  
✔️ **Decouples security from Kafka**—reduces operational complexity.  
❌ **Adds network latency**—authorization checks require API calls to OPA and Keycloak.  
❌ **Requires additional infrastructure**—IAM, OPA, and a proxy layer must be maintained.
## **4.4 Comparative Analysis**

|Approach|How It Works|Pros|Cons|
|---|---|---|---|
|**Kafka ACL-Based RBAC**|Kafka checks ACLs stored in ZooKeeper to allow or deny requests.|✅ Simple & native to Kafka. ✅ No external dependencies.|❌ Static—no dynamic updates. ❌ No role hierarchy.|
|**Custom Authorization Plugin**|Kafka queries an external database for role-based permissions.|✅ Fine-grained RBAC. ✅ Supports dynamic updates.|❌ Requires external database. ❌ More complex than ACLs.|
|**IAM-Based RBAC (Keycloak + OPA)**|A proxy intercepts requests, verifies JWTs, and checks policies via OPA.|✅ Centralized IAM management. ✅ Policy-driven, flexible authorization.|❌ Adds network latency. ❌ Requires IAM, OPA, and a proxy layer.|
