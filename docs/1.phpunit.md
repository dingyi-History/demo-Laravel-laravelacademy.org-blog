基于 Laravel 开发博客应用系列 —— 从测试开始（一）：创建项目和PHPUnit
 发布于 2015年11月29日
之前的部分都是在讲环境搭建和工具使用，从本节开始，将正式开始开发博客项目：我们将会创建一个本系列教程余下部分都会使用的项目，并且使用测试驱动开发（TDD）的方式开发本项目，以此展现一个项目完整的开发流程。

1、创建博客项目
我们将遵循上一节提到的六步创建一个新 Laravel 5.1 项目的步骤，创建本节要用到的博客项目 —— blog。

首先，在本地主机安装应用骨架：

nonfu@ubuntu:~/Code$ composer create-project laravel/laravel blog --prefer-dist
接下来，编辑 Homestead.yaml，添加站点信息及数据库信息：
```
sites:
    - map: test.app
      to: /home/vagrant/Code/test/public
    - map: blog.app
      to: /home/vagrant/Code/blog/public

databases;
    - homestead
    - blog

```
然后运行 homestead provision 重新启动 Homestead 虚拟机。

在本地主机中，添加如下这行到 hosts 文件：

192.168.10.10 blog.app
还是在本地主机中，运行如下命令本地安装 NPM 包：

nonfu@ubuntu:~/Code$ cd blog
nonfu@ubuntu:~/Code/blog$ sudo npm install
然后到数据库中创建本项目的数据库 blog，之后编辑.env文件，修改数据库名称：

// 将
DB_DATABASE=homestead

// 修改为
DB_DATABASE=blog
最后在浏览器中访问 http://blog.app 查看一切是否就绪。

2、运行 PHPUnit
Laravel 5.1 集成的单元测试（基于 PHPUnit）功能是开箱即用的，甚至还提供了一个简单的单元测试示例以确保使用 web 请求该应用是否返回期望的 200 响应状态码。

要运行 PHPUnit，在项目根目录下简单运行 phpunit 命令即可：

Laravel 提供的PHPUnit测试示例

如果执行 phpunit 命令报错：command not found 或者 permissions denied，前者可能是安装时有问题，phpunit 命令位于 Composer 安装目录下的 vendor/bin 目录中，而且该目录已经被添加到系统路径中；后者则是因为没有为 phpunit 设置正确的权限以致无权执行该命令。

要解决这两个问题，需要按以下步骤操作：

第一步——删除项目的 vendor 目录。

第二步——在项目根目录下使用 composer update 命令重新生成 vendor 目录。

注：以上步骤都在主机操作系统中操作，而不是在 Homestead 虚拟机中。
好了，就是这样，重新执行 phpunit 命令看看一切是否正常

Laravel 5.1 中的 PHPUnit 配置

在新创建的 Laravel 5.1 项目根目录下有一个 phpunit.xml 文件，该文件包含了 PHPUnit 的配置项。

查看 phpunit.xml 文件会看到测试文件位于 tests 目录下，该目录下默认已经有两个测试文件了：

ExampleTest.php —— 包含一个 testBasicExample() 测试，ExampleTest 继承自 TestCase。
TestCase.php —— Laravel 测试类的基类。
下面我们来看看 ExampleTest.php 中的 testBasicExample() 方法：

public function testBasicExample()
{
    $this->visit('/')->see('Laravel 5');
}
该测试会访问应用首页并查看页面是否包含 Laravel 5，还有比这更简单的测试实现吗？！

TestCase 类提供了很多针对 Laravel 5.1 应用的方法和属性用于单元测试，此外还提供了很多断言方法和 Crawler 类型的测试。下面让我们来一一探究。

Laravel 5.1 中 Crawler 测试的方法和属性

Crawler 意为（网络）爬虫，Crawler 测试允许你在 web 应用中测试页面访问。

