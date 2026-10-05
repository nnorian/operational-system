# Lab 3 – The Command Line: Shell, Pipes and Text Filters

Done on my laptop, Arch Linux, GNU bash 5.3.

## Purpose of the work

Learn to move fast in bash, predict what the shell does to a command line before it
runs (globbing, quoting, expansions), redirect stdout and stderr, chain commands, run
background jobs, and answer questions about data with one line of small filters
joined by pipes. Finally, turn the one-liners into a small script.

---

## Part 1. Move fast, find help

Tab: `ls /usr/sh` + Tab → the only match is `/usr/share/`, so the first Tab completed
it; the second Tab listed everything inside `/usr/share` (275 entries, so bash asked
"Display all 275 possibilities?"). `cd /et` + Tab once → completed to `cd /etc/`
because `/etc` is the only name starting with `et`.

Ctrl+R: after `date`, `whoami`, `uname -r`, Ctrl+R and typing `dat` found
`date` in the history (reverse search); Enter ran it again.

```
$ type cd ; type ls ; type echo ; which ls
cd is a shell builtin
ls is /usr/bin/ls
echo is a shell builtin
/usr/bin/ls
$ nosuchprog ; echo "exit code: $?"
bash: nosuchprog: command not found
exit code: 127
```

[Figure 1.1 – Tab completion]
[Figure 1.2 – Output of type and which]
[Figure 1.3 – The directories in PATH]
[Figure 1.4 – Exit code of a missing command]

**Observe**

1. `cd` and `echo` are built-ins, `ls` is an external program (`/usr/bin/ls`).
   `cd` must be a built-in because the current directory belongs to a process. An
   external program runs in a child process; if it called `chdir()` it would change
   only the child's directory, and the child exits right after. The shell's own
   directory would stay the same, so `cd` would do nothing.
2. My PATH has 19 directories (count them on Figure 1.3). `ls` is in `/usr/bin`
   (`/bin` is only a symlink to `usr/bin` on Arch, and `/bin` is not in PATH).
3. 127 – the shell's code for "command not found".
4. `-t` sorts by modification time, newest first (`ls -lt`; add `-r` for oldest first).

---

## Part 2. What the shell does to your line

Files: `a.c  'alfa beta.txt'  b.c  file1.txt  file2.txt  notes.txt`

| Command | My prediction | Real output | Explanation (if different) |
|---|---|---|---|
| `echo *.c` | `a.c b.c` | `a.c b.c` | – |
| `echo file?.txt` | `file1.txt file2.txt` | `file1.txt file2.txt` | – |
| `echo [a-b]*` | `a.c b.c` | `a.c alfa beta.txt b.c` | `[a-b]*` = any name starting with a or b, and `alfa beta.txt` starts with a; it is one file, echo just prints the space |
| `echo *.xyz` | (empty line) | `*.xyz` | when a pattern matches nothing, bash leaves it unchanged |
| `ls alfa beta.txt` | error | `ls: cannot access 'alfa': No such file or directory` and the same for `'beta.txt'` | the space splits it into two arguments |
| `ls "alfa beta.txt"` | `alfa beta.txt` | `'alfa beta.txt'` | ls adds quotes when printing to a terminal (see note) |
| `echo '$HOME' "$HOME" \$HOME` | `$HOME /home/nnorian $HOME` | `$HOME /home/nnorian $HOME` | – |
| `echo {1..5} ; echo $((7 * 6))` | `1 2 3 4 5` / `42` | `1 2 3 4 5` / `42` | – |
| `echo "today is $(date +%A), I am $(whoami)"` | `today is Monday, I am nnorian` | `today is Monday, I am nnorian` | – |
| `touch log{1..3}.txt && ls log*` | `log1.txt log2.txt log3.txt` | `log1.txt log2.txt log3.txt` | – |

[Figure 2.1 – Files created in the play directory]
[Figure 2.2 – Globbing results]
[Figure 2.3 – The ls error on a name with a space, and the fix]
[Figure 2.4 – Single quotes, double quotes and backslash]

**Observe**

2. The shell. Before running `echo`, bash expands `*.c` into the matching file names
   (globbing) and passes them as separate arguments. `echo` only receives
   `a.c` and `b.c`; it never sees the `*`.
3. Without quotes the shell splits the line on spaces, so `ls` got two arguments,
   `alfa` and `beta.txt`, and neither file exists. Fixes: quotes
   (`ls "alfa beta.txt"` or `ls 'alfa beta.txt'`) or a backslash before the space
   (`ls alfa\ beta.txt`). Tab completion writes the backslash for you.
4. Single quotes keep everything literally: no `$` variables, no `$( )`, no
   backslash escapes. Double quotes still expand `$VAR`, `$(cmd)` and `$((...))`, but
   stop word splitting and globbing. A backslash quotes only the next character.

