

``` css
CLIENT (HTTP/JSON)
     ↓
ROUTES/api.php
     ↓
MIDDLEWARE (auth, cors, throttle)
     ↓
FORM REQUEST (validazione input)
     ↓
CONTROLLER (logica API)
     ↓
MODEL (interazione DB)
     ↓
RESOURCE (formattazione output)
     ↓
RISPOSTA JSON (status code + headers)
```

- `**Il client** (browser, Postman, app mobile) invia una richiesta HTTP JSON:`
    ```{ "title": "Compilare report", "completed": false }`

- **Route** intercetta l’endpoint:
    `Route::post('/tasks', [TaskController::class, 'store']);`

- Laravel crea un’istanza di **Form Request** (`StoreTaskRequest`)
    - valida input
    - autorizza o blocca
    - restituisce `422` se fallisce

- Il **Controller** riceve solo dati validi:
    `$task = $request->user()->tasks()->create($request->validated());`

- Il **Model** `Task` gestisce la logica DB tramite Eloquent ORM.
- Il **Resource** (`TaskResource`) trasforma il modello in JSON leggibile:
    `{ "id": 1, "title": "Compilare report", "completed": false }`

- Laravel aggiunge header HTTP standard (`Content-Type`, `Status`) e invia la risposta.


## Rotte (`routes/api.php`)


- punto di ingresso API
- **dedicato alle API stateless**, quindi:
	- usa di default il **middleware `api`** (gestisce CORS, rate limiting, serializzazione JSON);
	- le rotte non hanno sessione o cookie come nel web;
	- restituiscono risposte JSON per impostazione predefinita.

In pratica, è il punto in cui definisci _quali endpoint REST esistono_ e _a quali controller vengono instradati_.


``` php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\Api\V1\{
    AuthController,
    TaskController,
    ReportController
};

Route::prefix('v1')->group(function () {

    // Auth endpoints
    Route::post('/auth/login',  [AuthController::class, 'login']);
    Route::post('/auth/logout', [AuthController::class, 'logout'])
        ->middleware('auth:sanctum');

    // Protette da autenticazione
    Route::middleware(['auth:sanctum', 'throttle:60,1'])->group(function () {
        Route::apiResource('tasks', TaskController::class);
        Route::get('reports/daily', [ReportController::class, 'daily']);
    });
});

```

- `prefix('v1')` → versioning (utile per compatibilità futura)
- `auth:sanctum` → middleware che richiede token valido
- `apiResource` → genera automaticamente 5 rotte RESTful:
    - `GET /tasks` → index
    - `GET /tasks/{id}` → show
    - `POST /tasks` → store
    - `PUT/PATCH /tasks/{id}` → update
    - `DELETE /tasks/{id}` → destroy

## Controller 
## `app/Http/Controllers/Api/V1/TaskController.php`

**Logica** che gestisce la richiesta e chiama i modelli.

Ogni metodo corrisponde a un’operazione REST:
``` php
class TaskController extends Controller
{
    public function index(Request $request)    // GET /tasks
    public function store(StoreTaskRequest $r)  // POST /tasks
    public function show(Task $task)            // GET /tasks/{id}
    public function update(UpdateTaskRequest $r, Task $task) // PUT/PATCH
    public function destroy(Task $task)         // DELETE
}
```

## Model (`app/Models/Task.php`)

Rappresenta la **tabella del database** e incapsula la logica dei dati.

``` php
class Task extends Model {     protected $fillable = ['title','completed','due_at'];     protected $casts = ['completed'=>'bool','due_at'=>'datetime']; }
```

- `$fillable` → protegge dai mass assignment (sicurezza)
- `$casts` → converte tipi nativi (`bool`, `datetime`)
- può avere **relazioni**:

``` php
    public function user() {
	     return $this->belongsTo(User::class); 
	 }
	 ```


## Form Request (`app/Http/Requests/Api/V1/StoreTaskRequest.php`)

Gestisce **validazione e autorizzazione input**.

``` php
public function rules(): array {
     return [
		  'title'     => ['required','string','max:255'],
		 'completed' => ['sometimes','boolean'],         
		 'due_at'    => ['nullable','date'],     
	 ]; 
 }
 ```

Laravel blocca automaticamente i dati errati con status `422`.

## Resource (`app/Http/Resources/Api/V1/TaskResource.php`)

Gestisce **l’output JSON** verso il client.

``` php
public function toArray($request): array {     
	return [         
		'id'=> $this->id,         
		'title'     => $this->title,         
		'completed' => $this->completed,         
		'dueAt'     => optional($this->due_at)?->toIso8601String(),     
	]; 
}
```


Puoi usarla:

``` php
return new TaskResource($task);// singolo record 
// oppure 
return TaskResource::collection(Task::paginate());
```



## Middleware

I **middleware** filtrano le richieste prima del controller.

Esempi:
- `auth:sanctum` → verifica token
- `throttle:api` → limita richieste al secondo
- `cors` → abilita accesso cross-domain

Puoi anche crearne di personalizzati:

``` bash
php artisan make:middleware LogRequestTime
```


## Versionamento API (`v1`, `v2`, …)

Quando un client dipende da una versione stabile, puoi introdurre una nuova API senza rompere le vecchie:

```php 
app/Http/Controllers/Api/V1/TaskController.php app/Http/Controllers/Api/V2/TaskController.php
```

In `routes/api.php`:

``` php 
Route::prefix('v1')->group(...); Route::prefix('v2')->group(...);
```


