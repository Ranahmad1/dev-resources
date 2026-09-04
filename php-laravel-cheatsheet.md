# PHP & Laravel Cheatsheet

## PHP Basics

### Variables & Types
```php
$name = "Ahmad";        // string
$age = 20;              // int
$price = 99.9;          // float
$active = true;         // bool
$nothing = null;        // null

// Type checking
gettype($age);          // "integer"
is_string($name);       // true
is_numeric("42");        // true

// Type casting
(int) "42";
(string) 42;
(bool) 0;               // false
```

### Strings
```php
$s = "Hello World";
strlen($s);             // 11
strtoupper($s);         // "HELLO WORLD"
strtolower($s);         // "hello world"
str_replace("World", "PHP", $s);
substr($s, 0, 5);       // "Hello"
strpos($s, "World");    // 6
trim($s);               // Remove whitespace
explode(" ", $s);       // ["Hello", "World"]
implode(", ", $arr);    // Join array

// Heredoc
$text = <<<EOT
Line 1
Line 2
EOT;

// String interpolation
$msg = "Hello, $name!";
$msg = "Hello, {$person['name']}!";
```

### Arrays
```php
// Indexed
$fruits = ["apple", "banana", "mango"];
$fruits[] = "grape";            // Append
array_push($fruits, "kiwi");    // Append
array_pop($fruits);             // Remove last
array_shift($fruits);           // Remove first
array_unshift($fruits, "pear"); // Add to start
count($fruits);                 // Length
in_array("apple", $fruits);     // true
array_search("banana", $fruits);// 1 (index)
sort($fruits);                  // Sort ascending
rsort($fruits);                 // Sort descending
array_reverse($fruits);
array_slice($fruits, 1, 2);     // Sub-array
array_merge($arr1, $arr2);

// Associative
$person = [
    "name" => "Ahmad",
    "age" => 20,
    "city" => "Faisalabad"
];
$person["name"];                // "Ahmad"
$person["email"] = "a@b.com"; // Add key
unset($person["city"]);         // Remove key
array_key_exists("name", $person); // true
array_keys($person);            // ["name","age"]
array_values($person);          // ["Ahmad",20]

// Functional
array_map(fn($x) => $x * 2, $nums);
array_filter($nums, fn($x) => $x > 2);
array_reduce($nums, fn($carry, $x) => $carry + $x, 0);
```

### Control Flow
```php
// If
if ($age >= 18) {
    echo "Adult";
} elseif ($age >= 13) {
    echo "Teen";
} else {
    echo "Child";
}

// Ternary & Null coalescing
$status = $active ? "Active" : "Inactive";
$name = $_GET['name'] ?? "Guest";  // Null coalescing

// Match (PHP 8+)
$result = match($status) {
    "active" => "User is active",
    "banned" => "User is banned",
    default => "Unknown"
};

// Loops
foreach ($fruits as $fruit) {
    echo $fruit;
}

foreach ($person as $key => $value) {
    echo "$key: $value";
}

for ($i = 0; $i < 10; $i++) {
    echo $i;
}

while ($condition) {
    // ...
}
```

### Functions
```php
function greet(string $name, string $greeting = "Hello"): string {
    return "$greeting, $name!";
}

// Anonymous function
$square = function($n) { return $n ** 2; };

// Arrow function (PHP 7.4+)
$square = fn($n) => $n ** 2;

// Spread operator
function sum(int ...$nums): int {
    return array_sum($nums);
}
echo sum(1, 2, 3, 4);  // 10
```

### OOP
```php
class User {
    private string $name;
    protected int $age;
    public static int $count = 0;

    public function __construct(string $name, int $age) {
        $this->name = $name;
        $this->age = $age;
        self::$count++;
    }

    public function getName(): string {
        return $this->name;
    }

    public static function getCount(): int {
        return self::$count;
    }
}

// Inheritance
class Admin extends User {
    public function __construct(string $name, int $age) {
        parent::__construct($name, $age);
    }

    public function ban(): string {
        return "{$this->getName()} banned!";
    }
}

$user = new User("Ahmad", 20);
echo $user->getName();
echo User::$count;
```

---

## Laravel Cheatsheet

### Artisan Commands
```bash
# Create
php artisan make:controller UserController
php artisan make:controller UserController --resource  # CRUD
php artisan make:model User -m                         # Model + migration
php artisan make:migration create_posts_table
php artisan make:request StoreUserRequest
php artisan make:middleware AuthMiddleware
php artisan make:seeder UserSeeder
php artisan make:factory UserFactory
php artisan make:command SendEmails
php artisan make:mail WelcomeMail
php artisan make:job ProcessPayment

# Database
php artisan migrate                # Run migrations
php artisan migrate:rollback       # Undo last migration
php artisan migrate:fresh          # Drop all + migrate
php artisan migrate:fresh --seed   # Fresh + seed
php artisan db:seed                # Run seeders
php artisan db:seed --class=UserSeeder

# Cache
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
php artisan optimize

# Utility
php artisan route:list             # View all routes
php artisan tinker                 # Interactive REPL
php artisan serve                  # Start dev server
php artisan queue:work             # Process queue jobs
```

