# The Linux Programming Interface
> **Author:** Michael Kerrisk

## ภาพรวมของหนังสือ

The Linux Programming Interface คือคู่มือฉบับสมบูรณ์สำหรับการเขียนโปรแกรมระดับ system programming บน Linux/UNIX ความยาว 1,556 หน้า ครอบคลุม 64 บท + appendix 6 หัวข้อ เขียนโดย Michael Kerrisk ซึ่งเป็น maintainer ของ man-pages project ของ Linux kernel เอง หนังสือเล่มนี้ไม่ได้มีแค่ Linux — ยังครอบคลุม UNIX/POSIX standard ด้วย ทำให้ใช้เป็น reference ข้าม platform ได้

---

## บทที่ 1-3: พื้นฐานและประวัติศาสตร์

### บทที่ 1 — History and Standards
- UNIX เริ่มจาก Ken Thompson ที่ Bell Labs ปี 1969 บน Digital PDP-7 แล้วย้ายไป PDP-11
- C language พัฒนาโดย Dennis Ritchie เพื่อ rewrite UNIX kernel ในภาษา level สูง (ปี 1973) — นี่คือจุดเริ่มต้นที่ UNIX port ไป hardware ต่างๆ ได้
- UNIX แตกเป็น 2 สายหลัก: **BSD** (จาก UC Berkeley, มี TCP/IP, vi, C shell) และ **System V** (จาก AT&T) ทำให้เกิดปัญหา fragmentation — แต่ละ vendor มี UNIX เฉพาะของตัวเอง (SunOS, AIX, HP-UX ฯลฯ)
- **GNU Project** (Richard Stallman, 1984): พัฒนา tools สำหรับ UNIX ฟรี — Emacs, GCC, bash, glibc — แต่ขาด kernel
- **Linux Kernel** (Linus Torvalds, 1991): เขียน kernel สำหรับ Intel 386 โดยได้แรงบันดาลใจจาก Minix ของ Andrew Tanenbaum
- **Standards**: ANSI C (1989/99) → POSIX.1 (1988) → SUSv3/POSIX.1-2001 → SUSv4/POSIX.1-2008 ทำให้ code portable ระหว่าง UNIX implementations ได้

### บทที่ 2 — Fundamental Concepts
- **Kernel**: ส่วนกลางของ OS ที่จัดการ hardware, process, memory, file system — ทำงานอยู่ใน kernel mode
- **Shell**: program ที่รับคำสั่งจาก user แล้วส่งให้ kernel ทำ (bash, sh, dash ฯลฯ)
- **Users and Groups**: ทุก process มี Real UID/GID และ Effective UID/GID — ใช้สำหรับ permission checking
- **Single Directory Hierarchy**: Linux มี root `/` เป็นจุดเริ่มต้นของ file tree ทั้งหมด — ต่างจาก Windows ที่มี drive หลายตัว
- **File I/O Model**: ทุกอย่างเป็น file — เครื่องพิมพ์, device, network socket, แม้แต่ process info ใน `/proc`
- **Processes vs Programs**: Program คือ binary file ที่อยู่บน disk; Process คือ instance ที่กำลังรันอยู่ มี memory space, PID, state เป็นของตัวเอง
- **Memory Layout**: Text (code) → Data (initialized) → BSS (uninitialized) → Heap → Stack
- **Virtual Memory**: แต่ละ process มี address space เป็นของตัวเอง kernel จัด mapping ไป physical memory ให้
- **Static vs Shared Libraries**: Static linking ฝัง code ลง binary ตรง; Shared linking (.so/.dll) ใช้ runtime ช่วยให้ binary เล็กลงและ update library ได้โดยไม่ต้อง recompile
- **Signals**: กลไก interrupt ระดับ software — เช่น SIGINT (Ctrl+C), SIGTERM, SIGKILL
- **Threads**: process ตัวเดียวมี multiple execution paths ร่วม memory space เดียวกัน
- **IPC (Interprocess Communication)**: Pipes, shared memory, message queues, semaphores, sockets
- **Job Control**: ทำให้ shell รัน background/foreground process ได้ ( jobs, fg, bg )
- **`/proc` File System**: virtual filesystem ที่ kernel สร้างขึ้น realtime ให้ user อ่าน process info และ system info

