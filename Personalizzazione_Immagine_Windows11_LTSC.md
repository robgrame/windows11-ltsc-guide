# Personalizzazione Immagine Windows 11 LTSC e LTSC IoT 2024

## Panoramica

La personalizzazione dell'immagine per Windows 11 Enterprise LTSC 2024 e Windows 11 IoT Enterprise LTSC 2024 segue regole specifiche imposte dal modello di servicing LTSC. L'obiettivo è mantenere stabilità e integrità del sistema operativo, limitando le modifiche a ciò che è esplicitamente supportato.

Le personalizzazioni devono essere eseguite **offline** (prima del primo boot o del sysprep) tramite DISM oppure configurate tramite unattend.xml e provisioning package.

---

## Personalizzazioni supportate

### 1. Integrazione driver

```cmd
:: Montare l'immagine
Dism /Mount-Image /ImageFile:"D:\Images\install.wim" /Index:1 /MountDir:"D:\Mount"

:: Aggiungere un singolo driver
Dism /Image:"D:\Mount" /Add-Driver /Driver:"C:\Drivers\NIC\driver.inf"

:: Aggiungere una cartella di driver (ricorsivo)
Dism /Image:"D:\Mount" /Add-Driver /Driver:"C:\Drivers" /Recurse

:: Smontare e salvare
Dism /Unmount-Image /MountDir:"D:\Mount" /Commit
```

> **Nota:** Usare solo driver con file .inf firmati. Driver di terze parti con installer custom devono essere estratti prima dell'integrazione.

### 2. Windows Update (Cumulative Update)

```cmd
:: Aggiungere Cumulative Update (CU)
:: Nota: a partire da Windows 11 24H2/LTSC 2024, la SSU è inclusa nel pacchetto CU
Dism /Image:"D:\Mount" /Add-Package /PackagePath:"C:\Updates\CU-KB7654321.msu"
```

> **Nota:** A partire da Windows 11 24H2 (e quindi LTSC 2024), Microsoft ha unificato la Servicing Stack Update (SSU) all'interno del Cumulative Update. Non è più necessario installare la SSU separatamente.

### 3. Language Pack e componenti linguistici

```cmd
:: Aggiungere Language Pack completo
Dism /Image:"D:\Mount" /Add-Package /PackagePath:"E:\LangPacks\Microsoft-Windows-Client-Language-Pack_x64_it-it.cab"

:: Aggiungere componenti linguistici (handwriting, OCR, speech, text-to-speech)
Dism /Image:"D:\Mount" /Add-Package /PackagePath:"E:\LangPacks\Microsoft-Windows-LanguageFeatures-Handwriting-it-it-Package~amd64.cab"
Dism /Image:"D:\Mount" /Add-Package /PackagePath:"E:\LangPacks\Microsoft-Windows-LanguageFeatures-OCR-it-it-Package~amd64.cab"
Dism /Image:"D:\Mount" /Add-Package /PackagePath:"E:\LangPacks\Microsoft-Windows-LanguageFeatures-Speech-it-it-Package~amd64.cab"
Dism /Image:"D:\Mount" /Add-Package /PackagePath:"E:\LangPacks\Microsoft-Windows-LanguageFeatures-TextToSpeech-it-it-Package~amd64.cab"
```

> **Importante:** I Language Pack devono essere integrati **prima del primo boot** (offline servicing). Dopo il primo boot, l'integrazione potrebbe fallire o generare problemi.

### 4. Features on Demand (FoD)

Le Features on Demand supportate su LTSC includono un sottoinsieme delle FoD disponibili per le edizioni GA. Scaricare l'ISO FoD corrispondente alla versione LTSC dal VLSC.

```cmd
:: Abilitare .NET Framework 3.5
Dism /Image:"D:\Mount" /Enable-Feature /FeatureName:NetFx3 /All /Source:"E:\FoD\sources\sxs"

:: Aggiungere RSAT (esempio: Active Directory tools)
Dism /Image:"D:\Mount" /Add-Package /PackagePath:"E:\FoD\Microsoft-Windows-Rsat-ActiveDirectory-DS-LDS-Package~amd64.cab"

:: Elencare tutte le feature disponibili nell'immagine
Dism /Image:"D:\Mount" /Get-Features
```

#### FoD comunemente supportate su LTSC 2024

