#们可以如上面的简介里面所描述的，很快捷地从Docker Hub中获取一些别人配置好的 _image。_除此之外，我们也可以手工加载。就是一些预先配置
#下面介绍如何手工加载我们为本课程配置的 image。
#从 Teaching Dockers 里面下载docker image文件**：** bioinfo_PartI-PartII-PartIII1-3.tar.gz到本地，并且通过在 Power shell 中输入以下命令导入将_image_导入到 Docker 中。
#需要将路径C:\Users\xxx\Desktop\bioinfo_PartI-PartII-PartIII1-3.tar.gz换成打包的docker _image_的实际路径，建议使用绝对路径。
#注意windows系统的路径中使用的是反斜杠。
# 假设我们把image下载到了桌面上
#docker load -i C:\Users\xxx\Desktop\bioinfo_PartI-PartII-PartIII1-3.tar.gz
#下载完我移到了桌面的xxdocker 所以就是
#在powershell 里面输入 用户名是yang 桌面的文件夹xxdocker 不用预先解压
docker load -i C:\Users\yang\Desktop\xxdocker\bioinfo_PartI-PartII-PartIII1-3.tar.gz
#之后可以看到他加载 
#<im<img width="684" alt="1688270177772" src="https://github.com/Yangxiaoxi99/Yangxiaoxi99/assets/134954385/1f0a1b4e-e86d-4275-b045-69f28daae4d8">
#g width="684" alt="1688270177772" src="https://github.com/Yangxiaoxi99/Yangxiaoxi99/assets/134954385/b84f2b75-8ece-4608-a3d8-ca622e9ae992">
创建共享文件夹为lzlesson
docker run --name=lz_lesson -dt -h lz_docker --restart unless-stopped -v C:\Users\yang\Desktop\lz_lesson_share:/home/test/share xfliu1995/bioinfo_tsinghua:2(这是配置文件名 装载之后的）
在宿主机上新建一个文件夹用于和docker container 实现文件共享。这里假设我们在桌面上新建了一个名为bioinfo_tsinghua_share的文件夹

在宿主机上新建一个文件夹用于和docker container 实现文件共享。这里假设我们在桌面上新建了一个名为bioinfo_tsinghua_share的文件夹
然后，用docker run新建一个名为 bioinfo_tsinghua 的 container。
请把C:\Users\xxx\Desktop\bioinfo_tsinghua_share换成你刚刚新建的共享目录在你的宿主机上的绝对路径
这里的参数含义和在mac上基本一致:
-dt --restart unless-stopped: 设置该_container_能一直在后台保持运行状态
-v C:\Users\xxx\Desktop\bioinfo_tsinghua_share:/home/test/share: 将宿主机的目录C:\Users\xxx\Desktop\bioinfo_tsinghua_share作为数据卷挂载到_container的/home/test/share目录。我们在container_中修改/home/test/share目录下的内容，就相当于修改宿主机~/Desktop/bioinfo_tsinghua_share目录下的内容。
在 Windows 10 Pro 上，Docker中的/home/test/share目录归root所有，所以要将其改为归test用户所有:
#用root的身份(-u root)运行已在运行的container: bioinfo_tsinghua中的chown命令,把/home/test/share的所有权给test用户
docker exec -u root bioinfo_tsinghua chown test:test /home/test/share

Run and exit the container
如果_container创建成功，之后每次只需要启动Docker程序，然后在 Power shell 中输入以下命令即可进入container_：
docker exec -it bioinfo_tsinghua bash
docker exec用于在一个正在运行的_container_中执行命令
-it: 交互(interactive, -i)式的运行_container_中的bash命令，并在terminal中显示输入输出(-t)
注：如果power shell在这一步出现了卡死的情况，可以尝试使用windows的其他终端，如cmd等。
之后即可运行_container_中提供的各种Linux命令；
输入 exit 即可回到 Power shell。


就会显示如下：
faee: Loading layer [==================================================>]  150.6MB/150.6MB
4b450b6fe243: Loading layer [==================================================>]  141.6MB/141.6MB
1445083be176: Loading layer [==================================================>]  1.706MB/1.706MB
1edc726a9c1e: Loading layer [==================================================>]  166.1MB/166.1MB
Loaded image: xfliu1995/bioinfo_tsinghua:2
PS C:\WINDOWS\system32> mkdir  ~/Desktop/lz_lesson_share


    目录: C:\Users\yang\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          2023/7/2     12:23                lz_lesson_share


PS C:\WINDOWS\system32> docker run --name=lz_lesson -dt  -h lz_docker --restart unless-stopped -v ~/Desktop/lz_lesson_share:/home/test/share xfliu1995/bioinfo_tsinghua:2
8b910f252342333877e431d8b9ad68aec806a9c630f08cb79935f241305c8574
PS C:\WINDOWS\system32> #用root的身份(-u root)运行已在运行的container: bioinfo_tsinghua中的chown命令,把/home/test/share 的所有权给test用户
PS C:\WINDOWS\system32> docker exec -u root lz_lesson chown test:test /home/test/share
PS C:\WINDOWS\system32> docker exec -it lz_lesson bash
test@lz_docker:~$ exit
exit
PS C:\WINDOWS\system32>

集群使用操作没学 待探究
