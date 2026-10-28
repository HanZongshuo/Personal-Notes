登录指令，秘钥路径在D:\SSH
```
ssh -i "D:\SSH\id_ed25519" -o MACs="-*etm*@openssh.com,umac-128@openssh.com" hanzongs@puhti.csc.fi
```
/projappl/project_2017796
# Linux 系统指令
`[hanzongs@puhti-login15 ~]` 这说明目前在home目录下，因为有~。

常用的指令
1. ls list the contents of the directory
2. pwd print working directory  `/users/hanzongs`
3. cd 导航到对应目录， cd .. 返回上一个文件夹， cd - 返回上一个文件夹
4. mkdir make directory
5. touch New_file.txt 创建一个新的文件
	1. touch -m old_file 更新时间为系统时间 
6. vim 一个文件，可以进行修改文件，但是刚进去只能输入命令，可以通过按i进入插入模式，然后按Esc来退出插入模式，`:wq` 执行保存并退出，`:q!`来不保存强制退出。
7. `cp New_file.txt Copied.txt` 复制一个文件，`cp-rdir_to_copy/new_copy_dir/` 这里的r表示递归
8.  `rm file_to_copy.txt` 删除文件，`rm -r dir_to_remove/` 删除文件夹，`rm -rf dir_with_content_to_remove/` 无条件删除系统中所所有的文件夹
9. mv 这个命令同时承担了改名和移动位置两项任务 `mv source_file destination_folder/` `mv old_file.txt new_named_file.txt`
10. chmod change mode of file ，可以更改文件的模式，r(read)-4, w (write)-2, x(execute)–1，通过数字组合可以确定权限状态，比如
	- **7** ($4+2+1$): 读 + 写 + 执行 (**全开**)
	- **6** ($4+2$): 读 + 写
	- **5** ($4+1$): 读 + 执行
	- **4**: 只读
	然后chmod 755 file 意思就是把7,5,5 三种级别的权限分别开放给Owner(User), Group, Other
11.  ./ 的意思是执行当前目录下的程序，例如C语言的程序或者sh文件
12. sudo 是超级管理员权限，然后exit表示退出当前SSH连接，然后shutdown就是关机指令
13. cat 可以把内容全部打印在屏幕上，`cat test.py` 也可以合并文件`cat file1.txt file2.txt > combined.txt`
14. 
	1. `python3 loop.py &` 把程序进行启动 
	2. `ps -ux | grep loop.py` 获得启动程序的PID
	3. `kill <PID>` 结束掉这个程序，通过PID
15.  使用Tab可以补全文件名称而不需要打全
16. clear 可以清楚Terminal的命令
17. history 可以显示输入的所有指令，输入!520（指令编号）就可以直接执行这个命令
18. whoami 显示当前的用户名
19. uname -a可以查看系统的内核
20. grep "你要找的词" 文件名  可以搜索出来文件中哪里用到了你要找的词
21. `history | grep "mv"`可以采取这种组合的办法来搜索