### บทที่ 3 — System Programming Concepts
- **System Calls**: จุดเชื่อมต่อระหว่าง user space → kernel — แต่ละ call ทำงานช้า (microseconds) เพราะต้อง switch mode
- **Library Functions**: wrapper ที่นั่งอยู่บน system calls เช่น `printf()` เรียก `write()`, `malloc()` เรียก `brk()`/`mmap()`
- **glibc (GNU C Library)**: standard C library ที่ Linux ใช้ — ครอบคลุม stdio, string, math, locale, threading
- **Error Handling**: system call คืนค่า -1 แล้วตั้ง `errno` — ต้องเช็คเสมอ ห้าม忽略了
- **Feature Test Macros**: เช่น `_GNU_SOURCE`, `_POSIX_C_SOURCE` บอก compiler ว่าต้องการ API level ไหน — สำคัญสำหรับ portability
- **Portability**: ใช้ header `<limits.h>`, `<unistd.h>` เพื่อเช็คค่า max/min ของแต่ละ system; หลีกเลี่ยง hardcoded constants

---

## บทที่ 4-5, 13-14: File I/O แบบละเอียด

### บทที่ 4 — File I/O: The Universal I/O Model
- **`open()`**: เปิดไฟล์ คืน file descriptor (int) — ใช้ flags เช่น `O_RDONLY`, `O_WRONLY`, `O_CREAT`, `O_TRUNC`
- **`read()` / `write()`**: อ่าน/เขียน data จาก/ไป file descriptor — ไม่ใช่ `\0` terminated เหมือน stdio
- **`close()`**: ปิด file descriptor
- **`lseek()`**: เลื่อน cursor ไปตำแหน่งไหนก็ได้ — random access
- **`creat()`**: shorthand สำหรับ `open()` แบบ write-only + create + truncate
- **Universality of I/O**: ใช้ `open()`/`read()`/`write()` กับ regular files, pipes, devices, terminals ได้เหมือนกันหมด

### บทที่ 5 — File I/O: Further Details
- **Atomicity**: ถ้า `write()` น้อยกว่า PIPE_BUF (4096 bytes) จะเป็น atomic — สำคัญมากสำหรับ pipes ที่มีหลาย writer
- **`fcntl()`**: file control — เปลี่ยน flags ของ open file เช่น เพิ่ม `O_APPEND` แบบ dynamic
- **`dup()` / `dup2()`**: duplicate file descriptor — เช่น redirect stdout ไป file ด้วย `dup2(fd, STDOUT_FILENO)`
- **`pread()` / `pwrite()`**: อ่าน/เขียน ณ offset ที่กำหนด โดยไม่ต้อง `lseek()` ก่อน — ลด race condition
- **`readv()` / `writev()`**: scatter-gather I/O — อ่าน/เขียนหลาย buffers พร้อมกันใน system call เดียว
- **`truncate()` / `ftruncate()`**: เปลี่ยนขนาดไฟล์
- **Nonblocking I/O**: เปิด `O_NONBLOCK` — ถ้าทำอะไรไม่ได้จะ return ทันทีแทนที่จะ block
- **`/dev/fd/`**: directory จำลองที่มี file descriptor แต่ละตัวเป็น symlink

### บทที่ 13 — File I/O Buffering
- **Buffer Cache (kernel)**: kernel เก็บ data ไว้ใน memory ก่อนเขียนลง disk — `write()` return ทันที ไม่รอ disk
- **stdio Buffering**: library มี buffer ของตัวเอง — full buffering (file), line buffering (terminal), unbuffered (stderr)
- **`fflush()`**: force flush buffer ของ stdio
- **`fsync()` / `fdatasync()`**: force flush ทั้ง stdio + kernel buffer ลง disk — สำคัญสำหรับ data integrity
- **`sync()`**: flush ทุก dirty buffer ทั้ง system
- **Direct I/O (`O_DIRECT`)**: bypass buffer cache เลย — ใช้สำหรับ database ที่จัดการ caching เอง

### บทที่ 14 — File Systems
- **Device Special Files**: device driver แต่ละตัวมี file ใน `/dev/` — block device (disk) กับ character device (terminal, serial)
- **I-node**: structure ใน kernel ที่เก็บ metadata ของไฟล์ (permissions, owner, size, block pointers) — ไม่เก็บชื่อไฟล์
- **Hard Links**: ชื่อไฟล์หลายชื่อชี้ไป I-node เดียวกัน — ลบ link ตัวสุดท้าย file ถึงจะหาย
- **Symbolic (Soft) Links**: file พิเศษที่เก็บ path ของไฟล์อีกตัว — ต่างจาก hard link เพราะข้าม filesystem ได้
- **Virtual File System (VFS)**: abstraction layer ที่ kernel ใช้รองรับ filesystem หลายชนิด (ext4, XFS, Btrfs, NTFS ฯลฯ)
- **`mount()` / `umount()`**: attach/detach filesystem เข้า/ออกจาก directory tree
- **`statvfs()`**: ดูข้อมูล filesystem เช่น block size, free space

