# VAIL 🔐

## Encrypted Media Vault for Android

VAIL is a **local-first encrypted media vault for Android** designed to keep personal photos and videos inside a private, encrypted vault while giving the user direct control over importing, viewing, organizing, backing up, restoring, and migrating their data.

VAIL is built around a simple idea:

> **Your media should remain yours, locally and under your control.**

The public `VAIL-Releases` repository contains official Android release builds. The application source repository is maintained separately.

---

## ✨ Current Release

### VAIL 1.0.1

**Release tag:** `v1.0.1`  
**Version code:** `3`  
**Minimum Android:** Android 8.0+ (API 26)  
**Target Android:** API 36  
**Release type:** Production signed release

### What's new in 1.0.1

- ✅ **Batch MOVE confirmation** — moving multiple photos/videos now uses a single Android deletion confirmation instead of asking once per file.
- ✅ **Album media filters** — Album Detail now supports **All / Photos / Videos** filtering.
- ✅ **Improved filtered selection flow** — changing the album filter clears the current selection so actions always apply to the visible media set.
- ✅ **Safer MOVE handling** — encrypted copies are verified before the original media is touched.

---

## 📥 Download

Download the latest APK from the **Releases** section of this repository.

### Direct APK

The latest release provides a production-signed APK for direct Android installation.

**Recommended:** download the asset named `VAIL-v1.0.1.apk` from the `v1.0.1` release.

> Android may show a warning when installing an APK downloaded outside Google Play. This is expected for direct distribution. Only install builds downloaded from the official VAIL release repository.

---

## 🧭 What is VAIL?

VAIL is not a cloud photo gallery and not a social media application. It is a **private local-first media vault** intended for personal use.

The core workflow is:

```text
Device Gallery
      │
      ▼
  Import Media
      │
      ├── COPY ──► Encrypted copy stays inside VAIL
      │
      └── MOVE ──► Encrypted copy verified ──► original removed
      │
      ▼
 Encrypted Vault
      │
      ├── Gallery
      ├── Albums
      ├── Photos / Videos viewer
      ├── Recycle Bin
      └── Backups / Restore / Migration
```

VAIL is designed so that the encrypted vault remains the primary storage area for protected media while the user controls when content is imported, restored, moved, or permanently deleted.

---

# 🔐 Security Architecture

VAIL uses multiple security layers rather than relying on a single protection mechanism.

## Encryption

- **AES-256-GCM** for media/data encryption.
- Unique file encryption keys are used in the vault architecture.
- Encrypted content is stored locally rather than as ordinary gallery files.

## Key Derivation

- **Argon2id** is used for key derivation where supported by the application security layer.
- A **PBKDF2-HMAC-SHA512 fallback** is available for compatibility.

## Android Keystore

VAIL integrates with the **Android Keystore** for hardware-backed key protection where available.

The implementation can use:

- **StrongBox** when the device supports it.
- **TEE-backed Keystore** on compatible devices when StrongBox is unavailable.

## PIN / Vault Protection

VAIL protects access to the vault using an application-controlled PIN flow with additional anti-brute-force behavior.

Implemented protections include:

- PIN-protected vault access.
- Lockout behavior after repeated failed attempts.
- Auto-lock.
- Secure lock screen behavior.
- Screenshot protection via Android `FLAG_SECURE`.
- Release builds use R8 code shrinking/obfuscation.

## Recovery Key

VAIL includes a recovery-key flow based on a **BIP-39 mnemonic** so that vault recovery is not dependent only on remembering the normal application PIN.

The recovery material should be stored privately and securely by the user.

---

# 🗂️ Media & Vault Features

## 📥 Import Media

Import photos and videos from the device media library into the encrypted vault.

### COPY mode

COPY keeps the original media in the normal device gallery while VAIL stores an encrypted copy inside the vault.

### MOVE mode

MOVE is designed for users who want the protected copy to replace the original gallery copy.

The MOVE pipeline follows a strict sequence:

```text
Copy encrypted media
        ↓
Verify encrypted copy
        ↓
Request Android deletion consent
        ↓
Delete original only after confirmation
```

For multiple selected items, VAIL uses a **single Android confirmation request** where the platform supports batch deletion.

If the user cancels or denies the deletion request, the originals are left untouched.

---

## 🖼️ Gallery

VAIL provides an internal encrypted-media gallery for viewing protected content without exposing the encrypted vault files as normal gallery media.

Features include:

- Photo browsing.
- Video browsing.
- Multi-select.
- Media actions.
- Album organization.

---

## 📁 Albums

VAIL supports album-based organization for vault media.

Album Detail provides:

