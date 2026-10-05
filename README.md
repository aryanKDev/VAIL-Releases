# VAIL 🔐

## Encrypted Media Vault for Android

VAIL is a **local-first encrypted media vault for Android** built for people who want private photos and videos stored inside an encrypted vault with practical tools for viewing, organizing, backing up, recovering, and moving their data.

> **Keep your media private. Keep control in your hands.**

This repository is the **official public release repository** for VAIL. Application source code is maintained separately.

---

## 🚀 Latest Release — VAIL 1.0.2

**Release tag:** `v1.0.2`  
**Version code:** `4`  
**Minimum Android:** Android 8.0+ (API 26)  
**Target SDK:** API 36  
**Release type:** Production-signed APK

### What's new in 1.0.2

- ✅ **Batch MOVE confirmation** — moving multiple photos and videos now uses a single Android deletion confirmation where supported, instead of asking once per file.
- ✅ **Album filters** — Album Detail now supports **All / Photos / Videos**.
- ✅ **Safer MOVE workflow** — VAIL verifies the encrypted copies before the original media is touched.
- ✅ **Improved filtered selection** — changing the album filter clears the current selection so actions apply only to the currently visible media.
- ✅ **Scoped Media Viewer navigation** — opening media from a filtered album now keeps viewer navigation within the active **All / Photos / Videos** filter.
- ✅ **Viewer navigation fix** — photos and videos no longer appear outside the selected filter while swiping.
- ✅ **Improved single-item handling** — filtered albums with one item no longer allow invalid next/previous navigation.

[**Download VAIL 1.0.2 →**](../../releases/tag/v1.0.2)

---

# 📥 Download & Install

Download the latest official VAIL APK directly from the GitHub Releases page.

### 🚀 VAIL 1.0.2