下面是一些 Crawler 测试中常用的属性和方法：
```
$this->response：web应用返回的最后一个响应
$this->currentUri：当前访问的URL
visit($uri)：通过GET请求访问给定URI
get($uri, array $headers = [])：通过GET请求获取给定URI页面的内容，可以传递请求头信息（可选）
post($uri, array $data = [], array $headers = [])：提交POST请求到给定URI
put($uri, array $data = [], array $headers = [])：提交PUT请求到给定URI
patch($uri, array $data = [], array $headers = [])：提交PATCH请求到给定URI
delete($uri, array $data = [], array $headers = [])：提交DELETE请求到给定URI
followRedirects()：根据最后响应进行任意重定向
see($text, $negate = false)：断言给定文本在页面中是否出现
seeJson(array $data = null)：断言响应中是否包含JSON，如果传递了$data，还要断言包含的JSON是否与给定的匹配
seeStatusCode($status)：断言响应是否包含期望的状态码
seePageIs($uri)：断言当前页面是否与给定URI匹配
seeOnPage($uri)和landOn($uri)：seePageIs()的别名
click($name)：使用给定body、name或者id点击链接
type($text, $element)：使用给定文本填充输入框
check($element)：检查页面上的checkbox复选框
select($option, $element)：选择页面上下拉列表的某个选项
attach($absolutePath, $element)：上传文件到表单
press($buttonText)：通过使用给定文本的按钮提交表单
withoutMiddleware()：在测试中不使用中间件
dump()：输出最后一个响应返回的内容
Laravel 5.1 提供给 PHPUnit 的方法和属性

```

下面是 Laravel 5.1 提供给 PHPUnit 使用的应用方法和属性：

```
$app：Laravel 5.1 应用实例
$code：Artisan命令返回的最后一个码值
refreshApplication()：刷新应用。该操作由TestCase的setup()方法自动调用
call($method, $uri, $parameters = [], $cookies = [], $files = [], $server = [], $content = null)：调用给定URI并返回响应
callSecure($method, $uri, $parameters = [], $cookies = [], $files = [], $server = [], $content = null)：调用给定HTTPS URI并返回响应
action($method, $action, $wildcards = [], $parameters = [], $cookies = [], $files = [], $server = [], $content = null)：调用控制器动作并返回响应
route($method, $name, $routeParameters = [], $parameters = [], $cookies = [], $files = [], $server = [], $content = null)：调用命名路由并返回响应
instance($abstract, $object)：在容器中注册对象实例
expectsEvents($events)：指定被给定操作触发的事件列表
withoutEvents()：无需触发事件模拟事件调度
expectsJobs($jobs)：为特定操作执行被调度的任务列表
withSession(array $data)：设置session到给定数组
flushSession()：清空当前session中的内容
startSession()：开启应用Session
actingAs($user)：为应用设置当前登录用户
be($user)：为应用设置当前登录用户
seeInDatabase($table, array $data, $connection = null)：断言给定where条件在数据库中存在
notSeeInDatabase($table, $array $data, $connection = null)：断言给定where条件在数据库中不存在
missingFromDatabase($table, array $data, $connection = null)：notSeeInDatabase()的别名
seed()：填充数据库
artisan($command, $parameters = [])：执行Artisan命令并返回码值

```

上述这些方法和属性都可以在测试类中使用。

Laravel 5.1 中 PHPUnit 的断言方法

除了标准的 PHPUnit 断言方法（如 assertEquals()、assertContains()、assertInstanceOf() 等）之外，Laravel 5.1 还提供了很多额外的断言用于帮助编写 web 应用的测试用例：

```
assertPageLoaded($uri, $message = null)：断言最后被加载的页面；如果加载失败抛出异常：$uri/$message
assertResponseOk()：断言客户端返回的响应状态码是否是200
assertReponseStatus($code)：断言客户端返回的响应状态码是否和给定码值相匹配
assertViewHas($key, $value = null)：断言响应视图包含给定数据片段
assertViewHasAll($bindings)：断言视图包含给定数据列表
assertViewMissing($key)：断言响应视图不包含给定数据片段
assertRedirectedTo($uri, $with = [])：断言客户端是否重定向到给定URI
assertRedirectedToRoute($name, $parameters = [], $with = [])：断言客户端是否重定向到给定路由
assertRedirectedToAction($name, $parameters = [], $with = [])：断言客户端是否重定向到给定动作
assertSessionHas($key, $value = null)：断言session包含给定键/值
assertSessionHasAll($bindings)：断言session包含给定值列表
assertSessionHasErrors($bindings = [])：断言session包含绑定错误
assertHasOldInput()：断言session中包含上一次输入

```
关于测试方法的使用示例，可参考 Laravel 测试文档，下一节我们将继续探讨测试使用：如何使用Gulp进行TDD（测试驱动开发）。