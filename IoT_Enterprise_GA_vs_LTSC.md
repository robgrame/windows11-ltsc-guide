# Windows 11 IoT Enterprise GA (General Availability Channel) — Guida comparativa

## Cos'è Windows 11 IoT Enterprise GA

Windows 11 IoT Enterprise esiste in **due varianti** con modelli di servicing completamente diversi:

| Aspetto | IoT Enterprise **GA** (GAC) | IoT Enterprise **LTSC** |
|---------|:---------------------------:|:-----------------------:|
| Feature update | ✅ Annuali (come Win11 Enterprise) | ❌ Nessuno |
| Supporto per versione | 36 mesi | 10 anni |
| Microsoft Store | Non incluso di default | Non incluso |
| Microsoft Edge | ✅ Preinstallato | ❌ Non preinstallato |
| Feature embedded (UWF, Shell Launcher) | ✅ | ✅ |
| Lifecycle Policy | Modern Lifecycle | Fixed Lifecycle |
| Target | Dispositivi IoT che beneficiano di aggiornamenti | Dispositivi mission-critical, stabilità assoluta |

> **Punto chiave:** IoT Enterprise GA è essenzialmente Windows 11 Enterprise + feature embedded IoT (UWF, Shell Launcher, Keyboard Filter) con feature update regolari.

---

## Differenze chiave rispetto a LTSC

### Vantaggi di IoT Enterprise GA vs LTSC

1. **Supporto software più ampio** — Teams, M365 Apps, VS non sono bloccati
2. **Feature update** — nuove funzionalità OS ogni anno
3. **Edge preinstallato** — nessuna necessità di deploy manuale
4. **Modern Lifecycle** — compatibilità con prodotti che richiedono OS aggiornato

### Svantaggi di IoT Enterprise GA vs LTSC

1. **Supporto solo 36 mesi** per versione → richiede upgrade periodico
2. **Meno stabile** per scenari ultra-long-life (non adatto a dispositivi 10+ anni)
3. **Feature update obbligatori** per restare in supporto
4. **Potenziali breaking change** ad ogni aggiornamento annuale

---

## Matrice di supporto software — IoT Enterprise GA vs LTSC

| Software | IoT Enterprise GA | IoT Enterprise LTSC | Note |
|----------|:-----------------:|:-------------------:|------|
| Office LTSC 2024 | ✅ | ✅ | Supportato su entrambe |
| Microsoft 365 Apps | ✅ | ❌ | GA segue Modern Lifecycle → supportato |
| .NET 8 (LTS) | ✅ | ✅ | Supportato su entrambe |
| .NET 9 (STS) | ✅ | ✅ | Supportato su entrambe |
| Power BI Desktop | ✅ | ✅ * | * LTSC: supporto non dichiarato esplicitamente |
| Visual Studio 2022 / 2026 | ❌ | ❌ | "Windows Enterprise IoT" escluso (GA e LTSC) |
| SSMS 22.x | ❌ | ❌ | "Windows Enterprise LTSC editions" + IoT esclusi |
| SQL Server 2022 | ✅ | ✅ | Supportato su entrambe |
| Microsoft Teams (new) | ✅ | ❌ | GA supportato; LTSC bloccato dal 15/08/2025 |
| Microsoft Edge | ✅ (preinstallato) | ❌ (non preinstallato) | Installabile manualmente su LTSC |
| Windows Terminal | ✅ | ✅ | Installabile via WinGet |
| PowerShell 7.x | ✅ | ✅ | Supportato su entrambe |
| Azure CLI | ✅ | ✅ | Supportato su entrambe |

### Punti critici

- **Visual Studio e SSMS** → Non supportati su **nessuna** edizione IoT Enterprise (né GA né LTSC). La doc Microsoft esclude esplicitamente "Windows Enterprise IoT".
- **Teams** → Supportato **solo su GA**, bloccato su LTSC
- **Microsoft 365 Apps** → Supportato **solo su GA**, non supportato su LTSC

---

## Attivazione KMS — IoT Enterprise GA

L'attivazione KMS per IoT Enterprise GA usa la stessa GVLK della versione LTSC:

