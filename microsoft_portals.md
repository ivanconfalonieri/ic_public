# Portali Microsoft - RECAP

### 🧭 **Tabella riassuntiva e comparativa dei portali Microsoft**

| **Portale**                       | **URL**                                           | **Funzioni principali**                                                       | **Sovrapposizioni / Differenze**                                                                              | **Target**                       |
| --------------------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| **Microsoft 365 Admin Center**    | `admin.microsoft.com`                             | Gestione utenti, licenze, gruppi, servizi Microsoft 365, domini, billing      | È il punto centrale per la maggior parte delle attività M365. Rimanda ad altri portali per gestione avanzata. | IT Admin M365 / Helpdesk         |
| **Azure Portal**                  | `portal.azure.com`                                | Gestione servizi cloud, risorse Azure, networking, VM, Azure AD, log, costi   | È il portale cloud principale, include molti sottosistemi. Include accesso a Entra, Intune, ecc.              | IT Admin / DevOps / Sviluppatori |
| **Microsoft Entra Admin Center**  | `entra.microsoft.com`                             | Identità, accesso, autenticazione, Conditional Access, Identity Governance    | È l'evoluzione di Azure AD Portal. Alcune configurazioni restano anche in Azure Portal.                       | Identity & Security Admin        |
| **Microsoft Intune (Endpoint)**   | `intune.microsoft.com` o `endpoint.microsoft.com` | Gestione dispositivi, app, aggiornamenti, policy, sicurezza endpoint          | Si integra con Entra e Azure per identità e policy. Parte di Microsoft Endpoint Manager.                      | Endpoint Admin / IT Security     |
| **Microsoft Defender / Security** | `security.microsoft.com`                          | Sicurezza (Microsoft Defender XDR), gestione minacce, incidenti, SIEM         | Lavora a stretto contatto con Entra (identità) e Intune (device).                                             | SOC Team / IT Security           |
| **Microsoft Purview**             | `compliance.microsoft.com`                        | Data governance, compliance, protezione info, eDiscovery                      | Focus su compliance e regolamenti. Si integra con Entra per accessi e Intune per protezione dei dati.         | Compliance Officer / CISO        |
| **Exchange Admin Center**         | `admin.exchange.microsoft.com`                    | Email, flussi, regole, protezione posta, caselle, connettori                  | È accessibile anche da M365 Admin Center. Gestione specifica Exchange Online.                                 | Exchange Admin / Email Admin     |
| **SharePoint Admin Center**       | `admin.sharepoint.com`                            | Gestione SharePoint Online, siti, storage, OneDrive, condivisione             | È parte del tenant Microsoft 365, ma ha logica separata da Teams e Exchange.                                  | SharePoint Admin / IT Admin      |
| **Teams Admin Center**            | `admin.teams.microsoft.com`                       | Policy, configurazione, gestione utenti, voce, dispositivi Teams              | Legato a Microsoft 365, ma con gestione molto dettagliata di policy e permessi.                               | Teams Admin / Comunicazioni      |
| **Power Platform Admin Center**   | `admin.powerplatform.microsoft.com`               | Gestione ambienti Power Apps, Power Automate, sicurezza app, DLP policy       | Integrato con Entra (per le identità) e M365 (per i connettori).                                              | Power Users / App Maker / Admin  |
| **Partner Center**                | `partner.microsoft.com`                           | Gestione clienti, licenze CSP, incentivi, profilo partner, supporto Microsoft | Usato dai partner Microsoft e CSP per rivendere, supportare, accedere ai benefici.                            | Partner / MSP / Rivenditori      |
| **Windows Autopilot**             | Parte di `endpoint.microsoft.com`                 | Provisioning automatizzato di dispositivi Windows                             | Fa parte di Intune. Serve per preconfigurare dispositivi prima della consegna.                                | IT Admin / Deployment Specialist |
| **Microsoft Learn / Training**    | `learn.microsoft.com`                             | Formazione ufficiale, documentazione, laboratori, certificazioni              | Non è un portale admin, ma fondamentale per formazione e aggiornamento.                                       | Tutti gli utenti                 |
| **Azure DevOps**                  | `dev.azure.com`                                   | CI/CD, repository, work items, pipelines, project tracking                    | Non è parte diretta di M365. Incentrato su sviluppo e delivery di software.                                   | Sviluppatori / DevOps            |
| **Visual Studio Subscriptions**   | `visualstudio.microsoft.com`                      | Gestione licenze e benefici per sviluppatori                                  | Solo per utenti con sottoscrizioni VS. Non legato a M365 direttamente.                                        | Sviluppatori                     |
| **Microsoft Volume Licensing**    | `licensing.microsoft.com`                         | Gestione licenze volume (contratti aziendali, download software, product key) | Usato da aziende con contratti Enterprise Agreement.                                                          | Procurement / IT Admin           |