[⬇️ **Download VAIL 1.0.2 APK**](https://github.com/aryanKDev/VAIL-Releases/releases/download/v1.0.2/app-release.apk)

Or view all available releases:

[📦 **View All Releases**](https://github.com/aryanKDev/VAIL-Releases/releases)

### Installation

1. Download the APK using the link above.
2. Open the downloaded `app-release.apk` file on your Android device.
3. Approve Android's installation prompt.
4. Launch VAIL and unlock your vault.

### ⚠️ About APK Installation

Because VAIL is currently distributed directly as an APK rather than through Google Play, Android may show a warning about installing an app from an external source. This is normal for a sideloaded APK.

For safety, download VAIL only from this official release repository:

[🔐 **Official VAIL Releases**](https://github.com/aryanKDev/VAIL-Releases/releases)

> **Important:** When updating VAIL, install the new APK over the existing installation.  
> Do **not uninstall VAIL first** if you need your existing local vault data to remain available.

# 🔄 Updating VAIL

VAIL is designed to support normal Android updates when the new build is signed with the same application signing identity.

To update:

1. Download the newer APK from the official **Releases** page.
2. Open it on the device where VAIL is already installed.
3. Choose **Update / Install** when Android prompts you.
4. Open VAIL and unlock the vault.
5. Verify your media and albums after the update.

> **Do not uninstall VAIL before updating** if you need the existing local vault data to remain available. Uninstalling an Android app can remove its private application storage.

---

# 🧭 What is VAIL?

VAIL is a **private, local-first media vault**. It is designed around keeping protected media inside an encrypted local vault rather than turning the app into another cloud photo gallery.

The main flow is:

```text
Device Gallery
      │
      ▼
  Import Media
      │
      ├── COPY ──► Encrypted copy stored in VAIL
      │
      └── MOVE ──► Encrypted copy verified ──► original removed
      │
      ▼
 Encrypted Vault
      │
      ├── Gallery
      ├── Albums
      ├── Photo / Video Viewer
      ├── Recycle Bin
      └── Backup / Restore / Migration
```

---

# ✨ Features

## 🔐 Secure Vault

- PIN-protected vault access.
- Biometric unlock support on compatible devices.
- Auto-lock.
- Failed-attempt lockout behavior.
- Secure-screen protection to reduce screenshot and screen-capture exposure.
- Local encrypted storage for protected media.

## 🖼️ Private Media Gallery

Browse photos and videos stored inside the encrypted vault using VAIL's internal gallery and viewer.

VAIL focuses on **viewing and organizing protected media**. It is intentionally not a photo editor or video editor.

## 📥 COPY & MOVE Import

Choose how imported media should behave.

### COPY

The original media remains in the normal device gallery while VAIL stores an encrypted copy inside the vault.

### MOVE

VAIL first creates and verifies the encrypted vault copy. Only after successful verification does it request Android permission to remove the original.

For multi-item MOVE operations, VAIL requests a **single batch deletion confirmation where Android supports it**.

If the deletion request is cancelled or denied, the original media is left untouched.

---

# 📁 Albums

Organize vault media into albums and quickly filter the content you want to see.

Album Detail supports:

| Filter | Shows |
|---|---|
| **All** | Photos + videos |
| **Photos** | Photos only |
| **Videos** | Videos only |

Additional behavior:

- Media counts are shown for photos and videos.
- Multi-selection works with the active filter.
- Changing the filter clears the current selection.
- Empty filter states tell you when no photos or videos match the selected filter.
- Filtering changes the displayed view without changing the underlying album data.

---

# 🗑️ Recycle Bin

Deleted vault media can be managed through VAIL's Recycle Bin.

Supported actions include:

- Restore deleted media.
- Permanently delete items.
- Automatic purge behavior for eligible items.
- Cleanup of related vault metadata and album references during permanent deletion.

The Recycle Bin is separate from the Android device gallery. MOVE operations deal with the original device media independently from the encrypted vault copy.

---

# ☁️ Encrypted Backup & Restore

VAIL supports encrypted backup and restore workflows, including **Google Drive** integration.

The backup system is designed to preserve the cryptographic information needed to recover the vault while validating backup integrity before making destructive changes.

Key properties include:

- Vault key envelope information for recovery.
- HMAC-based integrity validation.
- Per-file SHA-256 verification.
- Validation before modifying the destination vault.
- Re-wrapping of file encryption keys when required by the restored vault context.

### Google Drive

VAIL can store encrypted backups in the user's Google Drive account.

The cloud copy is intended to remain an **encrypted backup**, not a normal folder of viewable photos and videos.

> Keep the Google account, VAIL PIN, and recovery information protected. Cloud storage security also depends on the security of the account controlling it.

---

# 🔑 Recovery Key

VAIL provides a recovery-key flow based on a **BIP-39 mnemonic**.

The recovery phrase is important for vault recovery and should be stored privately in a secure offline location.

Do not share your recovery phrase with anyone.

---

# 📱 Phone-to-Phone Migration

VAIL includes a local migration workflow for transferring an encrypted vault between compatible devices.

The migration protocol uses cryptographic key agreement and integrity/authentication checks, including:

- ECDH P-256
- HKDF-SHA256
- SAS verification
- HMAC integrity validation

The goal is to make the transfer explicit and verifiable rather than silently trusting an unexpected device.

---

# 🕵️ Privacy-Oriented Features

VAIL includes additional privacy-focused controls such as:

- **Decoy Vault** support.
- **Panic Lock** behavior.
- Calculator-style access flow with a hidden long-press unlock interaction.
- Auto-lock.
- Secure-screen protection.

These are privacy tools, not guarantees of invisibility or complete forensic resistance.

---

# 🛡️ Security Architecture

VAIL uses several layers of protection rather than depending on one mechanism.

### Media encryption

- **AES-256-GCM** for encrypted media/data.
- File encryption keys are protected within the vault's key hierarchy.

### Key derivation

- **Argon2id** where supported by the security layer.
- **PBKDF2-HMAC-SHA512** fallback for compatibility.

### Android Keystore

VAIL integrates with Android Keystore for protected key material and can use:

- **StrongBox** on supported devices.
- **TEE-backed Keystore** on compatible devices when StrongBox is unavailable.

### Application hardening

Release builds use Android production signing and **R8 code shrinking/obfuscation**. Secure-screen controls are also used to reduce exposure through screenshots and screen recording surfaces.

---

# 🧱 Technology Stack

VAIL uses a hybrid React Native + native Android architecture.

### Application

- React Native 0.87
- React 19
- TypeScript
- Kotlin native modules
- Android platform APIs

### Storage & security

- AES-256-GCM
- Argon2id
- PBKDF2-HMAC-SHA512 fallback
- Android Keystore
- SQLCipher
- Room
- BIP-39 recovery mnemonic

### State / local data

- Zustand
- MMKV

### Cloud

- Google Drive

### Build

- Gradle / Android Gradle Plugin
- R8
- Android App Bundle support
- Production-signed APK releases

---

# 🏗️ Architecture Overview

```text
┌───────────────────────────────────────┐
│           React Native UI             │
│                                       │
│ Gallery · Albums · Viewer · Settings │
└───────────────────┬───────────────────┘
                    │
                    ▼
┌───────────────────────────────────────┐
│       TypeScript Native Bridges       │
│                                       │
│   Vault · Media · Album · Security    │
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

---

# 🔒 Security Principles

### Local-first

The protected vault is designed primarily around local encrypted storage under the user's control.

### Verify before destructive actions

MOVE operations verify encrypted copies before requesting deletion of original device media.

### Integrity before restore

Backup content is validated before the restore process modifies the vault.

### Defense in depth

Encryption, key derivation, Android Keystore, PIN protection, lockout behavior, secure-screen controls, and application hardening are combined rather than treated as separate guarantees.

### User-controlled recovery

Recovery and backup workflows are intended to keep the user in control of the information needed to recover the vault.

---

# ✅ VAIL 1.0.2 Validation

VAIL 1.0.2 was verified through automated checks and targeted real-device update verification.

### Automated tests

- **TypeScript:** PASS
- **Jest:** 93/93 tests passed
- **Android unit tests:** PASS

### Real-device verification

The production 1.0.2 build was verified through automated checks and the latest debug update was installed in-place on an Android 15 Vivo test device without uninstalling the existing app.

Verified:

- ✅ Installation/update success.
- ✅ Existing vault data preserved.
- ✅ Existing encrypted media and albums accessible.
- ✅ All / Photos / Videos album filters working.
- ✅ Filtered Media Viewer navigation stays within the active filter.
- ✅ Multi-item MOVE uses a single confirmation where supported.
- ✅ No crash during the final verification flow.

---

# ⚠️ Security & Privacy Notes

VAIL is designed as a privacy-focused application, but no application should be considered completely unbreakable or immune to every attack.

Please keep in mind:

- Keep your VAIL PIN private.
- Keep your BIP-39 recovery phrase private and securely backed up.
- Protect the Google account used for encrypted backups.
- Download release APKs only from the official repository.
- Keep Android and device security updates current.
- A rooted or otherwise compromised device can weaken the security boundary of applications.
- Software deletion cannot guarantee literal physical zero-overwrite behavior on every storage device; VAIL's design relies on encryption and cryptographic key protection/destruction rather than claiming guaranteed NAND erasure.
- VAIL has not undergone an independent third-party security audit for the current public release.

---

# 🚫 What VAIL Does Not Claim

VAIL does **not** claim:

- Absolute or unbreakable security.
- Guaranteed forensic invisibility.
- Guaranteed physical NAND erasure.
- Complete protection against a compromised Android device.
- A third-party security audit that has not been performed.

The objective is a strong, practical, privacy-oriented encrypted vault with user-controlled local storage, backup, recovery, organization, and migration.

---

# 📋 Release History

| Version | Highlights |
|---|---|
| **1.0.2** | Scoped Album Media Viewer navigation, filtered viewer fixes, improved single-item navigation handling |
| **1.0.1** | Batch MOVE confirmation, Album All/Photos/Videos filters, safer MOVE verification, improved filtered selection |
| **1.0.0** | Initial production release of the VAIL encrypted media vault |

See the [**Releases**](../../releases) page for all official builds.

---

# 🧑‍💻 About This Repository

This repository is intended for **public distribution of official VAIL Android builds**.

The application source code is maintained separately from this release repository.

### Official repositories

- **Source:** [aryanKDev/VAIL](https://github.com/aryanKDev/VAIL)
- **Releases:** [aryanKDev/VAIL-Releases](https://github.com/aryanKDev/VAIL-Releases)

---

# ⭐ Support VAIL

If you use VAIL and find it useful, consider giving the repository a ⭐ on GitHub and sharing feedback through the project's official channels.

**VAIL — Keep your media private. Keep control in your hands. 🔐**