| Edizione | GVLK |
|----------|------|
| Windows 11 IoT Enterprise LTSC 2024 | `KBN8V-HFGQ4-MGXVD-347P6-PDQGT` |
| Windows 11 IoT Enterprise (GA) | Stessa GVLK della versione Enterprise corrispondente |

> **Nota:** Verificare sul portale VLSC la chiave specifica per la versione GA in uso (24H2, 25H2, ecc.)

---

## Microsoft Store e distribuzione app

| Aspetto | IoT Enterprise GA | IoT Enterprise LTSC |
|---------|:-----------------:|:-------------------:|
| Store preinstallato | ❌ Non di default | ❌ Non di default |
| Edge preinstallato | ✅ | ❌ |
| WinGet disponibile | ✅ (con App Installer) | Installazione manuale necessaria |
| Intune "Store app (new)" | ✅ | ✅ (richiede WinGet) |
| App consumer preinstallate | ❌ Minimal | ❌ Minimal |

---

## Personalizzazione immagine

Le capacità di personalizzazione sono **identiche** tra GA e LTSC:

- ✅ Integrazione driver (DISM)
- ✅ Cumulative Update
- ✅ Language Pack
- ✅ Features on Demand (sottoinsieme IoT)
- ✅ Unified Write Filter (UWF)
- ✅ Shell Launcher V2
- ✅ Keyboard Filter / Gesture Filter
- ✅ Custom Logon
- ✅ Assigned Access (Kiosk)
- ❌ Microsoft Store (non aggiungibile via DISM)

**Differenza pratica:** Su GA, le feature update successive possono modificare componenti dell'immagine. Su LTSC, l'immagine rimane stabile per l'intero ciclo di vita.

---

## Ciclo di vita comparato

```
2024        2025        2026        2027        2028        2029        2030        2034
  |-----------|-----------|-----------|-----------|-----------|-----------|-----------|
  [=== IoT Enterprise GA 24H2 (36 mesi) ===]
              [=== IoT Enterprise GA 25H2 (36 mesi) ===]
  [====== Enterprise LTSC 2024 (5 anni) ======]
  [======================== IoT Enterprise LTSC 2024 (10 anni) =====================]
```

---

## Quando scegliere GA vs LTSC

| Scenario | Edizione consigliata |
|----------|---------------------|
| Dispositivo con vita utile > 5 anni | IoT Enterprise **LTSC** |
| Necessità di Teams / M365 Apps | IoT Enterprise **GA** |
| Ambiente regolamentato, stabilità assoluta | IoT Enterprise **LTSC** |
| Chiosco/POS con aggiornamenti gestiti | IoT Enterprise **GA** o LTSC |
| Dispositivo medicale / SCADA | IoT Enterprise **LTSC** |
| Thin client con produttività utente | IoT Enterprise **GA** |
| Dispositivo disconnesso / air-gapped | IoT Enterprise **LTSC** |

---

## Riferimenti documentazione Microsoft

- [Windows IoT Enterprise Overview](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/overview)
- [Windows IoT Enterprise FAQ](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/faq)
- [Windows 11 IoT Enterprise Lifecycle](https://learn.microsoft.com/en-us/lifecycle/products/windows-11-iot-enterprise)
- [Windows 11 IoT Enterprise LTSC 2024 - What's New](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/whats-new/windows-11-iot-enterprise-ltsc-2024)
- [Windows IoT Support Lifecycle and Upcoming Releases](https://techcommunity.microsoft.com/blog/iotblog/windows-iot-support-lifecycle-and-upcoming-releases/2511888)
- [Visual Studio 2022 System Requirements (unsupported OS list)](https://learn.microsoft.com/en-us/visualstudio/releases/2022/system-requirements)
- [SSMS System Requirements](https://learn.microsoft.com/en-us/ssms/system-requirements)
- [Teams OS Support Policy](https://learn.microsoft.com/en-us/microsoftteams/get-clients)
- [Windows IoT Enterprise Licensing Guide (PDF)](https://www.arrow.com/ais/msembedded/wp-content/uploads/sites/3/2022/11/New-Windows_11_IoT_Enterprise_Licensing-Guide_Oct-20.pdf)
