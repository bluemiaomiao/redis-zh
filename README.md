> 本文件在 "redis-zh" 项目中支持中文, 请看 "README_zh_CN.md" 文件！

# Redis 源码介绍

基于 `redis-zh/version.txt` 版本分析。

## 依赖库

依赖库目录位于 `redis-zh/deps`，有以下几个依赖库：

* hdr_histogram
* hiredis
* jemalloc
* linenoise
* lua

## 测试工具

测试工具目录位于 `redis-zh/tests` 下：

* assets
* cluster
* integration
* modules
* sentinel
* support
* unit

## 工具包

工具包目录位于 `redis-zh/utils` 下：

* build-static-symbols.tcl
* cluster_fail_time.tcl
* create-cluster
* gen-test-certs.sh
* generate-command-help.rb
* graphs
* hashtable
* hyperloglog
* install_server.sh
* lru
* redis-copy.rb
* redis-sha1.rb
* redis_init_script
* redis_init_script.tpl
* releasetools
* systemd-redis_multiple_server@service
* systemd-redis_server.service
* tracking_collisions.c
* whatisdoing.sh

## 主要实现

主要实现的代码目录就是 `redis-zh/src` 下面所有的代码：

* src/Makefile
* src/acl.c
* src/adlist.c
* src/adlist.h
* src/ae.c: 事件驱动的底层实现
* src/ae.h
* src/ae_epoll.c: 事件驱动的底层实现
* src/ae_evport.c: 事件驱动的底层实现
* src/ae_kqueue.c: 事件驱动的底层实现
* src/ae_select.c: 事件驱动的底层实现
* src/anet.c
* src/anet.h: 网络连接的底层实现
* src/aof.c: AOF持久化的底层实现
* src/asciilogo.h
* src/atomicvar.h
* src/bio.c
* src/bio.h
* src/bitops.c
* src/blocked.c
* src/childinfo.c
* src/cli_common.c
* src/cli_common.h
* src/cluster.c: 集群的实现
* src/cluster.h
* src/config.c
* src/config.h
* src/connection.c
* src/connection.h
* src/connhelpers.h
* src/crc16.c
* src/crc16_slottable.h
* src/crc64.c
* src/crc64.h
* src/crcspeed.c
* src/crcspeed.h
* src/db.c: 数据库的实现
* src/debug.c
* src/debugmacro.h
* src/defrag.c
* src/dict.c: 字典的实现
* src/dict.h
* src/endianconv.c
* src/endianconv.h
* src/evict.c
* src/expire.c
* src/fmacros.h
* src/geo.c: Geo的实现
* src/geo.h
* src/geohash.c
* src/geohash.h
* src/geohash_helper.c
* src/geohash_helper.h
* src/gopher.c
* src/help.h
* src/hyperloglog.c: HyperLogLog的实现
* src/intset.c: 整数集合的实现
* src/intset.h
* src/latency.c
* src/latency.h
* src/lazyfree.c
* src/listpack.c
* src/listpack.h: Streams的底层结构
* src/listpack_malloc.h
* src/localtime.c
* src/lolwut.c
* src/lolwut.h
* src/lolwut5.c
* src/lolwut6.c
* src/lzf.h
* src/lzfP.h
* src/lzf_c.c
* src/lzf_d.c
* src/memtest.c
* src/mkreleasehdr.sh
* src/module.c
* src/modules
    * src/modules/Makefile
    * src/modules/gendoc.rb
    * src/modules/helloacl.c
    * src/modules/helloblock.c
    * src/modules/hellocluster.c
    * src/modules/hellodict.c
    * src/modules/hellohook.c
    * src/modules/hellotimer.c
    * src/modules/hellotype.c
    * src/modules/helloworld.c
* src/monotonic.c
* src/monotonic.h
* src/mt19937-64.c
* src/mt19937-64.h
* src/multi.c
* src/networking.c: 网络连接的底层实现
* src/notify.c
* src/object.c: Redis对象的实现
* src/pqsort.c
* src/pqsort.h
* src/pubsub.c: PubSub的实现
* src/quicklist.c: 快速链表的实现
* src/quicklist.h
* src/rand.c
* src/rand.h
* src/rax.c: Streams的底层结构
* src/rax.h
* src/rax_malloc.h
* src/rdb.c: RDB持久化的底层实现
* src/rdb.h
* src/redis-benchmark.c
* src/redis-check-aof.c
* src/redis-check-rdb.c
* src/redis-cli.c: 客户端的实现
* src/redis-trib.rb
* src/redisassert.h
* src/redismodule.h
* src/release.c
* src/replication.c: 主从复制的实现
* src/rio.c
* src/rio.h
* src/scripting.c
* src/sds.c: 动态字符串的实现
* src/sds.h
* src/sdsalloc.h
* src/sentinel.c: 哨兵的实现
* src/server.c: 服务器程序的实现
* src/server.h
* src/setcpuaffinity.c
* src/setproctitle.c
* src/sha1.c
* src/sha1.h
* src/sha256.c
* src/sha256.h
* src/siphash.c
* src/slowlog.c
* src/slowlog.h
* src/solarisfixes.h
* src/sort.c
* src/sparkline.c
* src/sparkline.h
* src/stream.h
* src/syncio.c
* src/t_hash.c: 字典的底层实现
* src/t_list.c: 列表的底层实现
* src/t_set.c: 集合的底层实现
* src/t_stream.c: Stream的底层实现
* src/t_string.c: 字符串的底层实现
* src/t_zset.c: 有序集合的底层实现
* src/testhelp.h
* src/timeout.c
* src/tls.c
* src/tracking.c
* src/util.c
* src/util.h
* src/valgrind.sup
* src/version.h
* src/ziplist.c: 压缩列表的实现
* src/ziplist.h
* src/zipmap.c
* src/zipmap.h
* src/zmalloc.c
* src/zmalloc.h
