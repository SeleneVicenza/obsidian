Framework JavaScript per costruire **interfacce dichiarative** basate su **componenti** e su un **flusso di dati unidirezionale**. Il rendering è riconciliato in modo efficiente tramite un algoritmo di diff (Virtual DOM) e, dalle versioni recenti, include un **scheduler concorrente** che permette aggiornamenti più reattivi (transitions, Suspense).

# Concetti fondamentali

## Componenti a funzione

``` tsx
function Hello({ name }: { name: string }) {
   return <h1>Ciao {name}</h1>; 
}
```

- **Props**: input immutabili del componente.
- **Children**: composizione (slot “naturali” di React).

## Stato e ciclo di vita con Hook

Gli hook eliminano le classi e modellano il ciclo di vita in modo esplicito.

##### `useState` + `useEffect`

``` tsx
function Clock() {
   const [time, setTime] = React.useState(() => new Date());
   React.useEffect(() => {     
	   const id = setInterval(() => setTime(new Date()), 1000);     
	   return () => clearInterval(id); // cleanup   
   }, []);   
   return <span>{time.toLocaleTimeString()}</span>; 
}
```

##### `useMemo` e `useCallback` (ottimizzazioni mirate)

``` tsx

const total = React.useMemo(() => items.reduce((s,i) => s+i.price, 0), [items]); const onAdd = React.useCallback((p: Product) => setItems(v => [...v, p]), []);

```

> Usa `useMemo/useCallback` solo quando previeni concretamente render costosi/ricreazioni di funzioni passate a children memorizzati.

##### `useRef`
- Mantiene **mutabili** non-rendering (timer, elementi DOM, cache locale).  

``` tsx
const inputRef = React.useRef<HTMLInputElement>(null);
```

##### `useReducer` + Context per stato condiviso

``` tsx
type State = { count: number }; 
type Action = { type: 'inc' | 'dec' };  

function reducer(s: State, a: Action): State {
   if (a.type === 'inc') return { count: s.count + 1 };
   if (a.type === 'dec') return { count: s.count - 1 };   
   return s; 
} 
 
const CounterCtx = React.createContext<{
   state: State; dispatch: React.Dispatch<Action>; 
} | null>(null);  

function CounterProvider({ children }: { children: React.ReactNode }) {
   const [state, dispatch] = React.useReducer(reducer, { count: 0 });
      return <CounterCtx.Provider value={{ state, dispatch }}>{children}</CounterCtx.Provider>; 
}

```

# Rendering, chiavi e riconciliazione

- Le **key** identificano gli elementi in liste e guidano il diff.
- Key **stabili** e **uniche** nel sottoalbero (evita l’indice se l’ordine può cambiare).

``` tsx
{todos.map(t => <TodoItem key={t.id} todo={t} />)}
```

---

# Gestione form: controllati vs non controllati

### Controllato
``` tsx
const [value, setValue] = React.useState(''); 
<input value={value} onChange={e => setValue(e.target.value)} />
```

### Non controllato (ref)

``` tsx
const ref = React.useRef<HTMLInputElement>(null); 
<form onSubmit={() => console.log(ref.current!.value)}>
   <input ref={ref} defaultValue="Ciao" /> 
</form>
```

> Controllati: validazioni live e logica centralizzata. Non controllati: più leggeri.

---

# Data-Fetching moderno

## Suspense + boundary di errore (pattern raccomandato)

Con librerie come **React Query** o **SWR** ottieni cache, deduplica, retry e integrazione Suspense.

Esempio con React Query (concettuale):

``` tsx
function Products() {
   const { data } = useQuery({ queryKey: ['products'], queryFn: fetchProducts, suspense: true });
   return <ul>{data.map(p => <li key={p.id}>{p.name}</li>)}</ul>; 
}  
// Uso: 
<Suspense fallback={<Spinner />}>
   <ErrorBoundary fallback={<ErrorView />}>
        <Products />   
	</ErrorBoundary>
</Suspense>
```

## Transitions (UI reattiva durante filtri/ricerche)

``` tsx
const [isPending, startTransition] = React.useTransition(); 
function onFilter(q: string) {
   startTransition(() => setQuery(q)); // priorità bassa: non blocca input 
}
```

---

# Performance e best practice

- **Evita** re-render inutili: struttura lo stato vicino a chi lo usa.
- **Memoization selettiva**:
    - `React.memo(Component)` quando i props cambiano raramente.
    - `useMemo/useCallback` per calcoli pesanti o prop-funzione passate a children memorizzati.
- **Virtualizzazione liste** (es. `react-window`) per liste lunghe.
- **Split per route/chunk** (code-splitting con `lazy` + `Suspense`):

``` tsx
const Settings = React.lazy(() => import('./Settings')); 
<Suspense fallback={<Spinner />}><Settings /></Suspense>
```

---

# Architettura e stato globale

