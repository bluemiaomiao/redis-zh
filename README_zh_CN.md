此README只是一个快速*快速开始*文档。您可以在[redis.io](https://redis.io)找到更详细的文档。

啥是Redis?
--------------
Redis通常被称为一个*数据结构*服务器。这意味着Redis通过一组命令提供了对可变数据结构的访问，这些命令是使用一个带有TCP套接字和简单协议的*服务器/客户端*模型发送的。因此，不同的进程可以以共享的方式查询和修改相同的数据结构。

在Redis中实现的数据结构有几个特殊的属性:

* Redis关心将它们存储在磁盘上，即使它们总是被提供并修改到服务器内存中。这意味着Redis速度很快，但它也是非易失性的。
* 数据结构的实现强调内存效率，因此与使用高级编程语言建模的相同数据结构相比，Redis内部的数据结构可能会使用更少的内存。
* Redis提供了许多在数据库中很常见的功能，比如复制、可调的耐久性、集群和高可用性。

另一个很好的例子是将Redis视为memcached的一个更复杂的版本，其中的操作不仅仅是set和get，还包括处理复杂数据类型（如列表、集合、有序数据结构等）的操作。

如果您想了解更多信息，以下是所选起点的列表：

* Redis数据类型简介。https://redis.io/topics/data-types-intro
* 直接在浏览器中尝试Redis。http://try.redis.io
* Redis命令的完整列表。https://redis.io/commands
* Redis官方文档中还有更多内容。https://redis.io/documentation

构建Redis
--------------
Redis可以在Linux、OSX、OpenBSD、NetBSD和FreeBSD上编译和使用。我们支持大端(big-endian)和小端(little-endian)体系结构，并且都支持32位和64位系统。

它可以在Solaris派生的系统（例如SmartOS）上编译，但我们的对该平台的支持是“尽力而为”，Redis不保证在Linux、OSX和任何BSD发行版中也能正常工作。

简单的执行下面的命令:

```shell
make
```

要使用TLS支持进行构建，您需要OpenSSL开发库（例如，在Debian/Ubuntu上安装libssl-dev）并运行:

```shell
make BUILD_TLS=yes
```

要使用systemd支持进行构建，您需要systemd开发库（例如，作为Debian/Ubuntu上的libsystemd-dev或CentOS上的systemd-devel）并运行：

```shell
make USE_SYSTEMD=yes
```

要为Redis程序名添加后缀，请使用：

```shell
make PROG_SUFFIX="-alt"
```

构建32位的Redis请使用：

```shell
make 32bit
```

在构建Redis之后，最好使用以下工具对其进行测试：

```shell
make test
```

如果构建了TLS，则在启用TLS的情况下运行测试（您需要保证`tcl-tls`被安装）:

```shell
./utils/gen-test-certs.sh
./runtest --tls
```

修复依赖项或缓存生成选项的生成问题
--------------

如果在使用32位目标构建Redis之后，您需要重建64位的目标，或者相反，您需要在Redis源码目录执行 `redis distclean`。

如果在尝试构建Redis的32位二进制文件时出现构建错误，请尝试以下步骤：

* 安装libc6-dev-i386 (也可以试试g++-multilib).
* 尝试使用下面的命令来取代 `make 32bit` :

```shell
make CFLAGS="-m32 -march=native" LDFLAGS="-m32"
```

分配器
--------------

在构建Redis时，通过设置"MALLOC"环境变量来选择非默认内存分配器。默认情况下，Redis是根据libc malloc编译和链接的，但在Linux系统上，jemalloc是默认的。选择此默认值是因为事实证明，jemalloc比libc malloc具有更少的碎片问题。

要强制对libc malloc进行编译，请使用:

```shell
make MALLOC=libc
```

要在Mac OS X系统上针对jemalloc进行编译，请使用:

```shell
make MALLOC=jemalloc
```

单调时钟
---------------

默认情况下，Redis将使用POSIX clock_gettime函数作为单调时钟源进行构建。在大多数现代系统中，内部处理器时钟可用于提高性能。注意事项如下：http://oliveryang.net/2015/09/pitfalls-of-TSC-usage/

要在支持处理器内部指令时钟的情况下构建，请使用:

```shell
make CFLAGS="-DUSE_PROCESSOR_CLOCK"
```

细致的构建
---------------

默认情况下，Redis将使用用户友好的彩色输出进行构建。如果希望看到更详细的输出，请使用以下命令:

```shell
make V=1
```

运行Redis
---------------

要以默认配置运行Redis，只需输入：

```shell
cd src
./redis-server
```

如果你想提供你的redis.conf，必须使用附加参数（配置文件的路径）运行它：

```shell
cd src
./redis-server /path/to/redis.conf
```

通过使用命令行将参数直接作为选项传递，可以更改Redis配置。例如：

```shell
./redis-server --port 9999 --replicaof 127.0.0.1 6379
./redis-server /etc/redis/6379.conf --loglevel debug
```

redis.conf中的所有选项也支持作为选项使用命令，名字完全相同。

运行带有TLS的Redis
--------------------

请查阅[TLS.md](TLS.md)文件，了解如何将Redis与TLS结合使用的更多信息。

Redis怎么玩
--------------------

您可以使用redis cli来使用redis。启动redis服务器实例，然后在另一个终端中尝试以下操作：

```shell
cd src
./redis-cli
redis> ping
PONG
redis> set foo bar
OK
redis> get foo
"bar"
redis> incr mycounter
(integer) 1
redis> incr mycounter
(integer) 2
redis>
```

您可以在以下位置找到所有可用命令的列表：https://redis.io/commands.