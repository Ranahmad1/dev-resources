# PHP & Laravel Cheatsheet

## PHP Basics

### Variables & Types
```php
$name   = "Ahmad";   // string
$age    = 20;        // int
$price  = 99.9;      // float
$active = true;      // bool
$data   = null;      // null
$arr    = [1,2,3];   // array

// Type hints (PHP 7+)
function add(int $a, int $b): int { return $a + $b; }

// Type juggling
$x = "5" + 3;        // 8 (int)
$x = "5" . 3;        // "53" (string concat)

// Null coalescing (PHP 7+)
$name = $_GET['name'] ?? 'Guest';
$name ??= 'Guest';  // assign if null

// Strict types
declare(strict_types=1);
```

### Strings
```php
$s = "Hello, World!";

strlen($s);                // 13
strtoupper($s);
strtolower($s);
trim($s); ltrim(); rtrim();
str_replace("World","PHP",$s);
substr($s, 7, 5);          // "World"
strpos($s, "World");       // 7 (false if not found)
str_contains($s,"World");  // true (PHP 8)
str_starts_with($s,"Hello");// true (PHP 8)
str_ends_with($s,"!");      // true (PHP 8)
str_repeat("ab", 3);       // "ababab"
number_format(1234567.89, 2, '.', ','); // "1,234,567.89"
sprintf("Hello %s, you are %d", $name, $age);
explode(",", "a,b,c");     // ["a","b","c"]
implode(", ", ["a","b"]);  // "a, b"
preg_match('/\d+/', $s, $m);  // regex match
preg_replace('/\s+/', '-', $s);

// Heredoc
$text = <<<EOT
Line 1: $name
Line 2
EOT;

// Nowdoc (no interpolation)
$text = <<<'EOT'
Literal $name here
EOT;
```

### Arrays
```php
// Indexed
$fruits = ["apple", "banana", "mango"];
$fruits[] = "grape";                  // append
array_push($fruits, "kiwi");          // append (returns new count)
array_pop($fruits);                   // remove last
array_shift($fruits);                 // remove first
array_unshift($fruits, "pear");       // prepend
count($fruits);
in_array("apple", $fruits);           // true
array_search("banana", $fruits);      // index or false
sort($fruits);
rsort($fruits);
array_reverse($fruits);
array_slice($fruits, 1, 2);
array_splice($fruits, 1, 1, ["cherry"]); // replace
array_merge($a, $b);
array_unique($fruits);
array_flip($fruits);                  // swap keys & values

// Associative
$person = [
    "name" => "Ahmad",
    "age"  => 20,
    "city" => "Faisalabad"
];
$person["role"] = "admin";
unset($person["city"]);
array_key_exists("name", $person);
array_keys($person);
array_values($person);
ksort($person);   // sort by key
asort($person);   // sort by value, keep keys

// Functional
array_map(fn($x) => $x * 2, $nums);
array_filter($nums, fn($x) => $x > 2);
array_reduce($nums, fn($carry,$x) => $carry+$x, 0);
usort($arr, fn($a,$b) => $a['age'] <=> $b['age']); // spaceship

// Spread
$merged = [...$arr1, ...$arr2];
```

### Control Flow
```php
// if
if ($age >= 18) {
    echo "Adult";
} elseif ($age >= 13) {
    echo "Teen";
} else {
    echo "Child";
}

// Ternary & null coalescing
$label  = $active ? "Active" : "Inactive";
$name   = $_GET['name'] ?? "Guest";

// Match (PHP 8)
$label = match($role) {
    "admin"  => "Administrator",
    "editor" => "Content Editor",
    "user"   => "Regular User",
    default  => "Guest"
};

// Loops
foreach ($fruits as $fruit) { echo $fruit; }
foreach ($person as $key => $value) { echo "$key: $value"; }
for ($i = 0; $i < 10; $i++) { echo $i; }
while ($condition) { /* ... */ }
do { /* ... */ } while ($condition);
```

