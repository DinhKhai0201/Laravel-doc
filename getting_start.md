# Install laravel
## Via Laravel Installer
`composer global require laravel/installer`
## Tạo mới một project
`laravel new blog`
## Chạy composer và npm để cài đặt các gói cần thiết trong dự án
    composer install
    npm install
## Tạo database và config database
Vào Phpmyadmin để tạo một database mới, ví dụ tên database được tạo là blog_laravel
Sau đó mở terminal,ta thực hiện lệnh sau để copy ra file env:
`cp .env.example .env`
Tiếp theo cập nhật file env:
    DB_CONNECTION=mysql          
    DB_HOST=127.0.0.1            
    DB_PORT=3306                 
    DB_DATABASE=blog_laravel     
    DB_USERNAME={your username login to phpmyadmin}        
    DB_PASSWORD={your password login to phpmyadmin}
## Tạo ra key cho dự án
`php artisan key:generate`
## Tạo ra các bảng và dữ liệu mẫu cho database
    php artisan migrate
    php artisan db:seed
## public thư mục lưu trữ 
Sau khi cài dự án bạn phải public thư mục lưu trữ của bạn khi người dùng upload ảnh.
`php artisan storage:link`
## Local Development Server
`php artisan serve`
