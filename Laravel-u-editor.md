# laravel整合百度富文本编辑器

# [laravel-u-editor](https://segmentfault.com/a/1190000002635263) for Laravel 5.*
# [laravel-u-editor-packages](https://github.com/stevenyangecho/laravel-u-editor) for Laravel 5.*

```
sql
-- composer.json中：
"stevenyangecho/laravel-u-editor": "~1.2"
composer update
-- config/app.php，providers key中：
Stevenyangecho\UEditor\UEditorServiceProvider::class
php artisan vendor:publish

-- 模板中使用
@include('UEditor::head')
<!-- 加载编辑器的容器 -->
<script id="container" name="content" type="text/plain">
    这里写你的初始化内容
</script>
<!-- 实例化编辑器 -->
<script type="text/javascript">
    var ue = UE.getEditor('container');
        ue.ready(function() {
        ue.execCommand('serverparam', '_token', '{{ csrf_token() }}');//此处为支持laravel5 csrf ,根据实际情况修改,目的就是设置 _token 值.    
    });
</script>
```
