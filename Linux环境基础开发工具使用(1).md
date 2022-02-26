# Linux环境基础开发工具使用(1)

#### 一.Linux软件包管理器yum

* **软件安装方式**

  * 源码安装
  * rpm安装 (类似于安装包)
  * yum 安装 (本身会考虑依赖关系)
    * linux安装软件，可能会存在大量的软件依赖关系，使前两种安装非常麻烦
    * yum就相当于linux的客户端

* yum 安装的部分指令

  * yum list 就是搜索，可以搜索yum上面所有的软件(root用户操作，普通用户需要用sudo来提升权限)

  * 可以通过管道和grep来进行搜索

    * 例：sudo yum list | grep 'sl'

  * yum安装指令:sudo yum install (-y可以不需要询问直接安装完成) sl.x86.64(要下载安装的软件名)

  * yum卸载指令：sudo yum remove sl.x86_64(要删除的文件名)

    * 一般删除的时候会有询问是否删除
    * 如果不想要询问可以加一个-y ：sudo yum -y remove sl.x86.64

  * ![image-20220225165007124](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220225165007124.png)

    **cd /etc/yum.repos.d/ ** (yum源在这个路径下面)，只需要**关注Centos-Base.repo**这个文件就可以了（就是yum所使用的基础服务，yum找软件时优先从这里面**找软件**）

  * **注意点：**

    * yum要工作，必须联网
    * centos里面，只能有一个yum在运行
  
* 有一个实现linux和windows文件互传的软件安装包：yum install -y lrzsz.x86_64

  * rz 是windows传到linux上
  * sz 是linux传到windows上
    * sz file_name(文件名)

#### 二.Linux开发工具

* **IDE(集成开发环境)**
* Linux编写代码：**vi ，vim**：文本编辑器(从定位上和记事本没有区别)

##### **Linux编辑器--vim使用**

* vim 就是一个文本编辑器，只能写代码

  * 打开 vim方法：**vim file_name**(文件名，可存在可不存在，不存在直接创建)
  * 退出方法**shift + ：**然后输入q

* vim是多模式的编辑器

  * **命令模式**(打开的默认模式)

    * 输入**i/a/o**进入插入模式
      1. i 进入光标位置不动
      2. a 进入插入模式光标后移一位
      3. o 进入插入模式另开一行

    * 光标相关：
      1. **h(左) j(下) k(上) l(右) **；
      2. 光标位置锚点**shift+^ (行首)  shift+$ (行尾)**
      3. 行跳转：**gg(起始行)  shift +g (结束行)   n+shift+g (跳转到指定行，n代表行数) **
      4. **w**(向后)/**b**(向前) ：以单词为单位进行光标移动
    * 文本操作：
      1. **yy** 复制当前行; **nyy**复制当前行及其之后的n行（包含当前行）;
      2. **u**：撤销误操作
         * crtl+r撤销最近一次的撤销
      3. **p**：粘贴，**np**：一次重复粘贴n行
      4. **dd**：删除光标当前所在行 （支持ndd）
         * （先）dd --> （后）p就是剪切（支持npp）
      5. **shift+~** 快速大小写切换
      6. **x**：删除光标之后的一个字符(从左向右删)，支持nx
         * **shift+x**(相当于**X**)：右向左删，支持nX
      7. **r**：替换单个字符，光标所在的字符，支持nr
         * **shift+r**(**R**)：替换模式，直接进行多个内容的替换

  * **底行模式-->shift + ：**

    * w 表示写入，q表示退出，一般对文件进行修改之后需要保存就使用shift+：然后输入wq

    * !表示强制，可以强制写入或者强制退出。例：q!，w!，wq!

    * set nonu 关闭行号

    * set nu 显示行号

    * vs file_name(文件名)  可以进行多文件操作

      1. [ctrl+w+w]光标在多文件中切换

      2. **注意：**光标在哪个文件退出的就是哪个文件

  * **插入模式**

    * 想要退出按 **esc **可以退出到命令模式
    * 一般不能插入模式直接转换成底行模式

##### vim的简单配置

* vim配置在自己的配置文件中，只会影响自己的操作

* root 有自己的vim配置文件，只影响自己

* 配置

  1. 首先在工作目录下创建 .vimrc,然后用vim打开

  2. 之后就可以在里面写指令然后保存退出，例：set nu保存退出后再使用vim就会自动显示行号

  3. curl -sLf https://gitee.com/HGtz2222/VimForCpp/raw/master/install.sh -o ./install.sh && bash ./install.sh

     只支持centos 7

  4. sudo 添加信任关系![image-20220226190505472](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220226190505472.png)

     只需要找到上面##后面的语句然后在root下面添加上自己的用户即可

