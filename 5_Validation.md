# Validation
 - Sử dụng `Validate` thông qua $request
    $validatedData = $request->validate([
            'title' => 'required|unique:posts|max:255',
            'body' => 'required',
        ]);
## Stopping On First Validation Failure
` 'title' => 'bail|required|unique:posts|max:255'` 

## Hiển thị error 

    @if ($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach ($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif

## Optional Fields
` 'publish_at' => 'nullable|date',`

## Form Request Validation
 - Khi dự án phức tạp hơn thì cần tạo 1 form request riêng

 - cmd : `php artisan make:request StoreBlogPost` nằm trong thư mục `app/Http/Requests`
 - Tạo `validate` tại `rules()`:
    public function rules()
    {
        return [
            'title' => 'required|unique:posts|max:255',
            'body' => 'required',
        ];
    }

 - Và sử dụng tại Controller :
    public function store(StoreBlogPost $request)
    {
        // The incoming request is valid...

        // Retrieve the validated input data...
        $validated = $request->validated();
    }

## Available Validation Rules
[link](https://laravel.com/docs/7.x/validation#available-validation-rules)



