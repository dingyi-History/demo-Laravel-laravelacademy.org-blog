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

  * 要点4.就是Laravel中实现某个内置功能一般而言有三种实现方式：门面、依赖注入和辅助函数。这里Route对应门面、$router对应依赖注入实例、get对应辅助函数，门面本质上对应的也是依赖注入的实例类，所以Route和$router除了写法不一样，本质上完全一致；辅助函数实际上调用的也是实例类上的方法，只是写法更加简便，此外除了group之外，其他路由方法都对应有辅助函数，具体可以去Illuminate/Foundation目录下查看helpers.php中的函数。

* 5.[使用Bower+Gulp集成前端资源](http://laravelacademy.org/post/2299.html)

* 6.[在后台实现文章标签增删改查功能](http://laravelacademy.org/post/2320.html)

* 7.[实现文件上传管理功能](http://laravelacademy.org/post/2333.html)

* 8.[后台文章增删改查功能实现支持Markdown](http://laravelacademy.org/post/2358.html)

* 9.[前台功能优化：给博客换上漂亮的主题 & 完善博客功能](http://laravelacademy.org/post/2371.html)