the lab was done on my personal laptop running Arch Linux

Part 1
```terminal 
[nnorian@nnorian ~]$ cd /home/nnorian/uni/os/os-course
[nnorian@nnorian os-course]$ mkdir -p ~/os-lab1 && cd ~/os-lab1
[nnorian@nnorian os-lab1]$ whoami
nnorian
[nnorian@nnorian os-lab1]$ uname -a
Linux nnorian 7.2.6-arch2-1 #1 SMP PREEMPT_DYNAMIC Mon, 14 Sep 2026 22:41:30 +0000 x86_64 GNU/Linux
[nnorian@nnorian os-lab1]$ uptime
 15:50:33 up 1 day, 22:52,  1 user,  load average: 0.33, 0.60, 0.60
[nnorian@nnorian os-lab1]$ pwd
/home/nnorian/os-lab1
[nnorian@nnorian os-lab1]$ ls -la /
total 60
dr-xr-xr-x  17 root root  4096 Nov  4  2025 .
dr-xr-xr-x  17 root root  4096 Nov  4  2025 ..
lrwxrwxrwx   1 root root     7 Oct 12  2025 bin -> usr/bin
drwxr-xr-x   4 root root  4096 Jan  1  1970 boot
drwxr-xr-x  21 root root  4340 Sep 26 16:58 dev
drwxr-xr-x  99 root root  4096 Sep 28 15:40 etc
drwxr-xr-x   3 root root  4096 Oct 30  2025 home
lrwxrwxrwx   1 root root     7 Oct 12  2025 lib -> usr/lib
lrwxrwxrwx   1 root root     7 Oct 12  2025 lib64 -> usr/lib
drwx------   2 root root 16384 Oct 30  2025 lost+found
drwxr-xr-x   2 root root  4096 Oct 12  2025 mnt
drwxr-xr-x   8 root root  4096 Jul 21 12:08 opt
dr-xr-xr-x 488 root root     0 Sep 26 16:58 proc
drwx------   8 root root  4096 Apr 28 08:50 root
drwxr-xr-x  38 root root   880 Sep 28 15:40 run
lrwxrwxrwx   1 root root     7 Oct 12  2025 sbin -> usr/bin
lrwxrwxrwx   1 root root    19 Nov  4  2025 snap -> /var/lib/snapd/snap
drwxr-xr-x   4 root root  4096 Oct 30  2025 srv
dr-xr-xr-x  13 root root     0 Sep 26 16:58 sys
drwxrwxrwt  25 root root   560 Sep 28 15:47 tmp
drwxr-xr-x   9 root root  4096 Sep 23 20:16 usr
drwxr-xr-x  14 root root  4096 Sep 25 22:31 var
[nnorian@nnorian os-lab1]$ mkdir demo && cd demo
[nnorian@nnorian demo]$ echo "hello operating systems" > note.txt
[nnorian@nnorian demo]$ cp note.txt copy.txt
[nnorian@nnorian demo]$ mv copy.txt renamed.txt
[nnorian@nnorian demo]$ ls -l
total 8
-rw-r--r-- 1 nnorian nnorian 24 Sep 28 15:52 note.txt
-rw-r--r-- 1 nnorian nnorian 24 Sep 28 15:52 renamed.txt
[nnorian@nnorian demo]$ rm renamed.txt
[nnorian@nnorian demo]$ ls -l note.txt
-rw-r--r-- 1 nnorian nnorian 24 Sep 28 15:52 note.txt
[nnorian@nnorian demo]$ chmod 600 note.txt
[nnorian@nnorian demo]$ ls -l note.txt
-rw------- 1 nnorian nnorian 24 Sep 28 15:52 note.txt
[nnorian@nnorian demo]$ chmod 644 note.txt
[nnorian@nnorian demo]$ 


```

1.   the owner is nnorian and group is nnorian
2. `-rw-------` shows the rights andmeans: 
3. `-` is a regular file,
4. `rw-`  the owner can read and write
5. and the two `---` groups mean the group and everyone else have no access to file 
6. Directories under `/`: 
 - `/etc` holds config files 
 - `/dev` holds device files
