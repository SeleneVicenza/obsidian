In un’applicazione web, un **middleware** è una **funzione che riceve la richiesta (`req`) e la risposta (`res`) prima che la richiesta arrivi all’handler finale**.

Serve per:
- eseguire **logica comune** (es. loggare, validare, autenticare, trasformare dati);
- **modificare `req` o `res`**;
- **decidere se proseguire** verso la prossima funzione oppure terminare la richiesta.

# Flusso di una richiesta

1. **Il client invia la richiesta**
    - Può essere un browser, un’app mobile, un altro servizio.
    - Esempio: `GET /users?id=5`

2. **Il server riceve la richiesta**
    - Node riceve i dati via **socket TCP**.
    - Il modulo `http` (o il framework sopra di esso) crea un oggetto `req` e `res`.

3. **Middleware globali**
    - Ogni `app.use(...)` viene eseguito in ordine.
    - Esempio: logging (`morgan`), parsing (`express.json()`), CORS, sicurezza (`helmet`).

4. **Middleware specifici / di rotta**
    - Se la richiesta corrisponde a una rotta (`/users`), Express esegue eventuali middleware definiti su quella rotta. 
    - Esempio: autenticazione, validazione parametri.

5. **Route handler (controller)**
    - La funzione finale che esegue la logica di business: legge dal DB, chiama API esterne, elabora dati.
    - Genera la **risposta** (JSON, HTML, file, ecc.).

6. **Middleware di errore (se serve)**
    - Se durante l’esecuzione qualcosa genera un errore, viene intercettato dal middleware con firma `(err, req, res, next)`.    
    - Restituisce tipicamente `500 Internal Server Error` o un messaggio personalizzato.

7. **Il server invia la risposta**
    - Viene scritto lo **status code** (`200`, `404`, `500` …).  
    - Vengono inviati gli **header** (`Content-Type`, `Content-Length`, ecc.).    
    - Viene inviato il **body** (HTML, JSON, testo, file).
 
8. **Connessione chiusa o riutilizzata**
    - Con **HTTP/1.1 Keep-Alive**, la connessione può rimanere aperta per ulteriori richieste.
    - Con **HTTP/2**, si multiplexano più richieste sulla stessa connessione.
