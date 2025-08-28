
[[MIDDLEWARE]] per la gestione della [[Sessione utente]] *lato server*. 


Di default `express-session` usa la **MemoryStore** → non adatta alla produzione.  

**In produzione**:
- **Redis**: `connect-redis`
- **MongoDB**: `connect-mongo`
- **MySQL/PostgreSQL**: `express-mysql-session` / `connect-pg-simple`

---
### Installazione

``` bash
npm install express-session`
```

---
### Come funziona

1. Quando un client si connette → il [[middleware]] assegna un **ID di sessione univoco**.
2. L’ID viene salvato in un **cookie sul browser** (di default `connect.sid`).
3. I dati della sessione sono salvati **lato server** (in memoria o su uno store).
4. Ad ogni richiesta successiva, il browser invia il cookie → Express recupera i dati della sessione.

---
### Opzioni importanti

- `secret`: stringa o array di stringhe per firmare i cookie.
- `cookie`: configurazione cookie (es. `maxAge`, `secure`, `httpOnly`).
- `store`: backend per salvare le sessioni (default = memoria).

---

