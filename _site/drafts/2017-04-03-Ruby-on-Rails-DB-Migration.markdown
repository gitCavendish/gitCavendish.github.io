## 这里集合所有关于 DB Migration 中问题的解法以及相关基础知识。

---

#### 如何更改某个 model 的其中一个 column 的 数据类型（style）和默认值(default value)

step 1

`rails g migration change _ xxx _ column _ xxx`
(注： 'change _ xxx _ column _ xxx' 这部分只是一个文件名称，没有规定，按照你认为容易理解的方式写就好)

step 2

在上面一步生成的 .rb 文件中的：
`def change`

`end`
中间填入：
`change_column :table_name, :column_name, :column_style, default: 1`

step 3

在终端运行 `rake db:migrate`
然后查看 db/schema 中的表格看是否修改成功。

step2的图例解释。

![屏幕快照 2017-02-08 下午10.41.55.png](http://user-image.logdown.io/user/22166/blog/21189/post/1396447/ZjXo2AsZRXmC2yWrl7xO_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-02-08%20%E4%B8%8B%E5%8D%8810.41.55.png)

在上图中：
table _ name  对应  products
column _ name 对应  title, description, quantity......绿色带引号的都是
column _ style 对应 行首的 string, text, integer.......
default 就是这些column的默认值，上图中只有 quantity 有个默认值为 1
想要修改某一个column的数据类型和默认值就是这样。

#### 那么，同样的，想修改某一个 model 中的某个 column 的名称

将 step 2 中的代码替换为

`rename_column :旧的column名称, :新的column名称`
然后
`rake db:migrate`

#### 不改 column 的 style 只改 default 值

`change_column_default :table名称, :column名称, 1`
（后面的 ‘1’ 就是你要修改的默认值）
