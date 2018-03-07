## Fastlane--Packaging
* 自动化打包，通过fastlane自动发布

![](https://github.com/daniulaolu/lujhPrivate/blob/master/resounce.png)

## podspec文件
```objc
* Pod::Spec.new do |s|
* s.name        = 'lujhPrivate'
* s.version     = '1.0.4'
* s.authors     = { 'daniulaolu' => '287929070@qq.com' }
* s.homepage    = 'https://github.com/daniulaolu/lujhPrivate'
* s.summary     = 'a dropdown menu for ios like wechat homepage.'
* s.source      = { :git => 'https://github.com/daniulaolu/lujhPrivate.git',
* :tag => s.version.to_s }
* s.license     = { :type => "MIT", :file => "LICENSE" }
* s.platform = :ios, '7.0'
* s.requires_arc = true
* s.source_files = "lujhPrivate", "*.{h,m}"
* s.resource     = 'lujhPrivate/lujh.bundle'
* s.ios.deployment_target = '7.0'
* s.frameworks   =  'QuartzCore', 'Security', 'UIKit', 'Foundation', 'CoreGraphics','CoreTelephony'
* s.dependency 'SAMKeychain'
* end
```
## 制作方法

### 1.写好代码，上传到github

      //github上创建项目仓库的时候记得创建LICENSE(许可证/授权)文件,此文件必须要有
      
### 2.创建.podspec

      cd到你项目的目录,执行命令
      
```objc       
pod spec create lujhPrivate
```

### 3.编辑.podspec
      
      按照上面的样式来编辑
      
```objc       
s.name：名称，pod search搜索的关键词,注意这里一定要和.podspec的名称一样,否则报错
s.version：版本号，to_s：返回一个字符串
s.author:作者
s.homepage:项目主页地址
s.summary: 项目简介
s.source:项目源码所在地址
s.license:许可证
s.platform:项目支持平台
s.requires_arc: 是否支持ARC
s.source_files:需要包含的源文件
s.public_header_files:需要包含的头文件
s.ios.deployment_target:支持的pod最低版本
其他一些非必要字段

s.social_media_url:社交网址
s.resources:资源文件
s.dependency:依赖库，不能依赖未发布的库
s.license= { :type => “MIT”, :file => “LICENSE” }
这里建议这样写,如果写别的会报警告,导致后面一直提交失败
```

### 4.验证.podspec

      到此检查一下你工程中有以下文件：
      
      .podspec文件,
  
      LICENSE文件

```objc
pod spec lint lujhPrivate.podspec --verbose
```

### 5.trunk需要CocoaPods 

```objc 
pod trunk me
```
      若未注册，执行以下命令，邮箱以及用户名请对号入座。用户名我使用的是Github上的用户名。
 
```objc
 // 加上--verbose可以输出详细错误信息，方便出错时查看。
      
pod trunk register example@example.com 'lujhPrivate'  --verbose
```

      注册完成之后会给你的邮箱发个邮件,进入邮箱邮件里面有个链接,需要点击确认一下。
      
      注册完成后使用pod trunk me检验注册是否成功
      
 ### 6.将自己的项目打成tag
   
      因为cocoapods是依赖tag版本的,所以必须打tag,以后再次更新只需要把你的项目打一个tag，然后修改.podspec文件中的版本接着提交到cocoapods官方就可以了,提交命令请看下面
    
      在终端执行以下命令：为git打tag, 第一次需要在前面加一个v
      
```objc 
git tag "v1.0.0" 
      
git push --tags
```
### 7.发布

```objc 
pod trunk push lujh.podspec
```
      时间较长，耐性等待，大概5-10分钟
      
###8.测试自己的cocoapods

     这个时候使用pod search搜索的话会提示搜索不到，可以执行以下命令更新本地search_index.json文件
  
```objc 
rm ~/Library/Caches/CocoaPods/search_index.json
```
     然后
     
```objc 
pod search lujhPrivate
```

## 完整命令

```objc 
pod trunk register example@example.com 'lujhPrivate'  --verbose
pod trunk me
pod spec create lujhPrivate
 
//编辑 lujhPrivate.podspec
 
pod spec lint lujhPrivate.podspec
git tag "v1.0.0"
git push --tags
pod trunk push lujhPrivate.podspec 
rm ~/Library/Caches/CocoaPods/search_index.json
pod search lujhPrivate
```

## My weixin
![(author)](https://github.com/daniulaolu/PushParameterWithDict-/blob/master/xiaolu.jpg)
