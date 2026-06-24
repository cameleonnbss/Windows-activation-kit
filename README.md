<div align="center">

# Windows Activation Kit

**A streamlined Windows activation toolkit built on the proven MAS core.**

<p>
  <img src="https://img.shields.io/badge/version-3.11-blue" alt="Version" />
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License" />
  <img src="https://img.shields.io/badge/platform-Windows%2010%20%7C%2011%20%7C%20Server-lightgrey" alt="Platform" />
  <img src="https://img.shields.io/badge/language-Batchfile-4EAA25" alt="Language" />
</p>

<i>For educational and research purposes only. Use at your own risk.</i>

</div>

---

## Overview

**Windows Activation Kit** is a single-file, open-source command-line tool that simplifies Microsoft Windows activation. It packages the battle-tested core components of [Microsoft Activation Scripts (MAS)](https://massgrave.dev) by **massgravel** into a clean, branded interface with a no-nonsense interactive menu.

No installations. No dependencies. No product keys required. Just run the script with administrator privileges and follow the on-screen prompts.

---

## Features

| Feature | Description |
|---|---|
| **HWID Digital License** | Permanently activates Windows 10/11 by generating a genuine digital license tied to your hardware ID. Survives reinstalls on the same machine. |
| **Ohook Activation** | Activates Microsoft Office via the Ohook method — a reliable, registry-based approach for Office 2016–2024. |
| **KMS38 Activation** | Activates Windows/Server until the year 2038 using a KMS emulator with an extended timer. |
| **Online KMS Activation** | Standard KMS activation with 180-day renewable cycles. Requires periodic re-activation. |
| **TSforge Activation** | Alternative activation engine for edge-case scenarios. |
| **Update Checker** | Automatically pings upstream to detect new MAS releases on launch. |
| **Unattended Mode** | Full CLI switch support for silent, scripted deployment. |
| **Self-Healing** | Auto-relaunches under the correct architecture (x64/ARM64) and elevates to admin when needed. |

---

## Supported Operating Systems

| OS | Supported Versions |
|---|---|
| **Windows 10** | All supported consumer & enterprise editions (build 10240+) |
| **Windows 11** | All supported consumer & enterprise editions (build 22000+) |
| **Windows Server** | 2008 R2 / 2012 / 2012 R2 / 2016 / 2019 / 2022 / 2025 |
| **Legacy** | Windows 7 SP1 / 8 / 8.1 (limited method support) |

> **Note:** Windows Vista RTM is not supported. Upgrade to SP1 or later.

---

## Quick Start

### Prerequisites

- Windows Vista SP1 or later
- **Administrator privileges** (the script will auto-request elevation)
- Windows PowerShell (pre-installed on all supported versions)
- Internet connection (for KMS and update-check features)

### Interactive Mode

1. **Download** `Windows_Activator.bat` 
2. **Right-click** the file and select **"Run as administrator"**.
3. Choose an option from the menu using your keyboard.

```
[1] Activate Windows   - HWID Digital License
[0] Exit
```

OR

1. Clone the repository.

**CMD**
```cmd
git clone https://github.com/cameleonnbss/Windows-activation-kit.git
cd Windows-activation-kit
````

**PowerShell**

```powershell
git clone https://github.com/cameleonnbss/Windows-activation-kit.git
Set-Location Windows-activation-kit
```

2. Launch the activator with Administrator privileges.

**CMD**

```cmd
Windows_Activator.bat
```

**PowerShell**

```powershell
Start-Process .\Windows_Activator.bat -Verb RunAs
```

3. Select an option from the menu.

```text
[1] Activate Windows   - HWID Digital License
[0] Exit
```




### Command-Line Mode (Unattended / Silent)

All activation methods can be invoked directly via CLI switches — ideal for automation, scripts, and deployment pipelines.

```cmd
:: HWID Digital License activation
Windows_Activator.bat /HWID

:: Ohook Office activation
Windows_Activator.bat /Ohook

:: KMS38 activation (valid until 2038)
Windows_Activator.bat /KMS38

:: Online KMS activation (180-day cycle)
Windows_Activator.bat /KMS

:: TSforge activation
Windows_Activator.bat /TSforge

:: Silent mode — suppresses all output
Windows_Activator.bat /HWID /S
```

> Refer to the official MAS [command-line switches reference](https://massgrave.dev/command_line_switches) for the full list of advanced options.

---

## Activation Methods Explained

### HWID (Digital License)

The recommended method for Windows 10 and 11. It creates a **genuine Microsoft digital license** that is permanently bound to your motherboard's hardware ID. Once activated, Windows will automatically re-activate itself after clean reinstalls on the same hardware — no internet or script required.

### Ohook

A dedicated Office activation method that injects a benign registry hook, causing Office to bypass the volume licensing check. Works with Office 2016, 2019, 2021, and 2024 on supported Windows builds.

### KMS38

Uses a local KMS emulator with the timer set to January 19, 2038. Ideal for systems where you want long-term activation without periodic renewal. Not suitable for Windows versions below 10 (build 14393).

### Online KMS

Connects to public KMS servers to activate Windows and/or Office with a standard 180-day license. Requires internet access and must be renewed before expiry. The script can optionally set up a renewal task.

### TSforge

An alternative activation engine included for compatibility with edge-case Windows/Office configurations where other methods may not apply.

---

## Project Structure

```
Windows-activation-kit/
├── README.md               # This file
├── LICENSE                 # MIT License
└── Windows_Activator.bat  # Main script (everything in one file)
```

The entire tool is a **single self-contained batch file** — no additional scripts, modules, or external dependencies are required.

---

## How It Works

1. **Architecture Detection** — The script detects whether it is running under x86, x64, or ARM64 and automatically re-launches itself under the correct native process (`Sysnative` / `SysArm32`).
2. **Admin Elevation** — If not already running as administrator, the script invokes a UAC elevation prompt via PowerShell.
3. **Environment Validation** — Checks for a working Null service, correct PowerShell `FullLanguage` mode, .NET runtime, and absence of known conflicting software.
4. **Update Check** — Pings upstream servers to determine if a newer version of MAS is available.
5. **Menu or CLI Dispatch** — Presents the interactive menu or, if CLI switches are provided, directly executes the requested activation method.
6. **Activation** — The selected method runs its procedure (key injection, ticket generation, registry hook, etc.) and reports the result.

---

## Troubleshooting

| Issue | Solution |
|---|---|
| **"Null service is not running"** | The Windows `Null` system service must be running. See the [MAS fix guide](https://massgrave.dev/fix_service). |
| **"PowerShell is not working properly"** | Your antivirus may be blocking script execution. Temporarily disable real-time protection or add an exclusion. |
| **"Windows Sandbox detected"** | Activation is not possible inside Windows Sandbox due to missing licensing components. |
| **"Unsupported OS version"** | Ensure you are running Windows Vista SP1 or later with PowerShell 2.0+ installed. |
| **LF line ending error** | The script requires CRLF line endings. Re-download the file or convert line endings. |
| **False positive by antivirus** | Batch scripts that interact with licensing services are frequently flagged. This is a known issue addressed in the script header comments. |

For additional help, visit the [MAS troubleshooting page](https://massgrave.dev/troubleshoot).

---

## Credits & Acknowledgements

This project is built on the **[Microsoft Activation Scripts (MAS)](https://massgrave.dev)** by [massgravel](https://github.com/massgravel). The core activation logic, error handling, and architecture management originate from MAS v3.11.

- **MAS Repository:** [github.com/massgravel/Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)
- **MAS Homepage:** [massgrave.dev](https://massgrave.dev)

---

## Disclaimer

> **This tool is provided for educational and research purposes only.** The author(s) assume no liability for misuse, damage, or legal consequences arising from the use of this software. Activation of software without a valid license may violate the terms of your software agreement and applicable laws in your jurisdiction. You are solely responsible for ensuring compliance with all relevant laws and regulations.

---

## License

This project is released under the **[MIT License](LICENSE)**.

---

<div align="center">

**Made with ❤️ by [CAMZZZ](https://github.com/cameleonnbss)**

</div>