---

## บทที่ 15-17, 18-19: File Metadata และ Monitoring

### บทที่ 15 — File Attributes
- **`stat()`**: ดึง metadata ทั้งหมดของไฟล์ — owner, group, permissions, size, timestamps, inode number
- **Timestamps**: mtime (content changed), ctime (metadata/permission changed), atime (last accessed)
- **`chmod()` / `fchmod()`**: เปลี่ยน permissions — rwxrwxrwx (owner/group/other)
- **`chown()`**: เปลี่ยน owner/group ของไฟล์
- **`umask()`**: ตั้ง default permission mask สำหรับไฟล์ใหม่
- **Set-User-ID / Set-Group-ID**: binary ที่รันด้วย permission ของ owner — เช่น `passwd` ต้องเขียน `/etc/shadow`
- **Sticky Bit**: ใน directory เช่น `/tmp` — user ลบได้เฉพาะไฟล์ของตัวเอง

### บทที่ 16 — Extended Attributes
- metadata เพิ่มเติมที่นอกเหนือจาก standard UNIX attributes เช่น `security.selinux`, `user.mime_type`
- ใช้ `setxattr()`, `getxattr()`, `listxattr()`, `removexattr()`

### บทที่ 17 — Access Control Lists (ACLs)
- ขยาย beyond user/group/other — กำหนด permission ให้ user หรือ group เฉพาะตัวได้
- `getfacl` / `setfacl` สำหรับจัดการจาก command line
- ACL_MASK: กำหนด upper bound สำหรับ ACL entries ทั้งหมด

### บทที่ 18 — Directories and Links
- Directory คือ file พิเศษที่เก็บ list ของ (filename → inode number)
- **`mkdir()` / `rmdir()`**: สร้าง/ลบ directory
- **`opendir()` / `readdir()`**: อ่าน directory entries
- **`nftw()`**: walk directory tree แบบ recursive
- **`chdir()` / `getcwd()`**: เปลี่ยน/ดู current working directory
- **`realpath()`**: resolve symbolic links จนได้ absolute path จริง
- **`dirname()` / `basename()`**: แยก path ออกเป็น directory part กับ filename part
- **`chroot()`**: เปลี่ยน root directory ของ process — ใช้สำหรับ chroot jail (security)

### บทที่ 19 — Monitoring File Events (inotify)
- **inotify API**: kernel-level ที่แจ้งเตือนเมื่อไฟล์/directory ถูกแก้ไข — ใช้ใน file sync, IDE, security monitoring
- Events เช่น `IN_CREATE`, `IN_DELETE`, `IN_MODIFY`, `IN_MOVED_FROM`, `IN_CLOSE_WRITE`
- `inotify_add_watch()` กำหนดว่าจะ monitor ไฟล์ไหน ด้วย event อะไร
- `read()` จาก inotify file descriptor จะคืน inotify event records

---

## บทที่ 20-22: Signals

### บทที่ 20 — Signals: Fundamental Concepts
- Signal คือ software interrupt ที่ส่งถึง process — มี default action: terminate, ignore, core dump, stop, continue
- 信号-types สำคัญ: SIGINT (2), SIGTERM (15), SIGKILL (9), SIGSTOP (19), SIGCHLD (17), SIGALRM (14)
- **`kill()`**: ส่ง signal ถึง process อื่น (ต้องมี permission)
- **Signal Mask**: block signal ไม่ให้ส่งจนกว่าจะ unblock
- **Signals Are Not Queued**: ถ้าส่ง signal เดียวกันหลายครั้งขณะ blocked จะได้แค่ครั้งเดียว
- **`sigaction()`**: ตั้ง signal handler แบบ advanced — ใช้แทน `signal()` เพราะ behavior ไม่ standard

### บทที่ 21 — Signals: Signal Handlers
- Signal handler ต้องระวังเรื่อง reentrancy — เรียกเฉพาะ async-signal-safe functions เท่านั้น (ไม่ใช่ `malloc()`, `printf()`)
- **`sig_atomic_t`**: ตัวแปรที่อ่าน/เขียนได้ atomic ใน signal handler
- **`sigaltstack()`**: ใช้ alternate stack สำหรับ signal handler — จำเป็นเมื่อ stack overflow
- **`SA_SIGINFO`**: รับข้อมูลเพิ่มเติมเกี่ยวกับ signal เช่น sender PID, signal number, memory fault address
- **System Call Interruption**: signal อาจ interrupt blocking system call — ใช้ `SA_RESTART` ให้ restart อัตโนมัติ หรือเช็ค `errno == EINTR`