- `/proc` shows live kernel and process info,
- `/home` holds user folders (like all the personal folders repositories and photoss)
 - `/boot` holds the kernel and bootloader needed for laptop to boot up
Part 2
``` 
[nnorian@nnorian ~]$ ps aux |head
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  20752 14944 ?        Ss   Sep26   0:04 /usr/lib/systemd/systemd --switched-root --system --deserialize=56
root           2  0.0  0.0      0     0 ?        S    Sep26   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    Sep26   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   Sep26   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   Sep26   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   Sep26   0:00 [kworker/R-kvfree_rcu_reclaim]
root           7  0.0  0.0      0     0 ?        I<   Sep26   0:00 [kworker/R-slub_flushwq]
root           8  0.0  0.0      0     0 ?        I<   Sep26   0:00 [kworker/R-netns]
root          11  0.0  0.0      0     0 ?        I<   Sep26   0:00 [kworker/0:0H-kblockd]
[nnorian@nnorian ~]$ ps aux | wc -l
483
[nnorian@nnorian ~]$ 

top - 15:55:59 up 1 day, 22:57,  1 user,  load average: 0.34, 0.50, 0.57
Tasks: 489 total, 1 running, 487 sleep, 0 d-sleep, 0 stopped, 1 zombie
%Cpu(s):  0.6 us,  0.4 sy,  0.0 ni, 98.5 id,  0.0 wa,  0.3 hi,  0.1 si,  0.0 st 
MiB Mem :  31195.2 total,  11800.5 free,  12800.8 used,   7726.5 buff/cache     
MiB Swap:   4096.0 total,   4096.0 free,      0.0 used.  18394.4 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND      
  75684 nnorian   20   0 1181508 197468 155432 S   3.6   0.6   0:09.55 kgx          
   2875 nnorian   20   0 4116744 288032 143772 S   2.7   0.9  42:30.90 gnome-shell  
  37654 nnorian   20   0 5634692 276860 123228 S   1.7   0.9   3:18.21 claude       
    483 root     -51   0       0      0      0 S   0.7   0.0   1:04.13 irq/69-ASUP+ 
  37504 nnorian   20   0 1448.4g 271164 123516 S   0.7   0.8   2:31.42 electron     
  64328 nnorian   20   0 2146996 933080 302464 S   0.7   2.9   3:16.74 Telegram     
     44 root      rt   0       0      0      0 S   0.3   0.0   0:03.41 migration/4  
    612 root      20   0       0      0      0 S   0.3   0.0   0:48.03 napi/phy0-0  
    712 root      20   0 1281960  82080  35220 S   0.3   0.3   0:29.62 netbird      
    716 root      20   0 2437784  56372  36848 S   0.3   0.2   0:50.03 containerd   
   2664 nnorian   20   0    7628   5696   2736 S   0.3   0.0   0:04.62 dbus-broker  
   5669 root      20   0 2684032 163928  58264 S   0.3   0.5   2:57.13 dockerd      
   7128 nnorian   20   0   53.1g 613412 381492 S   0.3   1.9  20:27.92 chrome       
   7758 nnorian   20   0 1450.3g 384196 140596 S   0.3   1.2   4:10.55 chrome       
  51246 nnorian   20   0 1448.4g 340308 162700 S   0.3   1.1   0:46.56 chrome       
  62717 root      20   0       0      0      0 I   0.3   0.0   0:41.47 kworker/u64+ 
  69241 nnorian   20   0 1448.5g 614108 181072 S   0.3   1.9   3:01.64 chrome       
  73118 nnorian   20   0 5618292 249048 120668 S   0.3   0.8   0:07.42 claude       
  73902 nnorian   20   0 5614192 260624 120660 S   0.3   0.8   0:07.77 claude       
  75118 nnorian   20   0 5610028 242584 116592 S   0.3   0.8   0:04.13 claude       
  77498 nnorian   20   0 1392.3g 187924 136880 S   0.3   0.6   0:01.47 obsidian     
  77956 nnorian   20   0   10508   7932   5664 R   0.3   0.0   0:00.06 top          
      1 root      20   0   20752  14944  10484 S   0.0   0.0   0:04.69 systemd      
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.06 kthreadd     
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqu+ 
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-r+ 
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-s+ 
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-k+ 
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-s+ 
      8 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-n+ 
     11 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0+ 
     14 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-m+ 
     15 root      20   0       0      0      0 S   0.0   0.0   0:00.60 ksoftirqd/0  
     16 root      -2   0       0      0      0 I   0.0   0.0   0:57.20 rcu_preempt  
     17 root      -2   0       0      0      0 S   0.0   0.0   0:00.00 rcub/0       
[nnorian@nnorian ~]$ sleep 300 &
[1] 78010
[nnorian@nnorian ~]$ jobs
[1]+  Running                    sleep 300 &
[nnorian@nnorian ~]$ ps -ef | grep sleep
nnorian    78010   77839  0 15:56 pts/2    00:00:00 sleep 300
nnorian    78146   77839  0 15:56 pts/2    00:00:00 grep --color=auto sleep
[nnorian@nnorian ~]$ kill 78010
[nnorian@nnorian ~]$ sleep 300 &
[2] 78209
[1]   Terminated                 sleep 300
[nnorian@nnorian ~]$ PID=$!
[nnorian@nnorian ~]$ ls /proc/$PID/
arch_status         environ            maps           pagemap       stat
attr                exe                mem            personality   statm
autogroup           fd                 mountinfo      projid_map    status
auxv                fdinfo             mounts         root          syscall
cgroup              gid_map            mountstats     sched         task
clear_refs          io                 net            schedstat     timens_offsets
cmdline             ksm_merging_pages  ns             sessionid     timers
comm                ksm_stat           numa_maps      setgroups     timerslack_ns
coredump_filter     limits             oom_adj        smaps         uid_map
cpu_resctrl_groups  loginuid           oom_score      smaps_rollup  wchan
cwd                 map_files          oom_score_adj  stack
[nnorian@nnorian ~]$ cat /proc/$PID/status | head -20
Name:	sleep
Umask:	0022
State:	S (sleeping)
Tgid:	78209
Ngid:	0
Pid:	78209
PPid:	77839
TracerPid:	0
Uid:	1000	1000	1000	1000
Gid:	1000	1000	1000	1000
FDSize:	256
Groups:	150 960 991 998 1000 
NStgid:	78209
NSpid:	78209
NSpgid:	78209
NSsid:	77839
Kthread:	0
VmPeak:	    6016 kB
VmSize:	    6016 kB
VmLck:	       0 kB
[nnorian@nnorian ~]$ kill $PID
[nnorian@nnorian ~]$ 

```

