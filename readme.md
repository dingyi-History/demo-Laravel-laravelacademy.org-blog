# 学习laravel学院博客系列

## 要点记录

* 1.[从测试开始（一）：创建项目和PHPUnit](http://laravelacademy.org/post/2232.html)
  * 要点

* 2.[从测试开始（二）：使用Gulp实现自动化测试](http://laravelacademy.org/post/2249.html)
  * 要点

* 3.[十分钟搭建博客系统](http://laravelacademy.org/post/2265.html)
  * 要点

* 4.[构建博客后台管理系统](http://laravelacademy.org/post/2279.html)

  * 要点1. 切换导航标签: `Request::is('admin/upload*')`

  ```
    <li @if (Request::is('admin/post*')) class="active" @endif>
  ```

  * 要点2. 修改登录重定向路径，修改 app/Http/Controllers/Auth/AuthController

  ```
  protected $redirectAfterLogout = '/auth/login';
  protected $redirectTo = '/admin/post';
  ```

  * 要点3. 登录成功后跳转链接是 /home,编辑 app/Http/Middleware/RedirectIfAuthenticated.php 文件，修改第38行代码修改

  ```
  return new RedirectResponse('/admin/post');
  ```