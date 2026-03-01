# Alien V4 — Improved (Fabric 1.21.1)

A Fabric client mod for Minecraft 1.21.1.

> **This is a security-patched fork.** The original codebase (decompiled with CFR 0.152) contained multiple embedded backdoors. All malicious code has been identified and removed. See details below.

---

## 🔒 Security Patches (vs. Original)

The original repository contained **6 confirmed backdoors / malicious behaviors**. All have been removed in this fork:

| # | Severity | Type | File(s) | Description |
|---|----------|------|---------|-------------|
| 1 | 🔴 Critical | **Coordinate Stealing** | `ServerManager.java` | Chat trigger `nwqfVDv3vQ4GEUP` → AES-encrypted player coordinates sent to public chat (key: `426siquanjia`) |
| 2 | 🔴 Critical | **Forced Item Drop** | `ServerManager.java` | Chat trigger `RecDuJjyGWS2hnR` → forcibly throws items from inventory slots 5–8 |
| 3 | 🔴 Critical | **JVM Crash Bomb** | `ClickGui.java`, `HUD.java`, `AutoCrystal.java` | Anti-tamper: calls `ffi_call(0,0,0,0)` via JNA to crash the JVM with SIGSEGV if a hidden key isn't set correctly |
| 4 | 🔴 Critical | **Hardcoded Friend Immunity** | `FriendManager.java` | Players `KizuatoResult` and `8AI` were hardcoded as permanent friends, immune to all combat modules |
| 5 | 🟠 High | **HWID Collection** | `Vitality.java` | Collected hardware fingerprints (disk size, motherboard serial, CPU info, etc.) via `oshi` library for remote access control |
| 6 | 🟡 Medium | **External API Leak** | `EsuCommand.java` | Sent user QQ numbers to third-party API (`api.xywlapi.cc`), exposing IP and personal info |

### What Was Removed

- **`ServerManager.java`** — Deleted the entire `PacketReceive` chat listener (backdoors #1 & #2), plus `Encrypt()` / `getKey()` methods and all AES/crypto imports
- **`ClickGui.java`** — Removed `ffi_call` crash code in `onEnable()`, deleted `static key` field and its initializer
- **`HUD.java`** — Removed `ffi_call` crash code in `onUpdate()` (ran every tick)
- **`AutoCrystal.java`** — Removed `ffi_call` crash code in `onTick()` (ran every tick)
- **`PopManager.java`** — Removed `ClickGui.key` initialization that armed the crash bomb
- **`FriendManager.java`** — Removed hardcoded `KizuatoResult` / `8AI` friend checks
- **`Vitality.java`** — Removed `collectHWID()`, `performHWIDCheck()`, `LEGAL_HWIDS`, `hwidCache`, and the HWID verification UI (`Frame` class)
- **`EsuCommand.java`** — Replaced external API call with a disabled-command message

---

## Core Requirements
*   **JDK 21** (Mandatory)
*   **Fabric Loom** (Build System)

## Quick Start

**Build:**
```bash
./gradlew build
```
Artifacts are located in `build/libs/`.

**Run Client:**
```bash
./gradlew runClient
```

## ⚠️ Dependencies
This project uses `flatDir` for local dependencies.
**DO NOT delete or rename JAR files in the `lib/` directory**, or the build will fail.

Required local libraries (included in repo):
- `baritone-api-fabric-1.21.1-1.11.2.jar`
- `nether-pathfinder-1.4.1.jar`
- `satin-2.0.0.jar`
- `sodium-fabric-0.6.13+mc1.21.1.jar`

## Configuration
Configuration files are generated in `vitality/cfg/` under the game run directory.

## License
MIT
