# 🔍 AI Server Detective Lab

## Your Mission

You've just joined a major AI research lab as a junior engineer. There's been some strange behavior on the training servers, and your team leader has asked you to investigate. Your detective work will help identify what's been happening with the ML models and who's been accessing the servers.

## What You'll Learn

- How to investigate server logs like a pro engineer
- Commands that save hours of manual work
- Skills used daily by AI/ML engineers

## Setup Your Investigation Hub

1. First, create your investigation workspace:
	- Create a directory called `detective_lab`
	- Enter into that directory
2. Gather the Evidence
	We'll download two critical log files using `curl`:

```bash

# Template for downloading:

curl -o <local_file_name> <remote_url>

```

You need these files:
- Server activity logs:
```

https://raw.githubusercontent.com/eliza-guseva/ai-eng-nbs-public/refs/heads/master/ai-eng-nbs-1-master/detective_lab/activity.txt

```
- ML training logs:
```

https://raw.githubusercontent.com/eliza-guseva/ai-eng-nbs-public/refs/heads/master/ai-eng-nbs-1-master/detective_lab/logs.txt

```

## Your Investigation Tasks

1. Survey the Scene
	- Check what files you've collected
	- Examine the contents of each file
	- Count how many log entries you're dealing with
2. Track Down Problems
	- Find all ERROR messages
	- Count the total number of errors
	- Save all errors to a separate file for your report
3. User Activity Analysis
	- Find who logged in last. 
	  - Note, you can use `grep` to find lines with 'logged in'
	  - You can use sort to order the file. To sort in reverse, do `sort -r`. You need to sort by time. sort automatically splits lines by empty space and calls the element fields. Time is the 6th element. So you can also use `sort -k6`, meaning sort based on the string starting with 6th field.
	  - Try combining these commands using the pipe operator (|)!
	- Combine the log files for a complete timeline
	- Create a list of all unique users who accessed the system
	 - Hint: Use cut -d' ' -f2 to get just the user names 
	   - The cut command is like taking scissors to each line of text and keeping just the piece you want.
	   - -d' ' means "cut at spaces"
	   - -f2 means "give me piece number 2"
	- In order to get unique users, you can "pipe" the output through `uniq`, it returns only unique lines
4. Secure the Evidence
	- Create a backup of all your files

## Pro Tips

- These might look like small files, but the commands you're learning work the same way on massive server logs
- Real AI engineers use these exact techniques to debug training issues
- Each command you learn is like adding a new tool to your detective toolkit

  

Need help? Check the reference notebook: https://github.com/AI-Engineering-bootcamp/ai-eng-nbs-public/blob/master/bash.ipynb

  

Good luck, detective! 🕵️‍♂️