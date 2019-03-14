### Ubuntu 的一些错误解决
### 在处理时发生错误 unattended-upgrades
	原因是包管理器没有正确关闭。需要重启计算机或者重新打开终端输入：
	sudo rm /var/lib/dpkg/lock
	sudo dpkg --configure -a