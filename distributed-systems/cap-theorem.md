Absolutely. Here are clear, exam-friendly **CAP Theorem notes** with examples and key points.

# CAP Theorem — Notes

## 1\. What is CAP Theorem?

**CAP Theorem** is a principle in distributed systems proposed by **Eric Brewer**.

It states that a distributed system cannot simultaneously guarantee all three of the following:

- **C — Consistency**
- **A — Availability**
- **P — Partition Tolerance**

In the presence of a **network partition**, a distributed system must choose between **Consistency and Availability**.

> **CAP = Consistency + Availability + Partition Tolerance**

---

## 2\. C — Consistency

Consistency means that **every read receives the most recent write or an error**.

### Example

Suppose we have two servers:

```
Server A        Server B
Balance = ₹100  Balance = ₹100
```

If ₹50 is added to Server A:

```
Server A → ₹150
Server B → ₹150
```

A consistent system ensures that clients don't read an outdated value from Server B after the update has been committed.

### Simple definition

> **Every node sees the same/latest data.**

---

## 3\. A — Availability

Availability means that **every request receives a response**, even if that response might not contain the latest data.

### Example

If Server A goes down, Server B can still respond:

```
Client → Server A ❌
       → Server B ✅
```

The system continues to serve requests.

### Simple definition

> **The system is always ready to respond.**

---

## 4\. P — Partition Tolerance

Partition tolerance means that the system continues operating even when there is a **network failure between nodes**.

### Example

Imagine:

```
Server A  ←──X──→  Server B
            Network failure
```

Server A and Server B cannot communicate with each other.

A partition-tolerant system continues functioning despite this communication failure.

### Simple definition

> **The system continues working despite network partitions.**

---

# 5\. What is a Network Partition?

A **network partition** occurs when nodes in a distributed system cannot communicate with each other.

For example:

```
       Network
          X
          |
   ┌──────┴──────┐
   ↓             ↓
Server A      Server B
```

Both servers are running, but they cannot exchange information.

This is important because **network failures are unavoidable in distributed systems**.

Therefore, real distributed systems generally need **Partition Tolerance (P)**.

---

# 6\. CAP Combinations

There are three possible combinations:

| Combination | Meaning |
| --- | --- |
| **CP** | Consistency + Partition Tolerance |
| **AP** | Availability + Partition Tolerance |
| **CA** | Consistency + Availability |

However, there's an important point:

### During a network partition:

You must choose:

```
        Partition
            |
       ┌────┴────┐
       ↓         ↓
      CP        AP
       |         |
Consistency   Availability
```

You cannot guarantee both **C and A** during the partition.

---

# 7\. CP System

**CP = Consistency + Partition Tolerance**

A CP system chooses **consistency over availability** when a partition occurs.

If nodes cannot communicate, the system may reject or delay some requests rather than return potentially stale data.

### Example

Suppose:

```
Server A = ₹100
Server B = ₹100

Network partition occurs.
```

A client sends an update to Server A.

Server B cannot know about the update.

A CP system may refuse requests from Server B until consistency can be guaranteed.

```
Request → Server B
             ↓
          ERROR
```

### Advantages

- Strong consistency
- Prevents stale/inconsistent data
- Suitable for critical transactions

### Disadvantage

- Some requests may be unavailable during a partition.

---

# 8\. AP System

**AP = Availability + Partition Tolerance**

An AP system chooses **availability over immediate consistency** during a partition.

The system continues responding even if some nodes have outdated data.

### Example

```
Server A = ₹100
Server B = ₹100

Network partition occurs.

Server A → ₹150
Server B → ₹100
```

Both servers can continue responding.

Later, when the network connection is restored, the system can synchronize the data.

This is often associated with **eventual consistency**.

### Advantages

- High availability
- System continues working during network failures
- Good for large-scale distributed applications

### Disadvantage

- Data may temporarily be inconsistent.

---

# 9\. CA System

**CA = Consistency + Availability**

A CA system provides both consistency and availability **when there is no network partition**.

The problem is that a truly distributed system cannot simply ignore network partitions.

Therefore:

> **CA is generally not considered a practical choice for systems that must tolerate network partitions.**

A traditional single-node database can often be viewed as CA because there is no distributed network partition between database nodes.

---

# 10\. Real-World Examples

| Type | Example | Priority |
| --- | --- | --- |
| **CP** | HBase | Consistency |
| **CP** | ZooKeeper | Consistency |
| **AP** | Cassandra | Availability |
| **AP** | Dynamo-style systems | Availability |
| **CA** | Single-node relational DB | Consistency + Availability |

_Exact CAP classification can depend on configuration and implementation._

---

# 11\. CAP Theorem Example — Banking

Imagine a banking system with two servers:

```
             Bank System
            /           \
       Server A       Server B
       ₹10,000        ₹10,000
```

A network partition occurs:

```
Server A   X   Server B
```

A customer deposits ₹5,000 through Server A.

Now:

```
Server A → ₹15,000
Server B → ₹10,000
```

### If the system chooses CP

Server B may stop accepting certain operations until consistency is restored.

**Result:** Consistency is preserved, but availability is reduced.

### If the system chooses AP

Server B continues accepting requests.

**Result:** Availability is preserved, but temporary inconsistency may occur.

---

# 12\. CAP vs ACID

Don't confuse **CAP** with **ACID**.

### CAP

Deals primarily with **distributed systems and network partitions**.

```
CAP
├── Consistency
├── Availability
└── Partition Tolerance
```

### ACID

Deals with **database transactions**.

```
ACID
├── Atomicity
├── Consistency
├── Isolation
└── Durability
```

The word **Consistency** has related but different meanings in the two contexts.

---

# 13\. CAP Theorem Diagram

```
             CAP
              |
      ┌───────┼───────┐
      ↓       ↓       ↓
      C       A       P
Consistency Availability Partition
                      Tolerance

            During Partition
                  |
             ┌────┴────┐
             ↓         ↓
            CP        AP
             |         |
        Consistency Availability
```

---

# 14\. Easy Way to Remember

### **C — Correct data**

Every read gets the latest consistent value.

### **A — Always responds**

Every request gets a response.

### **P — Partition survives**

System keeps operating despite network communication failures.

Remember:

> **When a Partition happens, choose C or A.**

So:

**CAP → During Partition → CP or AP**

---

# 15\. Interview Questions

### Q1. What is CAP theorem?

CAP theorem states that a distributed system cannot guarantee **Consistency, Availability, and Partition Tolerance simultaneously during a network partition**.

### Q2. What happens during a network partition?

The system has to choose between:

- **Consistency (CP)**
- **Availability (AP)**

### Q3. Why is Partition Tolerance important?

Because network failures and communication delays can occur in distributed systems. A system must be able to continue operating despite such failures.

### Q4. What is a CP system?

A system that prioritizes **Consistency and Partition Tolerance**, potentially sacrificing availability during a partition.

### Q5. What is an AP system?

A system that prioritizes **Availability and Partition Tolerance**, potentially allowing temporary inconsistency.

---

## One-Line Revision

> **CAP theorem says that in a distributed system, when a network partition occurs, you cannot guarantee both strong Consistency and Availability at the same time; you must choose between CP and AP.**