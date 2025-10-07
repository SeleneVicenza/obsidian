## 1. **MVC (Model–View–Controller)**

### Concetto

Laravel segue l’architettura **MVC**, separando logica, presentazione e dati.

- **Model:** rappresenta una tabella (classe Eloquent)
    
- **View:** file Blade per la parte visiva
    
- **Controller:** gestisce la logica e coordina le chiamate
    

### Esempio

``` php
// routes/web.php 
Route::get('/users', [UserController::class, 'index']);  

// app/Http/Controllers/UserController.php 
public function index() {
   $users = User::all();   
   return view('users.index', compact('users')); }
```

 Separazione chiara e testabile tra livelli.

---

## 🔹 2. **[[Eloquent]] 
###  Concetto

È il sistema [[ORM]] di Laravel che mappa le tabelle del DB in classi PHP.  
Ogni modello rappresenta una tabella, e i record diventano oggetti manipolabili direttamente.

### Esempio

`$user = User::find(1); $user->email = 'new@mail.com'; $user->save();`

Relazioni:

``` php
class User extends Model {
   public function posts() {     
	   return $this->hasMany(Post::class);  
   } 
}

```


### Da ricordare

- Relazioni: `hasOne`, `hasMany`, `belongsTo`, `belongsToMany`
- Query fluide: `User::where('active', 1)->get()`
- Eager loading: `User::with('posts')->get()`

---

## 3. **Routing & Controller REST**

### Concetto

Laravel gestisce le rotte tramite file `routes/web.php` o `routes/api.php`.  
Ogni rotta può puntare a un Controller (anche resource-based).

### 🧩 Esempio

`Route::resource('products', ProductController::class);`

Crea automaticamente le rotte:
- `GET /products` → index
- `POST /products` → store
- `GET /products/{id}` → show
- `PUT /products/{id}` → update
- `DELETE /products/{id}` → destroy

Approccio RESTful standard, perfetto per API Vue/React.

---

##  4. **Middleware & Sicurezza**

### Concetto

Il middleware è un “filtro” tra richiesta e risposta HTTP.  
Serve per autenticazione, validazione token, rate limiting, ecc.

### Esempio

`Route::get('/profile', [UserController::class, 'show'])      ->middleware('auth');`

### Da ricordare

- Middleware globali in `app/Http/Kernel.php`
- Middleware personalizzati: `php artisan make:middleware CheckRole`
- Protezione CSRF automatica nei form Blade (`@csrf`)

---

## 5. **Migrazioni, Seeder e Artisan**

### Concetto

Laravel gestisce il DB in modo versionato e automatizzabile.  
Le **migrazioni** creano e modificano le tabelle, i **seeder** popolano dati di test, **Artisan** è la CLI che automatizza tutto.

### Esempio

`php artisan make:migration create_users_table php artisan migrate php artisan db:seed`

### Da ricordare

- Tutto è versionato e rollbackabile (`php artisan migrate:rollback`)
- Schema fluido:

``` php
Schema::create('users', function (Blueprint $table) {     
    $table->id();     
    $table->string('name');     
    $table->timestamps(); 
});
```

