# Linux环境基础开发工具使用(2)

#### Linux编译器--gcc/g++基础使用

##### 一.gcc的使用

* **gcc file_name**（文件名）   就可以直接编译了

  <img src="C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227104354905.png" alt="image-20220227104354905" style="zoom:150%;" />

  运行程序就是 **./a.out**（也可以用绝对路径 方式运行）

* **gcc -E(预处理) file_name**

  * 预处理核心工作：进行头文件展开，去注释，条件编译，宏替换等等

  * -E 就是说开始翻译，完成预处理之后停下来。
  * 如果后面不跟一个临时文件，-E 后会把结果输出到显示屏上不方便观察，所以一般再加个 **-o** file_name.i (-E后形成的临时文件后缀为**.i**)
    * 用法：gcc -E file_name -o file_name.i
    * ![image-20220227105427832](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227105427832.png)

* **gcc -S(编译) file_name.i**（也可以再用file_name编译）

  * 编译的核心工作：生成汇编
    * 计算机不可以直接执行汇编语言，并且需要编译器
  * **-S** 开始进行程序的编译，完成编译之后停下来

  * 形成的文件后缀为**.s** 
    * 用法：gcc -S file_name.i -o file_name.s
    * ![image-20220227111317388](C:\Users\yangyr0206\Desktop\Linux课件\Linux\image-20220227111317388.png)

* **gcc -c(汇编)  file_name.s**(同样可以使用file_name)

  * 形成文件后缀为**.o**
    * ![image-20220227112641277](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227112641277.png)
  * 以二进制形式查看：od file_name.o
  * **注**：汇编形成的二进制文件，并不可以直接执行，叫可重定向目标文件（类似于vs下的**.obj**）
  * **-c**：开始进行程序的翻译，完成汇编工作就停下

* **gcc file_name.o -o file_name**(链接直接加上.o文件即可)

  * 链接
    * ![image-20220227114107732](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227114107732.png)

  * 需要链接来将我们自己代码中的函数调用，外部数据和库关联起来
  * 语言也是有库的（c语言提供了一套头文件+一套库文件libc.a和libc.so）

* 直接编译代码的两种写法（以test.c为例）

  * gcc test.c -o test(这个名字可以不和原文件名相同)

  * gcc -o test test.c

  * ldd 可以看自己的程序用了哪个库

    ![image-20220227121433608](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227121433608.png)

* **链接如何理解**

  * 可以理解为：为了让多个功能模块之间同时开发组合实现可执行

  * Linux中：静态库：**.a ** 动态库：**.so**；都与程序成功运行有关

  * 链接就是把自己写的C程序和语言上或者第三方库提供的方法关联起来

  * 所以链接也分为：静态链接  和  动态链接

    * 静态链接：库中的有关代码拷贝进自己的可执行程序中（不在需要任何库）
    * 动态链接简单理解就是：当程序运行到某个函数，自己没有定义，就会自动到库中去寻找相关信息，找到了就会执行并返回结果

  * **gcc默认采用动态链接方式，形成可执行程序**

    * ![image-20220227123425900](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227123425900.png)使用的是动态链接的共享库

    * file file_name(可执行程序)  可以查看文件信息

    * 如果想要静态链接需要在最后加 -static

      ![image-20220227123932041](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227123932041.png)如果出现这样的报错就是意味着没有安装C的静态库，安装一下就好了：**sudo yum install glibc-static**报错使用这个指令安装就可以了![image-20220227125448479](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227125448479.png)

##### 二.g++使用

* 首先gcc不能编译C++，但是g++可以跑C，但是不建议用g++去跑C

* g++的操作基本上跟gcc相同

  ![image-20220227173841257](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227173841257.png)

#### Linux调试器--gdb基础使用

* 首先是进入：**gdb file_name**(可执行程序)；退出直接输入**quit**
  * readelf -S filename (查看一个可执行程序的段构成)
* 如果一个程序是可以被调试的，那么该程序的二进制文件一定加入了一些debug信息，反之成立
  * 在centos中，默认可执行程序是release版本
  * 想要可以调试需要在执行的时候加入一些选项，例如：gcc test.c -o test -g(加上-g就是debug版本)![image-20220227180115478](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220227180115478.png)
* gdb调试程序，必须是debug方式，也就是加个-g

##### gdb调试命令

* r/run 直接跑程序相当于 vs的ctrl+F5
* list / l 显示行号

