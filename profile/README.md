# 🔐 Pluto-Secure – Encrypted Password Manager for Microcontrollers

> A professional embedded security project developed in CircuitPython for the Arduino Nano RP2040 Connect. -> Transitioning to C and our custom RP2040 Board

**Pluto-Secure** is an experimental password and credential manager built from scratch to demonstrate secure design and encryption practices in constrained environments. It leverages CircuitPython and cryptographic modules to store, encrypt, and retrieve passwords securely.

This project is part of a professional portfolio to showcase full-cycle secure embedded development.

---

## 🎯 Project Goal

> Build a complete, secure password vault from concept to implementation, demonstrating secure architecture, cryptography, and hardware integration.
> Consider the usability and easy to use to it maximum level, understand the customer and add human-to-machine interaction. 

---

## 📐 Phase 1: Requirements Engineering

### ✅ Current Functional Requirements (MVP)

1. Store passwords securely in onboard flash memory.
2. Encrypt passwords using AES-CTR with a hashed fingerprint template.
3. Retrieve stored passwords only with correct authentication.
4. Store the master key securely using **hash + salt**, in our secure module ATEC608.
5. Provide a password suggestion tool:
   - Custom length
   - Customizable character set: letters, numbers, symbols
6. Communicate via Serial to chrome extension in a secure way and receive commands from it.
7. Perform a backup session with user provided key.

---

### 🧩 Future Requirements

8. Automatically identify which key to provide based on login page or context.
9. Secure export/import of encrypted data.

---

## 🛡️ Phase 2: Threat Modeling

Following the **STRIDE** model:

| Threat | Risk | Mitigation |
|--------|------|------------|
| **Spoofing** | Unauthorized access to secrets | Salted hash comparison of master key |
| **Tampering** | Modification of saved credentials | AES-CTR encryption with per-entry IV, use ATEC608 to ensure secure Boot |
| **Repudiation** | Lack of traceability | (Planned) Optional logging system |
| **Information Disclosure** | Reading `/keys.db` file directly | All data encrypted with strong symmetric encryption |
| **Denial of Service** | File deletion or filesystem corruption | Physical recovery mode and backups |
| **Elevation of Privilege** | Gaining access to all secrets | Context-based segmentation and per-entry authentication (future) |

---

## 🔐 Phase 3: Security by Design

- **Symmetric encryption**: AES-CTR with 16-byte blocks
- **Authentication**: SHA256 + salt hash of master key
- **Data integrity**: Ensured via IV separation and optional HMAC (future)
- **Separation of concerns**: clean modular architecture

---

## 📁 Project Structure

```bash
CIRCUITPY/
├── boot.py                 # Boot mode logic (safe or writable)
├── code.py                 # Main controller
├── auth_manager.py         # Master key management (registration & validation)
├── crypto_engine.py        # AES encryption engine
├── key_store.py            # Encrypted credential storage
├── keygen.py               # Secure password generator
├── ui_serial.py            # Serial interface for user input
├── /auth.db                # Salt + hash of master password
├── /keys.db                # Encrypted credentials database
└── /logs.txt               # (Optional) Access logs
