## 进程概念done

___

补：

* 进程是具有独立性的
* 写实拷贝：简单点理解就是，当有一个人想要修改父子进程里面的数据的时候，操作系统不会立即就对代码进行修改，而是重新开辟一个进程，把当前进程的执行信息拷贝过去，然后在新开辟的进程中去做数据的修改，这样可以保护数据的独立性

在vim中如果不想退出vim去查man手册可以：

* 在命令行输入!man 后面加要查的(q退出）<img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220311151012799.png" alt="image-20220311151012799" style="zoom:50%;" />
* 不退出编译程序：!make
* 不退出执行程序：！./myproc

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

