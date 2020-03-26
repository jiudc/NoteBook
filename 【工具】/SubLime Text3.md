# Sublime Text3

## 安装

1.  到官网下载软件并安装

2.  打开网站: https://hexed.it

3.  点击“Open file”，然后选择“sublime_text.exe” （ sublime_text.exe文件位于Sublime Text 3的安装目录下 ）

4.  选择右侧的“Search”，然后在“Search for”框输入“97 94 0D”，点击“Search Now”

5. 下方出现一个搜索结果；这里为 0x0058F144

6.  点击搜索结果，修改搜索结果“97 94 0D”为“00 00 00”

7.  点击“Export”把修改好的exe程序下载并替换原来的sublime_text.exe

8.  打开sublime text3，然后Help-Enter License-输入下方的license

	```shell
	----- BEGIN LICENSE -----
	TwitterInc
	200 User License
	EA7E-890007
	1D77F72E 390CDD93 4DCBA022 FAF60790
	61AA12C0 A37081C5 D0316412 4584D136
	94D7F7D4 95BC8C1C 527DA828 560BB037
	D1EDDD8C AE7B379F 50C9D69D B35179EF
	2FE898C4 8E4277A8 555CE714 E1FB0E43
	D5D52613 C3D12E98 BC49967F 7652EED2
	9D2D2E61 67610860 6D338B72 5CF95C69
	E36B85CC 84991F19 7575D828 470A92AB
	------ END LICENSE ------
	```

	9、 最后，为了防止sublime text3检测，添加以下内容到host文件中

	```shell
	127.0.0.1 www.sublimetext.com
	127.0.0.1 sublimetext.com
	127.0.0.1 sublimehq.com
	127.0.0.1 telemetry.sublimehq.com
	127.0.0.1 license.sublimehq.com
	127.0.0.1 45.55.255.55
	127.0.0.1 45.55.41.223
	0.0.0.0 license.sublimehq.com
	0.0.0.0 45.55.255.55
	0.0.0.0 45.55.41.223
	```

	10. 如何添加右键菜单?（此方法添加的右键菜单有图标）
		使用以下批处理（请将批处理放在和主程序一个目录下运行）：

	```bash
	@ECHO OFF
	PUSHD %~DP0
	TITLE Sublime Text
	Md "%WinDir%\System32\test_permissions" 2>NUL||(Echo 请使用右键管理员身份运行&&Pause >NUL&&Exit)
	Rd "%WinDir%\System32\test_permissions" 2>NUL
	SetLocal EnableDelayedExpansion
	SET /P ST=输入a添加右键菜单，输入d删除右键菜单：
	if /I "%ST%"=="a" goto Add
	if /I "%ST%"=="d" goto Remove
	:Add
	reg add "HKEY_CLASSES_ROOT\*\shell\Sublime Text"         /t REG_SZ /v "" /d "&Sublime Text"   /f
	reg add "HKEY_CLASSES_ROOT\*\shell\Sublime Text"         /t REG_EXPAND_SZ /v "Icon" /d "%~dp0sublime_text.exe" /f
	reg add "HKEY_CLASSES_ROOT\*\shell\Sublime Text\command" /t REG_SZ /v "" /d "%~dp0sublime_text.exe \"%%1\"" /f
	 
	exit
	:Remove
	reg delete "HKEY_CLASSES_ROOT\*\shell\Sublime Text" /f
	exit
	```

## 常用插件安装

### 安装package Control

Ctl+Shift+P打开Package Control，输入install，选择安装

## SFTP