### Routes (web.php / api.php)
```php
use App\Http\Controllers\UserController;

// Basic
Route::get('/users', [UserController::class, 'index']);
Route::post('/users', [UserController::class, 'store']);
Route::get('/users/{id}', [UserController::class, 'show']);
Route::put('/users/{id}', [UserController::class, 'update']);
Route::delete('/users/{id}', [UserController::class, 'destroy']);

// Resource (all CRUD at once)
Route::resource('users', UserController::class);
Route::apiResource('posts', PostController::class);

// Group with middleware
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
    Route::resource('posts', PostController::class);
});

// Named route
Route::get('/profile', [ProfileController::class, 'index'])->name('profile');
route('profile');   // Generate URL

// Route with prefix
Route::prefix('admin')->group(function () {
    Route::get('/users', [AdminController::class, 'users']);
});
```

### Eloquent ORM
```php
// All records
$users = User::all();

// Find by ID
$user = User::find(1);
$user = User::findOrFail(1);  // 404 if not found

// Where conditions
$users = User::where('active', true)->get();
$users = User::where('age', '>=', 18)
             ->where('city', 'Faisalabad')
             ->orderBy('name')
             ->limit(10)
             ->get();

// First
$user = User::where('email', $email)->first();
$user = User::where('email', $email)->firstOrFail();

// Create
$user = User::create(['name' => 'Ahmad', 'email' => 'a@b.com']);

// Update
$user->update(['age' => 21]);
User::where('id', 1)->update(['status' => 'active']);

// Delete
$user->delete();
User::destroy(1);
User::where('active', false)->delete();

// Count
$count = User::count();
$count = User::where('active', true)->count();

// Relationships
$posts = $user->posts;          // hasMany
$user = $post->user;            // belongsTo
$roles = $user->roles;          // belongsToMany

// Eager loading (prevent N+1)
$users = User::with('posts', 'profile')->get();
$posts = Post::with('user.profile')->get();
```

### Request & Validation
```php
// In Controller
public function store(Request $request) {
    $validated = $request->validate([
        'name'     => 'required|string|max:255',
        'email'    => 'required|email|unique:users,email',
        'age'      => 'required|integer|min:1|max:120',
        'password' => 'required|min:8|confirmed',
    ]);

    $user = User::create($validated);
    return redirect()->route('users.index')->with('success', 'User created!');
}

// Request data
$request->all();
$request->only(['name', 'email']);
$request->except(['password']);
$request->input('name', 'default');
$request->get('name');
$request->has('email');     // bool
$request->file('avatar');
$request->ip();
$request->method();         // "POST"
$request->isMethod('post'); // true
```

### Response & Redirects
```php
// Views
return view('users.index', compact('users'));
return view('users.show', ['user' => $user]);

// JSON (API)
return response()->json(['success' => true, 'data' => $users]);
return response()->json(['error' => 'Not found'], 404);

// Redirect
return redirect('/dashboard');
return redirect()->route('home');
return redirect()->back();
return redirect()->back()->withErrors($validator);
return redirect()->route('users.index')->with('success', 'Done!');

// In Blade: {{ session('success') }}
```

### Blade Templates
```blade
{{-- Comment --}}
{{ $variable }}           {{-- Echo (escaped) --}}
{!! $html !!}             {{-- Echo (unescaped, careful!) --}}

@if ($user->isAdmin())
    <p>Admin</p>
@elseif ($user->active)
    <p>Active User</p>
@else
    <p>Regular User</p>
@endif

@foreach ($users as $user)
    <p>{{ $user->name }}</p>
@endforeach

@forelse ($posts as $post)
    <p>{{ $post->title }}</p>
@empty
    <p>No posts found.</p>
@endforelse

@for ($i = 0; $i < 5; $i++)
    {{ $i }}
@endfor

{{-- Layouts --}}
@extends('layouts.app')
@section('content')
    <h1>Page Content</h1>
@endsection

{{-- In layout: --}}
@yield('content')

{{-- Components --}}
@include('partials.navbar')
@include('partials.alert', ['type' => 'success'])

{{-- CSRF (required in forms) --}}
<form method="POST" action="/users">
    @csrf
    @method('PUT')  {{-- For PUT/PATCH/DELETE --}}
</form>

{{-- Errors --}}
@error('email')
    <span class="error">{{ $message }}</span>
@enderror
```
