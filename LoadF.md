# 📋 LoadForge: High-Performance Rust Load Tester

> **Status:** Active Development  
> **Objective:** Build a lightweight, ultra-fast, concurrent HTTP load-testing CLI tool in Rust that rivals `k6` or `hey`, with zero garbage collection pauses and a minimal memory footprint.

---

## 🎯 Success Metrics (The "Definition of Done")
1. **Performance:** The tool itself must consume minimal CPU/RAM while generating massive load (e.g., sustaining 10,000+ requests per second locally).
2. **Accuracy:** Latency percentiles (p50, p90, p95, p99) must be mathematically accurate, calculated using an industry-standard algorithm (like HDR Histogram).
3. **Concurrency:** Must safely handle hundreds of concurrent worker tasks without data races, deadlocks, or channel bottlenecks.
4. **Developer Experience:** The CLI must have a beautiful, intuitive `--help` menu, clear error messages, and a real-time progress bar.

---

## 🗺️ Phase 1: CLI Setup & Core Architecture
*Goal: Lay the foundation with strict typing and a professional CLI interface.*
- [ ] **Task 1.1: Project Initialization & Dependencies**  
  Initialize a new Rust binary crate. Add `clap` (with `derive`), `tokio`, and `reqwest` to `Cargo.toml`.
- [ ] **Task 1.2: Strict Argument Parsing**  
  Use `clap::Parser` to create a `Config` struct. It must accept: `-u`/`--url` (String), `-c`/`--concurrency` (usize), `-n`/`--requests` (usize), and `-m`/`--method` (String, default "GET").
- [ ] **Task 1.3: Basic Async HTTP Request**  
  Write a simple async function that takes the parsed `Config`, creates a `reqwest::Client`, and fires a single test request to the target URL, printing the status code and duration.

---

## 🗺️ Phase 2: Concurrency & Execution Engine
*Goal: Scale from 1 request to thousands of concurrent requests safely.*
- [ ] **Task 2.1: The Worker Pool**  
  Use `tokio::spawn` to spin up `C` (concurrency) number of asynchronous worker tasks.
- [ ] **Task 2.2: Metrics Aggregation via Channels**  
  Create a `tokio::sync::mpsc` channel. Each worker must send a `RequestResult` (containing success/failure boolean and duration in milliseconds) back to a central aggregator task.
- [ ] **Task 2.3: The Aggregator Loop**  
  Write the main aggregator task that listens to the channel, counts total successes/failures, and collects all durations until exactly `N` (total requests) have been processed.

---

## 🗺️ Phase 3: Metrics & Statistical Analysis
*Goal: Turn raw duration data into meaningful, professional-grade performance metrics.*
- [ ] **Task 3.1: Integrate HDR Histogram**  
  Add the `hdrhistogram` crate. Feed the collected durations into the histogram instead of a raw `Vec`.
- [ ] **Task 3.2: Calculate Percentiles**  
  Write a function that extracts and formats the p50, p90, p95, and p99 latency values from the histogram.
- [ ] **Task 3.3: Calculate RPS**  
  Measure the total wall-clock time of the test and calculate the true Requests Per Second (Total Requests / Total Time).

---

## 🗺️ Phase 4: Polish, UI & Output
*Goal: Make it look and feel like a world-class developer tool.*
- [ ] **Task 4.1: Real-Time Terminal UI**  
  Integrate the `indicatif` crate. Add a progress bar that updates in real-time as the aggregator receives results from the channel.
- [ ] **Task 4.2: The Final Report**  
  Print a clean, beautifully formatted summary table to the terminal when the test completes (Total Requests, Time Elapsed, RPS, Success Rate, Latency Percentiles).
- [ ] **Task 4.3: JSON/CSV Export (Bonus)**  
  Add a `-o`/`--output` flag that allows the user to save the final statistical report to a file for later analysis.

---

## ⚖️ Rules of Engagement
1. **No `unwrap()` in Production Logic:** We will handle errors gracefully using `Result` and `thiserror` or `anyhow`.
2. **Measure Everything:** We will test LoadForge *against itself* or a simple local Axum server to prove its own overhead is negligible.
3. **One Task at a Time:** We will build this incrementally. I will provide the exact boilerplate and challenges for each step.

---

### 🚦 Next Step
Review this backlog. When you are ready to write the first line of code, reply with:

**"Start LoadForge Phase 1, Task 1.1"** 

I will give you the exact `cargo` commands, the `Cargo.toml` setup, and your first coding challenge to build the CLI parser. Let's forge something incredible. 🔨🚀
