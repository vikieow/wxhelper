# wxhelper
wechat hook 。PC端微信逆向学习。支持3.8.0.41，3.8.1.26,3.9.0.28版本。
#### 免责声明:
本仓库发布的内容，仅用于学习研究，请勿用于非法用途和商业用途！如因此产生任何法律纠纷，均与作者无关！

#### 项目说明：
本项目是个人学习学习逆向的项目，主要参考   https://github.com/ljc545w/ComWeChatRobot ，在此基础上实现了微信的的其它版本的部分内容。

#### 实现原理：  
逆向分析PC端微信客户端，定位相关功能关键Call，编写dll调用关键Call。然后将该dll文件注入到微信进程。   
dll在注入成功时，创建了一个默认端口为19088的http服务端，然后所有的功能直接可以通过http协议调用。    

#### 使用说明：
支持的版本3.8.0.41，3.8.1.26,3.9.0.28。  
源码和主要实现在相应的分支内。  
src:主要的dll代码  
tool：简单的注入工具，一个是控制台，一个是图形界面。  
python: 简单的服务器，用以接收消息内容。    
source: 简单的命令行远程注入源码。   


0.首先安装对应的微信版本，主分支是3.8.0.41版本,分支对应相应的微信版本号.    
1.通过cmake构建成功后，将wxhelper.dll注入到微信，本地启动tcp server，监听19088端口。  
2.通过http协议与dll通信，方便客户端操作。  
3.接口的url为http://127.0.0.1:19088， 注入成功后，直接进行调用即可。  
4.特别注意数据库查询接口需要先调用获取到句柄之后，才能进行查询。  
5.相关功能只在win11环境下进行简单测试，其他环境无法保证。  
6.注意个别接口在3.8.0.41版本没有实现，具体参考源码。  
7.对应分支接口文档都是支持指定版本的，其他版本不支持，请特别注意版本。  
8.相应分支的文档对应相应版本，带有删除线的接口表示该版本的暂未实现，其他版本有实现。后续会继续实现。

#### 参与项目
个人精力和水平有限，项目还有许多不足，欢迎提出 issues 或 pr。期待你的贡献。



#### 找call方法 
个人常用的方法，请参考https://github.com/ttttupup/wxhelper/wiki  


#### 编译环境

Visual Studio 2022(x86)  

Visual Studio code   

cmake  

vcpkg
#### 构建步骤
以下是在vscode中操作，vs中的操作类似。  
1.安装vcpkg，cmake，vscode  

2.安装相应的库，如果安装的版本不同，则根据vcpkg安装成功后提示的find_package修改CMakeLists.txt内容即可。或者自己编译。
```
    vcpkg  install mongoose  
    vcpkg  install nlohmann-json
```
3.vscode 配置CMakePresets.json,主要设置CMAKE_C_COMPILER 和CMAKE_CXX_COMPILER 为cl.exe.参考如下
```
 {
            "name": "x86-release",
            "displayName": "x86-release",
            "description": "Sets Ninja generator, build and install directory",
            "generator": "Ninja",
            "binaryDir": "${sourceDir}/out/build/${presetName}",
            "architecture":{
                "value": "x86",
                "strategy": "external"
            },
            "cacheVariables": {
                "CMAKE_C_COMPILER": "cl.exe",
                "CMAKE_CXX_COMPILER": "cl.exe",
                "CMAKE_BUILD_TYPE": "Release",
                "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install/${presetName}",
                "CMAKE_TOOLCHAIN_FILE": {
                    "value": "C:/soft/vcpkg/scripts/buildsystems/vcpkg.cmake",
                     "type": "FILEPATH"
                  }
            },
            "environment": {

            }
          
        }
```
4.vscode中右键configure all  projects,在Terminal中点击Run Task，如没有先配置build任务，然后运行即可   

5.命令行注入工具，注入命令 
``` javascript
    //-i  注入程序名   -p 注入dll路径   
    // -u 卸载程序名   -d 卸载dll名称
    // -m pid  关闭微信互斥体，多开微信
    //注入  
    ConsoleInject.exe  -i demo.exe -p E:\testInject.dll
    //卸载 
    ConsoleInject.exe  -u demo.exe -d  testInject.dll
    //多开
    ConsoleInject.exe  -m 1222
```

#### 更新说明
2022-12-26 ： 增加3.8.1.26版本支持。  

2022-12-29 ： 新增提取文字功能。  

2023-01-02 ： 退出微信登录。  

2023-01-31 ： 新增修改群昵称（仅支持3.8.1.26）。  

2023-02-01 ： 新增拍一拍（仅支持3.8.1.26）。  

2023-02-04 ： 新增群消息置顶和取消置顶。  

2023-02-06 ： 新增确认收款。  

2023-02-08 ： 新增朋友圈消息。  

2023-02-09 ： 新增3.9.0.28版本基础功能。  

2023-02-13 ： 新增群昵称和微信名称。    

2023-02-17 :  新增通过wxid添加好友,搜索查找微信。  

2023-03-02 ： 新增发送@消息     

2023-03-04 ： 新增消息附件下载      

2023-03-21 ： 新增hook语音   

#### 功能预览：
0.检查是否登录    
1.获取登录微信信息    
2.发送文本    
3.发送@文本   
5.发送图片     
6.发送文件  
9.hook消息  
10.取消hook消息  
11.hook图片    
12.取消hook图片    
13.hook语音   
14.取消hook语音    
17.删除好友    
19.通过手机或qq查找微信  
20.通过wxid添加好友  
25.获取群成员  
26.获取群成员昵称  
27.删除群成员  
28.增加群成员   
31.修改群昵称   
32.获取数据库句柄      
34.查询数据库    
35.hook日志   
36.取消hook日志    
40.转发消息        
44.退出登录    
45.确认收款     
46.联系人列表      
47.获取群详情   
48.获取解密图片    
49.图片提取文字ocr    
50.拍一拍  
51.群消息置顶消息    
52.群消息取消置顶    
53.朋友圈首页    
54.朋友圈下一页    
55.获取联系人或者群名称    
56.获取消息附件（图片，视频，文件）   
#### 感谢
https://github.com/ljc545w/ComWeChatRobot  

https://github.com/NationalSecurityAgency/ghidra  

https://github.com/x64dbg/x64dbg  
