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
 - Tạo mới 1 controller: 

    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use App\User;

    class UserController extends Controller
    {
        /**
        * Show the profile for the given user.
        *
        * @param  int  $id
        * @return View
        */
        public function show($id)
        {
            return view('user.profile', ['user' => User::findOrFail($id)]);
        }
    }

 - Sau đó gọi từ route :
`Route::get('user/{id}', 'UserController@show');`

 - Nếu tạọ 1 nest controller : `App\Http\Controllers\Photos\AdminController`
 thì tại route sẽ thế này : `Route::get('foo', 'Photos\AdminController@method');`

 ## Single Action Controllers
  - Nếu trong controller chỉ xử lý 1 acction duy nhất, thì ta sử dụng hàm `__invoke`:
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use App\User;

    class ShowProfile extends Controller
    {
        /**
        * Show the profile for the given user.
        *
        * @param  int  $id
        * @return View
        */
        public function __invoke($id)
        {
            return view('user.profile', ['user' => User::findOrFail($id)]);
        }
    }

`Route::get('user/{id}', 'ShowProfile');`
 - cmd tạo --invoke : `php artisan make:controller ShowProfile --invokable`

 ## Controller Middleware
  - Sử dụng midleware tại route : `Route::get('profile', 'UserController@show')->middleware('auth');`
   - Hoẵc sử dụng trong controller : 

    class UserController extends Controller
    {
        /**
        * Instantiate a new controller instance.
        *
        * @return void
        */
        public function __construct()
        {
            $this->middleware('auth');

            $this->middleware('log')->only('index');

            $this->middleware('subscribed')->except('store');
        }
    }
    

## Resource Controllers
`php artisan make:controller PhotoController --resource`
 - Giúp code gọn hơn, không cần phải ghi nhiều method crud
    class PhotoController extends Controller
    {
        /**
        * Display a listing of the resource.
        *
        * @return \Illuminate\Http\Response
        */
        public function index()
        {
            //
        }

        /**
        * Show the form for creating a new resource.
        *
        * @return \Illuminate\Http\Response
        */
        public function create()
        {
            //
        }

        /**
        * Store a newly created resource in storage.
        *
        * @param  \Illuminate\Http\Request  $request
        * @return \Illuminate\Http\Response
        */
        public function store(Request $request)
        {
            //
        }

        /**
        * Display the specified resource.
        *
        * @param  int  $id
        * @return \Illuminate\Http\Response
        */
        public function show($id)
        {
            //
        }

        /**
        * Show the form for editing the specified resource.
        *
        * @param  int  $id
        * @return \Illuminate\Http\Response
        */
        public function edit($id)
        {
            //
        }

        /**
        * Update the specified resource in storage.
        *
        * @param  \Illuminate\Http\Request  $request
        * @param  int  $id
        * @return \Illuminate\Http\Response
        */
        public function update(Request $request, $id)
        {
            //
        }

        /**
        * Remove the specified resource from storage.
        *
        * @param  int  $id
        * @return \Illuminate\Http\Response
        */
        public function destroy($id)
        {
            //
        }
    }

 - Tiếp theo bạn khai báo route cho controller
 `Route::resource('photos', 'PhotoController');`
 - Chỉ với 1 dòng khai báo như này, là bạn đã khai báo cho tất cả các action trong PhotoController.
 - Khai báo cho nhiều resource controller cùng 1 lúc:
    Route::resources([
        'photos' => 'PhotoController',
        'posts' => 'PostController'
    ]);
 - KHi chỉ muốn dùng một số action nào đó:
    Route::resource('photos', 'PhotoController')->only([
        'index', 'show'
    ]);

    Route::resource('photos', 'PhotoController')->except([
        'create', 'store', 'update', 'destroy'
    ]);
 - Các action
 | Verb  | URI  | Action  | Route  |
|---|---|---|---|
| GET  | 	/photos  | index  | photos.index  |
| GET  | /photos/create  |  create  | photos.create |
| POST  | 	/photos  | store  | photos.store  |
| GET  | /photos/{photo}  |  show  | photos.show |
| GET  | 		/photos/{photo}/edit  | edit  | photos.edit  |
| PUT/PATCH  | /photos/{photo}  |  update  | photos.update |
| DELETE  | /photos/{photo}  |  destroy  | photos.destroy |
### Spoofing Form Methods
 - DÙng cho các method put, patch ,delete 
    <form action="/foo/bar" method="POST">
        @method('PUT')
    </form>
### Cách nhận Request
 - Route : `Route::put('user/{id}', 'UserController@update');`
 - Controller : 
    <?php

    namespace App\Http\Controllers;

    use Illuminate\Http\Request;

    class UserController extends Controller
    {
        /**
        * Update the given user.
        *
        * @param  Request  $request
        * @param  string  $id
        * @return Response
        */
        public function update(Request $request, $id)
        {
            //
        }
    }

# HTTP Requests
## Accessing The Request
 - Sử dụng `Illuminate\Http\Request` method để nhận `request`
    <?php

    namespace App\Http\Controllers;

    use Illuminate\Http\Request;

    class UserController extends Controller
    {
        /**
        * Store a new user.
        *
        * @param  Request  $request
        * @return Response
        */
        public function store(Request $request)
        {
            $name = $request->input('name');

            //
        }
    }
## Retrieving The Request Path
`$uri = $request->path();`
    if ($request->is('admin/*')) {
        //
    }
## Retrieving The Request URL
    // Without Query String...
    $url = $request->url();

    // With Query String...
    $url = $request->fullUrl();
## Retrieving The Request Method
    $method = $request->method();

    if ($request->isMethod('post')) {
        //
    }