---

### 🔍 **Confronto diretto tra portali simili o collegati**

| **Portale A**                       | **Portale B**                           | **Differenze principali**                                                                                       |
| ----------------------------------- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Azure Portal**                    | **Microsoft Entra**                     | Entra è focalizzato solo su identità e accessi; Azure gestisce tutte le risorse cloud, incluse quelle di Entra. |
| **Intune (endpoint.microsoft.com)** | **Azure Portal**                        | Intune gestisce dispositivi; Azure può mostrarli ma non offre la gestione completa (es. policy, app, update).   |
| **Microsoft 365 Admin Center**      | **Exchange / SharePoint / Teams Admin** | M365 Admin è un “cruscotto” centrale, ma per modifiche avanzate reindirizza agli admin center specifici.        |
| **Security Portal**                 | **Purview (Compliance)**                | Security = protezione attiva (minacce); Purview = governance dei dati e conformità.                             |
| **Entra**                           | **Intune / Endpoint**                   | Entra gestisce chi accede; Intune cosa può fare il dispositivo con quell’identità.                              |
| **Power Platform**                  | **Azure Portal**                        | Power Platform è low-code/no-code; Azure offre strumenti di sviluppo pro e infrastruttura.                      |

---

![image](https://github.com/user-attachments/assets/315e38f8-9a1f-4ab4-be0d-a7a66cd0636d)

---

### 🔧 **portal.azure.com (Azure Portal)** – *Architetturale*

* **Focus**: infrastruttura, risorse cloud, rete, VM, database, sicurezza, automazione.
* **Livello**: *infrastrutturale e cloud-native* (IaaS, PaaS).
* **Gestisce**:

  * Risorse distribuite (VM, network, storage account)
  * Azure Active Directory (ora Microsoft Entra)
  * Integrazione con automazione, DevOps, e monitoraggio
* **Target**: Cloud architect, DevOps, sistemisti, IT esperti

---

### ⚙️ **admin.microsoft.com (Microsoft 365 Admin Center)** – *Applicativo*

* **Focus**: servizi Microsoft 365 (licenze, utenti, posta, Teams, OneDrive).
* **Livello**: *applicativo e user-oriented* (SaaS).
* **Gestisce**:

  * Utenti, gruppi, licenze
  * Configurazioni Exchange, SharePoint, Teams
  * Fatturazione, supporto, compliance di alto livello
* **Target**: IT Admin, helpdesk, amministratori tenant M365

---

### 🔁 **Sovrapposizione tra i portali**

| **Ambito**                         | **Portale Primario**                             | **Portale Secondario (accesso parziale o duplicato)**                     | **Note**                                                                    |
| ---------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **Identità (Azure AD / Entra ID)** | `entra.microsoft.com` (Entra)                    | `portal.azure.com` mostra le stesse identità con meno feature             | Entra è il nuovo standard, ma Azure resta agganciato per retrocompatibilità |
| **Utenti Microsoft 365**           | `admin.microsoft.com`                            | `entra.microsoft.com` li mostra anche come utenti di directory            | Entrambi modificano utenti, ma Entra ha più opzioni su MFA, CA, federazione |
| **Ruoli e permessi**               | `entra.microsoft.com`                            | `admin.microsoft.com` permette cambio ruoli limitati (es. admin Exchange) | I ruoli granulari vanno sempre fatti in Entra                               |
| **Sicurezza e autenticazione**     | `entra.microsoft.com` / `security.microsoft.com` | `admin.microsoft.com` mostra sintesi e alert minori                       | Admin M365 vede alert, ma non configura policy avanzate                     |
| **Dispositivi**                    | `endpoint.microsoft.com` (Intune)                | `entra.microsoft.com` li mostra, ma non li gestisce                       | Entra mostra registrazione, Intune li configura                             |
| **Accesso alle app SaaS**          | `entra.microsoft.com`                            | visibile anche in `admin.microsoft.com` → **Azure AD Apps**               | Solo Entra permette SSO, provisioning automatico, ecc.                      |

---

### 🧠 **...in pratica...**

* 🔵 **Azure Portal** → il "datacenter virtuale"
* 🟢 **Entra** → la "portineria" (controlla chi entra, come e con quali chiavi)
* 🟠 **Microsoft 365 Admin** → la "reception aziendale" (gestisce chi lavora, con quali strumenti)
* 🔴 **Intune** → la "sala IT" (gestisce dispositivi, policy, aggiornamenti)

---



