### Ubuntu 小技巧
### 创建软件桌面快捷方式
	1. 进入 /usr/share/applications/
	2. 创建 name.desktop文件
	3. 写入代码，保存(去掉中文)

		[Desktop Entry]
		 
		Encoding=UTF-8
		 
		Name=Navicat Premium                       （ 生成快捷图标的名字 ）
		 
		Comment=The Smarter Way to manage dadabase （ 对软件的描述 ）
		 
		Exec=/bin/sh "/opt/navicat/start_navicat"  （ start_navicat所在路径 ）
		 
		Icon=/opt/navicat/navicat.png              （ 下载图片所在位置 ）
		 
		Categories=Application;Database;MySQL;navicat
		 
		Version=1.0
		 
		Type=Application