### บทที่ 22 — Signals: Advanced Features
- **Core Dump**: kernel สร้าง file memory dump เมื่อ process ตายจาก signal ร้ายแรง — ใช้กับ gdb
- **Realtime Signals** (SIGRTMIN ถึง SIGRTMAX): ต่างจาก standard signals เพราะqueued ได้, มี priority, ไม่丢失
- **`sigsuspend()`**: atomically unblock signal + wait — ใช้สำหรับ等待 event อย่างมีประสิทธิภาพ
- **`signalfd()`**: อ่าน signals ผ่าน file descriptor แทน signal handler — ทำให้ integrate เข้า event loop ได้
- **`timerfd_create()`**: ใช้ timer ผ่าน file descriptor — เหมือนกัน integrate กับ select/poll/epoll

---

## บทที่ 23: Timers and Sleeping

- **Interval Timers** (`setitimer()`): ตั้ง timer ที่ส่ง SIGALRM ทุก X วินาที
- **`sleep()` / `nanosleep()`**: หยุด execution — `nanosleep()` ให้ precision สูงกว่า
- **POSIX Clocks** (`clock_gettime()`): ดูเวลาแบบ high-resolution — `CLOCK_MONOTONIC` (ไม่ reset เมื่อเปลี่ยนเวลา), `CLOCK_REALTIME`
- **POSIX Timers** (`timer_create()`): สร้าง timer ที่ส่ง signal, หรือ spawn thread เมื่อหมดเวลา
- **`timerfd`**: timer ผ่าน file descriptor — integrate กับ event loop ได้ดี
- **Overruns**: ถ้า timer ยิงเร็วเกินไป event จะ lost — ต้องเช็ค overrun count

---

## บทที่ 24-28: Processes

### บทที่ 24 — Process Creation
- **`fork()`**: สร้าง child process ที่เป็น copy ของ parent — return 0 ใน child, PID ของ child ใน parent
- **File Sharing**: หลัง `fork()` parent-child share file offsets — ถ้า parent เขียน offset 500 child จะเห็นด้วย
- **Copy-on-Write (COW)**: memory ยังไม่ copy จริงจนกว่าจะมีการเขียน — ทำให้ `fork()` เร็ว
- **`vfork()`**: เหมือน `fork()` แต่ไม่ copy memory — ใช้ร่วมกับ `execve()` เท่านั้น (deprecated)

### บทที่ 25 — Process Termination
- **`_exit()` vs `exit()`**: `_exit()` ออกจาก kernel ทันที; `exit()` เรียก exit handlers, flush stdio buffers ก่อน
- **Exit Handlers**: ใช้ `atexit()` ลงทะเบียน function ที่จะเรียกตอน process ออก — stack-like (LIFO)
- **Exit Status**: 0 = success, 1-255 = error; ใช้ `WEXITSTATUS()` เอาค่าจริง

### บทที่ 26 — Monitoring Child Processes
- **`wait()` / `waitpid()`**: parent รอให้ child ตายแล้วเอา exit status — ถ้าไม่ wait จะกลายเป็น zombie
- **Zombie Process**: process ที่ตายแล้วแต่ parent ยังไม่ได้ `wait()` — เก็บ exit status ไว้
- **Orphan Process**: parent ตายก่อน child — kernel เปลี่ยน parent เป็น init (PID 1)
- **`SIGCHLD`**: ส่งถึง parent เมื่อ child ตาย — handler ต้องเรียก `waitpid()` ด้วย `WNOHANG`

### บทที่ 27 — Program Execution
- **`execve()`**: แทนที่ memory image ของ process ด้วย program ใหม่ — PID ไม่เปลี่ยน
- **`execl()`, `execv()`, `execle()`, `execvp()`**: wrapper functions หลายแบบสำหรับ convenience
- **`system()`**: เรียก shell command จาก C — ปลอดภัยน้อยกว่า `fork()` + `execve()` เอง
- **Interpreter Scripts**: ไฟล์ text ที่มี shebang (`#!/path/to/interpreter`) — kernel จะเรียก interpreter มารัน script

