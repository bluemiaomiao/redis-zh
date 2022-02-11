> 本文件在 "redis-zh" 项目中支持中文, 请看 "README_zh_CN.md" 文件！

# Redis 源码介绍

## 依赖包

```
.
├── deps
│   ├── hdr_histogram
│   │   ├── hdr_atomic.h
│   │   ├── hdr_histogram.c
│   │   └── hdr_histogram.h
│   ├── hiredis
│   │   ├── adapters
│   │   │   ├── ae.h
│   │   │   ├── glib.h
│   │   │   ├── ivykis.h
│   │   │   ├── libev.h
│   │   │   ├── libevent.h
│   │   │   ├── libuv.h
│   │   │   ├── macosx.h
│   │   │   └── qt.h
│   │   ├── alloc.c
│   │   ├── alloc.h
│   │   ├── appveyor.yml
│   │   ├── async.c
│   │   ├── async.h
│   │   ├── async_private.h
│   │   ├── dict.c
│   │   ├── dict.h
│   │   ├── examples
│   │   │   ├── example-ae.c
│   │   │   ├── example-glib.c
│   │   │   ├── example-ivykis.c
│   │   │   ├── example-libev.c
│   │   │   ├── example-libevent-ssl.c
│   │   │   ├── example-libevent.c
│   │   │   ├── example-libuv.c
│   │   │   ├── example-macosx.c
│   │   │   ├── example-push.c
│   │   │   ├── example-qt.cpp
│   │   │   ├── example-qt.h
│   │   │   ├── example-ssl.c
│   │   │   └── example.c
│   │   ├── fmacros.h
│   │   ├── hiredis-config.cmake.in
│   │   ├── hiredis.c
│   │   ├── hiredis.h
│   │   ├── hiredis.pc.in
│   │   ├── hiredis_ssl-config.cmake.in
│   │   ├── hiredis_ssl.h
│   │   ├── hiredis_ssl.pc.in
│   │   ├── net.c
│   │   ├── net.h
│   │   ├── read.c
│   │   ├── read.h
│   │   ├── sds.c
│   │   ├── sds.h
│   │   ├── sdsalloc.h
│   │   ├── sdscompat.h
│   │   ├── sockcompat.c
│   │   ├── sockcompat.h
│   │   ├── ssl.c
│   │   ├── test.c
│   │   ├── test.sh
│   │   └── win32.h
│   ├── jemalloc
│   │   ├── autogen.sh
│   │   ├── bin
│   │   │   ├── jemalloc-config.in
│   │   │   ├── jemalloc.sh.in
│   │   │   └── jeprof.in
│   │   ├── build-aux
│   │   │   ├── config.guess
│   │   │   ├── config.sub
│   │   │   └── install-sh
│   │   ├── config.stamp.in
│   │   ├── configure
│   │   ├── configure.ac
│   │   ├── doc
│   │   │   ├── html.xsl.in
│   │   │   ├── jemalloc.xml.in
│   │   │   ├── manpages.xsl.in
│   │   │   └── stylesheet.xsl
│   │   ├── include
│   │   │   ├── jemalloc
│   │   │   │   ├── internal
│   │   │   │   │   ├── arena_externs.h
│   │   │   │   │   ├── arena_inlines_a.h
│   │   │   │   │   ├── arena_inlines_b.h
│   │   │   │   │   ├── arena_stats.h
│   │   │   │   │   ├── arena_structs_a.h
│   │   │   │   │   ├── arena_structs_b.h
│   │   │   │   │   ├── arena_types.h
│   │   │   │   │   ├── assert.h
│   │   │   │   │   ├── atomic.h
│   │   │   │   │   ├── atomic_c11.h
│   │   │   │   │   ├── atomic_gcc_atomic.h
│   │   │   │   │   ├── atomic_gcc_sync.h
│   │   │   │   │   ├── atomic_msvc.h
│   │   │   │   │   ├── background_thread_externs.h
│   │   │   │   │   ├── background_thread_inlines.h
│   │   │   │   │   ├── background_thread_structs.h
│   │   │   │   │   ├── base_externs.h
│   │   │   │   │   ├── base_inlines.h
│   │   │   │   │   ├── base_structs.h
│   │   │   │   │   ├── base_types.h
│   │   │   │   │   ├── bin.h
│   │   │   │   │   ├── bin_stats.h
│   │   │   │   │   ├── bit_util.h
│   │   │   │   │   ├── bitmap.h
│   │   │   │   │   ├── cache_bin.h
│   │   │   │   │   ├── ckh.h
│   │   │   │   │   ├── ctl.h
│   │   │   │   │   ├── div.h
│   │   │   │   │   ├── emitter.h
│   │   │   │   │   ├── extent_dss.h
│   │   │   │   │   ├── extent_externs.h
│   │   │   │   │   ├── extent_inlines.h
│   │   │   │   │   ├── extent_mmap.h
│   │   │   │   │   ├── extent_structs.h
│   │   │   │   │   ├── extent_types.h
│   │   │   │   │   ├── hash.h
│   │   │   │   │   ├── hooks.h
│   │   │   │   │   ├── jemalloc_internal_decls.h
│   │   │   │   │   ├── jemalloc_internal_defs.h.in
│   │   │   │   │   ├── jemalloc_internal_externs.h
│   │   │   │   │   ├── jemalloc_internal_includes.h
│   │   │   │   │   ├── jemalloc_internal_inlines_a.h
│   │   │   │   │   ├── jemalloc_internal_inlines_b.h
│   │   │   │   │   ├── jemalloc_internal_inlines_c.h
│   │   │   │   │   ├── jemalloc_internal_macros.h
│   │   │   │   │   ├── jemalloc_internal_types.h
│   │   │   │   │   ├── jemalloc_preamble.h.in
│   │   │   │   │   ├── large_externs.h
│   │   │   │   │   ├── log.h
│   │   │   │   │   ├── malloc_io.h
│   │   │   │   │   ├── mutex.h
│   │   │   │   │   ├── mutex_pool.h
│   │   │   │   │   ├── mutex_prof.h
│   │   │   │   │   ├── nstime.h
│   │   │   │   │   ├── pages.h
│   │   │   │   │   ├── ph.h
│   │   │   │   │   ├── private_namespace.sh
│   │   │   │   │   ├── private_symbols.sh
│   │   │   │   │   ├── prng.h
│   │   │   │   │   ├── prof_externs.h
│   │   │   │   │   ├── prof_inlines_a.h
│   │   │   │   │   ├── prof_inlines_b.h
│   │   │   │   │   ├── prof_structs.h
│   │   │   │   │   ├── prof_types.h
│   │   │   │   │   ├── public_namespace.sh
│   │   │   │   │   ├── public_unnamespace.sh
│   │   │   │   │   ├── ql.h
│   │   │   │   │   ├── qr.h
│   │   │   │   │   ├── rb.h
│   │   │   │   │   ├── rtree.h
│   │   │   │   │   ├── rtree_tsd.h
│   │   │   │   │   ├── size_classes.sh
│   │   │   │   │   ├── smoothstep.h
│   │   │   │   │   ├── smoothstep.sh
│   │   │   │   │   ├── spin.h
│   │   │   │   │   ├── stats.h
│   │   │   │   │   ├── sz.h
│   │   │   │   │   ├── tcache_externs.h
│   │   │   │   │   ├── tcache_inlines.h
│   │   │   │   │   ├── tcache_structs.h
│   │   │   │   │   ├── tcache_types.h
│   │   │   │   │   ├── ticker.h
│   │   │   │   │   ├── tsd.h
│   │   │   │   │   ├── tsd_generic.h
│   │   │   │   │   ├── tsd_malloc_thread_cleanup.h
│   │   │   │   │   ├── tsd_tls.h
│   │   │   │   │   ├── tsd_types.h
│   │   │   │   │   ├── tsd_win.h
│   │   │   │   │   ├── util.h
│   │   │   │   │   └── witness.h
│   │   │   │   ├── jemalloc.sh
│   │   │   │   ├── jemalloc_defs.h.in
│   │   │   │   ├── jemalloc_macros.h.in
│   │   │   │   ├── jemalloc_mangle.sh
│   │   │   │   ├── jemalloc_protos.h.in
│   │   │   │   ├── jemalloc_rename.sh
│   │   │   │   └── jemalloc_typedefs.h.in
│   │   │   └── msvc_compat
│   │   │       ├── C99
│   │   │       │   ├── stdbool.h
│   │   │       │   └── stdint.h
│   │   │       ├── strings.h
│   │   │       └── windows_extra.h
│   │   ├── jemalloc.pc.in
│   │   ├── m4
│   │   │   └── ax_cxx_compile_stdcxx.m4
│   │   ├── msvc
│   │   │   ├── ReadMe.txt
│   │   │   ├── jemalloc_vc2015.sln
│   │   │   ├── jemalloc_vc2017.sln
│   │   │   ├── projects
│   │   │   │   ├── vc2015
│   │   │   │   │   ├── jemalloc
│   │   │   │   │   │   ├── jemalloc.vcxproj
│   │   │   │   │   │   └── jemalloc.vcxproj.filters
│   │   │   │   │   └── test_threads
│   │   │   │   │       ├── test_threads.vcxproj
│   │   │   │   │       └── test_threads.vcxproj.filters
│   │   │   │   └── vc2017
│   │   │   │       ├── jemalloc
│   │   │   │       │   ├── jemalloc.vcxproj
│   │   │   │       │   └── jemalloc.vcxproj.filters
│   │   │   │       └── test_threads
│   │   │   │           ├── test_threads.vcxproj
│   │   │   │           └── test_threads.vcxproj.filters
│   │   │   └── test_threads
│   │   │       ├── test_threads.cpp
│   │   │       ├── test_threads.h
│   │   │       └── test_threads_main.cpp
│   │   ├── run_tests.sh
│   │   ├── scripts
│   │   │   ├── gen_run_tests.py
│   │   │   └── gen_travis.py
│   │   ├── src
│   │   │   ├── arena.c
│   │   │   ├── background_thread.c
│   │   │   ├── base.c
│   │   │   ├── bin.c
│   │   │   ├── bitmap.c
│   │   │   ├── ckh.c
│   │   │   ├── ctl.c
│   │   │   ├── div.c
│   │   │   ├── extent.c
│   │   │   ├── extent_dss.c
│   │   │   ├── extent_mmap.c
│   │   │   ├── hash.c
│   │   │   ├── hooks.c
│   │   │   ├── jemalloc.c
│   │   │   ├── jemalloc_cpp.cpp
│   │   │   ├── large.c
│   │   │   ├── log.c
│   │   │   ├── malloc_io.c
│   │   │   ├── mutex.c
│   │   │   ├── mutex_pool.c
│   │   │   ├── nstime.c
│   │   │   ├── pages.c
│   │   │   ├── prng.c
│   │   │   ├── prof.c
│   │   │   ├── rtree.c
│   │   │   ├── stats.c
│   │   │   ├── sz.c
│   │   │   ├── tcache.c
│   │   │   ├── ticker.c
│   │   │   ├── tsd.c
│   │   │   ├── witness.c
│   │   │   └── zone.c
│   │   └── test
│   │       ├── include
│   │       │   └── test
│   │       │       ├── SFMT-alti.h
│   │       │       ├── SFMT-params.h
│   │       │       ├── SFMT-params11213.h
│   │       │       ├── SFMT-params1279.h
│   │       │       ├── SFMT-params132049.h
│   │       │       ├── SFMT-params19937.h
│   │       │       ├── SFMT-params216091.h
│   │       │       ├── SFMT-params2281.h
│   │       │       ├── SFMT-params4253.h
│   │       │       ├── SFMT-params44497.h
│   │       │       ├── SFMT-params607.h
│   │       │       ├── SFMT-params86243.h
│   │       │       ├── SFMT-sse2.h
│   │       │       ├── SFMT.h
│   │       │       ├── btalloc.h
│   │       │       ├── extent_hooks.h
│   │       │       ├── jemalloc_test.h.in
│   │       │       ├── jemalloc_test_defs.h.in
│   │       │       ├── math.h
│   │       │       ├── mq.h
│   │       │       ├── mtx.h
│   │       │       ├── test.h
│   │       │       ├── thd.h
│   │       │       └── timer.h
│   │       ├── integration
│   │       │   ├── MALLOCX_ARENA.c
│   │       │   ├── aligned_alloc.c
│   │       │   ├── allocated.c
│   │       │   ├── cpp
│   │       │   │   └── basic.cpp
│   │       │   ├── extent.c
│   │       │   ├── extent.sh
│   │       │   ├── mallocx.c
│   │       │   ├── mallocx.sh
│   │       │   ├── overflow.c
│   │       │   ├── posix_memalign.c
│   │       │   ├── rallocx.c
│   │       │   ├── sdallocx.c
│   │       │   ├── thread_arena.c
│   │       │   ├── thread_tcache_enabled.c
│   │       │   ├── xallocx.c
│   │       │   └── xallocx.sh
│   │       ├── src
│   │       │   ├── SFMT.c
│   │       │   ├── btalloc.c
│   │       │   ├── btalloc_0.c
│   │       │   ├── btalloc_1.c
│   │       │   ├── math.c
│   │       │   ├── mq.c
│   │       │   ├── mtx.c
│   │       │   ├── test.c
│   │       │   ├── thd.c
│   │       │   └── timer.c
│   │       ├── stress
│   │       │   └── microbench.c
│   │       ├── test.sh.in
│   │       └── unit
│   │           ├── SFMT.c
│   │           ├── a0.c
│   │           ├── arena_reset.c
│   │           ├── arena_reset_prof.c
│   │           ├── arena_reset_prof.sh
│   │           ├── atomic.c
│   │           ├── background_thread.c
│   │           ├── background_thread_enable.c
│   │           ├── base.c
│   │           ├── bit_util.c
│   │           ├── bitmap.c
│   │           ├── ckh.c
│   │           ├── decay.c
│   │           ├── decay.sh
│   │           ├── div.c
│   │           ├── emitter.c
│   │           ├── extent_quantize.c
│   │           ├── fork.c
│   │           ├── hash.c
│   │           ├── hooks.c
│   │           ├── junk.c
│   │           ├── junk.sh
│   │           ├── junk_alloc.c
│   │           ├── junk_alloc.sh
│   │           ├── junk_free.c
│   │           ├── junk_free.sh
│   │           ├── log.c
│   │           ├── mallctl.c
│   │           ├── malloc_io.c
│   │           ├── math.c
│   │           ├── mq.c
│   │           ├── mtx.c
│   │           ├── nstime.c
│   │           ├── pack.c
│   │           ├── pack.sh
│   │           ├── pages.c
│   │           ├── ph.c
│   │           ├── prng.c
│   │           ├── prof_accum.c
│   │           ├── prof_accum.sh
│   │           ├── prof_active.c
│   │           ├── prof_active.sh
│   │           ├── prof_gdump.c
│   │           ├── prof_gdump.sh
│   │           ├── prof_idump.c
│   │           ├── prof_idump.sh
│   │           ├── prof_reset.c
│   │           ├── prof_reset.sh
│   │           ├── prof_tctx.c
│   │           ├── prof_tctx.sh
│   │           ├── prof_thread_name.c
│   │           ├── prof_thread_name.sh
│   │           ├── ql.c
│   │           ├── qr.c
│   │           ├── rb.c
│   │           ├── retained.c
│   │           ├── rtree.c
│   │           ├── size_classes.c
│   │           ├── slab.c
│   │           ├── smoothstep.c
│   │           ├── spin.c
│   │           ├── stats.c
│   │           ├── stats_print.c
│   │           ├── ticker.c
│   │           ├── tsd.c
│   │           ├── witness.c
│   │           ├── zero.c
│   │           └── zero.sh
│   ├── linenoise
│   │   ├── example.c
│   │   ├── linenoise.c
│   │   └── linenoise.h
│   ├── lua
│   │   ├── etc
│   │   │   ├── all.c
│   │   │   ├── lua.hpp
│   │   │   ├── lua.ico
│   │   │   ├── lua.pc
│   │   │   ├── luavs.bat
│   │   │   ├── min.c
│   │   │   ├── noparser.c
│   │   │   └── strict.lua
│   │   ├── src
│   │   │   ├── fpconv.c
│   │   │   ├── fpconv.h
│   │   │   ├── lapi.c
│   │   │   ├── lapi.h
│   │   │   ├── lauxlib.c
│   │   │   ├── lauxlib.h
│   │   │   ├── lbaselib.c
│   │   │   ├── lcode.c
│   │   │   ├── lcode.h
│   │   │   ├── ldblib.c
│   │   │   ├── ldebug.c
│   │   │   ├── ldebug.h
│   │   │   ├── ldo.c
│   │   │   ├── ldo.h
│   │   │   ├── ldump.c
│   │   │   ├── lfunc.c
│   │   │   ├── lfunc.h
│   │   │   ├── lgc.c
│   │   │   ├── lgc.h
│   │   │   ├── linit.c
│   │   │   ├── liolib.c
│   │   │   ├── llex.c
│   │   │   ├── llex.h
│   │   │   ├── llimits.h
│   │   │   ├── lmathlib.c
│   │   │   ├── lmem.c
│   │   │   ├── lmem.h
│   │   │   ├── loadlib.c
│   │   │   ├── lobject.c
│   │   │   ├── lobject.h
│   │   │   ├── lopcodes.c
│   │   │   ├── lopcodes.h
│   │   │   ├── loslib.c
│   │   │   ├── lparser.c
│   │   │   ├── lparser.h
│   │   │   ├── lstate.c
│   │   │   ├── lstate.h
│   │   │   ├── lstring.c
│   │   │   ├── lstring.h
│   │   │   ├── lstrlib.c
│   │   │   ├── ltable.c
│   │   │   ├── ltable.h
│   │   │   ├── ltablib.c
│   │   │   ├── ltm.c
│   │   │   ├── ltm.h
│   │   │   ├── lua.c
│   │   │   ├── lua.h
│   │   │   ├── lua_bit.c
│   │   │   ├── lua_cjson.c
│   │   │   ├── lua_cmsgpack.c
│   │   │   ├── lua_struct.c
│   │   │   ├── luac.c
│   │   │   ├── luaconf.h
│   │   │   ├── lualib.h
│   │   │   ├── lundump.c
│   │   │   ├── lundump.h
│   │   │   ├── lvm.c
│   │   │   ├── lvm.h
│   │   │   ├── lzio.c
│   │   │   ├── lzio.h
│   │   │   ├── print.c
│   │   │   ├── strbuf.c
│   │   │   └── strbuf.h
│   │   └── test
│   │       ├── bisect.lua
│   │       ├── cf.lua
│   │       ├── echo.lua
│   │       ├── env.lua
│   │       ├── factorial.lua
│   │       ├── fib.lua
│   │       ├── fibfor.lua
│   │       ├── globals.lua
│   │       ├── hello.lua
│   │       ├── life.lua
│   │       ├── luac.lua
│   │       ├── printf.lua
│   │       ├── readonly.lua
│   │       ├── sieve.lua
│   │       ├── sort.lua
│   │       ├── table.lua
│   │       ├── trace-calls.lua
│   │       ├── trace-globals.lua
│   │       └── xd.lua
│   └── update-jemalloc.sh
```