### OOP (PHP 8)
```php
class User {
    public static int $count = 0;

    public function __construct(
        private string $name,      // promoted properties
        private string $email,
        protected int  $age = 0,
        public readonly string $role = 'user'  // readonly (PHP 8.1)
    ) {
        self::$count++;
    }

    public function getName(): string { return $this->name; }

    public static function getCount(): int { return self::$count; }

    public function __toString(): string { return $this->name; }
}

// Interface
interface Authenticatable {
    public function login(): bool;
    public function logout(): void;
}

// Abstract
abstract class BaseModel {
    abstract public function validate(): bool;

    public function save(): bool {
        return $this->validate();
    }
}

// Traits
trait Timestamps {
    public function createdAt(): string {
        return date('Y-m-d H:i:s');
    }
}

class Admin extends User implements Authenticatable {
    use Timestamps;

    public function login(): bool { return true; }
    public function logout(): void { }

    public function ban(User $user): string {
        return "{$user->getName()} has been banned.";
    }
}

// Enum (PHP 8.1)
enum Status: string {
    case Active   = 'active';
    case Inactive = 'inactive';
    case Banned   = 'banned';
}

$s = Status::Active;
$s->value;   // 'active'
$s->name;    // 'Active'
Status::from('active');       // Status::Active
Status::tryFrom('invalid');   // null
```

---

## Laravel

### Artisan Commands
```bash
# Make
php artisan make:model     Post -mfsc  # model + migration + factory + seeder + controller
php artisan make:controller PostController --resource
php artisan make:request    StorePostRequest
php artisan make:middleware EnsureAdmin
php artisan make:policy     PostPolicy --model=Post
php artisan make:event      PostPublished
php artisan make:listener   SendPublishNotification --event=PostPublished
php artisan make:job        ProcessImage --queue
php artisan make:mail       WelcomeMail --markdown=emails.welcome
php artisan make:notification OrderShipped
php artisan make:command    SendDailyReport

# Database
php artisan migrate
php artisan migrate:rollback
php artisan migrate:fresh --seed
php artisan db:seed --class=UserSeeder
php artisan tinker

# Cache / Optimize
php artisan cache:clear
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan optimize
php artisan optimize:clear

# Other
php artisan route:list --columns=method,uri,name,action
php artisan serve --port=8000
php artisan queue:work --tries=3 --timeout=60
php artisan schedule:run
```

### Routes
```php
use App\Http\Controllers\PostController;

// Basic
Route::get('/posts',         [PostController::class, 'index'])->name('posts.index');
Route::post('/posts',        [PostController::class, 'store'])->name('posts.store');
Route::get('/posts/{post}',  [PostController::class, 'show'])->name('posts.show');
Route::put('/posts/{post}',  [PostController::class, 'update']);
Route::delete('/posts/{post}',[PostController::class, 'destroy']);

// Resource
Route::resource('posts', PostController::class);
Route::apiResource('posts', PostController::class);   // no create/edit

// Constraints
Route::get('/users/{id}', ...)->where('id', '[0-9]+');

// Middleware groups
Route::middleware(['auth', 'verified'])->group(function () {
    Route::resource('posts',    PostController::class);
    Route::resource('comments', CommentController::class)->only(['store','destroy']);
});

// Prefix + name prefix
Route::prefix('admin')->name('admin.')->middleware('admin')->group(function () {
    Route::get('/dashboard', [AdminController::class, 'index'])->name('dashboard');
    Route::resource('users', AdminUserController::class);
});

// Fallback
Route::fallback(fn() => response()->json(['message'=>'Not Found'],404));

// Helpers
route('posts.show', $post);        // URL
route('posts.show', ['id'=>1]);
redirect()->route('posts.index');
URL::signedRoute('verify', ['id'=>1]);
```

