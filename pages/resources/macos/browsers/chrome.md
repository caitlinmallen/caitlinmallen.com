---
layout: page 
title: Browser Analysis - Chrome
permalink: /resources/macos/browsers/chrome
---

## History

*   Database found in _/Users/$user/Library/Application Support/Google/Chrome/Default/History_
    *   Contains SQLite tables with information such as downloads, metadata, URLs, etc.
*   Similar to the Safari database -> different table names and fields
    *   URL ids with timestamps are stored in a **visits** table and need to be correlated to the id value in the **urls** table to build a timeline
    *   Example sqlite3 query to lookup the plain text URLs in the **urls** table from the URL field in the **visits** table
        *   _SELECT datetime(((v.visit\_time/1000000)-11644473600), ‘unixepoch’), u.url FROM visits v INNER JOIN urls u ON u.id = v.url_
*   When Chrome is in use, you cannot access this database unless it is copied to a new location or closed

### Downloads

*   Located inside the **downloads** table of _Chrome History.db_
*   Details of what was downloaded include
    *   If the file was opened by Chrome after being downloaded
        *   1 means the file was opened, 0 means not opened by clicking on the file after a completed download
            *   Not updated if opened via Finder
    *   The **danger\_type** \-> if a file was marked as suspicious by Chrome upon download
        *   Apart of the [Chromium browser](https://source.chromium.org/chromium/chromium/src/+/main:components/safe_browsing/content/resources/download_file_types.asciipb?q=download_file_types.asciipb%20-f:%2Fgen%2F&ss=chromium)
        *   [Guide to the values](https://dfir.blog/chrome-values-lookup-tables/)
    *   Total bytes
    *   Timestamps
        *   Stored in Epoch
    *   Referrer
        *   May be empty depending on the download
            *   Can also use the **downloads\_url\_chains** table to look up the referring URL
*   Example sqlite3 query to grab timestamp, URL, file location, danger type, and opened value from the downloads table

### Other Chrome Files of Interest

*   _/Users/$user/Library/Application Support/Google/Chrome/Default/Preferences_
    *   Contains information on plugins, extensions, sites using geolocation, popups, notifications, DNS prefetching, certificate exceptions, etc.
    *   Great place to check if a setting is enabled or not
*   _/Users/$user/Library/Application Support/Google/Chrome/Extensions_
    *   Contains information about extensions
    *   Extension IDs are randomized at install time
*   _/Users/$user/Library/Application Support/Google/Chrome/Default/Cookies_
    *   Some data is encrypted in this database
    *   Links are not encrypted
        *   If user deleted history but not cookies, this can recover some data
*   _/Users/$user/Library/Application Support/Google/Chrome/Default/Last Session_
*   _/Users/$user/Library/Application Support/Google/Chrome/Default/Last Tabs_
    *   Files contain sites that were active on the browser during the last closure
    *   Can view the files with the strings command
*   _/Users/$user/Library/Application Support/Google/Chrome/Default/Bookmarks_
    *   Verbose dictionary of all the sites the user has bookmarked and timestamps for when they were added
