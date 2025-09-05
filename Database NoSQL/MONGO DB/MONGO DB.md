**database non relazionale** ([[Database NoSQL]]) basato su **documenti**.  
I dati vengono memorizzati in **documenti BSON** (Binary JSON), che sono simili a oggetti JSON ma con tipi aggiuntivi (Date, ObjectId, ecc.).

Utilizza:
- **Database** → contenitore principale
- **Collection** → equivalente alle “tabelle”
- **Document** → equivalente ai record, ma in formato JSON/BSON ([[JSON vs BSON]])


# Differenze tra query classiche (SQL) e query in MongoDB

## 1. **Modello dei dati**

- **SQL (relazionale)**: dati in **tabelle** con righe e colonne, schema rigido, relazioni con chiavi primarie/esterne.

- **MongoDB (NoSQL document)**: dati in **documenti BSON** (simili a JSON), raccolti in **collezioni**, schema flessibile.


---

## 2. **Selezione dei dati**

- **SQL**: usa `SELECT ... FROM ... WHERE ...`
- **MongoDB**: usa `find({ condizioni })`

``` sql

SELECT * FROM utenti WHERE attivo = 1;

```


``` js

db.utenti.find({ attivo: true })

```

---

## 3. **Proiezione dei campi**

- **SQL**: scegli le colonne nella SELECT.
- **MongoDB**: usi il secondo parametro di `find()`.


``` sql

SELECT nome, email FROM utenti;

```

``` js 

db.utenti.find({}, { nome: 1, email: 1, _id: 0 })

```

---

## 4. **Filtri e operatori**

- **SQL**: `=`, `<`, `>`, `<=`, `>=`, `IN`, `LIKE`.
- **MongoDB**: `$eq`, `$lt`, `$gt`, `$lte`, `$gte`, `$in`, `$regex`.


``` sql 

SELECT * FROM utenti WHERE eta > 25;

```

``` js

db.utenti.find({ eta: { $gt: 25 } })

```
---

## 5. **AND / OR**

- **SQL**: parole chiave `AND`, `OR`.
- **MongoDB**: oggetto JSON con campi multipli (AND implicito) oppure operatori `$and` / `$or`.



``` sql

SELECT * FROM utenti WHERE attivo = 1 AND ruolo = 'Analista';

```

``` js

db.utenti.find({ attivo: true, ruolo: "Analista" })

```

---

## 6. **Ordinamento e limitazione**

- **SQL**: `ORDER BY ... LIMIT ...`
- **MongoDB**: `.sort()` e `.limit()`.

``` sql 

SELECT * FROM utenti ORDER BY eta DESC LIMIT 5;

```

``` js 

db.utenti.find().sort({ eta: -1 }).limit(5)

```

---

## 7. **Inserimento**

- **SQL**: `INSERT INTO ... VALUES ...`
- **MongoDB**: `insertOne()` o `insertMany()`.


``` sql 

INSERT INTO utenti (nome, ruolo) VALUES ('Selene', 'Analista');

```

``` js 

db.utenti.insertOne({ nome: "Selene", ruolo: "Analista" })

```

---

## 8. **Aggiornamento**

- **SQL**: `UPDATE ... SET ... WHERE ...`
- **MongoDB**: `updateOne()`, `updateMany()` con operatori `$set`, `$inc`, `$unset`.


``` sql

UPDATE utenti SET ruolo = 'Manager' WHERE nome = 'Selene';

```

```js 

db.utenti.updateOne({ nome: "Selene" }, { $set: { ruolo: "Manager" } })

```

---

## 9. **Cancellazione**

- **SQL**: `DELETE FROM ... WHERE ...`
    
- **MongoDB**: `deleteOne()`, `deleteMany()`.

``` sql 

DELETE FROM utenti WHERE attivo = 0;

```

``` js 
db.utenti.deleteMany({ attivo: false })

```

---

## 10. **Aggregazioni e raggruppamenti**

- **SQL**: `GROUP BY`, `HAVING`, funzioni aggregate (`COUNT`, `SUM`, `AVG`).
- **MongoDB**: `aggregate()` con `$group`, `$match`, `$sum`, `$avg`.


``` sql

SELECT ruolo, COUNT(*) as totale FROM utenti GROUP BY ruolo;

```

``` js 

db.utenti.aggregate([   { $group: { _id: "$ruolo", totale: { $sum: 1 } } } ])

```

# SQL vs MongoDB

|**Operazione**|**SQL (classico)**|**MongoDB**|
|---|---|---|
|**Selezione (SELECT)**|`SELECT * FROM utenti WHERE attivo = 1;`|`db.utenti.find({ attivo: true })`|
|**Selezione campi**|`SELECT nome, email FROM utenti;`|`db.utenti.find({}, { nome: 1, email: 1, _id: 0 })`|
|**Filtro (WHERE)**|`SELECT * FROM utenti WHERE eta > 25;`|`db.utenti.find({ eta: { $gt: 25 } })`|
|**AND / OR**|`SELECT * FROM utenti WHERE attivo = 1 AND ruolo = 'Analista';`|`db.utenti.find({ attivo: true, ruolo: "Analista" })`|
||`SELECT * FROM utenti WHERE ruolo = 'Analista' OR ruolo = 'Dev';`|`db.utenti.find({ $or: [ { ruolo: "Analista" }, { ruolo: "Dev" } ] })`|
|**LIKE**|`SELECT * FROM utenti WHERE nome LIKE '%len%';`|`db.utenti.find({ nome: { $regex: "len", $options: "i" } })`|
|**Ordinamento**|`SELECT * FROM utenti ORDER BY eta DESC LIMIT 5;`|`db.utenti.find().sort({ eta: -1 }).limit(5)`|
|**Inserimento**|`INSERT INTO utenti (nome, ruolo) VALUES ('Selene','Analista');`|`db.utenti.insertOne({ nome: "Selene", ruolo: "Analista" })`|
|**Aggiornamento**|`UPDATE utenti SET ruolo = 'Manager' WHERE nome = 'Selene';`|`db.utenti.updateOne({ nome: "Selene" }, { $set: { ruolo: "Manager" } })`|
|**Cancellazione**|`DELETE FROM utenti WHERE attivo = 0;`|`db.utenti.deleteMany({ attivo: false })`|
|**Conteggio**|`SELECT COUNT(*) FROM utenti WHERE attivo = 1;`|`db.utenti.countDocuments({ attivo: true })`|
|**Group By**|`SELECT ruolo, COUNT(*) FROM utenti GROUP BY ruolo;`|`db.utenti.aggregate([{ $group: { _id: "$ruolo", totale: { $sum: 1 } } }])`|