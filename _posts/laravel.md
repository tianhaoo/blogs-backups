---
title: laravel
date: 2018-01-20 00:05:04
categories:
- 后端
tags:
  - laravel
---

记录学习laravel的笔记




## 环境配置

* composer
* 开启php的一些拓展
* 在本地安装laravel
* ` composer global require "laravel/installer" `
* `laravel new appName`

## 基本配置

#### 在config/app.php中修改
* timezone 时区
* locale 语言包
<!-- more -->
## 文件夹结构

######app 目录
包含应用程序的所有核心代码，应用中所有的类都应该放在这里

######routes 目录
web.php 文件 提供会话状态、CSRF防护和coolie加密。如果应用不提供无状态的、restful风格的API，则所有的路由都应该在这里定义。

###### ...

## 请求生命周期

###### 开始
index.php 文件加载 Composer 生成定义的自动加载器，然后从 bootstrap/app.php 脚本中检索 Laravel 应用程序的实例。Laravel 本身采取的第一个动作是创建一个 application/ service container （服务容器）的实例。

###### 服务容器
Laravel 服务容器是用于管理类的依赖和执行依赖注入的工具。依赖注入这个花俏名词实质上是指：类的依赖项通过构造函数，或者某些情况下通过「setter」方法「注入」到类中。

来看一个简单的例子:
```
<?php

namespace App\Http\Controllers;

use App\User;
use App\Repositories\UserRepository;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * 用户存储库的实现。
     *
     * @var UserRepository
     */
    protected $users;

    /**
     * 创建新的控制器实例。
     *
     * @param  UserRepository  $users
     * @return void
     */
    public function __construct(UserRepository $users)
    {
        $this->users = $users;
    }

    /**
     * 显示指定用户的 profile。
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        $user = $this->users->find($id);

        return view('user.profile', ['user' => $user]);
    }
}
```

在这个例子中，控制器 UserController 需要从数据源中获取 users 。因此，我们要注入可以获取 users 的服务。在这种情况下，UserRepository 可能是使用 Eloquent 从数据库中获取 user 信息。因为 Repository 是通过 UserRepository 注入的，所以我们可以轻易地将其切换为另一个实现。这种注入方式的便利之处还体现在当我们为应用编写测试时，我们还可以轻松地「模拟」或创建 UserRepository 的虚拟实现。

**想要构建强大的大型应用，至关重要的一件事是：要深刻的理解 Laravel 服务容器。**

###### HTTP/控制器内核

* 位于app/Http/Kernel.php中的php内核定义了在执行请求之前运行的bootstrappers数组。这个数组负责：配置错误处理、配置日志目录、检测应用程序环境以及执行其他需要完成的任务。

* HTTP内核还定义了所有请求被应用程序处理之前必须经过的中间件的列表。这些中间件处理HTTP会话的读写、验证CSRF令牌等。

* HTTP 内核的 handle 方法的方法签名非常简单：接收 Request 并返回 Response。可以把内核当作是代表整个应用程序的大黑盒，给它 HTTP 请求，它就返回 HTTP 响应。


###### 用户认证系统

* `php artisan make:auth`

* `php artisan migrate`

middleware的使用方法

```
Route::get('profile', function () {
    // 只有认证过的用户可以...
})->middleware('auth');
```

### 更换laravel站点的语言为中文

* [下载语言包]()
* 修改文件 config/app.php 'locale' => 'zh_cn'
* 把当前的文件 下载到 resources/lang/zh_cn/

### blade模板
#### 显示数据
可以使用 「中括号」 包住变量将数据传递给 Blade 视图。如下面的路由设置：
```
Route::get('greeting', function () {
    return view('welcome', ['name' => 'Samantha']);
});
```
可以像这样显示 name 变量的内容：
```
Hello, {{ $name }}.
```
其实，你可以在 Blade 打印语法中放置任何 PHP 代码：
```
The current UNIX timestamp is {{ time() }}.
```
#### 引入css或者js文件`URL::asset()`

* 引入站内的文件
```
<link rel="stylesheet" href="{{ URL::asset('css/bootstrap.css') }}">
<script type="text/javascript" src="{{ URL::asset('js/jquery.min.js') }}"></script>
```
> 注：默认相对于web根目录，也就是public目录

* 引入站外css和js：
```
<link rel="stylesheet" href="{{ URL::asset('//netdna.bootstrapcdn.com/bootstrap/3.0.3/css/bootstrap.min.css') }}">
<script type="text/javascript" src="{{ URL::asset('//code.angularjs.org/1.2.13/angular.js') }}"></script>
```

#### 数据库
* 数据库的配置文件放置在`config/database.php`文件中
* 使用 Artisan 命令`make:migration`来创建迁移：
```
php artisan make:migration create_users_table
```
* 新的迁移文件会被放置在 database/migrations 目录中。每个迁移文件的名称都包含了一个时间戳，以便让 Laravel 确认迁移的顺序。

> --table 和 --create 选项可用来指定数据表的名称，或是该迁移被执行时是否将创建的新数据表。这些选项需在预生成迁移文件时填入指定的数据表：