## 主要实现

```
├── src
│   ├── acl.c
│   ├── adlist.c
│   ├── adlist.h
│   ├── ae.c
│   ├── ae.h
│   ├── ae_epoll.c
│   ├── ae_evport.c
│   ├── ae_kqueue.c
│   ├── ae_select.c
│   ├── anet.c
│   ├── anet.h
│   ├── aof.c
│   ├── asciilogo.h
│   ├── atomicvar.h
│   ├── bio.c
│   ├── bio.h
│   ├── bitops.c
│   ├── blocked.c
│   ├── childinfo.c
│   ├── cli_common.c
│   ├── cli_common.h
│   ├── cluster.c
│   ├── cluster.h
│   ├── config.c
│   ├── config.h
│   ├── connection.c
│   ├── connection.h
│   ├── connhelpers.h
│   ├── crc16.c
│   ├── crc16_slottable.h
│   ├── crc64.c
│   ├── crc64.h
│   ├── crcspeed.c
│   ├── crcspeed.h
│   ├── db.c
│   ├── debug.c
│   ├── debugmacro.h
│   ├── defrag.c
│   ├── dict.c
│   ├── dict.h
│   ├── endianconv.c
│   ├── endianconv.h
│   ├── evict.c
│   ├── expire.c
│   ├── fmacros.h
│   ├── geo.c
│   ├── geo.h
│   ├── geohash.c
│   ├── geohash.h
│   ├── geohash_helper.c
│   ├── geohash_helper.h
│   ├── gopher.c
│   ├── help.h
│   ├── hyperloglog.c
│   ├── intset.c
│   ├── intset.h
│   ├── latency.c
│   ├── latency.h
│   ├── lazyfree.c
│   ├── listpack.c
│   ├── listpack.h
│   ├── listpack_malloc.h
│   ├── localtime.c
│   ├── lolwut.c
│   ├── lolwut.h
│   ├── lolwut5.c
│   ├── lolwut6.c
│   ├── lzf.h
│   ├── lzfP.h
│   ├── lzf_c.c
│   ├── lzf_d.c
│   ├── memtest.c
│   ├── mkreleasehdr.sh
│   ├── module.c
│   ├── modules
│   │   ├── Makefile
│   │   ├── gendoc.rb
│   │   ├── helloacl.c
│   │   ├── helloblock.c
│   │   ├── hellocluster.c
│   │   ├── hellodict.c
│   │   ├── hellohook.c
│   │   ├── hellotimer.c
│   │   ├── hellotype.c
│   │   └── helloworld.c
│   ├── monotonic.c
│   ├── monotonic.h
│   ├── mt19937-64.c
│   ├── mt19937-64.h
│   ├── multi.c
│   ├── networking.c
│   ├── notify.c
│   ├── object.c
│   ├── pqsort.c
│   ├── pqsort.h
│   ├── pubsub.c
│   ├── quicklist.c
│   ├── quicklist.h
│   ├── rand.c
│   ├── rand.h
│   ├── rax.c
│   ├── rax.h
│   ├── rax_malloc.h
│   ├── rdb.c
│   ├── rdb.h
│   ├── redis-benchmark.c
│   ├── redis-check-aof.c
│   ├── redis-check-rdb.c
│   ├── redis-cli.c
│   ├── redis-trib.rb
│   ├── redisassert.h
│   ├── redismodule.h
│   ├── release.c
│   ├── replication.c
│   ├── rio.c
│   ├── rio.h
│   ├── scripting.c
│   ├── sds.c
│   ├── sds.h
│   ├── sdsalloc.h
│   ├── sentinel.c
│   ├── server.c
│   ├── server.h
│   ├── setcpuaffinity.c
│   ├── setproctitle.c
│   ├── sha1.c
│   ├── sha1.h
│   ├── sha256.c
│   ├── sha256.h
│   ├── siphash.c
│   ├── slowlog.c
│   ├── slowlog.h
│   ├── solarisfixes.h
│   ├── sort.c
│   ├── sparkline.c
│   ├── sparkline.h
│   ├── stream.h
│   ├── syncio.c
│   ├── t_hash.c
│   ├── t_list.c
│   ├── t_set.c
│   ├── t_stream.c
│   ├── t_string.c
│   ├── t_zset.c
│   ├── testhelp.h
│   ├── timeout.c
│   ├── tls.c
│   ├── tracking.c
│   ├── util.c
│   ├── util.h
│   ├── valgrind.sup
│   ├── version.h
│   ├── ziplist.c
│   ├── ziplist.h
│   ├── zipmap.c
│   ├── zipmap.h
│   ├── zmalloc.c
│   └── zmalloc.h
```

