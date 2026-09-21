# NFCTEC JavaCard Tool: A GlobalPlatform Host for CAP Load, Secure Channel, and Card Management

A Windows GlobalPlatform host for Java Card work: SCP02/SCP03, CPLC readout, drag-and-drop CAP download, and an APDU console in one workspace.

[Download Windows installer 1.2.0](./NFCTEC_JavaCard_Tool_Setup_1.2.0.exe) · [www.nfctec.com](https://www.nfctec.com) · [Blog article](https://www.nfctec.com/en/blog/nfctec-javacard-tool)

## NFCTEC JavaCard Tool

Java Card work usually means several tools at once: a GlobalPlatform shell for the secure channel, another utility for CAP load, and a console for raw APDUs. NFCTEC JavaCard Tool puts those steps in one desktop host so you can authenticate, inspect the card, load a package, and debug APDUs without switching windows.

Version 1.2.0 is built for PC/SC readers on Windows. It targets development, sample bring-up, and small-batch personalization—not a replacement for a factory CMS, but a practical bench tool.

### What it is

The application is a GlobalPlatform host. It selects the Issuer Security Domain, runs INITIALIZE UPDATE / EXTERNAL AUTHENTICATE, then wraps later commands according to the security level you chose.

Typical flow:

1. Connect a PC/SC reader and select the ISD.
2. Authenticate with ENC / MAC / DEK (SCP02 or SCP03, detected from the card).
3. List applets, applications, the ISD, and SSDs.
4. Drop a `.cap` file, check the format, then download (optional install after load).
5. Read CPLC, probe free memory, put keys, or send APDUs from the console.

### Secure channel: pick what the card accepts

![Channel — ISD, keys, and SCP02 authentication](2.png)

EXTERNAL AUTHENTICATE P1 must match what the card allows. This build exposes the three levels that real GP cards commonly accept:

1. **00 Plain** — no secure messaging. Commands go as `80 …` with a clear data field. Useful when you need to see LOAD/INSTALL on the wire.
2. **01 Plain + MAC** — C-MAC only. Data stays in the clear; the card checks the MAC.
3. **03 ENC + MAC** — C-ENC and C-MAC. After authentication, LOAD / INSTALL / DELETE are sent as `84` with encrypted data and a MAC. This is the setting to use if you do not want a clear CAP on the bus.

SCP02 wrapping follows the usual i=15 pattern (C-MAC on the modified plaintext APDU, then encrypt). SCP03 encrypts first with the encryption counter, then computes AES-CMAC on the ciphertext—aligned with GlobalPlatform Amendment D and common host tools.

Authenticate after you change the security level. A wrong P1 typically returns `6A86`.

ISD presets include GlobalPlatform / NXP JCOP (`A000000151000000`), Visa, Mastercard, Gemalto, Oberthur / Idemia, and China Mobile USIM. You can also type a custom AID or run an empty SELECT to discover the ISD.

### CAP download

CAP files can be browsed or dragged onto the CAP page. The tool checks that the file is a Java Card CAP (ZIP with `Header.cap` / `.cap` Header component) before anything is sent to the card. Invalid files are rejected with an error; they never become LOAD blocks.

Download path:

1. optional DELETE of the same package
2. INSTALL [for load]
3. LOAD in blocks
4. optional INSTALL [for install] if you enable install-after-load

Block size is limited so C-MAC / C-ENC still fit in a short APDU. Progress shows as `LOAD n/N` in the status area; the log stays compact so a large CAP does not freeze the UI.

If you authenticated with `03`, the payload on the reader is ciphertext plus MAC—not a raw `80 E8` CAP image.

### CPLC and the card list

![Card — registry list after GET STATUS](3.png)

Card Info reads GET DATA `9F7F` and parses the 42-byte CPLC: IC fabricator, IC type, OS id, dates (YDDD), serial, batch, and a CUID. Known vendor and OS codes (for example NXP / JCOP) are shown next to the hex.

The Card page lists registry objects from GET STATUS (ISD, applications, load files and modules). After a successful CAP load, the list is refreshed. An empty application list returning `6A88` is normal if you loaded a package but did not install an instance.

Right-click an entry to copy the AID, Select, Delete (optionally with related objects), Lock / Unlock, or Install.

### APDU console and extras

![APDU Console — send hex commands with a live log](1.png)

The console sends hex APDUs and handles GET RESPONSE (`61xx` / `6Cxx`). After a successful authenticate, wrapping follows the current security level. Command history keeps the last 20 APDUs.

Also included:

1. Put Key and Store Data
2. Card lifecycle (INITIALIZED / SECURED / LOCKED)
3. Lock / unlock of selected applications
4. Free-memory probe (log output)
5. Light and dark themes

Default test keys (`4041…4E4F`) are for lab cards only. Failed authentications are counted: too many tries can lock the ISD.

### Who it is for

1. Engineers bringing up JCOP, ACOS, or other GP Java Cards on the bench
2. Teams that need a visible SCP02/SCP03 session plus CAP load in one UI
3. Anyone who wants CPLC decoded instead of a raw `9F7F` dump

Website: [www.nfctec.com](https://www.nfctec.com)

NFCTEC JavaCard Tool 1.2.0 is available as a Windows installer ([`NFCTEC_JavaCard_Tool_Setup_1.2.0.exe`](./NFCTEC_JavaCard_Tool_Setup_1.2.0.exe)). Install, attach a reader, authenticate, then load a CAP or open Card Info.
