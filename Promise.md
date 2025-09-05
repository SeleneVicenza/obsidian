oggetto che rappresenta il risultato **futuro** di un’operazione asincrona.  

3 stati:
1. **pending** → in attesa (l’operazione non è ancora completata).
2. **fulfilled** → completata con successo → ritorna un **valore**.
3. **rejected** → completata con errore → ritorna un **motivo (errore)**

``` csharp

Promise → pending
   |
   | resolve() → then()
   |
   | reject()  → catch()
   |
   → finally()
   
```




``` js

const myPromise = new Promise((resolve, reject) => {
  // Operazione asincrona
  const ok = true
  if (ok) {
    resolve("Operazione riuscita")
  } else {
    reject("Errore!")
  }
})

myPromise
  .then(result => console.log(result))   // se resolve
  .catch(error => console.error(error))  // se reject
  .finally(() => console.log("Finita"))

```

### 1. `.then()`

- Usato per gestire il **risultato positivo**.
- Può restituire un nuovo valore o un’altra Promise (per chaining).

``` js
getUser()   .then(user => getOrders(user.id))   .then(orders => console.log("Ordini:", orders))

```

### 2. `.catch()`

- Gestisce gli **errori** (stato _rejected_).
``` js
getUser()
   .then(user => getOrders(user.id))   
   .catch(err => console.error("Errore:", err))

```

### 3. `.finally()`

- Eseguito **sempre**, indipendentemente da successo o errore (cleanup).
    

``` js 

fetchData()
   .then(data => console.log("OK:", data))
   .catch(err => console.error("KO:", err))   
   .finally(() => console.log("Richiesta completata"))

```

## Catena di Promise

Ogni `.then()` ritorna **una nuova Promise**.  
Questo consente di concatenare operazioni in sequenza:

```

Promise.resolve(2)
   .then(n => n * 2)   // 4   
   .then(n => n + 3)   // 7   
   .then(console.log)  // stampa 7

```

==Nota: Se in una catena una Promise viene **reject**, il controllo passa subito al primo `.catch()` successivo.==

# Callback vs Promise vs Async/Await

|**Caratteristica**|**Callback**|**Promise**|**Async/Await**|
|---|---|---|---|
|**Stile**|Funzioni annidate|Catena `.then()`|Codice sequenziale|
|**Gestione errori**|Deve essere gestita manualmente (`err, result`)|`.catch()`|`try/catch`|
|**Leggibilità**|Rischio di _callback hell_ (annidamenti profondi)|Migliore grazie al chaining|Massima: sembra codice sincrono|
|**Composizione**|Difficile coordinare più operazioni|`Promise.all`, `race`, `allSettled`, `any`|Si appoggia alle Promise|
|**Uso tipico**|JS vecchio, Node.js <= v6|Standard moderno|Moderno + chiaro|

- **callback** solo se lavori con codice legacy.
- **Promise** per scrivere asincrono leggibile e comporre operazioni.
- **async/await** per il massimo della chiarezza → dietro le quinte lavora sempre con Promise