| Feature | Nome pacchetto |
|---------|---------------|
| .NET Framework 3.5 | `NetFx3` |
| PowerShell ISE | `Microsoft-Windows-PowerShell-ISE` |
| Windows Media Player | `WindowsMediaPlayer` |
| RSAT Tools | `Rsat.*` (vari pacchetti) |
| WordPad | ~~`Microsoft-Windows-WordPad`~~ — **Rimosso in Windows 11 24H2/LTSC 2024** |
| Steps Recorder | ~~`Microsoft-Windows-StepsRecorder`~~ — **Rimosso in Windows 11 24H2/LTSC 2024** |
| Font aggiuntivi | Vari pacchetti font |

#### FoD NON disponibili su LTSC

- Componenti correlati a Cortana
- Microsoft Store e framework correlati
- Widget e feature consumer Windows 11
- Copilot e integrazioni AI

### 5. Branding OEM e configurazione OOBE

Personalizzazione tramite **unattend.xml**:

```xml
<settings pass="specialize">
  <component name="Microsoft-Windows-Shell-Setup">
    <OEMInformation>
      <Manufacturer>Nome Azienda</Manufacturer>
      <Logo>C:\Windows\System32\oemlogo.bmp</Logo>
      <SupportPhone>+39 02 1234567</SupportPhone>
      <SupportURL>https://support.azienda.it</SupportURL>
    </OEMInformation>
    <ComputerName>*</ComputerName>
    <TimeZone>W. Europe Standard Time</TimeZone>
  </component>
</settings>

<settings pass="oobeSystem">
  <component name="Microsoft-Windows-Shell-Setup">
    <OOBE>
      <HideEULAPage>true</HideEULAPage>
      <HideWirelessSetupInOOBE>true</HideWirelessSetupInOOBE>
      <NetworkLocation>Work</NetworkLocation>
      <ProtectYourPC>1</ProtectYourPC>
    </OOBE>
  </component>
</settings>
```

---

## Personalizzazioni NON supportate

| Operazione | Motivo del blocco |
|------------|-------------------|
| Aggiungere Microsoft Store | Componente rimosso dall'immagine LTSC; non reintegrabile via DISM |
| Integrare Microsoft Edge (Chromium) | Non supportato come pacchetto DISM su LTSC |
| Aggiungere app consumer (Teams, Clipchamp, Phone Link) | Pacchetti AppX consumer non compatibili |
| Modificare componenti core del sistema | File protetti dal servicing stack |
| Aggiungere nuove app shell o modificare la shell | Bloccato a livello di OS |
| Installare Store app come parte dell'immagine | AppX framework limitato su LTSC |

---

## Funzionalità esclusive IoT Enterprise LTSC

Windows 11 IoT Enterprise LTSC 2024 include feature aggiuntive per scenari embedded e kiosk che **non sono disponibili** su Enterprise LTSC standard:

### Unified Write Filter (UWF)

Protegge il disco da scritture non autorizzate. Tutte le modifiche vengono scritte in un overlay in RAM e scartate al riavvio.

```powershell
# Abilitare la feature UWF
dism /online /enable-feature /featurename:Client-UnifiedWriteFilter

# Dopo il riavvio - proteggere il volume C:
uwfmgr volume protect C:

# Abilitare il filtro
uwfmgr filter enable

# Aggiungere esclusioni (cartelle che possono essere scritte)
uwfmgr file add-exclusion C:\Logs
uwfmgr file add-exclusion C:\AppData

# Aggiungere esclusioni registry
uwfmgr registry add-exclusion "HKLM\SOFTWARE\MyApp"
```

**Casi d'uso:** chioschi, ATM, dispositivi in ambienti pubblici dove il disco deve rimanere invariato.

### Shell Launcher V2

Permette di sostituire la shell Windows Explorer con un'applicazione custom.

```powershell
# Configurare Shell Launcher per un utente specifico
$namespace = "root\standardcimv2\embedded"
$class = "WESL_UserSetting"

Set-WmiInstance -Namespace $namespace -Class $class -Arguments @{
    User = "KioskUser"
    Shell = "C:\KioskApp\KioskApp.exe"
    ShellLauncherType = 0
}
```

**Casi d'uso:** dispositivi single-purpose, digital signage, terminali POS.

### Keyboard Filter

Blocca combinazioni di tasti specifiche per impedire all'utente di uscire dall'applicazione.

```powershell
# Bloccare Ctrl+Alt+Del
Set-WmiInstance -Namespace "root\standardcimv2\embedded" -Class "WEKF_PredefinedKey" -Arguments @{
    Id = "Ctrl+Alt+Del"
    Enabled = $true
}
```

### Gesture Filter

Disabilita gesture touch di sistema (swipe dal bordo, pinch-to-zoom su elementi OS) per dispositivi touchscreen in modalità kiosk.