- **Locale** con `useState/useReducer`.
- **Condiviso semplice**: Context + reducer.
- **App complesse**: librerie dedicate
    - **Redux Toolkit** (immutabilità, middleware, devtools, RTK Query per dati remoti).
    - **Zustand** (store minimale, selettori, ottimo per UI).
    - **Jotai/Recoil** (stato per atomi/derivazioni).
    - **React Query**/**SWR** per **server state** (dati remoti: cache/invalidation/polling).


> Separare **UI state** (client) da **server state** (remoto) riduce complessità.

---

# Routing

- **React Router** per SPA tradizionali (loader/action, nested routes).
- **Next.js** per SSR/SSG/ISR e **React Server Components (RSC)**: ottimo per SEO, performance e data-fetching lato server “per componente”.

---

# React Server Components (RSC)

- Permettono di renderizzare alcuni componenti **solo sul server** (niente bundle client), con accesso diretto a DB/API e **streaming** al client.
- Next.js (App Router) è il modo più maturo per usarli oggi.

---

# Error handling

- **Error boundaries** catturano errori di rendering:

``` tsx
class ErrorBoundary extends React.Component<{ fallback: React.ReactNode }, { hasError: boolean }> {
   state = { hasError: false };
   static getDerivedStateFromError() { return { hasError: true }; }
   render() { return this.state.hasError ? this.props.fallback : this.props.children; }
}
```

---

# Testing

- **React Testing Library (RTL)** per test d’integrazione orientati all’utente.
- **Vitest/Jest** come test runner.


``` tsx
import { render, screen, fireEvent } from '@testing-library/react'; 
test('incrementa', () => {
   render(<Counter />);
   fireEvent.click(screen.getByRole('button', { name: /+/i }));
   expect(screen.getByText(/1/)).toBeInTheDocument(); 
   
});
```

---

# Tooling moderno

- **Vite** per DX e build veloci.
- **TypeScript** consigliato: component props tipate, hook sicuri.
- **ESLint + React hooks rules** per vincoli d’uso corretti.
- **Prettier** per formattazione consistente.

---

# Pattern utili (pratici)

## Contenitore/Presentazionale (separa logica da UI)

``` tsx

function useProducts() {
   return useQuery({ queryKey: ['products'], queryFn: fetchProducts }); 
} 

function ProductsView({ data }: { data: Product[] }) {
   return <ul>{data.map(p => <li key={p.id}>{p.name}</li>)}</ul>; 
} 

function ProductsContainer() {
   const { data = [] } = useProducts();
   return <ProductsView data={data} />; 
}

```

## Custom Hook per incapsulare logica riusabile

``` tsx
function useLocalStorage<T>(key: string, init: T) {
   const [value, setValue] = React.useState<T>(() => {
     const raw = localStorage.getItem(key);
     return raw ? JSON.parse(raw) as T : init;  
	});  
	React.useEffect(() => { localStorage.setItem(key, JSON.stringify(value)); }, [key, value]);   
	return [value, setValue] as const; 
}
```

---

# Accessibilità e UX

- Usa elementi semantici (`<button>`, `<label>`, `<nav>`).
- Gestisci focus e **aria-attributes** (screen reader).
- Mantieni colori/contrasti adeguati; non affidarti solo al colore per lo stato.

---

# Quando scegliere cosa

- **SPA classica**: Vite + React Router + React Query/Zustand.
- **SEO/SSR, performance globale, RSC**: **Next.js** (App Router).
- **Stato complesso multi-dominio**: Redux Toolkit (+ RTK Query per dati remoti).

---

# Checklist rapida per componenti “solidi”

1. Prop tipate con TypeScript (evita `any`).
2. Stato minimo e localizzato.
3. Effetti puliti e idempotenti (dipendenze corrette).
4. Chiavi stabili nelle liste.
5. `memo/useMemo/useCallback` solo quando servono.
6. Error boundary dove il fallimento è plausibile.
7. Suspense/lazy per carichi lenti.
8. Test RTL su casi d’uso principali.

---

## Mini-esempio completo (filtri + transition + memo)

``` tsx
type Product = { id: string; name: string; price: number };  

function Catalog({ products }: { products: Product[] }) {
   const [query, setQuery] = React.useState('');
   const [isPending, startTransition] = React.useTransition();
       
   const filtered = React.useMemo(() => {
       const q = query.toLowerCase();     
       return products.filter(p => p.name.toLowerCase().includes(q));
   }, [products, query]);    
   function onChange(e: React.ChangeEvent<HTMLInputElement>) {
       const q = e.target.value;     
       startTransition(() => setQuery(q)); // UI resta reattiva durante il filtro   
   }    
   
   return (
       <div>       
	       <input placeholder="Cerca…" onChange={onChange} />       
	       {isPending && <span>Filtrando…</span>}
		   <ul>         
			   {filtered.map(p => <li key={p.id}>{p.name} – €{p.price}</li>)}      
		   </ul>     
	   </div>   
   ); 
}
```



