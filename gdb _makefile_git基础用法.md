# gdb _makefile_git基础用法

#### 一. gdb调试命令(接上)

* **r/run** 直接跑程序相当于 vs的ctrl+F5

* **list / l** 显示行号

* 断点：**b**(全称**breakpoint**)
  * 用法：**b/breakpoint n**(n是行号)
  * 一般 **b fun**(函数名) 断点打在了函数的入口处
  
* 取消断点：**d/delete n**(编号)

  * 可以连续取消 ![image-20220304104036043](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304104036043.png)

* **disable** 禁用断点   **enable** 启用断点

  ![image-20220304104534417](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304104534417.png)

* **info b** 显示断点

  <img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304102109993.png" alt="image-20220304102109993" style="zoom: 100%;" />

* **s/step** 进入函数（逐语句）

* **n /next**逐过程

* **display var**(变量名)   常显示(类似于监视窗口)

  * **display &var** 查看地址

    ![image-20220304102443525](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304102443525.png)

  * **undisplay n**(这里是编号) 取消常显示

    ![image-20220304102643911](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304102643911.png)

* **p/P** 只显示一次

  * p var(变量名)

* **finish** 结束当前函数

* **continue** 跑完当前断点，到下一个断点处

* **until** 跳转到指定行

* **bt** 查看调用堆栈

* **set var**(变量) 修改变量的值

  例：**set var i=90**

#### 二. Linux项目自动化构建工具--make/Makefile

**首先了解make 是一条命令   Makefile是一个文件；Makefile 会维护两种东西：依赖关系和依赖方法；make 和Makefile 加起来可以达到形成可执行程序的目的**

* 创建Makefile文件

  ![image-20220304111601416](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304111601416.png)

* 用vim 打开，尽量不要空格不要空行

  ![image-20220304112906214](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304112906214.png)

  * **:**  左边的文件依赖于右边的文件

  * 然后直接回车到第二行然后按tab键开始写依赖方法

  * **.PHONY:clean** (有点像类型关键字一样)，后面的**clean:**  (clean没有依赖关系)

     **rm -f test** 是clean的依赖方法，之后make clean 就会执行rm -f test这个功能![image-20220304113857162](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304113857162.png)

  **所以make和make clean 相当于vs里面的生成和清理解决方案**

**make在去扫描makefile文件的时候只执行一个目标依赖关系，默认是第一个，所以想要执行对应的需要make file(文件名)**

**例：make clean /如果clean在上test在下那就是make test**

* **.PHONY**修饰对应的符号，伪目标的概念（伪目标总是可执行的）![image-20220304115050531](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304115050531.png)

**接下来是Makefile的特殊符号**

* 首先**$@ **代表的就是依赖关系中的目标文件，**$^**代表的是右边的文件列表

  ![image-20220304123318856](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304123318856.png)

