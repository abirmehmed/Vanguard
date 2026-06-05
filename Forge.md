# 📋 Project Forge IDE: Local AI Code Editor (Low-RAM Optimized)

> **Status:** Active Development  
> **Hardware Target:** 6GB RAM Laptop (Fedora Linux)  
> **Objective:** Build a lightweight, Tauri-based code editor with a pure-Rust AI engine that runs entirely locally using < 1.5GB total RAM.

---

## 🎯 Success Metrics (The "Definition of Done")
1. **RAM Usage:** The entire application (Editor + AI Model) must use **< 1.5GB RAM** while running.
2. **Model Size:** Must run a quantized coding model (500M - 800M parameters) entirely on CPU.
3. **Latency:** Code completion suggestions must appear in **< 2 seconds**.
4. **Offline:** Must function 100% offline with zero external API calls.

---

## 🗺️ Phase 1: The Rust AI Engine (Core)
*Goal: Build a standalone Rust CLI that can load and run a tiny AI model.*
- [ ] **Task 1.1: Project Initialization**  
  Initialize a new Rust binary crate. Add dependencies: `candle-core`, `candle-transformers`, `tokenizers`, `serde`, `serde_json`.
- [ ] **Task 1.2: Model Download Script**  
  Write a Rust script to download a quantized model (e.g., `Qwen2.5-Coder-0.5B` or `TinyLlama-1.1B` 4-bit quantized) from Hugging Face. Verify it saves to a local `models/` directory.
- [ ] **Task 1.3: Load & Infer (CLI)**  
  Write a Rust function that loads the model into memory using `candle`. Accept a text prompt via CLI args, tokenize it, run inference, and print the generated text.
- [ ] **Task 1.4: RAM Benchmark**  
  Add code to measure and print the process's RAM usage (using `sysinfo` crate). Verify it stays **< 1GB** while the model is loaded.

---

## 🗺️ Phase 2: The Tauri Desktop Shell
*Goal: Build the lightweight GUI frontend.*
- [ ] **Task 2.1: Tauri Setup**  
  Initialize a Tauri app (`npm create tauri-app`). Configure it for Linux. Verify it builds and runs a blank window.
- [ ] **Task 2.2: Code Editor Component**  
  Integrate a lightweight web-based code editor (e.g., CodeMirror 6 or Monaco Editor) into the Tauri frontend. Verify syntax highlighting works for Rust/JS.
- [ ] **Task 2.3: File System Bridge**  
  Use Tauri's Rust backend to implement `open_file` and `save_file` commands. Allow the user to load and save `.rs` or `.txt` files from the editor.

---

## 🗺️ Phase 3: Bridging AI & Editor
*Goal: Connect the frontend to the Rust AI engine.*
- [ ] **Task 3.1: IPC Command Setup**  
  Create a Tauri command `async fn generate_completion(code: String) -> String` in the Rust backend. It should accept the current code context and return the AI's suggestion.
- [ ] **Task 3.2: Frontend Integration**  
  Write a JavaScript/TypeScript function that grabs the text before the cursor, sends it to the Rust backend via `invoke()`, and displays the result in a popup or inline.
- [ ] **Task 3.3: Streaming Response**  
  Refactor the AI inference to stream tokens one by one (using Tauri's `Event` system) so the user sees the AI "typing" the response in real-time, rather than waiting for the full output.

---

## 🗺️ Phase 4: The "Copilot" Feature (Automation)
*Goal: Implement background, non-blocking code completion.*
- [ ] **Task 4.1: Debounced Trigger**  
  Add a listener to the editor that triggers 500ms after the user stops typing. It should send the current context to the AI engine silently in the background.
- [ ] **Task 4.2: Inline Ghost Text**  
  Display the AI's suggestion as "ghost text" (gray, non-editable) after the cursor. Allow the user to press `Tab` to accept it.
- [ ] **Task 4.3: RAM Guard**  
  Implement a check that unloads the model from memory if the app is idle for 5 minutes, and reloads it on demand, to keep baseline RAM usage low.

---

## 🗺️ Phase 5: Polish & Optimization
*Goal: Make it usable and prove the metrics.*
- [ ] **Task 5.1: System Monitor Widget**  
  Add a small UI element in the editor footer showing current RAM usage and "Model Loaded" status.
- [ ] **Task 5.2: Final Benchmark Report**  
  Run the app, open a file, trigger a completion. Record: Peak RAM usage, Time-to-first-token, and Total generation time. Document this in `README.md`.
- [ ] **Task 5.3: Build & Package**  
  Run `cargo tauri build` to produce a standalone Linux AppImage or deb file. Verify it runs on a fresh Fedora install (or your own system) without needing Rust installed.

---

## ⚖️ Hardware Constraints & Rules
1. **Model Limit:** We will **only** use models < 1B parameters (e.g., `Qwen2.5-Coder-0.5B`, `StableCode-0.7B`). No 7B+ models.
2. **Quantization:** All models must be **4-bit quantized** (q4_0 or q4_K_M) to minimize RAM.
3. **No GPU Required:** All inference must run on CPU. We will not rely on CUDA.
4. **One Task at a Time:** We will build this incrementally. I will provide the exact code and dependencies for each step.

---

### 🚦 Next Step
Review this backlog. When you are ready to write the first line of code, reply with:

**"Start Forge IDE Phase 1, Task 1.1"** 

I will provide the exact `cargo` commands, the `Cargo.toml` dependencies optimized for low RAM, and your first coding challenge to load the model. Let's build the future of local coding. 🚀
