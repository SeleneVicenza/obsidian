È una **libreria JavaScript/TypeScript per fare richieste HTTP** (GET, POST, PUT, DELETE, …) da **Node.js** o dal **browser**.  
Si basa su **Promise**, (lavora con `async/await`).

### Vantaggi principali rispetto alle API native (`http` o `fetch`)

1. **Più semplice**:
    - `axios.get(url, { params })` → costruisce da solo la query string.
    - `axios.post(url, data)` → invia JSON automaticamente con header corretti.


2. **JSON automatico**:
    - Converte la risposta in JSON (`res.data`) senza dover chiamare `res.json()`.


3. **Gestione errori uniforme**:
    - Distingue chiaramente tra errore HTTP (4xx/5xx), assenza di risposta e problemi di rete.


4. **Supporto timeout nativo**:
    - Evita richieste bloccate per sempre (`timeout: 5000`).


5. **Interceptors**:
    - Puoi agganciare funzioni per aggiungere token di autenticazione o loggare tutte le richieste/risposte.


6. **Compatibilità Node + Browser**:
    - Stessa API sia lato client che lato server.


7. **Istanza configurabile**:
    - Puoi creare un “client” preconfigurato con `baseURL`, header comuni e usarlo ovunque.




 **Axios rende le chiamate HTTP più semplici, sicure e consistenti**, **gestendo in automatico** molti dettagli che con `fetch` o `http` dovresti fare a mano.

| Caratteristica                      | **Axios**                                                                | **fetch** (Node 18+)                     | **http/https** (core)              |
| ----------------------------------- | ------------------------------------------------------------------------ | ---------------------------------------- | ---------------------------------- |
| Stile API                           | Promise (`async/await`)                                                  | Promise (`async/await`)                  | Callback/stream (promisificabile)  |
| Ambiente                            | Browser + Node                                                           | Browser + Node                           | Solo Node                          |
| Query string (`params`)             | ✅ automatico (`params`)                                                  | ⚠️ manuale (`URLSearchParams`)           | ⚠️ manuale                         |
| Body JSON in POST                   | ✅ automatico (`Content-Type` e stringify)                                | ⚠️ manuale: `headers` + `JSON.stringify` | ⚠️ manuale, stream                 |
| Parsing JSON                        | ✅ `res.data`                                                             | ⚠️ `await res.json()`                    | ⚠️ accumulare chunk + `JSON.parse` |
| Gestione errori                     | ✅ unificata (`error.response`, `error.request`)                          | ⚠️ `res.ok` da controllare + try/catch   | ⚠️ errori rete + status da gestire |
| Timeout                             | ✅ nativo (`timeout`)                                                     | ⚠️ via `AbortController`                 | ⚠️ via socket/req.setTimeout       |
| Interceptors                        | ✅ request/response                                                       | ❌                                        | ❌                                  |
| Retry                               | ➕ via plugin (`axios-retry`)                                             | ➕ manuale o lib esterne                  | ➕ manuale                          |
| Upload/Download file                | ✅ semplice                                                               | ✅ buono                                  | ✅ massimo controllo (stream)       |
| Progress (upload/download)          | ✅ `onUploadProgress`/`onDownloadProgress` (browser; in Node con adapter) | ⚠️ non standard in Node                  | ✅ gestibile a mano (stream)        |
| Redirect                            | ✅ gestiti                                                                | ✅ gestiti                                | ⚠️ manuale o opzioni               |
| Cookies (browser)                   | ✅ facile                                                                 | ⚠️ policy CORS                           | ❌ N/A                              |
| CORS (browser)                      | – lato server                                                            | – lato server                            | – lato server                      |
| Tipizzazione TS                     | ✅ ottima (generics)                                                      | ✅ base                                   | ⚠️ bassa, manuale                  |
| Controllo `Agent`/Keep-Alive (Node) | ⚠️ via adapter                                                           | ⚠️ limitato                              | ✅ completo (`http.Agent`)          |
| Dipendenze                          | 📦 sì                                                                    | 📦 no                                    | 📦 no                              |
| Performance pura                    | Ottima per DX                                                            | Ottima                                   | Massima (a basso livello)          |
| Complessità codice                  | Bassa                                                                    | Media                                    | Alta                               |
