## MVC概述——以Laravel为例

#### M -> Model V -> View C -> Controller

首先来明确一个概念，**什么是MVC框架**？

要想回答这个问题，我们要从开发框架讲起来。

面对大型的软件项目时，我们如果不制定一个合理的架构模式，就会容易将代码写成**屎山**，即所谓的过度耦合，容易被破坏。

软件架构模式就是为了这种情况而存在的，它是一个**通用的**、**可重用的**解决方案。

常见的软件架构模式：

* 分层模式
* 客户端-服务器模式
* 代理模式
* 解释器模式
* 以及今天要讲的MVC模式，即模型-视图-控制器模式

MVC框架起源于1978年的施乐，原本是为Smalltalk所设计的一种软件架构，遵循一种动态的程序设计。它分为以下几个模块：

* 模型（Model）—— 一些可重用的实现**算法**，对数据的**管理方式**和与**数据库的交换**
* 视图（View） —— 图形界面的设计
* 控制器（Controller） —— 转发**请求**，对请求的**处理**

其中，Model拥有最高的非依赖性，它有对数据的**直接**访问权限，而Controller则起到了一个**响应事件**[^1]和组织的作用，负责沟通用户的行为和数据Model。

从这个意义上说，我们寒假所做的MVC框架开发实际上是**不正确**的，它更多地只是一个VC开发，Controller拥有了对数据的直接访问权限。

#### PHP中的MVC

在各种语言中都有MVC框架的具体实现，比如*Python*中的*Django*，*Javascript*中的*Angular.js*，*Ember.js*……

而PHP中的MVC框架有**Prado**，**ThinkPHP**，**Symfony**以及今天要讲的**Laravel**等等，其中Laravel是Symfony的改进和继承，也是目前最大和最完备的PHP开发框架之一。

对Laravel的安装：

```bash
composer global require laravel/installer
```

这样可以在本地安装好一个可全局调用的Laravel框架。

要创建项目，我们使用以下命令

```bash
laravel new "Your Project name"
```

它会在当前的文件夹创建一个空的Laravel框架。

接下来我们说说这个框架的文件夹结构

* app：包含了项目的核心代码，比如控制器，类等等，这个文件夹在一个App的命名空间里，并遵循PSR-4的自动加载标准
  * Console：包含你在本地使用php artisan命令访问框架所需的一切命令，你可以添加来在转移框架和之后运行的时候的命令
  * Exceptions：包含你的项目对错误和异常的具体处理代码，负责在用户使用时碰到Bug时**抛出**（你的**被玩坏了……）。
  * Http：Controller，middleware以及对表单请求的PHP代码全都在这个文件夹，顾名思义，它是用来处理传入框架的Http请求的。
  * Providers：包含了Service Providers，比如auth和route等集成的应用

  app文件夹还可以拥有许多自行创建的文件夹，可以通过

  ```php
  php artisan list make
  ```

  来查看。

* bootstrap：网站缓存和优化，在服务器上建议将该目录设置为可读可写。

* config：顾名思义，网站的组态和配置。

* database：存放你的数据库结构和model代码，甚至可以将轻量数据库也存在里面（

* public：服务器的静态资源，包括图片，js和css代码。

* resources：包括了自己编写的view模板（blade.php)，**未编译**的sass和less文件等。

* routes：重定向的定义，包含了web，api，console和channels四个文件，顾名思义，分别负责了网页请求，api调用，命令行调用和广播频道的配置。

* storage：在实际运行时的**编译好**的blade模板以及网站缓存、事件日志等等，其中的storage/app/public也可以存放用户上传的一些小型文件，同样，设置为可读可写。

* tests：测试环境所自动运行的一些脚本

* vendor：存放所引用的composer模块

#### 坐下，都是基本操作

接下来我们来学习一下Laravel的一些基本使用。

1. Routing

   我们之前说过，routes文件夹里包含了重定向和路径的定义。一个基本的路径定义写法如下

   ```php
   Route::get('/login', function () {
       /* callback */
   });
   ```

   当然我们不止能使用Get方法，也有Post，put，patch等等……

   我们也可以用match和any来包含多个方法。

   group可以用来定义二级路径：

   ```php
   Route::group(['prefix'=>'uri'],function(){
       Route::get();
       Route::post();
       Route::match([]);
   });
   ```

   redirect用于重定向，默认返回状态是302，当然，你可以改成301[^2]；

   我们使用 **{}** 来包含一个url里的变量，注意的是，变量传入function只与顺序有关，而和名称无关。

   ```php
   Route::get()->where([{} => 'Regular Expression']);
   ```

   使用where语句来进行正则表达式匹配。

   ```PHP
   Route::get()->name();
   ```

   使用name方法能够很方便的定义一个url的名称，在全局都可以使用route()方法进行重用。如果这个route中有变量，使用 **['' => '']** 来传递。

   middleware方法用于将middleware分配给route。

   同样的还有namespace，name和domain等方法作为语法糖来更好的分类routes，在此不一一介绍。

   使用fallback来匹配未被定义的路径。

   **最后需要注意的是，在html中写的表单请使用@csrf来通过安全验证。**

