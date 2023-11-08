# Level 21 → Level 22

## Level Goal
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

## Commands you may need to solve this level
cron, crontab, crontab(5) (use “man 5 crontab” to access this)

## Solution
```
ssh bandit21@bandit.labs.overthewire.org -p 2220
```
```
NvEJF7oVjkddltPSrdKEFOllh9V1IBcq
```
```
ls /etc/cron.d/
```
```
cat /etc/cron.d/cronjob_bandit22
```
```
cat /usr/bin/cronjob_bandit22.sh
```
```
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

## Password for the next level
```
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
```
