# LOG

    Log::emergency($message);
    Log::alert($message);
    Log::critical($message);
    Log::error($message);
    Log::warning($message);
    Log::notice($message);
    Log::info($message);
    Log::debug($message);

## Write logs
    <?php

    namespace App\Http\Controllers;

    use App\Http\Controllers\Controller;
    use App\User;
    use Illuminate\Support\Facades\Log;

    class UserController extends Controller
    {
        /**
        * Show the profile for the given user.
        *
        * @param  int  $id
        * @return Response
        */
        public function showProfile($id)
        {
            Log::info('Showing user profile for user: '.$id);

            return view('user.profile', ['user' => User::findOrFail($id)]);
        }
    }

 ## Writing To Specific Channels
`Log::channel('slack')->info('Something happened!');`

