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

* Redis数据类型简介。[https://redis.io/topics/data-types-intro](https://redis.io/topics/data-types-intro)
* 直接在浏览器中尝试Redis。[http://try.redis.io](http://try.redis.io)
* Redis命令的完整列表。[https://redis.io/commands](https://redis.io/commands)
* Redis官方文档中还有更多内容。[https://redis.io/documentation](https://redis.io/documentation)

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

默认情况下，Redis将使用POSIX clock_gettime函数作为单调时钟源进行构建。在大多数现代系统中，内部处理器时钟可用于提高性能。注意事项如下：[http://oliveryang.net/2015/09/pitfalls-of-TSC-usage/](http://oliveryang.net/2015/09/pitfalls-of-TSC-usage/)

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

您可以在以下位置找到所有可用命令的列表：[https://redis.io/commands](https://redis.io/commands)。

Installing Redis
-----------------

In order to install Redis binaries into /usr/local/bin, just use:

    % make install

You can use `make PREFIX=/some/other/directory install` if you wish to use a
different destination.

Make install will just install binaries in your system, but will not configure
init scripts and configuration files in the appropriate place. This is not
needed if you just want to play a bit with Redis, but if you are installing
it the proper way for a production system, we have a script that does this
for Ubuntu and Debian systems:

    % cd utils
    % ./install_server.sh

_Note_: `install_server.sh` will not work on Mac OSX; it is built for Linux only.

The script will ask you a few questions and will setup everything you need
to run Redis properly as a background daemon that will start again on
system reboots.

You'll be able to stop and start Redis using the script named
`/etc/init.d/redis_<portnumber>`, for instance `/etc/init.d/redis_6379`.

Code contributions
-----------------

Note: By contributing code to the Redis project in any form, including sending
a pull request via Github, a code fragment or patch via private email or
public discussion groups, you agree to release your code under the terms
of the BSD license that you can find in the [COPYING][1] file included in the Redis
source distribution.

Please see the [CONTRIBUTING][2] file in this source distribution for more
information, including details on our process for security bugs/vulnerabilities.

[1]: https://github.com/redis/redis/blob/unstable/COPYING
[2]: https://github.com/redis/redis/blob/unstable/CONTRIBUTING

Redis internals
===

If you are reading this README you are likely in front of a Github page
or you just untarred the Redis distribution tar ball. In both the cases
you are basically one step away from the source code, so here we explain
the Redis source code layout, what is in each file as a general idea, the
most important functions and structures inside the Redis server and so forth.
We keep all the discussion at a high level without digging into the details
since this document would be huge otherwise and our code base changes
continuously, but a general idea should be a good starting point to
understand more. Moreover most of the code is heavily commented and easy
to follow.

Source code layout
---

The Redis root directory just contains this README, the Makefile which
calls the real Makefile inside the `src` directory and an example
configuration for Redis and Sentinel. You can find a few shell
scripts that are used in order to execute the Redis, Redis Cluster and
Redis Sentinel unit tests, which are implemented inside the `tests`
directory.

Inside the root are the following important directories:

* `src`: contains the Redis implementation, written in C.
* `tests`: contains the unit tests, implemented in Tcl.
* `deps`: contains libraries Redis uses. Everything needed to compile Redis is inside this directory; your system just needs to provide `libc`, a POSIX compatible interface and a C compiler. Notably `deps` contains a copy of `jemalloc`, which is the default allocator of Redis under Linux. Note that under `deps` there are also things which started with the Redis project, but for which the main repository is not `redis/redis`.

There are a few more directories but they are not very important for our goals
here. We'll focus mostly on `src`, where the Redis implementation is contained,
exploring what there is inside each file. The order in which files are
exposed is the logical one to follow in order to disclose different layers
of complexity incrementally.

Note: lately Redis was refactored quite a bit. Function names and file
names have been changed, so you may find that this documentation reflects the
`unstable` branch more closely. For instance, in Redis 3.0 the `server.c`
and `server.h` files were named `redis.c` and `redis.h`. However the overall
structure is the same. Keep in mind that all the new developments and pull
requests should be performed against the `unstable` branch.

server.h
---

The simplest way to understand how a program works is to understand the
data structures it uses. So we'll start from the main header file of
Redis, which is `server.h`.

All the server configuration and in general all the shared state is
defined in a global structure called `server`, of type `struct redisServer`.
A few important fields in this structure are:

* `server.db` is an array of Redis databases, where data is stored.
* `server.commands` is the command table.
* `server.clients` is a linked list of clients connected to the server.
* `server.master` is a special client, the master, if the instance is a replica.

There are tons of other fields. Most fields are commented directly inside
the structure definition.

Another important Redis data structure is the one defining a client.
In the past it was called `redisClient`, now just `client`. The structure
has many fields, here we'll just show the main ones:
```c
struct client {
    int fd;
    sds querybuf;
    int argc;
    robj **argv;
    redisDb *db;
    int flags;
    list *reply;
    char buf[PROTO_REPLY_CHUNK_BYTES];
    // ... many other fields ...
}
```
The client structure defines a *connected client*:

* The `fd` field is the client socket file descriptor.
* `argc` and `argv` are populated with the command the client is executing, so that functions implementing a given Redis command can read the arguments.
* `querybuf` accumulates the requests from the client, which are parsed by the Redis server according to the Redis protocol and executed by calling the implementations of the commands the client is executing.
* `reply` and `buf` are dynamic and static buffers that accumulate the replies the server sends to the client. These buffers are incrementally written to the socket as soon as the file descriptor is writeable.

As you can see in the client structure above, arguments in a command
are described as `robj` structures. The following is the full `robj`
structure, which defines a *Redis object*:

    typedef struct redisObject {
        unsigned type:4;
        unsigned encoding:4;
        unsigned lru:LRU_BITS; /* lru time (relative to server.lruclock) */
        int refcount;
        void *ptr;
    } robj;

Basically this structure can represent all the basic Redis data types like
strings, lists, sets, sorted sets and so forth. The interesting thing is that
it has a `type` field, so that it is possible to know what type a given
object has, and a `refcount`, so that the same object can be referenced
in multiple places without allocating it multiple times. Finally the `ptr`
field points to the actual representation of the object, which might vary
even for the same type, depending on the `encoding` used.

Redis objects are used extensively in the Redis internals, however in order
to avoid the overhead of indirect accesses, recently in many places
we just use plain dynamic strings not wrapped inside a Redis object.

server.c
---

This is the entry point of the Redis server, where the `main()` function
is defined. The following are the most important steps in order to startup
the Redis server.

* `initServerConfig()` sets up the default values of the `server` structure.
* `initServer()` allocates the data structures needed to operate, setup the listening socket, and so forth.
* `aeMain()` starts the event loop which listens for new connections.

There are two special functions called periodically by the event loop:

1. `serverCron()` is called periodically (according to `server.hz` frequency), and performs tasks that must be performed from time to time, like checking for timed out clients.
2. `beforeSleep()` is called every time the event loop fired, Redis served a few requests, and is returning back into the event loop.

Inside server.c you can find code that handles other vital things of the Redis server:

* `call()` is used in order to call a given command in the context of a given client.
* `activeExpireCycle()` handles eviction of keys with a time to live set via the `EXPIRE` command.
* `performEvictions()` is called when a new write command should be performed but Redis is out of memory according to the `maxmemory` directive.
* The global variable `redisCommandTable` defines all the Redis commands, specifying the name of the command, the function implementing the command, the number of arguments required, and other properties of each command.

networking.c
---

This file defines all the I/O functions with clients, masters and replicas
(which in Redis are just special clients):

* `createClient()` allocates and initializes a new client.
* the `addReply*()` family of functions are used by command implementations in order to append data to the client structure, that will be transmitted to the client as a reply for a given command executed.
* `writeToClient()` transmits the data pending in the output buffers to the client and is called by the *writable event handler* `sendReplyToClient()`.
* `readQueryFromClient()` is the *readable event handler* and accumulates data read from the client into the query buffer.
* `processInputBuffer()` is the entry point in order to parse the client query buffer according to the Redis protocol. Once commands are ready to be processed, it calls `processCommand()` which is defined inside `server.c` in order to actually execute the command.
* `freeClient()` deallocates, disconnects and removes a client.

aof.c and rdb.c
---

As you can guess from the names, these files implement the RDB and AOF
persistence for Redis. Redis uses a persistence model based on the `fork()`
system call in order to create a thread with the same (shared) memory
content of the main Redis thread. This secondary thread dumps the content
of the memory on disk. This is used by `rdb.c` to create the snapshots
on disk and by `aof.c` in order to perform the AOF rewrite when the
append only file gets too big.

The implementation inside `aof.c` has additional functions in order to
implement an API that allows commands to append new commands into the AOF
file as clients execute them.

The `call()` function defined inside `server.c` is responsible for calling
the functions that in turn will write the commands into the AOF.

db.c
---

Certain Redis commands operate on specific data types; others are general.
Examples of generic commands are `DEL` and `EXPIRE`. They operate on keys
and not on their values specifically. All those generic commands are
defined inside `db.c`.

Moreover `db.c` implements an API in order to perform certain operations
on the Redis dataset without directly accessing the internal data structures.

The most important functions inside `db.c` which are used in many command
implementations are the following:

* `lookupKeyRead()` and `lookupKeyWrite()` are used in order to get a pointer to the value associated to a given key, or `NULL` if the key does not exist.
* `dbAdd()` and its higher level counterpart `setKey()` create a new key in a Redis database.
* `dbDelete()` removes a key and its associated value.
* `emptyDb()` removes an entire single database or all the databases defined.

The rest of the file implements the generic commands exposed to the client.

object.c
---

The `robj` structure defining Redis objects was already described. Inside
`object.c` there are all the functions that operate with Redis objects at
a basic level, like functions to allocate new objects, handle the reference
counting and so forth. Notable functions inside this file:

* `incrRefCount()` and `decrRefCount()` are used in order to increment or decrement an object reference count. When it drops to 0 the object is finally freed.
* `createObject()` allocates a new object. There are also specialized functions to allocate string objects having a specific content, like `createStringObjectFromLongLong()` and similar functions.

This file also implements the `OBJECT` command.

replication.c
---

This is one of the most complex files inside Redis, it is recommended to
approach it only after getting a bit familiar with the rest of the code base.
In this file there is the implementation of both the master and replica role
of Redis.

One of the most important functions inside this file is `replicationFeedSlaves()` that writes commands to the clients representing replica instances connected
to our master, so that the replicas can get the writes performed by the clients:
this way their data set will remain synchronized with the one in the master.

This file also implements both the `SYNC` and `PSYNC` commands that are
used in order to perform the first synchronization between masters and
replicas, or to continue the replication after a disconnection.

Other C files
---

* `t_hash.c`, `t_list.c`, `t_set.c`, `t_string.c`, `t_zset.c` and `t_stream.c` contains the implementation of the Redis data types. They implement both an API to access a given data type, and the client command implementations for these data types.
* `ae.c` implements the Redis event loop, it's a self contained library which is simple to read and understand.
* `sds.c` is the Redis string library, check http://github.com/antirez/sds for more information.
* `anet.c` is a library to use POSIX networking in a simpler way compared to the raw interface exposed by the kernel.
* `dict.c` is an implementation of a non-blocking hash table which rehashes incrementally.
* `scripting.c` implements Lua scripting. It is completely self-contained and isolated from the rest of the Redis implementation and is simple enough to understand if you are familiar with the Lua API.
* `cluster.c` implements the Redis Cluster. Probably a good read only after being very familiar with the rest of the Redis code base. If you want to read `cluster.c` make sure to read the [Redis Cluster specification][3].

[3]: https://redis.io/topics/cluster-spec

Anatomy of a Redis command
---

All the Redis commands are defined in the following way:

    void foobarCommand(client *c) {
        printf("%s",c->argv[1]->ptr); /* Do something with the argument. */
        addReply(c,shared.ok); /* Reply something to the client. */
    }

The command is then referenced inside `server.c` in the command table:

    {"foobar",foobarCommand,2,"rtF",0,NULL,0,0,0,0,0},

In the above example `2` is the number of arguments the command takes,
while `"rtF"` are the command flags, as documented in the command table
top comment inside `server.c`.

After the command operates in some way, it returns a reply to the client,
usually using `addReply()` or a similar function defined inside `networking.c`.

There are tons of command implementations inside the Redis source code
that can serve as examples of actual commands implementations. Writing
a few toy commands can be a good exercise to get familiar with the code base.

There are also many other files not described here, but it is useless to
cover everything. We just want to help you with the first steps.
Eventually you'll find your way inside the Redis code base :-)

Enjoy!
