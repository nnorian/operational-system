# xv6 notes

## (1) System calls used by `user/cat.c`

| Call | Where in cat.c | What it asks the kernel to do |
|---|---|---|
| `open(argv[i], O_RDONLY)` | line 35 | Find the file by name and give back a file descriptor (a small number) the program can use to read it. |
| `read(fd, buf, sizeof(buf))` | line 12 | Copy up to 512 bytes from the open file into the program's `buf`. Returns how many bytes were copied, 0 at end of file, or -1 on error. |
| `write(1, buf, n)` | line 13 | Copy `n` bytes from `buf` to file descriptor 1 (standard output, the console). |
| `close(fd)` | line 40 | Release the file descriptor so the kernel can free the open-file entry. |
| `exit(0)` / `exit(1)` | lines 15, 20, 31, 37, 42 | End this process and hand the exit status to the parent (the shell). |

`fprintf(2, ...)` looks like a fifth call but is not a system call. It is a
library function in `user/printf.c` that ends up calling `write(fd, &c, 1)` once
per character (`putc`, line 12), so it is really more `write` calls to fd 2
(standard error).

How a call gets into the kernel: `read()` in user space is a three-line stub in
`user/usys.S` that puts the call number in register `a7` (`SYS_read` = 5, from
`kernel/syscall.h`) and runs `ecall`, which traps into the kernel. There,
`syscall()` in `kernel/syscall.c` looks the number up in the `syscalls[]` table
(`[SYS_read] = sys_read`, line 115) and calls the matching function.

## (2) Where `sys_read` is implemented

`kernel/sysfile.c`, line 69 (the body is lines 69–80). It reads the three
arguments (`argfd`, `argaddr`, `argint`), checks that the file descriptor is
valid, and passes the work to `fileread()` in `kernel/file.c`.

## (3) `kernel/` vs `user/`

Code in `kernel/` runs in privileged supervisor mode with full access to the
hardware and all memory, while code in `user/` runs as ordinary programs in
user mode, each in its own isolated memory, and can only reach files, devices
or other processes by asking the kernel through system calls.