## 测试代码

```shell
├── tests
│   ├── assets
│   │   ├── corrupt_empty_keys.rdb
│   │   ├── corrupt_ziplist.rdb
│   │   ├── default.conf
│   │   ├── encodings.rdb
│   │   ├── hash-zipmap.rdb
│   │   ├── minimal.conf
│   │   ├── nodefaultuser.acl
│   │   └── user.acl
│   ├── cluster
│   │   ├── cluster.tcl
│   │   ├── run.tcl
│   │   ├── tests
│   │   │   ├── 00-base.tcl
│   │   │   ├── 01-faildet.tcl
│   │   │   ├── 02-failover.tcl
│   │   │   ├── 03-failover-loop.tcl
│   │   │   ├── 04-resharding.tcl
│   │   │   ├── 05-slave-selection.tcl
│   │   │   ├── 06-slave-stop-cond.tcl
│   │   │   ├── 07-replica-migration.tcl
│   │   │   ├── 08-update-msg.tcl
│   │   │   ├── 09-pubsub.tcl
│   │   │   ├── 10-manual-failover.tcl
│   │   │   ├── 11-manual-takeover.tcl
│   │   │   ├── 12-replica-migration-2.tcl
│   │   │   ├── 12.1-replica-migration-3.tcl
│   │   │   ├── 13-no-failover-option.tcl
│   │   │   ├── 14-consistency-check.tcl
│   │   │   ├── 15-cluster-slots.tcl
│   │   │   ├── 16-transactions-on-replica.tcl
│   │   │   ├── 17-diskless-load-swapdb.tcl
│   │   │   ├── 18-info.tcl
│   │   │   ├── 19-cluster-nodes-slots.tcl
│   │   │   ├── 20-half-migrated-slot.tcl
│   │   │   ├── 21-many-slot-migration.tcl
│   │   │   ├── helpers
│   │   │   │   └── onlydots.tcl
│   │   │   └── includes
│   │   │       ├── init-tests.tcl
│   │   │       └── utils.tcl
│   │   └── tmp
│   ├── helpers
│   │   ├── bg_block_op.tcl
│   │   ├── bg_complex_data.tcl
│   │   ├── fake_redis_node.tcl
│   │   └── gen_write_load.tcl
│   ├── instances.tcl
│   ├── integration
│   │   ├── aof-race.tcl
│   │   ├── aof.tcl
│   │   ├── block-repl.tcl
│   │   ├── convert-zipmap-hash-on-load.tcl
│   │   ├── corrupt-dump-fuzzer.tcl
│   │   ├── corrupt-dump.tcl
│   │   ├── failover.tcl
│   │   ├── logging.tcl
│   │   ├── psync2-pingoff.tcl
│   │   ├── psync2-reg.tcl
│   │   ├── psync2.tcl
│   │   ├── rdb.tcl
│   │   ├── redis-benchmark.tcl
│   │   ├── redis-cli.tcl
│   │   ├── replication-2.tcl
│   │   ├── replication-3.tcl
│   │   ├── replication-4.tcl
│   │   ├── replication-psync.tcl
│   │   └── replication.tcl
│   ├── modules
│   │   ├── Makefile
│   │   ├── auth.c
│   │   ├── basics.c
│   │   ├── blockedclient.c
│   │   ├── blockonbackground.c
│   │   ├── blockonkeys.c
│   │   ├── commandfilter.c
│   │   ├── datatype.c
│   │   ├── defragtest.c
│   │   ├── fork.c
│   │   ├── getkeys.c
│   │   ├── hash.c
│   │   ├── hooks.c
│   │   ├── infotest.c
│   │   ├── keyspace_events.c
│   │   ├── misc.c
│   │   ├── propagate.c
│   │   ├── scan.c
│   │   ├── stream.c
│   │   ├── test_lazyfree.c
│   │   ├── testrdb.c
│   │   ├── timer.c
│   │   └── zset.c
│   ├── sentinel
│   │   ├── run.tcl
│   │   ├── tests
│   │   │   ├── 00-base.tcl
│   │   │   ├── 01-conf-update.tcl
│   │   │   ├── 02-slaves-reconf.tcl
│   │   │   ├── 03-runtime-reconf.tcl
│   │   │   ├── 04-slave-selection.tcl
│   │   │   ├── 05-manual.tcl
│   │   │   ├── 06-ckquorum.tcl
│   │   │   ├── 07-down-conditions.tcl
│   │   │   ├── 08-hostname-conf.tcl
│   │   │   ├── 09-acl-support.tcl
│   │   │   ├── 10-replica-priority.tcl
│   │   │   ├── helpers
│   │   │   │   └── check_leaked_fds.tcl
│   │   │   └── includes
│   │   │       ├── init-tests.tcl
│   │   │       ├── sentinel.conf
│   │   │       └── start-init-tests.tcl
│   │   └── tmp
│   ├── support
│   │   ├── benchmark.tcl
│   │   ├── cli.tcl
│   │   ├── cluster.tcl
│   │   ├── redis.tcl
│   │   ├── server.tcl
│   │   ├── test.tcl
│   │   ├── tmpfile.tcl
│   │   └── util.tcl
│   ├── test_helper.tcl
│   ├── tmp
│   └── unit
│       ├── acl.tcl
│       ├── aofrw.tcl
│       ├── auth.tcl
│       ├── bitfield.tcl
│       ├── bitops.tcl
│       ├── dump.tcl
│       ├── expire.tcl
│       ├── geo.tcl
│       ├── hyperloglog.tcl
│       ├── info.tcl
│       ├── introspection-2.tcl
│       ├── introspection.tcl
│       ├── keyspace.tcl
│       ├── latency-monitor.tcl
│       ├── lazyfree.tcl
│       ├── limits.tcl
│       ├── maxmemory.tcl
│       ├── memefficiency.tcl
│       ├── moduleapi
│       │   ├── auth.tcl
│       │   ├── basics.tcl
│       │   ├── blockedclient.tcl
│       │   ├── blockonbackground.tcl
│       │   ├── blockonkeys.tcl
│       │   ├── commandfilter.tcl
│       │   ├── datatype.tcl
│       │   ├── defrag.tcl
│       │   ├── fork.tcl
│       │   ├── getkeys.tcl
│       │   ├── hash.tcl
│       │   ├── hooks.tcl
│       │   ├── infotest.tcl
│       │   ├── keyspace_events.tcl
│       │   ├── misc.tcl
│       │   ├── propagate.tcl
│       │   ├── scan.tcl
│       │   ├── stream.tcl
│       │   ├── test_lazyfree.tcl
│       │   ├── testrdb.tcl
│       │   ├── timer.tcl
│       │   └── zset.tcl
│       ├── multi.tcl
│       ├── networking.tcl
│       ├── obuf-limits.tcl
│       ├── oom-score-adj.tcl
│       ├── other.tcl
│       ├── pause.tcl
│       ├── pendingquerybuf.tcl
│       ├── printver.tcl
│       ├── protocol.tcl
│       ├── pubsub.tcl
│       ├── quit.tcl
│       ├── scan.tcl
│       ├── scripting.tcl
│       ├── shutdown.tcl
│       ├── slowlog.tcl
│       ├── sort.tcl
│       ├── tls.tcl
│       ├── tracking.tcl
│       ├── type
│       │   ├── hash.tcl
│       │   ├── incr.tcl
│       │   ├── list-2.tcl
│       │   ├── list-3.tcl
│       │   ├── list-common.tcl
│       │   ├── list.tcl
│       │   ├── set.tcl
│       │   ├── stream-cgroups.tcl
│       │   ├── stream.tcl
│       │   ├── string.tcl
│       │   └── zset.tcl
│       ├── violations.tcl
│       └── wait.tcl
```

