# laravel 缩略图处理 Intervention/image

# [Intervention/image](https://github.com/Intervention/image) for laravel 5.*

```
sql
-- 安装扩展包 image-2.3.2.tar.gz
-- 扩展包下载地址：https://github.com/Intervention/image/releases
-- 安装步骤：
1>解压该安装包，在项目vendor下新建目录intervention，并将解压的包文件image，拷贝到该目录下
2>composer中注册插件包：在项目的composer.json文件中，找到require，下面加上
	"intervention/image": "^2.3"
3>设置自动加载：在项目config文件夹中，打开app.php，找到providers，下面加上：
	Intervention\Image\ImageServiceProvider::class,
  设置类的别名：再找到aliases，下面加上：
  	'Image'     => Intervention\Image\Facades\Image::class,
4>再把intervention/image/src/config下面的config.php文件拷贝到项目的config目录，并改名image.php
```

以下是案例
```
sql
// open an image file
$img = Image::make('public/foo.jpg');
// resize image instance
$img->resize(320, 240);
// insert a watermark
$img->insert('public/watermark.png');
// save image in desired format
$img->save('public/bar.jpg');
```

下面是自己调试出来的
```
sql
/**
 * 缩略图处理
 */
public function thumb($filePath){
    if ($filePath) {
        //得到文件名
        $url = explode('/',$filePath);
        $safeName = $url[count($url)-1];
        //重新拼接路径
        unset($url[0]);
        $url_new = implode('/',$url);
//            dd($url_new);
        $thumb_path = 'uploads/images/'.date('Y-m-d', time()).'/thumb_'.$safeName;
        Image::make($url_new)
                ->resize(300, 200)
                ->save($thumb_path);
        return '/'.$thumb_path;
    }
}
```