### Eloquent ORM
```php
// Retrieve
Post::all();
Post::find(1);
Post::findOrFail(1);
Post::findMany([1,2,3]);
Post::first();
Post::firstOrFail();
Post::firstOrCreate(['email'=>$email], ['name'=>$name]);
Post::firstOrNew(['email'=>$email]);
Post::updateOrCreate(['email'=>$email], ['name'=>$name]);

// Query
Post::where('status', 'published')
    ->where('user_id', auth()->id())
    ->orderByDesc('created_at')
    ->limit(10)
    ->get();

Post::whereIn('status', ['draft','published'])->get();
Post::whereBetween('created_at', [$start, $end])->get();
Post::whereNull('published_at')->get();
Post::whereHas('comments', fn($q) => $q->where('approved', true))->get();
Post::withCount('comments')->get();  // $post->comments_count

// Scopes
Post::published()->latest()->paginate(15);  // $post->scopePublished

// Pagination
$posts = Post::paginate(15);            // {{ $posts->links() }}
$posts = Post::simplePaginate(15);
$posts = Post::cursorPaginate(15);

// Create / Update / Delete
$post = Post::create($request->validated());
$post->update(['title' => $newTitle]);
$post->delete();
Post::destroy([1,2,3]);
Post::where('expired', true)->delete();

// Soft deletes (add SoftDeletes trait)
Post::withTrashed()->get();
Post::onlyTrashed()->get();
$post->restore();
$post->forceDelete();

// Relationships
$user->posts;                        // hasMany
$user->posts()->latest()->get();     // query relationship
$post->user;                         // belongsTo
$post->tags;                         // belongsToMany
$post->tags()->attach([1,2]);
$post->tags()->sync([1,3,5]);
$user->profile;                      // hasOne
$post->latestComment;                // hasOne with constraints

// Eager loading — prevent N+1
Post::with('user', 'tags', 'comments.user')->get();
Post::with(['comments' => fn($q) => $q->approved()])->get();
```

### Request & Validation
```php
public function store(Request $request): RedirectResponse
{
    $validated = $request->validate([
        'title'       => ['required', 'string', 'max:255'],
        'body'        => ['required', 'string'],
        'category_id' => ['required', 'exists:categories,id'],
        'tags'        => ['array'],
        'tags.*'      => ['exists:tags,id'],
        'image'       => ['nullable', 'image', 'max:2048'],
        'slug'        => ['required', 'unique:posts,slug'],
        'status'      => ['in:draft,published'],
    ]);

    // File upload
    if ($request->hasFile('image')) {
        $validated['image'] = $request->file('image')
            ->store('posts', 'public');
    }

    $post = auth()->user()->posts()->create($validated);

    return redirect()->route('posts.show', $post)
        ->with('success', 'Post created!');
}

// Request helpers
$request->all();
$request->only(['title','body']);
$request->except(['_token']);
$request->input('title', 'default');
$request->boolean('is_active');    // truthy check
$request->integer('page', 1);
$request->filled('name');          // not null AND not empty
$request->missing('token');
$request->file('avatar');          // UploadedFile
$request->ip();
$request->userAgent();
$request->header('Authorization');
$request->bearerToken();
```

### Blade Templates
```blade
{{-- Comment --}}
{{ $variable }}         {{-- echo, escaped --}}
{!! $html !!}           {{-- echo, unescaped ⚠️ --}}
{{ $var ?? 'default' }}

@if ($user->isAdmin())
    <p>Admin panel</p>
@elseif ($user->isEditor())
    <p>Editor tools</p>
@else
    <p>User view</p>
@endif

@unless ($user->banned) ... @endunless
@isset($var) ... @endisset
@empty($items) ... @endempty

@foreach ($posts as $post)
    <p>{{ $loop->iteration }}/{{ $loop->count }}: {{ $post->title }}</p>
    @if ($loop->last) <hr> @endif
@endforeach

@forelse ($posts as $post)
    <p>{{ $post->title }}</p>
@empty
    <p>No posts yet.</p>
@endforelse

@for ($i=0; $i<5; $i++) {{ $i }} @endfor
@while ($condition) ... @endwhile

{{-- Layout --}}
@extends('layouts.app')
@section('title', 'Page Title')       {{-- short form --}}
@section('content')
    <h1>Content</h1>
    @parent   {{-- append to parent section --}}
@endsection
@yield('content')
@yield('sidebar', 'Default sidebar')

{{-- Components --}}
<x-card :title="$post->title" :active="true">
    <x-slot:footer>Card footer</x-slot:footer>
    Card body here
</x-card>

{{-- Includes --}}
@include('partials.navbar')
@includeWhen($user->isAdmin(), 'admin.sidebar')
@includeFirst(['custom.header','default.header'])

{{-- Form helpers --}}
<form method="POST" action="{{ route('posts.store') }}">
    @csrf
    @method('PUT')   {{-- spoofing: PUT/PATCH/DELETE --}}
</form>

{{-- Validation errors --}}
@error('email')
    <p class="error">{{ $message }}</p>
@enderror

{{-- Auth --}}
@auth    ... @endauth
@guest   ... @endguest
@can('edit-post', $post) ... @endcan
```
