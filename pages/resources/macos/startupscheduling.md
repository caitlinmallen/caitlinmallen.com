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

## Persistence via KEXT

- Kernal Extension file
	- Allow the kernel to communicate with hardware
	- /System/Library/Extensions
		- KEXT files built into the OS X operating system
	- /Library/Extensions
		- 3<sup>rd</sup> party KEXT files
- Advanced malware may not even bother to use a launch daemon or agent if they have root access
	- Would instead maybe use a KEXT file
- Attackers can build a KEXT in advanced and move to the victim system
- Attackers can also use a KEXT file and launch daemon or agent for a backdoor
	- Keyloggers can be set up via a KEXT module
	- C2 server is communicated with via a launch daemon
- Keep an eye out for KEXT files that do not exist in those directories
	- Starting in Yosemite, you can no longer place unsigned KEXT files in either startup location
- KEXT files are bundles or folders that Finder will treat as one file
	- KEXT bundles can contain the following:
		- Information property list (info.plist)
			- Holds settings and requirements related to KEXT
		- KEXT binary
			- Binary that the KEXT will be responsible for executing
			- Mach-O format
		- Resources
			- Icons or other items that might be packaged with the driver if it needs to display a menu
		- KEXT bundles
			- Allows for plugins or other KEXTs that it is dependent on

  
### KEXT Commands

- List currently loaded KEXT files with the [kextstat](https://ss64.com/mac/kextstat.html) command

  
![enter image description here](https://user-images.githubusercontent.com/13186156/191578318-c8f14856-dae0-4745-bacf-96d3be112f1e.png)


- Loaded or unloaded via kextunload and kextload
	- Ex. _sudo kextunload /path/to/file_
	  - Can also use the CFBundle name
	- Ex. sudo kextload /path/to/file
- [codesign](https://www.unix.com/man_page/osx/1/codesign/) can be ran on KEXT bundles
	- Will tell you what KEXTs are signed and who signed them
