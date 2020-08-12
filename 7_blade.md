# Blade template (resources/views)

## kế thừa

    <!-- Lưu tại resources/views/layouts/master.blade.php -->
    
    <html>
        <head>
            <title>App Name - @yield('title')</title>
        </head>
        <body>
            @section('sidebar')
                <h3>Đây là sidebar chính.</h3>
            @show
    
            <div class="container">
                @yield('content')
            </div>
        </body>
    </html>

 - Khi định nghĩa một trang con, bạn có thể sử dụng từ khóa chỉ thị` @extends ` để chỉ định trang con nên kế thừa từ layout nào

    <!-- Lưu tại resources/views/child.blade.php -->
    
    @extends('layouts.master')
    
    @section('title', 'Tiêu đề trang')
    
    @section('sidebar')
        @parent
    
        <p>Đây là phần thêm vào bên dưới sidebar chính ở layout.</p>
    @endsection
    
    @section('content')
        <p>Đây là phần nội dung trong trang.</p>
    @endsection

 - `@yield`: bản thân từ này đồng nghĩa với từ output, do đó dĩ nhiên nhiệm vụ của nó là xuất dữ liệu. Vậy ta chỉ nên dùng cho việc xuất những `dữ liệu nhỏ`.
 - `@section` : có nghĩa là phần, đoạn hay mảnh. Rõ ràng từ khóa này nhắm tới việc phân nhỏ layout ra thành từng đoạn nhỏ để  các trang con ghi nội dung vào các vị trí đánh dấu này!
 - Về mặt kỹ thuật, thật sự khi sử dụng @section , các trang con sẽ có thể ghi đè, hoặc ghi nối thêm nội dung bằng từ khóa @parent . Khi có từ khóa này, Blade sẽ lấy nội dung trong section của layout “chắp vào” trước nội dung của @section  ở trang con. Còn khi sử dụng @yield , nội dung ở trang con sẽ luôn luôn ghi đè vào vị trí đánh dấu ở layout, mà không thể nối thêm dữ liệu ở layout vào như khi dùng @section .

## Hiển thị data
 `Hello, {{ $name }}.`

  - Khi data có dạng `html` : `Hello, {!! $name !!}.`

  - Data json : `@json($array)`

## Control Structures
### If Statements
    @if (count($records) === 1)
        I have one record!
    @elseif (count($records) > 1)
        I have multiple records!
    @else
        I don't have any records!
    @endif

    @unless (Auth::check())
        You are not signed in.
    @endunless


    @isset($records)
        // $records is defined and is not null...
    @endisset

    @empty($records)
        // $records is "empty"...
    @endempty

### Switch Statements
    @switch($i)
        @case(1)
            First case...
            @break

        @case(2)
            Second case...
            @break

        @default
            Default case...
    @endswitch

### Loops
    @for ($i = 0; $i < 10; $i++)
        The current value is {{ $i }}
    @endfor

    @foreach ($users as $user)
        <p>This is user {{ $user->id }}</p>
    @endforeach

    @forelse ($users as $user)
        <li>{{ $user->name }}</li>
    @empty
        <p>No users</p>
    @endforelse

    @while (true)
        <p>I'm looping forever.</p>
    @endwhile

## Validation Errors

    @error('title')
        <div class="alert alert-danger">{{ $message }}</div>
    @enderror


