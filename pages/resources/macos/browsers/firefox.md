---
layout: page 
title: Browser Analysis - Firefox
permalink: /resources/macos/browsers/firefox
---

## Quick Notes
*   Use of Firefox has dwindled since Chrome gained popularity
*   Open-sourced and well documented

## History

*   Handled in a sqlite3 database
    *   places.sqlite can be found at _/Users/$user/Library/Application Support/Firefox/Profiles/<profile>.default/places.sqlite_
        *   <profile> will be randomly determined when setting up Firefox
        *   **places.sqlite** holds a number of tables -> two similar to Chrome
            *   **mox\_historyvisits** -> Timestamped entry for each visited website and URL id
            *   **moz\_places** -> Correlate the URL id to the actual URL
*   Query to pull timestamp and URL from history
    *   _SELECT datetime(hv.visit\_date/1000000, ‘unixepoch’) as dt, p.url FROM moz\_historyvisits hv INNER JOIN moz\_places p ON hv.place\_id = p.id ORDER by dt ASC_

### Downloads

*   Old versions of Firefox used to store a sqlite3 database called downloads.sqlite
    *   Stored files that were downloaded and when
    *   Now in the places.sqlite file
*   Only need to collect one database
    *   Downloads can be found in the **moz\_annos** table
*   Data is stored differently from Chrome and Safari
    *   Every download creates multiple entries
    *   moz\_attribute\_id field can be found in the **moz\_anno\_attribute** table
        *   Numbers correspond to what was done
            *   1 -> bookmarkProperties/description
            *   2 -> Places/SmartBookmark
            *   3 -> places/exlcludeFromBackup
            *   4 -> PlacesOrganizer/OrganizerFolder
            *   5 -> PlacesOrganizer/OrganizerQuery
            *   6 -> downloads/destinationFileURI
            *   7 -> downloads/destinationFileName
            *   8 -> downloads/metadata
        *   Look for entries with **a moz\_attribute\_id of 6**
*   Query to pull timestamp, download location, and source URL
    *   _SELECT moz\_annos.dateAdded, moz\_annos.content, moz\_places.url FROM moz\_places, moz\_annos WHERE moz\_places.id = moz\_annos.place\_id AND anno\_attribute\_id=6_

### Other Firefox Files of Interest

*   _/Users/$user/Library/Application Support/Firefox/Profiles/<profile>/cookies.sqlite_
    *   Holds a table called **moz\_cookies**
    *   Cookies for visited sites
*   _/Users/$user/Library/Application Support/Firefox/Profiles/<profile>/extensions.json_
    *   Data on what extensions are installed as well as descriptions, extension homepages, and authors
