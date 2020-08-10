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
