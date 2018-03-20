## How-do-I-brew-install-local-archive

===========================================

在Mac OSX系统上，通过Homebrew安装webp包时候，执行命令 "brew install webp"，报错如下：

    `Error: Failed to download resource "webp"
    Download failed: https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-0.6.1.tar.gz`

Homebrew通过curl下载storage.googleapis.com资源失败。
想到一种解决途径是 
1. 自由发挥翻墙、代理方法将webp安装包下载到本地；
2. brew通过本地文件安装

*以下操作过程以webp安装包为例，类似问题，将需要的安装包名替换掉webp即可*

+ 步骤一： 关闭brew自动更新功能<br/>

>由于brew每次命令执行都自动更新 Updating Homebrew ..., 速度很慢，我们先尝试关掉brew的自动更新设置
> `vi ~/.bash_profile`
>
> 在文档末尾追加以下设置
>
> `#brew install without updating`
> `export HOMEBREW_NO_AUTO_UPDATE=1`
>
> 保存并关闭.bash_profile文件，执行以下命令，让修改生效
> `source .bash_profile`

+ 步骤二：代理下载安装文件libwebp-0.6.1.tar.gz<br/>

> 通过代理proxychains4 下载文件并保存为 libwebp-0.6.1.tar.gz

> `proxychain4s curl -O "https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-0.6.1.tar.gz"`


+ 步骤三：配置brew强制从本地文件安装<br/>

> 查看brew下载文件的缓存地址
>
> `brew --cache`
>
> 假设为/absolute/path/of/brew/cached/
>
> `mv ./libwebp-0.6.1.tar.gz /absolute/path/of/brew/cached/`
>
>
>
> 编辑webp的formula设置
> `brew edit webp`
> 
> 将
>    `url "https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-0.6.1.tar.gz"` 
> 替换为<br/>
>   `url "file:///absolute/path/of/brew/cached/libwebp-0.6.1.tar.gz"`


搞定！ 执行 `brew install webp`

P.S. proxychains4工具的安装及配置:   [点击查看](https://yq.aliyun.com/articles/27007)
