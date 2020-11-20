### 术语表

IDE - Intergrated Development Environment
AWS - Amazon Web Services
Haml - [HTML abstraction markup language](http://haml.info)


### code 释义

gem 'some', '>=1.3.0' 表示版本号大于等于 1.3.0
gem 'some', '~>1.3.0' 表示版本号大于等于 1.3.0，但小于等于1.3.1
gem 'some', '~>1.3' 表示版本号大于等于 1.3，但小于等于1.4

使用 `rails _5.x.x new app_name` 后查看 `rails -v` 如果不是指定的版本，可以修改gemfile中的版本号，然后执行`bundle update`

`rails db:migration VERSION=0` 可以把数据库重置到开始状态，如果修改数字为1,2，3等则是其他对应的版本。

p :name 等同于 puts :name.inspect
```ruby
2.5.0 :009 > p :name
:name
 => :name
2.5.0 :010 > puts :name
name
 => nil
2.5.0 :011 > puts :name.inspect
:name
 => nil
```

`link_to`, `csrf_meta_tags`, `stylesheet_link_tag`, `javascript_include_tag` 这些都是 view 中可用的 helper 方法。

使用 `curl -OL cdn.learnenough.com/kitten.jpg` 可用直接下载图片到当前路径

`command` + `/` 快速注释

注释erb文件中 ruby 代码的方式 `#` 加在内部，`<%#= image_tag("kitten.jpg", alt: "Kitten")%>`


[SSL](https://en.wikipedia.org/wiki/Transport_Layer_Security)实际指 Transport Layer Security, SSL-Secure Sockets Layer 是前者的预处理器。













### quotations

**rake**

在 Unix 中，把源码编译成可执行的程序时，Make 扮演了很重要的角色。Rake 是 Ruby 版 Make，是使用 Ruby 语言编写的 Make 类程序。

在 Rails 5 之前，Ruby on Rails 大量使用 Rake，因此为了维护以前的应用，一定要知道如何使用 Rake。或许，Rails 最常使用的两个 Rake 命令是 rake db:migrate（迁移数据库，更新数据模型）和 rake test（运行自动化测试组件）。使用 Rake 时，要确保使用的是 Rails 应用 Gemfile 文件中指定的版本，方法是使用 Bundler 提供的 bundle exec 命令。因此，执行迁移的 rails db:migrate 命令要写成：

$ bundle exec rake db:migrate”

**Rest**

**RE**presentational **S**tate **T**ransfer

REST 是一种架构风格，用于开发分布式、基于网络的系统和软件应用，例如万维网和 Web 应用。REST 理论很抽象，在 Rails 应用中，REST 意味着大多数组件（例如users和posts）都被模型化，变成资源（resource），可以创建（create）、读取（read）、更新（update）和删除（delete）。这些操作与关系型数据库中的 CRUD 操作和 HTTP 请求方法（POST、GET、PATCH 和 DELETE）对应。

**ActiveRecord::Base**

rails中 model 继承自ApplicationRecord

ApplicationRecord(app/models/application_record.rb) 又继承自 ActiveRecord::Base

ActiveRecord::Base 基类的存在让rails能与数据库进行交互并把数据库中table里的 column 处理为 model 中的 attribute，以及实现其他rails中用到的处理model的方式。

**ActionController::Base**

ActionController:Base 是 ApplicationController的superclass, 后者又是其他controller的superclass。意味着rails中很多controller中的功能和行为是由 ActionController::Base这个基类提供的。

> 与模型的继承类似，通过继承 ActionController::Base，Users 控制器和 Microposts 控制器获得了很多功能。例如，处理model对象的能力、过滤入站 HTTP 请求，以及把视图渲染成 HTML 的能力。Rails 应用中的所有控制器都继承自 ApplicationController，所以其中定义的规则会自动运用于应用中的每个action。

**TDD**

TDD 是一种测试技术，程序员要先编写失败的测试，然后再编写应用代码，让测试通过。

**When to test**
When deciding when and how to test, it’s helpful to understand why to test. In my view, writing automated tests has three main benefits:

作者认为的测试的三个主要好处：

1. Tests protect against regressions, where a functioning feature stops working for some reason.
测试可以避免倒退，也就是某个功能突然因什么原因不工作了。
2. Tests allow code to be refactored (i.e., changing its form without changing its function) with greater confidence.
测试让你在重构代码的时候更加自信。
3. Tests act as a *client* for the application code, thereby helping determine its design and its interface with other parts of the system.
测试的角色就如app代码的客户，因此可以帮助我们做出设计上的决定以及与系统其它部分的接口设计。

Although none of the above benefits require that tests be written first, there are many circumstances where test-driven development (TDD) is a valuable tool to have in your kit. Deciding when and how to test depends in part on how comfortable you are writing tests; many developers find that, as they get better at writing tests, they are more inclined to write them first. It also depends on how difficult the test is relative to the application code, how precisely the desired features are known, and how likely the feature is to break in the future.

In this context, it’s helpful to have a set of guidelines on when we should test first (or test at all). Here are some suggestions based on my own experience:

作者提出的关于'什么时候应该先写测试'的建议：

- When a test is especially short or simple compared to the application code it tests, lean toward writing the test first.
当用来测试代码比app本身代码少很多的的时候，倾向于先写测试。

- When the desired behavior isn’t yet crystal clear, lean toward writing the application code first, then write a test to codify the result.
当你想要的行为不太清晰明确的时候，可以先写实现app功能的代码，接着写测试来修正结果。

- Because security is a top priority, err on the side of writing tests of the security model first.
安全问题在很多app种都有很高优先级，可以先写针对安全问题的测试来确保这部分功能。

- Whenever a bug is found, write a test to reproduce it and protect against regressions, then write the application code to fix it.
当app中出现一个bug时，可以先一个测试复现这个bug实现的过程，然后修改app修复bug，之后测试中就可以覆盖到这个bug的实现过程。

- Lean against writing tests for code (such as detailed HTML structure) likely to change in the future.
某些将来经常会变动的部分(比如一些HTML细节)倾向于不写测试。

- Write tests before refactoring code, focusing on testing error-prone code that’s especially likely to break.
在重构之前写测试，聚焦在代码中容易出错的部分。

In practice, the guidelines above mean that we’ll usually write controller and model tests first and integration tests (which test functionality across models, views, and controllers) second. And when we’re writing application code that isn’t particularly brittle or error-prone, or is likely to change (as is often the case with views), we’ll often skip testing altogether.

基于上面提到的指导方向，我们通常会首先写针对 controller 和 model， 接着是integration集成测试(横跨M,V,C三部分的功能测试)。如果我们写的代码是不太可能出错的或者以后经常会变动的，那么通常就不写针对这部分的测试。

Our main testing tools will be controller tests (starting in this section), model tests (starting in Chapter 6), and integration tests (starting in Chapter 7). Integration tests are especially powerful, as they allow us to simulate the actions of a user interacting with our application using a web browser. Integration tests will eventually be our primary testing technique, but controller tests give us an easier place to start.

我们主要的测试工具是 controller 测试， model 测试， 以及 integration 测试。 Integration 测试尤其强大，他会模拟用户使用浏览器与app交互的整个过程。 Integration 最终会成为我们主要关注技术，但controller test会让我们上手更容易。


**controller test 基本思路**

controller test 的测试文件通常随generate controller的操作自动生成，以对应的名称放在 app/test/controllers 目录。

首先会 `require 'test_helper'` 这个文件（就在同级目录下）

写controller的单个测试通常会以

```ruby
test "shoud <http verb> <action>" do
  <http verb> specific_url
  assert_response :success
  assert ...
  ...
end
```

这种方式，其中外层 `<http verb> <action>` 是对这个测试单元的描述。

block中的代码才是实际测试的内容
`<http verb> specific_url` 指定了以什么request请求方式进入哪个controller的那个action， 之后assert应该得到什么样的结果。

`assert_select` 方法的作用是检查有没有指定的 HTML 标签。这种方法有时也叫“选择符”（selector），因此才为这个方法取这么一个名称。

```ruby
# 应该有一个title标签，里面包含 'Introduction page.'的文本内容。
assert_select 'title', 'Introduction page.'
```

```ruby
def setup
end
```
方法中的内容会在每一个测试之前执行

**minitest-reporter gem**
https://github.com/kern/minitest-reporters

可以用来自定义测试的输出格式，方便阅读。设置也相对简单。

**guard gem**
https://github.com/guard/guard

Guard automates various tasks by running custom rules whenever file or directories are modified.

Guard 通过运行自定义的规则监听文件变动，然后基于指定的变动自动化各种任务。其中就可以用来运行测试，通过配置我们可以让guard在指定的controller或View文件发生变动时自动运行对应的测试。

下面是 配置文件 Guardfile 中的一个示例：

```ruby
guard :minitest do
  # with Minitest::Unit
  watch(%r{^test/(.*)\/?test_(.*)\.rb$})
  watch(%r{^lib/(.*/)?([^/]+)\.rb$})     { |m| "test/#{m[1]}test_#{m[2]}.rb" }
  watch(%r{^test/test_helper\.rb$})      { 'test' }
end
```

`watch` 是guard的一个用来检测文件变化的method， 接受一个 regular expression，监听到特定文件变化


比如第一行 `/^test/(.*)\/?test_(.*)\.rb$/`
- 以 `/test` 开头的
- 任意数量任意字符`(.*)` 括号会生成一个带序号 matchdata 对象也就是后面第二行 block 中用到的 `m[1]`
- `/(.*)\/` 表示 /test目录下的任意名称的目录 /test/anyfile/
- `?test_(.*)\.rb$` /test/anyfile/ 目录下的0个或1个任何名称的 .rb 文件
这其实就监听到了rails app中 /test 文件目录下的所有 rb 测试文件

```ruby
r = /a(bc).*def(ghijk)xyz/
2.5.0 :006 > r.match "abcdefghijkxyz"
 => #<MatchData "abcdefghijkxyz" 1:"bc" 2:"ghijk">
2.5.0 :007 > $0
 => "irb"
2.5.0 :008 > $1
 => "bc"
2.5.0 :009 > $2
 => "ghijk"

2.5.0 :011 > (r.match "abcdefghijkxyz")[1]
 => "bc"
2.5.0 :012 > (r.match "abcdefghijkxyz")[2]
 => "ghijk"
2.5.0 :013 >
```

配置好 Guradfile 后，终端执行 `bundle exec guard`, 会跑起来一个监听服务器

```ruby
⮀ bundle exec guard
17:01:28 - INFO - Guard::Minitest 2.4.4 is running, with Minitest::Unit 5.11.3!
17:01:28 - INFO - Guard is now watching at '/Users/caven/Programming/learnruby/sample_app'
[1] guard(main)>
```

*Guard is now watching*

**Integration test**

使用 `rails generate integration_test some_name` 会在对应的文件夹中生成集成测试文件

```
⮀ rails g integration_test site_layout
Running via Spring preloader in process 52245
     invoke  test_unit
     create    test/integration/site_layout_test.rb
```
rails 自动在给出名称的后面加上 `_test` 来命名文件

针对布局中链接的测试，要检查网站的 HTML 结构，步骤如下：

- 访问根路径（首页）； get root_path
- 确认使用正确的模板渲染； assert_template
- 检查指向首页、“帮助”页面、“关于”页面和“联系”页面的地址是否正确。 assert_select

比如 `assert_select "a[href=?]", root_path, count: 2`
这里，我们同时指定标签名 a 和属性 href，检查有没有指定的链接, 还用到了count数链接数量

`rails test:integration` 指定只执行集成测试

**可以将view中用到的helper文件引入到 test/test_helper.rb中，让测试中也能用到 helper中的方法**

```ruby
class ActiveSupport::TestCase
  include ApplicationHelper
  fixtures :all
end
```

不管是contoller还是models还是integeration中的测试文件class的继承链上游都有 ActiveSupport::TestCase

```
Loading development environment (Rails 5.1.5)
2.5.0 :001 > ActionDispatch::IntegrationTest.superclass
 => ActiveSupport::TestCase
2.5.0 :002 > ActionDispatch::IntegrationTest.superclass.superclass
 => Minitest::Test
2.5.0 :003 >
```

所以这里include到的class中的methods能在所有类型测试文件中使用。

**对model进行unit test的思路**

column存在性和格式验证

- 先在setup中写合法的 model 数据，也就是各个column是符合要求的
比如：
```ruby
def setup
  @user = User.new(name: "Example User", email: "user@example.com")
end
```
- 写一个测试验证 `@user` 在这个背景下的合法性
```ruby
test "should be valid" do
  assert @user.valid?
end
```

- 接着将其中某个column改成不合法的，断言这个新model对象合法，让测试不通过

```ruby
test "name should be present" do
  @user.name = "    "
  assert_not @user.valid?
end
```

- 到model中写对应的`validates :name, presence: true`，这会使上面的assert_not通过
- 最后测试一遍看 `validates` 是否能让测试通过。
- 其他column也依照这个模式

比如验证某个column的长度

```ruby
test "email shoud not > 255 char" do
  @user.email = "a" * 244 + "@example.com"
  assert_not @user.valid?
end
```

column 唯一性验证

思路是将一个model对象存入数据库，然后使用 `.dup` 方法复制一个，然后测试后面复制的这个对象是否合法。
```ruby
test "email addresses should be unique" do
  duplicate_user = @user.dup
  @user.save
  assert_not duplicate_user.valid?
end
```

这会失败，因为添加model验证前，复制的对象是valid的

然后到model中添加 `uniqueness: { case_sensitive: false }`（默认true为前提） 验证看测试是否通过


**assert_no_difference**

assert_no_difference 可以算 assert_euqal 的约化版

比如下面这个

```ruby
test "invalid params should not create user successfully" do
  get signup_path
  assert_no_difference 'User.count' do
    post users_path, params: { user: {
      name: "invalid",
      email: "invalid",
      ...
      } }
    end
end
```

注意 `'User.count'` 是用引号包裹的，或者要使用 `User.count.to_s`

用 assert_euqal 表示就是

```ruby
test "invalid params should not create user successfully" do
  get signup_path
  pre_count = User.count
  post users_path, params: { user: {
    name: "invalid",
    email: "invalid",
    ...
    } }
  post_count = User.count
  assert_euqal pre_count, post_count
end
```

**integration 测试中用到的 assert_select**
rails guides:

We will take a look at assert_select to query the resulting HTML of a request in the "Testing Views" section below. It is used for testing the response of our request by asserting the presence of key HTML elements and their content.

assert_select 有两种接受参数的方式

`assert_select(selector, [equality], [message])` ensures that the equality condition is met on the selected elements through the selector. The selector may be a CSS selector expression (String) or an expression with substitution values.
这种方式先要(以css selector风格，string格式)给出 selector, 只择到selector指定到的这一节点的element, 然后给出对比条件，message

`assert_select(element, selector, [equality], [message])` ensures that the equality condition is met on all the selected elements through the selector starting from the element (instance of Nokogiri::XML::Node or Nokogiri::XML::NodeSet) and its descendants.
这种方式会先让你给出一个起点element, 然后给出selector， 拿到的是从 element 到你给出的 selector 位置的所有 elements

注意如果 selector 定位的是一个容器类的元素，那么可以使用block嵌套继续select内部的元素节点

比如

In the following example, the inner assert_select for li.menu_item runs within the collection of elements selected by the outer block:
```ruby
test "presence of menu_item class" do
  assert_select 'ul.navigation' do
    assert_select 'li.menu_item'
  end
end
```

上面这种只给selector的用法也就只测试是否存在对应的元素。

**follow_redirect!**

Follow a single redirect response. If the last response was not a redirect, an exception will be raised. Otherwise, the redirect is performed on the location header.

follow controller里的指定动作的 redirect 指令到达 redirect 之后的状态。比如新的页面等。

**在测试流程中使用 raise 能够探测测试是否覆盖到`raise` 所在的方法**

```ruby
def current_user
  # @current_user ||= User.find_by(id: session[:user_id])
  if (user_id = session[:user_id])
    @current_user ||= User.find_by(id: user_id)
  elsif (user_id = cookies.signed[:user_id]) # may be nil
    raise
    user = User.find_by(id: user_id)
    if user && user.authenticated?(cookies[:remember_token])
      log_in user
      @current_user = user # may be nil
    end
  end
end
```

上面例子中插入一个 raise 然后进行测试
如果测试仍然通过，那么说明之前写的测试没有覆盖到这里
如果失败了，则说明覆盖到了这里。

**assert_math 方法在test中的应用**

`assert_match` 既可以匹配 stirng 也可以匹配 regular expression

测试用可以用来检测url或其他字段中是否包含应有的信息

`assert_match CGI.escape(user.email), mail.body.encoded`

Rails中用来将字符串转译为url中可用的字符时使用

`CGI.escape(string)`

> Rails 中该如何转义 URL，让我体会到了技术是复杂的。为了查明该怎么做，我在 Google 中搜索“ruby rails escape url”，找到了两种主要方法：URI::encode(str) 和 CGI.escape(str)。一一试过之后，我发现需要的是第二个方法。（其实还有第三种方法，ERB::Util 库提供的 url_encode 方法，具有同样的效果。）





































---

erb文件中使用 `provide` 方法可以生成指定html text content，然后与一个 symbol 联系起来

`<% provide(:title, "Home") %>`

但没有 `=` 说明实际内容并没有注入页面代码，这里的:title也不实际代表 `<title>` 标签，作用类似 variable 可以使用其他名称，但为了对应一般与 tag 名称相同。

页面中如果要插入 provide 的内容要使用

`<%= yield(:title)%>`

**Rails并不是完全遵循ruby的所有规定**

在rails中我们没有用到过 StaticPagesController.new 这样的代码

```ruby
Loading development environment (Rails 5.1.5)
2.5.0 :001 > controller = StaticPagesController.new
 => #<StaticPagesController:0x00007fdd5b6bd768 @_action_has_layout=true, @_routes=nil, @_request=nil, @_response=nil>

 2.5.0 :004 > controller.class.superclass
 => ApplicationController
2.5.0 :005 > controller.class.superclass.superclass
 => ActionController::Base
2.5.0 :006 > controller.class.superclass.superclass.superclass
 => ActionController::Metal
2.5.0 :007 > controller.class.superclass.superclass.superclass.superclass
 => AbstractController::Base
2.5.0 :008 > controller.class.superclass.superclass.superclass.superclass.superclass
 => Object
```

**Asset pipeline**

Asset pipeline 实现功能的3个核心组件：

- asset directories 资源路径
- manifest files 清单文件
- preproocessor engines 预处理引擎

1. asset directories 资源路径
The Rails asset pipeline uses three standard directories for static assets, each with its own purpose:

- app/assets: assets specific to the present application 当前app正在使用的
- lib/assets: assets for libraries written by your dev team 你的开发团队写的库
- vendor/assets: assets from third-party vendors 第三方引入的文件

Each of these directories has a subdirectory for each asset class: images, JavaScript, and Cascading Style Sheets:

上述三个路径中又会平行地分出3个相同的文件夹：

- images/
- javascripts/
- stylesheets/

2. manifest files 清单文件

在这个app中是 `app/assets/stylesheets/application.css` 这个文件

Once you’ve placed your assets in their logical locations, you can use manifest files to tell Rails (via the Sprockets gem) how to combine them to form single files. (This applies to CSS and JavaScript but not to images.)

一旦你将前端资源文件放入了1中提到的合适的文件夹中，你就可以使用清单文件来告诉rails如何将这些文件（通过 Sprockets gem）整合成一个单独的文件（application.js和application.css）

清单文件中最终要的几行是
application.css
```css
/*
 .
 .
 .
 *= require_tree .
 *= require_self
*/
```

application.js
```js
//
//= require rails-ujs
//= require turbolinks
//= require_tree .
```
The key lines here are actually CSS comments, but they are used by Sprockets to include the proper files:

这关键的几行是写在注释中的，Sprockets 会基于这些信息进行文件的整合。

Here

`*= require_tree .`

ensures that all CSS files in the app/assets/stylesheets directory (including the tree subdirectories) are included into the application CSS. The line

这行让app/assets/stylesheets中的所有文件(包含子目录)会被整合到 application.css 中

`*= require_self`
specifies where in the loading sequence the CSS in application.css itself gets included.

保证application.css中自身包含的css和通过`@import`引入的文件也被一并整合。

3. preproocessor engines 预处理引擎

当assets文件被整合好之后，rails就会通过预处理引擎处理 view 中的各个 template，通过每个文件的不同拓展名，rails会使用不同的处理组合，最主要的几种拓展名是 .scss, .coffee, .erb。

The preprocessor engines can be chained, so that
这些预处理器可以被串联

`foobar.js.coffee`

gets run through the CoffeeScript processor, and

`foobar.js.erb.coffee`

gets run through both CoffeeScript and ERb (with the code running from right to left, i.e., CoffeeScript first).
多个拓展名的情况会从最左边的开始运行。

Efficiency in production

> Asset Pipeline 带来的好处之一是，能自动优化静态资源文件，在生产环境中使用效果极佳。CSS 和 JavaScript 的**传统组织方式**是，把不同功能的代码放在不同的文件中，而且排版良好（有很多缩进）。这么做对编程人员很友好，但在生产环境中使用却效率低下--加载大量的文件会明显增加页面的加载时间，这是影响用户体验的最主要因素之一。使用 Asset Pipeline，生产环境中应用的所有样式都集中到一个 CSS 文件中（application.css），所有 JavaScript 代码都集中到一个 JavaScript 文件中（application.js），而且还会简化（minify）这些文件，删除不必要的空格，减小文件大小。这样我们就最好地平衡了两方面的需求--开发方便，线上高效。


**sass的基本用法示例**

1. 对DOM结构的模拟嵌套
```css
.center {
  text-align: center;
}

.center h1 {
  margin-bottom: 10px;
}
```
使用 Sass 可将其改写成

```css
.center {
  text-align: center;
  h1 {
    margin-bottom: 10px;
  }
}
```

```css
#logo {
  float: left;
  margin-right: 10px;
  ...
}

#logo:hover {
  color: #fff;
}
```

可以写成

```css
#logo {
  float: left;
  margin-right: 10px;
  ...
  &:hover {
    color: #fff;
  }
}
```

2. 使用variable存储经常会用到的值

比如css中多个地方用到了 `color: #777;`

那么就可以在 css 文件的顶层写

```
$light-gray: #777;
```

然后把色号替换掉，这样不用去前面找用过的色号，尤其色号很长记不住时，而且使用变量名称更好记忆。

```
color: $light-gray
```

**root_path 与 root_url**

> 本书遵守一个约定：只有重定向使用 `_url` 形式，其余都使用 `_path` 形式。（因为 HTTP 标准严格要求重定向的 URL 必须完整。不过在大多数浏览器中，两种形式都可以正常使用。）


**routes中几个参数**

get arg1, arg2(opitonal)

get 指明verb， arg1 指明url也就是网页地址实际用到的地址名称， arg2可以给出参数指出这个request送往那个controller的哪个action

比如可以将

`get 'static_pages/help'`

改写为

`get '/help', to: 'static_pages#help'`

第一种方式rails会根据给出的路由默认去到 static_pages_controller中找help，但网页地址会很啰嗦 `root/static_pages/help`

第二种方式修改了 url 让网页地址变为`root/help`，简洁直白了很多，但如果不告诉rails去到哪个controller它是无法自己知道的。

注意controller导向的参数中间使用的是 `#`， url导向的参数用的是 `/`

`as: :some_path`只是修改具名路由，比如把 help_path 改为 helf_path 但不会网页中改变实际用到的 url

**针对rails的身份验证gem**

[clearance](https://github.com/thoughtbot/clearance)
[authlogic](https://github.com/binarylogic/authlogic)
[Devise](https://github.com/plataformatec/devise)
[cancancan](  root 'static_pages#home')

**针对rails的文件上传处理 gem**

[paperclip](https://github.com/thoughtbot/paperclip)

**rails migration 操作中的 up 和 down**

http://edgeguides.rubyonrails.org/active_record_migrations.html#using-the-up-down-methods

**Rails 中 model 的class都继承自 ApplicationReord 再继承自 ActiveRecord::Base**

```ruby
2.5.0 :009 > User.class
 => Class
2.5.0 :010 > User.superclass
 => ApplicationRecord(abstract)
2.5.0 :011 > User.superclass.superclass
 => ActiveRecord::Base
2.5.0 :012 > User.superclass.superclass.superclass
 => Object
2.5.0 :013 > User.superclass.superclass.superclass.superclass
 => BasicObject
```

**update_attributes 和 update_attribute**

update_attributes 方法接受一个指定对象属性的散列作为参数，如果操作成功，会执行更新和保存两个操作（保存成功时返回 true）。注意，如果任何一个数据验证失败了，例如存储记录时需要密码（6.3 节实现），update_attributes 操作就会失败。如果只需要更新单个属性，可以使用 update_attribute 方法，跳过验证

**某个model对象的errors是一个 ActiveModel::Errors object**

```ruby
2.5.0 :003 > user = User.new(name:"", email:"")
 => #<User id: nil, name: "", email: "", created_at: nil, updated_at: nil>
2.5.0 :004 > user.valid?
 => false
2.5.0 :005 > user.errors
 => #<ActiveModel::Errors:0x00007ff56b856728 @base=#<User id: nil, name: "", email: "", created_at: nil, updated_at: nil>, @messages={:name=>["can't be blank"], :email=>["can't be blank"]}, @details={:name=>[{:error=>:blank}], :email=>[{:error=>:blank}]}>
2.5.0 :006 > user.errors.messages
 => {:name=>["can't be blank"], :email=>["can't be blank"]}
2.5.0 :007 > user.errors.messages[:email]
 => ["can't be blank"]
```

`object.errors.full_messages` 则直接把错误信息汇总到一个 array 中

```ruby
2.5.0 :004 > u.errors.messages
 => {:password=>["can't be blank", "can't be blank", "is too short (minimum is 6 characters)"], :name=>["can't be blank"], :email=>["can't be blank", "is invalid"]}
2.5.0 :005 > u.errors.full_messages
 => ["Password can't be blank", "Password can't be blank", "Password is too short (minimum is 6 characters)", "Name can't be blank", "Email can't be blank", "Email is invalid"]
2.5.0 :006 >
```

**has_secure_password**

*has_secure_password(options = {})*

Adds methods to set and `authenticate` against a BCrypt password. This mechanism requires you to have a password_digest attribute.
使用这个method需要手动建立一个 `password_digest` 栏位

If password confirmation validation is not needed, simply leave out the value for password_confirmation (i.e. don't provide a form field for it). When this attribute has a nil value, the validation will not be triggered.

如果不需要密码确认的步骤，直接不填写就可以跳过。

- 在数据库中的 password_digest 列存储安全的密码哈希值；
- 获得一对虚拟属性（不会出现在schema中），password 和 password_confirmation，而且创建用户对象时会执行这两个栏位的存在性验证和匹配验证；
- 获得 `authenticate` 方法，如果密码正确，返回对应的用户对象，否则返回 false。

has_secure_password 方法会自动在对应的模型对象中添加 authenticate 方法。这个方法会计算给定密码的哈希值，然后与数据库中 password_digest 列的值比较，以此判断用户提供的密码是否正确。

authenticate("password")方法如果验证成功会返回对应的 model object , 失败返回 false , 可以加上 `!!` 将其转换为 boolean 值。

```
2.5.0 :003 > user = User.find_by(email: "mhartl@example.com")
  User Load (0.3ms)  SELECT  "users".* FROM "users" WHERE "users"."email" = ? LIMIT ?  [["email", "mhartl@example.com"], ["LIMIT", 1]]
 => #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com", created_at: "2018-03-19 07:13:04", updated_at: "2018-03-19 07:13:04", password_digest: "$2a$10$fUlr0KwclmHNb48K9Hb6c.0AoSTzCxE/qthm0p.esOI...">
2.5.0 :004 > user.password_digest
 => "$2a$10$fUlr0KwclmHNb48K9Hb6c.0AoSTzCxE/qthm0p.esOIi3uJqi9a7W"
2.5.0 :005 > user.authenticate("foobar")
 => #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com", created_at: "2018-03-19 07:13:04", updated_at: "2018-03-19 07:13:04", password_digest: "$2a$10$fUlr0KwclmHNb48K9Hb6c.0AoSTzCxE/qthm0p.esOI...">
2.5.0 :006 > user.authenticate("fooba")
 => false

2.5.0 :007 > !!user.authenticate("foobar")
 => true
2.5.0 :008 >
```

**关于params与strong paramete**

rails中出现的 params 实际是一个 hash

比如一次报错出现的， 简化一下就是 params: { nested key/value }
```ruby
Parameters:

{"utf8"=>"✓",
 "authenticity_token"=>"jhATc2UVeRWTMc+jhtWB5nkUrGau5ZYqZA8tJNmbc9Pd7PFhWVMsjG3LxWEZdTP222+zGMJ4uP1lrC4yxK2x+Q==",
 "user"=>{"name"=>"", "email"=>"", "password"=>"[FILTERED]", "password_confirmation"=>"[FILTERED]"},
 "commit"=>"Create my account"}
```

对照起来看strong parameter 的syntax就更清楚了
`params.require(:user).permit(:user,:email,:password,:password_confirmation)`


**关于flash**

```ruby
10:   def create
11:     @user = User.new(user_params) # 非最终版本
12:     if @user.save
13:       byebug
=> 14:       flash[:success] = "Welcome to Sample App!"
15:       redirect_to @user # or user_url(@user)
16:     else
17:       render "new"
18:     end
(byebug) @user.flash
*** NoMethodError Exception: undefined method `flash' for #<User:0x00007fda03c121b0>

nil
(byebug) flash
#<ActionDispatch::Flash::FlashHash:0x00007fda02b69bd8 @discard=#<Set: {}>, @flashes={}, @now=nil>
(byebug) flash[:success] = "Succeeded."
"Succeeded."
(byebug) flash[:success]
"Succeeded."
(byebug)
```

flash 是rails内建的一个hash对象 `#<ActionDispatch::Flash::FlashHash:0x00007fda02b69bd8 @discard=#<Set: {}>, @flashes={}, @now=nil>`

前面 14 行那里实际是在往 flash 对象里写入一个键值对，之后这个flash可以在view中被用到

当flash与特定model关联的时候，flash通常会在正确的时间点出现和消失

但如果flash触发的过程中与model没有太多关联就会出现存在时间过长，比如切换页面后仍然存在的情况

注意flash对象内部有一个 `@now` 实例变量，当这个值为true的时候，flash会确保在下一次 request 发起时消失

对应到方法就是 flash.now

`flash.now[:danger] = "Invalid email/password combinations"`

**content_tag helper**

以更加简洁干净的方式用嵌入的ruby生成html代码

比如
```html.erb
<div class="alert alert-<%= message_type%>"><%= message%></div>
```
改写为
```html.erb
<%= content_tag(:div, message, class: "alert alert-#{message_type}")%>
```
也可以使用helper方法 `tag`
```html.erb
<div class="alert alert-<%= message_type%>"><%= message%></div>
```

**helper 的可用范围**

app/helpers 下的文件中写的 helper 虽然文件名上区分了controller但实际都是通用的，都可以在各个页面的 view 中使用。如果将某个 helper 文件中的module引入 application_contoller 中，对应的 helper 方法还可以在各个controller中用到


**cookies 与 session**

![](https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/Snip20180320_11.png)

- 浏览器中 session 包含于cookies中
- session 是一类特殊的 cookie, 会在关闭浏览器之后过期
- cookies 中可以以其他key名称存储cookie
- rails 的 permanent 方法可以将指定cookie的过期时间设为20y，比如 `cookies.permanent[:key]`
- rails 的 session[:key] = id 方法存入的 id 值是经过加密计算的
- 而`cookies[:key] = id` 存入的cookie值会是原值
- 所以可以用 `signed` 方法来加密。
- 所以 `cookies.permanent.signed[:key] = id` 将 id 加密后的值存入cookies，可以理解为

`cookies[:key] = id`

`BCrypt::Password.new(remember_digest).is_password?(remember_token)` 出处

[bcrypt](https://github.com/rails/rails/blob/master/activemodel/lib/active_model/secure_password.rb)

![](https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/Snip20180320_11.png
)


**Form for 如何知道什么时候使用 post 什么时候用 patch**

如果在new页面form_for会自动使用post, 而在edit页面，会换做使用patch

Active Record 提供的 `new_record?`` 布尔值方法检测用户是新创建的还是已经存在于数据库中：

```ruby
⮀ rails c --sandbox
Running via Spring preloader in process 72152
Loading development environment in sandbox (Rails 5.1.5)
Any modifications you make will be rolled back on exit
2.5.0 :001 > u = User.new
=> #<User id: nil, name: nil, email: nil, created_at: nil, updated_at: nil, password_digest: nil, remember_digest: nil>
2.5.0 :002 > u.new_record?
=> true
2.5.0 :003 > u.new_record?
=> true
2.5.0 :004 > User.first.new_record?
 User Load (0.2ms)  SELECT  "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT ?  [["LIMIT", 1]]
=> false
2.5.0 :005 >
```

所以使用 form_for 构建表单时，如果 @object.new_record? 返回 true，Rails 发送 POST 请求，否则发送 PATCH 请求。

**has_secure_password 与 allow_nil: true 的配合**

在用户信息的更新页面，会有更新密码的输入框。

实际情况是用户不可能每次都更新密码，通常表单的密码修改栏用户不会填。

但是user.rb是限制了密码存在性与最小长度的，所以不处理是会报错的。

这时可以给 validates :passowrd 后加上 allow_nil: true

一个担心是加上之后用户密码被改成了空值怎么办？

实际不会，如果user.rb中写了has_secure_password，那么这里就还会有一层验证。


**某个页面登陆后的友好转向**

web app中有很多页面的操作都需要权限，也就是登陆。比如购物网站中可以不登陆就将商品加入购物车，但点击结账的时候就必须要登陆了。

或者要参加某个网站中特定页面的活动，浏览活动信息时不需要登录，但是点击报名参加可能就需要登录。

或者没登陆的时候点击my account之类的按键，这些都会转向注册或登陆页面。

当我们注册或登陆完成后，如果页面直接跳转到了首页，而不是登陆前我们正在浏览的页面，用户就需要重复之前的操作步骤，体验会很差。

这时就需要实现友好转向。

基本的思路是

- 先在session中存好当前页面地址 if request.get?
- 接着让用户在完成操作后重新定向到 session中存的地址或者默认地址
- 转向后删除session中存的地址（如果不删除，后续登录会不断重定向到受保护的页面，用户只能关闭浏览器。）
- 将store_location方法放到非登陆用户被重定向到登陆注册页面之前的点
- 在 sessions_controller 的create动作（也就是用户登陆时）中使用 redirect_back_or 方法

```ruby
# 存储最初访问的地址
def store_location
  session[:forwarding_url] = request.original_url if request.get? # get以外的表单会发生重复提交的冲突
end

# 重定向到session中存的之前访问页面的地址或默认地址
def redirect_back_or(default)
  redirect_to(session[:forwarding_url] || default)
  session.delete(:forwarding_url)
end
```
将store_location方法放到非登陆用户被重定向到登陆注册页面之前的点

```ruby
def logged_in_user
  unless current_user
    store_location
    flash[:danger] = "Please log in."
    redirect_to login_url
  end
end
```

---

rails目前的版本5.1.5会在加入一个boolean栏位之后，ActiveRecord 会自动给一个对应名称的问号method,比如

```ruby
rails g migration add_admin_to_users admin:boolean
rails db:migrate
rails c
User.first.admin? # <----
=> false
```

---

**hidden_field_tag()name, value=nil, options={}**

rails api 中的描述：

>Creates a hidden form input field used to transmit data that would be lost due to HTTP's statelessness or data that should be hidden from the user.

由于HTTP协议规定了request是 stateless 的，没有状态，每个request都是独立分开的。当需要在相邻的request之间传递信息时就可以使用 hidden_field_tag

```ruby
<%= form_for @user, url: password_reset_path(params[:id]) do |f| %>
  <%= hidden_field_tag :email, @user.email %>
<% end %>
```

送到controller之后可以用 params[:email] 拿到email

如果使用了 `f.hidden_field_tag :email, @user.email` 就会是在 params[:user][:email]


**model对象index页面的排序**

可以在 controller 的 index 中写 `@objects = Object.all.order('created_at desc')`

也可以在model中使用 `default_scope` 方法

```ruby
  default_scope -> { order(created_at: :desc) }
```

---

**request与response对象**

rails中的 request 对象是对http request的具化。
ActionDispatch::Request

它有 `new` 方法，在app中通常不需要手动new新的request对象，new方法 require 一个string作为环境参数

```ruby
new(env)

# File actionpack/lib/action_dispatch/http/request.rb, line 56
def initialize(env)
  super
  @method            = nil
  @request_method    = nil
  @remote_ip         = nil
  @original_fullpath = nil
  @fullpath          = nil
  @ip                = nil
end
```

可以在 rails console 中尝试

```ruby
2.5.0 :003 > ActionDispatch::Request.new("string")
 => #<ActionDispatch::Request:0x00007fe5678ca498 @env="string", @filtered_parameters=nil, @filtered_env=nil, @filtered_path=nil, @protocol=nil, @port=nil, @method=nil, @request_method=nil, @remote_ip=nil, @original_fullpath=nil, @fullpath=nil, @ip=nil>
2.5.0 :004 >
```

request对象有很多可用的 methods 比如 `reset_session` , `request_method` 等

对应地

rails 还有 response 对象

```ruby
ActionDispatch::Response < Object
```

这个class也有new方法，会在测试中作为某比对条件的参考。

> Represents an HTTP response generated by a controller action. Use it to retrieve the current state of the response, or customize the response. It can either represent a real HTTP response (i.e. one that is meant to be sent back to the web browser) or a TestResponse (i.e. one that is generated from integration tests).

>Response is mostly a Ruby on Rails framework implementation detail, and should never be used directly in controllers. Controllers should use the methods defined in ActionController::Base instead. For example, if you want to set the HTTP response's content MIME type, then use ActionControllerBase#headers instead of #headers.

Response 类是rails framework层级的类，不应该被用在任何 controller 中，而只能在集成测试中被用到。

**belongs_to 和 has_many 会自动根据给出的名称去找对应名称的键**

注意 foreign_key 在被拥有一放的 table 中

在 has_many declaration 中提到 foreign_key 指的的目标table中把哪个 column 作为与自身关联的 foreign_key ，而不是说自己这边的table中有这个foreign key

在 belongs_to declaration 中提到的 foreign_key 指的就是自己表中的哪个 column 做为 foreign key

>belongs_to associations must use the singular term. If you used the pluralized form in the above example for the author association in the Book model, you would be told that there was an "uninitialized constant Book::Authors". This is because Rails automatically infers the class name from the association name. If the association name is wrongly pluralized, then the inferred class will be wrongly pluralized too.

```ruby
class User < ApplicationReord
has_many :active_relationships, class_name: "Relationship",
                               foreign_key: "follower_id",
                               dependent: :destroy

```

`:active_relationships` 是一个表达意思（表意）的写法，定义这类关联是由 user 主动发起的


```ruby
class Relationship < ApplicationRecord
  belongs_to :follower, class_name: "User" # 省略了 foreign_key: :follower_id
  # 从relationships 表出发，可以拿着 follower_id 去找对应的 user_id
  belongs_to :followed, class_name: "User" # 省略了 foreign_key: :followed_id
  # 从relationships 表出发，可以拿着 followed_id 去找对应的 user_id
end
```

上面的例子中， belongs_to :follower, class_name: "User" 这一行，rails会首先找 relationships表中的  :follower_id column, 知道这是一个 外键 foreign key 栏位，接着 class_name: "User"， 则进一步说明，这个外键关联的是 users 表，也就是拿着 follower_id 去接 users 表中的 id(其主键)。

- u = User.first
- u.active_relationships.create(followed_id: 8) 新建一条Relationship记录， follower_id 是 u.id, followed_id 是 8
- u.active_relationships
- u.active_relationships[0].follower 实际是去找这个关系中的 follower_id，从语义上可以理解为找到这段关系中的follower
- u.active_relationships[0].followed 实际是去找这个关系中的 followed_id，从语义上可以理解为找到这段关系中被follow的user

语义上的理解容易随背景的更迭而不再适用，根本的根本是要搞清楚主键和外键在哪儿，rails怎么定位主键和外键

这里由于表中有 follower_id 栏位，所以可以直接写 belongs_to :follower 而不用再说明 follower_id 栏位用作外键，但如果 belongs_to 后想换其他名称，就要说明外键比如

```ruby
class Relationship < ApplicationRecord
  belongs_to :crazy_fan, class_name: "User", foreign_key: :follower_id
  # 从relationships 表出发，可以拿着 follower_id 去找对应的 user_id
  belongs_to :famous_star, class_name: "User", foreign_key: :followed_id
  # 从relationships 表出发，可以拿着 followed_id 去找对应的 user_id
end
```

console 中的检验

- u = User.first
- u.active_relationships.create(followed_id: 8)
- u.active_relationships
- u.active_relationships[0].crazy_fan
- u.active_relationships[0].famous_star

```ruby
2.5.0 :001 > u = User.first
  User Load (0.1ms)  SELECT  "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT ?  [["LIMIT", 1]]
 => #<User id: 1, name: "Example User", email: "example@railstutorial.org", created_at: ...>

 2.5.0 :006 > u.active_relationships.create(followed_id: 8)
   (0.1ms)  SAVEPOINT active_record_1
  User Load (0.2ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
  User Load (0.1ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 8], ["LIMIT", 1]]
  SQL (0.6ms)  INSERT INTO "relationships" ("follower_id", "followed_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["follower_id", 1], ["followed_id", 8], ["created_at", "2018-03-27 03:21:19.897615"], ["updated_at", "2018-03-27 03:21:19.897615"]]
   (0.0ms)  RELEASE SAVEPOINT active_record_1
 => #<Relationship id: 1, follower_id: 1, followed_id: 8, created_at: "2018-03-27 03:21:19", updated_at: "2018-03-27 03:21:19">

2.5.0 :007 > u.active_relationships
  Relationship Load (0.2ms)  SELECT  "relationships".* FROM "relationships" WHERE "relationships"."follower_id" = ? LIMIT ?  [["follower_id", 1], ["LIMIT", 11]]
 => #<ActiveRecord::Associations::CollectionProxy [#<Relationship id: 1, follower_id: 1, followed_id: 8, created_at: "2018-03-27 03:21:19", updated_at: "2018-03-27 03:21:19">]>

2.5.0 :008 > u.active_relationships[0].crazy_fan
  Relationship Load (0.2ms)  SELECT "relationships".* FROM "relationships" WHERE "relationships"."follower_id" = ?  [["follower_id", 1]]
 => #<User id: 1, name: "Example User", email: "example@railstutorial.org", created_at: ...>

2.5.0 :009 > u.active_relationships[0].famous_star
 => #<User id: 8, name: "Dr. Santina Zboncak", email: "example-7@railstutorial.org", created_at: ...>
2.5.0 :010 >
```

上面使用 has_many + belongs_to 最后到达的点是可以阐明 这段'关系'中的， follower 和 followed

进一步实现 user.following 和 user.follower 则要使用 has_many through

```ruby
class User < ApplicationReord
has_many :active_relationships, class_name: "Relationship",
                                foreign_key: "follower_id",
                                dependent: :destroy
has_many :following, through: :active_relationships, source: :followed # or foreign_key: :followed_id， source: :user??
```

rails 中 user.following 会是一个 array ，包含user关注的所有其他用户。

**CGI.escapeHTML 在test中的引用场景**

测试时我们可能用到

```ruby
assert_match expected_content, response.body
```

来测试response页面内容中是否包含特定内容
