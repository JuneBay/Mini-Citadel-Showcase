# Mini Citadel - Technical Details

**Project:** Mini Citadel - Trader Decision Support System (TDSS)
**Role:** Financial Systems Architect
**Period:** 1994 - Present (Trading Experience), 2020 - Present (Mini Citadel Dev)
**Repository:** https://github.com/JuneBay/Mini-Citadel.git

---

## 📋 Project Overview

Building on system trading logic designed since the mid-1990s, Mini Citadel is a **Centralized Trader Decision Support System (TDSS)**. It orchestrates multi-API signals and aggregates market data into a high-frequency dashboard.

It acts as a **master gateway**, coordinating trading signals across multiple trader workstations, providing real-time API health monitoring, automated data archiving, and a unified visualization interface.

**Core Value:**
- **TDSS:** Centralized decision support for traders
- **Centralized Gateway:** Orchestration of multi-machine signals & API monitoring
- **Data Archiving:** Automated historical data pipeline for algo-trading
- **Performance:** 90% faster lookup (50ms → <5ms) via O(1) architecture
- **Multi-Account Management:** Scalable architecture for multiple accounts
- **Domain Expertise:** 25+ years of financial market experience

---

## 🎯 Key Achievements & Metrics

### 1. Performance Optimization
- **Lookup Speed:** 50ms → <5ms (90% Improvement)
- **Data Structure:** DataFrame (O(n)) → Dictionary (O(1))
- **Memory Efficiency:** Direct memory reference (Zero-Copy)

### 2. UI Responsiveness
- **Latency Reduced:** 500ms → 100ms (80% Reduction)
- **Update Cycle:** 500ms → 100ms polling
- **Architecture:** Replaced HTTP calls with direct memory access

### 3. Centralized Architecture
- **Centralized Gateway:** Integrating signals from multiple trader machines
- **API Health Monitoring:** Real-time status tracking of all connected APIs
- **Multi-Machine Orchestration:** Seamless coordination between distributed workstations

### 4. Data Intelligence
- **Automated Data Archiving:** Scheduled pipeline for historical analysis
- **Multi-Account Support:** Scalable design for managing multiple trading accounts
- **Unified Visualization:** Aggregating heterogeneous API streams into one dashboard

### 5. System Stability
- **Thread-Safe:** 100% thread-safe concurrent access
- **WebSocket:** Real-time data processing
- **Compatibility:** Maintained v1.0 UI/UX while redesigning the core engine

---

## 🏗️ System Architecture

### System Structure (v1.2)

```
[Kiwoom API / External APIs]
   ↓ (WebSocket)
[Centralized Gateway & Data Engine]
   ├─ Signal Orchestration
   ├─ Real-Time (RT) Data Aggregation
   └─ API Health Monitor
   ↓ (Direct Memory Reference)
[In-Memory Data Store (Hashmap)]
   ├─ Ticker → Price Data (O(1))
   ├─ Portfolio → Holdings
   └─ Account → Balance/Risk
   ↓ (100ms Polling)
[PySide6 (Qt) Dashboard]
   ├─ Unified Visualization
   ├─ Risk Analysis
   └─ Multi-Account View
```

### Architectural Evolution

#### v1.0 (Before)
- **Flow:** API → DataFrame → HTTP API → UI
- **Lookup:** O(n) Linear Search
- **Latency:** ~500ms

#### v1.2 (After)
- **Flow:** API → Hashmap → Direct Memory → UI
- **Lookup:** O(1) Hash Access
- **Latency:** <100ms
- **New:** Centralized Gateway for multi-machine control

### Key Technical Implementations

#### 1. Hashmap-Based Structure
- Dictionary-based storage keyed by ticker symbol
- O(1) lookup performance
- Memory-efficient storage

#### 2. Centralized Gateway
- Orchestrates signals from Distributed Trader Machines
- Real-time API Health Monitoring
- Single point of control for multi-account operations

#### 3. Direct Memory Reference
- Removed HTTP overhead
- Direct binding between Engine and UI
- 80% reduction in UI latency

#### 4. Automated Archiving
- Scheduled jobs to capture and store market data
- Builds datasets for backtesting and algorithm development

---

## 💻 Tech Stack

### Backend (Python)
- **Asyncio:** Asynchronous I/O for high-concurrency
- **Websockets:** Real-time data streaming
- **Pandas:** Data manipulation and analysis
- **OpenPyXL:** Excel integration

### GUI
- **PySide6 (Qt):** High-performance desktop UI
- **QThread:** Background worker threads

### Data Storage
- **In-Memory:** Dictionary/Hashmap for real-time data
- **Excel/CSV:** Persistence and backtesting data

---

## 🔧 Solved Technical Challenges

### 1. Lookup Bottleneck
**Problem:** O(n) DataFrame search took 500ms for 1,000 tickers.
**Solution:** Migrated to O(1) Hashmap/Dictionary structure.
**Result:** Lookup time reduced to <5ms (90% faster).

### 2. UI Latency
**Problem:** HTTP API overhead caused visible UI lag.
**Solution:** Implemented direct memory reference architecture.
**Result:** Latency reduced to 100ms (80% improvement).

### 3. Multi-Machine Coordination
**Problem:** Fragmented signals from multiple trading machines.
**Solution:** Built a Centralized Gateway to orchestrate all signals.
**Result:** Unified control and synchronized execution.

### 4. API Stability
**Problem:** Unknown API disconnections causing data gaps.
**Solution:** Real-time API Health Monitoring system.
**Result:** Immediate detection and auto-reconnection logic.

---

## 📊 Performance Comparison

| Metric | v1.0 (Before) | v1.2 (After) | Improvement |
|--------|--------------|-------------|-------------|
| **Lookup** | 500ms (O(n)) | **<5ms** (O(1)) | **90% Faster** |
| **UI Latency** | 500ms | **100ms** | **80% Lower** |
| **Updates** | 500ms poll | **100ms** poll | **5x Freq** |
| **Architecture** | Independent | **Centralized** | **Unified Control** |

---

## 📁 Technical Documents
- **Repository:** https://github.com/JuneBay/Mini-Citadel.git
