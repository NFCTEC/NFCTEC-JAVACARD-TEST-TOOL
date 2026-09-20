# NFCTEC JavaCard Tool

Windows host utility for **Java Card** development, personalization, and production support.

It talks to cards over **PC/SC**, opens a **GlobalPlatform** secure channel (**SCP02** / **SCP03**), and gives you an APDU console, card registry, and CAP downloader in one workspace.

**Website:** [https://www.nfctec.com](https://www.nfctec.com)

**Current release:** 1.2.0 — [Download Windows installer](./NFCTEC_JavaCard_Tool_Setup_1.2.0.exe)

---

## What this tool is for

NFCTEC JavaCard Tool is a GlobalPlatform host for engineers who work with Java Card applets, Issuer Security Domains, and payment / identity card profiles.

Typical jobs:

- Send raw or secure APDUs and inspect every response in a live log
- Authenticate to the ISD with ENC / MAC / DEK keysets
- List packages, applets, applications, ISD and SSD
- Install, delete, select, lock / unlock, and change card lifecycle
- Parse a `.cap` file, load it to the card, and optionally install an instance
- Read CPLC / key info, put keys, store perso data, and probe free memory

It is built for lab and production support — not a toy APDU sender.

---

## Screenshots

### APDU Console

Connect a PC/SC reader, type a hex APDU, and send. After authentication, the same console wraps traffic with the active secure channel.

![APDU Console](1.png)

### Secure Channel

Pick an ISD AID, set KVN and security level, enter ENC / MAC / DEK, then authenticate. The tool auto-detects **SCP02** (3DES) or **SCP03** (AES-CMAC).

![Channel authentication](2.png)

### Card registry

List applets, modules, applications, ISD and SSD. Right-click for Select, Delete, Lock / Unlock, and Install.

![Card registry](3.png)

---

## Features

### Reader & session

- Desktop **PC/SC** (Windows; macOS / Linux in the same product line)
- Reader dropdown, Connect / Disconnect, Reset
- Connection state shown in the header (`OFFLINE` / connected / `AUTH SCP02` / `AUTH SCP03`)
- Global traffic log: copy, clear, collapse

### APDU Console

- Send hex APDUs (`CLA INS P1 P2 [Lc] [Data] [Le]`)
- Command history (last 20)
- Optional auto **GET RESPONSE** for `61xx` / `6Cxx`
- After **Authenticate**, commands are MAC’d / encrypted according to the selected security level:
  - `00` Plain
  - `01` Plain + MAC
  - `03` ENC + MAC

### Channel (secure channel)

- Built-in ISD AID presets:
  - GlobalPlatform / NXP JCOP — `A000000151000000`
  - Visa Card Manager
  - Mastercard Card Manager
  - Gemalto / G&D
  - Oberthur / Idemia
  - China Mobile USIM ISD
  - Empty SELECT (discover ISD)
- Custom AID field
- Key version (KVN)
- ENC / MAC / DEK (AES-128 hex; SCP02 uses 3DES from the same key material)
- One-click **Fill default GP keys** (`40…4F`)
- Save keyset locally
- Payment AID chips (PPSE, Visa, Mastercard, UnionPay, JCB, Discover, Amex) for SELECT tests

### Card management

- **List** — GET STATUS registry: packages, applets, applications, ISD, SSD
- **Select** / **Delete** / **Delete + related** (package with related objects)
- **Install + Make Selectable** with privilege presets (Card Reset, SD, DAP, DM, …)
- **Lock / Unlock**
- Card lifecycle: OP_READY, INITIALIZED, SECURED, CARD_LOCKED, TERMINATED
- **Put Key** (3DES / AES)
- **Store Data** / perso
- **Card Info** — CPLC (`9F7F`) and key information
- **Memory** — persistent / transient RAM probe

### CAP download

- Drop or browse a Java Card `.cap`
- Parse package AID and module AID before load
- Reject invalid files (needs `javacard/Header.cap` inside the CAP zip)
- Sequence: DELETE (optional existing) → INSTALL [for load] → LOAD in blocks
- Optional **Install + make selectable after load**
- Configurable load block size and cancel

### Settings & About

- Auto GET RESPONSE on/off
- Dark / light theme (NFCTEC teal)
- About: version, feature list, company site

---

## Install (Windows)

1. Download [`NFCTEC_JavaCard_Tool_Setup_1.2.0.exe`](./NFCTEC_JavaCard_Tool_Setup_1.2.0.exe)
2. Run the installer (administrator)
3. Default location: `Program Files\NFCTEC JavaCard Tool`
4. Start Menu shortcut is created; desktop shortcut is optional
5. Uninstall from Windows **Apps & Features**

**Requirements**

- Windows 10 or later (64-bit)
- A PC/SC CCID reader (contact or contactless)
- Java Card / GlobalPlatform card with known ISD keys

---

## Quick start

1. Plug in the reader and insert / tap the card.
2. Choose the reader in the header and click **Connect**.
3. Open **Channel**, pick the ISD (or keep GlobalPlatform / NXP JCOP), fill keys, click **Authenticate**.
4. Use **Card → List** to see what is on the card.
5. Use **Console** for ad-hoc APDUs, or **CAP** to load an applet.

### Default test keys (lab only)

| Field | Default |
| --- | --- |
| ISD AID | `A000000151000000` |
| ENC / MAC / DEK | `404142434445464748494A4B4C4D4E4F` |

These are the well-known GlobalPlatform sample keys. Production cards must use your own keyset. Change them on **Channel** and tap **Save**.

---

## About NFCTEC

[NFCTEC](https://www.nfctec.com) builds full-stack **NFC and smart card** solutions: software, hardware, and cloud — from prototype to certified production.

We work across payment, transit, identity, access, IoT, and wallets, with stacks that include **Java Card**, **GlobalPlatform**, EMV, ICAO 9303, MIFARE, DESFire, FeliCa, and NFC Forum tags.

| | |
| --- | --- |
| Website | [https://www.nfctec.com](https://www.nfctec.com) |
| Software | Mobile wallet SDKs, issuance & reading for ePassports, EMV, Java Card, NTAG 424 DNA, DESFire, MIFARE, FeliCa |
| Hardware | USB / Serial / USB CCID readers, modules, antennas, blank cards |
| Cloud | Issuance & verification APIs, HSM-backed keys, SaaS consoles |
| Location | Room 4-412, Minke Technology Park, Longgang District, Shenzhen, China |

More product tools on the site: EMV Parser, APDU Debugger, NDEF Editor, MIFARE Toolkit.

---

## Version

**NFCTEC JavaCard Tool 1.2.0**

© NFCTEC · [www.nfctec.com](https://www.nfctec.com)
