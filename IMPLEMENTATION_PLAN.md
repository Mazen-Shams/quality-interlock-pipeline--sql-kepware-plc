# Active Implementation Plan: Production Baseline Configuration

> **Configuration File:** [`bckup 28-7-2026.json`](file:///c:/Users/st1ahmedma/OneDrive%20-%20Prometeon/Desktop/SQL%20Server%20-%20Kepware%20-%20PLC%20pipeline%20flow/bckup 28-7-2026.json) (Restored from [`bckup 28-7-2026.json`](file:///c:/Users/st1ahmedma/OneDrive%20-%20Prometeon/Desktop/SQL%20Server%20-%20Kepware%20-%20PLC%20pipeline%20flow/bckup%20bckup 28-7-2026.json))

---

## 1. Active Machines Scope (Machines 2, 3, 9, 12, 22)

| Machine Code | Driver | IP / Target Address | Tags / Memory Address | Status & Configuration Details |
| :--- | :--- | :--- | :--- | :--- |
| **`02-TRGB2`** | Siemens TCP/IP | `192.168.20.102` | `1-Execute_Pulse` / `1-Tread Block Tag` | Standardized Tag Names |
| **`03-TRGB3`** | Siemens TCP/IP | `192.168.20.103` | `1-Execute_Pulse` / `1-Tread Block Tag` | Standardized Tag Names |
| **`09-TRGH9`** | Siemens TCP/IP | `192.168.20.109` | `1-Execute_Pulse` / `1-Tread Block Tag` | **Standardized to match TRGB2 & TRGB3** |
| **`12-TRGD12`** | Siemens TCP/IP | `192.168.20.112` (NAT) | `DB66.DBX0.1` / `DB66.DBX0.0` / `DB66.STRING2.250` | Fixed Link Tag Targets (`BU-12-TRGD12.CPU`) |
| **`22-TRGC22`** | AB ControlLogix | `<192.168.20.122>,1,0` | `Global.TreadBarcode_Pulse` / `NOTOK` | **Renamed from LCZ22 to TRGC22** |

---

## 2. Completed Updates Applied to `bckup 28-7-2026.json`

1. **TRGH9 Tag Standardization:** Updated PLC and Link Tag names for Machine 9 from `Excute` / `Tread Block Tag` to **`1-Execute_Pulse`** and **`1-Tread Block Tag`** to match Machines 2 & 3.
2. **Machine 22 Renamed (`22-TRGC22`):** Refactored channel `BU-22-TRGC22`, device `TRGC_22`, SP device `22-TRGC22`, and Advanced Tag Group `22-TRGC22`.
3. **Machine 12 Link Tags Fixed:** Pointed `EXECUTE` and `Block_output_link` directly to `BU-12-TRGD12.CPU` tags.

