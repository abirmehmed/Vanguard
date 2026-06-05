# 📋 Project Orbit: The 10x Personal Browser

> **Status:** Active Development  
> **Timeline:** 6 Months  
> **Objective:** Build a lightweight, privacy-focused, and highly optimized daily-driver browser in Rust that provides a 10x better User Experience than Brave/Chrome, using < 700MB RAM for heavy workloads.

---

##  Success Metrics (The "Definition of Done")
1. **RAM Efficiency:** 50 open tabs (with 5 active) must consume **< 700MB RAM** via the Memory Vault.
2. **Cleanliness:** 99% of ads, trackers, cookie banners, and newsletter popups are blocked automatically.
3. **Navigation Speed:** History/Bookmarks search via `Ctrl+K` must return results in **< 10ms**.
4. **Daily Driver:** The browser must be stable enough to replace Brave/Chrome as the primary browser by Month 6.

---

## 🗺️ Phase 1: The Core Shell & Navigation (Month 1)
*Goal: Build the foundational window, UI, and basic tab management.*
- [ ] **Task 1.1: Tauri & Workspace Setup**  
  Initialize a Tauri app. Configure the Rust backend and the frontend (React/Vue/Svelte or vanilla JS). Ensure it compiles and opens a blank window on Fedora Linux.
- [ ] **Task 1.2: Custom Browser Chrome UI**  
  Build the top UI bar: URL input, navigation buttons (Back, Forward, Reload), and a tab strip. Style it to look modern and minimalist.
- [ ] **Task 1.3: Webview Integration**  
  Use `wry` (via Tauri) to load a webview inside the main UI. Connect the URL bar to the webview so typing a URL and hitting Enter loads the page.
- [ ] **Task 1.4: Basic Tab Management**  
  Implement the logic to create a new tab, close a tab, and switch between tabs. Ensure the URL bar updates when switching tabs.

---

## ️ Phase 2: The Nuisance Blocker & Privacy Engine (Month 2)
*Goal: Make the web feel 10x cleaner and quieter.*
- [ ] **Task 2.1: Rust Ad-Blocker Integration**  
  Integrate the `adblock` crate (or similar) into the Rust backend. Load standard filter lists (EasyList).
- [ ] **Task 2.2: Network Request Interception**  
  Implement a custom protocol handler or navigation delegate in `wry` to intercept outgoing network requests. Block requests matching the ad/tracker lists *before* they load.
- [ ] **Task 2.3: CSS/JS Injection Engine**  
  Build a system to inject custom CSS and JavaScript into loaded pages.
- [ ] **Task 2.4: Nuisance Hiding**  
  Write a set of CSS rules and JS scripts to automatically hide "Accept Cookies" banners, newsletter modals, and chat widgets on popular websites.

---

## ️ Phase 3: The Memory Vault (Tab Suspension) (Month 3)
*Goal: Achieve the "50 tabs, 700MB RAM" holy grail.*
- [ ] **Task 3.1: Inactivity Tracker**  
  Implement a background timer in Rust that tracks the "last viewed" timestamp for every tab.
- [ ] **Task 3.2: Tab Unloading Logic**  
  When a tab is inactive for X minutes (e.g., 5 mins), destroy its webview instance to free up RAM. Keep only the URL, title, and favicon in memory (~1KB).
- [ ] **Task 3.3: Visual Snapshots**  
  Before destroying the webview, capture a lightweight screenshot or DOM snapshot and save it to disk. Display this snapshot in the tab UI to show the user what was there.
- [ ] **Task 3.4: Lazy Reloading**  
  When the user clicks a suspended tab, show the snapshot instantly, then silently reload the actual webview in the background. Restore the scroll position.

---

## 🗺️ Phase 4: The Bloat Stripper (Reader Mode) (Month 4)
*Goal: Make reading articles a distraction-free joy.*
- [ ] **Task 4.1: Content Extraction**  
  Integrate a readability algorithm (e.g., Mozilla's `Readability.js` ported to Rust, or a Rust equivalent like `readability-rs`) to extract the main text and images from an article.
- [ ] **Task 4.2: Reader View UI**  
  Build a clean, typography-focused UI for the extracted content. Add a toggle button in the URL bar to switch between "Web View" and "Reader View".
- [ ] **Task 4.3: Customization Engine**  
  Allow the user to change the font family, text size, and background color (Light/Dark/Sepia) in Reader Mode. Save these preferences locally.

---

## 🗺️ Phase 5: The Command Center & Local Data (Month 5)
*Goal: Frictionless, keyboard-first navigation.*
- [ ] **Task 5.1: SQLite Integration**  
  Set up `rusqlite` in the Rust backend. Create tables for `history` (url, title, timestamp) and `bookmarks` (url, title, folder).
- [ ] **Task 5.2: Data Persistence**  
  Hook into the webview's navigation events to automatically save visited pages to the history database. Implement a way to bookmark pages.
- [ ] **Task 5.3: The `Ctrl+K` Command Palette**  
  Build a floating search UI that appears when `Ctrl+K` is pressed. It should search across open tabs, history, and bookmarks simultaneously.
- [ ] **Task 5.4: Fuzzy Search in Rust**  
  Implement a fast fuzzy-search algorithm in Rust to filter the database results instantly as the user types in the Command Palette.

---

## ️ Phase 6: Zen Mode, Polish & Daily Driving (Month 6)
*Goal: Refine the UX and make it your primary browser.*
- [ ] **Task 6.1: Zen Mode (Auto-Hide UI)**  
  Implement logic to automatically hide the top UI bar (tabs, URL) when scrolling down or after 2 seconds of inactivity. Reveal it when the mouse moves to the top edge.
- [ ] **Task 6.2: Global Keyboard Shortcuts**  
  Map out and implement essential shortcuts: `Ctrl+T` (new tab), `Ctrl+W` (close tab), `Ctrl+L` (focus URL), `Ctrl+Shift+R` (Reader mode).
- [ ] **Task 6.3: RAM & Performance Profiling**  
  Use Linux system monitor and Rust profiling tools to measure RAM usage with 20+ tabs. Tweak the Memory Vault timers and snapshot compression to hit the < 700MB target.
- [ ] **Task 6.4: The "Daily Driver" Test**  
  Set Orbit as the default browser. Uninstall or hide Brave/Chrome. Use Orbit exclusively for 2 weeks, fixing any bugs or crashes that occur in real-world usage.

---

## ⚖️ Rules of Engagement
1. **Use Existing Engines:** We are using `wry` (WebKit/WebView2). We are *not* writing a rendering engine from scratch.
2. **Rust for the Heavy Lifting:** All ad-blocking, tab suspension logic, and search indexing must happen in the Rust backend, not the frontend JS.
3. **One Task at a Time:** We will tackle these sequentially. I will provide the exact code, dependencies, and debugging help for *one task* at a time.
4. **Embrace the Compiler:** When Rust or Tauri throws an error, we will read it together. It is our best teacher.

---

### 🚦 Next Step
Review this backlog. When you are ready to write the first line of code, reply with:

**"Start Orbit Phase 1, Task 1.1"** 

I will provide the exact `cargo` and `npm` commands to initialize the Tauri project, set up the workspace, and build the very first browser window. Let's build your perfect browser. 🚀