### บทที่ 28 — Process Creation in Detail
- **`clone()`**: system call ที่ `fork()` สร้างจาก — ควบคุมได้ว่าจะ share อะไรกับ parent (memory, files, signals, namespaces)
- **`clone()` flags**: CLONE_VM (share memory), CLONE_FILES (share fd table), CLONE_NEWNS (new mount namespace)
- **Process Accounting**: kernel บันทึกข้อมูลแต่ละ process ลง file (user time, system time, memory usage) เมื่อ exited
- **Speed**: `fork()` + `execve()` ช้าลงเรื่อยๆ เพราะ COW — `vfork()` เร็วกว่าแต่จำกัดการใช้งาน

---

## บทที่ 29-33: Threads

### บทที่ 29 — Threads: Introduction
- Thread คือ execution path หลายตัวภายใน process เดียว — share memory, file descriptors, signals
- **`pthread_create()`**: สร้าง thread ใหม่
- **`pthread_join()`**: รอ thread จบแล้วเอา return value
- **`pthread_detach()`**: ไม่ต้อง join — thread จะ cleanup ตัวเองเมื่อจบ
- **Thread-Specific Storage**: แต่ละ thread มี data เฉพาะตัวผ่าน thread-local storage (TLS)

### บทที่ 30 — Thread Synchronization
- **Mutex (`pthread_mutex_t`)**: lock ป้องกัน race condition บน shared variables — lock/unlock ก่อนเข้า critical section
- **Condition Variables (`pthread_cond_t`)**: รอ/แจ้งเตือนเมื่อ condition เปลี่ยน — ต้องใช้คู่กับ mutex
- **Deadlock**: สอง thread จับ lock สองตัวคนละตัว → รอตลอดกาล — ป้องกันด้วย lock ordering

### บทที่ 31 — Thread Safety
- **Thread-Safety**: function ที่เรียกพร้อมกันจากหลาย thread ได้โดยไม่เสียหาย — ต้องไม่ใช้ static/global state ที่ไม่ protected
- **Reentrant Function**: thread-safe + สามารถ re-enter ตัวเองได้ (ไม่ใช้ static buffer)
- **Thread-Specific Data API**: `pthread_key_create()`, `pthread_setspecific()`, `pthread_getspecific()` — data per-thread

### บทที่ 32 — Thread Cancellation
- **`pthread_cancel()`**: สั่งให้ thread อื่นหยุด — thread จะถูก cancel ที่ cancellation point เท่านั้น
- **Cleanup Handlers**: `pthread_cleanup_push()` / `pop()` — function ที่จะเรียกเมื่อถูก cancel เพื่อ cleanup resources
- **Deferred vs Asynchronous Cancellation**: Deferred (default) ปลอดภัยกว่า; Async อันตรายเพราะ cancel ได้ทุกที่

### บทที่ 33 — Threads: Further Details
- **Thread Stacks**: แต่ละ thread มี stack ของตัวเอง (default ~8MB) — ตั้งค่าด้วย `pthread_attr_setstacksize()`
- **Threads and Signals**: signal ถูกส่งถึง thread ที่กำลัง blocking — thread มี signal mask ของตัวเอง
- **Linux Threading Implementations**: LinuxThreads (เก่า) → NPTL (ใหม่, POSIX-compliant, เสถียรกว่า)
- **`thread_local` / `__thread`**: keyword สำหรับ thread-local storage (ไม่ต้องใช้ API)

---

## บทที่ 34-36: Process Groups, Scheduling, Resources

### บทที่ 34 — Process Groups, Sessions, and Job Control
- **Process Group**: group ของ processes ที่รับ signal เดียวกัน — ใช้กับ job control (Ctrl+Z, fg, bg)
- **Session**: group ของ process groups ที่มี controlling terminal เดียวกัน — เมื่อ logout session จะถูกฆ่า
- **Controlling Terminal**: terminal ที่เชื่อมกับ session — กด Ctrl+C จะส่ง SIGINT ไป foreground process group
- **Job Control**: shell สามารถ suspend (`SIGTSTP`), resume (`SIGCONT`), foreground/background process groups ได้
- **`SIGHUP`**: ส่งถึง process group เมื่อ controlling terminal ปิด — daemon ต้อง handle เพื่อไม่ตาย

### บทที่ 35 — Process Priorities and Scheduling
- **Nice Value** (-20 ถึง 19): ยิ่งน้อยยิ่งได้ priority สูง — ใช้ `nice()`, `setpriority()`
- **Realtime Scheduling**: `SCHED_FIFO` (preemptive), `SCHED_RR` (round-robin with time slice) — สำหรับ latency-critical tasks
- **`sched_setscheduler()`**: เปลี่ยน scheduling policy
- **CPU Affinity**: ผูก process กับ CPU เฉพาะตัว — ใช้ `sched_setaffinity()`

