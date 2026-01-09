---
layout: page 
title: Browser Analysis - Safari
permalink: /resources/macos/browsers/safari
---

- Default browser built into OS X

	- Integrated syncing across OS X and Ios DEVICES

	- Newer Safari versions store history in a sqlite3 database

- Timestamps are stored in seconds since 00:00:00 Jan 1, 2001 UTC

  

### Safari History plist

- Older version of Safari hold browser history as a binary in a plist

	- _/Users/\$user/Library/Safari/History.plist_

- **WebHistoryDomains.v\* key**

	- Dictionary of domains that have been visited

	- Number of times a domain has been visited

- **WebHistoryDates key**

	- URLs visited

	- Last visited dates

	- Redirect URLs

	- Page Title

- plist only has a single timestamp called '**lastVisitedDate**'

	- Written over each time a user revisits a URL

- Recommended to use the Safari History.db file over the plist

	- Sometimes both files exist if a user upgraded to a newer Safari version and never cleared their history

  

### Safari History Database

- More verbose timeline than the plist file

	- _/Users/\$user/Library/Safari/History.db_

- Contains the following tables

	- **History_items**

	- **History_tombstones**

	- **History_visits**

	- **Metdata**

- Timelines can be built from history_visits and history_items

	- _History_visits_

		- Holds an entry for each time a URL was visited with a unique identifier for the URL

		- To get it in plaintext, you need to perform a lookup on the history_items table

- Example query to display the timestamp and URL

	- _SELECT h.visit_time, i.url FROM history_visits h INNER JOIN history_items i ON h.history_items = i.id_

  
![history_visits and history_items](https://miro.medium.com/v2/resize:fit:1400/1*cxf621Qxcv_ZOuEBF_eUoQ.png)

  

### Safari Downloads

  

- Safari stores a property list file for all files downloaded

	- _/Users/\$user/Library/Safari/Downloads.plist_

- Convert the plist using plutil, plistbuddy, or defaults

	- _plutil -p /Users/\$user/Library/Safari/Downloads.plist_

- Most important key/value pairs

	- *DownloadEntryURL, DownloadEntryPath, and DownloadEntryDateAddedKey*

- Older versions will not have **DownloadEntryDateAddedKey**

  

### Other Safari Files of Interest

  

- Bookmarks and iCloud account synced bookmarks

	- _/User/\$user/Library/Safari/Bookmarks.plist_

- Most visited sites

	- _/Users/\$users/Library/Safari/TopSites.plist_

- Allowed push notifications and timestamp for when they were granted permission

	- _/Users/\$user/Library/Safari/UserNotificationPermissions.plist_

- Information about the latest exited Safari session

	- _/Users/\$user/Library/Safari/LastSession.plist_
