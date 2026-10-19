# Lab 3 – commands for every screenshot

Run each block in a normal terminal (not inside an editor). `clear` first so the
screenshot holds only that figure. Lines starting with `#` are instructions, not commands.

## One-time setup

```bash
# man is not installed on this laptop yet (needed for Part 1, step 4)
sudo pacman -S --needed man-db man-pages
sudo mandb
```

`~/os-lab3/access.log` is already generated (500 lines). `oneliners.sh` and
`report.sh` are already copied into `~/os-lab3`.

Do not keep your own notes in `~/os-lab3/notes.md`: Part 3 overwrites that file
with `echo first > notes.md`.

---

## Part 1

**Figure 1.1 – Tab completion**
```bash
clear
# type:  ls /usr/sh   then press Tab twice
#   1st Tab completes to /usr/share/ (the only match), 2nd Tab lists its contents
#   ("Display all 275 possibilities? (y or n)" -> press y or n)
# press Ctrl+C, then type:  cd /et   and press Tab once  -> becomes  cd /etc/
```

**Ctrl+R (no figure required, answer in report)**
```bash
date
whoami
uname -r
# press Ctrl+R, type  dat , press Enter  -> date runs again
```

**Figure 1.2 – Output of type and which**
```bash
clear
type cd ; type ls ; type echo ; which ls
```

**Figure 1.3 – The directories in PATH**
```bash
clear
echo $PATH
echo $PATH | tr ':' '\n'
echo $PATH | tr ':' '\n' | wc -l
```

**Figure 1.4 – Exit code of a missing command**
```bash
clear
nosuchprog ; echo "exit code: $?"
```

**Help (optional extra figure 1.5 for Observe 4)**
```bash
man ls          # type  /sort  Enter, then n, n ... ; find "-t" ; q to quit
clear
ls --help | head
man -k "list directory"
help cd
```

---

## Part 2

**Figure 2.1 – Files created in the play directory**
```bash
mkdir -p ~/os-lab3/play && cd ~/os-lab3/play
touch a.c b.c file1.txt file2.txt notes.txt "alfa beta.txt"
clear
ls
```

**Figure 2.2 – Globbing results**
```bash
clear
echo *.c
echo file?.txt
echo [a-b]*
echo *.xyz
```

**Figure 2.3 – The ls error on a name with a space, and the fix**
```bash
clear
ls alfa beta.txt
ls "alfa beta.txt"
ls alfa\ beta.txt
```

**Figure 2.4 – Single quotes, double quotes and backslash**
```bash
clear
echo '$HOME' "$HOME" \$HOME
```

**Figure 2.5 (extra) – Brace expansion, arithmetic, command substitution**
```bash
clear
echo {1..5} ; echo $((7 * 6))
echo "today is $(date +%A), I am $(whoami)"
touch log{1..3}.txt && ls log*
```

---

## Part 3

**Figure 3.1 – stdout and stderr in separate files**
```bash
cd ~/os-lab3
clear
ls /etc /nope > out.txt 2> err.txt
wc -l out.txt ; cat err.txt
```

**Figure 3.2 – Comparing the order of redirections**
```bash
clear
ls /etc /nope > all.txt 2>&1 ; wc -l all.txt ; head -2 all.txt ; tail -2 all.txt
echo ----------
ls /etc /nope 2>&1 > all.txt ; wc -l all.txt ; head -2 all.txt
```

**Figure 3.3 – The ; && || operators**
```bash
clear
echo first > notes.md ; echo second >> notes.md ; cat notes.md
wc -l < access.log ; wc -l access.log
ls /nope 2> /dev/null || echo "failed with code $?"
mkdir -p tmp && cd tmp && echo "I am in $(pwd)" ; cd ~/os-lab3
```

**Figure 3.4 – Running a background job and stopping it**
```bash
clear
sleep 100 &
jobs ; kill %1 ; jobs
# if the 2nd jobs still says Running, press Enter once and type  jobs  again
```

---

## Part 4

All in `~/os-lab3`.

**Figure 4.1 – Number of failed requests**
```bash
cd ~/os-lab3
clear
grep -c FAIL access.log
```