# OS 概念
# File Descriptor
[Linux文件描述符到底是什么？ - C语言中文网](https://c.biancheng.net/view/3066.html)
文件描述符是一个非负整数，对于程序代码来说，我们不直接操作复杂的文件结构，而是拿着这个数字（比如 `3`）告诉系统：“请往 3 号文件里写一段话”。

File Descriptor Table, FDT：每一个进程都有独立的唯一的表，表里每个元素都是指针，指向内核的文件表项。当传递给系统一个FD数字的时候，系统就会去表里查这个索引位置对应的指针。

文件表项 (File Table Entry, FDE)，这是内存中为**每一个打开的文件**创建的代理结构（数据结构）。记录目前读/写到文件的哪个字节了。

每个进程都有自己的一套 FD。
- 进程 A 的 FD `3` 可能对应的是“照片.jpg”。
- 进程 B 的 FD `3` 可能对应的是“歌词.txt”。
进程拿着自己的 FD（比如数字 `3`），去查看自己进程内存空间里的那张表（FDT）。查找这个 `3` 对应的指针。然后通过FDE来记录交互到哪里了。

 `creat()`：新办卡（创建文件）
- **内核动作**：磁盘分配空间（i-node） $\rightarrow$ 内核创建 **FDE**（状态/位置记录） $\rightarrow$ 进程 **FDT** 占座（分配 FD）。
- **注意**：若文件已存在，执行 `creat` 会将其内容**清空**（长度截断为 0）。
```
int fd = creat("test.txt", 0644);
```

chmod 用于改变文件对不同人群的访问权限。
- **对象 (Who)**：`u`(主人), `g`(组), `o`(其他人), `a`(所有人)。
- **操作 (+/-/=)**：`+`(增加), `-`(拿走), `=`(唯一设定)。
- **模式 (rwx)**：`r`(读=4), `w`(写=2), `x`(执行=1)。
- **例子（字母法）**：
    - `chmod u+x script.sh`：给主人增加“执行”权限。
    - `chmod g-w data.txt`：不让同组的人改这个文件。
- **例子（数字法）**：
    - `chmod 755 app`：主人=7(4+2+1,全开)；组/其他人=5(4+1,读与执行)。这是最常见的可执行程序权限。
    - `chmod 644 file.txt`：主人=6(4+2,读写)；其他人=4(只读)。这是最常见的普通文件权限。
    - 
 `chown`：换主人（所有权变更）
用于更改文件的“Owner”或“Group”。
- **作用**：决定谁才是文件的“主人”。
- **注意**：为了安全，通常只有 **root** 用户（超级管理员）有权执行。
- **例子**
    - `chown vivek demo.txt`：把 `demo.txt` 的主人从 root 换成用户 `vivek`
    - `chown vivek:devel demo.txt`：一箭双雕，主人换成 `vivek`，所属组换成 `devel` 组。
    - **效果**：换完后，`chmod` 里的 `u` 权限就对新主人 `vivek` 生效了

`open()` 它既可以打开已经存在的文件，也可以在文件不存在时创建一个新的。
`int open(const char *path, int flags, mode_t mode);`
- **`path`**：要打开的文件路径和名称
- **`flags`（标志位）**：告诉系统你打算怎么用这个文件。常用的有：
    - `O_RDONLY`：只读打开。
    - `O_WRONLY`：只写打开。
    - `O_RDWR`：读写打开。
    - `O_CREAT`：如果文件不存在就创建它（此时需要配合第三个参数 `mode`）。
    - `O_APPEND`：追加模式（写的时候直接跳到文件末尾）。
- **`mode`**：只有在使用了 `O_CREAT` 创建新文件时才生效，用来设置初始权限（如 `0644`）。

# POSIX 线程库
1. `sem_open` 文件操作中的open函数，如果不存在就根据参数进行创建，如果存在就直接打开。根据`oflag` 的组合决定了它是执行“创建”还是“打开”操作。
	1. **场景 A：仅打开 (仅指定 `oflag = 0`)**
	    - 要求信号量必须已经存在。
	    - 只需提供 `name` 和 `oflag` 两个参数。
        
	2. **场景 B：创建或打开 (`oflag = O_CREAT`)**
	    - 如果信号量不存在：**创建**它。此时必须提供 `mode`（权限）和 `value`（初始计数值）。
	    - 如果信号量已存在：**打开**它。此时会**忽略**你传入的 `mode` 和 `value` 参数。
    
	3. **场景 C：排他性创建 (`oflag = O_CREAT | O_EXCL`)**
	    - 如果信号量已存在：直接**报错返回**（通常 `errno` 会被设为 `EEXIST`）。
	    - 这确保了当前进程是唯一初始化该信号量的进程，防止重复初始化。
2. `int sem_wait(sem_t *sem);` 
	1. 当值>0的时候，信号量-1，函数立即返回，程序继续执行。看到空车停进去后车辆减少一个。当资源耗尽的时候，即值为0，那么排队等待。
3. `int sem_post(sem_t *sem);` 将信号量增加一个值，如果有其余的信号量因为其余线程执行`sem_wait` 而在苦苦等待，系统会随机唤醒一个。被唤醒的进入`sem_wait`。
4. `int sem_close(sem_t *sem);`关闭信号量，作用是解除关联而不是彻底销毁。作用是通知操作系统当前进程不在使用这个有名的信号量了。
5. `int sem_unlink(const char *name);`表示从系统中移除信号量，该信号量的名称会立即从内核命名空间中消失。内核会记录是否有进程在使用这个信号量，如果有的话就等到最后一个使用它的进程执行sem_close或者进程意外退出之后信号量被真正的销毁。


