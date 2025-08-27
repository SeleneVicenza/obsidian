**[[ORM]]** per Node.js che permette di interagire con database SQL (MySQL, MariaDB, PostgreSQL, SQLite, MSSQL) usando **oggetti JavaScript/TypeScript** invece di query SQL scritte a mano.

---
### Caratteristiche principali

- Supporto a più database SQL.
- Definizione dei modelli (tabelle) come **classi JavaScript**.
- Mapping tra oggetti e record DB (**CRUD** semplificato).
- Gestione delle **relazioni** (1:1, 1:N, N:M).
- Migrazioni per evoluzione dello schema DB.
- API **Promise-based**, compatibile con `async/await`.
- Supporto a **validazioni** e **hook** (beforeCreate, afterUpdate, ecc.).

---
### Vantaggi

- **Astrazione del SQL**: riduce il codice SQL da scrivere a mano.
- **Produttività**: operazioni CRUD rapide e consistenti.
- **Portabilità**: stesso codice su database diversi.
- **Struttura**: organizza meglio modelli, relazioni e migrazioni.
- **Integrazione**: Express, NestJS, Fastify.

---
### Definizione di un modello

``` js
const { Sequelize, DataTypes } = require('sequelize');  
// Connessione al database 
const sequelize = new Sequelize('testdb', 'root', 'password', {   host: 'localhost',   dialect: 'mysql' 
// oppure 'postgres', 'sqlite', 'mssql' 
});  
// Definizione modello User 
const User = sequelize.define('User', {   
	name: { type: DataTypes.STRING, allowNull: false },   
	email: { type: DataTypes.STRING, unique: true },   
	age: { type: DataTypes.INTEGER } 
}, {   
	tableName: 'users' 
});`
```

---
### Operazioni CRUD

- **Creazione**

``` js
await User.create({ name: 'Selene', email: 'selene@example.com', age: 25 });
```

- **Lettura**

``` js
const users = await User.findAll({ where: { age: { [Sequelize.Op.gt]: 18 } } });`
```


- **Aggiornamento**

``` js
await User.update({ age: 26 }, { where: { id: 1 } });
```

- **Eliminazione**

``` js 
await User.destroy({ where: { id: 1 } });
```

---
