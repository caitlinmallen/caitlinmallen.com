---
layout: page 
title: Property Lists & Launch Agents
permalink: /resources/macos
---
## Property List Breakdown

- File containing XML content
- Example Property List - [helloworld.plist](https://github.com/caitlinmallen/osxir/blob/main/misc/helloworld.plist)
- Label key is a unique identifier
  - Does not need to match property list file name
- **ProgramArguments key**
  - Developer provides the command or program they want to run along with necessary arguments
- **StartInterval key**
  - Optional in this example, but if used, will launch the program every specified number of seconds
- **RunAtLoad key**
  - Not used in example
  - When used and set to true, it ensures the program is executed at system startup rather than waiting for StartInterval
- A 'malicious' property list example - [iphotoHelperModule.plist](https://github.com/caitlinmallen/osxir/blob/main/misc/iphotoHelperModule.plist)
  - Starts iphotoHelper
  - Checks every 800 seconds to see if iphotoHelper is still running
    - If not running, will start it again
    - Persistence mechanism if their original backdoor is killed
- Malicious actors may make two property lists
  - One executes the backdoor
  - The other will check and see if the backdoor is still running
- **KeepAlive Key**
  - Important to malware actors who value a constant connection
  - **_SuccessfulExit_**
    - Boolean
    - When true, will restart program only if exited unsuccessfully
      - Ex. A force quit or crash
    - When false, the backdoor will re-execute immediately if it is found and/or
- Property Lists can be edited using [defaults](https://ss64.com/mac/defaults.html)
  - Write and delete switches

## Binary Property Lists

- Binary Property Lists (bplists) are property lists saved in binary format
  - Smaller file size than traditional XML property lists
  - Don't display in plain text
- Can be converted to XML using [plutil](https://theapplewiki.com/wiki/Plutil) and [PlistBuddy](https://www.unix.com/man_page/osx/8/plistbuddy/)
  - **_/usr/libexec/PlistBuddy -c &lt;target_file.plist&gt;_**
- Can also be read and printed using the '[defaults](https://ss64.com/mac/defaults.html) read' command

## Launchctl

- Plists being put in a startup location does not immediately load it by launchctl
  - Wait until next reboot (or login for user-based agents)
  - Force a load via '[launchctl](https://ss64.com/mac/launchctl.html) load'
    - **_launchctl load &lt;target_file.plist&gt;_**
  - Unload with launchctl unload
    - **_launchctl unload &lt;target_file.plist&gt;_**
    - Will kill process it points to
- Files do not need a .plist extension to be loaded by launchctl
  - Need to use the -F switch in launchctl to force the load
    - **_launchctl load -F &lt;target_file&gt;_**
    - Will not execute when restarted
- Listing active property lists
  - **_launchctl list_**
  - Results will show launch agents and daemons that are currently loaded
  - May be different depending on which user is running the command
    - Need to use [su](https://man7.org/linux/man-pages/man1/su.1.html) to change users and run as each user separately
  - Results
    - _PID_
      - If a dash, process is not currently active
    - _Label_
      - What software is being interacted with
    - _Status_
      - Exit code
      - 0 means successful execution

## Property List Overrides

- If a user wants to stop a plist from loading at startup but not fully delete it
  - Move the property list to a different directory
  - Keep plist file in same location and create an entry in the launchd override file
    - User override file
      - _/var/db/launchd.db/com.apple.launchd.peruser.&lt;uid&gt;/overrides.plist_
    - Root override file
      - _/var/db/launch.db/com.apple.launchd/overrides.plist_
    - Keeps plists in startup directories from being loaded
- Can force a load with the -F switch if needed
  - - **_launchctl load -F /System/Library/LaunchDaemons/com.apple.smbd.plist_**
- To ensure it runs at every reboot
  - - **_launchctl load -w /System/Library/LaunchDaemons/com.apple.smbd.plist_**
