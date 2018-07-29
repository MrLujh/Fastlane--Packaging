## Fastlane--Packaging
* 自动化打包，通过fastlane自动发布

![Mou icon](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/packaging.gif)


## Fastlane安装不在这里详细罗列，参照一下链接流程
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

* fastlane init  也可以不执行 直接把fastlane整个文件夹拷贝到新的项目中 Appfile和Fastfile配置成自己的项目就可以了-----fastlane init只是生成一些配置文件

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

#--发布蒲公英上的版本配置

for_lane :pgy do
  app_identifier "com.zidongdabao.cn" # The bundle identifier of your app
  apple_id "lu287929070@163.com" # Your Apple email address 
end

```

        Fastfile文件文件代码如下：

```objc

# 定义打包平台
default_platform :ios

platform :ios do
  before_all do
    git_pull
    last_git_commit
    sh "rm -f ./Podfile.lock"
       cocoapods(use_bundle_exec: false)

end

# 运行所有的测试
  lane :test do
    scan
end

# 提交一个新的Beta版本
# 确保配置文件是最新的
lane :beta do
   
    gym 
    pilot 
end

# 将新版本部署到应用程序商店
lane :release do
   
    gym 
    deliver(force: true) 
end

# 以下是发布版本的配置

lane :pgy do
    
sigh(
        app_identifier: "com.zidongdabao.cn" #项目的bundle identifler
    )

# 开始打包    
gym(
     
    scheme: “Fastlane--Packaging”, #指定项目的scheme名称
    configuration: "Release", # 指定打包方式，Release 或者 Debug
    silent: true, # 隐藏没有必要的信息
    clean: true, # 是否清空以前的编译信息 true：是
    workspace: "Fastlane--Packaging.xcworkspace",
    include_bitcode: false, #项目中的bitcode 设置
    output_directory: './pgy', # 指定输出文件夹
    output_name: "Fastlane--Packaging.ipa", #输出的ipa名称
    export_xcargs: "-allowProvisioningUpdates”, #忽略文件
    )

# 开始上传蒲公英
pgyer(api_key: "1303c11160b475cc56b9d5df820a17ed", user_key: "dd705842c35567b3f2620e6a047024f0")

end

end

```

* pgyer(api_key: "1303c11160b475cc56b9d5df820a17ed", user_key: "dd705842c35567b3f2620e6a047024f0"),
  在蒲谷英开发平台配置中查看
  
  
* ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/error_resource/error_o4.png)

### 4.在终端执行自动打包 

     在终端输入
```objc 
fastlane pgy
```

 
      一些报错处理：
 * ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/error_resource/error__01.png)
 
 * ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/error_resource/error_02.png)
 
 * ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/error_resource/error_03.png)
 
 
     打包发布成功如下：
 
 * ![(icon)](https://github.com/daniulaolu/Fastlane--Packaging/blob/master/fastlane%20init_resource/fastlane%20init_07.png)
 
 
## AppStore版本的操作和上面流程一样，把Appfile和Fastfile文件修改成发布版本的配置
  
## fatlane --命令说明

  *  scan -- 自动运行测试工具，并且可以生成漂亮的HTML报告
 
  *  cert -- 自动创建管理iOS代码签名证书
  
  *  sigh -- 一声叹息啊，这么多年和Provisioning Profile战斗过无数次。总是有这样那样的问题导致配置文件过期或者失效。sigh是用来创建、更新、下载、    修复Provisioning Profile的工具。
  
  *  pem -- 自动生成、更新推送配置文件
  
  *  match -- 一个新的证书和配置文件管理工具。我会另写一篇文章专门介绍这个工具。他会所有需要用到的证书传到git私有库上，任何需要配置的机器直接用match同步回来就不用管证书问题了，小团队福音啊！   
  
  *  gym -- Fastlane家族的自动化编译工具，和其他工具配合的非常默契
 
  *  produce -- 如果你的产品还没在iTunes Connect(iTC)或者Apple Developer Center(ADC)建立，produce可以自动帮你完成这些工作
  
  *  deliver -- 自动上传截图，APP的元数据，二进制(ipa)文件到iTunes Connect
  
  *  pilot -- 管理TestFlight的测试用户，上传二进制文件

