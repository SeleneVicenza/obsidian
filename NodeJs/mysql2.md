- È un **driver per MySQL/MariaDB** scritto per Node.js.
- È il successore migliorato di `mysql` (più veloce, sicuro e compatibile).
- Supporta:
    - **Promise API** (comodo con `async/await`);
    - **Prepared Statements** (evita **SQL injection**);
    - **Pool di connessioni** (per applicazioni web con molte query concorrenti);
    - **Streaming di risultati** (per dataset grandi).

---
## Installazione

``` bash

npm install mysql2`

```

---
## Connessione diretta

``` js

const mysql = require('mysql2');

const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'password',
  database: 'testdb'
});

// Query con callback
connection.query('SELECT * FROM users', (err, results, fields) => {
  if (err) throw err;
  console.log(results); // array di oggetti
});

connection.end();

```

---
## Connessione con Promise API
``` js
const mysql = require('mysql2/promise');

async function main() {
  const connection = await mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'password',
    database: 'testdb'
  });

  const [rows, fields] = await connection.execute(
    'SELECT * FROM users WHERE id = ?',
    [1] // parametro → prepared statement
  );

  console.log(rows);
  await connection.end();
}
main();

```

###### `connection.execute(sql, params)` usa **prepared statements**, quindi evita SQL injection.

---
## Pool di connessioni

Per app web conviene usare un **pool** (riutilizza connessioni invece di crearne una per ogni query):

``` js
const mysql = require('mysql2/promise');

const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: 'password',
  database: 'testdb',
  waitForConnections: true,
  connectionLimit: 10,   // massimo 10 connessioni
  queueLimit: 0
});

async function main() {
  const [rows] = await pool.query('SELECT * FROM products');
  console.log(rows);
}

main();

```

---
## Streaming dei risultati

### Definizione

Lo **streaming dei risultati** consente di elaborare insiemi di dati molto grandi senza caricare l’intero result set in memoria.  
Il driver `mysql2` espone uno stream leggibile tramite il metodo:

`connection.query('SELECT ...').stream()`

### Caratteristiche
- I record vengono restituiti **a chunk** (uno alla volta o in piccoli gruppi).
- Lo stream può essere collegato ad altri stream Node.js (`pipeline`, `pipe`).
- È particolarmente adatto per **export di dati** (CSV, JSON Lines, log).
- Supporta **backpressure**: se il consumatore è lento, la sorgente si adatta.

### Vantaggi
- **Efficienza in memoria**: evita di accumulare grandi dataset in RAM.
- **Scalabilità**: gestione fluida di milioni di righe.
- **Compatibilità con il modello asincrono** di Node.js.


``` js
const connection = mysql.createConnection({ /* config */ });

const query = connection.query('SELECT * FROM big_table');
query.stream()
  .on('data', row => console.log(row))
  .on('end', () => {
    console.log('Fatto!');
    connection.end();
  });
```
### Attenzioni operative
- **Rilasciare sempre la connessione** (`conn.release()`) al termine dello streaming.
- **Gestire gli errori dello stream** tramite gli opportuni listener o l’uso di `pipeline`.
- **Evitare l’utilizzo in transazioni lunghe**, per non mantenere lock prolungati sul database

## Sintesi
- Usa `mysql2/promise` + `async/await` → codice più pulito e sicuro.
- Preferisci **`execute` con `?`** per query parametrizzate (contro injection).
- In app web/API → **pool di connessioni** è la scelta giusta.
- Per dataset enormi → usa lo **streaming** per non saturare la memoria.

---