* 外键约束
```
chema::table('posts', function (Blueprint $table) {
    $table->integer('user_id')->unsigned();

    $table->foreign('user_id')->references('id')->on('users');
});
```
还可以为约束的「on delete」和「on update」属性指定所需的操作：
```
$table->foreign('user_id')
      ->references('id')->on('users')
      ->onDelete('cascade')
      ->onUpdate('cascade');
      // 父表更新时子表也更新，父表删除时子表匹配的项也删除。
```

##### 定义模型

```
chmod +x /usr/local/bin/docker-compose
```

请注意，我们并没有告诉 Eloquent，Flight 模型该使用哪一个数据表。除非数据表明确地指定了其它名称，否则将使用类的复数形式「蛇形命名」来作为表名。因此，在这种情况下，Eloquent 会假定 Flight 模型存储的是 flights 数据表中的记录。你可以通过在模型上定义 table 属性，来指定自定义数据表：

```
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * 与模型关联的数据表
     *
     * @var string
     */
    protected $table = 'my_flights';
}
```


**待解决**
longin.blade.php id=email

用户注册的流程


一些与用户认证有关的方法
```
Auth::guard('admin')    //指定看守 返回Auth对象
Auth::user();            //获取通过验证的用户 Auth::user()->name
Auth::check();            //检查是否验证
Auth::viaRemember();    //判断用户是否使用“记住我”cookie进行认证
Auth::login($user);        //将一个已存在的用户实例登录到应用中,传入实例必须是Illuminate\Contracts\Auth\Authenticatable 契约的实现
Auth::loginUsingId($userid);//通过用户ID登录到应用
Auth::once($credentials);//只在单个请求中将用户登录到应用，而不存储任何 Session 和 Cookie
Auth::attempt($credentials);//登录用户 ，$credentials是['email' => $email, 'password' => $password],这个方法会和数据库对比
Auth::onceBasic();
Auth::provider();
Auth::logout();            //注销验证用户
Auth::extend();            //自定义看守
Auth::provider();        //自定义用户提供者
```

找到了定义guard的地方
```php
trait GuardHelpers
{
    /**
     * The currently authenticated user.
     *
     * @var \Illuminate\Contracts\Auth\Authenticatable
     */
    protected $user;

    /**
     * The user provider implementation.
     *
     * @var \Illuminate\Contracts\Auth\UserProvider
     */
    protected $provider;

    /**
     * Determine if the current user is authenticated.
     *
     * @return \Illuminate\Contracts\Auth\Authenticatable
     *
     * @throws \Illuminate\Auth\AuthenticationException
     */
    public function authenticate()
    {
        if (! is_null($user = $this->user())) {
            return $user;
        }

        throw new AuthenticationException;
    }

    /**
     * Determine if the current user is authenticated.
     *
     * @return bool
     */
    public function check()
    {
        return ! is_null($this->user());
    }

    /**
     * Determine if the current user is a guest.
     *
     * @return bool
     */
    public function guest()
    {
        return ! $this->check();
    }

    /**
     * Get the ID for the currently authenticated user.
     *
     * @return int|null
     */
    public function id()
    {
        if ($this->user()) {
            return $this->user()->getAuthIdentifier();
        }
    }

    /**
     * Set the current user.
     *
     * @param  \Illuminate\Contracts\Auth\Authenticatable  $user
     * @return $this
     */
    public function setUser(AuthenticatableContract $user)
    {
        $this->user = $user;

        return $this;
    }

    /**
     * Get the user provider used by the guard.
     *
     * @return \Illuminate\Contracts\Auth\UserProvider
     */
    public function getProvider()
    {
        return $this->provider;
    }

    /**
     * Set the user provider used by the guard.
     *
     * @param  \Illuminate\Contracts\Auth\UserProvider  $provider
     * @return void
     */
    public function setProvider(UserProvider $provider)
    {
        $this->provider = $provider;
    }
```

### 配置laravel发送重置密码链接

```
MAIL_DRIVER=smtp
MAIL_HOST=smtp.qq.com
MAIL_PORT=465
MAIL_FROM_ADDRESS=360726539@qq.com
MAIL_USERNAME=360726539@qq.com
MAIL_PASSWORD=************** // 在QQ邮箱开通相应服务后可得
MAIL_FROM_NAME=360726539@qq.com
MAIL_ENCRYPTION=ssl
```
* 相关配置文件
`Illuminate\Auth\Notifications\ResetPassword`

### middleare

### guard

### validator

### gates

### policies


### http请求

Example via method
```
<?php namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

class UserController extends Controller {

    /**
     * Store a newly created resource in storage.
     *
     * @param  Illuminate\Http\Request  $request
     * @return Response
     */
    public function store(Request $request) {
        $name = $request->input('name');
    }
}
```
Example via constructor
```
<?php namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

class UserController extends Controller {

    protected $request;

    public function __construct(Request $request) {
        $this->request = $request;
    }

    /**
     * Store a newly created resource in storage.
     *
     * @return Response
     */
    public function store() {
        $name = $this->request->input('name');
    }
}
```


>默认情况下，使用{{ $var }}标签输出字符串，Blade模板引擎会自动转义（escape）HTML字符，如果需要原生输出需要使用{!! $var !!}标签！