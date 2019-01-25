title: iOS笔记(16)
date: 2013-03-01 02:38:47
tags: iOS
---


# 配置自己的CocoaPods库

## 序

默认安装的cocoapods确实很好用，可是毕竟自己会写一些库和修改一些第三方库来用。所幸cocoapods确实是一个神器。他可以定义自己的库来用。
如何安装Cocoapods，[请参考这篇](https://iiiyu.com/2013/03/01/learning-ios-notes-sixteen/)


<!--more-->

## 从头来设置

### 应用场景
我的boss写了一个基于MagicalRecord的CoreData的iCloud设置的类。也许我们以后要用到的机会毕竟多。单独拿出来成为一个库。现在用cocoapods来做成一个静态库。 我们这个库基于外部的MagicalRecord和SVProgressHUD.这两个库是用git submodule的方法来管理的



#### 第一步把代码托管到一个支持git的服务器

我选择了[bitbucket](https://bitbucket.org).因为没钱买github。

#### 第二步删除git submodule
git submodule虽然也可以实现第三方库的管理，但是相对于cocoapods来说太麻烦了。删除就不是想cocoapods这样在Podfile里面删除配置就好的。


**先删掉目录**

```
$git rm --cached Vendor/MagicalRecord
$git rm --cached Vendor/SVProgressHUD
$rm -rf Vendor
```

**删掉.gitmodules**

```
$rm .gitmodules
```

**修改.git/config**

把MagicalRecord和SVProgressHUD的条目删除

**最后commit一下**

```
$ git add .
$ git commit -m "Remove a submodule"
```

#### 第三步为原来的项目添加cocoapods支持

**先把项目里面引用submodule的地方删除**

**添加Podfile到项目目录里面**

内容为

```
platform :ios,  '6.0'
pod 'MagicalRecord', :git => 'https://github.com/iiiyu/MagicalRecord.git', :tag => 'sumiGridDiary2.1'
pod 'SVProgressHUD'
```

解释一下：
第一行后面是限制模拟器版本为6.0以上。
第二行因为MagicalRecord我们改了几行代码，因此用我们自己的版本。指定git地址。和tag标签。除了tag还可以指定branch和commit。格式一样
第三行用官方的最新版本


**安装Pod**

```
$pod install
```

我的安装结束以后提示

```
[!] The target `SIStore [Debug - Release]' overrides the `HEADER_SEARCH_PATHS' build setting defined in `Pods/Pods.xcconfig'.
    - Use the `$(inherited)' flag, or
    - Remove the build settings from the target.
```

按照提示修改项目的Build Settings就ok了。

**进入项目中把之前是包入“”的第三方头文件地方改为<>**

**添加.gitignore**

.gitignore内容为

```
*.xcodeproj/*
!*.xcodeproj/project.pbxproj
build
.DS_Store
._*
.svn
*.xcworkspace
Pods
Podfile.lock
```

打完收工。

这样就之前的库就可以跑在cocoapods的配置下了


#### 第四步创建自己的Podspec文件

初始化一个Podspec文件

```
$pod spec create SIStore
```

SIStore.podspec内容如下

```
#
# Be sure to run `pod spec lint SIStore.podspec' to ensure this is a
# valid spec.
#
# Remove all comments before submitting the spec. Optional attributes are commented.
#
# For details see: https://github.com/CocoaPods/CocoaPods/wiki/The-podspec-format
#
Pod::Spec.new do |s|
  s.name         = "SIStore"
  s.version      = "0.0.1"
  s.summary      = "A short description of SIStore."
  # s.description  = <<-DESC
  #                   An optional longer description of SIStore
  #
  #                   * Markdown format.
  #                   * Don't worry about the indent, we strip it!
  #                  DESC
  s.homepage     = "http://EXAMPLE/SIStore"

  # Specify the license type. CocoaPods detects automatically the license file if it is named
  # `LICEN{C,S}E*.*', however if the name is different, specify it.
  s.license      = 'MIT (example)'
  # s.license      = { :type => 'MIT (example)', :file => 'FILE_LICENSE' }
  #
  # Only if no dedicated file is available include the full text of the license.
  #
  # s.license      = {
  #   :type => 'MIT (example)',
  #   :text => <<-LICENSE
  #             Copyright (C) <year> <copyright holders>

  #             All rights reserved.

  #             Redistribution and use in source and binary forms, with or without
  #             ...
  #   LICENSE
  # }

  # Specify the authors of the library, with email addresses. You can often find
  # the email addresses of the authors by using the SCM log. E.g. $ git log
  #
  s.author       = { "Xiao ChenYu" => "apple.iiiyu@gmail.com" }
  # s.authors      = { "Xiao ChenYu" => "apple.iiiyu@gmail.com", "other author" => "and email address" }
  #
  # If absolutely no email addresses are available, then you can use this form instead.
  #
  # s.author       = 'Xiao ChenYu', 'other author'

  # Specify the location from where the source should be retrieved.
  #
  s.source       = { :git => "http://EXAMPLE/SIStore.git", :tag => "0.0.1" }
  # s.source       = { :svn => 'http://EXAMPLE/SIStore/tags/1.0.0' }
  # s.source       = { :hg  => 'http://EXAMPLE/SIStore', :revision => '1.0.0' }

  # If this Pod runs only on iOS or OS X, then specify the platform and
  # the deployment target.
  #
  # s.platform     = :ios, '5.0'
  # s.platform     = :ios

  # ――― MULTI-PLATFORM VALUES ――――――――――――――――――――――――――――――――――――――――――――――――― #

  # If this Pod runs on both platforms, then specify the deployment
  # targets.
  #
  # s.ios.deployment_target = '5.0'
  # s.osx.deployment_target = '10.7'

  # A list of file patterns which select the source files that should be
  # added to the Pods project. If the pattern is a directory then the
  # path will automatically have '*.{h,m,mm,c,cpp}' appended.
  #
  # Alternatively, you can use the FileList class for even more control
  # over the selected files.
  # (See http://rake.rubyforge.org/classes/Rake/FileList.html.)
  #
  s.source_files = 'Classes', 'Classes/**/*.{h,m}'

  # A list of file patterns which select the header files that should be
  # made available to the application. If the pattern is a directory then the
  # path will automatically have '*.h' appended.
  #
  # Also allows the use of the FileList class like `source_files' does.
  #
  # If you do not explicitly set the list of public header files,
  # all headers of source_files will be made public.
  #
  # s.public_header_files = 'Classes/**/*.h'

  # A list of resources included with the Pod. These are copied into the
  # target bundle with a build phase script.
  #
  # Also allows the use of the FileList class like `source_files' does.
  #
  # s.resource  = "icon.png"
  # s.resources = "Resources/*.png"

  # A list of paths to preserve after installing the Pod.
  # CocoaPods cleans by default any file that is not used.
  # Please don't include documentation, example, and test files.
  # Also allows the use of the FileList class like `source_files' does.
  #
  # s.preserve_paths = "FilesToSave", "MoreFilesToSave"

  # Specify a list of frameworks that the application needs to link
  # against for this Pod to work.
  #
  # s.framework  = 'SomeFramework'
  # s.frameworks = 'SomeFramework', 'AnotherFramework'

  # Specify a list of libraries that the application needs to link
  # against for this Pod to work.
  #
  # s.library   = 'iconv'
  # s.libraries = 'iconv', 'xml2'

  # If this Pod uses ARC, specify it like so.
  #
  # s.requires_arc = true

  # If you need to specify any other build settings, add them to the
  # xcconfig hash.
  #
  # s.xcconfig = { 'HEADER_SEARCH_PATHS' => '$(SDKROOT)/usr/include/libxml2' }

  # Finally, specify any Pods that this Pod depends on.
  #
  # s.dependency 'JSONKit', '~> 1.4'
end
```

注释里面描写的很详细，建议全部看完

去掉注释版本

```
Pod::Spec.new do |s|
  s.name         = "SIStore"
  s.version      = "0.0.1"
  s.summary      = "A short description of SIStore."
  s.homepage     = "http://EXAMPLE/SIStore"
  s.license      = 'MIT (example)'
  s.author       = { "Xiao ChenYu" => "apple.iiiyu@gmail.com" }
  s.source       = { :git => "http://EXAMPLE/SIStore.git", :tag => "0.0.1" }
  s.source_files = 'Classes', 'Classes/**/*.{h,m}'
end
```

第一行和最后一行保留下来然后不管就好了

* s.name 声明库的名称
* s.version 库的版本
* s.summary 一个简短的说明文档
* s.homepage 库的首页
* s.license 库的协议
* s.author 作者
* s.source 原代码的地址
* s.source_files 原代码的目录

我们要认真填写的有s.name、s.version、s.source、s.source_files。我们有依赖其他库所以还要写s.dependency。还有一个bundle还要写s.resourcs。



最终的结果内容如下:

```
Pod::Spec.new do |s|
  s.name         = "SIStore"
  s.version      = "0.0.2"
  s.summary      = "Sumi Interactive make a new CoreData and iCloud a Third-party library on MagicalRecord."
  s.homepage     = "https://iiiyu.com"
  s.license      = 'MIT'
  s.author       = { "Xiao ChenYu" => "apple.iiiyu@gmail.com" }
  s.source       = { :git => "https://iiiyu@bitbucket.org/iiiyu/sistore.git", :tag => "0.0.2" }
  s.source_files = 'SIStore/*.{h,m}'
  s.preserve_paths  = 'SIStoreDemo'
  s.resources    = 'SIStore/SIStore.bundle'
  s.framework    = 'CoreData'
  s.dependency 'MagicalRecord', :git => 'https://github.com/iiiyu/MagicalRecord.git', :tag => 'sumiGridDiary2.1'
  s.dependency 'SVProgressHUD'
  s.requires_arc = true
  s.platform     = :ios
end
```


## 坑
cocoapods有缓存。 我一直测试刚刚搞好的自己的库一直不对。 改了2小时。 才反应过来我一直打的标签是同一个，然后cocoapods在本地缓存了一个。只对比了标签以至于我改的东西都没有用。哭。。。。



## 参考文章

[如何编写一个CocoaPods的spec文件](http://ishalou.com/blog/2012/10/16/how-to-create-a-cocoapods-spec-file/)

[CocoaPods一个Objective-C第三方库的管理利器](http://m.blog.csdn.net/blog/totogo2010/8198694)
