# Chapter 1: Introduction to System Design

This chapter introduces the fundamentals and problem-solving mindset used in system design.
# Low-Level Design (LLD) — Notes

## 1. What is Low-Level Design (LLD)?

**Definition:**  
Designing the internal structure (“skeleton”) of an application by identifying classes/objects, their relationships, data flows, and how DSA solutions plug into this structure.

### DSA vs LLD

- **DSA:** Solves isolated problems (e.g. “find shortest path in an array/graph”) using algorithms like binary search, quicksort, Dijkstra’s, heaps, etc.
- **LLD:** Determines which objects exist in the system and how they interact, then applies DSA inside that structure.

---

## 2. Illustrative Story: Two Approaches to Building “QuickRide”

### Scenario

Build a ride-booking app (“QuickRide”) like Uber/Ola.

### Anurag’s DSA-First Approach

#### 1. Problem decomposition

- Map city intersections to graph nodes, roads to edges.
- Use **Dijkstra’s algorithm** to compute the shortest route.
- Use a **min-heap (priority queue)** to match riders to the closest drivers.

#### 2. Gaps

- No identification of classes/entities:
  - User
  - Rider
  - Location
  - Notification
  - Payment
- Omits data security, such as masking phone numbers.
- Missing integration points:
  - Notifications
  - Payment gateways
- No consideration for scaling to millions of users.

---

### Maurya’s LLD-First Approach

#### 1. Entity identification

Objects/classes:

- `User`
- `Rider`
- `Location`
- `NotificationService`
- `PaymentGateway`
- etc.

#### 2. Define relationships & interactions

- How `User` and `Rider` connect via `Location`.
- How `NotificationService` and `PaymentGateway` integrate.

#### 3. Non-functional concerns

- **Data Security:** Protect personal information.
- **Scalability:** Architect code to handle millions of users without performance collapse.

#### 4. Then apply DSA

Embed:

- **Shortest-path algorithm** inside the object-oriented framework.
- **Driver-matching heap** inside the object-oriented framework.

---

# 3. Core LLD Principles & Focus Areas

## 1. Scalability

- Handle large user volumes easily.
- Code structure should allow rapid, low-effort expansion, such as:
  - Adding servers
  - Adding new features

## 2. Maintainability

- New features shouldn’t break existing ones.
- Code should be easy to:
  - Debug
  - Locate bugs
  - Modify

## 3. Reusability

- Write loosely coupled, “plug-and-play” modules.
- Example:
  - A generic notification module.
  - A generic matching algorithm.

These modules can potentially be reused across applications such as:

- Zomato
- Swiggy
- Amazon delivery

---

# 4. What LLD Is Not — LLD vs HLD

**High-Level Design (HLD)** focuses on system architecture rather than detailed code structure.

### HLD typically covers:

#### Tech Stack

- Choice of languages/frameworks.
- Example:
  - Java
  - Python
  - Spring Boot

#### Database

- SQL
- NoSQL
- Hybrid approach

#### Server Scaling & Deployment

- Autoscaling
- Load balancers
- Deployment architecture
- Cost optimization on AWS/GCP

#### Cost Considerations

- Minimizing cloud/server expenses based on system load.

---

# 5. Summary & Takeaways

- **DSA = Brain of an application**
  - Algorithms solve specific computational tasks.

- **LLD = Skeleton of an application**
  - Defines object models.
  - Defines class relationships.
  - Organizes code.
  - Determines where algorithms plug into the application.

- **HLD = Architecture**
  - Defines system-wide infrastructure.
  - Determines technology stack.
  - Defines databases.
  - Defines servers and deployment architecture.

---

# 6. Key Line to Remember

> **“If DSA is the brain, LLD is the skeleton of your application.”**

[Back to the course index](../README.md)
