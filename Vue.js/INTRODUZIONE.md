Framework JavaScript per la costruzione di interfacce utente. Si basa su standard HTML, CSS e JavaScript e fornisce un modello di programmazione **dichiarativo** e **basato su componenti** che ti aiuta a sviluppare in modo efficiente interfacce utente, siano esse semplici o complesse.

``` js
import { createApp, ref } from 'vue' 

createApp({ 
	setup() { 
		return { 
			count: ref(0) 
		} 
	} 
}).mount('#app')
```

``` html
<div id="app"> 
	<button @click="count++"> 
		Il conteggio è: {{ count }} 
	</button> 
</div>
```

**Risultato**
Il conteggio è: 0

Due caratteristiche fondamentali di Vue:

- **Rendering Dichiarativo**: Vue estende l'HTML standard con una sintassi di template che ci permette di descrivere in modo dichiarativo l'output HTML basato sullo stato JavaScript.

	logica è _dichiarativa_:
	“descrivo **cosa** deve succedere nel DOM, non **come** farlo.”


- **Reattività**: Vue traccia automaticamente i cambiamenti dello stato JavaScript e aggiorna in modo efficiente il DOM quando avvengono modifiche.

---
## Framework Progressivo

Vue è un framework e un ecosistema che copre la maggior parte delle caratteristiche necessarie allo sviluppo frontend. Ma il web è estremamente vario - le cose che costruiamo sul web possono essere estremamente differenti nella forma e nella grandezza $=>$ progettato per essere flessibile e **favorirne l'uso in modo incrementale**. 

Utilizzato in modi diversi:
- Migliorare l'HTML statico senza l'uso della compilazione
- Incorporare Web Components su qualsiasi pagina
- Applicazioni Single-Page (SPA)
- Fullstack / Rendering Server-Side (SSR)
- Jamstack / Generazione di Siti Statici (SSG)
- per usi su desktop, mobile, WebGL e perfino al terminale



---
## Architettura base

Un’app Vue è composta da tre livelli principali:

1. **View (Template)** – HTML arricchito con _direttive_ (`v-if`, `v-for`, ecc.)
2. **ViewModel (Component)** – contiene dati e logica in JavaScript
3. **Model (Reactive Data)** – stato osservabile, sincronizzato automaticamente

Questo schema si avvicina al **pattern MVVM** (_Model–View–ViewModel_).  
Il “motore reattivo” mantiene sincronizzati i dati e la UI in entrambe le direzioni.

---
## Struttura tipica di un progetto

Ogni componente Vue è contenuto in un file `.vue` detto **SFC (Single File Component)**, suddiviso in tre sezioni:

`<template>   <h1>{{ title }}</h1>   <button @click="increment">Aumenta</button> </template>  <script> export default {   data() {     return { count: 0, title: "Contatore" };   },   methods: {     increment() { this.count++; }   } } </script>  <style scoped> button { color: #42b983; } </style>`

📘 **Template:** definisce la vista  
📘 **Script:** definisce dati, metodi e logica  
📘 **Style:** definisce lo stile del componente (scoped = locale)

---

## 🧩 Ecosistema Vue

Vue non è solo un framework, ma un **ecosistema completo**.  
I moduli più usati sono:

|Modulo|Funzione|
|---|---|
|**Vue Router**|Gestisce la navigazione client-side (SPA)|
|**Pinia** _(o Vuex)_|Gestisce lo stato globale dell’app|
|**Vite**|Tool moderno di build e hot reload|
|**Composition API**|Nuovo modo di strutturare logica e riuso|
|**DevTools**|Estensione browser per debug e profiling|

---

## Componenti Single-File[​](https://it.vuejs.org/guide/introduction.html#single-file-components)

Nei progetti Vue che sfruttano gli strumenti di compilazione, i componenti Vue vengono creati utilizzando un formato di file simile a HTML chiamato **Single-File Component** (Componente in un singolo file, anche noto come file `*.vue` , abbreviato in **SFC**). Un file SFC, come suggerisce il nome, incorpora in un singolo file la logica (JavaScript), il template (HTML) e lo stile (CSS) del componente. 

Esempio precedente, riscritto in formato SFC:



