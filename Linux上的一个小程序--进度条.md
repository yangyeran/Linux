# Linux上的一个小程序--进度条

* 回车换行：

  * 回车：回到当前行的最开始

  * 换行：列不变，新起一行

* printf 如果不带\n 数据不会被立即刷新，而是被暂存在缓冲区当中(显示器设备刷新策略就是行刷新，遇到\n即进行刷新)

* **fflush**(就是立即刷新)

  **fflush(stdout);**

  ![image-20220304151854672](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304151854672.png)

  C程序，默认会打开三个输入输出流：stdin(键盘)  stdout(显示器)  stderr(显示器)![image-20220304152157953](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304152157953.png)

* \r 只回车不换行

* 这样就可以写一个简单的倒计时

  ![image-20220304174219971](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304174219971.png)

  **凡是显示到显示器上面的内容都是字符，凡事从键盘读取的内容都是字符(键盘，显示器是字符设备)**；如果把=5 换成10

  会出现90 80 70 60。。。因为当成字符的话9只是一个字符而10是两个字符，所以只会覆盖掉第一个字符；解决方法就是%2d即可，这就是进度条的代码

  ![image-20220304185506680](C:\Users\yangyr0206\AppData\Roaming\Typora\typora-user-images\image-20220304185506680.png)