### Custom Logon

Nasconde elementi dell'interfaccia di logon (pulsante di spegnimento, cambio utente, ecc.) per scenari embedded.

### Confronto completo IoT vs Enterprise LTSC

| Feature | IoT Enterprise LTSC | Enterprise LTSC |
|---------|:-------------------:|:---------------:|
| Unified Write Filter (UWF) | ✅ | ❌ |
| Shell Launcher V2 | ✅ | ❌ |
| Keyboard Filter | ✅ | ❌ |
| Gesture Filter | ✅ | ❌ |
| Custom Logon | ✅ | ❌ |
| Embedded Lockdown Manager | ✅ | ❌ |
| Assigned Access (Kiosk) completo | ✅ | ✅ (limitato) |
| AppLocker / WDAC | ✅ | ✅ |
| Supporto esteso | **10 anni** | 5 anni |

---

## Workflow di creazione immagine personalizzata

### Sequenza raccomandata

```
1. Montare install.wim
2. Integrare Servicing Stack Update (SSU)
3. Integrare Cumulative Update (CU)
4. Integrare Language Pack
5. Integrare componenti linguistici (handwriting, OCR, speech)
6. Integrare Features on Demand
7. Integrare driver
8. Smontare e committare l'immagine
9. Applicare l'immagine su hardware di riferimento
10. Configurare tramite unattend.xml / OOBE
11. Installare applicazioni LOB
12. Eseguire sysprep /generalize /oobe /shutdown
13. Catturare l'immagine finale
```

### Sysprep finale

```cmd
:: Eseguire sysprep con unattend
C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:C:\Unattend\unattend.xml
```

### Cattura immagine

```cmd
:: Catturare l'immagine dal reference machine
Dism /Capture-Image /ImageFile:"D:\Images\custom-ltsc.wim" /CaptureDir:"C:\" /Name:"Windows 11 LTSC 2024 Custom"
```

---

## Validazione dell'immagine

```cmd
:: Verificare le informazioni dell'immagine
Dism /Get-ImageInfo /ImageFile:"D:\Images\custom-ltsc.wim"

:: Verificare i pacchetti integrati
Dism /Image:"D:\Mount" /Get-Packages

:: Verificare le feature abilitate
Dism /Image:"D:\Mount" /Get-Features

:: Verificare i driver integrati
Dism /Image:"D:\Mount" /Get-Drivers
```

---

## Errori comuni da evitare

1. **Integrare Language Pack dopo il primo boot** → può causare corruzione dell'immagine
2. **Hardcodare il ComputerName** nel unattend → conflitti SID in deployment multipli
3. **Usare la ISO FoD sbagliata** → i pacchetti devono corrispondere esattamente alla build LTSC
4. **Tentare di aggiungere componenti consumer** → fallimento garantito, possibile corruzione
5. **Dimenticare di validare unattend.xml** con Windows System Image Manager (WSIM)
6. **Cercare di installare WordPad o Steps Recorder** → rimossi in Windows 11 24H2/LTSC 2024

---

## Riferimenti documentazione Microsoft

### Servicing e DISM
- [DISM Image Management](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism-image-management-command-line-options-s14)
- [DISM Operating System Package Servicing](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options)
- [Add and Remove Drivers Offline](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-and-remove-drivers-to-an-offline-windows-image)
- [Features on Demand (non-language)](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod)
- [Language Packs for Windows](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/add-language-packs-to-windows)

### Deployment e Sysprep
- [Sysprep Overview](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)
- [Unattended Windows Setup Reference](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/)
- [OEM Deployment of Windows](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/oem-deployment-of-windows-desktop-editions)
- [Windows System Image Manager (WSIM)](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/wsim/windows-system-image-manager-technical-reference)

### IoT Enterprise e Lockdown
- [Windows IoT Enterprise Overview](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/overview)
- [Unified Write Filter (UWF)](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/customize/unified-write-filter)
- [Shell Launcher V2](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/customize/shell-launcher)
- [Keyboard Filter](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/customize/keyboardfilter)
- [Assigned Access (Kiosk Mode)](https://learn.microsoft.com/en-us/windows/configuration/assigned-access/overview)

### LTSC
- [Windows 11 LTSC - What's New](https://learn.microsoft.com/en-us/windows/whats-new/ltsc/whats-new-windows-11-2024)
- [LTSC Servicing Overview](https://learn.microsoft.com/en-us/windows/deployment/update/waas-overview#long-term-servicing-channel)
- [Windows 11 IoT Enterprise LTSC Feature List](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/hardware/hardware-requirements)
