### Ubuntu 的一些错误解决
##
### Navicat过期后的恢复，乱码解决
>$ cd //进入/home 目录  
>$ rm -rf .navicat64/ //删除 navicat64 位
  
再次重启 navicat 就会重新安装，并重新计时
#### Navicat乱码解决
安装好看的中文字体
>$ sudo apt-get install ttf-wqy-microhei  #文泉驿-微米黑  
>$ sudo apt-get install ttf-wqy-zenhei  #文泉驿-正黑     
>$ export LANG="zh_CN.UTF-8" //更改中文 UTF-8 编码

进入 navicat 工具(T)->选项(最下面的)->常规->编辑器->记录; //对应 1,3,4 重启navicat

### 在处理时发生错误 unattended-upgrades
	原因是包管理器没有正确关闭。需要重启计算机或者重新打开终端输入：
	sudo rm /var/lib/dpkg/lock
	sudo dpkg --configure -a
### Ubuntu(Linux) 下 PHP导入 Excel 出错 
> 错误信息：Attempted to load class "PHPExcel_Reader_excel2007" from the global namespace.       

原因:Ubuntu(Linux)大小写敏感，调用的时候用excel2007找不到类

    解决: 修改类名的大小写 出处：IOFactory.php
    
        private static $autoResolveClasses = array(
                'Excel2007',
                'Excel5',
                'Excel2003XML',
                'OOCalc',
                'SYLK',
                'Gnumeric',
                'HTML',
                'CSV',
            );