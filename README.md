# S3 Sync (s3_sync) #

## Description ##

S3 Sync is a bash script that will setup automatic syncing between your local directory and Amazon S3, utilizing the inotify linux kernel utility and s3cmd.  It can also sync from your S3 bucket to a local directory via cron.

After failing to effectively get File Conveyor working due to repeated python dependency issues and lack of documentation, I wrote this simple bash script to take advantage of the utilities available in almost all Linux distributons.

Unlike File Conveyor this script does not perform any image compression, css compression, or any other file manipulation, it just moves files back and forth from S3.

Big shout out to osse on irce.freenode.net #bash for the assist on my config files loop.

## Requirements ##
s3cmd -  which is available in most package repositories
kernel version 2.6 or above

## Install ##
Download the package
within the s3_sync directory run 
```
#!bash

chmod +x s3_sync && ./s3_sync install
```


This performs the following actions
It will create your configuration directory at /etc/s3_sync/
Inside that directory will be an example configuration file.

You can copy that file to any number of configuration files
All files must end with a .conf extension to be read.

Install will also walk you through s3cmd configuration.  S3 sync utilizes its own configuration file found at /etc/s3_sync/.s3cfg

s3_sync is copied to /usr/sbin

## Operation ##

When install is complete and you have setup at least one configuration, you can run 
```
#!bash

s3_sync start
```


To stop all your s3_syncs you can run
```
#!bash

 s3_sync stop
```

Warning: if you change a config before issuing s3_sync stop you can be left with rogue sync processes.  This script is not tracking pids, and if you have another inotifywait function running on same exact folder s3_sync stop will stop them as well.
If you have rogue processes you can find them with 
```
#!bash

ps -ax|grep 'inotifywait'.
```


To restart you can run
```
#!bash

 s3_sync restart
```


## Disclaimer ##
This script is still in beta and has not been tested on all distributions.
If you choose to utilize the syncdown functionality (Amazon S3 -> your directory) you do run the risk of having files automatically deleted from your filesystem.