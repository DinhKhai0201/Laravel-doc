# Environment Configuration
Laravel sử dụng `.env` để lưu các giá trị Cấu hình cho dự án

## Environment Variable Types
| .env Value  | env() Value  |
|---|---|
| true  | (bool) true  |
| (true)  | (bool) true  | 
| false  | (bool) false  |
| (false)  | (bool) false  | 
| empty  | (string) ''  |
| (empty)  | (string) ''  | 
| null  | (null) null  |
| (null)  | (null) null  | 

Nếu định nghĩa một biến string có khoảng trắng , ta dùng dấu `" "`:

`APP_NAME="My Application"`

## Retrieving Environment Configuration

Các gía trị trong `.env` được liệt kê tại `$_ENV PHP super-global ` 

Trong các file config , biết `.env ` được gọi dưới dạng thế này

`'debug' => env('APP_DEBUG', false),`

Gía trị `false` phía sau được dùng khi biến `APP_DEBUG` không tồn tại trong file `.env`

## Xác định Current Environment

`$environment = App::environment();`

Bạn có thể sử dụng để kiếm tra các biến môi trường có match với giá trị trong ngoặc hay không, kết quả sẽ trả về `true` hoặc `false`

    if (App::environment('local')) {
        // The environment is local
    }
    
    if (App::environment(['local', 'staging'])) {
        // The environment is either local OR staging...
    }

## Accessing Configuration Values
Bạn có thể truy cập các giá trị trong config bằng hàm global `config` từ bất cứ nơi nào trong dự án

Các giá trị Configuration có thể được truy cập bằng cú pháp `dot`, bao gồm tên của tệp và tùy chọn bạn muốn truy cập.

`$value = config('app.timezone');`

## Configuration Caching
Để dự án chạy nhanh hơn, chúng ta cache tất cả Configuration files

`php artisan config:cache`

