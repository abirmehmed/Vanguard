# 📋 Project Vanguard: Engineering Task Backlog

> **Status:** Active Development  
> **Objective:** Build a universal, plug-and-play Polyglot Compute Offload Engine to make legacy applications 10x faster without rewriting them.

---

## 🎯 Success Metrics (The "Definition of Done")
Before marking any phase complete, the system must prove it meets these criteria:
1. **Performance:** The Rust engine processes the target heavy task in **< 5ms**, while the native PHP fallback takes **> 50ms**.
2. **Resilience:** Manually deleting the Rust pod (`kubectl delete pod`) results in **zero 500 errors** for the end-user. The Circuit Breaker must seamlessly trigger the PHP fallback.
3. **Elasticity:** Under a load test of 500+ RPS, KEDA must automatically scale the Rust deployment from 1 to 3+ replicas, keeping latency flat, then scale back down when traffic stops.
4. **Security:** A simulated compromised PHP pod *cannot* ping or access any other service in the cluster except the Rust engine on its specific port (enforced by K8s Network Policies).

---

## 🗺️ Phase 1: The Rust Core Engine
*Goal: Build the ultra-fast, bulletproof calculation engine.*

- [ ] **Task 1.1: Workspace & Axum Setup**  
  Initialize a Rust workspace. Create an `axum` HTTP server with `tokio`. Set up a basic `GET /health` endpoint that returns `200 OK`.
- [ ] **Task 1.2: The "Heavy Lift" Logic**  
  Implement the core business logic (e.g., Dynamic Flash Sale Pricing & Inventory Reservation). It must take a payload, perform complex math/validation, and return a result.
- [ ] **Task 1.3: Compile-Time Database Queries**  
  Integrate `sqlx` with a local PostgreSQL database. Write a query using the `sqlx::query_as!` macro. Prove that a typo in the SQL causes a *compile-time* error, not a runtime crash.
- [ ] **Task 1.4: Strict Error Handling**  
  Define a custom error enum using `thiserror`. Ensure all endpoints return structured, predictable JSON errors (e.g., `400 Bad Request` for validation, `500` for DB failures).
- [ ] **Task 1.5: Property-Based Testing**  
  Write `proptest` tests for the core math logic. Prove that the function never panics or returns invalid data, even when fed 10,000+ randomized, edge-case inputs.

---

## 🗺️ Phase 2: The Laravel Bridge
*Goal: Make the Rust engine feel like a native, safe part of the PHP ecosystem.*

- [ ] **Task 2.1: Composer Package Structure**  
  Initialize a local PHP package (`vanguard/vanguard-php`). Set up PSR-4 autoloading and basic PHPUnit/Pest tests.
- [ ] **Task 2.2: The Connector (FFI or gRPC)**  
  Implement the communication layer. Use PHP 8 FFI (or a gRPC client) to send the payload to the Rust engine and parse the JSON response.
- [ ] **Task 2.3: The Circuit Breaker & Fallback**  
  Implement a Circuit Breaker pattern. If the Rust engine fails or times out (e.g., > 100ms), the breaker "trips" and the code gracefully falls back to the native, slower PHP calculation.
- [ ] **Task 2.4: Latency Logging Pipeline**  
  Use Laravel’s Pipeline pattern to intercept the request. Measure the execution time. Log a structured JSON message comparing "PHP Time" vs "Rust Time" to prove the 10x speedup.

---

## 🗺️ Phase 3: Kubernetes Native & GitOps
*Goal: Deploy the system with enterprise-grade reliability, security, and auto-scaling.*

- [ ] **Task 3.1: Optimized Docker Builds**  
  Write a multi-stage `Dockerfile` for the Rust app using `lukemathwalker/cargo-chef` to cache dependencies and produce a minimal production binary (< 50MB final image).
- [ ] **Task 3.2: Helm Charts**  
  Create a Helm chart for the Rust engine. Parameterize the replica count, resource limits, and image tag in `values.yaml`.
- [ ] **Task 3.3: KEDA Auto-Scaling**  
  Install KEDA. Configure a `ScaledObject` that monitors HTTP requests per second (or a mock Redis queue) and scales the Rust deployment between 1 and 5 replicas.
- [ ] **Task 3.4: Zero-Trust Networking**  
  Apply a default-deny `NetworkPolicy` to the namespace. Create a specific policy allowing *only* the Laravel pods to communicate with the Rust pods on port 8080.
- [ ] **Task 3.5: Graceful Shutdown**  
  Configure `terminationGracePeriodSeconds` and a `preStop` hook in the Rust deployment to ensure in-flight requests finish before the pod is killed during a rollout or scale-down.

---

## 🗺️ Phase 4: The Real-Time Command Center
*Goal: Build the frontend dashboard that proves the value and allows dynamic control.*

- [ ] **Task 4.1: Strict TypeScript Frontend Setup**  
  Initialize a React/Vue project with strict TypeScript, Tailwind CSS, and a clean component architecture.
- [ ] **Task 4.2: Real-Time Telemetry Dashboard**  
  Use TanStack Query to fetch and display live metrics: Current RPS, Average Latency (PHP vs. Rust), and Active Rust Pod Count.
- [ ] **Task 4.3: Live Event Stream (SSE or WebSockets)**  
  Implement a live-updating feed that pushes real-time logs or health checks from the Rust engine to the dashboard without requiring page refreshes.
- [ ] **Task 4.4: Dynamic Configuration UI**  
  Build an admin form to update a configuration value (e.g., "Flash Sale Multiplier"). Submitting this form should update a K8s ConfigMap, triggering a safe, zero-downtime rolling update of the Rust engine.

---

## ⚖️ Rules of Engagement (How We Work)

1. **One Task at a Time:** We will tackle these tasks sequentially. I will provide the deep-dive instructions, code snippets, and debugging help for *one task* at a time.
2. **Tests are Mandatory:** No task is considered "complete" until it has passing tests (PHPUnit/Pest for PHP, `cargo test`/`proptest` for Rust).
3. **Measure Everything:** If a task claims to improve performance or security, you must prove it with data (e.g., `cargo bench`, `kubectl describe`, Lighthouse, or `k6`).
4. **Embrace the Compiler:** When Rust or TypeScript yells at you, we will read the error together. It is your best teacher.

---
