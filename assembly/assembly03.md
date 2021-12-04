# Debug 实战

* [Debug 实战](#debug-实战)
   * [Debug 是什么](#debug-是什么)
   * [Debug 实战](#debug-实战-1)
      * [Debug -r](#debug--r)
      * [Debug -d](#debug--d)
      * [Debug -e](#debug--e)
      * [Debug -u](#debug--u)
      * [Debug -t](#debug--t)
      * [Debug -a](#debug--a)
   * [总结](#总结)

我们上篇文章了解了一下基本的寄存器，这篇文章我们来进行实际操作一下。

我们以后将会用到很多 Debug 命令，这里我们先来熟悉一下它们。

## Debug 是什么

Debug 是 Windows / Dos 操作系统提供的一种功能。使用 Debug 能让我们方便查看 CPU 各种寄存器的值、内存情况，方便我们调试指令、跟踪程序的运行过程。

接下来我们会用到很多 debug 命令，但是使用这些命令的前提是，你需要在电脑上安装一下 debug，Windows/Mac 都可以安装，获取链接我已经给你找出来了。阿，忘记说了，我们这里使用的是 *Dos box*来模拟汇编的操作环境。

传送门（Mac 和 Windows 都是）：https://www.dosbox.com/download.php?main=1

![image-20211017223618159](https://tva1.sinaimg.cn/large/008i3skNly1gvionrp66nj60j30fl40s02.jpg)

下载完成后打开 DosBox ，打开之后是这样的。

![image-20211017223818599](https://tva1.sinaimg.cn/large/008i3skNly1gvioptcx9gj60hj0b0jsj02.jpg)

此时我们输入 debug 命令应该提示的是 

![image-20211017223853097](https://tva1.sinaimg.cn/large/008i3skNly1gvioqevrplj605b025q2q02.jpg)

因为我们还没有进行连接和挂载，此时我们执行

```c
mount c D:\debug
```

执行这条命令时，你需要现在 D 盘下创建一个 debug 文件夹，然后我们挂载到 debug 下面。

并且执行 `C:` 切换到 C 盘路径下。

![image-20211017224242053](https://tva1.sinaimg.cn/large/008i3skNly1gvioudksqwj60b802gmx102.jpg)

此时我们就可以执行 debug 命令了。

![image-20211017224458748](https://tva1.sinaimg.cn/large/008i3skNly1gviowrfe6dj60az03jq2u02.jpg)

这里需要注意一点，我在 Windows 10 系统下搭建 Debug 环境时，在挂载完成后输入 debug ，还是提示 *Illegal command:debug* ，此时你需要再下载一个 debug.exe ，贴心的我也把下载地址给你了。

下载地址：https://pan.baidu.com/s/177arSA34plWqV-iyffWpEw#list/path=%2F 密码：3akd

需要下载里面的 *debug.exe*，然后把它放在你挂载的路径下，这里我挂载的路径时 D 盘下的 debug 文件夹。

放置完成之后，再输入 debug 就可以了。

---

因为每次打开 Dosbox 都会执行上面这些命令，真的好烦，那怎么办呢？一个简单的办法是在 Dosbox 安装路径下找到 

![image-20211017225250971](https://tva1.sinaimg.cn/large/008i3skNly1gvip4xvmh4j60uq06b75j02.jpg)

打开之后，在末尾键入

![image-20211017225326436](https://tva1.sinaimg.cn/large/008i3skNly1gvip5ka5mtj60ex079wf102.jpg)

就 OK 了，下次直接打开 Dosbox ，会默认执行这三条命令，至此，就是我搭建 Dosbox 遇到的所有问题了。

## Debug 实战

玩儿汇编得学会用 Debug ，Debug 是一种**调试程序**，通过 Debug 能让我们能够看到内存值，跟踪堆栈情况，看到寄存器所暂存的内容等，同时也能够更好地帮助我们理解汇编代码，所以学会 Debug ，**非常重要**，这是一种不可或缺的动手能力。

下面我们会用到几种 Debug 命令，这里先简单介绍下。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwiil6fj2qj31040rm76a.jpg" alt="image-20211117222634775" style="zoom:50%;" />

Debug 命令有很多，不过常用的一般就上面这几个。

好了，现在我们直接进入正题，开始在 Dosbox 上正式进行 Debug 操作，首先打开 Dosbox。

嗯。。。。。。这个界面我们打开很多次了。

那我写个命令呢？ 好吧，没演示过，下面就来了！

### Debug -r

亲，用 **Debug -r** 就可以查看和修改 CPU 寄存器内容了呢。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwij6myjunj30b30ayt99.jpg" alt="image-20211117224711832" style="zoom:67%;" />

查看寄存器内容。

![image-20211117225112134](https://tva1.sinaimg.cn/large/008i3skNly1gwijaserauj30hv08qjsw.jpg)

>这里需要注意一下 -r 大小写的问题，Debug -r 是查看寄存器内容。而 -R 则是无效指令。

上图列出来了很多寄存器，你可能觉得无从下手，不要乱，我们先从最基本的开始入手，也就是 CS 和 IP，CS（Code Segment）是代码段寄存器，一般也被称为段基址，可以认为是程序访问的入口，CPU 需要从 CS 中找到从哪个位置开始取指执行，但是我们还不知道要取哪一段，这时候 IP 的作用就体现出来了，IP（Instruction Pointer）就是指令指针寄存器，也叫做偏移地址，它会告诉我们从段基址开始，取哪一段的地址。

可以使用段基址:偏移地址来确定内存中的指定地址。

>这里我们只是简单聊一下这两个寄存器的概念，要了解这两个寄存器的具体作用，可以看笔者的上一篇文章 

使用 -r 也能够修改寄存器的内容，如下所示

![image-20211119182455417](https://tva1.sinaimg.cn/large/008i3skNly1gwkmucc2tcj30ht04ywes.jpg)

-r 一般的格式是 **-r 寄存器**，然后系统会进行冒号提示，后面就是你要修改的内容。

### Debug -d

使用 -d 指令可以查看内存中的内容。

![image-20211119183448427](https://tva1.sinaimg.cn/large/008i3skNly1gwkn4mplxrj30hw04o0te.jpg)

输出的内存值默认是按照 CS:IP 的地址开始的，由于 CS 的值默认是 073F，而 IP 默认是 0100，所以 -d 的内存值是 073F:0100 。

-d 的格式很多，下面只介绍一下常用的几种格式。

形似 -d 1000:0 这种 **-d 段基址 偏移地址**的格式可以产生如下输出。

![image-20211119184935344](https://tva1.sinaimg.cn/large/008i3skNly1gwknk06eokj30ht04oaax.jpg)

如上图所示，Debug 会列出指定内存单元中的的内容。上图中的每一个 00 都表示 8 位，如果是 4A，那么这八位展开来说就是 0010 1011 。每一行有 16 个 8 位，所以构成了 128 位内存地址。

>为什么都是 00 呢，因为内存单元的值没有被改写，说白了就是这块内存区域没有存值，如何改写我们后面回收。

每一行的中间都有一个 `-`，这个是为了便于我们阅读来设置的，- 号前后都有 8 个内存单元，这样便于查看。

右侧几个 ...... 表示每个内存单元可显示的 ASCII 码字符，因为内存没有值，所以也没有对应的 ASCII 码。我们可以数一下，每行有 16 个 . ，这表示每一个 00 都对应了一个 ASCII 码。

我们可以使用 -d 1000:9 这种 **-d 段基址:起始偏移地址** 格式来显示从 1000 的第几位开始。

![image-20211119194232683](https://tva1.sinaimg.cn/large/008i3skNly1gwkp33ju17j30ht051q3n.jpg)

Debug 从 1000:9 开始，一直到 1000:88，一共是  128 个字节，第一行中的 1000:0 ~ 1000:8 中的内容没有显示。

还可以使用 -d 1000:0 9 这种 **-d 段基址:起始偏移地址 结尾偏移地址**的格式来输出。

![image-20211119194856418](https://tva1.sinaimg.cn/large/008i3skNly1gwkp9rckhuj30hm01bmx0.jpg)

还可以是使用 -d 偏移地址来在不指定段基址的情况下，查看内存值。

![image-20211119201832383](https://tva1.sinaimg.cn/large/008i3skNly1gwkq4k0nz6j30hu04l74z.jpg)

### Debug -e

上面说的都是查看内存中指定位置或者区域的值，下面我们要来改写一下内存值。

使用 `-e` 可以改写内存值，比如我们想要改写 1000:0 ~ 1000:f 中的内容，可以使用 -e 1000:0 0 1 2 3 4 5 6 7 8 9 0 a b c d e f 这种方式，如下图所示。

![image-20211119201236510](https://tva1.sinaimg.cn/large/008i3skNly1gwkpye62l3j30hr09vgnj.jpg)

这里需要注意下，在进行 -e 改写的时候，每个值中间都有一个空格，如果没有空格的话，会当做一个内存值来看待。

然后用 -d 1000:0 看到我们刚改写的内存值。

还可以使用提问的方式来逐个修改从某一地址开始的内存单元的内容。

还是用 1000:100 来举例子，输出 -e 1000:100 后按下回车键。

![image-20211119205201568](https://tva1.sinaimg.cn/large/008i3skNly1gwkr3efybjj30hu08a75h.jpg)

如上图所示，可以看到我们先输入了一次 -e 1000:100 这个指令，然后按下了回车键。

>注意，如果这里你按下了回车键，就相当于整个 -e 改写的过程已经完成。

如果你想要继续改写后面内存中的值，你需要按下空格键。

我们改写了 1000:100 之后的内存值，然后使用 -d 1000:100 查看我们改写的内容是否生效。

-e 命令还可以支持写入字符，比如我们可以向 1000:0 这个位置开始写入数值和字符，-e 1000:0 1 'a' 2 'b' e 'c' 。

![image-20211119210708341](https://tva1.sinaimg.cn/large/008i3skNly1gwkrj4kmlrj30hk05ijsa.jpg)

如上图所示，当我们向内存写入字符 'a' 'b' 'c' 的时候，会自动转换为 ASCII 码进行存储，在最右侧可以找到刚刚写入的字符。

### Debug -u

如何向内存中写入一段机器码呢？比如我们想要在内存中写入一段机器码。

![image-20211119220152007](https://tva1.sinaimg.cn/large/008i3skNly1gwkt42hpzfj30b3068mx7.jpg)

我们可以使用 -e 来进行写入，向内存中写入 b8 01 00 b9 02 00 01 c8 这个机器码，如下所示

![image-20211119224014093](https://tva1.sinaimg.cn/large/008i3skNly1gwku7zxc5zj30hd04x0tl.jpg)

我们使用 -e 写入之后，使用 -d 查看内存值，可以发现我们刚刚写入的值，但是却看不到机器码，所以机器码该如何看呢？

别急，还有个 -u 命令，这个就是看机器码的，如下图所示，我们使用 -u 命令显示我们写入的机器码。

![image-20211119224851565](https://tva1.sinaimg.cn/large/008i3skNly1gwkugyebazj30an07taaq.jpg)

可以看到 1000:0000 ~ 1000:0006 这个内存地址使我们写入的机器码，-u 这个命令就是将内存单元的内容翻译为汇编指令并显示。

-u 输出的结果分为三部分显示：

* 最左侧是每一条机器指令的地址；
* 中间是机器指令；
* 最右侧是机器指令执行的汇编指令。

1000:0 处存放的是写入的机器码 B8 01 00 组成的机器指令，对应的汇编指令是 MOV AX,0001。

1000:0003 处存放的是写入的机器码 B9 02 00 组成的机器指令，对应的汇编指令是 MOV CX,0002。

1000:0006 处存放的是写入的机器码 C1 C8 所组成的机器指令，对应的汇编指令是 add ax,cx。

### Debug -t

上面介绍的一系列指令包括我们上面提到的 Debug -e 机器码都是向内存中进行写入，那么如何执行这些指令呢？

我们可以使用 Debug -t 来执行写入的指令。使用 Debug -t 可以执行由 CS:IP 指向的指令。

既然是 -t 能够执行从 CS:IP 指向的命令，所以我们有必要将 CS:IP 指向 1000:0（因为我们前面将指令写在了 1000:0 处）。

首先我们需要执行 -r cs 1000 ，-r ip 0 把 CS:IP 赋值为 1000:0。

然后执行 -t 指令，下图是已经执行过的指令截图。

![image-20211120060826534](https://tva1.sinaimg.cn/large/008i3skNly1gwl76cpwdhj30nu04gt9c.jpg)

可以看到，执行完 -t 指令之后，MOV AX,0001 这条指令被执行，当前 AX 寄存器的内容变为了 0001，这条汇编指令的意思就是把 0001 移动到 AX 寄存器中。

继续执行 -t 之后，我们可以看到寄存器的变化。

![image-20211120061105506](https://tva1.sinaimg.cn/large/008i3skNly1gwl793vluuj30hu0avwg5.jpg)

### Debug -a

毕竟机器指令不是那么好懂，写入很不方便，所以有没有办法能够支持我们直接写入汇编指令呢？还真有，Debug 提供了 -a 这种方式来实现汇编指令的写入。如下图所示

![image-20211120230320647](https://tva1.sinaimg.cn/large/008i3skNly1gwm0ig66zsj30hl07yq3i.jpg)

可以看到，我们使用了 -a 命令来对 1000:0 进行写入，分别输入 mov ax,1 mov bx,2 mov cx,3 add ax,bx add ax,cx add ax,ax 指令，然后按回车进行确定执行。

我们使用 -d 1000:0 f 可以看到从偏移地址 0 处开始的第 f 个内存指令（因为最大写入的地址只是 f）。

![image-20211120231016386](https://tva1.sinaimg.cn/large/008i3skNly1gwm0pm78t8j30hz01b0sm.jpg)

上图中的 1000:000F 为什么有值呢，因为我们上面已经执行过这个写入了。

另外，使用 -a 可以从一个预设的地址处开始输入指令。

## 总结

今天和大家聊了一下 Debug 的基本用法，主要包括

* -r 查看、修改寄存器中的内容
* -d 查看内存中的指令
* -e 修改内存中的内容
* -u 可以将内存中的内容解释为机器指令和对应的汇编指令
* -t 执行 CS:IP 处的指令
* -a 以汇编得形式向内存写入内容

汇编指令的选项有很多，上面介绍的这些属于经常用到的指令，这些指令要能够熟练使用。

***

强烈安利一下作者自己的公众号，干货内容非常多，并且在公众号后台回复「cxuan」和「网络」有作者自己肝的 PDF，快来领取吧。

![image-20210716163352584](https://tva1.sinaimg.cn/large/008i3skNly1gsivkbczxoj31l20t8al5.jpg)

![image-20210716163433337](https://tva1.sinaimg.cn/large/008i3skNly1gsivl4khz9j31d60h8mze.jpg)

