# Linux System Call Quick Reference
原作者: Jialong He
作者邮箱: Jialong_he@bigfoot.com
作者主页: http://www.bigfoot.com/~jialong_he

## 简介
系统调用是由 Linux 内核提供的服务。在 C 编程中，**libc** 中提供了许多系统调用的包裹函数，而且我们会经常用到。手册第二部分(man 2) 提供了关于系统调用的更多信息。如果想要浏览，可以使用 " man 2 intro" 命令。 

## 系统调用实例
```
#include <syscall.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/types.h>

int main(void) {
    /*-----------------------------*/
    /* direct system call */
    /* SYS_getpid (func no. is 20) */
    /*-----------------------------*/
    ID1 = syscall(SYS_getpid);
    printf ("syscall(SYS_getpid)=%ld\n", ID1);
    /*-----------------------------*/
    /* "libc" wrapped system call */
    /* SYS_getpid (Func No. is 20) */
    /*-----------------------------*/
    ID2 = getpid();
    printf ("getpid()=%ld\n", ID2);
    return(0);
}

}
```

## 系统调用速查表
|编号|函数名|作用|源代码位置|
|:---|:---------|:--------------|:-------|
|1|exit|终止当前进程|kernel/exit.c|
|2|fork|创建一个子进程|arch/i386/kernel/process.c|
|3|read|从文件描述符读入|fs/read_write.c|
|4|write|写入到文件描述符|fs/read_write.c|
|5|open|打开文件或设备|fs/open.c|
|6|close|关闭一个文件描述符|fs/open.c|
|7|waitpid|等待进程结束|kernel/exit.c|
|8|creat|创建一个文件或设备（使用 “man 2 open”查看更多信息|fs/open.c|
|9|link|为文件创建一个新名字|fs/namei.c|
|10|unlink|删除掉文件的一个名字，可能删除掉这个名字指向的文件|fs/namei.c|
|11|execve|执行程序|arch/i386/kernel/process.c|
|12|chdir|改变工作目录|fs/open.c|
|13|time|获取以秒为单位的时间|kernel/time.c|
|14|mknod|创建一个特殊或者普通的文件|fs/namei.c|
|15|chmod|改变文件权限|fs/open.c|
|16|lchown|改变文件所有权|fs/open.c|
|17|stat|获得文件状态|fs/stat.c|
|18|lseek|修改 读/写 文件偏移量|fs/read_write.c|
|19|getpid|获得 pid|kernel/sched.c|
|20|mount|挂载文件系统|fs/super.c|
|21|umount|卸载文件系统|fs/super.c|
|22|setuid|设置实际用户ID|kernel/sys.c|
|23|getuid|获得实际用户ID|kernel/sched.c|
|24|stime|设置系统时间和日期|kernel/time.c|
|25|ptrace|允许父进程控制子进程的执行|arch/i386/kernel/ptrace.c|
|26|alarm|设置发送信号的闹钟|kernel/sched.c|
|27|fstat|获得文件状态|fs/stat.c|
|28|pause|在信号到来之前休眠进程|arch/i386/kernel/sys_i386.c|
|29|utime|设置文件访问和修改时间|fs/open.c|
|30|access|检查用户对文件的权限|fs/open.c|
|31|nice|改变进程优先级|kernel/sched.c|
|32|sync|更新超级块|fs/buffer.c|
|33|kill|想进程发送信号|kernel/signal.c|
|34|rename|改变文件的名字或位置|fs/namei.c|
|35|mkdir|创建一个目录|fs/namei.c|
|36|rmdir|删除一个目录|fs/namei.c|
|37|dup|复制一个打开的文件描述符|fs/fcntl.c|
|38|pipe|创建一个进程间的通道(管道)|arch/i386/kernel/sys_i386.c|
|39|times|获得进程时间|kernel/sys.c|
|40|brk|调整进程数据段空间的大小|mm/mmap.c|
|41|setgid|设置实际用户组ID|kernel/sys.c|
|42|getgid|获得实际用户组|kernel/sched.c|
|43|sys_signal|ANSI C 信号处理|kernel/signal.c|
|44|geteuid|获得有效用户ID|kernel/sched.c|
|45|getegid|获得有效用户组ID|kernel/sched.c|
|46|acct|启用或禁用进程记账|kernel/acct.c|
|47|umount2|卸载文件系统|fs/super.c|
|48|ioctl|控制设备|fs/ioctl.c|
|49|fcntl|文件控制|fs/fnctl.c|
|50|mpx|未实现| |
|51|setpgid|设置进程组 id|kernel/sys.c|
|52|ulimt|未实现| |
|53|olduname|淘汰的 uname 系统调用|arch/i386/kernel/sys_i386.c|
|54|umask|设置文件创建掩码|kernel/sys.c|
|55|chroot|改变根目录|fs/open.c|
|56|ustat|获得文件系统信息|fs/super.c|
|57|dup2|复制文件描述符|fs/fcntl.c|
|58|getppid|获得父进程 id|kernel/sched.c|
|59|getpgrp|获得进程组 id|kernel/sys.c|
|60|setsid|创建一个会话，设置进程组id|kernel/sys.c|
|61|sigaction|POSIX 信号处理函数|arch/i386/kernel/signal.c|
|62|sgetmask|ANSI C 信号处理函数|kernel/signal.c|
|63|ssetmask|ANSI C 信号处理|kernel/signal.c|
|64|setreuid|设置实际和有效用户 id|kernel/sys.c|
|65|setregid|设置实际和有效用户组 id|kernel/sys.c|
|66|sigsupend|设置信号掩码，挂起进程等待特定信号|arch/i386/kernel/signal.c|
|67|sigpending|检查阻塞或者等待的信号|kernel/signal.c|
|68|sethostname|设置主机名|kernel/sys.c|
|69|setrlimit|设置最大系统资源|kernel/sys.c|
|70|getrlimit|获得系统最大资源|kernel/sys.c|
|71|getrusage|获得系统资源使用情况|kernel/sys.c|
|72|gettimeodday|获得日期和时间|kernel/time.c|
|73|settimeofday|设置日期和时间|kernel/time.c|
|74|getgroups|获得后备组 id 列表|kernel/sys.c|
|75|setgroups|设置后备组 id 列表|kernel/sys.c|
|76|old_select|同步 I/O 多路复用|arch/i386/kernel/sys_i386.c|
|77|symlink|为一个文件创建符号链接|fs/namei.c|
|78|lstat|获得文件状态|fs/stat.c|
|79|readlink|读取符号链接的内容|fs/stat.c|
|80|uselib|选择共享库|fs/exec.c|
|81|swapon|启用文件或者设备的交换文件|mm/swapfile.c|
|82|reboot|重启或者启用/禁用 Ctrl-Alt-Del|kernel/sys.c|
|83|old_readdir|读取目录项|fs/readdir.c|
|84|old_mmap|映射内存页|arch/i386/kernel/sys_i386.c|
|85|munmap|取消映射内存页|mm/mmap.c|
|86|truncate|截断文件长度|fs/open.c|
|87|ftruncate|截断文件长度|fs/open.c|
|88|fchmod|改变文件的访问权限|fs/open.c|
|89|fchown|改变文件的所有者和所有组|fs/open.c|
|90|getpriority|获得程序调度优先级|kernel/sys.c|
|91|setpriority|设置程序调度优先级|kernel/sys.c|
|92|profil|进行时间分析| |
|93|statfs|获得文件系统信息|fs/open.c|
|94|fstatfs|获得文件系统信息|fs/open.c|
|95|ioperm|设置端口 i/o 权限|arch/i386/kernel/ioport.c|
|96|socketcall| socket 系统调用|net/socket.c|
|97|syslog|读取或/和清除内核信息环形缓冲区|kernel/printk.c|
|98|setitimer|设置内部定时器的值|kernel/itimer.c|
|99|getitimer|获得内部定时器的值|kernel/itimer.c|
|100|sys_newstat|获得文件状态|fs/stat.c|
|101|sys_newlstat|获得文件状态|fs/stat.c|
|102|sys_newfstat|获得文件状态|fs/stat.c|
|103|old_uname|获得当前内核的名字和信息|arch/i386/kernel/sys_i386.c|
|104|iopl|改变 I/O 特权等级|arch/i386/kernel/ioport.c|
|105|vhangup|挂起当前终端|fs/open.c|
|106|idle|创建进程 0 idle|arch/i386/kernel/process.c|
|107|vm86old|进入模拟 8086 模式|arch/i386/kernel/vm86.c|
|108|wait4|等待进程终止，BSD 风格|kernel/exit.c|
|109|swapoff|停用文件/设备的交换文件|mm/swapfile.c|
|110|sysinfo|返回全部的系统信息|kernel/info.c|
|111|ipc|Sytem V IPC 系统调用|arch/i386/kernel/sys_i386.c|
|112|fsync|把内存中的文件同步到磁盘上|fs/buffer.c|
|113|sigreturn|从信号处理函数返回，并且清理栈帧|arch/i386/kernel/signal.c|
|114|clone|创建子进程|arch/i386/kernel/process.c|
|115|setdomainname|设置域名|kernel/sys.c|
|116|uname|获得当前内核的名字和信息|kernel/sys.c|
|117|modify_ldt|获取或设置 ldt|arch/i386/kernel/ldt.c|
|118|adjtimex|调整内核时钟|kernel/time.c|
|119|mprotect|设置内存映像保护|mm/mprotect.c|
|120|sigprocmask|POSIX 信号处理函数|kernel/signal.c|
|121|create_module|创建一个可载入的模块项|kernel/module.c|
|122|init_module|初始化一个可载入的模块项|kernel/module.c|
|123|delete_module|删除一个可载入的模块项|kernel/module.c|
|124|get_kernel_syms|回复导出的内核模块符号|kernel/module.c|
|125|quotactl|维护磁盘配额|fs/dquot.c|
|126|getpgid|获得进程组 id|kernel/sys.c|
|127|fchdir|改变工作目录|fs/open.c|
|128|bdflush|启动,刷新,或调整缓冲区脏刷新为守护进程|fs/buffer.c|
|129|sysfs|获得文件系统类型信息|fs/super.c|
|130|personality|设置进程执行域|kernel/exec_domain.c|
|131|afs_syscall|(未实现)| |
|132|setfsuid|设置用来文件系统检查的用户 id|kernel/sys.c|
|133|setfsgid|设置用来文件系统检查的组 id|kernel/sys.c|
|134|sys_llseek|移动可扩展的读/写的文件指针|fs/read_write.c|
|135|getdents|读取目录项|fs/readdir.c|
|136|select|同步 I/O 多路复用|fs/select.c|
|137|flock|在一个打开的文件上添加或移除文件锁|fs/locks.c|
|138|msync|同步文件和内存映射|mm/filemap.c|
|139|readv|读取数据到多个缓冲区|fs/read_write.c|
|140|writev|将数据写到多个缓冲区|fs/read_write.c|
|141|sys_getsid|获得会话领导进程的组 id |kernel/sys.c|
|142|fdatasync|同步内核态文件到硬盘中|fs/buffer.c|
|143|sysctl|读/写系统变量|kernel/sysctl.c|
|144|mlock|对内存页上锁|mm/mlock.c|
|145|munlock|对内存页解锁|mm/mlock.c|
|146|mlockall|禁用调用进程的内存页面|mm/mlock.c|
|147|munlockall|重新启用调用进程的内存页面|mm/mlock.c|
|148|sched_setparam|设置调度变量|kernel/sched.c|
|149|sched_getparam|获得调度变量|kernel/sched.c|
|150|sched_setscheduler|设置调度算法变量|kernel/sched.c|
|151|sched_getscheduler|获得调度算法变量|kernel/sched.c|
|152|sched_yield|让出处理器|kernel/sched.c|
|153|sched_get_priority_max|获得最大的静态权限范围|kernel/sched.c|
|154|sched_get_priority_min|获得最小的静态权限范围|kernel/sched.c|
|155|sched_rr_get_interval|获得有名进程的 SCHEDRR interval |kernel/sched.c| 
|156|nanosleep|暂停执行指定的时间(纳秒单位)|kernel/sched.c|
|157|mremap|重新映射虚拟内存地址|mm/mremap.c|
|158|setresuid|设置实际、有效、设置用户/组 id|kernel/sys.c|
|159|getresuid|获得实际、有效、设置用户/组 id|kernel/sys.c|
|160|vm86|进入模拟 8086 模式 |arch/i386/kernel/vm86.c|
|161|query_module|查询内核的关于模块的各个位|kernel/module.c|
|162|poll|等待一个文件描述符上的某些事件|fs/select.c|
|163|nfsservctl|内核 nfs 后台的系统调用|fs/filesystems.c|
|164|setresgid|设置实际、有效、设置用户/组 id |kernel/sys.c|
|165|getresgid|设置实际、有效、设置用户/组 id|kernel/sys.c|
|166|prctl|对线程的操作|kernel/sys.c|
|167|rt_sigreturn| |arch/i386/kernel/signal.c|
|168|rt_sigaction| |kernel/signal.c|
|169|rt_sigprocmask| |kernel/signal.c|
|170|rt_sigpending| |kernel/signal.c|
|171|rt_sigtimedwait| |kernel/signal.c|
|172|rt_sigqueueinfo| |kernel/signal.c|
|173|rt_sigsuspend| |kernel/signal.c|
|174|pread|从文件描述符指定的偏移量开始读|fs/read_write.c|
|175|sys_pwrite|从文件描述符指定的偏移量开始写|fs/read_write.c|
|176|chown|改变文件所有权|fs/open.c|
|177|getcwd|获得当前工作目录|fs/dcache.c|
|178|capget|获得进程权限|kernel/capability.c|
|179|capset|设置进程权限|kernel/capability.c|
|180|sigaltstack|设置/获得信号栈内容|arch/i386/kernel/signal.c|
|181|sendfile|在文件描述符之间传输文件|mm/filemap.c|
|182|getpmsg|未实现||
|183|putpmsg|未实现||
|184|vfork|创建一个子进程，然后阻塞父进程|arch/i386/kernel/process.c|
