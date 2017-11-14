# Laravel5.2-IsLogin
laravel5.2利用middleware判定登陆情况, 防止直接登陆后台
### 首先kernel.php文件注册一个中间件islogin
```php
'islogin' => \App\Http\Middleware\Login::class,
```
### 然后把需要登陆后才能显示的路由写进一个组group里面
```php
Route::group(['middleware' => ['web','islogin']], function () {
    Route::get('admin/index','Admin\IndexController@index');//后台主页路由
    Route::get('admin/welcome','Admin\IndexController@welcome');//后台副主页路由
    Route::get('admin/exit','Admin\LoginController@quit');//退出路由
});
```
### 在Middleware目录下创建对应的Login.php中间件文件编写权限判断是否有session值（这个session是在访问admin/login登陆路由时产生的）
```php
<?php
namespace App\Http\Middleware;
use Closure;

class AdminLogin{

    public function handle($request,Closure $next){

        if (!session('admin')){
            return redirect('admin/login');
        }
        return $next($request);
    }

}
```
### 另外退出功能可以把session赋值空,并跳回登陆页
```php
session(['user'=>null]);
return redirect('admin/login');
```