- **All** media.
- **Photos** only.
- **Videos** only.
- Photo/video counts.
- Multi-selection using the currently filtered media set.
- Empty states when a selected filter contains no media.

Changing the filter does not mutate the underlying album data; it only changes what is displayed.

---

## 🎞️ Media Viewer

VAIL includes a dedicated internal media viewer for protected content.

The project intentionally focuses on **viewing**, not editing. VAIL does not attempt to become a photo editor or video editor.

---

# 🗑️ Recycle Bin

VAIL includes a Recycle Bin for deleted vault media.

Supported actions include:

- Restore media.
- Permanently delete media.
- Automatic purge behavior for eligible items.
- Cleanup of associated metadata and album references during permanent deletion.

For MOVE operations, deletion of the original device media is separate from deletion of the encrypted vault copy.

---

# ☁️ Backup & Restore

VAIL supports encrypted backup and restore workflows, including Google Drive integration.

## Encrypted backups

Backups contain the information required to restore the encrypted vault while maintaining cryptographic integrity checks.

The backup system includes:

- Vault key envelope information.
- Integrity verification.
- Per-file SHA-256 verification.
- Protection against restoring corrupted or tampered backup content.
- Re-wrapping of file encryption keys where required by the restored vault context.

The application validates backup integrity before making vault modifications.

## Google Drive

VAIL can use Google Drive for user-controlled encrypted backups.

The Drive integration is intended for storing encrypted backup data. The backup itself remains encrypted by VAIL rather than being uploaded as ordinary photos and videos.

> **Important:** a cloud backup is still only as secure as the account and credentials protecting that cloud storage. Enable strong account security and keep recovery information private.

---

# 📱 Device Migration

VAIL includes a phone-to-phone migration flow for moving encrypted vault data between compatible devices.

The project uses a local migration protocol with:

- ECDH P-256 key agreement.
- HKDF-SHA256 key derivation.
- Authentication / SAS verification.
- HMAC-based integrity validation.

The migration flow is intended to prevent silent acceptance of an unexpected destination/device.

---

# 🕵️ Privacy / Stealth Features

VAIL includes optional privacy-oriented features such as:

- **Decoy Vault** support.
- **Panic Lock** behavior.
- Auto-lock.
- Secure-screen protection.
- A calculator-style access flow with a hidden long-press unlock interaction.

These features are intended as privacy tools, not as a guarantee of invisibility or complete forensic resistance.

---

# 🧱 Technology Stack

## Android / Application

- **React Native 0.87**
- **React 19**
- **TypeScript**
- **Kotlin native modules**
- Android platform APIs

## Security / Storage

- Android Keystore
- AES-256-GCM
- Argon2id
- PBKDF2-HMAC-SHA512 fallback
- SQLCipher
- Room database
- BIP-39 recovery mnemonic

## State / Local App Data

- Zustand
- MMKV

## Cloud / Integration

- Google Drive

## Build / Release

- Gradle / Android Gradle Plugin
- R8
- Android App Bundle (AAB)
- Production signed APK

---

# 🏗️ Architecture Overview

VAIL follows a hybrid React Native + native Android architecture.

```text
┌───────────────────────────────────────┐
│           React Native UI             │
│                                       │
│  Gallery · Albums · Viewer · Settings │
└───────────────────┬───────────────────┘
                    │
                    ▼
┌───────────────────────────────────────┐
│       TypeScript Native Bridges       │
│                                       │
│   Vault / Media / Album / Security    │
└───────────────────┬───────────────────┘
                    │
                    ▼
┌───────────────────────────────────────┐
│         Native Kotlin Layer           │
│                                       │
│ Keystore · Crypto · MediaStore · DB   │
└───────────────────┬───────────────────┘
                    │
                    ▼
┌───────────────────────────────────────┐
│         Encrypted Local Vault         │
│                                       │
│ Encrypted Media · Metadata · Keys     │
└───────────────────────────────────────┘
```

The UI is built in React Native while security-sensitive Android functionality is implemented through Kotlin native modules.

---

# 🛡️ Security Design Principles

VAIL is designed around these principles:

### Local-first
Protected media is intended to live primarily in the user's local encrypted vault.

### Encryption by default inside the vault
Vault media is stored in encrypted form rather than as ordinary gallery files.

### Fail-safe destructive operations
MOVE deletion is performed only after the encrypted copy has been verified and Android deletion consent has been obtained.

### Integrity verification
Backup restore and media movement use integrity verification before destructive actions.

### User-controlled recovery
Recovery keys and backup flows are intended to remain under the user's control.

