# Attivazione KMS per Windows 11 LTSC e LTSC IoT 2024

## Panoramica

Per attivare Windows 11 Enterprise LTSC 2024 e Windows 11 IoT Enterprise LTSC 2024 tramite un server KMS esistente, è necessario installare una **nuova KMS Host Key (CSVLK)** che supporti queste edizioni. Non è richiesto un server KMS dedicato.

## Prerequisiti

- Server KMS con Windows Server 2016 o successivo
- Accesso al portale Volume Licensing Service Center (VLSC)
- Porta TCP 1688 aperta tra client e server KMS
- Minimo **25 client** Windows per raggiungere la soglia di attivazione KMS

## Procedura sul server KMS

```cmd
:: 1. Installare la nuova CSVLK per Windows 11 LTSC 2024
slmgr /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX

:: 2. Attivare l'host KMS con Microsoft
slmgr /ato

:: 3. Verificare lo stato dell'host KMS
slmgr /dlv
```

> **Nota:** La CSVLK per Windows 11 è retrocompatibile e attiva anche le versioni precedenti (Windows 10, ecc.).

## GVLK per i client

| Edizione | GVLK |
|----------|------|
| Windows 11 Enterprise LTSC 2024 | `M7XTQ-FN8P6-TTKYV-9D4CC-J462D` |
| Windows 11 IoT Enterprise LTSC 2024 | `KBN8V-HFGQ4-MGXVD-347P6-PDQGT` |

## Configurazione lato client

```cmd
:: Impostare la GVLK (se non già presente)
slmgr /ipk M7XTQ-FN8P6-TTKYV-9D4CC-J462D

:: (Opzionale) Puntare manualmente al server KMS
slmgr /skms nome_server_kms:1688

:: Forzare l'attivazione
slmgr /ato
```

## Riferimenti documentazione Microsoft

- [KMS Client Setup Keys (GVLK)](https://learn.microsoft.com/en-us/windows-server/get-started/kms-client-activation-keys)
- [Volume Activation Overview](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/volume-activation-windows)
- [Configure a KMS Host](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/configure-a-kms-host)
- [Plan for Volume Activation](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/plan-for-volume-activation-client)
- [Volume Licensing Service Center (VLSC)](https://www.microsoft.com/Licensing/servicecenter/default.aspx)
- [Windows 11 LTSC 2024 - What's New](https://learn.microsoft.com/en-us/windows/whats-new/ltsc/whats-new-windows-11-2024)
