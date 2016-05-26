列出你认为本实验中重要的知识点，以及与对应的OS原理中的知识点，并简要说明你对二者的含义，关系，差异等方面的理解（也可能出现实验中的知识点没有对应的原理知识点）
	
	文件系统——ucore文件系统的总体架构设计(vfs/sfs)
	文件读写操作
	文件系统的执行(do_execve)

列出你认为OS原理中很重要，但在实验中没有对应上的知识点

	没有涉及到sfs的存储框架的内容(比如目录项、文件的建立)

练习0：填写已有实验
	
	(1)在trap.c文件中，修改了响应键盘输入的中断处理
	case IRQ_OFFSET + IRQ_KBD:
        // There are user level shell in LAB8, so we need change COM/KBD interrupt processing.
        c = cons_getc();
        {
          extern void dev_stdin_write(char c);
          dev_stdin_write(c);
        }
        break;
	(2)在proc.c中，为进程控制块增加files_struct变量file_sp;
	
练习2: 完成基于文件系统的执行程序机制的实现（需要编码）
	
	修改load_icode()函数
	主要步骤如下:
	(1) 为当前进程分配内存空间mm_struct
    (2) 建立新的页目录表
    (3) 复制数据到进程的内存空间，包括读取文件数据，建立地址映射，分配页表
    (4) 建立用户堆栈，将变量复制到栈中
    (5) 加载页目录表的起始地址 lcr3
    (6) 建立trapframe
    (7) 异常处理：如果上述步骤出现问题，清理内存空间
     