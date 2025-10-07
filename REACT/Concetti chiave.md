## 1. **Componenti e JSX**

### Concetto

React è basato su **componenti riutilizzabili** scritti con sintassi JSX (JavaScript + XML).  
Ogni componente gestisce una parte dell’interfaccia.

###  Esempio

`function Hello({ name }) {   return <h2>Ciao {name}!</h2>; }`

 I componenti possono essere:
- **Funzionali** (moderni, con hook)
- **Classici** (più rari oggi)

---

## 2. **Hooks fondamentali**

### Concetto

Gli Hook sostituiscono le classi e permettono di gestire **stato e ciclo di vita** in modo funzionale.

|Hook|Funzione|Esempio|
|---|---|---|
|`useState()`|Stato locale|`const [count, setCount] = useState(0)`|
|`useEffect()`|Effetti collaterali|`useEffect(() => fetchData(), [])`|
|`useRef()`|Accesso diretto al DOM|`ref.current.focus()`|
|`useMemo()`|Computazione memorizzata|`useMemo(() => calc(a,b), [a,b])`|

---

## 3. **Props e State**

### Concetto

- **Props** = dati che un componente riceve dal genitore
- **State** = dati interni che cambiano nel tempo

### Esempio

`function Counter({ step }) {   const [count, setCount] = useState(0);   return (     <button onClick={() => setCount(count + step)}>       {count}     </button>   ); }`

 Props = `step`, State = `count`.

---

## 4. **Gestione API e Ciclo di vita**

### Concetto

React usa `useEffect` per eseguire codice al montaggio del componente — ideale per fetch API.

### Esempio

`useEffect(() => {   axios.get("/api/users").then(res => setUsers(res.data)); }, []);`

 Concetti chiave:
- Chiamate asincrone (`async/await`)
- Stato di caricamento (`loading`, `error`, `data`)
- Cleanup con `return () => ...` per liberare risorse

---

## 5. **Routing e Gestione Stato Globale**

### Concetto

React Router gestisce la navigazione _client-side_, mentre Context o Redux gestiscono lo stato condiviso.

### Esempio Routing

`<BrowserRouter>   <Routes>     <Route path="/" element={<Home />} />     <Route path="/users/:id" element={<User />} />   </Routes> </BrowserRouter>`

### Esempio Stato Globale

`const AppContext = createContext();  function App() {   const [user, setUser] = useState(null);   return (     <AppContext.Provider value={{ user, setUser }}>       <Profile />     </AppContext.Provider>   ); }`