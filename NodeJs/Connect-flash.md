**[[Middleware]] per Express** (basato su `connect`) che permette di gestire messaggi temporanei (flash messages) salvati in **sessione** e resi disponibili **solo per la richiesta successiva**.

- notifiche di **successo/errore** (es. dopo login, registrazione, salvataggio form),
- messaggi di feedback per l’utente,
- comunicazioni “usa e getta” tra redirect consecutivi.

==Richiede **sessioni attive**, quindi va usato insieme a [[Express-session]].==

---
### Installazione

``` bash
npm install connect-flash
```


---
### API essenziali

- `req.flash(type, message)` → aggiunge un messaggio flash di un certo tipo.
- `req.flash(type)` → recupera e rimuove i messaggi di quel tipo.
- Restituisce sempre un **array** (può contenere più messaggi).


---
### Vantaggi

- Semplice e leggero.
- Integrato con session → non richiede DB o storage separato.
- Ottimo per UX con redirect (es. dopo POST → Redirect → GET).


---

### Attenzioni operative

- I messaggi sono **transitori**: disponibili solo per la richiesta successiva.
- Non adatto per **log persistenti** o notifiche complesse.

---