### บทที่ 36 — Process Resources
- **`getrusage()`**: ดู resource usage ของ process เช่น user/system time, memory, page faults
- **Resource Limits** (`getrlimit()` / `setrlimit()`): จำกัด CPU time, memory, open files, processes — prevent runaway processes
- **`ulimit`** command: ตั้ง resource limits จาก shell

---

## บทที่ 37-40: Daemons, Security, Capabilities

### บทที่ 37 — Daemons
- Daemon คือ process ที่รัน background ตลอด time เช่น `httpd`, `sshd`, `syslogd`
- **Pattern**: `fork()` → setsid() → chdir("/") → close stdio → umask(0) → loop
- **`syslog` API**: ส่ง log messages ไป unified logging system — ใช้ `syslog()`, `openlog()`, `closelog()`
- **SIGHUP**: daemon ใช้ reload config — handle SIGHUP แล้วอ่าน config ใหม่

### บทที่ 38 — Writing Secure Privileged Programs
- หลักการ: operate with least privilege, อย่า trust input, อย่า expose sensitive info, check return values
- Set-User-ID programs ต้องระวัง buffer overflow, TOCTOU race conditions, environment variable injection
- Drop privileges ทันทีหลังทำ task ที่ต้องใช้ root

### บทที่ 39 — Capabilities
- Linux capabilities แบ่ง root power ออกเป็น ~37 หน่วยย่อย เช่น CAP_NET_ADMIN (network config), CAP_SYS_ADMIN
- แทนที่จะเป็น root/full user — process ได้แค่ capability ที่จำเป็น
- File capabilities: กำหนดว่า binary นี้ต้องมี capability อะไรบ้างเมื่อ execute

### บทที่ 40 — Login Accounting
- **`utmp`**: บันทึก users ที่ login อยู่ตอนนี้
- **`wtmp`**: บันทึก login/logout history
- **`lastlog`**: บันทึก last login ของแต่ละ user
- ใช้ `getutxent()`, `pututxline()` API ในการอ่าน/เขียน

---

## บทที่ 41-42: Shared Libraries

### บทที่ 41 — Fundamentals
- **Static Libraries** (`.a`): archive ของ object files — linking เสร็จตอน compile
- **Shared Libraries** (`.so`): code ที่ share ได้ระหว่างหลาย process — linking เสร็จตอน runtime
- **Soname**: ชื่อ logical ของ library เช่น `libfoo.so.1` → file จริง `libfoo.so.1.2.3`
- **Position-Independent Code (PIC)**: code ที่รันได้ที่ address ไหนก็ได้ — จำเป็นสำหรับ shared library
- **`ldconfig`**: สร้าง symlinks ให้ dynamic linker ค้นหา shared libraries ได้เร็ว

### บทที่ 42 — Advanced Features
- **`dlopen()` / `dlsym()` / `dlclose()`**: โหลด shared library แบบ runtime — ใช้สำหรับ plugin systems
- **Symbol Visibility**: ควบคุมว่า symbol ไหน export ออกจาก shared library ด้วย version scripts
- **Preloading**: `LD_PRELOAD` โหลด library ก่อน — ใช้ debug, intercept function calls
- **`LD_DEBUG`**: แสดง debug output ของ dynamic linker

---

## บทที่ 43-55: Interprocess Communication (IPC)

### บทที่ 43 — IPC Overview
- IPC methods: Pipes, FIFOs, System V IPC (message queues, semaphores, shared memory), POSIX IPC, memory mappings, file locking, sockets
- เปรียบเทียบ: bandwidth, latency, ease-of-use, portability

### บทที่ 44 — Pipes and FIFOs
- **Pipe** (`pipe()`): unidirectional channel ระหว่าง parent-child — data flow ทางเดียว
- **FIFO (Named Pipe)**: pipe ที่มีชื่อใน filesystem — process ที่ไม่ใช่ parent-child ก็ใช้ได้
- **Pipe Chaining**: `cmd1 | cmd2 | cmd3` — shell สร้าง pipe chain ให้อัตโนมัติ
- **`popen()`**: เปิด pipe ไป/จาก shell command — ใช้รัน command เอา output มาประมวลผล

### บทที่ 45-48 — System V IPC
- **IPC Keys**: ใช้ `ftok()` สร้าง key สำหรับ ipcget()
- **Message Queues**: ส่ง/รับ messages แบบ structured — kernel เก็บ queue ให้
- **Semaphores**: synchronization primitive — นับจำนวน permits สำหรับเข้าถึง shared resource
- **Shared Memory**: `shmget()` + `shmat()` — memory segment ที่ share ได้ระหว่าง processes เร็วที่สุดใน IPC ทุกวิธี
- **`ipcs` / `ipcrm`**: commands สำหรับดู/ลบ IPC objects

