# 安装可选软件包

Haiku 最近引入了一个临时的脚本以弥补合适软件包管理的缺失。该脚本为 installoptionalpackage，允许终端用户非常容易的下载和安装 Haiku 常用的第三方软件。

## 使用要求

* 需求：最新 Haiku 版本
* 需求：网络连接

## 命令行参数

* -a 添加一个或多个软件包及所有依赖关系
* -s 显示最终需要安装的所有软件包列表
* -f 移除缓存数据，列出可供安装软件包
* -h 打印帮助信息
* -l 列出可供安装软件包

## 首次运行脚本

首次运行 `installoptionalpackage` 脚本，它会为可用的 Haiku 支持第三方软件在 `/boot/common/data/optional-packages/` 中构建数据缓存。您可以使用 `-f` 参数清除本地生成的缓存。

<pre>
~> installoptionalpackage -l
Fetching OptionalPackages ...
Fetching OptionalPackageDependencies ...
Fetching OptionalBuildFeatures ...
Generating a list of Package Names ...
...warning: Beam cannot be installed because of DevelopmentMin
...warning: Bluetooth cannot be installed
...warning: Development cannot be installed
...warning: DevelopmentBase cannot be installed
...warning: DevelopmentMin cannot be installed
...warning: ICU-devel cannot be installed because of DevelopmentMin
...warning: LibLayout cannot be installed because of DevelopmentMin
...warning: NetFS cannot be installed because of UserlandFS
...warning: UserlandFS cannot be installed
...warning: Welcome cannot be installed
...warning: WifiFirmwareScriptData cannot be installed


Available Optional Packages:
abi-compliance-checker apr apr-util beae bebook behappy beoscompatibility bepdf bezillabrowser
bzip cdrecord clockwerk clucene cmake curl cvs expat friss gettext git keymapswitcher libevent
libiconv libxml2 libxslt links mandatorypackages mercurial nano neon netsurf ocaml opensound
openssh openssl p7zip pcre pe perl python rsync sed sqlite subversion tar trackernewtemplates
transmission vim vision vlc webpositive wonderbrush xz-utils yasm
</pre>

## 安装可选软件包

如下所示，我们将安装 Vim 软件包。安装过程非常简单，打开 Haiku 终端，然后执行 `installoptionalpackage`，并附以 `-a` 参数以安装指定的软件包及其依赖关系。

<pre>
~> installoptionalpackage -a vim

To be installed:  Vim LibIconv

Installing vim-7.2-r1a2-x86-gcc2-2010-05-07.zip ...

Downloading http://haiku-files.org/files/optional-packages/vim-7.2-r1a2-x86-gcc2-201... ...

2010-05-10 17:02:43 URL:http://haiku-files.org/files/optional-packages/vim-7.2-r1a2-x86-gcc2-2010-05-07.zip [8519537/8519537] -> "vim-7.2-r1a2-x86-gcc2-2010-05-07.zip" [1]

Extracting vim-7.2-r1a2-x86-gcc2-2010-05-07.zip ...

Installing libiconv-1.13.1-r1a2-x86-gcc2-2010-04-21-a.zip ...

Downloading http://haiku-files.org/files/optional-packages/libiconv-1.13.1-r1a2-x86-... ...

2010-05-10 17:02:50 URL:http://haiku-files.org/files/optional-packages/libiconv-1.13.1-r1a2-x86-gcc2-2010-04-21-a.zip [1559805/1559805] -> "libiconv-1.13.1-r1a2-x86-gcc2-2010-04-21-a.zip" [1]

Extracting libiconv-1.13.1-r1a2-x86-gcc2-2010-04-21-a.zip ...
</pre>