Note: when the output is a terminal, GNU ls (coreutils 8.25+) prints names that
contain spaces or special characters in quotes (`QUOTING_STYLE=shell-escape`), so
you can copy the name and paste it back into the shell as one argument. When the
output goes to a pipe or a file (`ls | cat`) there are no quotes.

---

## Part 3. Redirection, chaining and jobs

```
$ ls /etc /nope > out.txt 2> err.txt
$ wc -l out.txt ; cat err.txt
204 out.txt
ls: cannot access '/nope': No such file or directory
$ ls /etc /nope > all.txt 2>&1 ; tail -2 all.txt
xdg
xml
$ echo first > notes.md ; echo second >> notes.md ; cat notes.md
first
second
$ wc -l < access.log
500
$ ls /nope 2> /dev/null || echo "failed with code $?"
failed with code 2
$ mkdir -p tmp && cd tmp && echo "I am in $(pwd)" ; cd ~/os-lab3
I am in /home/nnorian/os-lab3/tmp
$ sleep 100 &
[1] 39515
$ jobs ; kill %1 ; jobs
[1]+  Running                 sleep 100 &
[1]+  Terminated              sleep 100
```

Order of redirections:

```
$ ls /etc /nope > all.txt 2>&1 ; wc -l all.txt
205 all.txt                  <- listing + error line, all in the file
$ ls /etc /nope 2>&1 > all.txt ; wc -l all.txt
ls: cannot access '/nope': No such file or directory     <- on the screen
204 all.txt                  <- only the listing
```

[Figure 3.1 – stdout and stderr in separate files]
[Figure 3.2 – Comparing the order of redirections]
[Figure 3.3 – The ; && || operators]
[Figure 3.4 – Running a background job and stopping it]

**Observe**

1. `err.txt` got the error message (stderr, fd 2); `out.txt` got the listing of
   `/etc` (stdout, fd 1), 204 lines (the `/etc:` header plus 203 names).
2. Redirections are done left to right, and `2>&1` means "make fd 2 point to where
   fd 1 points *now*".
   `> all.txt 2>&1`: first fd 1 → all.txt, then fd 2 → the same file, so both go into
   the file (205 lines).
   `2>&1 > all.txt`: first fd 2 → where fd 1 is now (the terminal), then only fd 1
   is moved to all.txt. The error stays on the screen and the file has 204 lines.
3. With `wc -l access.log`, wc opens the file itself, so it knows the name and prints
   it. With `wc -l < access.log`, the shell opens the file and connects it to wc's
   stdin; wc only reads data from fd 0 and has no file name to print.
4. `;` runs the next command always, no matter how the first ended.
   `&&` runs the next command only if the previous one succeeded (exit code 0).
   `||` runs the next command only if the previous one failed (exit code ≠ 0).

---

## Part 4. Answer questions with one-liners

Each line of access.log: `date time user result page`, fields separated by one
space, so `cut -d' '` works: f2 = time, f3 = user, f4 = OK/FAIL, f5 = page.

| Q | One-liner | Answer |
|---|---|---|
| Q1 | `grep -c FAIL access.log` | **100** |
| Q2 | `cut -d' ' -f5 access.log \| sort -u \| nl` | **4** pages: /page0, /page1, /page2, /page3 |
| Q3 | `cut -d' ' -f5 access.log \| sort \| uniq -c` | 125 each (125 × 4 = 500 ✓) |
| Q4 | `grep FAIL access.log \| cut -d' ' -f3 \| sort \| uniq -c \| sort -rn \| awk 'NR==1{max=$1} $1==max'` | **user3 and user5**, 15 failures each |
| Q5 | `echo "user3: $(grep -cw user3 access.log) requests, $(grep -w user3 access.log \| grep -c FAIL) failed"` | **72 requests, 15 failed** |
| Q6 | `grep FAIL access.log \| tail -3 \| cut -d' ' -f2,3` | `10:10 user0`, `10:15 user5`, `10:20 user3` |
| Q7 | `cut -d: -f7 /etc/passwd \| sort \| uniq -c \| sort -rn` | 39 `/usr/bin/nologin`, 3 `/usr/bin/bash`, 1 `/usr/bin/git-shell` |
| Q8 | `ls /etc \| wc -l ; ls -l /etc \| wc -l` | 203 and 204 |

Check: per-user failures (full list behind Q4) are 15 user3, 15 user5 and 14 for
each of user0, user1, user2, user4, user6 → 15 + 15 + 5 × 14 = 100 = Q1 ✓.
Q3 adds up to 500 ✓.

Q4 needs "user **or users**": there is a tie, so `head -1` would hide user5. The awk
at the end remembers the first (biggest) count and prints every line with that count.