### บทที่ 49 — Memory Mappings
- **`mmap()`**: map file หรือ anonymous memory เข้า virtual address space — อ่าน/เขียนผ่าน pointer
- **Private vs Shared Mapping**: Private (copy-on-write), Shared (เปลี่ยนทั้งสองฝั่ง)
- **`munmap()`**: unmap region
- **`msync()`**: sync memory-mapped region กลับ file
- ใช้สำหรับ: โหลด executable, shared memory, memory-mapped files

### บทที่ 50 — Virtual Memory Operations
- **`mprotect()`**: เปลี่ยน protection (read/write/execute) ของ memory region
- **`mlock()` / `mlockall()`**: lock memory ไม่ให้ swap ออก — สำหรับ realtime systems
- **`mincore()`**: เช็คว่า memory page ไหนอยู่ใน RAM
- **`madvise()`**: ให้ advice กับ kernel ว่าจะใช้ memory ยังไง — `MADV_SEQUENTIAL`, `MADV_RANDOM`, `MADV_WILLNEED`

### บทที่ 51-54 — POSIX IPC
- POSIX versions ของ message queues, semaphores, shared memory — ใช้ file-descriptor-based API แทน System V
- **POSIX Message Queues**: ใช้ `mq_open()`, `mq_send()`, `mq_receive()` — ต่างจาก SysV เพราะเป็น file-like
- **POSIX Semaphores**: `sem_open()` (named) / `sem_init()` (unnamed) — เร็วกว่า SysV semaphores
- **POSIX Shared Memory**: `shm_open()` + `mmap()` — ใช้ virtual filesystem `/dev/shm`

### บทที่ 55 — File Locking
- **`flock()`**: advisory lock ระดับ whole file — ง่ายแต่มีข้อจำกัด
- **`fcntl()` record locks**: lock เฉพาะ byte range ของไฟล์ — ใช้กับ concurrent access
- **Mandatory Locking**: kernel enforce lock — แต่ deprecated เพราะ performance issues
- **`/proc/locks`**: ดู locks ทั้งหมดในระบบ
- ใช้ทำ: prevent duplicate instances, protect shared resources

---

## บทที่ 56-61: Sockets

### บทที่ 56 — Sockets: Introduction
- **`socket()`**: สร้าง socket endpoint — เลือก type (stream, datagram, raw) และ domain
- **`bind()`**: ผูก socket กับ address (IP + port)
- **Stream Sockets (TCP)**: reliable, connection-oriented — `listen()` → `accept()` → `read()`/`write()`
- **Datagram Sockets (UDP)**: unreliable, connectionless — `sendto()` / `recvfrom()`

### บทที่ 57 — Sockets: UNIX Domain
- ใช้ communicate ระหว่าง process บนเครื่องเดียว — address เป็น filesystem path
- เร็วกว่า TCP/IP เพราะไม่ผ่าน network stack
- **`socketpair()`**: สร้างคู่ connected sockets — ใช้ parent-child communication

### บทที่ 58 — TCP/IP Fundamentals
- Network layers: Data Link → Network (IP) → Transport (TCP/UDP) → Application
- **IP Addresses**: IPv4 (32-bit, เช่น 192.168.1.1) และ IPv6 (128-bit)
- **Port Numbers**: ระบุ application ภายใน host — well-known ports 0-1023
- **TCP**: reliable, ordered, flow-controlled — 3-way handshake (SYN → SYN-ACK → ACK)
- **UDP**: fast, no guarantee — ใช้กับ DNS, streaming, gaming

### บทที่ 59 — Sockets: Internet Domains
- **`inet_pton()` / `inet_ntop()`**: แปลง IP address ระหว่าง binary ↔ string
- **`getaddrinfo()`**: DNS resolution แบบ protocol-independent — return linked list ของ addresses
- **`getnameinfo()`**: แปลง address กลับเป็น hostname + service name
- **Client-Server Pattern**: Client `connect()` → Server `accept()` → แล้ว `read()`/`write()` ทั้งคู่

### บทที่ 60 — Server Design
- **Iterative Server**: รับ request ทีละตัว — ง่ายแต่ช้า
- **Concurrent Server**: `fork()` หรือ thread per connection — scale ได้ดี
- **`inetd` (Internet Superserver)**: daemon ตัวเดียวจัดการ listening ports หลายตัว — fork ให้ service ที่เหมาะสม

