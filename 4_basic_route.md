# Routing
 - routes trong Laravel chứa : 1 `url` và 1 hàm `closure` 

    Route::get('foo', function () {
        return 'Hello World';
    });
 - Cấu hình các route cho dự án nằm trong thư mục `routes`(các file routes tự động được load bởi framework)
 - `routes/web.php` định nghĩa route cho UI
 - `routes/api.php` định nghĩa route cho api
 - Ví dụ, chúng ta sẽ truy cập được đường link `http://your-app.test/user` khi định nghĩa route như dưới: 
`Route::get('/user', 'UserController@index');`

## Các methods

    Route::get($uri, $callback);
    Route::post($uri, $callback);
    Route::put($uri, $callback);
    Route::patch($uri, $callback);
    Route::delete($uri, $callback);
    Route::options($uri, $callback);

- Sử dụng match method:

    Route::match(['get', 'post'], '/', function () {
        //
    });

- Sử dụng any method(cho bất cứ method nào):

    Route::any('/', function () {
        //
    });

## CSRF Protection
Khi `Form html` gọi đến các methods post, put, delete thì cần thêm `CSRF token field`. Nếu không request sẽ bị từ chối

    <form method="POST" action="/profile">
        @csrf
        ...
    </form>
## Redirect Routes
`Route::redirect('/here', '/there');`
 - Mặc định, Route::redirect sẽ trả về status code là `302`(chuyển hướng tạm thời )
 - Custom status bằng cách : `Route::redirect('/here', '/there', 301);`

## View Routes
 - Dùng khi route chỉ trả về view

    Route::view('/welcome', 'welcome');
    Route::view('/welcome', 'welcome', ['name' => 'Taylor']);

## Parameters trong route
    Route::get('user/{id}', function ($id) {
        return 'User '.$id;
    });

 - Khi có nhiều parameters :

     Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
        //
    });
## Optional Parameters
 - Khi một Parameter có giá trị tùy chọn

    Route::get('user/{name?}', function ($name = 'John') {
        return $name;
    });
## Regular Expression Constraints
    Route::get('user/{name}', function ($name) {
        //
    })->where('name', '[A-Za-z]+');
    
    Route::get('user/{id}', function ($id) {
        //
    })->where('id', '[0-9]+');
    
    Route::get('user/{id}/{name}', function ($id, $name) {
        //
    })->where(['id' => '[0-9]+', 'name' => '[a-z]+']);

## Named Routes
    Route::get('user/profile', function () {
        //
    })->name('profile');

`Route::get('user/profile', 'UserProfileController@show')->name('profile');`

## Route Groups
 - Dùng để share route attributes khi mà số lượng route nhiều mà không cần định nghĩa trong mỗi route (middleware or namespaces)
### Middleware
    Route::middleware(['first', 'second'])->group(function () {
        Route::get('/', function () {
            // Uses first & second Middleware
        });
    
        Route::get('user/profile', function () {
            // Uses first & second Middleware
        });
    });
### Namespaces
    Route::namespace('Admin')->group(function () {
        // Controllers Within The "App\Http\Controllers\Admin" Namespace
    });

### Sub-Domain Routing
    Route::domain('{account}.myapp.com')->group(function () {
        Route::get('user/{id}', function ($account, $id) {
            //
        });
    });

## Route Prefixes
    Route::prefix('admin')->group(function () {
        Route::get('users', function () {
            // Matches The "/admin/users" URL
        });
    });
## Route Name Prefixes
    Route::name('admin.')->group(function () {
        Route::get('users', function () {
            // Route assigned name "admin.users"...
        })->name('users');
    });
## Route Model Binding (Eloquent models)
    Route::get('api/users/{user}', function (App\User $user) {
        return $user->email;
    });

## Fallback Routes
 - Được thực thi khi không có route naò match 
 - Được đặc ở dưới cùng của file route

    Route::fallback(function () {
        //
    });
## Form Method Spoofing
 - HTML forms không hổ trợ PUT, PATCH or DELETE actions.
 - Chúng ta cần thêm hidden _method field trong form. 

    <form action="/foo/bar" method="POST">
        <input type="hidden" name="_method" value="PUT">
        <input type="hidden" name="_token" value="{{ csrf_token() }}">
    </form>
 - Hoặc sử dụng  @method Blade để tạo _method input:
    <form action="/foo/bar" method="POST">
        @method('PUT')
        @csrf
    </form>

## Accessing The Current Route
    $route = Route::current();
    $name = Route::currentRouteName();
    $action = Route::currentRouteAction();
    
# Middleware
`php artisan make:middleware nameMiddleWare`
 - Lệnh command sẽ tạo 1 class `nameMiddleWare` tại thư mục `app/Http/Middleware`
    <?php
    namespace App\Http\Middleware;
    use Closure;
    class nameMiddleWare
    {
        /**
        * Handle an incoming request.
        *
        * @param  \Illuminate\Http\Request  $request
        * @param  \Closure  $next
        * @return mixed
        */
        public function handle($request, Closure $next)
        {
            if ($request->age <= 200) {
                return redirect('home');
            }
            return $next($request);
        }
    }
## Registering Middleware
### Global Middleware
If you want a middleware to run during every HTTP request to your application, list the middleware class in the $middleware property of your `app/Http/Kernel.php` class.
### Assigning Middleware To Routes
    // ĐỊnh nghĩa tên tại App\Http\Kernel Class...

    protected $routeMiddleware = [
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
        'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
    ];
 - Sau đó sử dụng trong route:
    Route::get('admin/profile', function () {
        //
    })->middleware('auth');
 - Sử dụng nhiều middleware: 
    Route::get('/', function () {
        //
    })->
 - Hoặc gọi trực tiếp :
    use App\Http\Middleware\CheckAge;

    Route::get('admin/profile', function () {
        //
    })->middleware(CheckAge::class);
 - Khi sử dụng middleware trong group như có route không sử dụng `middleware` đó:
    use App\Http\Middleware\CheckAge;

    Route::middleware([CheckAge::class])->group(function () {
        Route::get('/', function () {
            //
        });

        Route::get('admin/profile', function () {
            //
        })->withoutMiddleware([CheckAge::class]);
    });

# CSRF Protection (cross-site request forgery)
 - Các cuộc tấn công giả mạo là một kiểu khai thác độc hại theo đó các lệnh trái phép được thực hiện thay mặt cho người dùng đã được xác thực.
 - Laravel tự động tạo `token CSRF` cho mỗi session user đang hoạt động. Mã token này được sử dụng để xác minh rằng người dùng đã xác thực là người thực sự đưa ra yêu cầu đối với ứng dụng.
 - Mỗi lần tạo form, chúng ta cần khai báo csrf
    <form method="POST" action="/profile">
        @csrf
        ...
    </form>
 - Ví dụ cần thêm Uri để tránh bị csrf như stripe,.. Ta thêm các Uri tai `VerifyCsrfToken` Middleware
    <?php
    namespace App\Http\Middleware;
    use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as Middleware;
    class VerifyCsrfToken extends Middleware
    {
        /**
        * The URIs that should be excluded from CSRF verification.
        *
        * @var array
        */
        protected $except = [
            'stripe/*',
            'http://example.com/foo/bar',
            'http://example.com/foo/*',
        ];
    }
## X-CSRF-TOKEN
 - Khi sử dụng ajax 
 `<meta name="csrf-token" content="{{ csrf_token() }}">`
    $.ajaxSetup({
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
        }
    });

# Controllers (app/Http/Controllers)
Cmd : `php artisan make:controller myController`


