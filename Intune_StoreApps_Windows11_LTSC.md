# Distribuzione App Microsoft Store tramite Intune su Windows 11 LTSC e LTSC IoT

## Panoramica

Le edizioni Windows 11 Enterprise LTSC 2024 e Windows 11 IoT Enterprise LTSC 2024 **non includono il Microsoft Store** per impostazione predefinita. Tuttavia, è possibile distribuire app dello Store tramite Microsoft Intune sfruttando l'integrazione con **WinGet** (Windows Package Manager), che non richiede la presenza dell'app Store sul dispositivo.

## Chiarimento sulla posizione ufficiale Microsoft

La documentazione Microsoft riporta:

> *"Windows 11 Enterprise LTSC 2024 was first available on October 1, 2024. Features in Windows 11 Enterprise LTSC 2024 are similar to Windows 11, version 24H2. The LTSC release is intended for special use devices. Support for LTSC by apps and tools, such as in-box apps and Microsoft Store, that are designed for the general availability channel release of Windows might be limited."*

### Cosa significa concretamente

| Cosa dice la documentazione | Significato pratico |
|-----------------------------|---------------------|
| "might be limited" | Le app non sono testate, garantite né supportate su LTSC |
| "special use devices" | LTSC è destinato a chioschi, ATM, dispositivi medicali, SCADA — usarlo come workstation generica è fuori dallo scenario supportato |
| "in-box apps" | App come Calcolatrice, Foto, Paint non sono preinstallate o sono in versione legacy ridotta |
| "Microsoft Store" | Non è presente di default, bisogna usare WinGet o sideload |

### Implicazioni operative

- **Non è tecnicamente impossibile** installare e usare Store app su LTSC, ma Microsoft **non garantisce** compatibilità presente e futura
- Gli aggiornamenti delle app potrebbero smettere di supportare LTSC in qualsiasi momento
- Eventuali bug riscontrati su LTSC non sono prioritari per i team di sviluppo delle app
- Se un'app smette di funzionare dopo un aggiornamento, **non è coperto dal supporto Microsoft**

### Raccomandazione

Prima di distribuire app Store su LTSC in produzione:
1. Testare l'app nell'ambiente LTSC target
2. Verificare che il vendor dell'app supporti ufficialmente LTSC
3. Avere un piano di fallback (es. versione Win32/desktop dell'app)
4. Documentare che si opera in uno scenario "best effort" e non formalmente supportato

---

## Prerequisiti sui dispositivi LTSC

### WinGet (Windows Package Manager)

WinGet deve essere presente sul dispositivo. Verificare con:

```cmd
winget --version
```

Se assente, installare manualmente le seguenti dipendenze in ordine:

1. **Microsoft.VCLibs** (VC++ UWP Desktop Runtime)
2. **Microsoft.UI.Xaml**
3. **Microsoft.DesktopAppInstaller** (include WinGet)

```powershell
# Installazione dipendenze e App Installer
Add-AppxPackage -Path "Microsoft.VCLibs.x64.14.00.Desktop.appx"
Add-AppxPackage -Path "Microsoft.UI.Xaml.2.8.x64.appx"
Add-AppxPackage -Path "Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle"
```

### Dove scaricare i pacchetti

| Pacchetto | Fonte |
|-----------|-------|
| App Installer + WinGet | [GitHub - microsoft/winget-cli/releases](https://github.com/microsoft/winget-cli/releases) |
| Dipendenze (VCLibs, UI.Xaml) | Incluse negli asset della release GitHub |
| Download diretto dallo Store | [store.rg-adguard.net](https://store.rg-adguard.net/) (Product ID: `9NBLGGH4NNS1`) |

### Altre condizioni

- Sideloading abilitato (default su Windows 11)
- Dispositivo enrolled in Microsoft Intune
- Connettività internet per il download delle app

## Distribuzione da Intune: "Microsoft Store app (new)"

### Procedura

1. Accedere a [Microsoft Intune admin center](https://intune.microsoft.com)
2. Navigare in **Apps > Windows > + Add**
3. Selezionare il tipo **"Microsoft Store app (new)"**
4. Cercare l'app desiderata nel catalogo (es. Company Portal, Microsoft To Do)
5. Configurare le informazioni dell'app e le assegnazioni:
   - **Required**: installazione automatica sul dispositivo
   - **Available**: l'utente può installarla dal Company Portal
6. Salvare e monitorare lo stato in **Apps > Monitor > App install status**

### Funzionamento

- Intune utilizza **WinGet** in background per l'installazione
- **Non è necessaria** la presenza dell'app Microsoft Store sul client
- Gli aggiornamenti delle app vengono gestiti tramite WinGet

## Alternativa: distribuzione come Win32 App

Per app non disponibili nel catalogo WinGet o per scenari offline:

1. Scaricare il pacchetto MSIX/AppX dell'app
2. Creare un pacchetto `.intunewin` con [Microsoft Win32 Content Prep Tool](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool)
3. In Intune: **Apps > Windows > + Add > Windows app (Win32)**
4. Configurare comando di installazione, detection rules e assegnazioni

## Considerazioni per LTSC IoT

- Stesse modalità di LTSC Enterprise, con identico supporto WinGet
- Verificare che il profilo IoT non abbia restrizioni aggiuntive su AppX/MSIX
- In ambienti air-gapped, preferire la distribuzione Win32 offline

## Riferimenti documentazione Microsoft

- [Add Microsoft Store apps to Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/apps/store-apps-microsoft)
- [Prerequisites for Microsoft Store apps in Intune](https://learn.microsoft.com/en-us/mem/intune/apps/store-apps-microsoft#prerequisites)
- [Windows Package Manager (WinGet) documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/)
- [Install WinGet client prerequisites](https://learn.microsoft.com/en-us/windows/package-manager/winget/#install-winget)
- [Add Win32 apps to Microsoft Intune](https://learn.microsoft.com/en-us/mem/intune/apps/apps-win32-app-management)
- [Microsoft Win32 Content Prep Tool](https://learn.microsoft.com/en-us/mem/intune/apps/apps-win32-prepare)
- [Windows 11 LTSC - What's new](https://learn.microsoft.com/en-us/windows/whats-new/ltsc/whats-new-windows-11-2024)
- [App Installer - GitHub releases](https://github.com/microsoft/winget-cli/releases)
