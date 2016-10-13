title: 交叉编译nginx
date: 2015-11-14 17:08:26
tags: [nginx,交叉编译]
categories: nginx
---

## （转）nginx交叉编译问题处理

@[nginx|交叉编译]

本文转载自：[http://blog.csdn.net/fish43237/article/details/40515897](http://blog.csdn.net/fish43237/article/details/40515897)

工作中在ubuntu上交叉编译目标平台是openwrt的nginx程序，期间遇到了很多问题，在这片文章中都找到了答案，顺利的实现了编译，先谢谢这篇文章的作者，我就不再进行二次创作了，转载到我的博客只是为了备忘。

<!--more-->

> ### 关键词
* （1）nginx 、 nginx-1.6.2
* （3）mips、openwrt
* （2）cross compile 、交叉编译
* （5）openssl 、 pcre 、zlib
* （8）sha1 library is not found
* （9）SSL modules require the OpenSSL library. 
* （10）make 没有什么可以做的为 `default'。
* （11）没有规则可以创建 nginx.o 需要的目标 nginx.h
* （12）checking for C compiler ... found but is not working
* （13）autotest not found
* （14）ELF ld-uClibc.so.0 not found
* （15）can not detect int size
* （16）autotest.c: No such file or directory
* （17）没有规则可以创建 nginx.o 需要的目标 nginx.h 
* （18）If you meant to cross compile, use \`--host'.See config.log for more details
* （19）ngx\_errno.c:In function 'ngx\_strerror' ngx\_errno.c:In function 'ngx\_strerror\_init': 
* （20）error: 'NGX\_SYS\_NERR' undeclared (first use in this function）
* （21）Relocations in generic ELF (EM: 3)
* （22）libssl.a:could not read symbols: File in wrong format
* （23）x86cpuid.s

### 正文：
一般来说，编译 nginx 不会有太大的问题。但是因为 nginx 对交叉编译的支持不太好。所以如果想 nginx 移植到其它环境中，会出现比较多的问题。网上也能找到几篇关于 nginx 的交叉编译的文章，但是都是针对旧一点的版本。本文 编译的是 最新的 stalble 版本，nginx-1.6.2

### 环境设置、声明变量、执行configure
#### 1. openwrt 环境变量
	 STAGING_DIR=/home/ubuntu/workplace/sdk/toolchain:$STAGING_DIR
	 export STAGING_DIR;

	 export PATH=$PATH:/home/ubuntu/workplace/sdk/toolchain/bin
#### 2. nginx 编译用到的变量
	BUILD_PATH=$PWD
	INSTALL_PATH=$PWD/install
	CC_PATH=/home/ubuntu/workplace/sdk/toolchain/bin/mips-openwrt-linux-uclibc-gcc
	CPP_PATH=/home/ubuntu/workplace/sdk/toolchain/bin/mips-openwrt-linux-uclibc-g++
	CONFIG_DIR=/app/nginx
	LOG_DIR=/app/nginx/log
	TEMP_DIR=/app/nginx/tmp 
#### 3. 执行configure，生成Makefile
	./configure \
	--prefix=$INSTALL_PATH\
	--builddir=$BUILD_PATH/build \
	--conf-path=$CONFIG_DIR/nginx.conf \
	--error-log-path=$LOG_DIR/error.log \
	--pid-path=$CONFIG_DIR/nginx.pid \
	--lock-path=$CONFIG_DIR/nginx.lock \
	--http-log-path=$LOG_DIR/access.log \
	--http-client-body-temp-path=$TEMP_DIR/body \
	--http-proxy-temp-path=$TEMP_DIR/proxy \
	--http-fastcgi-temp-path=$TEMP_DIR/fastcgi \
	--without-http_uwsgi_module \
	--without-http_scgi_module \
	--without-http_gzip_module \
	--with-http_ssl_module \
	--with-pcre=/home/ubuntu/packets/pcre-6.7 \
	--with-openssl=/home/ubuntu/packets/openssl-1.0.1i \
	--with-cc=$CC_PATH  \
	--with-cpp=$CPP_PATH \
	--with-cc-opt="-I /home/ubuntu/workplace/sdk/toolchain/include" \
	--with-ld-opt="-L /home/ubuntu/workplace/sdk/toolchain/lib"
#### 4. 执行make 和make install


### 遇到的问题
>（1）问题内容：  
checking for C compiler ... found but is not working  
./configure error : C compiler gcc is not found

>（2）原因分析：  
configure首先会编译一个小测试程序，通过测试其运行结果来判断编译器是否能正常工作，由于交叉编译器所编译出的程序是无法在编译主机上运行的，故而产生此错误。

>（3）解决办法：  
编辑auto/cc/name文件，将21行的“exit 1”注释掉（令测试程序不会报错）。

***
>（1）问题内容：  
autotest : 4 : not found  
autotest:Syntax error: Unterminated quoted string bytes   
./configure : error:can not detect int size  
cat : /home/ubuntu/packets/nginx-1.6.2/build/autotest.c: No such file or directory  

>（2）原因分析：  
configure通过运行测试程序来获得“int、long、longlong”等数据类型的大小，由于交叉编译器所编译出的程序无法在编译主机上运行而产生错误。

>（3）解决办法：  
可以通过修改configure文件来手动指定各数据类型的大小，但会非常麻烦。这里，由于编译主机与目标平台均为32位系统，故可以用“gcc”替代“mips-openwrt-linux-gcc”来进行数据类型大小的测试（注意：不同的编译环境可能编译器有点不同）  
编辑auto/types/sizeof文件，大概36行的位置（ \$CC 改为 gcc ）  
ngx\_test="\$CC \$CC\_TEST\_FLAGS \$CC\_AUX\_FLAGS  
该为ngx\_test="gcc \$CC\_TEST\_FLAGS \$CC\_AUX\_FLAGS 

***
>（1）问题内容：  
./configure:error:can not detect int size  
cat:/home/ubuntu/packets/nginx-1.6.2/build/autotest.c: No such file ordirectory

>（2）原因分析：检测不到size

>（3）解决办法：两个问题，一个是检测不到size。一个是找不到测试程序源代码。  
先解决 size 问题，修改 auto/types/sizeof 文件，找到 ngx_size= 这一行，改为 ngx_size=4  
修改完之后，再次configure，发现 autotest.c 的问题也消失了。

***
>（1）问题内容：sha1 library is not found

>（2）原因分析：弄不清楚

>（3）解决办法：configure的时候增加参数 --with-http_ssl_module

***
>（1）问题内容：./configure : error :SSL modules require the OpenSSL library.

>（2）原因分析：没有说清楚openssl的源代码路径

>（3）解决办法：指明openssl代码的路径。--with-openssl=DIR（DIR表示openssl源代码解压的路径）

***
>（1）问题内容： make:没有什么可以做的为`default'。

>（2）原因分析：没有说清楚openssl的源代码路径

>（3）解决办法：  
进入build目录（具体是哪个目录，需要察看Makefile，执行make）  
譬如说我的 configure 生成的 Makefile 里面有一段  
>
	build:  
	$(MAKE) -f /home/ubuntu/packets/nginx-1.6.2/build/Makefile  
	$(MAKE) -f /home/ubuntu/packets/nginx-1.6.2/build/Makefile manpage  
所以应该进入/home/ubuntu/packets/nginx-1.6.2/build目录，执行make

***
>（1）问题内容：没有规则可以创建 “/home/ubuntu/packets/nginx-1.4.7/build/src/core/nginx.o” 需要的目标 “src/core/nginx.h” 。

>（2）原因分析：不知道是不是软件的 bug，指定build路径后，build路径里面的源代码是空的

>（3）解决办法：把主目录的src文件夹里面的东西，拷贝到build目录（build目录请根据自己实际情况进行确定，参考上面那个问题）

***
>（1）问题内容：  
checking whether the C compiler works... configure: error: cannot run Ccompiled programs.  
If you meant to cross compile, use \`--host'.  
See `config.log' for more details.  
make:\*\*\* [/home/ubuntu/packets/pcre-6.7/Makefile]错误1  

>（2）原因分析：出错的是pcre而不是openssl。原因是编译 pcre 没有指定 `--host' 参数

>（3）解决办法：  
打开build目录的Makefile，找到 pcre 参数设定的那几行。  
>
	/home/ubuntu/packets/pcre-6.7/Makefile :  /home/ubuntu/packets/nginx-1.6.2/build/Makefile  
	cd /home/ubuntu/packets/pcre-6.7 \  
	&& if [ -f Makefile ] ; then $(MAKE) distclean; fi \
	&& CC="$(CC)" CFLAGS="-O2 -fomit-frame-pointer -pipe "\
	./configure --disable-shared
在configure那里再添加一个参数 --host=mips-openwrt-linux（具体添加需要根据实际情况，参考我的编译器是mips-openwrt-linux-gcc，所以host是mips-openwrt-linux）

***
>（1）问题内容：  
src/os/unix/ngx_errno.c : In function'ngx_strerror':  
src/os/unix/ngx_errno.c : 37 : 31 :error:'NGX_SYS_NERR' undeclared (first use in this function)  
src/os/unix/ngx_errno.c : 37 : 31 :note: each undeclared identifier is reported only once for eachfunction it appears in  
src/os/unix/ngx_errno.c : In function'ngx_strerror_init':  
src/os/unix/ngx_errno.c : 58 :11 : error : 'NGX_SYS_NERR' undeclared (first use in this function)  
make:\*\*\* [/home/ubuntu/packets/nginx-1.6.2/build/src/os/unix/ngx\_error.o] 错误1  

>（2）原因分析：  
NGX\_SYS\_NERR未定义，NGX\_SYS\_NERR正常情况下应定义在objs/ngx\_auto\_config.h文件中，特别注意，这是一个auto性质的文件，只有在执行了./configure后，才能生成这个文件。宏NGX\_SYS\_NERR的意义为，在Linux系统中有132个错误编码。

>（3）解决办法：  
找到ngx\_auto\_config.h这个文件（我的ngx\_auto\_config.h在build目录，不同的configure参数，这个文件的位置可能不一样）
在文件中添加如下三行：  
	#ifndef NGX_SYS_NERR
	#define NGX_SYS_NERR  132
	#endif
然后，继续执行make。

***
>（1）问题内容：  
./dftables pcre_chartables.c  
/bin/bash:./dftables:无法执行二进制文件

>（2）原因分析：dftables需要在编译的时候运行，而交叉编译的程序是不能运行的，要用系统的gcc来编译才能运行

>（3）解决办法：用x86的gcc来单独编译dftables（我的是ubuntu自带的gcc）。  
进入pcre代码所在的目录，并且执行
	gcc -Wall -O2 -DCROSS_COMPILE dftables.c -o dftables
执行完之后，返回刚才的目录，执行 make 继续编译

***
>（1）问题内容：  
openssl-1.0.1i/.openssl/lib/libssl.a(s23_meth.o):Relocations in generic ELF(EM: 3)  
libssl.a:could not read symbols: File in wrong format

>（2）原因分析：原因是编译在nginx执行configure的时候，交叉编译参数没有很好的传到openssl。

>（3）解决办法：   
打开主目录的Makefile，搜索到包含openssl-1.0.1i/.openssl/include/openssl/ssl.h的那一行（可能不同的版本会有一点点区别，搜索openssl，找到最接近那行）  
然后，在config的参数那里增加一个，指明交叉编译器所在的路径（请根据实际情况设定）  
 --cross-compile-prefix=/home/ubuntu/............/mips-openwrt-linux-  
(需要注意的是，最后那里的横杠不能少，如果编译器是mips-openwrt-linux-gcc那就是mips-openwrt-linux-，如果是arm-linux-gcc就是arm-linux-)  
修改完makefile之后，会导致make把所有东西都构建一遍，所以之前已经解决了的那几个问题可能会重新出现，需要再解决一回。

***

>（1）问题内容：  
x86cpuid.s:Assembler messages:  
x86cpuid.s:168:Error: Unrecognized opcode `hlt'

>（2）原因分析：原因是编译在nginx执行configure的时候，交叉编译参数没有很好的传到openssl。

>（3）解决办法：找到上面那个问题添加--cross-compile-prefix的那一行，继续添加一个参数no-asm，禁用汇编代码。

***
>（1）问题内容：  
./home/ubuntu/packets/nginx-1.6.2/build/src/core/ngx_cycle.o:In function \`ngx_init_cycle':  
/home/ubuntu/packets/nginx-1.6.2/build/src/core/ngx_cycle.c:457:undefined reference to \`ngx_shm_free'  
/home/ubuntu/packets/nginx-1.6.2/build/src/core/ngx_cycle.c:462:undefined reference to \`ngx_shm_alloc'  
/home/ubuntu/packets/nginx-1.6.2/build/src/core/ngx_cycle.c:648:undefined reference to \`ngx_shm_free'  
/home/ubuntu/packets/nginx-1.6.2/build/src/event/ngx_event.o:In function \`ngx_event_module_init':  
/home/ubuntu/packets/nginx-1.6.2/build/src/event/ngx_event.c:525:undefined reference to \`ngx_shm_alloc'  
collect2:ld returned 1 exit status  
make:\*\*\* [/home/ubuntu/packets/nginx-1.6.2/build/nginx]错误1

>（2）原因分析：\`ngx\_shm\_free'函数未定义  
通过查看源码可以发现，\`ngx\_shm\_free'定义在src/os/unix/ngx\_shmem.c文件中，这个函数要正常使用的话必须要求  
“NGX\_HAVE\_MAP\_ANON、NGX\_HAVE\_MAP\_DEVZERO、NGX\_HAVE\_SYSVSHM”这三个宏中有一个被定义。

>（3）解决办法：  
修改ngx\_auto\_config.h ,加入这几行：
	#ifndef NGX_HAVE_SYSVSHM
	#define NGX_HAVE_SYSVSHM 1
	#endif