the  PID of nr 1 is `systemd` (`/usr/lib/systemd/systemd`).
thats the init system, basically the first process made for user wich starts services
there were about 482 processes cuz its my personal laptop and i have a few applications working 
`ps aux | wc -l` gave 483, minus 1 for the header line. `top` showed 489 tasks because it ran at a slightly different moment ( weirdly when i was tking the responses the first two commands were cut off after i looked live at the processes running on my laptop)
the State line said `S (sleeping)` baucause as you mentioned in the question, it is indeed sleeping 
Part 3
```
[nnorian@nnorian ~]$ free -h
               total        used        free      shared  buff/cache   available
Mem:            30Gi        12Gi        11Gi       623Mi       7.6Gi        17Gi
Swap:          4.0Gi          0B       4.0Gi
[nnorian@nnorian ~]$ cat /proc/meminfo | head -6
MemTotal:       31943852 kB
MemFree:        11966108 kB
MemAvailable:   18724236 kB
Buffers:          397424 kB
Cached:          7236192 kB
SwapCached:            0 kB
[nnorian@nnorian ~]$ sleep 300 & PID=$!
[1] 78734
[nnorian@nnorian ~]$ grep VmRSS /proc/$PID/status
VmRSS:	    4252 kB
[nnorian@nnorian ~]$ kill $PID
[nnorian@nnorian ~]$ 
```
total RAM is 30 GiB  wich you can see from the MemTotal 31943852 kB
Free is 11 GiB, and 17 GiB is available ( what is making me happy tbh)
swap is  the place where the things i am not currently using are kept, where the kernel moves memory pages out of RAM when RAM runs low
me personnaly i have 4 GiB, and it's **zram0**, which is compressed swap kept in RAM so bassicaly it stil l in ram and not as expected on my hard drive 
VmRSS is 4252 kB (about 4 MB), while VmSize is 6016 kB. thats actually quite large for just a process that is not used 
Part 4:
```
[nnorian@nnorian ~]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme0n1p2  937G  452G  438G  51% /
devtmpfs         16G     0   16G   0% /dev
tmpfs            16G   47M   16G   1% /dev/shm
efivarfs        128K   28K   96K  23% /sys/firmware/efi/efivars
tmpfs           6.1G  2.0M  6.1G   1% /run
none            1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
none            1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
none            1.0M     0  1.0M   0% /run/credentials/systemd-networkd.service
tmpfs            16G  2.9M   16G   1% /tmp
/dev/nvme0n1p1 1022M   66M  957M   7% /boot
tmpfs           3.1G  241M  2.9G   8% /run/user/1000
[nnorian@nnorian ~]$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
zram0       253:0    0     4G  0 disk [SWAP]
nvme0n1     259:0    0 953.9G  0 disk 
├─nvme0n1p1 259:1    0     1G  0 part /boot
└─nvme0n1p2 259:2    0 952.9G  0 part /
[nnorian@nnorian ~]$ du -sh ~/os-lab1
12K	/home/nnorian/os-lab1
[nnorian@nnorian ~]$ ls -l /dev | head
total 0
drwxr-xr-x   2 root    root              60 Sep 26 16:58 accel
crw-r--r--   1 root    root       10,   235 Sep 26 16:58 autofs
drwxr-xr-x   2 root    root             120 Sep 26 16:58 block
crw-rw----   1 root    disk       10,   234 Sep 26 16:58 btrfs-control
drwxr-xr-x   3 root    root              60 Sep 26 16:58 bus
drwxr-xr-x   2 root    root            5100 Sep 28 15:37 char
crw-------   1 root    root        5,     1 Sep 26 16:58 console
lrwxrwxrwx   1 root    root              11 Sep 26 16:58 core -> /proc/kcore
drwxr-xr-x  18 root    root             360 Sep 26 16:58 cpu
[nnorian@nnorian ~]$ mount | head
/dev/nvme0n1p2 on / type ext4 (rw,relatime)
devtmpfs on /dev type devtmpfs (rw,nosuid,size=15896188k,nr_inodes=3974047,mode=755,inode64,huge=advise)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,inode64,huge=advise,usrquota)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=600,ptmxmode=000)
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot,memory_hugetlb_accounting)
none on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
efivarfs on /sys/firmware/efi/efivars type efivarfs (rw,nosuid,nodev,noexec,relatime)
bpf on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
[nnorian@nnorian ~]$ 

```

/ is mounted on /dev/nvme0n1p2 the device file that is Non-Volatile Memory Express a protocol that manages storage device wit hte 0 as the first strage device and n1 the namespace and the p is partiction cuz ssp normally is partitioned 
the entrie i will explain is the `/dev/nvme0n1` is the NVMe SSD
"Everything is a file basically as interesting and simple as it is all the disks, terminals and kernel data even all the applications you dwd with  proprietary code all show up as paths you can read with ls or cat just like regular files, not all will be huma readable thought 

top showed 1 zombie process.
The [1] Terminated sleep 300  message appeared after `kill`.
Interesting output: lsblk showed zram0 [SWAP]
Groups: 150 960 991 998 1000 in /proc/$PID/status


The operating system manages  four main groups of resources that are : files, processes, memory and devices. 
during the lab i saw:
1. file management with `ls -l`, which showed each file's owner group and permissions
2.  process management with `ps aux`, which listed about 482 running processes starting with `systemd` as PID 1.
3. `free -h` showed how the OS splits 30 GiB of RAM and 4 GiB of swap among processes
4. `lsblk` showed the disks it hides behind file-like device entries such as `/dev/nvme0n1`.
