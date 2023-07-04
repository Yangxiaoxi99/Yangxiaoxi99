          意思                                         解释                                           用法和含义
1)
man	查询某一命令的具体参数，例如：man wget           manual 查询口令
mkdir	创建文件夹                                                          mkdir +文件夹名
cd	目录切换（注意区别相对路径与绝对路径）         cd changedirectory       cd后面为空时，进入默认家目录（这里为 `/home/test`）   cd /usr/local  # 进入根目录(目录名输入一部分即可按TAB键自动补全，非常好用)
ls	显示文件夹中文件列表                               list                ls-l（和-la一样）      ls -a （列出来所有内容）  ls-la(显示当前文件列表 包括权限大小日期名字）
cat	直接查看文件          
wc	查看文件行数、字数                              word count
cut	取出文件中的特定列或字符                         
sort	排序     
uniq	去重复
grep	文件中关键词搜索，返回行                    用于过滤自己想要的和不想要的
chmod	修改文件的访问权限
awk

3)
pwd 显示当前目录路径
tree 以树形结构显示文件夹                         tree +文件夹名 或进入路径后直接tree    #显示 linux 文件夹下文件（夹）
touch 创建文件                                         touch old_file
mkdir 创建文件夹                                       mkdir cp_folder
cp    复制文件(夹)，                               cp old_file old_file2 # 复制文件                cp -r old_folder old_folder2  # 复制文件夹，需要加上 -r           
mv 重命名或移动文件（夹）                              mv old_file new_file        #文件重命名 文件文件 文件覆盖。如果new_file存在，将覆盖new_file。
                                                     mv old_folder new_folder    #文件夹重命名。文件夹夹归入其中。如果new_folder已经存在，把old_folder移动到new_folder中
                                                      mv new_file new_folder     #将文件移动到新目录
rmdir 删除文件夹                                       rmdir old_folder2   # 只能是空文件夹
rm 删除文件（夹）                                        rm -r new_folder  # 删除非空文件夹（可以非空）     rm old_file2   # 删除文件

4)查看文件
cat 直接查看文件                                             cat file_name
wc 查看文件行数、字数                                        wc -l file_name #查看文件行                                wc -c file_name #查看文件字数
head 查看文件前几行                                     head file_name #查看文件前 10 行（思考：为什么只显示 8 行）       head -n 6 file_name #查看文件前 6 行
tail 查看文件后几行                                     tail file_name #查看文件后 10 行                               tail -n 4 file_name #查看文件后 4 行
more/less 翻页查看文件                                  more file_name # 按 d 向下翻页，翻完后（或按 q）退出 (由于文件过小，需要把终端调窄才有效果)
                                                       less file_name # 按 d 向下翻页，u 向上翻页，q 退出

5) 文件信息提取和操作
cut 取出文件中的特定列或字符                             cut -f 4 file_name  #取出第 4 列
                                                       cut -d ";" -f 2 file_name  # 以分号作为输入字段的分隔符（默认为制表符），取出第 2 列
sed 编辑文件                                            sed 's/a/A/g' file_name     #将文件中所有的 a 替换为 A
                                                       sed -n '3,6 p' file_name    #打印第3到6行
                                                        sed '2 q' file_name         #打印前2行
grep 文件中关键词搜索，返回行                             grep 'CDS' file_name       #显示匹配上 'CDS' 的所有行
                                                        grep -v 'CDS' file_name    #显示没有匹配上'CDS'的所有行 过滤含有CDS的行数
                                                        grep -w 'gene' file_name    #必须与整个字匹配 (思考第 8 行中的 gene_id 包含 gene，为什么没有显示这一行)
                                                      #用grep -v排除comment line(以#开头的部分)以及长度为0的空白行
                                                      # '^'匹配行首，'$'匹配行尾
                                                      # '^#'匹配行首为'#'的行
                                                      # 如果'^$'可以匹配到某一行，则表示该行为空行(行首紧接着行尾，之间没有其他字符)
                                                          grep -v "^#" 1.gtf | grep -v '^$' | wc -l 

                                                        # 过滤空白空行(除了换行符还可能包括空白字符，如空格和制表符的行)，显示前10行结果
                                                            # '\s'匹配空白字符，'*'表示这样的字符会出现0次到多次
                                                            # '^\s*$'表示在一行的开始和结束之间只有0到多个空白字符
                                                        cat 1.gtf | awk '$0!~/^\s*$/{print}' | head -10
                                                          grep -v '^\s*$' 1.gtf | head -10

