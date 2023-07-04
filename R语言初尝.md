# open ubuntu  and enter
sudo -s #进入root 权限 输入密码
apt -y install r-base #安装R包 如果安装显示 Ubuntu E: “Unable to locate package“错误解决办法 参考https://blog.csdn.net/weixin_46672140/article/details/119115185 
sudo apt-get update   #root权限下更新apt 更新完后重试
apt -y install r-base #开始R包装载 安装成功不报错
