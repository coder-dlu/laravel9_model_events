# laravel9_model_events
- Laravel cung cấp danh sách các model events và mỗi model event have it's own function. nó rất hữu ích. 
+ creating: Call Before Create Record.
+ created: Call After Created Record.
+ updating: Call Before Update Record.
+ updated: Class After Updated Record.
+ deleting: Call Before Delete Record.
+ deleted: Call After Deleted Record.
+ retrieved: Call Retrieve Data from Database.
+ saving: Call Before Creating or Updating Record.
+ saved: Call After Created or Updated Record.
+ restoring: Call Before Restore Record.
+ restored: Call After Restore Record.
+ replicating: Call on replicate Record
## 1: Create Product Model with events
- app/Models/Product.php
```Dockerfile
<?php
  
namespace App\Models;
  
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Log;
use Str;
   
class Product extends Model
{
    use HasFactory;
  
    protected $fillable = [
        'name', 'slug', 'detail'
    ];
  
    /**
     * Write code on Method
     *
     * @return response()
     */
    public static function boot() {
  
        parent::boot();
  
        /**
         * Write code on Method
         *
         * @return response()
         */
        static::creating(function($item) {            
            Log::info('Creating event call: '.$item); 
  
            $item->slug = Str::slug($item->name);
        });
  
        /** 
         * Write code on Method
         *
         * @return response()
         */
        static::created(function($item) {           
            /*
                Write Logic Here
            */ 
  
            Log::info('Created event call: '.$item);
        });
  
        /**
         * Write code on Method
         *
         * @return response()
         */
        static::updating(function($item) {            
            Log::info('Updating event call: '.$item); 
  
            $item->slug = Str::slug($item->name);
        });
  
        /**
         * Write code on Method
         *
         * @return response()
         */
        static::updated(function($item) {  
            /*
                Write Logic Here
            */    
  
            Log::info('Updated event call: '.$item);
        });
  
        /**
         * Write code on Method
         *
         * @return response()
         */
        static::deleted(function($item) {            
            Log::info('Deleted event call: '.$item); 
        });
    }
}
```
## 2: Create Record: Creating and Created Event
- app/Http/Controllers/ProductController.php
```Dockerfile
<?php
  
namespace App\Http\Controllers;
  
use App\Models\Product;
use Illuminate\Http\Request;
  
class ProductController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        Product::create([
            'name' => 'silver',
            'detail' => 'This is silver'
        ]);
  
        dd('done');
    }
}
```
Output Log File:
```Dockerfile
[2020-10-20 14:37:26] local.INFO: Creating event call: {"name":"silver","detail":"This is silver"}  

[2020-10-20 14:37:26] local.INFO: Created event call: {"name":"silver","detail":"This is silver","slug":"silver","updated_at":"2020-10-20T14:37:26.000000Z","created_at":"2020-10-20T14:37:26.000000Z","id":5} 
```
## 3:Update Record: Updating and Updated Event
- app/Http/Controllers/ProductController.php
```Dockerfile
<?php
  
namespace App\Http\Controllers;
  
use App\Models\Product;
use Illuminate\Http\Request;
  
class ProductController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        Product::find(5)->update([
            'name' => 'silver updated',
            'detail' => 'This is silver'
        ]);
   
        dd('done');
    }
}
```
- Output Log File:
```Dockerfile
[2020-10-20 14:39:04] local.INFO: Updating event call: {"id":5,"name":"silver updated","detail":"This is silver","created_at":"2020-10-20T14:37:26.000000Z","updated_at":"2020-10-20T14:37:26.000000Z","slug":"silver"}  

[2020-10-20 14:39:04] local.INFO: Updated event call: {"id":5,"name":"silver updated","detail":"This is silver","created_at":"2020-10-20T14:37:26.000000Z","updated_at":"2020-10-20T14:39:04.000000Z","slug":"silver-updated"}  
```
## 4: Delete Record: Deleted Event
- app/Http/Controllers/ProductController.php
```Dockerfile
<?php
  
namespace App\Http\Controllers;
  
use App\Models\Product;
use Illuminate\Http\Request;
  
class ProductController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        Product::find(5)->delete();
   
        dd('done');
    }
}
```
- Output Log File:
```Dockerfile
[2020-10-21 03:14:45] local.INFO: Deleted event call: {"id":5,"name":"silver updated","detail":"This is silver","created_at":"2020-10-20T14:37:26.000000Z","updated_at":"2020-10-20T14:39:04.000000Z","slug":"silver-updated"}
```