sort 排序                                               sort -k 4 file_name           # 按照第 4 列排序
                                                        sort -k 5 file_name           # 按照第 5 列排序 (ASCII码顺序)
                                                        sort -k 5 -n file_name        # 按照第 5 列排序 (ASCII数值顺序)
uniq 去重复                                              uniq -c file_name    # 去重复并且计算重复频率（仅能处理串联重复）
                                                        Tips：file_name 中没有重复的行，该命令的效果要在下一节的最后一条命令中才能直观地看到。 


6) 压缩和数据流重定向
gzip    压缩文件                                         gzip file_name
gunzip 解压缩文件（.gz 文件）                             gunzip file_name.gz
tar    打包压缩、解压缩文件（夹） -z .gzip 格式   -c 打包压缩   f 指定压缩文件名           tar -zcv -f cp_folder.tar.gz cp_folder   #打包压缩文件夹（gzip格式）
                                -z .gzip 格式  -t 查看压缩包里的文件名 -f 指定压缩文件名  tar -ztv -f cp_folder.tar.gz             #查看压缩文件夹中的文件名（gzip格式）
                                -z .gzip 格式  -x 解压   -f 指定压缩文件名              tar -zxv -f cp_folder.tar.gz             #打开包并解压缩（gzip格式）
                                -c 打包压缩  -x 解压  -t 查看压缩包里的文件名  -z .gzip 格式 -f 指定压缩文件名
-C：建立一个压缩文件的参数指令(create的意思) ；-x：解开一个压缩文件的参数指令!
-t：查看tarfile里面的文件! 特别注意，在参数的下达中，c/x/t仅能存在一个!不可同时存在!因为不可能同时压缩与解压缩。
-z：是否同时具有gzip的属性?亦即是否需要用gzip压缩?
-j：是否同时具有bzip 2的属性?亦即是否需要用bzip 2压缩?
-V：压缩的过程中显示文件!这个常用，但不建议用在背景执行过程!
-f：使用档名，请留意，在f之后要立即接档名喔!不要再加参数! 例如使用『tar-z cv fPt files file』就是错误的写法， 要写成『tar-z cvP ft files file』才对喔!
-p：使用原文件的原来属性(属性不会依据使用者而变)-P：可以使用绝对路径来压缩!
-N：比后面接的日期(yyyy/mm/dd)还要新的才会被打包进新建的文件中!--exclude FILE：在压缩的过程中， 不要将FILE打包!

> 将终端结果输出给文件，会创建新文件或者覆盖原文件
cat file_name > new_file  # 将文件的内容输出到一个新文件 cat new_file
>> 将终端结果输出给文件，内容会加在原文件尾部
sed -n '8 p' file_name >> new_file # 将文件的第 8 行附加到新文件的尾部
cat new_file

| 管道，将左边命令的标准输出（standard output）作为右边命令接受的标准输入（standard input）
head -n 6 file_name | tail -n 3
# 输出文件的前 6 行，通过管道转发给 tail 取出后 3 行，也就是原始文件的 4-6 行。
cut -f 4 file_name | sort | uniq -c
# 输出文件的第 4 列，通过管道转发 sort 进行排序，通过管道转发到 uniq  去重复并且计算重复频率。
Tips
1. 管道命令只处理前一个命令正确输出，不处理错误输出（standard error）。
1. 管道命令右边命令，必须能够接收标准输入流命令才行。

   
7) 查看、修改文件权限

用户及用户组：文件所有者 u(user)，用户组 g(group)，其他人 o(other)，所有人 a(all)
chmod 修改文件的访问权限，分为数字模式和符号模式。   数字模式：chmod 755 file_name  chmod -R 755 cp_folder     # -R  修改该目录中所有文件的权限
三位数分别表示文件所有者，用户组，其他人  r 表示可读，w 表示可写，x 表示可执行 用数字表示：可读 r=4，可写 w=2，可执行 x=1
例如：777 表示所有用户对文件具有读、写、执行权限；755 表示文件所有者对文件具有可读、可写、可执行权限，其他用户只具有可读、可执行权限。
符号模式：
chmod u+x,go=rx file_name   #使文件的所有者加上可执行权限，将用户组和其他人权限设置为可读和可执行
chmod o-x file_name    #使其他人对文件除去可执行权限
chmod a+x cp_folder    #使所有人对文件夹加上可执行权限    + 加入 - 除去 = 设置

9) 其他命令
top 监视计算机使用情况（按 q 退出）
date 显示系统的时间和日期，可用于为程序运行时长进行计时
which 寻找可执行文件，显示路径
ctrl-c 终止当前进程
ctrl-z 暂停当前进程

11) 清理  rm new_file   rm -r cp_folder   rm cp_folder.tar.gz
                                                      
