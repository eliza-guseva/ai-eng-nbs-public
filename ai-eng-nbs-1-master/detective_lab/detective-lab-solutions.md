# AI Server Detective Lab - Solutions Guide

## Setup Solutions
1. `mkdir detective_lab`
2. `cd detective_lab`
3. Download commands:
```bash
curl -o activity.txt https://raw.githubusercontent.com/eliza-guseva/ai-eng-nbs-public/refs/heads/master/ai-eng-nbs-1-master/detective_lab/activity.txt
curl -o logs.txt https://raw.githubusercontent.com/eliza-guseva/ai-eng-nbs-public/refs/heads/master/ai-eng-nbs-1-master/detective_lab/logs.txt
```

## Investigation Tasks Solutions
4. `ls -l`
5. `cat activity.txt` and `cat logs.txt`
6. `wc -l activity.txt logs.txt`
7. `grep "ERROR" logs.txt`
8. `grep -c "ERROR" logs.txt`
9. `grep "ERROR" logs.txt > error_report.txt`
10. `grep "logged in" activity.txt | sort -r -k5 | head -n 1`
11. `cat logs.txt activity.txt > combined_logs.txt`
12. `grep "User" activity.txt | cut -d' ' -f2 | sort | uniq`
13. `mkdir backup && cp *.txt backup/`

## Notes
- The pipe (`|`) operator 
```
Think of | as a pipe connecting two commands - the output of the first command "flows" into the second command as input. 

For example in: sort activity.txt | head -n 1
- First, sort activity.txt puts all lines in alphabetical order
- Then that sorted text "flows through the pipe" to head -n 1
- head -n 1 takes that sorted text and shows just the first line
```
- The `cut` command for finding unique users
```
In: grep "User" activity.txt | cut -d' ' -f2 | sort | uniq

This long command breaks down into steps:
1. grep "User" activity.txt  -> finds all lines with "User"
2. cut -d' ' -f2            -> splits each line at spaces and takes 2nd piece
   (-d' ' means "split at spaces", -f2 means "take field #2")
3. sort                     -> puts usernames in order
4. uniq                     -> removes duplicates

So if you had lines like:
"User alice logged in"
"User bob logged in"
"User alice logged in"

It would give you just:
alice
bob
```
- Real-world usage example
```
Instead of opening each log file and scrolling through thousands of lines to count errors, 
grep -c "ERROR" logs.txt instantly tells you the count!

Imagine doing this manually on a real server with millions of log lines - these commands save hours of work.
```
- For large files doing something like that is so much faster this is than manual inspection
