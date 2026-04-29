# Supportabilità Software Microsoft su Windows 11 LTSC e LTSC IoT 2024

## Panoramica

Windows 11 Enterprise LTSC 2024 e Windows 11 IoT Enterprise LTSC 2024 sono progettati per scenari di stabilità a lungo termine. Questo documento analizza la compatibilità e il supporto ufficiale dei principali software Microsoft su queste edizioni.

> **Principio generale:** I software Microsoft che seguono il Modern Lifecycle Policy (aggiornamenti continui) potrebbero avere supporto limitato o assente su LTSC. I prodotti con rilascio "perpetuo" o LTSC-aligned sono generalmente pienamente supportati.

---

## Matrice di supporto

| Software | Supportato su LTSC 2024 | Supportato su IoT LTSC 2024 | Fine supporto | Note |
|----------|:-----------------------:|:---------------------------:|:-------------:|------|
| Office LTSC 2024 | ✅ | ✅ | Ottobre 2029 | Combinazione raccomandata |
| Microsoft 365 Apps | ❌ **Non supportato** | ❌ **Non supportato** | N/A | Non supportato su edizioni LTSC |
| .NET 8 (LTS) | ✅ | ✅ | Novembre 2026 | x64, x86, Arm64 |
| .NET 9 (STS) | ✅ | ✅ | Maggio 2026 | x64, x86, Arm64 |
| .NET Framework 4.8.1 | ✅ | ✅ | Incluso nel OS | Preinstallato |
| Power BI Desktop | ✅ * | ✅ * | Continuo | Richiede .NET 4.7.2+ e WebView2 (*supporto inferito, non dichiarato esplicitamente per LTSC) |
| Visual Studio 2022 / 2026 | ❌ | ❌ | N/A | **Non supportato su LTSC** (policy esplicita Microsoft) |
| SSMS 22.x | ❌ | ❌ | N/A | **Non supportato su edizioni LTSC** (esplicito nella doc) |
| SQL Server 2022 | ✅ | ✅ | Gennaio 2028 (mainstream) | Pienamente supportato |
| Microsoft Teams (new) | ❌ | ❌ | Bloccato dal 15/08/2025 | Non supportato su LTSC |
| Microsoft Edge (Chromium) | ⚠️ | ⚠️ | Continuo | Non preinstallato; installabile manualmente, supporto non esplicito per LTSC |
| Windows Terminal | ✅ | ✅ | Continuo | Installabile via WinGet o MSIX |
| PowerShell 7.x | ✅ | ✅ | Varia per versione | Installazione separata |
| Azure CLI | ✅ | ✅ | Continuo | MSI installer |

---

## Dettaglio per prodotto

### Office LTSC 2024

**Stato:** ✅ Pienamente supportato — combinazione raccomandata da Microsoft.

- Rilasciato specificamente per ambienti LTSC e disconnessi
- Solo aggiornamenti di sicurezza, nessun feature update
- Include: Word, Excel, PowerPoint, Outlook, Access, Publisher, OneNote
- Licenza perpetua, non richiede connessione cloud
- Supporto mainstream fino a **ottobre 2029**

```
Requisiti minimi:
- Processore: 1.1 GHz dual-core
- RAM: 4 GB
- Disco: 4 GB disponibili
- Display: 1280 x 768
```

### Microsoft 365 Apps (ex Office 365 ProPlus)

**Stato:** ❌ **NON supportato su Windows 11 LTSC 2024.**

Microsoft dichiara esplicitamente nella documentazione ufficiale:

> *"Office LTSC 2024 is supported on Windows 11 LTSC 2024, but Microsoft 365 Apps are not."*

| Aspetto | Dettaglio |
|---------|-----------|
| Installazione | Tecnicamente possibile, ma **non supportata** |
| Supporto Microsoft | Nessuno — in caso di problemi non verrà fornita assistenza |
| Motivo | M365 Apps richiede un OS con Modern Lifecycle (feature update regolari) |
| Alternativa supportata | **Office LTSC 2024** |

**Raccomandazione:** Usare esclusivamente **Office LTSC 2024** su ambienti LTSC. Se servono le funzionalità cloud di Microsoft 365 Apps, è necessario utilizzare Windows 11 con canale GA (es. 24H2).

### .NET Runtime (.NET 8, .NET 9, .NET Framework)

**Stato:** ✅ Pienamente supportato.

| Versione | Tipo | Fine supporto | Architetture |
|----------|------|:-------------:|:------------:|
| .NET Framework 4.8.1 | Preinstallato | Lifecycle OS | x64 |
| .NET 8 | LTS | Novembre 2026 | x64, x86, Arm64 |
| .NET 9 | STS | Maggio 2026 | x64, x86, Arm64 |
| .NET 10 (previsto) | LTS | TBD | x64, x86, Arm64 |

