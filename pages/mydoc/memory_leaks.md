---
title: Memory leaks
sidebar: mydoc_sidebar
permalink: memory_leaks.html
folder: mydoc
---

<br>

Memory leaks are checked using [Valgrind](https://valgrind.org/). To compile the executable, run `make valgrind`. The resulting executable will be available at `build/valgrind.out`.

To use valgrind to check for memory leaks, use the command below:

```sh
valgrind --tool=memcheck --leak-check=full --track-origins=yes --verbose --error-exitcode=1 ./build/valgrind.out
```

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseValgrind" aria-expanded="false" aria-controls="collapseValgrind">
    Example: Using Valgrind to check for memory leaks
  </button>
</p>
<div class="collapse" id="collapseValgrind">
  <div class="card card-body">
    <pre><code>==1670== Memcheck, a memory error detector
==1670== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==1670== Using Valgrind-3.15.0-608cb11914-20190413 and LibVEX; rerun with -h for copyright info
==1670== Command: ./build/valgrind.out
==1670== 
--1670-- Valgrind options:
--1670--    --tool=memcheck
--1670--    --leak-check=full
--1670--    --track-origins=yes
--1670--    --verbose
--1670--    --error-exitcode=1
--1670-- Contents of /proc/version:
--1670--   Linux version 4.15.0-1092-aws (buildd@lgw01-amd64-016) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #98-Ubuntu SMP Wed Jan 6 22:22:51 UTC 2021
--1670-- 
--1670-- Arch and hwcaps: AMD64, LittleEndian, amd64-cx16-lzcnt-rdtscp-sse3-ssse3-avx-avx2-bmi-f16c-rdrand
--1670-- Page sizes: currently 4096, max supported 4096
--1670-- Valgrind library directory: /usr/lib/x86_64-linux-gnu/valgrind
--1670-- Reading syms from /root/project/build/valgrind.out
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/ld-2.31.so
--1670--   Considering /usr/lib/x86_64-linux-gnu/ld-2.31.so ..
--1670--   .. CRC mismatch (computed 975d0390 wanted 30bd717f)
--1670--   Considering /lib/x86_64-linux-gnu/ld-2.31.so ..
--1670--   .. CRC mismatch (computed 975d0390 wanted 30bd717f)
--1670--   Considering /usr/lib/debug/lib/x86_64-linux-gnu/ld-2.31.so ..
--1670--   .. CRC is valid
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/valgrind/memcheck-amd64-linux
--1670--    object doesn't have a symbol table
--1670--    object doesn't have a dynamic symbol table
--1670-- Scheduler: using generic scheduler lock implementation.
--1670-- Reading suppressions file: /usr/lib/x86_64-linux-gnu/valgrind/default.supp
==1670== embedded gdbserver: reading from /tmp/vgdb-pipe-from-vgdb-to-1670-by-???-on-f1b806dae18f
==1670== embedded gdbserver: writing to   /tmp/vgdb-pipe-to-vgdb-from-1670-by-???-on-f1b806dae18f
==1670== embedded gdbserver: shared mem   /tmp/vgdb-pipe-shared-mem-vgdb-1670-by-???-on-f1b806dae18f
==1670== 
==1670== TO CONTROL THIS PROCESS USING vgdb (which you probably
==1670== don't want to do, unless you know exactly what you're doing,
==1670== or are doing some strange experiment):
==1670==   /usr/lib/x86_64-linux-gnu/valgrind/../../bin/vgdb --pid=1670 ...command...
==1670== 
==1670== TO DEBUG THIS PROCESS USING GDB: start GDB like this
==1670==   /path/to/gdb ./build/valgrind.out
==1670== and then give GDB the following command
==1670==   target remote | /usr/lib/x86_64-linux-gnu/valgrind/../../bin/vgdb --pid=1670
==1670== --pid is optional if only one valgrind process is running
==1670== 
--1670-- REDIR: 0x4022e10 (ld-linux-x86-64.so.2:strlen) redirected to 0x580c9ce2 (???)
--1670-- REDIR: 0x4022be0 (ld-linux-x86-64.so.2:index) redirected to 0x580c9cfc (???)
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/valgrind/vgpreload_core-amd64-linux.so
--1670--    object doesn't have a symbol table
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/valgrind/vgpreload_memcheck-amd64-linux.so
--1670--    object doesn't have a symbol table
==1670== WARNING: new redirection conflicts with existing -- ignoring it
--1670--     old: 0x04022e10 (strlen              ) R-> (0000.0) 0x580c9ce2 ???
--1670--     new: 0x04022e10 (strlen              ) R-> (2007.0) 0x0483f060 strlen
--1670-- REDIR: 0x401f5f0 (ld-linux-x86-64.so.2:strcmp) redirected to 0x483ffd0 (strcmp)
--1670-- REDIR: 0x4023370 (ld-linux-x86-64.so.2:mempcpy) redirected to 0x4843a20 (mempcpy)
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.28
--1670--    object doesn't have a symbol table
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/libgcc_s.so.1
--1670--    object doesn't have a symbol table
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/libc-2.31.so
--1670--   Considering /usr/lib/x86_64-linux-gnu/libc-2.31.so ..
--1670--   .. CRC mismatch (computed 86b78530 wanted e380f01c)
--1670--   Considering /lib/x86_64-linux-gnu/libc-2.31.so ..
--1670--   .. CRC mismatch (computed 86b78530 wanted e380f01c)
--1670--   Considering /usr/lib/debug/lib/x86_64-linux-gnu/libc-2.31.so ..
--1670--   .. CRC is valid
--1670-- Reading syms from /usr/lib/x86_64-linux-gnu/libm-2.31.so
--1670--   Considering /usr/lib/x86_64-linux-gnu/libm-2.31.so ..
--1670--   .. CRC mismatch (computed fcb42c76 wanted f6c95789)
--1670--   Considering /lib/x86_64-linux-gnu/libm-2.31.so ..
--1670--   .. CRC mismatch (computed fcb42c76 wanted f6c95789)
--1670--   Considering /usr/lib/debug/lib/x86_64-linux-gnu/libm-2.31.so ..
--1670--   .. CRC is valid
--1670-- REDIR: 0x4aed600 (libc.so.6:memmove) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec900 (libc.so.6:strncpy) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed930 (libc.so.6:strcasecmp) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec220 (libc.so.6:strcat) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec960 (libc.so.6:rindex) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aeedd0 (libc.so.6:rawmemchr) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4b09e60 (libc.so.6:wmemchr) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4b099a0 (libc.so.6:wcscmp) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed760 (libc.so.6:mempcpy) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed590 (libc.so.6:bcmp) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec890 (libc.so.6:strncmp) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec2d0 (libc.so.6:strcmp) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed6c0 (libc.so.6:memset) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4b09960 (libc.so.6:wcschr) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec7f0 (libc.so.6:strnlen) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec3b0 (libc.so.6:strcspn) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed980 (libc.so.6:strncasecmp) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec350 (libc.so.6:strcpy) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aedad0 (libc.so.6:memcpy@@GLIBC_2.14) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4b0b0d0 (libc.so.6:wcsnlen) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4b099e0 (libc.so.6:wcscpy) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec9a0 (libc.so.6:strpbrk) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec280 (libc.so.6:index) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aec7b0 (libc.so.6:strlen) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4af5d20 (libc.so.6:memrchr) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed9d0 (libc.so.6:strcasecmp_l) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed550 (libc.so.6:memchr) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4b09ab0 (libc.so.6:wcslen) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aecc60 (libc.so.6:strspn) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed8d0 (libc.so.6:stpncpy) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aed870 (libc.so.6:stpcpy) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aeee10 (libc.so.6:strchrnul) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4aeda20 (libc.so.6:strncasecmp_l) redirected to 0x48311d0 (_vgnU_ifunc_wrapper)
--1670-- REDIR: 0x4bd5490 (libc.so.6:__strrchr_avx2) redirected to 0x483ea10 (rindex)
--1670-- REDIR: 0x4ae7260 (libc.so.6:malloc) redirected to 0x483b780 (malloc)
--1670-- REDIR: 0x48f8c10 (libstdc++.so.6:operator new(unsigned long)) redirected to 0x483bdf0 (operator new(unsigned long))
--1670-- REDIR: 0x4bd8670 (libc.so.6:__memcpy_avx_unaligned_erms) redirected to 0x48429f0 (memmove)
--1670-- REDIR: 0x48f6e60 (libstdc++.so.6:operator delete(void*)) redirected to 0x483cf50 (operator delete(void*))
--1670-- REDIR: 0x4ae7850 (libc.so.6:free) redirected to 0x483c9d0 (free)
==1670== 
==1670== HEAP SUMMARY:
==1670==     in use at exit: 0 bytes in 0 blocks
==1670==   total heap usage: 390 allocs, 390 frees, 81,472 bytes allocated
==1670== 
==1670== All heap blocks were freed -- no leaks are possible
==1670== 
==1670== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)</code></pre>
  </div>
</div>