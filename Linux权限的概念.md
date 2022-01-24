# Linux权限的概念

* ps axj | grep bash 查看进程

#### 1.Linux中，默认有两类用户

* root：超级管理员，具有非常高的权限
* 普通用户：具有一般权限，需要受权限约束的
* su - 普通转换成超级管理员（root密码）
* 用户回退：建议exit或者ctrl+d，不建议su - 用户
* sudo 临时权限提升，执行后续命令，以root身份执行（要的是普通用户的密码）

#### 2.理解什么是权限

* 权限约束的是人，文件本身就有的天然的权限属性 r + w +x
* 简单来说：一件事情是否允许被特定的人做（权限=人+事物的属性）

#### 3.Linux 中的用户类别：

![image-20220224132738557](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220224132738557.png)

* 拥有者 owner
* 所属组 group
  * 主要就是方便能让自己和同组的人看到，而不然其他人看到
* 其他 other
* 拥有者，所属组，other：指的是一种角色身份，而root和普通用户是一个确定的人

#### 4.标识文件类型

1. 第一列是文件类型

![image-20220224200055424](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220224200055424.png)

* -: 普通文件 [文本，各种动静态库，可执行文件，源程序]
* d：目录文件
* c：字符设备文件（主要键盘与显示器）
* b：块设备文件 （主要磁盘）
* p：管道文件（主要通信）
* l：链接文件（软连接）

**Linux下一切皆文件**

2. 后面九列字符

* ![image-20220224201149325](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220224201149325.png)
  * 后面九列三三为一组，分别对应拥有者，所属组，其他人的权限。
  * 举例：r(是否具有读)w(是否具有写)-(是否具有可执行) ：这就代表可以具有读写权限，但是没有执行权限

#### 5.修改权限

* **chmod 修改文件权限（读写执行，且是永久生效）**
  * chmod u(改的是拥有者)+(增加权限) /     -(删除权限)  file_name(文件名)
  * chmod g(改的是所属组)+/- file_name
  * chmod o(改的是其他人)+/-   file_name
  * 可以连着修改的例如：chmod u+rwx,g+rwx,o+rwx file_name
  * chmod a(代表all就是所有的都一起加) +/- file_name 
* 





