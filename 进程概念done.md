## 进程概念done

___

补：

* 进程是具有独立性的
* 写实拷贝：简单点理解就是，当有一个人想要修改父子进程里面的数据的时候，操作系统不会立即就对代码进行修改，而是重新开辟一个进程，把当前进程的执行信息拷贝过去，然后在新开辟的进程中去做数据的修改，这样可以保护数据的独立性

在vim中如果不想退出vim去查man手册可以：

* 在命令行输入!man 后面加要查的(q退出）<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311151012799.png" alt="image-20220311151012799" style="zoom:50%;" />
* 不退出编译程序：!make
* 不退出执行程序：！./myproc
* 前台进程(状态后有+)：特点就是可以用ctrl+c 来结束进程，输入一些命令行指令没有反应
* 后台进程(状态后没有+)：特点就是可以执行很多命令行指令，但是ctrl+c已经不能杀掉进程了，用可以用kill杀

___

#### 通过系统调用创建进程--fork初识

<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311143913288.png" alt="image-20220311143913288" style="zoom: 50%;" />

##### **如何理解fork创建子进程**

1. **./cmd  or run command(跑某些命令),fork** ：在操作系统角度，上面的创建进程的方式，没有差别。
2. fork本质是创建进程---->系统里面多了一个进程---->与进程相关的内核数据结构(task_struct)+进程的代码和数据
   * 我们只是fork了创建了子进程，但是子进程对应的代码数据呢？？--->默认情况下会‘’继承‘’父进程的代码和数据，内核数据结构task_struct也会以父进程为模板，初始化子进程的task_struct
   * fork之后父子进程代码是共享的(之前也是相同的只是父进程的程序计数器的属性也会被子进程继承下来，所以子进程不会去执行fork之前的代码)
   * 一般父子代码只有一份，默认情况下数据也是‘’共享的‘’，不过需要考虑修改的情况！(通过**写实拷贝**来完成进程数据的独立性)

##### fork的返回值

* 通过fork的返回值来让子进程和父进程做不同的事情

  * 失败 < 0
  * 成功：给父进程返回子进程的pid，给子进程返回0<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311163742755.png" alt="image-20220311163742755" style="zoom:50%;" />
  * 父子进程谁先return谁就会发生写实拷贝

* 可以看到一点就是那个父进程的父进程是就是命令行(bash)

  <img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311153046284.png" alt="image-20220311153046284" style="zoom:50%;" />

* 如何理解返回值的设置

  * 父：子=1：n因为是一对多，所以需要让父进程来控制子进程的话就需要子进程的pid
  * 可以发现有两个进程在跑<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311181609757.png" alt="image-20220311181609757" style="zoom:60%;" /><img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311181328767.png" alt="image-20220311181328767" style="zoom:50%;" />
  * 通过if语句来分流；fork之后父子**不确定**谁先运行(**调度器**会自己决定)

---

##### 进程状态

![image-20220311182710814](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311182710814.png)

**进程的状态信息在task_struct(PCB)中**

* 进程状态的意义：方便OS快速判断进程，完成特定的功能，比如调度(本质是一种分类)

* 具体状态：

  1. R(running运行状态)：不一定正在占用CPU，处于运行状态的含义是**处于运行队列当中，可以随时被CPU调度**
  
  2. 当我们完成某种任务的时候，任务条件不具备，需要进程进行某种等待可以用**S**或者**D**表示（进程不会只等待CPU资源也会等待其他的资源，这时被称为等待队列）

  3. **所谓的进程，在运行的时候，可能因为运行需要，而在不同的队列里！！在不同的队列里所处的状态是不一样的！！**

  **我们把，从运行状态的task_struct放到等待队列中，就叫做挂起等待(阻塞)；从等待队列，放到运行队列，被CPU调度就叫做唤醒进程**
  
  4. S(sleeping浅度睡眠状态)：可中断睡眠状态
  
  5. D(disk sleep深度睡眠状态)：不可中断状态(进程如果处于D状态，不可被杀掉！)
  
  6. T(stopped暂停状态)：数据就不会在进行更新
  
     kill -l 可以查看一些对进程的操作，给进程发信号可以使进程暂停或者运行<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220318164751991.png" alt="image-20220318164751991" style="zoom:60%;" />
  
     暂停用法就是:**kill -19 PID**
  
     <img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220318165040991.png" alt="image-20220318165040991" style="zoom: 67%;" />
  
     继续运行用法：**kill -18 PID**（但是会被调到后台运行可以用-9 来结束运行）![image-20220318165415061](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220318165415061.png)
  
  7. t(tracing stop追踪状态)：也是一种暂停，断点的应用就是追踪状态
  
  8. X(dead 死亡状态)：回收进程资源=进程相关的内核数据结构+代码和数据
  
  9. Z(zombie 僵尸状态)： 辨别退出死亡原因(获取进程退出的信息)；如果没有人检测或回收进程(父进程)，该进程退出就进入Z
  
     **自动查看进程的命令行脚本：while : ; do ps axj |head -1 && ps axj |grep test |grep -v grep; sleep 1; echo "###################"; done **(大概意思就是每1秒查看一次进程的数据，并且打印完之后会打印##############来分隔)
  
     ![image-20220318172844998](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220318172844998.png)
  
  10. 有一种现象<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220318163918221.png" alt="image-20220318163918221" style="zoom: 60%;" />
  
  当死循环打印hello的时候可以发现只有很少的时间进程处于R状态，大部分时间处于S状态！！！这是因为数据输出到外设很慢，IO等待外设是需要花时间的，所以大部分时间都在休眠
  
* **孤儿进程**

  * 父进程先死那么就会由1号进程领养，1号进程也就是操作系统![image-20220319144823947](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220319144823947.png)

---

##### 进程优先级

* **基本概念**

  * CPU资源分配的先后顺序，就是指进程的优先权
  * 优先权高的进程有优先执行权利。配置进程优先权对多任务环境的Linux很有用，可以改善系统性能。
  *  还可以把进程运行到指定的CPU上，这样一来，把不重要的进程安排到某个CPU，可以大大改善系统整 体性能。

* **查看系统进程**

  * **ps -l** 查看进程(PRI就是优先级)
  * **ls -nl** 查看UID<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220312093416790.png" alt="image-20220312093416790" style="zoom:50%;" />
  * **Linux中的优先级数据，值越小优先级越高**

  **PRI and NI**

  * PRI就是对应的优先级数据
  * NI(nice)就是对应的修正数据
  * 加入nice值后，PRI(new)=PRI(old)+nice (每一次都会重置)
  * nice其取值范围是-20至19，一共40个级别

  **PRI vs NI**

  * 要强调一点的是，进程的nice值不是进程的优先级，他们不是一个概念，但是进程nice值会影响到进程的优先级变化。
  * 可以理解nice值是进程优先级的修正数据

  **进程优先级的调整：**

  * 优先级再怎么设置，也只能是一种相对的优先级，不能出现绝对的优先级，否则会出现很严重的进程“饥饿问题”
  * **调度器：较为均衡的让每个进程享受CPU的资源**

  **其他概念：**

  * **竞争性**: 系统进程数目众多，而CPU资源只有少量，甚至1个，所以进程之间是具有竞争属性的。为了高效完成任务，更合理竞争相关资源，便具有了优先级
  * **独立性**: 多进程运行，需要独享各种资源，多进程运行期间互不干扰 
  * **并行**: 多个进程在多个CPU下分别，同时进行运行，这称之为并行 
  * **并发**: 多个进程在一个CPU下采用进程切换的方式，在一段时间之内，让多个进程都得以推进，称之为并发


