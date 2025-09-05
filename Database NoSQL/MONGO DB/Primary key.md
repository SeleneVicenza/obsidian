- In **ogni documento** di una collezione c’è sempre un campo chiamato **`_id`**.

- Questo campo rappresenta la **primary key**:
    - Deve essere **unico** in tutta la collezione.
    - Non può essere **null**.  
    - Non può essere **modificato** dopo l’inserimento.    

- MongoDB crea automaticamente un indice univoco su `_id`.
	- Senza nessuna specifica, genera automaticamente un **`ObjectId`** (12 byte).
	- Può essere **qualsiasi valore univoco**: stringa, numero, UUID, ecc.


``` json

{
  "_id": ObjectId("650f1d3bcf9f9b1234567890"),
  "nome": "Selene",
  "ruolo": "Analista"
}

```

## Differenze rispetto a SQL

|**SQL**|**MongoDB**|
|---|---|
|Primary Key = campo definito in tabella|`_id` presente in ogni documento|
|PK può essere su più colonne (composite)|`_id` è sempre **singolo campo**|
|PK definito dallo sviluppatore|`_id` creato automaticamente se non specificato|
|Indice PK = clustered index|`_id` ha sempre indice univoco automatico|

# Evitare i duplicati?

## 1) Indici univoci (la soluzione principale)

Creano un vincolo a livello di storage: due documenti non possono avere lo stesso valore (o combinazione di valori) nei campi indicati.

### Indice univoco semplice

``` js

// Evita duplicati su email 
db.users.createIndex({ email: 1 }, { unique: true })

```

==**Nota:** se esistono già duplicati, la creazione fallisce==

### Indice univoco composito

``` js
// Evita duplicati sul pair (codiceFiscale, aziendaId)
db.persone.createIndex({ codiceFiscale: 1, aziendaId: 1 }, { unique: true })

```

### Unicità “case-insensitive” (collation)

``` js

db.users.createIndex(   { email: 1 },   { unique: true, collation: { locale: "it", strength: 2 } } // strength 2 = case-insensitive ) 
// Importante: tutte le query/insert devono usare la stessa collation quando serve confrontare/validare

```

### Indice univoco parziale (partial index)

Utile se vuoi vincolare l’unicità **solo** su documenti “attivi” (o con un certo stato).

``` js 

db.users.createIndex(   { email: 1 },   { unique: true, partialFilterExpression: { attivo: true } } )

```


> In cluster **shardato**: gli indici univoci **devono includere il prefisso della shard key** (o essere sull’`_id`). Pianifica la chiave di shard di conseguenza.

## 2) Upsert idempotenti (evitare duplicati da race conditions / retry)

Pattern: **chiave naturale** o **chiave tecnica** con indice univoco + `upsert: true`.

``` js 
// Se esiste, aggiorna; altrimenti inserisce (senza creare duplicati) 
db.users.updateOne(   { email: "selene@example.com" },       
// filtro su chiave unica   
{ $set: { nome: "Selene", ruolo: "Analista" } },   { upsert: true } )

```

- In caso di retry o richieste concorrenti, l’indice unico garantisce **idempotenza**.


---

## 3) Evitare duplicati **dentro gli array**

`$addToSet` invece di `$push` (lista).

``` js
// Non duplica l’elemento se già presente 
db.liste.updateOne(   { _id: 1 },   { $addToSet: { tags: "opcu" } } ) 
// Per più valori: 
db.liste.updateOne(   { _id: 1 },   { $addToSet: { tags: { $each: ["opcu", "scanner", "keyence"] } } } )
```

---

## 4) Validazione di schema (complementare)

Non rimuove duplicati, ma aiuta a **normalizzare** i dati (es. lowercase) prima del vincolo di unicità.

``` js
db.createCollection("users", {   validator: {     $jsonSchema: {       bsonType: "object",       required: ["email"],       properties: {         email: { bsonType: "string", pattern: "^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$" }       }     }   } }) 
// In app: salva sempre email in lowercase → poi indice unico su { email: 1 }
```


# Bonificare duplicati già presenti

Prima di creare l’indice unico, identifica e risolvi.

## 1 Trova duplicati per campo

``` js
db.users.aggregate([   { $group: { _id: { email: "$email" }, ids: { $push: "$_id" }, count: { $sum: 1 } } },   { $match: { count: { $gt: 1 } } } ])
```

## 2 Tenere il “primo” e rimuovere gli altri

_(Versione semplice, lato client o script Node.js)_

``` js

const dups = db.users.aggregate([   { $group: { _id: "$email", ids: { $push: "$_id" }, count: { $sum: 1 } } },   { $match: { count: { $gt: 1 } } } ]).toArray();  dups.forEach(g => {   g.ids.shift(); 
// tieni il primo   
db.users.deleteMany({ _id: { $in: g.ids } }); });

```

- indice unico:

``` js

db.users.createIndex({ email: 1 }, { unique: true })

```