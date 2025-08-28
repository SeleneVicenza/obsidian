File versionato che descrive in modo **incrementale e ripetibile** le modifiche da applicare allo **schema del database**.

---
### Funzioni
- **evoluzione controllata dello schema** (creazione/modifica/eliminazione di tabelle, colonne, indici, vincoli).
- **sincronizzazione** dell’ambiente di sviluppo, test e produzione.
- **cronologia delle modifiche** come parte del codice sorgente (version control).

---
### Caratteristiche

- Ogni [[migration]] contiene due operazioni principali:
    - `up`: applica la modifica (es. aggiunge una colonna).
    - `down`: annulla la modifica (rollback).
- Organizzate in sequenza cronologica (timestamp o numerazione).
- Eseguite da un **migration runner** (es. `sequelize-cli db:migrate`, Artisan).

---
### Vantaggi

- **Tracciabilità** delle modifiche allo schema.
- **Riproducibilità** in diversi ambienti.
- **Rollback** sicuro in caso di errore.
- Maggiore **collaborazione** in team (ogni dev applica le stesse migrazioni).

---
