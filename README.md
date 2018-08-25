# HanLP Android 示例

## portable版

portable版零配置，仅提供中文分词、简繁拼音、同义词等功能。只需在`build.gradle`中加入依赖：

```
dependencies {
    compile 'com.hankcs:hanlp:portable-1.6.8'
}
```

## 自定义版

HanLP的**全部**功能（分词、简繁、拼音、文本分类、句法分析）都兼容安卓，具体配置方法如下：

1. 下载[hanlp.jar](http://nlp.hankcs.com/download.php?file=jar)放入`app/libs`。
2. 下载[data.zip](http://nlp.hankcs.com/download.php?file=data)解压到`app/src/main/assets `，按需删除不需要的文件以减小apk体积。
3. 在程序启动时（通常是`MainApplication`或`MainActivity`的`onCreate`方法）执行初始化代码：

```java
    private void initHanLP()
    {
        try
        {
            Os.setenv("HANLP_ROOT", "", true);
        }
        catch (ErrnoException e)
        {
            throw new RuntimeException(e);
        }
        final AssetManager assetManager = getAssets();
        HanLP.Config.IOAdapter = new IIOAdapter()
        {
            @Override
            public InputStream open(String path) throws IOException
            {
                return assetManager.open(path);
            }

            @Override
            public OutputStream create(String path) throws IOException
            {
                throw new IllegalAccessError("不支持写入" + path + "！请在编译前将需要的数据放入app/src/main/assets/data");
            }
        };
    }
```

之后就可以像普通Java项目一样调用HanLP的全部功能了。

欢迎参考[HanLP文档](https://github.com/hankcs/HanLP)以了解更多信息。

![screenshot](http://wx1.sinaimg.cn/large/006Fmjmcly1fudp7hx5gnj30u01hcwgm.jpg)

