
---
layout: post
title: Run programs in the background in Linux
show-avatar: true
tags:
  - blogs
  - linux
  - background
published: true
---


# Run programs in the background

It is an important function to run programs in the background in linux especially in cluster. 

## Using 'nohup':

### Run in the background
```
nohup application > output.txt &

exit
```

### Check jobs in the background
```
jobs -l
```

### Take to the foreground
```
fg 
```

### Kill a job
```
kill
```

## Using 'screen':

Run the screen command to start a new “screen”. Optionally, include the -S option to give it a name. 
```
$ screen -S mycommand
```

In the new screen session, execute the command or script you wish to put in the background.
```
$ /path/to/myscript.sh
```

Press Ctrl + A on your keyboard, and then D. This will detach the screen, then you can close the terminal, logout of your SSH session, etc, and the screen will persist. To see a list of screens, use this command.
```
$ screen -ls
```

To reattach to a screen, use the following command, substituting the number below with the process ID of your own. 
```
$ screen -r 2741
```
