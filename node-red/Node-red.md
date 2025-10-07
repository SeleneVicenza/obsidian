Piattaforma low-code basata su Node.js per orchestrare integrazioni tramite **nodi** collegati in **flussi** (IoT, API, automazioni).

**==Flow-based programming==**: **collegamenti tra blocchi** (nodi) con dei **fili** (wires). Ogni blocco fa una cosa **molto specifica** (es. leggere da MQTT, trasformare, scrivere su HTTP).

## Il “linguaggio” dei flussi: `msg`

- I nodi si passano **un oggetto JavaScript** chiamato `msg`.
- **Convention**: il contenuto principale viaggia in `msg.payload`. Ma puoi usare anche altre proprietà (`msg.topic`, `msg.headers`, `msg.error`, ecc.).

- Il **Function node** è dove scrivi **JS puro** per aggiustare il messaggio:
    
``` js
    // Esempio const x = Number(msg.payload); 
    msg.payload = { value: x, ts: Date.now() }; msg.topic = "scanner/value";
    return msg;    
```
    

> Mentalità: immagina `msg` come una **busta** che passa di mano in mano. Ogni nodo può leggere/scrivere campi nella busta.


## 3) Tipi di nodi e loro ruolo

- **==Input==**: generano o ricevono messaggi (Inject, HTTP In, MQTT In, OPC UA Client, Modbus Read, File In).
- **==Processing==**: trasformano/decidono (Function, Change, Switch, Template, JSON, CSV, Delay).
- **==Output==**: inviano fuori (Debug, HTTP Response, MQTT Out, File Out, DB, Email).
- **==Utility/Strutturali==**: Subflow (riuso), Link (collegamenti fra tab), Catch/Status (errori e stati), Complete (fine flusso).

> Regola d’oro: **separa** ingressi, trasformazioni e uscite. I flussi restano leggibili e testabili.


## 4) Stato e “memoria” del flusso: `context`

- **msg**: dura solo per quel messaggio.
- **flow**: variabili condivise **nella tab** (pagina) corrente.
- **global**: variabili condivise **nell’intero runtime**.
- Possono essere **volatili** (memoria) o **persistenti** (file/DB) a seconda della configurazione.
    
    `// Nel Function node let count = flow.get("count") || 0; flow.set("count", ++count);      // memoria (default) // Variante persistente (se abilitata): flow.set("count", count, "file");`
    

> Quando serve memoria? Per contatori, cache, stato di un polling. **Non** per segreti (quelli vanno in variabili d’ambiente o credenziali cifrate).

---

## 5) Deploy: perché a volte “non succede niente”

- **Full**: ridispiega tutto.
- **Modified Flows**: solo i flussi modificati.
- **Modified Nodes**: solo i nodi modificati.  
    Se un nodo non si riavvia, può dipendere dalla modalità di deploy scelta.

> Abitudine sana: in sviluppo usa “Modified Flows”; in investigazione di bug complessi, prova “Full”.

---

## 6) Variabili d’ambiente (niente hard-code)

- Per endpoint, IP, credenziali usa **ENV**, non valori fissi nei nodi.
- Nei campi dei nodi (quando supportato) usa `$(NOME_VAR)`. Nel Function node: `process.env.NOME_VAR`.

> Così sposti il flusso da **test → staging → produzione** senza toccare i nodi.

---

## 7) Errori e debug: come ragiona un flusso in errore

- **Debug node**: vedi `payload` o **l’intero `msg`** (consigliato quando impari).
- **Catch**: intercetta errori lanciati dai nodi a monte → puoi loggare, ritentare, notificare.
- **Status**: mostra lo stato corrente di un nodo (connesso/disconnesso, errori, ecc.).
- **Delay/Trigger**: controllano ritmo (throttling, debounce) e timeout.

> Tattica: crea una tab “**TestBench**” con Inject preconfigurati (casi nominali, edge case, errori simulati). È il tuo “laboratorio”.

---

## 8) Subflow e Link: riuso e decoupling

- **Subflow**: un “mini-flow riutilizzabile” (puoi parametrizzarlo). Ideale per validazioni, mapping ripetuti, logger uniformi.
- **Link In/Out**: collegano punti distanti o tab diverse senza fili lunghi. Li usi come un **bus interno**.

> Disegna per **moduli**: _Ingress_ (sorgenti), _Map_ (normalizzazioni), _Service_ (logica), _Egress_ (uscite). Più facile da mantenere.

---

## 9) Sicurezza (basi corrette)

- **adminAuth**: proteggi **editor** e API amministrative con utenti/password (hash bcrypt).
- **credentialSecret**: cifra le credenziali salvate.
- **HTTPS/Reverse proxy**: metti l’editor dietro TLS; opzionale Basic Auth a monte.
- **Niente segreti nei flow**: usa ENV o secret manager.

> Non è burocrazia: evita che qualcuno apra l’editor in rete e “giochi” coi tuoi impianti.