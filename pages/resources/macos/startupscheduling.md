# Additional System Startup & Scheduling Methods

## Crontab

- Popular Unix tool for executing scheduled tasks
  - OS X still allows the use of cron for scheduling
    - Not frequently abused due to the ease of visibility into the cronjob
    - If you go to edit your cronjob, you will see the malicious one as well
      - crontab -e allows you to edit your scheduled tasks
      - crontab -l prints scheduled tasks
- Files stored in plaintext
  - Stored in /usr/lib/cron/tabs
- Cron is enabled by default on OS X

![cron format](https://tecadmin.net/wp-content/uploads/2013/03/crontab-2.png)

- Some built in features include:
  - @reboot /path/to/script -> Will run script at every system startup
  - @yearly /path/to/script -> Will run script at the first minute of every year
  - @monthly /path/to/script -> Will run script 00:00 on the 1<sup>st</sup> of every month
  - @daily /path/to/script -> Will run a daily log file cleanup using the cleanup-logs shell script at 00:00 each day
- When collecting cron data, ensure you are dumping the user and root user crontab
