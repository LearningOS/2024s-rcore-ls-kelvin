## 功能实现

1. 在TaskControlBlock中加入TaskInfo，删除TaskStatus，修改所有更新TaskStatus的代码，改为更新TaskInfo.status。

2. 在trap_handler函数中，匹配为系统调用时，调用全局变量TASK_MANAGER，添加两个函数，一个用于添加当前任务当前调用的次数，一个用于更新任务调度时间。

3. 最后在sys_task_info中，调用全局变量TASK_MANAGER，添加一个函数，返回当前TaskInfo的克隆，赋给_ti。

## 问答题

1. **SBI**: RustSBI version 0.3.0-alpha.2, adapting to RISC-V SBI v1.0.0  
**bad_address**: PageFault in application, bad addr = 0x0, bad instruction = 0x804003ac, kernel killed it.  
**bad_instructions**: IllegalInstruction in application, kernel killed it.  
**bad_register**: IllegalInstruction in application, kernel killed it.

2. 1. 内核栈的栈顶；第一种是启动应用程序时，切换到U态；第二种是 在处理完Trap后返回U态。
   2. sstatus保存当前CPU特权级，恢复它可以在sret后进入U态；sepc保存Trap处理完后下一条指令位置，恢复它可以在sret正常进行U态下一条指令；sscratch保存用户栈栈顶地址，恢复它可以恢复用户栈数据。
   3. 跳过x2是要基于它来找到每个寄存器应该被保存到的正确的位置；跳过x4是因为应用不会用到。
   4. sp指向用户栈栈顶，sscratch指向内核栈栈顶。
   5. sret，因为这条指令的作用是让CPU将当前的特权级按照sstatus的SPP字段设置为U，然后CPU会跳转到sepc寄存器指向的那条指令继续执行。
   6. sp指向内核栈栈顶，sscratch指向用户栈栈顶。
   7. ecall

## 荣誉准则

1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

    > 无

2. 此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

    > 无

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。