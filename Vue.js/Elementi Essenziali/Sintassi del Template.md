Vue utilizza una sintassi di template basata su HTML che ti permette in maniera dichiarativa di legare il DOM renderizzato con i dati dell'istanza del componente sottostante. Tutti i template Vue sono HTML sintatticamente valido che possono essere letti da parser e da browser conformi alle specifiche HTML.

Sotto il cofano, Vue compila i template in codice JavaScript altamente ottimizzato. Combinato con il sistema di reattività, Vue può capire in maniera intelligente il numero minimo di componenti da ri-renderizzare e applicare il minimo numero di modifiche al DOM quando cambia lo stato dell'app.

Se hai familiarità con i concetti del Virtual DOM e preferisci la potenza di JavaScript, con il supporto opzionale a JSX, puoi anche [scrivere direttamente le render function](https://it.vuejs.org/guide/extras/render-function.html) invece dei template. Tuttavia, nota che non godono dello stesso livello di ottimizzazioni durante la compilazione dei template.

---
## Interpolazione del Testo

La forma più basilare di data binding è l'interpolazione del testo utilizzando la sintassi "Mustache" (doppie parentesi graffe):



``` template
<span>Messaggio: {{ msg }}</span>
```

Il tag mustache verrà sostituito con il valore della proprietà msg [dall'istanza del componente corrispondente](https://it.vuejs.org/guide/essentials/reactivity-fundamentals.html#declaring-reactive-state). Sarà anche aggiornato ogni volta che la proprietà msg cambia.

---
## HTML puro

Le doppie parentesi graffe interpretano i dati come testo normale, non HTML. Per produrre vero HTML, avrai bisogno di utilizzare la [direttiva `v-html`](https://it.vuejs.org/api/built-in-directives.html#v-html):


``` template
<p>Usando l'interpolazione del testo: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

Usando l'interpolazione del testo: <span style="color: red">Questo dovrebbe essere rosso.</span>

Using v-html directive: Questo dovrebbe essere rosso.


Attributo `v-html` $->$ **direttiva** (directive). 
Le [[direttive]] sono prefissate con `v-` per indicare che sono attributi speciali forniti da Vue e applicano un comportamento reattivo speciale al DOM renderizzato. Con questo codice stiamo dicendo "Mantieni l'HTML interno di questo elemento sempre aggiornato con la proprietà `rawHtml` dell'istanza attiva corrente".

Il contenuto dello `span` verrà sostituito con il valore della proprietà rawHtml e interpretato come HTML grezzo - i data binding saranno ignorati. Nota che non è possibile utilizzare `v-html` per comporre frammenti di template, poiché Vue non è un motore di templating basato su stringhe. Si preferisce, invece, l'uso dei componenti come unità fondamentali per la riutilizzazione e la composizione dell'interfaccia utente.

##### Avviso di Sicurezza
La renderizzazione dinamica di HTML arbitrario sul tuo sito web può essere molto pericolosa in quanto può facilmente portare a [vulnerabilità XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). Utilizza `v-html` solo su contenuti di fiducia e **mai** su contenuti forniti dall'utente.

----
## Associazioni di Attributi (binding)

Le parentesi graffe non possono essere utilizzate all'interno degli attributi HTML. Si può usare, invece, la [direttiva `v-bind`](https://it.vuejs.org/api/built-in-directives.html#v-bind):



``` template
<div v-bind:id="dynamicId"></div>
```

La direttiva `v-bind` indica a Vue di mantenere sincronizzato l'attributo `id` dell'elemento con la proprietà dinamica `dynamicId` del componente. Se il valore associato è `null` o `undefined`, l'attributo verrà rimosso dall'elemento renderizzato.

### Forma abbreviata[​](https://it.vuejs.org/guide/essentials/template-syntax.html#shorthand)

dato che `v-bind` è utilizzato così di frequente, ha una sintassi abbreviata dedicata:



``` template
<div :id="dynamicId"></div>
```

Gli attributi che iniziano con `:` possono sembrare un po' diversi dall'HTML normale, ma in realtà è un carattere valido per i nomi degli attributi e tutti i browser supportati da Vue possono analizzarlo correttamente. Non appaiono, inoltre, nel markup finale renderizzato. La sintassi abbreviata è opzionale, ma probabilmente la apprezzerai quando imparerai qualcosa in più sul suo utilizzo più avanti.

> Per il resto della guida, negli esempi di codice utilizzeremo la sintassi abbreviata, dato che è l'uso più comune per gli sviluppatori Vue.

### Attributi Booleani

Gli [Attributi Booleani](https://html.spec.whatwg.org/multipage/common-microsyntaxes.html#boolean-attributes) sono attributi che possono indicare valori veri / falsi con la loro presenza su un elemento. Ad esempio,[`disabled`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/disabled) è uno degli attributi booleani più comunemente utilizzati.

In questo caso `v-bind` funziona un po' diversamente:

template

```
<button :disabled="isButtonDisabled">Button</button>
```

L'attributo `disabled` sarà incluso se `isButtonDisabled` ha un [valore truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy). Sarà incluso anche se il valore è una stringa vuota, mantenendo la coerenza con `<button disabled="">`. Per altri [valori falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) l'attributo sarà omesso.

### Associazione Dinamica di Attributi Multipli[​](https://it.vuejs.org/guide/essentials/template-syntax.html#dynamically-binding-multiple-attributes)

Se hai un oggetto JavaScript che rappresenta attributi multipli che assomiglia a questo:


``` js

const objectOfAttrs = {
  id: 'container',
  class: 'wrapper'
}
```

Puoi associarli a un singolo elemento utilizzando `v-bind` senza un argomento:

template

```
<div v-bind="objectOfAttrs"></div>
```

----
## Utilizzo delle Espressioni JavaScript[​](https://it.vuejs.org/guide/essentials/template-syntax.html#using-javascript-expressions)

Finora abbiamo associato nei nostri template solo semplici chiavi di proprietà ma, all'interno di tutti i data binding, Vue supporta tutta la potenza delle espressioni JavaScript:

template

```
{{ number + 1 }}

{{ ok ? 'SI' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

Queste espressioni verranno valutate come JavaScript nello scope dei dati nell'istanza del componente corrente.

Nei template Vue le espressioni JavaScript possono essere utilizzate nelle seguenti posizioni:

- All'interno delle interpolazioni di testo (mustache)
- Nel valore dell'attributo di qualsiasi direttiva Vue (attributi speciali che iniziano con `v-`)

### Singole espressioni[​](https://it.vuejs.org/guide/essentials/template-syntax.html#expressions-only)

Un'espressione è un pezzo di codice che può essere valutato per ottenere un valore. Un semplice test è verificare se può essere utilizzato dopo `return`.

Pertanto, il seguente codice **NON** funzionerà:

template

```
<!-- questa è un'istruzione, non un'espressione: -->
{{ var a = 1 }}

<!-- nemmeno il controllo di flusso funzionerà , usa espressioni ternarie -->
{{ if (ok) { return message } }}
```

### Chiamata di Funzioni[​](https://it.vuejs.org/guide/essentials/template-syntax.html#calling-functions)

È possibile chiamare un metodo esposto da un componente all'interno di un'espressione di binding:

template

```
<time :title="toTitleDate(date)" :datetime="date">
  {{ formatDate(date) }}
</time>
```

TIP

Le funzioni chiamate all'interno delle espressioni di binding verranno chiamate ogni volta che il componente si aggiorna, quindi accertati che non abbiano alcun effetto collaterale, come modificare i dati o innescare operazioni asincrone.

### Accesso Limitato alle Globali[​](https://it.vuejs.org/guide/essentials/template-syntax.html#restricted-globals-access)

Le espressioni del template sono isolate e hanno accesso solo a una [lista limitata delle Globali](https://github.com/vuejs/core/blob/main/packages/shared/src/globalsAllowList.ts#L3). La lista espone le globali native usate comunemente come `Math` e `Date`.

Le Globali (oggetti, funzioni) non incluse nella lista, ad esempio le proprietà aggiunte dall'utente alla `window`, non saranno accessibili nelle espressioni del template. Puoi comunque definire esplicitamente delle globali aggiuntive per tutte le espressioni di Vue, aggiungendole a [`app.config.globalProperties`](https://it.vuejs.org/api/application.html#app-config-globalproperties).