```vue
<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>

<template>
  <button @click="count++">Il conteggio è: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

L'SFC è una caratteristica distintiva di Vue ed è il modo consigliato per la creazione di componenti Vue **se** il progetto richiede l'uso e la configurazione della compilazione.

Vue si occuperà di tutta la configurazione degli strumenti di compilazione.

---
## Stili delle API

Due diversi stili di API: **Options API** e **Composition API**.

### Options API

Logica di un componente $=>$ oggetto contenente opzioni come `data`, `methods`, and `mounted`. 

Le proprietà definite dalle opzioni vengono esposte all'interno delle funzioni su `this`, che fa riferimento all'istanza del componente:

``` vue
<script>
export default {
  // Le proprietà restituite da data() diventano parte dello stato reattivo.
  // e saranno esposte su `this`.
  data() {
    return {
      count: 0
    }
  },

  // I metodi sono funzioni che modificano lo stato e innescano degli aggiornamenti.
  // Possono essere usati come gestori di eventi nei template (tramite `v-on`).
  methods: {
    increment() {
      this.count++
    }
  },

  // Gli hook del ciclo di vita vengono chiamati in diverse fasi
  // del ciclo di vita di un componente.
  // Questa funzione verrà chiamata quando il componente sarà montato.
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">Il conteggio è: {{ count }}</button>
</template>
```

Incentrata sul concetto di "component instance" ('istanza di componente', `this` nell'esempio), che tipicamente si allinea meglio con un **modello mentale basato sulle Classi** per gli utenti provenienti da linguaggi OOP. 
Più accessibile ai principianti, nascondendo i dettagli della reattività e imponendo un'organizzazione del codice tramite gruppi di opzioni.

### Composition API

Logica di un componente $->$ funzioni dell'API. 

Negli SFC, di solito, la Composition API è utilizzata con [`<script setup>`](https://it.vuejs.org/api/sfc-script-setup.html). 

L'attributo `setup` è un suggerimento che, durante la compilazione, permette a Vue di eseguire delle trasformazioni e usare la Composition API con meno codice ripetitivo (imports e le variabili / funzioni di livello superiore dichiarate in `<script setup>` possono essere usate direttamente nel template)


``` vue
<script setup>
	import { ref, onMounted } from 'vue'
	
	// stato reattivo
	const count = ref(0)
	
	// funzioni che modificano lo stato e innescano l'aggiornamento
	function increment() {
	  count.value++
	}
	
	// hook del ciclo di vita
	onMounted(() => {
	  console.log(`The initial count is ${count.value}.`)
	})
</script>

<template>
  <button @click="increment">Il conteggio è: {{ count }}</button>
</template>
```

Si concentra sulla dichiarazione di variabili di stato reattive direttamente nell'ambito di una funzione e, per gestire la complessità, sulla composizione dello stato da più funzioni insieme. È un modo di scrivere codice più libero e richiede una comprensione di come funziona la reattività in Vue per essere utilizzata in modo efficace. In cambio la sua flessibilità consente strutture per l'organizzazione e il riutilizzo della logica più potenti.

---
## 🧠 Concetti chiave

1. **Reattività:** ogni variabile in `data()` è “osservata” da Vue.  
    Se cambia, la UI si aggiorna automaticamente.

2. **Direttive:** arricchiscono l’HTML (es. `v-for`, `v-if`, `v-model`).

3. **Componenti:** blocchi riutilizzabili che compongono l’interfaccia.

4. **Props & Emits:** comunicazione padre-figlio.

5. **Lifecycle Hooks:** punti di ingresso per eseguire logica in momenti precisi.

6. **Computed e Watchers:** reagiscono ai cambiamenti dei dati.

7. **Routing & Stato globale:** separano viste e dati condivisi.


---

## 🧩 Differenze rispetto a React

|Aspetto|Vue.js|React|
|---|---|---|
|Sintassi|Template HTML con direttive|JSX (HTML in JS)|
|Two-way binding|Integrato (`v-model`)|Manuale con `useState`|
|Stato globale|Pinia/Vuex|Context/Redux|
|Lifecycle|Funzioni come `mounted()`|`useEffect()`|
|Curva di apprendimento|Più dolce, più dichiarativo|Più libero ma più verboso|
|Integrazione in progetti esistenti|Facilissima (script tag o modulo)|Di solito richiede build completa|

 In sintesi:

> Vue è ideale per chi vuole **sviluppare interfacce pulite, riattive e integrate** anche in contesti ibridi (es. Laravel Blade + Vue).

---

## Come si avvia un progetto

Con **Vite (tool ufficiale)**:

`npm init vue@latest my-project cd my-project npm install npm run dev`

Struttura generata:

`src/  ├─ components/  ├─ assets/  ├─ App.vue  └─ main.js`

`main.js` monta l’app sul DOM:

`import { createApp } from 'vue' import App from './App.vue'  createApp(App).mount('#app')`

---

## Integrazione con Laravel

Laravel e Vue si integrano perfettamente:

- Laravel gestisce **API REST o controller Blade**
- Vue gestisce **interfaccia e interattività**
- Vite coordina la build tra PHP e JS


Esempio d’integrazione:

`composer require laravel/ui php artisan ui vue npm install && npm run dev`

---

##  Punti di forza 
- Leggibilità e curva di apprendimento morbida
- Reattività automatica → meno codice imperativo
- Ottima integrazione con Laravel (stack classico full-stack)
- Ecosistema completo e leggero
- Ottime performance grazie a Virtual DOM e Vite
