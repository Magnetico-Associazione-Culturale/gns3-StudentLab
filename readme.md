# Guida rapida: Configurazione del Laboratorio GNS3 con Vagrant 🚀

Questa guida ti aiuterà a configurare rapidamente l'ambiente di laboratorio GNS3 sul tuo computer, utilizzando Vagrant per semplificare l'installazione della macchina virtuale.

## 1. Prerequisiti Essenziali

Prima di iniziare, assicurati di avere installato i seguenti software e di aver configurato il tuo computer correttamente:

* **Vagrant:** Lo strumento che useremo per gestire la macchina virtuale.
    * **Scarica da:** [https://www.vagrantup.com/downloads](https://www.vagrantup.com/downloads)
    * Segui le istruzioni di installazione per il tuo sistema operativo.
    * **Importante:** Dopo l'installazione, chiudi e riapri il terminale/Prompt dei Comandi. Verifica l'installazione digitando `vagrant --version`.
* **Oracle VirtualBox:** Il software di virtualizzazione su cui girerà la macchina virtuale GNS3.
    * **Scarica da:** [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
    * Segui le istruzioni di installazione per il tuo sistema operativo.
    * **Cruciale:** Assicurati che la **Tecnologia di Virtualizzazione (Intel VT-x / AMD-V)** sia abilitata nelle impostazioni del BIOS/UEFI del tuo computer. Senza di essa, le macchine virtuali non potranno essere eseguite.

    ### Verifica la Tecnologia di Virtualizzazione (VT-x / AMD-V) ✅

    La virtualizzazione hardware è **indispensabile** per il corretto funzionamento di VirtualBox. Ecco come verificare se è abilitata sul tuo sistema:

    * **Per Windows:**
        1.  Apri il **Task Manager** (Gestione attività) premendo `Ctrl + Shift + Esc` o cliccando con il tasto destro sulla barra delle applicazioni e selezionando "Gestione attività".
        2.  Vai alla scheda **"Prestazioni"** (Performance).
        3.  Clicca su **"CPU"**.
        4.  Cerca la voce **"Virtualizzazione"** (Virtualization). Dovrebbe essere indicata come **"Abilitato"** (Enabled).
        5.  Se è "Disabilitato", dovrai abilitarla nel BIOS/UEFI del tuo computer (vedi sotto).
            (Vedi immagine image_15121c.png, image_14f836.png, image_0b15b7.png, image_0a1977.png per riferimento)
    * **Per macOS:**
        1.  Apri l'applicazione **Terminale** (cerca in Spotlight o in `Applicazioni/Utility`).
        2.  Digita il seguente comando e premi Invio:
            ```bash
            sysctl -a | grep -E "machdep.cpu.features|VMX"
            ```
        3.  Se l'output include `VMX`, la virtualizzazione Intel VT-x è supportata e abilitata.
    * **Per Linux:**
        1.  Apri un terminale.
        2.  Digita il seguente comando e premi Invio:
            ```bash
            lscpu | grep -E "Virtualization|VT-x|AMD-V"
            ```
        3.  Cerca le voci come `Virtualization: VT-x` (Intel) o `Virtualization: AMD-V` (AMD).
        4.  Poi verifica se il kernel supporta i moduli:
            ```bash
            grep -E 'svm|vmx' /proc/cpuinfo
            ```
            Se non c'è output, potrebbe non essere abilitata o supportata.

    * **Se la Virtualizzazione è Disabilitata:** Dovrai **riavviare il tuo computer** e accedere alle impostazioni del **BIOS/UEFI** (solitamente premendo un tasto come `Canc`, `F2`, `F10`, `F12` all'avvio). Cerca una sezione relativa a "CPU Configuration", "Virtualization Technology", "VT-x", "AMD-V" o "Intel Virtualization Technology" e **abilitarla**. Salva le modifiche e riavvia il sistema.

* **GNS3 Graphical User Interface (GUI):** L'applicazione che userai sul tuo computer per disegnare le topologie di rete e connetterti alla macchina virtuale GNS3.
    * **Scarica da:** [https://gns3.com/software/download](https://gns3.com/software/download)
    * Installa la versione adatta al tuo sistema operativo.
* **Risorse Hardware:** Per un'esperienza fluida, è consigliato avere almeno **8 GB di RAM totali** sul tuo computer (la VM GNS3 userà 4 GB) e una CPU moderna.

---

## 2. Configurazione del Laboratorio GNS3

Segui attentamente questi passaggi per preparare il tuo ambiente di laboratorio:

### Passaggio 1: Scarica il Package del Laboratorio e Prepara la Cartella

1.  **Scarica il file ZIP del laboratorio** che ti è stato fornito (es. `GNS3_Lab_VM_v1.0.1.zip` o `GNS3.VM.VirtualBox.2.2.50.zip`). Questo file contiene la configurazione di Vagrant e la macchina virtuale GNS3 pre-configurata. **Salva questo file nella tua cartella `Downloads` (`Scarica`).**

2.  **Apri un terminale** sul tuo computer:
    * **Windows:** Cerca "PowerShell" nel menu Start e aprilo.
    * **macOS/Linux:** Apri l'applicazione "Terminale".

3.  **Crea la cartella per il tuo laboratorio GNS3 e naviga al suo interno.** Questa è la cartella principale del tuo progetto, dove saranno estratti tutti i file.

    * **Per Windows (PowerShell):**
        ```powershell
        New-Item -Path "$env:USERPROFILE\Documents\" -Name "GNS3-Lab" -ItemType "Directory"
        cd "$env:USERPROFILE\Documents\GNS3-Lab"
        ```

    * **Per macOS/Linux (Bash):**
        ```bash
        mkdir -p ~/Documents/GNS3-Lab
        cd ~/Documents/GNS3-Lab
        ```

### Passaggio 2: Estrai il Contenuto nella Cartella del Laboratorio

Ora che sei nella cartella corretta, estrai il file ZIP scaricato al suo interno.

1.  **Estrai il contenuto del file ZIP** nella cartella corrente del tuo laboratorio.

    * **Per Windows (PowerShell):**
        ```powershell
        Expand-Archive -LiteralPath "$env:USERPROFILE\Downloads\GNS3.VM.VirtualBox.2.2.50.zip" -DestinationPath "."
        ```
        *(Nota: `.` indica la cartella corrente. Sostituisci `GNS3.VM.VirtualBox.2.2.50.zip` con il nome esatto del file ZIP che hai scaricato se diverso.)*

    * **Per macOS/Linux (Bash):**
        ```bash
        unzip "$HOME/Downloads/GNS3_Lab_VM_v1.0.1.zip" -d "."
        ```
        *(Nota: `.` indica la cartella corrente. Sostituisci `GNS3_Lab_VM_v1.0.1.zip` con il nome esatto del file ZIP che hai scaricato se diverso.)*
        * Se `unzip` non è installato, potresti doverlo installare: `sudo apt install unzip` (Linux Debian/Ubuntu) o `brew install unzip` (macOS con Homebrew).

    **Dopo l'estrazione, la cartella `GNS3-Lab` (o qualsiasi nome tu le abbia dato) dovrebbe contenere `Vagrantfile` e `gns3-vm.box`.**

### Passaggio 3: Avvia la Macchina Virtuale GNS3

1.  **Assicurati di essere ancora nella cartella del tuo laboratorio GNS3** (dove hai estratto i file) nel terminale.
    * Se hai chiuso il terminale o cambiato directory, riaprilo e naviga di nuovo a quella cartella:
        * **Per Windows (PowerShell):** `cd "$env:USERPROFILE\Documents\GNS3-Lab"`
        * **Per macOS/Linux (Bash):** `cd ~/Documents/GNS3-Lab`
2.  **Avvia la macchina virtuale GNS3** con Vagrant. Questo comando impiegherà alcuni minuti, specialmente la prima volta, poiché Vagrant importerà la VM e la configurerà:
    ```bash
    vagrant up
    ```
    * **Cosa succede:** Vagrant rileverà il file `gns3-vm.box` locale, lo aggiungerà al suo catalogo interno (se non presente), creerà una nuova macchina virtuale in VirtualBox, le assegnerà 4GB di RAM e 2 CPU, e configurerà la rete Host-Only con l'indirizzo IP **`192.168.56.101`**.
    * Durante l'avvio, potresti vedere messaggi relativi all'autenticazione SSH. Se la VM si avvia correttamente, significa che l'autenticazione ha funzionato.

### Passaggio 4: Configura la GUI di GNS3 per Connettersi alla VM

Una volta che la macchina virtuale è avviata (il terminale dovrebbe tornare al prompt dopo `vagrant up` e la VM dovrebbe essere "Running" in VirtualBox), puoi connettere la GUI di GNS3:

1.  **Avvia l'applicazione GNS3 GUI** sul tuo computer (non dentro la VM).
2.  Nel menu di GNS3, vai su `Modifica` (Edit) > `Preferenze` (Preferences).
3.  Nella finestra delle Preferenze, seleziona `Server` > `Server Remoti` (Remote Servers) nel pannello di sinistra.
4.  Se GNS3 ha già rilevato un server, puoi modificarlo. Altrimenti, clicca su **`Aggiungi` (Add)**.
5.  Configura il server remoto con i seguenti dettagli:
    * **Host:** `192.168.56.101` (Questo è l'indirizzo IP che Vagrant ha assegnato alla tua VM GNS3 sulla rete Host-Only).
    * **Porta (Port):** `3080`
6.  Clicca `Applica` (Apply) o `OK`.
7.  Verifica che la connessione al server remoto sia riuscita. Dovrebbe esserci un **puntino verde** accanto all'indirizzo IP del server in `Server Remoti`.

---

## 3. Inizia il Tuo Laboratorio! 🥳

Ora sei pronto per iniziare i tuoi esercizi di laboratorio GNS3!

* Trascina e rilascia i dispositivi dalla barra laterale sul tuo spazio di lavoro.
* Tutte le immagini dei dispositivi che l'istruttore ha pre-caricato nella VM saranno immediatamente disponibili.
* I dispositivi verranno eseguiti all'interno della macchina virtuale GNS3, liberando risorse dal tuo computer host.

---

## 4. Comandi Vagrant Utili

Questi comandi ti aiuteranno a gestire la tua macchina virtuale GNS3:

* **`vagrant up`**: Avvia la VM. Se è la prima volta, la importerà e configurerà.
* **`vagrant suspend`**: Sospende la VM. Salva lo stato attuale della VM, permettendoti di riprendere rapidamente più tardi.
* **`vagrant resume`**: Riprende una VM sospesa.
* **`vagrant halt`**: Spegne la VM in modo pulito. Non salva lo stato, è come un normale spegnimento.
* **`vagrant status`**: Mostra lo stato attuale della VM.
* **`vagrant destroy`**: Elimina completamente la VM da VirtualBox. **Attenzione: Questo comando elimina la macchina virtuale e qualsiasi configurazione o dato al suo interno.** Usalo se vuoi ricominciare da zero o se il tuo laboratorio è in uno stato irrecuperabile.

---

## 5. Risoluzione dei Problemi Comuni 💡

* **`vagrant` non riconosciuto:** Assicurati che Vagrant sia installato correttamente e che il tuo terminale sia stato riavviato dopo l'installazione.
* **La VM non si avvia / Errore di Virtualizzazione:** **Rivedi il "Passaggio 1: Prerequisiti Essenziali" per verificare che la Tecnologia di Virtualizzazione sia abilitata.** Se è disabilitata, dovrai abilitarla nel BIOS/UEFI del tuo computer.
* **La GUI di GNS3 non si connette alla VM:**
    * Verifica che la VM sia in esecuzione (`vagrant status` dovrebbe mostrare `running`).
    * Assicurati di aver inserito l'indirizzo IP corretto (`192.168.56.101`) e la porta (`3080`) nelle preferenze della GUI di GNS3.
    * Apri VirtualBox Manager: vai su `File` > `Gestore reti host...` (Host Network Manager...). Assicurati che esista un "VirtualBox Host-Only Ethernet Adapter" (es. `vboxnet0`) e che sia attivo.
* **Spazio su Disco:** I file delle VM e delle immagini possono essere grandi. Assicurati di avere abbastanza spazio libero sul disco prima di scaricare il package del laboratorio.