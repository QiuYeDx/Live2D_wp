## 前言

这几年已经有许多大佬在自家博客引入看板娘了，而且相关仓库也比较丰富。不过都不完美，各有一些问题，且已经数年没有更新相关教程了，尤其是在WordPress中引入，会有许多格外的步骤需要做，我已经摸着石头过河了，接下来就把方法介绍给有兴趣的朋友们吧～

## 步骤

### 准备工作

在我的仓库中下载源文件，并把live2d文件夹上传至自己的服务器，置于博客文件夹下。PS：可以放在目录下的任意位置，比如根目录(…/wp-blog/)或主题目录(…/wp-blog/wp-content/themes/${主题名字}/)下。

### 正式开始

在当前主题的functions.php文件中找到这几行：
```php
function my_style__scripts() {	
	wp_enqueue_script('jquery');	
        ......
}
```
为了能正确调用jQuery并正确添加JS包，在“……”的位置添加以下语句：

```php
wp_enqueue_style( 'live2d-css', get_template_directory_uri() . '/live2d/css/live2d.css' );
wp_enqueue_script( 'live2d', get_template_directory_uri() . '/live2d/js/live2d.js', array('jquery', "jquery-ui-accordion", "jquery-ui-core", "jquery-ui-tabs"), '', true );
wp_enqueue_script( 'message', get_template_directory_uri() . '/live2d/js/message.js', array('jquery', "jquery-ui-accordion", "jquery-ui-core", "jquery-ui-tabs"), '', false );
```
注意引号中的路径（比如’/live2d/css/live2d.css’）是相对于当前主题的路径的（即…/wp-blog/wp-content/themes/${主题名字}/live2d/css/live2d.css），要匹配到你的live2d的位置。

然后在你博客当前主题的页脚文件（footer.php）引入脚本，在 body 标签结束前插入如下代码：

```php
<script type="text/javascript">
    var message_Path = '/live2d/'
    var home_Path = 'https://qiuyedx.com/'  //此处修改为你的域名，必须带斜杠
</script>
<script type="text/javascript">
    loadlive2d("live2d", "/live2d/model/tia/model.json"); 
    //注意此处路径是相对于博客系统根目录的，即.../wp-blog/live2d/model/tia/model.json"，按自己放的位置填写即可
</script>
```
到这里以后，还有一个天坑，有些同学会发现文字一直不显示，只有模型在，那么请在message.js中把所有代码放在这个花括号中：

```js
jQuery(document).ready(function($){
    //全部代码都放在这里
}
```
最后，鼠标放在页面某个元素上时，需要 Live2D 看板娘提示的请修改 message.json 文件。后续也可以添加其他Live2D素材包，在此修改json路径即可。

示例：

```json
{
    "mouseover": [
        {
            "selector": ".title a",  //此处修改为你页面元素的标签名
            "text": ["要看看 {text} 么？"]  //此处修改为你需要提示的文字
        },
        {
            "selector": "#searchbox",
            "text": ["在找什么东西呢，需要帮忙吗？"]
        }
    ],
    "click": [  //此处是 Live2D 看板娘的触摸事件提示
        {
            "selector": "#landlord #live2d",
            "text": ["不要动手动脚的！快把手拿开~~", "真…真的是不知羞耻！","Hentai！", "再摸的话我可要报警了！⌇●﹏●⌇", "110吗，这里有个变态一直在摸我(ó﹏ò｡)"]
        }
    ]
}
```
到此为止。刷新你的博客页面，看看效果吧！