2. Middleware

   我们继续来处理http的请求，我们经常会碰到一种情况，那就是处理未登录用户的请求，传统的写法的话，我们需要在controller里使用if来进行特判，这一点都不优雅！

   现在我们可以使用middleware来预处理这样的请求了。[^3]

   使用

   ```bash
   php artisan make:middleware name
   ```

   来创建一个middleware

   我们会在app/http/middleware中看到我们生成的middleware，你可以增加你想要的任何操作。

   middleware分为传递前和传递后的处理，具体的代码区别是

   Before：

   ```php
   class BeforeMiddleware
   {
       public function handle($request, Closure $next)
       {
           // Perform action
   
           return $next($request);
    	}
   }
   ```

   After:

   ```php
   class AfterMiddleware
   {
       public function handle($request, Closure $next)
       {
           $response = $next($request);
   
           // Perform action
   
           return $response;
       }
   }
   ```

   你需要在kernel里注册你的middleware来进行全局调用。

   和Route一样，middleware也可以有group来进一步筛选。

   我们也可以向middleware中传入参数，在route中的写法是：

   ```php
   Route::get('/', function ($id) {
       //
   })->middleware('name:parameter');
   ```

3. Controller

   终于到了我们耳熟能详的controller部分，我们之前两部分都在讨论如何处理http请求，同样，controller的任务也是用来处理http请求，只不过，它可以说是一个终点了。

   Laravel中的Controller定义和其他的MVC框架区别不大，你可以命名Controller了名称，以及其中的函数。在需要调用的时候，使用"Class@function"的写法来使用。

   在Controller里我们可以使用闭包来定义一个独有的Middleware，使得代码更简洁。

   ```php
   $this->middleware(function ($request, $next) {
       // ...
   
       return $next($request);
   });
   ```

   使用_construct和update函数来对controller引入变量。

   我们可以定义一个resource类的controller来控制对服务器资源的访问请求，如对图片的访问。

   ```php
   php artisan make:controller PhotoController --resource
   ```

4. Request

   request功能是laravel在symfony的基础上加以完善的一个类。它被包含在"Illuminate\Http\Request"中。

   下面介绍几个常用的函数。

   * 使用path()来获取请求的路径。

   * is()可以允许你模式匹配请求路径是否合法。

   * url和fullurl可以让你获取请求链接，区别在于url不含有查询字段

   * 使用input()和query()来获取传入controller的变量，你也可以使用具体的表单名或是传入变量名来获取（注意返回的是**数组**）

   * flash()可以将用户的input都保存在session中方便用户的日后调用。

   * 很多时候我们可能会输错密码，但我们并不想让我们的其他输入被清空，我们可以使用：

     ```php
     return redirect('login')->withInput(
         $request->except('password')
     );
     ```

     这样的写法。

   * file()可以执行很多文件上传的操作，留待以后讲解。

   * only和except。

5. Response

   到了处理完请求给出回应的时候了，Laravel的response模块也十分完善。

   如果你只是简单的return一个字符串或者数组，laravel都会帮你很好的将其处理成JSON，但是更复杂的是：

   ```php
   return response($content)
               ->withHeaders([
                   'Content-Type' => $type,
                   'X-Header-One' => 'Header Value',
                   'X-Header-Two' => 'Header Value',
               ]);
   ```

   来自定义返回的响应头。

   使用cookie()，也可以返回给客户端一些设置好的cookie。

   redirect和back()，顾名思义，不多作介绍。多提一句，redirect可以重定向到网页或者是另一个Controller或middleware。

   away()可以跳转到外链，而with()可以顺便设置好session。

   view()来返回一个网页.

   最后值得一提的是，与文件上传相对，这里也可以使用download()来下载一个文件。

   你甚至可以使用marcos来写一个自定义的response。