### บทที่ 61 — Advanced Socket Topics
- **`sendfile()`**: ส่ง file ผ่าน socket โดยไม่ต้อง copy ผ่าน user space — zero-copy, เร็วมาก
- **TCP State Machine**: ESTABLISHED → FIN_WAIT → TIME_WAIT → CLOSED
- **`SO_REUSEADDR`**: reuse port ที่อยู่ใน TIME_WAIT — จำเป็นสำหรับ restart server เร็ว
- **Out-of-Band Data**: TCP urgent mode — ส่งข้อความฉุกเฉินข้าม data stream
- **Passing File Descriptors**: ส่ง open file descriptor ผ่าน UNIX domain socket — ทำได้จริง!
- **`tcpdump`**: packet sniffer สำหรับ debug network traffic

---

## บทที่ 62-64: Terminal, Alternative I/O, Pseudoterminals

### บทที่ 62 — Terminals
- Terminal driver ทำ line editing (backspace, Ctrl+U) ให้ — **canonical mode**
- **Noncanonical Mode**: อ่านได้ทันทีไม่ต้องรอ newline — ใช้สำหรับ apps ที่ต้องการ input ทีละ character
- **`termios` API**: ดู/เปลี่ยน terminal attributes — baud rate, echo, special characters
- Terminal special characters: `VINTR` (Ctrl+C), `VSUSP` (Ctrl+Z), `VEOF` (Ctrl+D), `VKILL` (Ctrl+U)

### บทที่ 63 — Alternative I/O Models
- ปัญหา: blocking I/O ไม่ efficient เมื่อต้อง monitor หลาย file descriptors
- **`select()` / `poll()`**: I/O multiplexing — รอ event จาก multiple fds ใน system call เดียว
- **`epoll` (Linux-specific)**: เร็วกว่า select/poll มากเมื่อมี fd เป็นพันๆ — scalable, event-driven
- **Signal-Driven I/O (`SIGIO`)**: kernel ส่ง signal เมื่อ fd พร้อมอ่าน/เขียน
- **`pselect()`**: like `select()` + atomic signal unblock — แก้ race condition
- **Self-Pipe Trick**: pipe เข้าตัวเอง + select() — รวม signals เข้า event loop
- **Level-Triggered vs Edge-Triggered**: level = แจ้งเรื่อยๆ จนกว่าจะทำ; edge = แจ้งเฉพาะตอนเปลี่ยน

### บทที่ 64 — Pseudoterminals
- PTY จำลอง terminal คู่ — master side (app ควบคุม) + slave side (program คิดว่าเป็น terminal จริง)
- ใช้ใน: terminal emulators (xterm, gnome-terminal), `script` command, SSH sessions
- **`posix_openpt()` / `grantpt()` / `unlockpt()` / `ptsname()`**: API สำหรับเปิด PTY master/slave

---

## Appendix

- **A. Tracing System Calls**: ใช้ `strace` ติดตาม system calls ที่ program เรียก — debug tool อันดับ 1
- **B. Parsing Command-Line Options**: `getopt()`, `getopt_long()` — สำหรับ parse arguments แบบ standard
- **C. Casting NULL Pointer**: NULL pointer ไม่ always เป็น address 0 — casting ไป type อื่นอาจทำให้ compiler เตือน
- **D. Kernel Configuration**: compile kernel เอง — เลือก modules ที่ต้องการ
- **E. Further Sources of Information**: man pages, LWN.net, kernel newbies, Linux documentation project
- **F. Solutions to Selected Exercises**: คำตอบแบบฝึกหัดบางข้อจากแต่ละบท

---

## สรุปภาพรวม

หนังสือเล่มนี้คือ "Bible" ของ Linux/UNIX system programming — ไม่มีเล่มไหนที่ครอบคลุมเท่านี้อีกแล้ว 64 บท ตั้งแต่ระดับพื้นฐาน (file I/O, process, signal) ไปจนถึงระดับสูง (shared libraries, IPC, sockets, epoll, pseudoterminals) ทุกหัวข้อมีตัวอย่าง code (C) ให้ทดลองได้จริง

**จุดแข็ง**: depth ทุกหัวข้อ, ครอบคลุม POSIX + Linux-specific, มี exercises, มี appendix สำหรับ tool ต่างๆ

**เหมาะสำหรับ**: นักพัฒนาที่อยากเข้าใจว่า OS ทำงานยังไง, คนที่เขียน daemons/services, network programmers, ใครที่อยากผ่าน interview ระดับ senior/backend/infra

---
