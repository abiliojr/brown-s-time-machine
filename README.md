# Doc Brown's time machine

Doc Brown's time machine is yet another rsync powered backup script. This one needs almost nothing to run. It allows the final user to build his/her own backup system using things like cron.

Whenever this script is called, it will create a new directory in the localdir (see bellow) with the timestamp in format year, month, day, hour, minutes and seconds. After that, it will proceed to remove old backups, if needed, as ordered in the parameters (see bellow).

New snapshots are created using links to the previous backups, so only the files that changed will use extra disk space. They are hard links, so there will be no data loss when old backups are removed.

In case there is any problem during the file transfer, the program will abort, removing the unfinished backup dir.

# Installation
Just put the script wherever you want. You must have bash, bc and rsync in order to use it.

# Usage
Call it like ```dbtm remotedir localdir [snapshots] [rsyncextraparams] [prefix]```

Parameters __remotedir__ and __localdir__ follow the rsync notation. For example:
```rbackup@doc.brown.server:/usr/local/www/wordpress backups/website``` will access the _doc.brown.server_ and backup the contents of his wordpress site into the directory _backups/website_.

In the case you want to back up several remote directories at once, you can pass them between quotes (as a single parameter), in the _rsync_ style:
```"rbackup@doc.brown.server:~/crazy_ideas.txt :/usr/local/www/wordpress"``` will backup the file named _crazy_ideas.txt_ in addition to his website.

__Snapshots__ will tell the program how many snapshots to keep. After the number is reached, older snapshots are automatically erased. You'll probably want to set it to _"smart"_ (set by default) in order to emulate Apple Time Machine behavior. In this mode it will keep:
- __All__ backups with __less than 24 hours__.
- An __hourly__ backup for the __first week__.
- A __daily__ backup for the __first month__.
- A __weekly__ backup for the __first year__.
- A __monthly__ backup __afterwards__.

__Rsyncextraparams__ can be used to pass extra parameters to _rsync_, for example:
```"-e 'ssh -i keys/id_rsa' --bwlimit=200 --exclude-from='wp_skip.lst'"```, will let the use of a specific _RSA key_, limit the bandwidth to 200 Kb/s, and use the patterns contained in the local file _wp_skip.lst_ to be excluded from the transfer.

__Prefix__ can be set to any string to mark a group of snapshots like belonging to the same backup. This can be useful if you're backing up several things inside the same directory.  

# Notes
This script started his life a few years ago as a quick hack. In it's different versions, it has been used in dozens of situations. With time, things like _Time Machine_ emulation were added. I encourage you to contact me in case you used it and have suggestions or bug reports.

The work here contained is published under the Simplified BSD License. Read LICENSE for more information.

Time Machine is a trademark of Apple Inc.