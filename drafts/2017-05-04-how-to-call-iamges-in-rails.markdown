---
title:  "Rails中图片档案的处理"
date:   2017-05-4 01:04:23
categories: [Programming]
tags: [Ruby on Rails]
---

**在rails专案中通常放图片的地方有两个:**

- app/assets/images
- public/

要注意 public文件夹和app是一个层级的文件夹。

**rails中呼叫图片有两种方式：**

- 使用 `<img src=" " alt=" ">` 也就是html的方式来呼叫
- 使用 `<%= image_tag " " %> ` 也就是rails的方式呼叫

**所以问题是：**

1. 图片到底放哪里，app/assets/images/ 还是 public ?
2. 引用图片的时候路径怎么写 ？
3. 为什么有些图片（非链接图）在development环境下可以显示但部署之后就挂了？

[stack overflow上的一个回答很有启发](http://stackoverflow.com/questions/20135841/rails-4-static-assets-in-public-or-app-assets)。但想弄清楚不同情况下到底应该怎么应对，我们可以使用一个简单的例子来验证。


### 准备工作:

`rails new image-demo`

`cd image-demo`

`git init`

`rails g scaffold user name:string description:text`

`rake db:migrate`

接着安装了 gem 'bootstrap-sass' 备用。

在 http://localhost:3000/users  页面

![](/photos/postimages/屏幕快照 2017-05-05 下午2.34.44.png)

---

## Development 环境

---

#### 1 使用html <img> tag 引用 app/assets/images/ 路径下的图片

语法上使用`<img src=" " alt=" ">` 没有疑问，但src=" " 这里的路径应该怎么写？可能的写法有

```ruby

<img src="app/assets/images/pic-A.png" alt="no pic">
<img src="assets/images/pic-A.png" alt="no pic">
<img src="/images/pic-A.png" alt="no pic">
<img src="pic-A.png" alt="no pic">
·
·
·
<img src=" " alt=" ">

```
但以上这些都无效，rails中使用html的img呼叫 app/assets/images/ 路径下的图片唯一有效的写法是：
> **`<img src="assets/pic-A.png" alt="no pic">`**   assets前也可以加个斜杠 `/`

也就是说不需要写完整的路径，但也不能直接写图片文件名，需要从assets/开始写，并要跳过中间的 image/ 路径，直接由 **assets/  加上 完整文件名，** 文件类型不能省略。

延伸一点的情况是，app/assets/images/路径下又新建了路径，比如新建了一个路径：

 **app/assets/images/subdirectory/pic-A.png**

同样是只省去中间的 images/ 路径 `<img src="assets/subdirectory/pic-A.png">` 这样即可引用成功。



#### 2 使用html <img> tag 引用 public/ 路径下的图片

**先看看直接放 public/ 目录下的图片：**

现在在此路径下有一张图： public/pic-B.png ，可能的写法有两种：

```ruby

<img src="public/pic-B.png">
<img scr="pic-B.png">

```

正确的答案是后一种 `<img scr="pic-B.png">` (也可以在前面加个斜杠`/`)也就是省去了 public/ 这个路径，直接写文件名。

如果有多层路径呢？我们新建一个 public/level-1/level-2/ 这个路径，然后试试。

这时直接写 src="pic-B.png" 就无效了，要加上后面的路径: `<img src="level-1/level-2/pic-B.png">` 。

**那么可以看到这里的规则和前面assets那里类似，这里是需要省去最前面的 public/ ，之后如果有子路径只要加上就可以生效。**

#### 3 使用rails的 iamge_tag 引用图片

**app/assets/目录下的图：**

- 使用 <%= image_tag "pic-A-out.png"%> 才能正确读取到 app/assets/images/pic-A-out.png, 其他写法一概不行，甚至在前面加个 `/` 都会失效。

- 如果有多层路径，只需要在前面加上 `子路径名称/` 就可以了，比如引用 app/assets/images/subdirectory/pic-A.png 使用的是 `<%= image_tag "subdirectory/pic-A.png"%>` 依旧省去了images/，记得不要在开头的路径前加 `/` ,比如写 /subdirectory/pic-A.png 就会失效。


**public/目录下的图：**

- 当使用 <%= image_tag "pic-B-out.png"%> 引用不到 public/ 目录下的文件，如上面所写这样rails会去app/assets/目录下找图，而这里要想使用 public/目录下的图，只需在前面加上一个斜杠`<%= image_tag "/pic-B-out.png"%>`

- 如果public/下有子路径，只要加上子路径名称就可以了，但记得要以 `/` 开头， 比如正确的写法是：`<%= iamge_tag "/level-1/level-2/pic-B.png"%>`


#### app/views/users/index.html.erb 代码参考（仅给出tbody部分）:

```ruby

<tbody>
        <tr>
          <td>app/assets/images/image-A.jpg</td>
          <td><code>img src="assets/image-A.jpg"</code></td>
          <td></td>
          <td><img src="assets/image-A.jpg"></td>                     # image-A
        <tr>

        <tr>
          <td>app/assets/images/image-A.jpg</td>
          <td></td>
          <td><code> image_tag "image-A.jpg"</code>
              <br><br>这里不能在前面加 `/` ，不然会导向public/路径
          </td>
          <td><%= image_tag "image-A.jpg"%></td>                      # image-A
        <tr>

        <tr>
          <td>app/assets/images/subdirectory/image-B.jpg</td>
          <td><code> img src="assets/subdirectory/image-B.jpg"</code></td>
          <td></td>
          <td><img src="assets/subdirectory/image-B.jpg"></td>        # image-B
        <tr>

        <tr>
          <td>app/assets/images/subdirectory/image-B.jpg</td>
          <td></td>
          <td><code> image_tag "subdirectory/image-B.jpg"</code>
              <br><br>这里不能在前面加 `/` ，不然会导向public/路径
          </td>
          <td><%= image_tag "subdirectory/image-B.jpg"%></td>         # image-B
        <tr>

        <tr>
          <td>public/image-C.jpg</td>
          <td><code>img src="/image-C.jpg"</code></td>
          <td></td>
          <td><img src="/image-C.jpg"></td>                           # image-C
        <tr>

        <tr>
          <td>public/image-C.jpg</td>
          <td></td>
          <td><code> image_tag "/image-C.jpg"</code>
              <br><br>这里前面必须加 `/` ，不然会导向app/assets/路径
          </td>
          <td><%= image_tag "/image-C.jpg"%></td>                     # image-C
        <tr>

        <tr>
          <td>public/level-1/level-2/image-D.jpg</td>
          <td><code>img src="level-1/level-2/image-D.jpg"</code></td>
          <td></td>
          <td><img src="level-1/level-2/image-D.jpg"></td>            # image-D
        <tr>

        <tr>
          <td>public/level-1/level-2/image-D.jpg</td>
          <td></td>
          <td><code> image_tag "/level-1/level-2/image-D.jpg"</code>
              <br><br>这里前面必须加 `/` ，不然会导向app/assets/路径
          </td>
          <td><%= image_tag "/level-1/level-2/image-D.jpg"%></td>     # image-D
        <tr>
  </tbody>
```

#### 本地网页端 http://localhost:3000 的效果是：

（图片字太小可以在新窗口中单独打开查看）

![](/photos/postimages/屏幕快照 2017-05-05 下午10.57.56.png)


---
**自此我们理清了在development环境下单纯引用图片的写法，下面验证这些写法在production环境中是否有效，部署使用heroku。**
---

## production 环境下图片引用

---

#### 1 本地可行的写法push到heroku 会有什么结果？

完全保留上面development环境中的代码，push到heroku,发生的结果是：

![](/photos/postimages/屏幕快照 2017-05-05 下午10.58.08.png)

**结果是使用html的 `<img src="">` 方式引用的assets/ 路径下的图片都失效了（另外张在assets/下的子目录里）**

这是为什么呢？记得文章开头提到的[stack overflow上的一个回答](http://stackoverflow.com/questions/20135841/rails-4-static-assets-in-public-or-app-assets)吗？。有一句是：

> This is as a normal browser does not have access to your app directory, but does have access to public.

因为普通的浏览器没有权限进入你的 app/ 路径，但有权限进到 public/ ， 所以被挡掉了，但 public/ 下没有assets/文件夹以及对应图片，所以浏览器找不到。

但使用 `<%= image_tag "xx"%>` 方式引用的图片都正常，因为这样写图片就是由rails来经手的，属于程序内部的调用（暂时这么理解）。

#### 2 解决问题1的方法？



##### 在本地运行 `RAILS_ENV=production bundle exec rake assets:precompile` ，然后保存，重新push heroku。这涉及到 heroku上assetspipeline的用法，[heroku上有说明文档](https://devcenter.heroku.com/articles/rails-4-asset-pipeline)。

在Rails的官方guides中有这么一段：

>In production, Rails precompiles these files to public/assets by default. The precompiled copies are then served as static assets by the web server. The files in app/assets are never served directly in production.

在production环境中，Rails会默认预压缩这些文档（app/assets/路径的所有文档）并放到 public/assets/目录。被压缩好的副本之后会作为静态档案提供给web服务器。**app/assets/中的档案绝不会在production环境中被直接提供。**



默认情况下，在push到heroku过程中，precompile是会运行的，但根据目前的尝试，结果是经常失效。预压缩的结果在部署后本地看不到，但是可以通过本地跑 `rake assets:precompile` 来模拟。运行完之后可以看到public/路径下多了一个 assets/文件夹(但下面的文件结构有差异，比如images/，stylesheets/,javascripts/ 这些路径没有了)，这里面就是 app/assets/ 中那些文档经过处理后再被放到这里供web server 使用的文档。

**执行 `rake assets:precompile`终端返回信息：**(可以看到这里新生成的文件名加上了一长串Fingerprint，这是为了避免浏览器缓存。比如我们没有修改css, js ,或者某张图片的文件名，但是对里面的内容作了修改，那么这个Fingerprint就会变化，浏览器再次载入的时候就会识别到这个变化重新去载入新的文件。)

![](/photos/postimages/屏幕快照 2017-05-06 上午8.36.46.png)

**生成的publis/assets/路径：**

![](/photos/postimages/Snip20170506_45.png)

**实际上：**

- 在development环境中，`<img src="assets/imge-A.jpg">` 实际调用的是 /assets/images/image-A.jpg  这个文件

- 在production环境中，这里的`<img src="assets/imge-A.jpg">`实际调用的是 public/assets/image-A-(fingerprint).jpg  这个文件(原来assets/images/下的图档处理后被直接释放到public/assets/目录下)

- 在development环境中，`<img src="assets/subdirectory/imge-B.jpg">`实际调用的是 /assets/images/subdirectory/image-B.jpg  这个文件

- 在production环境中，`<img src="assets/subdirectory/imge-B.jpg">`实际调用的是 public/assets/subdirectory/image-B-(fingerprint).jpg  这个文件（原来assets/images/subdirectory/下的图档处理后被放到 public/assets/subdirectory/目录下。）



---

## CSS中的图片

css中用图通常是作为背景 [background-image](https://www.w3schools.com/css/css_background.asp) 这种形式，代码的写法是`background-image: url("photo.jpg")`或者 `background-image: url("http://baseline.tennis.com/assets/build/img/content-6.jpg")` 引用网址的形式。那么在Rails专案中css文件的图片引用规则是怎样的？我们同样可以用方面的方法来进行试验。

#### 1 可以确认的是，用网址引用的方式是不会失效的。

我们要确认的主要是图片放在专案中的情况，仍然是 public/ 或者 app/assets/images/ 路径两种情况。**很容易猜到的是，既然属于html的范围，那么权限上是应该是相同的。那就是在production环境中，css里无法引用到app/assets/images/中的图片，除非经过的了assetspipeline的压缩处理放到了public/assets/中。** 但这需要验证。

准备了4张不同的图： fore.png, back.png, serve.png, smash.png

分别放到了 app/assets/images/, app/assets/images/subdirectory/, public/, public/level-1/level-2/ 四个路径下。

#### 2 在 development 下测试







---
## 图片放哪里？

[在之前写本地development下使用carrierwave上传图片的文章中](https://gitcavendish.github.io/2017/How-to-upload-multi-files-in-rails/)，carrierwave默认的存储路径是在 public/uploads/ 下:

**app/uploaders/box_uploader.rb**
```ruby
def store_dir
  "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
end
```

- 如果是用户上传的图片

在production环境中通常我们会[使用s3这类服务来存储图片](https://gitcavendish.github.io/2017/store-files-in-amazon-aws-s3/)。这类图片通常是与某个model的实例挂钩的，他们经过了carrierwave的预处理，有特定的外部键方便调用。且这部分图片可以由用户上传，体量可能会增加得很快，所以放在专门的存储服务器是更好的选择。

- 如果是用于网站前端的图片档案

 如果要放到 assets/images/ 目录下，并使用`<img src="">`方式引用的话，记得把 config.assets.compile = 设为true， 放到public/下则不影响。**如果使用`<%= image_tag ""%>`方式引用的话则无所谓，只是注意路径格式中的`/`符号。**


---

## 总结：

1. 如果你有一张图 **app/assets/images/photo.jpg** 使用html `<img src="assets/photo.jpg ">` 写法，在 **本地development环境** 时会先去 public/assets/ 路径下找图，找不到的话再会去 app/assets/images/ 中找。这种写法想要在 **production环境** 中生效，需要在config/environments/production.rb 中将 config.assets.compile = 设为 true。

2. 如果使用Rails的 `<%= iamge_tag " "%>` 来写在两种环境中都生效。但有一点区别是关于斜杠`/`符号的，如果路径前加了 `/` 那rails就会去public 中找， 如果没有加rails会去 assets/images/ 中去找。

3. 如果是用于网站前端的图片档案，放在app/assets/images/ 或者 public/ 下都可以，只是注意 html 和 Rails 两种引用方式在配置上的不同和路径写法的区别。

4. 如果是用户上传的图档，建议使用carrierwave配合s3这类服务进行储存和管理。


**有时我们直接使用引用网址的方式来调用图片:**

![](/photos/postimages/屏幕快照 2017-05-06 上午11.22.44.png)

html方式：`<img src="https://gentle-peak-91259.herokuapp.com/assets/image-A.jpg">`

rails方式：`<%= image_tag "https://gentle-peak-91259.herokuapp.com/assets/image-A.jpg"%>`

两种写法都有效。在rails专案中为了节省脑力，可以全部都使用`<%= image_tag ""%>`的写法。**

---

上面内容说的都是在html.erb文件中的引用，那么css文件中的图档呢？<%= iamge_tag%> 的更多用法？下面是一些参考资料：

---

> “2 How to Use the Asset Pipeline

> In previous versions of Rails, all assets were located in subdirectories of public such as images, javascripts and stylesheets. With the asset pipeline, the preferred location for these assets is now the app/assets directory. Files in this directory are served by the [Sprockets](https://github.com/rails/sprockets-rails) middleware.

> Assets can still be placed in the public hierarchy. Any assets under public will be served as static files by the application or web server when config.public_file_server.enabled is set to true. You should use app/assets for files that must undergo some pre-processing before they are served.

>In production, Rails precompiles these files to public/assets by default. The precompiled copies are then served as static assets by the web server. The files in app/assets are never served directly in production.
>> 摘录来自: Ruby on Rails. “Ruby on Rails Guides v2”。 iBooks. [网址链接](http://guides.rubyonrails.org/asset_pipeline.html#manifest-files-and-directives)

**在之前的Rails版本中，诸如iamge,javascripts 以及 stylesheets等所有的assets都被置于app/public/路径下的子目录。伴随asset pipeline的引入，现在这些档案的优先位置在 app/assets/路径下。此路径下的档案由中间软件sprockets来进行处理。**
**Assets档案仍然可以放在app/public/路径下。当 config.public_file_server.enabled 被设置为 true 的时候，任何app/public/路径下的assets都会被应用程式或服务器视作|静态档案|,你应该将那些必须经过预处理之后再被调用的档案放在app/assets/路径下。**
**在production环境中，Rails会默认预先压缩这些(app/assets/路径下的)档案，并存放到app/public/assets/路径下，压缩好的档案随后会作为静态assets提供给服务器使用。app/assets/路径下的档案绝不会直接在production环境中被调用。**

---

> 2.3.1 CSS and ERB

The asset pipeline automatically evaluates ERB. This means if you add an erb extension to a CSS asset (for example, application.css.erb), then helpers like asset_path are available in your CSS rules:

.class { background-image: url(<%= asset_path 'image.png' %>) }
This writes the path to the particular asset being referenced. In this example, it would make sense to have an image in one of the asset load paths, such as app/assets/images/image.png, which would be referenced here. If this image is already available in public/assets as a fingerprinted file, then that path is referenced.

If you want to use a data URI - a method of embedding the image data directly into the CSS file - you can use the asset_data_uri helper.

#logo { background: url(<%= asset_data_uri 'logo.png' %>) }
This inserts a correctly-formatted data URI into the CSS source.

Note that the closing tag cannot be of the style -%>.

> 2.3.2 CSS and Sass

When using the asset pipeline, paths to assets must be re-written and sass-rails provides -url and -path helpers (hyphenated in Sass, underscored in Ruby) for the following asset classes: image, font, video, audio, JavaScript and stylesheet.

image-url("rails.png") returns url(/assets/rails.png)
image-path("rails.png") returns "/assets/rails.png"
The more generic form can also be used:

asset-url("rails.png") returns url(/assets/rails.png)
asset-path("rails.png") returns "/assets/rails.png"

> 2.3.3 JavaScript/CoffeeScript and ERB

If you add an erb extension to a JavaScript asset, making it something such as application.js.erb, you can then use the asset_path helper in your JavaScript code:

$('#logo').attr({ src: "<%= asset_path('logo.png') %>" });
This writes the path to the particular asset being referenced.

Similarly, you can use the asset_path helper in CoffeeScript files with erb extension (e.g., application.coffee.erb):

$('#logo').attr src: "<%= asset_path('logo.png') %>"

---

> “3.1.4 Linking to Images with the image_tag

The image_tag helper builds an HTML <img /> tag to the specified file. By default, files are loaded from public/images.

Note that you must specify the extension of the image.

<%= image_tag "header.png" %>

You can supply a path to the image if you like:”

“<%= image_tag "icons/delete.gif" %>

You can supply a hash of additional HTML options:

<%= image_tag "icons/delete.gif", {height: 45} %>

You can supply alternate text for the image which will be used if the user has images turned off in their browser. If you do not specify an alt text explicitly, it defaults to the file name of the file, capitalized and with no extension. For example, these two image tags would return the same code:


<%= image_tag "home.gif" %>
<%= image_tag "home.gif", alt: "Home" %>

You can also specify a special size tag, in the format "{width}x{height}":

<%= image_tag "home.gif", size: "50x20" %>

In addition to the above special tags, you can supply a final hash of standard HTML options, such as :class, :id or :name:”


“<%= image_tag "home.gif", alt: "Go Home",
                          id: "HomeImage",
                          class: "nav_bar" %>
”

---

> 6.1.2 image_path

Computes the path to an image asset in the app/assets/images directory. Full paths from the document root will be passed through. Used internally by image_tag to build the image path.

image_path("edit.png") # => /assets/edit.png

Fingerprint will be added to the filename if config.assets.digest is set to true.

image_path("edit.png") # => /assets/edit-”


> 6.1.3 image_url

Computes the URL to an image asset in the app/assets/images directory. This will call image_path internally and merge with your current host or your asset host.


image_url("edit.png") # => http://www.example.com/assets/edit.png

> 6.1.4 image_tag

Returns an HTML image tag for the source. The source can be a full path or a file that exists in your app/assets/images directory.


image_tag("icon.png") # => `<img src="/assets/icon.png" alt="Icon" />`



---