## 工具包

```

├── utils
│   ├── build-static-symbols.tcl
│   ├── cluster_fail_time.tcl
│   ├── corrupt_rdb.c
│   ├── create-cluster
│   │   └── create-cluster
│   ├── gen-test-certs.sh
│   ├── generate-command-help.rb
│   ├── graphs
│   │   └── commits-over-time
│   │       └── genhtml.tcl
│   ├── hashtable
│   │   └── rehashing.c
│   ├── hyperloglog
│   │   ├── hll-err.rb
│   │   └── hll-gnuplot-graph.rb
│   ├── install_server.sh
│   ├── lru
│   │   ├── lfu-simulation.c
│   │   └── test-lru.rb
│   ├── redis-copy.rb
│   ├── redis-sha1.rb
│   ├── redis_init_script
│   ├── redis_init_script.tpl
│   ├── releasetools
│   │   ├── 01_create_tarball.sh
│   │   ├── 02_upload_tarball.sh
│   │   ├── 03_test_release.sh
│   │   ├── 04_release_hash.sh
│   │   └── changelog.tcl
│   ├── speed-regression.tcl
│   ├── srandmember
│   │   ├── showdist.rb
│   │   └── showfreq.rb
│   ├── systemd-redis_multiple_servers@.service
│   ├── systemd-redis_server.service
│   ├── tracking_collisions.c
│   └── whatisdoing.sh

```

## 其他文件

```
├── redis.conf
├── runtest
├── runtest-cluster
├── runtest-moduleapi
├── runtest-sentinel
├── sentinel.conf
└── version.txt
```