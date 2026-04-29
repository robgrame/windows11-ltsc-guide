# Windows 11 LTSC e IoT Enterprise — Guida Operativa

Raccolta di documentazione operativa per la gestione di Windows 11 Enterprise LTSC 2024, Windows 11 IoT Enterprise LTSC 2024 e Windows 11 IoT Enterprise GA in ambienti enterprise.

## Prefazione: Le edizioni Windows 11 per scenari specializzati

Microsoft offre diverse edizioni di Windows 11 pensate per scenari che vanno oltre la postazione di lavoro tradizionale. Comprendere le differenze è fondamentale per scegliere l'edizione corretta e prevedere le implicazioni su software, aggiornamenti e ciclo di vita dei dispositivi.

### Windows 11 Enterprise LTSC (Long-Term Servicing Channel)

Edizione progettata per **workstation che richiedono stabilità assoluta** e non necessitano di feature update. Il sistema operativo viene rilasciato in una versione "congelata" che riceve esclusivamente aggiornamenti di sicurezza per **5 anni**. Tipicamente impiegata in scenari regolamentati, ambienti di produzione industriale, o postazioni dove le modifiche al sistema devono essere ridotte al minimo. Non include Microsoft Store, app consumer, né Copilot.

### Windows 11 IoT Enterprise LTSC

Variante della precedente destinata a **dispositivi embedded e special-purpose** con un ciclo di supporto esteso a **10 anni**. Include tutte le funzionalità di Enterprise LTSC più un set di feature dedicate alla protezione e al lockdown del dispositivo:

- **Unified Write Filter (UWF)** — protezione disco tramite overlay in RAM
- **Shell Launcher V2** — sostituzione dell'interfaccia Windows con un'applicazione custom
- **Keyboard Filter** — blocco combinazioni tasti per impedire l'uscita dall'app
- **Custom Logon** — personalizzazione/nascondimento dell'interfaccia di logon

Ideale per chioschi, ATM, dispositivi medicali, SCADA e digital signage.

### Windows 11 IoT Enterprise GA (General Availability Channel)

Edizione meno nota ma strategicamente importante: offre le **stesse feature embedded di IoT LTSC** (UWF, Shell Launcher, Keyboard Filter) ma segue il modello di servicing standard con **feature update annuali** e supporto di **36 mesi per versione**. Questo la rende compatibile con software che richiede un OS aggiornato (Teams, Microsoft 365 Apps) pur mantenendo le capacità di lockdown IoT.

È la scelta giusta quando servono sia le feature embedded sia la compatibilità software moderna, a patto di accettare aggiornamenti periodici.

### Riepilogo edizioni

| Edizione | Stabilità | Supporto | Feature embedded | Software moderno |
|----------|:---------:|:--------:|:----------------:|:----------------:|
| Enterprise LTSC | ✅ Massima | 5 anni | ❌ | ❌ Limitato |
| IoT Enterprise LTSC | ✅ Massima | **10 anni** | ✅ | ❌ Limitato |
| IoT Enterprise GA | ⚠️ Feature update | 36 mesi | ✅ | ✅ Ampio |

---

## Contenuto

| Documento | Descrizione |
|-----------|-------------|
| [KMS_Windows11_LTSC_Activation.md](KMS_Windows11_LTSC_Activation.md) | Attivazione KMS: procedura server e client, GVLK |
| [Intune_StoreApps_Windows11_LTSC.md](Intune_StoreApps_Windows11_LTSC.md) | Distribuzione app Store tramite Intune su LTSC |
| [Personalizzazione_Immagine_Windows11_LTSC.md](Personalizzazione_Immagine_Windows11_LTSC.md) | Creazione e personalizzazione immagine OS |
| [Supportabilita_Software_Microsoft_LTSC.md](Supportabilita_Software_Microsoft_LTSC.md) | Matrice compatibilità software Microsoft su LTSC |
| [IoT_Enterprise_GA_vs_LTSC.md](IoT_Enterprise_GA_vs_LTSC.md) | Confronto IoT Enterprise GA vs LTSC — differenze, software supportato, scelta |

## Disclaimer

Le informazioni contenute in questi documenti sono basate sulla documentazione Microsoft ufficiale disponibile ad aprile 2025. Verificare sempre le fonti originali per aggiornamenti. Per software di terze parti, consultare il rispettivo vendor.
