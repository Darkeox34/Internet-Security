# Lezione 10/03/2026

## Trojan

### Definizione di attacco
Una **definizione astratta di attacco** è:  
> Qualunque violazione delle regole di un sistema.

Le regole di un sistema sono definite dalla **policy**.

### Funzionamento di un Trojan
Un **Trojan** è un file che contiene due linee di codice di shell (comandi per l'interprete dei comandi di Linux).

Queste due linee di codice copiano la shell in una cartella temporanea, in modo che la shell acquisisca permessi più permissivi, tipicamente quelli della cartella `/tmp`.

I permessi del file copiato dipendono dalla **policy del comando di copia** (ad esempio il comando Linux `cp`). Anche l'operazione di copia ha quindi una policy che gestisce i permessi del file copiato, e questa policy può essere sfruttata da un attaccante.

Il Trojan successivamente modifica i permessi della copia, operazione possibile perché consentita dai permessi acquisiti durante la copia. In particolare, viene impostato il **bit SUID (`s`) sull'owner** del file.

Questo significa che:
- chiunque esegua il file
- lo eseguirà **con i privilegi del proprietario del file**.

---

### Organizzazione delle cartelle eseguibili

In passato molti file eseguibili si trovavano nella **cartella home** dell'utente.

Oggi, per motivi di sicurezza, i sistemi operativi utilizzano **più cartelle separate**, non solo per comodità ma anche per isolare ambienti sicuri da quelli meno sicuri.

Ad esempio in Linux:

- I file scaricati nella cartella `Downloads` **non hanno permessi di esecuzione**.
- I programmi installati si trovano tipicamente in `/usr/bin`.
- Le directory dei programmi sono separate dalle cartelle utente.

I programmi vengono trovati tramite la variabile di sistema **`$PATH`**, che contiene una lista di directory dove il sistema cerca gli eseguibili.

---

### Attacco tramite `$PATH`

Il `$PATH` è una lista ordinata di directory che indica **in quale ordine cercare i programmi da eseguire**.

Un possibile attacco consiste nel:

1. Inserire **una directory controllata dall'attaccante** all'inizio del `$PATH`.
2. Inserire in quella directory programmi con nomi comuni come:
   - `ls`
   - `echo`
3. Quando l'utente esegue `ls` o `echo`, verrà eseguito **il programma malevolo** invece di quello presente in `/usr/bin`.

Una misura di sicurezza comune è:

- **rimuovere `.` dal `$PATH`**, per evitare l'esecuzione accidentale di programmi nella directory corrente.

---

# Strumenti generali di sicurezza

Cosa possiamo fare per proteggerci?

Esistono diversi strumenti:

### 1. Cifratura / crittografia
Protezione dei dati tramite algoritmi matematici.

### 2. Policy
Insieme di regole che definiscono cosa è consentito e cosa non lo è.

⚠️ Una policy **non è un meccanismo di enforcement**, ma una definizione delle regole.

### 3. Autenticazione
Metodi per identificare l'utente:

- password
- PIN
- smart card
- smart token
- impronta digitale
- riconoscimento dell'iride

**Obiettivo dell'autenticazione:**
Possibile **domanda d'esame**.
> Individuare l'utente nel modo più univoco possibile.

⚠️ Non esiste un **metodo di autenticazione migliore in assoluto**: dipende sempre dal contesto e dalle necessità.

---

### 4. Programmi di protezione

Esempi:

- Antivirus
- IDS (Intrusion Detection System)
- Firewall

I prodotti commerciali di sicurezza contengono **insiemi di regole generali**, ma non sono specializzati per ogni singolo contesto.

---

### 5. Protocolli di sicurezza

Esempi:

- SSH
- SSL
- TLS

(Il professore non tratta più SSH da alcuni anni, ma **SSL e TLS sono nel programma**.)

---

### 6. Sensibilizzazione dell'utente

Formazione degli utenti sui comportamenti sicuri.

---

### Osservazione importante

Nessuno di questi strumenti è perfetto.

> **La sicurezza assoluta non esiste.**

Se esistesse, non esisterebbero attacchi informatici.

Ogni misura di sicurezza ha **limiti intrinseci**.

---

# Limiti della crittografia

Gli strumenti crittografici sono basati sulla matematica.

Alcuni sistemi crittografici sono più attaccabili di altri, mentre altri sono **teoricamente molto sicuri**.

Esempio: **RSA 2048**

L'algoritmo RSA è considerato sicuro perché:
- non è violabile in **tempo polinomiale** con le conoscenze attuali.

Tuttavia possono esistere vulnerabilità **software**, ad esempio:

- il **PRNG (Pseudo Random Number Generator)** usato per generare le chiavi potrebbe essere prevedibile.

Se il PRNG è debole:
- l'attaccante potrebbe **prevedere la chiave privata**, rendendo inutile la sicurezza teorica dell'algoritmo.

---

# Limiti di un firewall

### Insider threat

Un **insider** è qualcuno che attacca **dall'interno della rete**.

Se l'attacco proviene dall'interno:
- il firewall perimetrale può essere inefficace.

---

### Segmentazione della rete

Una soluzione è la **segmentazione della rete**, ad esempio tramite:

- VLAN
- firewall interni

Esempio universitario:

- rete WiFi studenti
- rete docenti

Queste reti sono separate.

Il firewall perimetrale **non basta**, quindi si usano **più livelli di firewall**.

---

# Definizione di sicurezza (per punti)

### 1. La sicurezza è un processo

> "Security is not a product, but a process."

La sicurezza è un processo continuo che include:

- aggiornamenti di sistema
- installazione di strumenti più moderni
- formazione del personale
- test di sicurezza
- valutazione dei rischi
- piani di continuità operativa
- gestione dei disastri

---

### 2. Catena dell'anello più debole

Un sistema è sicuro **quanto il suo componente più debole**.

Anche se molti componenti sono sicuri, un singolo punto debole può compromettere l'intero sistema.

---

### 3. La sicurezza non è assoluta

Bisogna sempre specificare:

> Sicuro **da cosa**?

Un sistema può essere:

- sicuro contro uno studente inesperto
- ma non contro un **penetration tester esperto**.
- sicuro contro un attacco XSS
- ma non contro un attacco SQL Injection

---

### 4. Analisi costi/benefici dell'attaccante

Un attacco dipende anche da:

- costo dell'attacco
- beneficio ottenibile

Se il costo è troppo alto, l'attacco può non essere conveniente.

---

### 5. Livelli di sicurezza

La sicurezza si implementa tramite **più livelli di difesa**.

Questo concetto è noto come **defense in depth**.

---

# Rischi di base per la sicurezza

La **valutazione del rischio** è il principale motore per decidere le misure di sicurezza.

---

## 1. Complessità del sistema

Un **browser web** è uno dei software più complessi esistenti.

Un browser include:

- motori JavaScript
- rendering HTML
- CSS
- multimedia
- accesso al filesystem
- gestione della posta
- networking

Questa complessità rende il browser **molto difficile da mettere in sicurezza**.

---

## 2. Combinazione di sistemi

I sistemi informatici sono **sistemi composti**.

Due sistemi sicuri separatamente **non sono necessariamente sicuri quando combinati**.

Esempio:

- sistema operativo
- repository degli aggiornamenti

Se combinati male, l'OS potrebbe permettere di **cambiare il repository degli aggiornamenti**, introducendo vulnerabilità.

---

## 3. Presenza inevitabile di bug

Statisticamente esistono **n bug ogni m linee di codice**.

Un bug è una **proprietà inattesa del software**.

Un bug può diventare:

- una vulnerabilità
- sfruttabile da un attaccante.

---

## 4. Proprietà emergenti

Quando si introducono **nuove funzionalità**, emergono nuove proprietà del sistema.

Queste possono generare **nuove vulnerabilità**, perché non sono ancora state studiate a fondo.

---

## 5. Interazione con l'uomo

Citazione famosa:

> "The human is the weakest link in the chain."

L'essere umano è storicamente considerato **l'anello debole della sicurezza**.

Le macchine possono essere programmate per comportarsi correttamente, mentre gli esseri umani:

- possono commettere errori
- possono essere manipolati (social engineering)

La sfida della tecnologia è progettare sistemi che:

- siano **sicuri**
- ma anche **usabili**, riducendo la probabilità di errore umano.

---

# Rischi digitali per la sicurezza

## 1. Automazione offensiva

Nel mondo digitale gli attacchi possono essere **automatizzati**.

Caratteristiche:

- le vulnerabilità sono **replicabili**
- un attacco può essere eseguito **su larga scala**

Esempio:

Se un dispositivo ha una vulnerabilità, è possibile attaccare **milioni di dispositivi identici** con lo stesso exploit.

---

### Attacchi difficili da tracciare

Gli attaccanti possono nascondersi dietro:

- proxy
- VPN
- botnet

Questo rende gli attacchi **difficili da attribuire**.

---

### Privacy dei dati

La digitalizzazione comporta che i nostri dati siano memorizzati in:

- numerosi database
- servizi online

Questo aumenta i **rischi per la privacy**.

---

## 2. Assenza di distanza

Nel mondo digitale **la distanza non esiste**.

Un attacco può essere lanciato **da qualsiasi parte del mondo**.

Questo implica che:

- chiunque può potenzialmente attaccare un sistema
- non esistono confini geografici per gli attacchi informatici.

---

### Problemi giuridici e Privacy Shield

In Europa esiste il **GDPR**, una delle normative più avanzate sulla protezione dei dati.

Tuttavia molti servizi digitali sono gestiti da aziende americane, ad esempio:

- Google
- Facebook
- Amazon

Si crea quindi un problema di **giurisdizione**.

Per gestire questa situazione è stato introdotto il **Privacy Shield**, un accordo legale per permettere il trasferimento dei dati tra:

- Unione Europea
- Stati Uniti

Questo problema è legato alla **sovranità digitale**.

Molti paesi, inclusa l'Italia, **non possono autosostenersi con soli servizi digitali nazionali**.

Utilizziamo quindi servizi internazionali come:

- Office
- Dropbox
- FileZilla

Questo rende complessa l'applicazione delle leggi nazionali sulla protezione dei dati.

La questione della sovranità digitale **rimane tuttora aperta**.