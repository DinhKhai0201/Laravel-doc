# Blade template (resources/views)

## kế thừa
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


