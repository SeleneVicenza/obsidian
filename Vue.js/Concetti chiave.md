## 1. **Reattività (Reactivity System)**

### Cos’è

Il cuore di Vue.  
Ogni variabile dichiarata dentro `data()` (Options API) o `ref()` / `reactive()` (Composition API) è **osservata**:  
quando cambia il valore, Vue aggiorna automaticamente il DOM virtuale.

### Esempio (Options API)

`<template>   <p>Contatore: {{ count }}</p>   <button @click="count++">+</button> </template>  <script> export default {   data() { return { count: 0 } } } </script>`

### Come funziona internamente

Vue usa un **proxy reattivo**: intercetta get/set degli oggetti e notifica le dipendenze.  
Non serve manipolare il DOM manualmente.

### 🔁 In React

Equivale a `useState()` + `setState()`  
👉 In Vue è più “automatico”, non richiede una funzione setter.

---

##  2. **Direttive (Directives)**

### Cos’è

Le direttive sono **attributi speciali** che aggiungono logica al template HTML.  
Sono la base della sintassi dichiarativa di Vue.

### Principali direttive

|Direttiva|Significato|Esempio|
|---|---|---|
|`v-if`|Mostra/nasconde in base a una condizione|`<p v-if="isVisible">Visibile</p>`|
|`v-for`|Crea un ciclo sugli elementi|`<li v-for="u in users">{{ u.name }}</li>`|
|`v-bind` o `:`|Collega attributi dinamici|`<img :src="url" />`|
|`v-on` o `@`|Gestisce eventi|`<button @click="save">Salva</button>`|
|`v-model`|Collega input ↔ variabile (two-way binding)|`<input v-model="email" />`|

### Da ricordare

- Le direttive rendono il template _reattivo e leggibile_
    
- Si possono creare **direttive personalizzate** (`app.directive()`)
    

### In React

Stessa logica con JSX condizionale e cicli:

`{isVisible && <p>Visibile</p>} {users.map(u => <li>{u.name}</li>)}`

---

## 🔹 3. **Props & Emits**

### Cos’è

Meccanismo di **comunicazione tra componenti**:

- _Props_ → dati dal componente padre al figlio
    
- _Emit_ → eventi dal figlio al padre
    

### Esempio

**Parent.vue**

`<UserCard :name="userName" @logout="handleLogout" />`

**UserCard.vue**

`<template>   <p>{{ name }}</p>   <button @click="$emit('logout')">Logout</button> </template>  <script> export default { props: ['name'] } </script>`

### Da ricordare

- I dati fluiscono **solo top → down**
    
- `$emit()` serve per notificare azioni al padre
    
- È buona pratica **tipizzare le props**
    
    `props: { name: { type: String, required: true } }`
    

### In React

Stesso concetto:

`<UserCard name={userName} onLogout={handleLogout} />`

---

## 4. **Lifecycle Hooks**

### Cos’è

Ogni componente Vue attraversa fasi di vita.  
I _lifecycle hooks_ sono metodi che permettono di eseguire codice in momenti specifici (creazione, montaggio, aggiornamento, distruzione).

### Principali hook (Options API)

|Hook|Quando viene eseguito|Uso tipico|
|---|---|---|
|`created()`|Dopo l’inizializzazione dati|chiamate API, setup logico|
|`mounted()`|Dopo l’inserimento nel DOM|accesso a elementi DOM|
|`updated()`|Dopo l’aggiornamento dei dati|log, ricalcoli|
|`unmounted()`|Alla distruzione del componente|pulizia risorse|

### Esempio

`<script> export default {   mounted() {     console.log("Componente montato");   },   unmounted() {     console.log("Componente distrutto");   } } </script>`

### In React

Equivalente a `useEffect(() => {...}, [])` con `return () => {...}` per cleanup.

---

## 🔹 5. **Computed Properties e Watchers**

### Cos’è

Due strumenti per gestire **dati derivati o reazioni a cambiamenti**:

- `computed` → proprietà calcolate e memorizzate automaticamente
    
- `watch` → osserva variabili e reagisce ai cambiamenti
    

---

### Esempio – _Computed_

`<script> export default {   data() {     return { first: 'Selene', last: 'Vicenza' }   },   computed: {     fullName() { return this.first + ' ' + this.last }   } } </script>`

➡️ `fullName` si aggiorna solo quando cambiano `first` o `last`.

---

### Esempio – _Watch_

`<script> export default {   data() { return { email: '' } },   watch: {     email(newVal) { console.log('Email aggiornata:', newVal) }   } } </script>`

➡️ Esegue codice reattivo al cambio di una variabile.

---

### Da ricordare

|Tipo|Scopo|Analogia React|
|---|---|---|
|`computed`|proprietà derivate, con caching|`useMemo()`|
|`watch`|side effects su cambio dati|`useEffect()`|

---

## Riepilogo “5 concetti chiave”

|#|Concetto|Descrizione sintetica|Analogia React|
|---|---|---|---|
|1|**Reactivity**|Dati osservabili, UI aggiornata automaticamente|useState|
|2|**Directives**|Logica dichiarativa nel template|JSX condizionale|
|3|**Props & Emits**|Comunicazione tra componenti|Props / callback|
|4|**Lifecycle Hooks**|Gestione delle fasi di vita|useEffect|
|5|**Computed & Watch**|Dati derivati e reazioni|useMemo / useEffect|