> **Nota:** .NET 8+ deve essere installato separatamente (non incluso nel OS). Usare il runtime installer o il SDK da [dot.net](https://dot.net/download).

### Power BI Desktop

**Stato:** ✅ * Supportato (con nota).

> **Nota:** I requisiti di sistema indicano "Windows 11 64-bit" senza menzionare esplicitamente le edizioni LTSC. Il supporto è inferito dalla compatibilità con Windows 11. Power BI Desktop è pienamente funzionante su LTSC 2024.

```
Requisiti:
- OS: Windows 11 64-bit (LTSC 2024 non menzionato esplicitamente)
- RAM: 8 GB minimo (16 GB raccomandati)
- Disco: 2 GB minimo (SSD con 10 GB raccomandato)
- Display: 1440 x 900 minimo
- .NET Framework 4.7.2+
- WebView2 Runtime (installato automaticamente)
```

**Nota importante:** Power BI Desktop è normalmente distribuito tramite Microsoft Store. Su LTSC, installare tramite:
- Download diretto MSI da [powerbi.microsoft.com](https://powerbi.microsoft.com/desktop/)
- WinGet: `winget install Microsoft.PowerBIDesktop`
- Distribuzione Intune come Win32 app

### Visual Studio 2022 / 2026

**Stato:** ❌ **NON supportato su edizioni LTSC.**

La documentazione ufficiale Microsoft dichiara esplicitamente:

> *"Visual Studio isn't intended to run on the Windows Long-term Servicing Channel (LTSC). However, building apps that run on Windows LTSC is supported."*

Questo vale per **tutte le versioni** di Visual Studio (2022, 2026 e successive).

| Aspetto | Dettaglio |
|---------|-----------|
| Installazione su LTSC | ❌ Non supportata (nessuna edizione LTSC) |
| Installazione su IoT Enterprise | ❌ Esplicitamente escluso |
| Build di app destinate a LTSC | ✅ Supportato (da macchina non-LTSC) |
| Motivo | VS richiede un OS con Modern Lifecycle e feature update |
| Alternativa | Usare Windows 11 GA (Pro/Enterprise) o Windows Server |

**Fonte:** [Visual Studio unsupported operating systems](https://learn.microsoft.com/en-us/troubleshoot/developer/visualstudio/installation/visual-studio-unsupported-operating-systems)

### SQL Server Management Studio (SSMS)

**Stato:** ❌ **NON supportato su edizioni LTSC.**

La pagina ufficiale dei [requisiti di sistema di SSMS 22](https://learn.microsoft.com/en-us/ssms/system-requirements) elenca esplicitamente tra i sistemi operativi **non supportati**:

> *"Windows Enterprise LTSC editions"*

| Aspetto | Dettaglio |
|---------|-----------|
| Installazione | Tecnicamente possibile, ma **non supportata** |
| Supporto Microsoft | Nessuno — in caso di problemi non verrà fornita assistenza |
| Motivo | SSMS 22 è basato sulla shell Visual Studio 2022, che esclude le edizioni LTSC |
| Alternativa | Usare SSMS da una macchina con Windows 11 GA, oppure Azure Data Studio |

> **Nota:** Anche Windows Enterprise IoT e Server IoT sono esplicitamente esclusi dal supporto SSMS 22.

### Microsoft Teams (New Client)

**Stato:** ❌ NON supportato su LTSC.

| Data | Evento |
|------|--------|
| Ottobre 2024 | Notifiche in-app che invitano all'upgrade OS |
| 15 agosto 2025 | **Blocco completo** — Teams mostra schermata di errore |

**Motivo:** Teams segue il Modern Lifecycle Policy e richiede un OS con feature update regolari.

**Alternative per ambienti LTSC:**
1. **Teams Web App** — tramite browser (Edge Chromium o Chrome)
2. **Azure Virtual Desktop** — con Windows 11 GA come OS sessione
3. **Workaround non supportati** — sideload MSIX (non raccomandato, può rompersi ad ogni aggiornamento)

### Microsoft Edge (Chromium)

**Stato:** ⚠️ Non preinstallato, installabile manualmente.

- Edge **non è incluso** nell'immagine Windows 11 LTSC 2024 di default
- Può essere scaricato e installato manualmente (MSI o WinGet)
- Una volta installato, riceve aggiornamenti automatici
- Non esiste una dichiarazione esplicita di supporto Microsoft per Edge su LTSC
- Distribuibile tramite Intune come Win32 app o Group Policy

```cmd
:: Installazione tramite WinGet (se disponibile)
winget install Microsoft.Edge

:: Alternativa: download MSI da
:: https://www.microsoft.com/edge/business/download
```

---

## Riepilogo decisionale

| Scenario | Software consigliato |
|----------|---------------------|
| Produttività office disconnessa | Office LTSC 2024 |
| Produttività office con cloud | Office LTSC 2024 (connettività M365 supportata fino a Ott 2029) |
| Sviluppo software | Visual Studio da macchina non-LTSC (Win11 GA o Windows Server) |
| Analisi dati | Power BI Desktop (MSI) |
| Gestione database | Azure Data Studio o SSMS da macchina non-LTSC |
| Comunicazione e collaborazione | Teams Web App (NO client desktop) |
| Browser | Edge Chromium (installazione manuale) |
| Automazione | PowerShell 7.x + Azure CLI |

---

## Ciclo di vita comparato

```
2024        2025        2026        2027        2028        2029        2030        2034
  |-----------|-----------|-----------|-----------|-----------|-----------|-----------|
  [====== Windows 11 Enterprise LTSC 2024 (5 anni) ======]
  [====== Office LTSC 2024 (5 anni) =====================]
  [======================== Windows 11 IoT Enterprise LTSC 2024 (10 anni) ==========]
  [== .NET 8 ==]
  [== .NET 9 ==]
  [Teams ❌]
```

---

## Nota sul software di terze parti

Le informazioni contenute in questo documento riguardano esclusivamente software Microsoft. Per qualsiasi software di terze parti (es. antivirus, strumenti di gestione, applicazioni LOB, middleware, agent di monitoraggio, ecc.) **è necessario verificare direttamente con il rispettivo vendor** la compatibilità e il supporto ufficiale su Windows 11 LTSC e IoT Enterprise LTSC 2024.

### Punti di attenzione

- **Non assumere compatibilità** basandosi sul supporto generico a "Windows 11": le edizioni LTSC hanno componenti mancanti (Store, AppX framework ridotto, assenza di feature update) che possono causare incompatibilità
- **Richiedere al vendor una dichiarazione scritta** di supporto per l'edizione specifica (Enterprise LTSC 2024 o IoT Enterprise LTSC 2024)
- **Verificare i prerequisiti runtime**: alcune applicazioni dipendono da componenti non presenti su LTSC (es. WebView2, Edge, .NET specifici, Visual C++ Redistributable)
- **Testare in ambiente pilota** prima del deployment in produzione
- **Documentare** la posizione ufficiale del vendor per ogni software critico, ai fini di audit e compliance

> **Principio generale:** Se il vendor non dichiara esplicitamente il supporto per le edizioni LTSC, considerare il software come **non supportato** su LTSC e pianificare di conseguenza.

---

## Riferimenti documentazione Microsoft

### Office e Microsoft 365
- [Overview of Office LTSC 2024](https://learn.microsoft.com/en-us/office/ltsc/2024/overview)
- [Office and Windows Configuration Support Matrix](https://learn.microsoft.com/en-us/lifecycle/faq/office-windows-configurations)
- [Office versions and connectivity to M365 services](https://learn.microsoft.com/en-us/microsoft-365-apps/end-of-support/microsoft-365-services-connectivity)
- [Office LTSC 2024 System Requirements](https://learn.microsoft.com/en-us/office/ltsc/2024/overview#system-requirements)

### .NET
- [.NET 8 Supported OS](https://github.com/dotnet/core/blob/main/release-notes/8.0/supported-os.md)
- [.NET 9 Supported OS](https://github.com/dotnet/core/blob/main/release-notes/9.0/supported-os.md)
- [.NET Downloads](https://dotnet.microsoft.com/download)

### Power BI
- [Power BI Desktop Download](https://powerbi.microsoft.com/desktop/)
- [Power BI Desktop System Requirements](https://learn.microsoft.com/en-us/power-bi/fundamentals/desktop-get-the-desktop)

### Visual Studio e Developer Tools
- [Visual Studio 2022 Compatibility](https://learn.microsoft.com/en-us/visualstudio/releases/2022/compatibility)
- [SSMS System Requirements](https://learn.microsoft.com/en-us/ssms/system-requirements)
- [PowerShell Installation on Windows](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows)

### Teams
- [Teams OS Requirements and Lifecycle](https://learn.microsoft.com/en-us/microsoftteams/get-clients)
- [Stay current with your OS for Teams support (MC898394)](https://learn.microsoft.com/en-us/microsoftteams/troubleshoot/teams-on-windows/teams-windows-ltsc-support)

### Windows LTSC
- [Windows 11 LTSC - What's New](https://learn.microsoft.com/en-us/windows/whats-new/ltsc/whats-new-windows-11-2024)
- [LTSC Servicing Overview](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview#long-term-servicing-channel)
- [Windows IoT Enterprise Overview](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/overview)
