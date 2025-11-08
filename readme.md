# Guida rapida: Configura il tuo laboratorio GNS3 personale  

Questa guida ti aiuterà a configurare rapidamente l'ambiente di laboratorio GNS3 sul tuo computer, per semplificare l'installazione della macchina virtuale e la sua gestione su [VirtualBox](https://www.virtualbox.org/) userai [Vagrant](https://developer.hashicorp.com/vagrant)

## 1. Prerequisiti 

Prima di iniziare, verifica che il tuo dispositivo soddisfi i requisiti minimi necessari ed assicurati di avere installato i seguenti software:

### Requisiti di sistema
* **≥ 4GB RAM** disponibile
* Virtualizzazione abilitata (VT-x / AMD-V) 

    La virtualizzazione hardware è **indispensabile** per il corretto funzionamento di VirtualBox. Ecco come verificare se è abilitata sul tuo sistema:

    * **Per Windows:**
        1.  Apri il **Task Manager** (Gestione attività) premendo `Ctrl + Shift + Esc` o cliccando con il tasto destro sulla barra delle applicazioni e selezionando "Gestione attività".
        2.  Vai alla scheda **"Prestazioni"** (Performance).
        3.  Clicca su **"CPU"**.
        4.  Cerca la voce **"Virtualizzazione"** (Virtualization). Dovrebbe essere indicata come **"Abilitato"** (Enabled).
        5.  Se è "Disabilitato", dovrai abilitarla nel BIOS/UEFI del tuo computer (vedi sotto).
            (Vedi immagine image_15121c.png, image_14f836.png, image_0b15b7.png, image_0a1977.png per riferimento)
    * **Per macOS con Processori Intel:**
        1.  Apri l'applicazione **Terminale** (cerca in Spotlight o in `Applicazioni/Utility`).
        2.  Digita il seguente comando e premi Invio:
            ```bash
            sysctl -a | grep -E "machdep.cpu.features|VMX"
            ```
        3.  Se l'output include `VMX`, la virtualizzazione Intel VT-x è supportata e abilitata.
  **Per macOS con Processori Apple Silicon(M1,M2,M3,M4,M5,...):**
        1.  Apri l'applicazione **Terminale** (cerca in Spotlight o in `Applicazioni/Utility`).
        2.  Digita il seguente comando e premi Invio:
            ```bash
            sysctl kern.hv_support
            ```
        3.  Se l'output include "kern.hv_support: 1", la virtualizzazione Intel VT-x è supportata e abilitata.
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

### Software Necessari
Assicurati di aver installato i seguenti software prima di procedere con il setup del tuo laboratorio su GNS3

* **Vagrant:** Lo strumento che useremo per gestire la macchina virtuale.
    * **Scarica da:** [https://www.vagrantup.com/downloads](https://www.vagrantup.com/downloads)
    * Segui le istruzioni di installazione per il tuo sistema operativo.
    * **Importante:** Dopo l'installazione, chiudi e riapri il terminale/Prompt dei Comandi. Verifica l'installazione digitando `vagrant --version`.
* **Oracle VirtualBox:** Il software di virtualizzazione su cui girerà la macchina virtuale GNS3.
    * **Scarica** [VirtualBox](https://www.virtualbox.org/wiki/Downloads) dal sito ufficiale.
    * Esegui l'installer di VirtualBox e segui le istruzioni del wizard per completare l'installazione.

* **GNS3 Graphical User Interface (GUI):** L'applicazione che userai sul tuo computer per eseguire i laboratori preconfigurati, creare topologie di rete ed mettere alla prova le  tue competenze. 
GNS3 prevede che la versione del software GUI sia la stessa in uso su GNS3 VM. Per tanto scaricheremo la versione GNS3 GUI 2.2.50 
    * **Scarica** [GNS3 GUI 2.2.50](https://github.com/GNS3/gns3-gui/releases/download/v2.2.50/GNS3-2.2.50-all-in-one.exe)
    * Segui le istruzioni di installazione per il tuo sistema operativo.
---

## 2. Configurazione del Laboratorio GNS3

Segui attentamente questi passaggi per preparare il tuo ambiente di laboratorio:

### Passaggio 1: Scarica il contenuto di questa repo da GitHub 

1.  **Apri un terminale** sul tuo computer:
    * **Windows:** Cerca "PowerShell" nel menu Start e aprilo.
    * **macOS/Linux:** Apri l'applicazione "Terminale".
2. **Scarica ed estrai i file di progetto.** 
Una volta dentro la cartella "documenti" scaricheremo l'archivio da GitHub e estrarremo la cartella di progetto contente tutti i file.

    * **Per Windows (PowerShell):**
        ```powershell
        cd "$env:USERPROFILE\Documents"
        
        Invoke-WebRequest -Uri "https://github.com/Magnetico-Associazione-Culturale/gns3-StudentLab/archive/refs/heads/main.zip" -OutFile "archive.zip"
        
        Expand-Archive -LiteralPath .\archive.zip -DestinationPath "."

        Remove-Item "archive.zip"

        cd ".\gns3-StudentLab-main\"
        ```

    * **Per macOS/Linux (Bash):**
        ```bash
        cd ~/Documents

        wget "https://github.com/Magnetico-Associazione-Culturale/gns3-StudentLab/archive/refs/heads/main.zip" -O "archive.zip"

        unzip "archive.zip" -d "."

        rm "archive.zip"

        cd gns3-StudentLab-main
        ```

### Passaggio 2: Scarica il file box vagrant pre-configurato 
1.  **Assicurati di essere ancora nella cartella del tuo laboratorio GNS3** (dove hai estratto i file) nel terminale.
    * Se hai chiuso il terminale o cambiato directory, riaprilo e naviga di nuovo a quella cartella:
        * **Per Windows (PowerShell):** `cd "$env:USERPROFILE\Documents\gns3-StudentLab-main"`
        * **Per macOS/Linux (Bash):** `cd ~/Documents/gns3-StudentLab-main`

2. **Scarica all'interno della cartella di progetto il file** `gns3-vm.box`
    * [Clicca qui](https://drive.google.com/file/d/1RwcVN6Ov3NVppj8GoKsLJzc-6TNMdVcv/view?usp=sharing) per scaricare il file `gns3-vm.box` 
    * Sposta il file scaricato nella cartella di progetto `Documents\gns3-StudentLab-main`  

### Passaggio 3: Avvia la Macchina Virtuale GNS3

1.  **Assicurati di essere ancora nella cartella del tuo laboratorio GNS3** (dove hai estratto i file) nel terminale.
    * Se hai chiuso il terminale o cambiato directory, riaprilo e naviga di nuovo a quella cartella:
        * **Per Windows (PowerShell):** `cd "$env:USERPROFILE\Documents\gns3-StudentLab-main"`
        * **Per macOS/Linux (Bash):** `cd ~/Documents/gns3-StudentLab-main`
2.  **Aggiorna il catalogo locale**
    ``` bash
    vagrant box add gns3-vm gns3-vm.box --force
    ```
3.  **Avvia la macchina virtuale GNS3** con Vagrant. Questo comando impiegherà alcuni minuti, specialmente la prima volta, poiché Vagrant importerà la VM e la configurerà:
    ```bash
    vagrant up
    ```
    * **Cosa succede:** Vagrant rileverà il file `gns3-vm.box` locale, lo aggiungerà al suo catalogo interno (se non presente), creerà una nuova macchina virtuale in VirtualBox, le assegnerà 4GB di RAM e 2 CPU, e configurerà la rete Host-Only con l'indirizzo IP **`192.168.56.101`**.
    * Durante l'avvio, potresti vedere messaggi relativi all'autenticazione SSH. Se la VM si avvia correttamente, significa che l'autenticazione ha funzionato.

### Passaggio 4: Configura la GUI di GNS3 per connettersi alla VM

Una volta che la macchina virtuale è avviata (il terminale dovrebbe tornare al prompt dopo `vagrant up` e la VM dovrebbe risultare *Running* in VirtualBox), puoi connettere la GUI di GNS3:

1. Avvia l’applicazione **GNS3 GUI** sul tuo computer (non dentro la VM).  
2. Nel menu di GNS3, vai su **Modifica (Edit) > Preferenze (Preferences)**.  
3. Nella finestra delle Preferenze, seleziona **Server > Main server** nel pannello di sinistra.  
4. **Disabilita** l’opzione “Enable local server”.  
5. Configura il **main server remoto** con i seguenti parametri:  
   - **Protocol:** HTTP  
   - **Host:** `192.168.56.101` *(oppure l’indirizzo IP assegnato alla tua VM GNS3)*  
   - **Port:** `80 TCP`  
   - **Auth:** abilitato (✓)  
   - **User:** `admin`  
   - **Password:** *non modificare questo campo. Lascia i valori di default*  
6. Clicca su **Applica (Apply)** o **OK**.  
7. Verifica che la connessione sia riuscita: dovresti vedere un **puntino verde** accanto all’indirizzo IP del server nella scheda del **Main Server**.

> 💡 **Nota:** Non abilitare i “Remote servers”. L’unico server da configurare è il *Main server*.

---

## 3. Inizia il Tuo Laboratorio! 🥳

Ora sei pronto per iniziare i tuoi esercizi di laboratorio GNS3!

* Apri il progetto di tuo interesse.
* Apri sul web la LabGuide di riferimento 

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
* **La VM non si avvia / Errore di Virtualizzazione:** **Rivedi i "Prerequisiti " per verificare che la Tecnologia di Virtualizzazione sia abilitata.** Se è disabilitata, dovrai abilitarla nel BIOS/UEFI del tuo computer.
* **La GUI di GNS3 non si connette alla VM:**
    * Verifica che la VM sia in esecuzione (`vagrant status` dovrebbe mostrare `running`).
    * Assicurati di aver inserito l'indirizzo IP corretto (`192.168.56.101`) e la porta (`3080`) nelle preferenze della GUI di GNS3.
    * Apri VirtualBox Manager: vai su `File` > `Gestore reti host...` (Host Network Manager...). Assicurati che esista un "VirtualBox Host-Only Ethernet Adapter" (es. `vboxnet0`) e che sia attivo.
* **Spazio su Disco:** I file delle VM e delle immagini possono essere grandi. Assicurati di avere abbastanza spazio libero sul disco prima di scaricare il package del laboratorio.
