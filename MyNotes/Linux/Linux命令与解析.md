1. `man`
	
	这条命令用来访问存储在Linux系统上的手册信息，在你想要查找的工具名称**前**输入`man`命令就可以找到相应的手册条目。如下图，我使用`man apt`来查看`apt`的手册条目。
	
	![](/home/kevin/Documents/MyNotes/Source/Linux/2019-09-03 17-47-27 的屏幕截图.png)
	
	这里面包含很多内容，有`NAME`名字和简述，`SYNOPSOS`命令语法，`DESCRIPTION`命令一般描述等等。在这里面可以用`空格键`进行翻页，`回车键`进行逐行查看。看完之后点击`q`键退出
	
2. `pwd`查看当前工作目录，例如：

   ```shell
   ➜  /home/kevin pwd              
   /home/kevin
   ```

3. `ls`查看系统中有那些文件，后面可以加不同的参数，来实现不同的需求。

   * `ls`不加参数，会显示当前目录下的文件和目录。

   * `ls -F`可以区分文件和目录，目录后面会有`/`作为区分

     ```shell
     ➜  / ls -F
     bin/    dev/   initrd.img@      lib64/       mnt/   root/  snap/  tmp/  vmlinuz@
     boot/   etc/   initrd.img.old@  lost+found/  opt/   run/   srv/   usr/  vmlinuz.old@
     ```
     
   * `ls -a`显示出所有隐藏文件，文件名前有`.`的就是隐藏文件

   * `ls -F -R`是递归显示子目录中的所有文件，`-R`是递归显示所有文件。这个显示有点长，可以按`Ctrl + C`终止

   * `ls -l`以列表的方式输入

4. `cd`切换目录命令，可以使用相对路径和绝对路径两种方式。例如：

   ```
   
   ```

   

5. 

6. 

7. 

8. 

9. 

  

  

  

