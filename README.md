# SeuWebHelper
东南大学校内无线局域网自动登录脚本，防电脑断链，永久保持在线

# Updates

- ![#f03c15](https://placehold.it/15/f03c15/000000?text=[使用提示]受校园网近期安全协议波动影响，请及时更新本软件以保障自动登录功能的正常使用！旧版本已不再适用)


*  1228 新增对Invalid_Location的故障自检
*  1228 新增对校园网SSL安全协议的自检支持
*  1220 新增在网络连接故障时的自动修复功能（重启后尤为有用）
*  1204 新增对EXE文件无参数运行模式的支持
*  1204 修复更换用户登录时，仍使用上一用户状态的BUG
*  1204 删除冗余提示信息，检测提示新增时间戳
*  1128 新增对命令行参数调用的支持
*  1128 修复长时间定时KeepAlive可能导致计时器失效的问题
*  1128 新增简易版执行程序`校园网登录.exe`

# Requirements

请使用Python3.5以上版本

# Tutorial

## 傻瓜式操作 使用教程

如果没有学过Python，本项目提供的`校园网登录.exe`可以拿过来直接使用，不用任何的环境配置。

<b>（1204更新支持）</b> 使用时，只需按照EXE提示输入您的个人校园网信息即可，将`校园网登录.exe`发送到桌面快捷方式能节省您的更多时间。<b>如果您仅需使用本软件，请勿往下看更多信息！</b>

使用`校园网登录.exe`时，您也可以选择给它传递参数，以避免每次打开都要重新输入个人信息的麻烦，和下文即将介绍的`命令行调用`章节一样，在调用时需要给他传递用户名、密码、检测间隔（可选）这三个参数项。使用的步骤如下（以任意用户为例）：

1. 右键`校园网登录.exe`，发送到桌面快捷方式
2. 回到桌面
3. 桌面上右键`校园网登录.exe`的快捷方式，点击属性
4. 属性对话框弹出
5. 在`目标`文本框中追加一段` 220170000 123456 15`
6. 最终修改为以下文字： `"D:\Program Files\校园网登录.exe" 220170000 123456 15` ，原来位置字串会有双引号，参数必须放双引号后
7. 确定，保存，完美使用！

## 通用 使用教程

安装`requirements.txt`中的依赖包，安装代码如下，请务必在本脚本文件夹下，使用管理员权限打开命令提示符，输入以下命令。

	pip install -r requirements.txt

依赖包安装后，请编辑`main.py`中的内容，将其中的用户名密码替换成自己的，然后根据实际情况取消下面几行控制代码的注释。

举例：每15分钟维护一次登录状态，发现被踢出后，自动重登录。

	helper = seuhelper("220170000", "123456") <- 这里是你的用户名密码，用来生成登录器对象

	# 单次操作：登录账户
	# helper.command_login()

	# 单次操作：注销账户
	# helper.command_logout()

	# 部署：10分钟检测一次登录状态，注销的话就自动重登
	helper.keep_alive(15) <- 这边需要取消注释，这样就可以激活操作：每15分钟更新一次登录状态，防掉线 （里面数字为分钟单位）
  
修改后请保存脚本，在当前文件夹下打开命令提示符，运行

	python main.py

此时只需将命令提示符最小化，脚本会自动地每15分钟检查一下你的登录状态，若被校园网踢出，脚本会自动重新登录！

本脚本还支持单次登录、单次注销两个额外功能，一般用来测试，对本脚本感兴趣的同学可以自行研究一下。

## 命令行调用 使用教程

1128更新的命令行参数调用模式允许用户使用命令行参数控制脚本动作。

举例来说，学号220170000，密码123456的同学想要以15分钟间隔保持自己的校园网在线状态，他可以运行以下代码：

	python main.py 220170000 123456 15

如果他输入的是：

	python main.py 220170000 123456

脚本会默认他想要的检测间隔是10分钟。

当然，命令行还支持无参数运行，此时就变得和`一般操作`一样了，需要用户打开`main.py`自行编辑里面的控制代码，详见上一节。

## 运行效果

无论您选择上面三种打开方式的哪一种，运行的效果都大体如下所示：

![新运行效果图](https://github.com/leyuwei/SeuWebHelper/blob/master/config/snapshot2.png)

![旧运行效果图](https://github.com/leyuwei/SeuWebHelper/blob/master/config/snapshot.png)

如果你想更加方便一点，可以在`run.bat`或`校园网登录.exe`文件上右键->发送到桌面快捷方式，在桌面快捷方式上就可以直接双击运行打开脚本。（P.S.桌面快捷方式支持修改名称和图标，可以修改使其更加美观！）
