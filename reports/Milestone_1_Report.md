# Milestone 1 Completion Report: Native WebSocket RPC Plugin

**Project:** Native WebSocket RPC Layer for Low-Latency WAX Chain Access

**Milestone:** 1 (Core Plugin Development)

**Status:** All tasks in Sprint 1 and Sprint 2 have been successfully implemented and tested.

---

## Sprint 1 Deliverables

### 1. Design WebSocket RPC protocol and message schema [0.5]
- **Status:** ✅ **Completed**
- **Details:** - Designed a robust binary header format (`ws_header`) containing `request_id`, `type`, and `payload_size` for efficient parsing.
  - Implemented JSON payload handling within binary frames to ensure compatibility with existing EOSIO data structures.
  - Integrated **Zlib compression** logic to optimize data transmission bandwidth.

### 2. Integrate WebSocket server into nodeos plugin architecture [0.5]
- **Status:** ✅ **Completed**
- **Details:** - Successfully developed `chain_ws_plugin` as a native `appbase::plugin` inside `nodeos`.
  - Implemented a dedicated **Thread Pool** for WebSocket I/O to ensure the main chain thread remains unblocked (Non-blocking architecture).
  - Utilized **Boost.Asio C++20 Coroutines** and **Strands** to manage concurrency and prevent race conditions safely.

### 3. Implement core request–response handling (e.g. get_info, get_block) [1]
- **Status:** ✅ **Completed**
- **Details:** - Implemented the `api_dispatcher` to map WebSocket request types to internal Chain API calls.
  - Successfully tested core read-only methods (`get_info`, `get_block`) with sub-millisecond response times (<1ms).

---

## Sprint 2 Deliverables

### 1. Implement additional WebSocket RPC handlers (e.g. get_account, push_transaction) [1]
- **Status:** ✅ **Completed**
- **Details:** - Expanded the dispatcher to support the full range of standard Chain APIs including:
    - **Read-Only:** `get_table_rows`, `get_account`, `get_abi`, `get_code`, etc.
    - **Read-Write:** `push_transaction`, `send_transaction` (with async processing support).
  - Implemented efficient C++ Macros (`CALL_RO_API`, `CALL_RW_API_ASYNC`) to standardize API calls and error handling.
  - Added support for specialized APIs like `get_transaction_status` and `get_accounts_by_authorizers` with proper error mapping for disabled features.

### 2. Documentation, build instructions, and example usage [0.5]
- **Status:** ✅ **Completed**
- **Details:** - Created a comprehensive `README.md` encompassing:
    - Build and installation guide within the `nodeos` source tree.
    - Configuration options (`config.ini`) for port, address, and thread count.
    - Full Protocol Specification (Header/Payload structure).
    - JavaScript/Node.js client examples for connecting and parsing responses.
  - Codebase fully documented with License headers and maintainable comments.

---

## Technical Achievements (Summary)

* **Performance:** Achieved ultra-low internal processing latency (~0.5ms) for standard queries within a Node.js environment. By maintaining a persistent connection, it effectively eliminates the 20ms+ overhead typically seen in initial HTTP handshakes and reduces subsequent request latency by over 50%.
* **Stability:** Implemented backpressure mechanisms (`max_pending`) and graceful shutdown procedures to ensure node stability under load.
* **Completeness:** The plugin is now fully functional and ready for the next phase (Gateway development).


## Delivered Codebase

| Repository | Description | Link |
| :--- | :--- | :--- |
| **WAX Blockchain (Fork)** | Modified `nodeos` integrated with the WebSocket architecture. | [![GitHub](https://img.shields.io/badge/GitHub-Repo-181717?style=for-the-badge&logo=github)](https://github.com/peeraphat-lakboon/wax-blockchain) |
| **Chain WS Plugin** | Core C++ source code for the native WebSocket plugin. | [![GitHub](https://img.shields.io/badge/GitHub-Repo-181717?style=for-the-badge&logo=github)](https://github.com/peeraphat-lakboon/chain-ws-plugin) |

---