6. View

   laravel的view()函数可以用来实现对前端页面的管理和修改。它们被存储为.blade.php的模板。

   当然很多时候，我们会厌倦频发传递参数给前端，这不但增加了工作量，也会很没效率，这个时候，我们就可以后View::Composer来完成这个一劳永逸的任务。

   ```php
    View::composer('dashboard', function ($view) {
               //
           });
   ```

   通过这样的闭包，或者是对路径的绑定，可以将这个页面和一个特定的php类绑定起来。

   **[]** 的语法可以将一个composer绑定到多个view上。

   我们现在要来看看这些模板的独有语法。

   Lavarel的blade模板最大的特点是，它本身即是用PHP语言储存的，因此blade中可以随意的插入PHP代码。

   @section可以来定义模块名称，@show可以显示该模块。

   使用@yield来显示模块，@extends来引入其他模板。

   我们给出一个例子：

   ```php
   <html>
       <head>
           <title>App Name - @yield('title')</title>
       </head>
       <body>
           @section('sidebar')
               This is the master sidebar.
           @show
   
           <div class="container">
               @yield('content')
           </div>
       </body>
   </html>
   ```

   ```php
   @extends('layouts.app')
   
   @section('title', 'Page Title')
   
   @section('sidebar')
       @parent
   
       <p>This is appended to the master sidebar.</p>
   @endsection
   
   @section('content')
       <p>This is my body content.</p>
   @endsection
   ```

   @slot和@component可以添加任意html成分，并可以 **[]** 来传递变量。

   在appserviceprovider中可以自定义自己的@，例如

   ```php
   Blade::component('components.alert', 'alert');
   ```

   传递变量使用blade echo方法，即 **{{ }}** ，在这里面也可以写任何PHP代码来直接处理输出。

   我们这边使用的 **{{ }}** 是经过转义来防止xss攻击的。

    使用@json来自动json编码一个数组。

   ```javascript
   <script>
       var app = @json($array);
   </script>
   ```

   熟悉的php控制结构和遍历也有相应的标签支持：

   ```php
   @if (count($records) === 1)
       I have one record!
   @elseif (count($records) > 1)
       I have multiple records!
   @else
       I don't have any records!
   @endif
   ```

   @unless和@isset、@empty也可以直接使用。

   更方便的，@auth和@guest可以帮助你快速校验用户身份。

   switch语句的写法如下：

   ```php
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
   ```

   循环的写法如下：

   ```php
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
   ```

   显然这些和我们平时写的php大同小异。

   如果实在有特殊的需要，我们也可以用@php和@endphp来插入任何的php代码。

   如果最后的最后，这些还不能满足你的需求，可以在appserviceprovider中定义新的@方法。

   比如：

   ```php
   Blade::directive('datetime', function ($expression) {
               return "<?php echo ($expression)->format('m/d/Y H:i'); ?>";
           });
   ```

7. Error handling

   最后我们可以来说说Laravel的错误处理系统。

   Laravel的debug选项是env中的APP_DEBUG选项，它和"config/app.php"中的debug相关联。

   在"app/Exception/Handler"中处理对错误是否进行日志记录

   ```php
   public function report(Exception $exception)
   {
       if ($exception instanceof CustomException) {
           //
       }
   
       parent::report($exception);
   }
   ```

   使用instanceof来筛选错误种类。

   ```php
    try {
        
       } catch (Exception $e) {
           report($e);
   
           return false;
       }
   ```

   使用report函数可以简单的在生产环境中记录错误且不停止运行，以上语句可以写在任一Controller中。

   render()可以返回特定的页面，其余与report基本相同。

   当我们遇到特定的http错误，比如404，403时，可以使用abort()。

   我们可以在"resources/view/error/"中自定义我们的http错误页面。

#### 还有一件事……

Laravel的许多强大功能在这节课上不能一一介绍完毕，比如**合理化校验**，**加密**，**日志**这些安全，维护问题，以及laravel和npm的配合（其实是我也不熟x，最后还有**文件处理**也是重要的一环。

这些内容在[Laravel官方文档](https://laravel.com/docs/5.8/blade)都有很详细的介绍，希望大家进一步去了解和学习。

当然，最后我也留了最重要的数据Model部分给下节课的皮老板继续讲解~

布置一个小小的作业，大家试着用今天所讲的Laravel框架的功能，写一个简单的登录系统，最优秀的代码将会被采纳，成为明年新生杯的系统（提示：活用auth）。

[^1]: 事件响应型的软件架构也有很普遍的应用，比如安卓开发。
[^2]: 当然使用permanentRedirect默认返回301.
[^3]: Laravel内置了authenticate的middleware来处理我们刚刚提到的情形。