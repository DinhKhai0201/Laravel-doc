# Directory Structure
## The Root Directory
### The App Directory
Thư mục `app` chứa core của ứng dụng.

### The Bootstrap Directory
Thư mục `bootstrap` chứa tệp app.php khởi động dự án. 

Thư mục này cũng chứa một thư mục `cache` chứa các tệp được tạo  để tối ưu hóa hiệu suất, chẳng hạn như `cache` của `route` và `services`.

### The Config Directory
Chứa tất cả các file config của ứng dụng

### The Database Directory
Chứa migrations, model factories, and seeds.

### The Public Directory
Chứa index,images, JavaScript, and CSS.

### The Resources Directory
Chứa language file, LESS, SASS, or JavaScript

### The Routes Directory
Chứa các `route` được định nghĩa. 
Gồm : 

    web.php
    api.php
    console.php
    channels.php

### The Storage Directory
Gồm app, framework, and logs;

 - App : chứa các file được tạo ra từ dự án
 - framework: chứa framework generated files and caches
 - logs : chứa application's log files

`storage/app/public ` chứa các file như avata, photo upload

### The Tests Directory
Chứa các file test

### The Vendor Directory

Chứa các Composer dependencies.

## The App Directory
Phần lớn bạn sẽ làm việc tại thư mục này

### The Broadcasting Directory
Chứa tất cả broadcast channel classes trong dự án
Ban đầu, thư mục này sẽ không tồn tại, bạn cần tạo khi muốn tạo chanel
`php artisan make:channel`

### The Console Directory
Chứa các custom Artisan commands 

`php artisan make:command`

### The Events Directory
Chứa các cảnh báo, sự kiện khi có actions được gọi
Ban đầu, thư mục này sẽ không tồn tại, bạn cần tạo khi muốn tạo evant
`event:generate ` và `make:event `
### The Exceptions Directory
Chứa các file xử lý ngoại lệ

### The Http Directory
 - controllers
 - middleware
 - requests

### The Jobs Directory
Sắp xếp các công việc theo hàng đợi cho ứng dụng

`make:job`
### The Listeners Directory
### The Mail Directory
### The Notifications Directory
### The Policies Directory
### The Providers Directory
### The Rules Directory

## Lifecycle
1. Tất cả các request gọi tới ứng dụng laravel đều chạy qua file public/index.php. File này load các phần của framework và tạo một instance của ứng dụng laravel.

2. Request sẽ chạy qua HTTP kernel hoặc Console kernel, HTTP kernel định nghĩa mảng Bootstrapper và HTTP middleware quản lý các cấu hình trước khi request được xử lý bởi ứng dụng.

3. Service providers sẽ chịu trách nhiệm khởi động tất cả các components khác nhau của framework.

4. Khi service providers được registered, request sẽ được gửi tới các route.