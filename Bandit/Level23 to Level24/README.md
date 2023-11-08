# Level 23 → Level 24

## Level Goal
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

## Commands you may need to solve this level
cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Solution
```
ssh bandit23@bandit.labs.overthewire.org -p 2220
```
```
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G
```
```
ls /etc/cron.d
```
```
cat /etc/cron.d/cronjob_bandit24
```
```
cat /usr/bin/cronjob_bandit24.sh
```
```
vim /var/spool/bandit24/foo/myScript.sh
```
Copy & Paste the next script.
```sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/bandit24_password
```
```
chmod +x /var/spool/bandit24/foo/myScript.sh
```
After few seconds..
```
cat /tmp/bandit24_password
```

## Password for the next level
```
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar
```
