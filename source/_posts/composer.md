---
title: ubuntu 16.04 composer的安装，卸载及使用国内镜像
date: 2017-6-1 14:48
tags: [ubuntu,composer,php]
---

### 安装
composer的安装比较简单，执行如下命令就可以

``` bash
$ apt-get install composer
```
### 卸载

有时候composer在使用过程中产生了一些错误，或者认为操作产生的错误，影响到composer的继续使用，这是不得不卸载重装（简单粗暴但是有效）。
首先卸载composer

``` bash
$ apt-get remove composer
```
然后把composer依赖的你不再需要的东西也删掉

``` bash
$ apt-get autoremove
```
### 使用国内镜像
由于composer的源镜像是在国外，导致我们在执行composer install 的时候会出现超时，而无法安装完成，比如腾讯云或阿里云服务器，以下为出错信息


>root@VM-180-14-ubuntu:/# composer install
>Loading composer repositories with package information
>Updating dependencies (including require-dev)
>Failed to decode response: zlib_decode(): data error
>Retrying with degraded mode, check https://getcomposer.org/doc/articles/troubleshooting.md#degraded-mode for more info
>
>	[Composer\Downloader\TransportException]                                     
>  	Your configuration does not allow connection to http://packagist.org. See h  
>  	ttps://getcomposer.org/doc/06-config.md#secure-http for details.             
>                                                                               
>
>install [--prefer-source] [--prefer-dist] [--dry-run] [--dev] [--no-dev] [--no-plugins] [--no-custom-installers] [--no-autoloader] [--no-scripts] [--no-progress] [-v|vv|vvv|--verbose] [-o|--optimize-autoloader] [-a|--classmap-authoritative] [--ignore-platform-reqs] [--] [<packages>]...

##### 有两种方式启用本镜像服务：


系统全局配置： 即将配置信息添加到 Composer 的全局配置文件 config.json 中。见“方法一”
单个项目配置： 将配置信息添加到某个项目的 composer.json 文件中。见“方法二”

方法一： 修改 composer 的全局配置文件（推荐方式）

打开命令行窗口（windows用户）或控制台（Linux、Mac 用户）并执行如下命令：

```bash
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
方法二： 修改当前项目的 composer.json 配置文件：

打开命令行窗口（windows用户）或控制台（Linux、Mac 用户），进入你的项目的根目录（也就是 composer.json 文件所在目录），执行如下命令：

```
composer config repo.packagist composer https://packagist.phpcomposer.com
```
上述命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息（你也可以自己手工添加）：

```
"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://packagist.phpcomposer.com"
    }
}
```