Q8 – prediction: they differ by one because of a header line. Explanation: `ls -l`
prints a first line `total 1234` (disk blocks used by the listed files), then one
line per entry; plain `ls` prints only the names. When its output is a pipe, `ls`
also puts one name per line (instead of columns), so the counts are 203 names vs
203 names + 1 "total" line.

**Bonus** – minute ends in 0 and request failed:

```
grep -E '10:[0-5]0 user[0-9]+ FAIL' access.log
grep -cE '10:[0-5]0 user[0-9]+ FAIL' access.log      ->  50
```

This line is also the last entry of `oneliners.sh`.
`[0-5]0` matches only minutes 00, 10, …, 50; `user[0-9]+ FAIL` makes sure the
result field right after the user is FAIL. 50 lines (every 10th request, since
minute = i mod 60 ends in 0 exactly when i is a multiple of 10, and those are
also multiples of 5, i.e. FAIL).

[Figure 4.1 – Number of failed requests]
[Figure 4.2 – Different pages requested]
[Figure 4.3 – Requests per page]
[Figure 4.4 – Users with the most failed requests]
[Figure 4.5 – Requests and failures of user3]
[Figure 4.6 – Last 3 failed requests]
[Figure 4.7 – Login shells in /etc/passwd]
[Figure 4.8 – ls vs ls -l line count]
[Figure 4.9 – Full run of oneliners.sh]
[Figure 4.10 – Bonus: grep -E]

---

## Part 5. Your first script

TODO completed in `report.sh`:

```bash
for n in 0 1 2 3 4 5 6; do
  echo "user$n: $(grep -c "user$n FAIL" "$log") failures"
done
```

```
$ ./report.sh access.log
500 requests, 100 failed
user0: 14 failures
user1: 14 failures
user2: 14 failures
user3: 15 failures
user4: 14 failures
user5: 15 failures
user6: 14 failures
$ ./report.sh nope.log ; echo "exit code: $?"
no such file: nope.log
exit code: 1
```

[Figure 5.1 – Contents of report.sh]
[Figure 5.2 – Running the script on access.log]
[Figure 5.3 – Error handling for a missing file]

**Observe**

1. `#!/bin/bash` (the shebang) tells the kernel which interpreter runs the file:
   `./report.sh access.log` is executed as `/bin/bash ./report.sh access.log`.
   Before `chmod +x` the file has no execute permission, so the shell answers
   `bash: ./report.sh: Permission denied` (exit code 126). `bash report.sh` would
   still work, because then bash is the program and the file is only read.
2. A name without `/` is looked up only in the PATH directories, and the current
   directory `.` is not in PATH (so a file called `ls` in some folder cannot replace
   the real `ls`). `report.sh` → `command not found`. `./report.sh` contains a `/`, so
   the shell uses that path directly and skips the PATH search.
3. `>&2` sends the message to stderr, so it still reaches the screen when stdout is
   redirected to a file or a pipe, and it does not mix with the real output.
   `exit 1` gives a non-zero exit code so the caller (`$?`, `&&`, `||`, another
   script, cron) knows the script failed; 0 means success.

**Bonus – cron**

```
* * * * * /home/nnorian/os-lab3/report.sh /home/nnorian/os-lab3/access.log >> /home/nnorian/os-lab3/report.log 2>&1
```

`* * * * *` = every minute of every hour, every day. Full paths are needed because
cron runs the job with a minimal environment (PATH is only `/usr/bin:/bin`) and starts
it in the home directory, not in `~/os-lab3`. `>>` appends, so every run adds to the
log instead of overwriting it, and `2>&1` sends errors into the same file; cron has no
terminal, so without it any error message would be lost. Arch does not ship cron, so I
installed `cronie` and started it with `systemctl enable --now cronie`.

Every minute the script added one block of 8 lines to report.log:

```
[paste the output of: cat ~/os-lab3/report.log]
```

After the test I removed the job with `crontab -r`; `crontab -l` then printed
`no crontab for nnorian`.

[Figure 5.5 – The crontab line]
[Figure 5.6 – report.log after two minutes, and removing the job]

---

## Conclusions

The shell does a lot of work before a program starts: it splits the line into
words, expands `*`, `?`, `[ ]`, `{ }`, `$VAR`, `$( )` and `$(( ))`, removes quotes,
sets up redirections and finds the program in PATH. The program only receives the
final list of arguments. Knowing this order explains the surprises in Part 2
(`echo *.xyz`, the file with a space) and in Part 3 (`2>&1 > file`). Every process
has stdin, stdout and stderr, and redirection only changes where these file
descriptors point. Small filters (grep, cut, sort, uniq, wc, head, tail) joined by
pipes answered every question about the log in one line, and the same lines became
a script with argument checks, a correct exit code and errors on stderr.