**Figure 4.2 – Different pages requested**
```bash
clear
cut -d' ' -f5 access.log | sort -u | nl
```

**Figure 4.3 – Requests per page**
```bash
clear
cut -d' ' -f5 access.log | sort | uniq -c
```

**Figure 4.4 – Users with the most failed requests**
```bash
clear
grep FAIL access.log | cut -d' ' -f3 | sort | uniq -c | sort -rn
grep FAIL access.log | cut -d' ' -f3 | sort | uniq -c | sort -rn | awk 'NR==1{max=$1} $1==max'
```
(first line = the full list, so the check "adds up to 100" is visible; second line = the one-liner from oneliners.sh)

**Figure 4.5 – Requests and failures of user3**
```bash
clear
echo "user3: $(grep -cw user3 access.log) requests, $(grep -w user3 access.log | grep -c FAIL) failed"
```

**Figure 4.6 – Last 3 failed requests (time and user)**
```bash
clear
grep FAIL access.log | tail -3 | cut -d' ' -f2,3
```

**Figure 4.7 – Login shells in /etc/passwd**
```bash
clear
cut -d: -f7 /etc/passwd | sort | uniq -c | sort -rn
```

**Figure 4.8 – ls vs ls -l line count**
```bash
clear
ls /etc | wc -l ; ls -l /etc | wc -l
ls -l /etc | head -2
```

**Figure 4.9 – Full run of oneliners.sh**
```bash
clear
cat oneliners.sh
```
then the run (71 lines because of the bonus, so zoom out with Ctrl+- or take two screenshots):
```bash
clear
bash oneliners.sh
```

**Figure 4.10 (bonus) – Minute ends in 0 and FAIL**
```bash
clear
grep -E '10:[0-5]0 user[0-9]+ FAIL' access.log | head -5
grep -cE '10:[0-5]0 user[0-9]+ FAIL' access.log
```

---

## Part 5

**Figure 5.1 – Contents of report.sh**
```bash
cd ~/os-lab3
clear
cat -n report.sh
```

**Figure 5.4 (extra, for Observe 1 and 2) – before chmod +x, and without ./**
```bash
clear
ls -l report.sh
./report.sh access.log        # Permission denied (no x bit yet)
report.sh access.log          # command not found (. is not in PATH)
```

**Figure 5.2 – Running the script on access.log**
```bash
clear
chmod +x report.sh
ls -l report.sh
./report.sh access.log
```

**Figure 5.3 – Error handling for a missing file**
```bash
clear
./report.sh nope.log ; echo "exit code: $?"
```

**Bonus – cron (Arch has no cron by default)**

1. Install and start cron (asks for your password):
```bash
sudo pacman -S --needed cronie
sudo systemctl enable --now cronie
systemctl is-active cronie            # must print: active
```
2. The script must be executable, or cron logs "Permission denied":
```bash
chmod +x ~/os-lab3/report.sh
rm -f ~/os-lab3/report.log
```
3. Add the line (EDITOR is not set, so pick nano explicitly):
```bash
EDITOR=nano crontab -e
```
paste this as the only line, then Ctrl+O, Enter, Ctrl+X:
```
* * * * * /home/nnorian/os-lab3/report.sh /home/nnorian/os-lab3/access.log >> /home/nnorian/os-lab3/report.log 2>&1
```

**Figure 5.5 – The crontab line**
```bash
clear
crontab -l
date
```

4. Wait at least 2 minutes (cron fires at second :00 of every minute).

**Figure 5.6 – report.log after two minutes, and removing the job**
```bash
clear
date
cat ~/os-lab3/report.log
crontab -r            # removes the whole crontab (there is nothing else in it)
crontab -l            # prints: no crontab for nnorian
```
Each run appends 8 lines (`500 requests, 100 failed` + user0…user6), so after 2 minutes the log has 16 or more lines.

Optional, to stop cron completely afterwards: `sudo systemctl disable --now cronie`.

---

## Upload

```bash
cd ~/uni/os/os-course
cp ~/os-lab3/oneliners.sh ~/os-lab3/report.sh lab3/   # only if you edited them in ~/os-lab3
# put Lab3_NameSurname_Group.docx into lab3/
git add lab3 && git commit -m "Lab 3" && git push
```