### Defense in depth
PIN protection, Android Keystore, encryption, secure-screen flags, lockout behavior, and code obfuscation are used together.

---

# 🔄 Updating VAIL

VAIL supports normal Android APK updates when the new build is signed with the same application signing identity.

For official release updates:

1. Download the new APK from this repository's **Releases** page.
2. Open the APK on your Android device.
3. Choose **Update / Install** when Android prompts you.
4. Launch VAIL and unlock the vault.
5. Verify your media and albums after the update.

> Do **not** uninstall VAIL before updating if you need the existing local vault data to remain available. Uninstalling an app can remove its private application storage.

---

# 📦 Release Artifacts

Each public release may contain the following artifacts:

| Artifact | Purpose |
|---|---|
| `.apk` | Direct Android installation / sideloading |
| `.aab` | Google Play publishing / store distribution |

For the current direct-download workflow, the **APK** is the primary user-facing artifact.

---

# ✅ VAIL 1.0.1 Verification

VAIL 1.0.1 was verified through automated tests, build checks, and device testing.

### Automated verification

- TypeScript: **PASS**
- Jest: **84/84 tests passed**
- Android unit tests: **89/89 tests passed**

### Device verification

Verified on an Android 15 Vivo test device:

- Production APK update: **PASS**
- Existing vault/data preservation: **PASS**
- Existing encrypted media and albums accessible: **PASS**
- All / Photos / Videos filters: **PASS**
- MOVE multiple-item confirmation behavior: **PASS**
- No application crash during verification: **PASS**

---

# 🔎 Release Integrity

For VAIL 1.0.1, the production APK was signed with the official VAIL production signing certificate.

### APK

**File:** `VAIL-v1.0.1.apk`  
**SHA-256:**

```text
057D99458A735A99E82E8D689521CE37ED9C46E776FE9FE0887435D6F3DA09FC
```

### AAB

**Version:** `1.0.1`  
**SHA-256:**

```text
89AAD780AC5297081B1A71A21BE993D241695D9F42AC0318B650AC05B33C7B7F
```

The APK was additionally verified with Android's signing verification tools before release.

---

# ⚠️ Security & Privacy Notes

VAIL is a privacy-focused application, but no software should be described as completely unbreakable, invisible, or immune to all attacks.

Important practical considerations:

- Keep your master PIN private.
- Keep your BIP-39 recovery phrase private and backed up securely.
- Protect the Google account used for encrypted cloud backups.
- Only install VAIL APKs from the official release repository.
- Keep Android and the device firmware updated.
- A compromised or rooted device can weaken the overall security boundary of any application.
- Physical storage hardware may not guarantee literal zero-overwrite behavior after deletion; VAIL's security model relies on encryption and key destruction rather than claiming guaranteed physical NAND erasure.
- VAIL has not been independently audited by a third-party security auditor as part of the current public release.

---

# 🚫 What VAIL Does Not Claim

VAIL intentionally does **not** claim:

- Absolute / unbreakable security.
- Guaranteed forensic invisibility.
- Guaranteed physical NAND erasure.
- Complete protection against a compromised Android device.
- End-to-end zero-knowledge cloud architecture.
- A third-party independent security audit that has not been performed.

The goal is a strong, practical, privacy-oriented encrypted vault with user-controlled local storage, backup, recovery, and migration.

---

# 🧪 Testing Philosophy

VAIL uses multiple layers of verification before a public release:

```text
Source changes
     ↓
TypeScript check
     ↓
Jest tests
     ↓
Android unit tests
     ↓
Production build
     ↓
Signature / version verification
     ↓
Real-device upgrade test
     ↓
Manual feature QA
     ↓
Public release
```

This repository is primarily for distributing the resulting production builds rather than for serving as the application source tree.

---

# 🗺️ Project Direction

VAIL is being developed as a premium personal privacy utility focused on:

- Better vault organization.
- Safer media operations.
- Reliable encrypted backups and recovery.
- Practical device migration.
- Privacy-oriented UX.
- Stronger Android security integration.

New releases may refine existing security and UX behavior without changing VAIL's local-first design philosophy.

---

# 📜 License

See the project's source repository for the applicable license and source-availability information.

---

# 👨‍💻 Project

**VAIL — Encrypted Media Vault for Android**

Developed by **Aryan Kushwaha**.

Official source repository: `aryanKDev/VAIL`  
Official release repository: `aryanKDev/VAIL-Releases`

---

## ⭐ Support the Project

If VAIL is useful to you, consider giving the release repository a ⭐ on GitHub and sharing feedback through the project's official channels.

**VAIL — Keep your media private. Keep control in your hands. 🔐**
