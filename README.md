# ⚡ Voltage Glitching (Fault Injection) - Knowledge Base

Questo repository contiene una guida tecnica e risorse sull'iniezione di guasti da tensione (**Voltage Glitching**), una tecnica di hardware hacking utilizzata per eludere le protezioni di sicurezza nei sistemi embedded.

## 📌 Introduzione
Il **Voltage Glitching** consiste nel manipolare l'alimentazione di un microcontrollore (MCU) o di un processore in un intervallo di micro o nanosecondi. L'obiettivo è provocare un errore nella logica digitale che consenta di saltare istruzioni (`JNE`, `JZ`, ecc.) o corrompere i dati in transito.

Il Voltage Glitching (o iniezione di guasti da tensione) è una tecnica di hardware hacking che consiste nel manipolare deliberatamente l'alimentazione di un dispositivo per provocarne malfunzionamenti. Applicando variazioni di tensione estremamente brevi e precise (glitches), è possibile ingannare il processore affinché salti istruzioni critiche, come controlli di password o verifiche di firma digitale, consentendo l'accesso a sistemi altrimenti bloccati.

## 🛠️ Strumenti Comuni
*   **Hardware:**
    *   [ChipWhisperer](https://newae.com) (Standard del settore).
    *   Raspberry Pi Pico (con firmware [PicoGlitcher](https://github.com)).
    *   Transistor MOSFET (per il cortocircuito controllato a massa).
    *   Oscilloscopio (per visualizzare la precisione del glitch).
*   **Software:**
    *   Python (per automatizzare il timing degli attacchi).
    *   Jupyter Notebooks (analisi dei dati dei guasti).

## 🚀 Metodologia dell'Attacco
1.  **Identificazione:** Individuare il pin `VCC` del processore e i condensatori di disaccoppiamento.
2.  **Preparazione:** Rimuovere condensatori che potrebbero ammorbidire il "glitch" (opzionale ma raccomandato).
3.  **Sincronizzazione (Trigger):** Identificare un evento (come il fronte di salita di un pin UART o un consumo energetico specifico) per attivare il glitch.
4.  **Brute-forcing:** Provare variazioni di:
    *   `Offset`: Tempo dall'attivazione al glitch.
    *   `Width`: Durata dell'impulso di bassa tensione.
5.  **Sfruttamento:** Confermare se il sistema ha saltato la verifica di sicurezza (es. "Login Success").

L'obiettivo è creare un'instabilità temporanea nei circuiti logici del chip.

*   **Istruzioni saltate:** Se il glitch si verifica proprio quando la CPU decide se una password è corretta, si può indurre il sistema a "saltare" alla sezione di codice di successo indipendentemente dal valore inserito.
*   **Bypass della protezione in lettura:** Viene spesso utilizzato per disattivare il bit di protezione in lettura (ROP) nei microcontrollori, consentendo di estrarre l'intero firmware.
*   **Corruzione dei dati:** Può causare la modifica dei dati letti dalla memoria, utile per attaccare algoritmi crittografici e filtrare chiavi segrete.

## 📂 Scenari di Utilizzo
- [x] **Bypass dei Checksum:** Saltare la verifica della firma del firmware.
- [x] **Dump della Memoria:** Disattivare i bit di protezione in lettura (Read-out Protection).
- [x] **Escalation dei Privilegi:** Forzare stati di amministratore su console o dispositivi IoT.

## 🛡️ Contromisure
*   **Rilevamento di Brown-out:** Circuiti che resettano il chip in caso di cadute di tensione insolite.
*   **Logica ridondante:** Eseguire controlli critici due volte in momenti diversi per garantire che un singolo glitch non influenzi entrambi.
*   **Ritardi casuali:** Aggiungere codice "spazzatura" o attese casuali (jitter) in modo che l'attaccante non possa prevedere il momento esatto in cui viene eseguita l'istruzione sensibile.

## ⚠️ Disclaimer
Questo materiale è a scopo esclusivamente educativo. L'uso di queste tecniche su dispositivi di terze parti senza autorizzazione è illegale.

---
⭐ *Realizzato per la comunità di Hardware Hacking.*

***

### Cos'è un attacco di Fault Injection in Cybersecurity e perché è importante?
Pubblicato: 6 giugno 2025

Scritto da: Brenda Buckman

**Effetto del Guasto**
Se ti chiedessimo di nominare alcune strategie di cyberattacco, probabilmente penseresti alle email di phishing o al ransomware. Ma hai sentito parlare di glitching?

Il glitching, noto anche come iniezione di guasti, è un sofisticato attacco basato su hardware che manipola le condizioni operative di un dispositivo, causando scompiglio nei sistemi sicuri. Ti interessa? Analizziamo in dettaglio cosa sono gli attacchi di glitching, come funzionano e perché sono importanti.

**Cos'è il Glitching?**
In poche parole, il glitching consiste nell'alterare selettivamente l'hardware di un dispositivo. Facendo ciò, gli attaccanti costringono il dispositivo a comportarsi in modo imprevisto, il che potrebbe aggirare le sue misure di sicurezza. L'obiettivo? Ottenere accesso non autorizzato, estrarre dati sensibili o modificare il comportamento del dispositivo.

Ecco i dettagli tecnici:

*   **Metodi di manipolazione dei guasti:** Implica la manipolazione intenzionale della tensione, dei segnali di clock o persino delle condizioni ambientali come la temperatura.
*   **Fase finale:** Gli attaccanti mirano ad aggirare controlli come la verifica delle password, estrarre chiavi di crittografia o manipolare il firmware.
*   **Differenza dalle vulnerabilità software:** Le vulnerabilità software prendono di mira difetti nel codice, mentre il glitching manipola le operazioni fisiche di un dispositivo.

Pensa al "glitching" come toccare il punto esatto di un dispositivo al momento giusto, facendolo funzionare male in un modo che avvantaggia l'attaccante.

**Tipi di Attacchi di Fault Injection**

Non tutti i guasti sono uguali! Ecco quattro metodi comuni utilizzati dagli hacker per sfruttare questi guasti:

1.  **Guasti di Tensione**
    L'attaccante manipola l'alimentazione del dispositivo, causando brevi fluttuazioni di tensione. Ciò può causare la corruzione dei dati o indurre il dispositivo a saltare istruzioni di sicurezza critiche.
    *   Complessità: Bassa
    *   Obiettivi tipici: Dispositivi IoT, microcontrollori

2.  **Guasti di Clock**
    Iniettando guasti di temporizzazione nei segnali di clock di un dispositivo, questo metodo altera il modo in cui i dati vengono elaborati, spesso aggirando i passaggi di autenticazione.
    *   Complessità: Media
    *   Obiettivi tipici: Sistemi embedded come console per videogiochi.

3.  **Guasti Elettromagnetici (EM)**
    In questo caso, vengono utilizzati impulsi elettromagnetici per interferire con il funzionamento di un chip. La precisione è fondamentale per questo attacco, ma è abbastanza potente da violare ambienti sicuri.
    *   Complessità: Alta
    *   Obiettivi tipici: Smart card, moduli crittografici sicuri

4.  **Guasti Termici**
    Il surriscaldamento o il raffreddamento eccessivo di un dispositivo al di fuori del suo intervallo operativo può causare comportamenti imprevisti, rendendolo vulnerabile a guasti di sicurezza.
    *   Complessità: Bassa
    *   Obiettivi tipici: Dispositivi embedded di consumo

| Metodo di Attacco       | Difficoltà | Obiettivi Tipici                          |
| :---------------------- | :--------- | :---------------------------------------- |
| Guasto di Tensione      | Basso      | IoT, microcontrollori                     |
| Guasto di Clock         | Medio      | Console per videogiochi, sistemi embedded |
| Guasti Elettromagnetici | Alto       | Smart card, chip sicuri                   |
| Guasti Termici          | Basso      | Dispositivi di uso domestico              |

**Come Funzionano i Guasti Tecnici**
Gli attacchi di fault injection richiedono precisione e strumenti specializzati per avere successo. Solitamente prendono di mira processori sicuri o sistemi embedded durante operazioni delicate come:

*   Processi di avvio
*   Validazioni crittografiche
*   Autenticazione password

Ecco come si sviluppa solitamente:

1.  Un attaccante identifica dove vengono eseguite operazioni sensibili (ad es. crittografia di chiavi, caricamento di memoria).
2.  Utilizzando strumenti come oscilloscopi o iniettori di tensione, provocano interruzioni programmate con precisione.
3.  Se l'operazione ha successo, il dispositivo salta o esegue in modo errato le istruzioni, consentendo all'attaccante di aggirare i controlli di sicurezza o estrarre dati sensibili.

**Esempio:** Durante un processo di avvio sicuro, un guasto tecnico potrebbe far saltare al dispositivo la verifica della firma, avviando così un software non attendibile.

Ti senti confuso? Non preoccuparti; tutto si riduce a una cosa: il tempismo è tutto nel glitching. L'attaccante deve provare, regolare e iterare... molto.

**Strumenti Utilizzati dagli Hacker Hardware**
Per sfruttare i guasti informatici è necessaria più di una mentalità criminale. Gli hacker necessitano di:

*   **Strumenti del mestiere:** Dispositivi come ChipWhisperer e GlitchKit.
*   **Apparecchiature di test:** Oscilloscopi, generatori di segnale e iniettori di tensione personalizzati.
*   **Punti di accesso:** Interfacce di debug (ad es. UART, JTAG) per monitoraggio e iniezione.

Questo non è il tipico attacco informatico che un hacker solitario può eseguire da un seminterrato. L'uso di guasti tecnici è deliberato, richiede competenze tecniche ed è spesso costoso.

**Esempi Reali di Guasti Tecnici**
Se pensi che i guasti tecnici siano rari, ripensaci. Ecco alcuni esempi notevoli di attacchi reali:

*   **Console per videogiochi:** La tecnica di sfruttare i guasti tecnici è stata ampiamente utilizzata per il "modding" di PlayStation e per aggirare le protezioni DRM.
*   **Smartphone:** Gli attaccanti sono riusciti ad aggirare le misure di sicurezza, esponendo dati crittografati.
*   **Dispositivi IoT:** I ricercatori hanno estratto il firmware da dispositivi IoT come telecamere intelligenti e router utilizzando la manipolazione della tensione.
*   **Smart card:** Moduli crittografici di carte bancarie sono stati vittime di guasti tecnici, causando la fuga di chiavi riservate.

Ogni esempio sottolinea la crescente importanza della sicurezza hardware nel mondo connesso di oggi.

**Guasti nell'Offensive Security**
L'uso di guasti tecnici non viene impiegato solo per scopi malevoli. Svolge anche un ruolo cruciale nei test di penetrazione legittimi e nelle simulazioni di attacco (red teaming). I professionisti della sicurezza offensiva ricorrono ai guasti tecnici per:

*   Testare e migliorare i meccanismi di avvio sicuro.
*   Effettuare reverse engineering di componenti di sistemi embedded.
*   Valutare le vulnerabilità in dispositivi IoT o industriali.

Per i ricercatori di sicurezza, i guasti tecnici sono più di un vettore di attacco; sono uno strumento per salvaguardare tecnologie critiche.

**Come Difendersi dagli Attacchi di Fault Injection**

Fortunatamente, esistono modi efficaci per respingere gli attacchi di fault injection. Ecco alcune strategie difensive chiave:

*   **Monitor di tensione e clock:** Utilizzare metodi basati su hardware, come il rilevamento di cali di tensione, per rilevare interruzioni dannose.
*   **Progettazione a prova di manomissione:** I chip vengono incapsulati in imballaggi a prova di manomissione con sensori a rete integrati.
*   **Cicli di avvio sicuri:** Ricontrollare le validazioni crittografiche più volte durante il processo di avvio.
*   **Pattern casuali:** Aggiungere ritardi o timing casuali per rendere difficile l'esecuzione di fault injection.
*   **Standard di conformità:** Seguire certificazioni di sicurezza come FIPS 140-3 per la progettazione di elementi sicuri.

Quanto più diversificato e imprevedibile è il funzionamento di un dispositivo, tanto più difficile sarà per gli attaccanti iniettare con successo guasti di sicurezza.

**Perché i Guasti Tecnici Sono Importanti**
Gli attacchi basati su guasti tecnici non sono solo teorici. Hanno implicazioni reali per settori che vanno dall'Internet delle Cose alla sanità e alla finanza.

Ecco perché il fault injection è fondamentale:

*   **Rivela vulnerabilità nascoste:** Il fault injection aggira anche le difese software più robuste attaccando direttamente l'hardware.
*   **Sfida per l'infrastruttura critica:** Immagina se i guasti tecnici venissero utilizzati contro sistemi di controllo industriale o dispositivi medici.
*   **La cybersecurity continua a evolversi:** I guasti tecnici evidenziano la necessità di incorporare difese consapevoli dell'hardware nei framework di cybersecurity.

Con la crescente onnipresenza dell'IoT e dei dispositivi embedded, la necessità di sistemi resilienti ai guasti non è mai stata maggiore.

| Tipo di Attacco          | Focus                                     | Esempio Chiave                                         |
| :----------------------- | :---------------------------------------- | :----------------------------------------------------- |
| Guasti                   | Manipolazione precisa delle condizioni    | Attacchi basati su tensione o clock                    |
| Attacchi a Canale Laterale | Osservazione di informazioni indirette    | Analisi di potenza/elettromagnetica                    |
| Hammering (Martello)     | Iniezione di guasti tramite righe di RAM  | Inversione di bit tramite accesso ripetuto             |
| Cold Boot Attacks        | Furto di dati dalla RAM dopo il riavvio | Estrazione di chiavi di crittografia dalla DRAM        |

Ognuno ha i propri strumenti e obiettivi, con diversi livelli di difficoltà e applicazione.

**Considerazioni Legali ed Etiche**
Prima di rispolverare il tuo ChipWhisperer, tieni presente che manipolare guasti di sistema senza permesso è illegale. Tuttavia:

*   **Uso Etico:** L'uso di guasti tecnici svolge un ruolo vitale nella ricerca accademica sulla sicurezza, nelle competizioni e nei test di penetrazione (con permesso).
*   **Divulgazione Regolata:** I guasti scoperti dovrebbero essere divulgati in modo responsabile ai produttori interessati.

Tenendo conto dell'etica e dell'intenzione, i guasti tecnici possono guidare l'innovazione anziché interromperla.

**Sviluppare Resilienza contro i Guasti Tecnici**
I guasti tecnici non scompariranno. Anzi, la loro rilevanza non fa che aumentare man mano che i dispositivi IoT, i sistemi automobilistici e i controlli industriali dipendono sempre più dalla sicurezza a livello hardware.

Per rimanere al sicuro:

*   Valuta la tua tecnologia alla ricerca di vulnerabilità a livello hardware.
*   Rimani informato sui nuovi vettori di attacco, come il glitching.
*   Investi in solide misure di protezione per i sistemi critici.
