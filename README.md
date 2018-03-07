## Fastlane--Packaging
* 自动化打包，通过fastlane自动发布

## 安装不在这里详细罗列，参照一下链接流程
*  https://www.jianshu.com/p/0a113f754c09



## 操作步骤


### 1.检查Fastlane是否正确安装。输入以下命令：

```objc
fastlane --version
```
      //可以看到Fastlane版本信息，我的是fastlane 2.84.0
      
### 2.打开终端，进入你的项目工程的根目录，输入以下命令

      cd到你项目的目录,执行命令
      
```objc       
fastlane init
```
* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20init_resource/fastlane%20init_01.png)

* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20init_resource/fastlane%20init_02.png)

*  bundl update ---这一步可能时间较长
* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20init_resource/fastlane%20init_03.png)

* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20init_resource/fastlane%20init_04.png)

* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20init_resource/fastlane%20init_05.png)

* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20init_resource/fastlane%20init_06.png)

### 2.蒲公英的Fastlane插件安装
      
      打开终端，进入你的项目工程的根目录，输入以下命令：
      
```objc       
fastlane add_plugin pgyer
```

* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20add_plugin_resource/fastlane%20add_plugin_01.png)

* 这里执行完就可以了
* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20add_plugin_resource/fastlane%20add_plugin_02.png)

### 3.配置Appfile和Fastfile文件 --这里手动配置，直接复制代码就可以了，把里面的配置改成自己的项目

 
       Appfile文件代码如下：
       
```objc

for_lane :pgy do
  app_identifier "com.zidongdabao.cn" # The bundle identifier of your app
  apple_id "lu287929070@163.com" # Your Apple email address 
end

# For more information about the Appfile, see:
#     https://docs.fastlane.tools/advanced/#appfile

```

        Fastfile文件文件代码如下：

```objc

# Customise this file, documentation can be found here:
# https://github.com/fastlane/fastlane/tree/master/fastlane/docs
# All available actions: https://docs.fastlane.tools/actions
# can also be listed using the `fastlane actions` command

# Change the syntax highlighting to Ruby
# All lines starting with a # are ignored when running `fastlane`

# If you want to automatically update fastlane if a new version is available:
# update_fastlane

# This is the minimum version number required.
# Update this, if you use features of a newer version
fastlane_version "2.38.0"

default_platform :ios

platform :ios do
  before_all do
    git_pull
    last_git_commit
    sh "rm -f ./Podfile.lock"
    # cocoapods
    cocoapods(use_bundle_exec: false)

  end

  desc "Runs all the tests"
  lane :test do
    scan
  end

  desc "Submit a new Beta Build to Apple TestFlight"
  desc "This will also make sure the profile is up to date"
  lane :beta do
    # match(type: "appstore") # more information: https://codesigning.guide
    gym # Build your app - more options available
    pilot

    # sh "your_script.sh"
    # You can also use other beta testing services here (run `fastlane actions`)
  end

  desc "Deploy a new version to the App Store"
  lane :release do
    # match(type: "appstore")
    # snapshot
    gym # Build your app - more options available
    deliver(force: true)
    # frameit
  end

  # You can define as many lanes as you want

  lane :pgy do
    sigh(
        app_identifier: "com.zidongdabao.cn"
    )
    
    increment_build_number
#     increment_build_number(
#   build_number: “1” # set a specific number
# )
    gym(
    scheme: “privateExemple”,
    configuration: "Release",
    silent: true,
    clean: true,
    workspace: "privateExemple.xcworkspace",
    include_bitcode: false,
    output_directory: './pgy',
    output_name: "privateExemple.ipa",
    export_xcargs: "-allowProvisioningUpdates”,
    )

    pgyer(api_key: "1303c11160b475cc56b9d5df820a17ed", user_key: "dd705842c35567b3f2620e6a047024f0")
  end

  after_all do |lane|
    # This block is called, only if the executed lane was successful

    # slack(
    #   message: "Successfully deployed new App Update."
    # )
  end

  error do |lane, exception|
    # slack(
    #   message: exception.message,
    #   success: false
    # )
  end
end


# More information about multiple platforms in fastlane: https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Platforms.md
# All available actions: https://docs.fastlane.tools/actions

# fastlane reports which actions are used
# No personal data is recorded. Learn more at https://github.com/fastlane/enhancer

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
