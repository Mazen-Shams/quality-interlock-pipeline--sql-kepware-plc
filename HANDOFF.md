# Session Handoff & Technical Operational Report

**Date:** August 4, 2026  
**Project:** Industrial PLC – KEPServerEX – SQL Server Real-Time Tread Verification Pipeline  
**Repository Path:** Relative (`./`)

---

## 1. Executive Summary & Verified System State

During this session, we validated the latest KEPServerEX export file **[`./bckup 3-8-2026.json`](./bckup 3-8-2026.json)**, resolved the SQL Stored Procedure `NULL` parameter issue, and standardized Link Tag modes.

### Key Milestones & Operational Guidance:
1. **Machine Tracking Scope:**
   * **`09-TRGH9` is currently the sole active machine** being tracked in live production.
   * Machines `02-TRGB2`, `03-TRGB3`, `12-TRGD12`, and `22-TRGC22` are temporarily on hold (disabled in the live KEPServerEX runtime), with their complete 100% verified configuration preserved inside **[`./bckup 3-8-2026.json`](./bckup 3-8-2026.json)** for instant re-activation when ready.

2. **Resolution of SQL Stored Procedure `NULL` Parameter Issue:**
   * **Root Cause:** When KEPServerEX ODBC driver re-initializes (`Reinitialize Stored Procedure = Enable`), internal SP input tags (`input__MCH_CODE`) are reset to blank/NULL. Because static derived tags (`MCH_CODE_X`) never change value, Link Tags set to *"On Data Change"* would not re-fire, causing `@MCH_CODE = NULL` to be sent to SQL Server.
   * **Standard Fix:** Configured **`Mch_code_link` Link Mode = `On Interval`** with **Link Rate = `1000 ms` (1 second)**. This continuously refreshes the machine code string into `input__MCH_CODE` every 1 second, making it 100% immune to ODBC driver resets or re-initialization clear events.

3. **Active Configuration File:**
   * Verified **[`./bckup 3-8-2026.json`](./bckup 3-8-2026.json)** containing the complete project architecture.

---

## 2. Infrastructure Quick Reference Matrix

| Parameter / Machine | Value / IP | Driver / Details | Key Tag Mappings | Live Status |
| :--- | :--- | :--- | :--- | :--- |
| **SQL Server Host** | `sql-server.plant.local` | Database: `DCollection` | SP: `[dbo].[csp_Check_Tread]` | Operational |
| **Windows System DSN** | `LCZ_MMS` | 32-bit ODBC Driver 17 | Connection Pool Active | Operational |
| **KEPServer Host IP** | `192.168.10.50` | KEPServerEX 6 Server Host | Server Running | Operational |
| **09-TRGH9** | `192.168.20.109` | Siemens TCP/IP Ethernet | `1-Execute_Pulse` / `1-Tread Block Tag` | **Active / Tracking** |
| **02-TRGB2** | `192.168.20.102` | Siemens TCP/IP Ethernet | `1-Execute_Pulse` / `1-Tread Block Tag` | Standby (On Hold) |
| **03-TRGB3** | `192.168.20.103` | Siemens TCP/IP Ethernet | `1-Execute_Pulse` / `1-Tread Block Tag` | Standby (On Hold) |
| **12-TRGD12** | `192.168.20.112` (NAT) | Siemens S7-300 (CPU 317TF) | `DB66.DBX0.1` / `DB66.DBX0.0` / `DB66.STRING2.250` | Standby (On Hold) |
| **22-TRGC22** | `<192.168.20.122>,1,0` | AB ControlLogix Ethernet | `Global.TreadBarcode_Pulse` / `NOTOK` | Standby (On Hold) |

---

## 3. Relative File Directory Map

* **[`./pipeline_master_guide.md`](./pipeline_master_guide.md):** Master 360-degree technical architecture guide.
* **[`./HANDOFF.md`](./HANDOFF.md):** Master session handoff report.
* **[`./IMPLEMENTATION_PLAN.md`](./IMPLEMENTATION_PLAN.md):** Active field staging plan for pending plant machines.
* **[`./bckup 3-8-2026.json`](./bckup 3-8-2026.json):** Active production KEPServerEX project JSON configuration file.
* **[`./AGENTS.md`](./AGENTS.md):** Root agent instruction rules.

---

## 4. Complete Plant Machine Scope (26 Machines Documented)

| Machine Family | Total Count | Machine Identifiers |
| :--- | :--- | :--- |
| **TRGB Series** | 6 | `TRGB1`, `TRGB2`, `TRGB3`, `TRGB4`, `TRGB5`, `TRGB6` |
| **TRGC Series** | 10 | `TRGC13`, `TRGC14`, `TRGC15`, `TRGC16`, `TRGC17`, `TRGC18`, `TRGC22`, `TRGC23`, `TRGC24`, `TRGC25` |
| **TRGD Series** | 5 | `TRGD10`, `TRGD11`, `TRGD12`, `TRGD19`, `TRGD20` |
| **TRGH Series** | 3 | `TRGH7`, `TRGH8`, `TRGH9` |
| **System / Auxiliary** | 2 | `RM301`, `MES1` |

