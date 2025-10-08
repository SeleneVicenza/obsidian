Le direttive sono attributi speciali con il prefisso `v-`. Vue fornisce un numero di [direttive native](https://it.vuejs.org/api/built-in-directives.html), tra cui `v-html` e `v-bind` che abbiamo introdotto in precedenza.

I valori degli attributi delle direttive dovrebbero essere singole espressioni JavaScript (ad eccezione di `v-for`, `v-on` e `v-slot`, che saranno presentati nell relative sezioni in seguito). Il compito di una direttiva è applicare reattivamente gli aggiornamenti al DOM quando il valore della sua espressione cambia. Prendiamo come esempio [`v-if`](https://it.vuejs.org/api/built-in-directives.html#v-if):

template

```
<p v-if="seen">Ora mi vedi</p>
```

Qui, la direttiva `v-if` può rimuovere / inserire l'elemento `<p>` in base alla veridicità del valore dell'espressione `seen`.

### Arguments[​](https://it.vuejs.org/guide/essentials/template-syntax.html#arguments)

Alcune direttive possono accettare un "argomento", indicato da due punti dopo il nome della direttiva. Ad esempio, la direttiva `v-bind` viene utilizzata per aggiornare in maniera reattiva un attributo HTML:

template

```
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>
```

Qui `href` è l'argomento, che indica alla direttiva `v-bind` di collegare l'attributo `href` dell'elemento al valore dell'espressione `url`. Nella forma abbreviata, tutto ciò che precede l'argomento (cioè `v-bind:`) è condensato in un singolo carattere `:`.

Un altro esempio è la direttiva `v-on` che ascolta gli eventi del DOM:

template

```
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>
```

Qui l'argomento è il nome dell'evento da ascoltare: `click`. `v-on` ha una corrispondente forma abbreviata, ovvero il carattere `@`. Più avanti parleremo nel dettaglio anche della gestione degli eventi.

### Argomenti Dinamici[​](https://it.vuejs.org/guide/essentials/template-syntax.html#dynamic-arguments)

È possibile utilizzare anche un'espressione JavaScript come argomento di direttiva, racchiudendola tra parentesi quadre:

template

```
<!--
Nota che ci sono alcune limitazioni all'uso di espressioni nell'argomento,
come spiegato nelle sezioni "Vincoli sul Valore dell'Argomento Dinamico" e "Vincoli sulla Sintassi dell'Argomento Dinamico" qui sotto.
-->
<a v-bind:[attributeName]="url"> ... </a>

<!-- shorthand -->
<a :[attributeName]="url"> ... </a>
```

Qui `attributeName` verrà valutato dinamicamente come un'espressione JavaScript e il suo valore generato verrà utilizzato come valore finale per l'argomento. Ad esempio, se l'istanza del tuo componente ha una proprietà dati, `attributeName`, il cui valore è `"href"`, allora questo binding sarà equivalente a `v-bind:href`.

Allo stesso modo puoi utilizzare argomenti dinamici per collegare un handler (gestore) ad un nome di evento dinamico:

template

```
<a v-on:[eventName]="doSomething"> ... </a>

<!-- shorthand -->
<a @[eventName]="doSomething">
```

In questo esempio, quando il valore di `eventName` è `"focus"`, `v-on:[eventName]` sarà equivalente a `v-on:focus`.

#### Vincoli sul Valore dell'Argomento Dinamico[​](https://it.vuejs.org/guide/essentials/template-syntax.html#dynamic-argument-value-constraints)

Gli argomenti dinamici si aspettano di ricevere (valutare) una stringa, con l'eccezione di `null`. Il valore speciale `null` può essere utilizzato per rimuovere esplicitamente il binding. Qualsiasi altro valore non stringa causerà un avviso.

#### Vincoli sulla Sintassi dell'Argomento Dinamico[​](https://it.vuejs.org/guide/essentials/template-syntax.html#dynamic-argument-syntax-constraints)

Le espressioni degli argomenti dinamici hanno alcuni vincoli di sintassi perché alcuni caratteri, come spazi e virgolette, non sono validi all'interno dei nomi degli attributi HTML. Ad esempio, il seguente codice è non valido:

template

```
<!-- Questo causerà un avviso (warning) del compilatore.  -->
<a :['foo' + bar]="value"> ... </a>
```

Se hai bisogno di passare un argomento dinamico complesso, è probabilmente meglio utilizzare una [computed property](https://it.vuejs.org/guide/essentials/computed.html), che tratteremo a breve.

Quando si utilizzano template in-DOM (template scritti direttamente in un file HTML), si dovrebbe anche evitare di nominare le chiavi con caratteri maiuscoli, poiché i browser convertiranno i nomi degli attributi in minuscolo:

template

```
<a :[someAttr]="value"> ... </a>
```

Il codice qui sopra verrà convertito in `:[someattr]` nei template in-DOM. Se il tuo componente ha una proprietà `someAttr` invece di `someattr`, il tuo codice non funzionerà. I template all'interno dei Componenti Single-File **non** sono soggetti a questo vincolo.

### Modificatori[​](https://it.vuejs.org/guide/essentials/template-syntax.html#modifiers)

I modificatori sono speciali suffissi, contrassegnati da un punto, che indicano ad una direttiva di comportarsi in maniera particolare. Ad esempio, il modificatore `.prevent` indica alla direttiva `v-on` di usare `event.preventDefault()` sull'evento innescato:

template

```
<form @submit.prevent="onSubmit">...</form>
```

Vedrai altri esempi di modificatori più avanti, [per `v-on`](https://it.vuejs.org/guide/essentials/event-handling.html#event-modifiers) e [per `v-model`](https://it.vuejs.org/guide/essentials/forms.html#modifiers), quando esploreremo queste funzionalità.

Ecco, infine, la sintassi completa della direttiva visualizzata:

![grafico della sintassi della direttiva](https://it.vuejs.org/assets/directive.69c37117.png)