# Database
## Configuration
`config/database.php`

## Queries
### Raw SQL Queries
 - Sử dụng `use Illuminate\Support\Facades\DB` để dùng raw queries 
 - Các method cung cấp : `select`, `update`, `insert`, `delete`, `statement`

#### Select
`$users = DB::select('select * from users where active = ?', [1]);` // trả về array
#### Insert
`DB::insert('insert into users (id, name) values (?, ?)', [1, 'Dayle']);`

#### Update 
`$affected = DB::update('update users set votes = 100 where name = ?', ['John']);`

#### Delete 
`$deleted = DB::delete('delete from users');`

#### General Statement
`DB::statement('drop table users');`

### Query Builder
 - Dùng `table` method on the `DB facade` to begin a `query`
#### Retrieving All Rows From A Table
` $users = DB::table('users')->get();`
 - Select cột muốn get : 

`DB::table('users')->select('name','email')->get();`
 - Raw expression
    $users = DB::table('users')
        ->select(DB::raw('count(*) as user_count, status'))
        ->where('status', '<>', 1)
        ->groupBy('status')
        ->get();
#### Retrieving A Single Row / Column From A Table
`$user = DB::table('users')->where('name', 'John')->first();`

 - Theo id: `$user = DB::table('users')->find(3);`


#### Thêm data
    DB::table('sanpham')->insert([
        'tenSP'=> 'Dell 1100',
        'maSP'=>'M004',
    ]);

#### Update dữ liệu

    DB::table('sanpham')->where('id', '2')->update([
    		'tenSP'=> 'Iphone XS',
    		'maSP'=>'M002',
    		'idDanhMuc'=>'1',
    	]);

#### Xóa dữ liệu 

`DB::table('sanpham')->where('id', '1')->delete();`


#### Where
##### Điều kiện bằng.
`DB::table('tablename')->where('column','filter')->get();`
##### Điều kiện lớn hơn(bé hơn)
`DB::table('tablename')->where('column','>','filter')->get();` // > or <>
##### Điều kiện khác
`DB::table('tablename')->where('column','<>','filter')->get();` 
##### Điều kiện lồng
`$users = DB::table('users')->where('votes', '>', 100)->orWhere('name', 'John')->get();` 
##### Tìm kiếm chuỗi
`DB::table('tablename')->where('column','like','filter')->get();` 
##### tìm khoản
`whereBetween('votes', [1, 100])`
 - không nằm trong khoản : `whereNotBetween('votes', [1, 100])`

 - kiểm tra giá trị của một cột có nằm trong một mảng các giá trị hay không.

 `whereIn('id', [1, 2, 3])`

#### join
##### inner join.
`$users = DB::table('users')->join('contacts', 'users.id', '=', 'contacts.user_id')->get();`
##### Left join.
`$users = DB::table('users')->leftjoin('contacts', 'users.id', '=', 'contacts.user_id')->get();`

#### OrderBy (desc or asc)
    $users = DB::table('users')
        ->orderBy('name', 'desc')
        ->get();

### Eloquent ORM
#### Tạo model
`php artisan make:model News

    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class News extends Model
    {
       protected $table = 'tableName';
       // lọc các cột sử dụng
       protected $fillable = ['column1', 'column2', .., 'columnn'];
       // khai báo timestamp
       public $timestamps = true;

       // Quan hệ 1-1
        public function phone()
        {
            return $this->hasOne('App\Phone','foreign_key', 'local_key');
        }
        //using : $phone = News::find(68)->phone;

        // trog model PHone
        //  public function user()
        //  {
        //       return $this->belongsTo('App\User','foreign_key', 'local_key');
        //  }

        // 1 - nhiều
        public function comments()
        {
            return $this->hasMany('App\Comment','foreign_key', 'local_key');
        }

        //Trong model comment
        //    public function post()
        //    {
        //        return $this->belongsTo('App\Post','foreign_key', 'local_key');
        //    }


    }`

#### Lấy ra tất cả dữ liệu
    <?php

    namespace App\Http\Controllers;

    use App\News;

    class homecontroller extends Controller
    {
    News::all();
    }

 - Lấy theo id : `News::find(1);`

 - Có diều kiện : `News::where('id', 5)->get();`
 - Chọn cột : `News::select('id', 'title')->get();`
 - Đếm : `News::all()->count();`

#### Thêm dữ liệu 
    $news = new News();
    $news->title = "tin tức 1";
    $news->categoryId = 1;
    $news->save();

#### Sửa dữ liệu
    $news = News::find(1);
    $news->title = 'toidicode.com';
    $news->save();

#### Xóa 
    $news= News::find(1);
    $news->delete();

