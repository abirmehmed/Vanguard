# 📋 Project Aether: High-Performance Real-Time Messaging Engine

> **Status:** Active Development  
> **Objective:** Build a hyper-efficient, real-time messaging engine in Rust capable of handling massive concurrent WebSocket connections with sub-millisecond latency, zero message loss, and seamless horizontal scaling.

---

## 🎯 Success Metrics (The "Definition of Done")
1. **Concurrency:** A single Rust instance must hold 10,000+ concurrent WebSocket connections using < 200MB of RAM.
2. **Latency:** Message routing (receive → process → broadcast) must complete in **< 5ms** (p99).
3. **Reliability:** Zero message loss or out-of-order delivery, guaranteed by monotonic `sequence_id`s and client-side reconnection logic.
4. **Scale:** Multiple Rust instances must successfully broadcast messages to each other via Redis Pub/Sub without duplication.

---

## 🗺️ Phase 1: The Core Engine (Rust Backend)
*Goal: Build the foundational WebSocket server and message routing logic.*
- [ ] **Task 1.1: Workspace & Axum Setup**  
  Initialize a Rust workspace. Create an `axum` server with `tokio`. Set up a basic `GET /health` endpoint and a `GET /ws` endpoint that successfully upgrades an HTTP connection to a WebSocket.
- [ ] **Task 1.2: Connection State Management**  
  Implement a thread-safe connection manager using `Arc<RwLock<HashMap<UserId, WebSocketTx>>>` to track active connections and broadcast messages to specific users.
- [ ] **Task 1.3: The Message Protocol**  
  Define strict, typed JSON schemas using `serde` for `ClientMessage` (send) and `ServerMessage` (receive, ack, error). Ensure all payloads include a `client_id` and `timestamp`.
- [ ] **Task 1.4: Echo & Broadcast Logic**  
  Implement a basic chat room. When a user sends a message, parse it, and broadcast it to all other connected clients in the same room. Handle graceful disconnects (remove from HashMap).

---

## 🗺️ Phase 2: State, Persistence & Scaling
*Goal: Make the system reliable, persistent, and capable of running across multiple servers.*
- [ ] **Task 2.1: Compile-Time Checked Persistence**  
  Integrate `sqlx` with PostgreSQL. Create a `messages` table. Write an async function that saves incoming messages to the DB using `sqlx::query!` or `query_as!` macros.
- [ ] **Task 2.2: Monotonic Sequence IDs**  
  Implement a mechanism to assign a strictly increasing `sequence_id` to every saved message (e.g., using a DB sequence or Redis `INCR`). Ensure the client can request "all messages after ID X".
- [ ] **Task 2.3: Redis Pub/Sub for Horizontal Scaling**  
  Integrate `redis` (with `tokio` support). When a message is received, publish it to a Redis channel. Subscribe to that channel to broadcast messages to users connected to *different* Rust server instances.
- [ ] **Task 2.4: Graceful Shutdown**  
  Implement `tokio::signal` to catch `SIGTERM`. On shutdown, send a "disconnect" frame to all active WebSockets, flush any pending DB writes, and close connections cleanly.

---

## 🗺️ Phase 3: The Web Client (The Proof)
*Goal: Build a modern, resilient frontend that proves the engine's capabilities.*
- [ ] **Task 3.1: Strict TypeScript Setup**  
  Initialize a React or Vue project with strict TypeScript, Tailwind CSS, and a clean component architecture (e.g., Vite).
- [ ] **Task 3.2: WebSocket Hook/Composable**  
  Build a custom `useWebSocket` hook that handles connecting, sending messages, and automatically reconnecting with exponential backoff if the connection drops.
- [ ] **Task 3.3: IndexedDB Caching**  
  Integrate a local storage solution (e.g., `idb` or `localforage`). Cache incoming messages locally so the chat history loads instantly on page refresh, even before the WebSocket reconnects.
- [ ] **Task 3.4: Sequence Gap Detection**  
  Implement logic in the frontend to detect if a `sequence_id` is missing (e.g., jumps from 10 to 15). If a gap is detected, automatically request the missing messages from a REST fallback endpoint.

---

## 🗺️ Phase 4: Infrastructure & Production Readiness
*Goal: Package and deploy the system to prove it can scale in a real-world environment.*
- [ ] **Task 4.1: Optimized Docker Builds**  
  Write a multi-stage `Dockerfile` for the Rust backend using `cargo-chef` to cache dependencies and produce a minimal, production-ready binary (< 50MB).
- [ ] **Task 4.2: Docker Compose Local Stack**  
  Create a `docker-compose.yml` that spins up the Rust backend, PostgreSQL, and Redis together for easy local development and testing.
- [ ] **Task 4.3: Kubernetes & Helm Deployment**  
  Write a basic Helm chart for the Rust engine. Parameterize replica count, resource limits, and environment variables (DB URL, Redis URL).
- [ ] **Task 4.4: Load Testing the Engine**  
  Write a custom Rust or `k6` script to simulate 1,000 concurrent WebSocket clients joining a room and sending messages. Measure server RAM usage and message latency.

---

## ⚖️ Rules of Engagement
1. **One Task at a Time:** We will tackle these sequentially. I will provide the exact boilerplate, dependencies, and coding challenges for *one task* at a time.
2. **No `unwrap()` in Production Logic:** We will handle errors gracefully using `Result`, `thiserror`, and proper logging (`tracing`).
3. **Measure Everything:** If a task claims to improve performance or reliability, we will prove it with data (e.g., `tokio-console`, `htop`, or load test logs).
4. **Embrace the Compiler:** When Rust yells at us, we will read the error together. It is our best teacher.

---

### 🚦 Next Step
Review this backlog. When you are ready to write the first line of code, reply with:

**"Start Aether Phase 1, Task 1.1"** 

I will provide the exact `cargo` commands, the `Cargo.toml` setup, and your first coding challenge to get the Axum WebSocket server running. Let's build. 🚀
