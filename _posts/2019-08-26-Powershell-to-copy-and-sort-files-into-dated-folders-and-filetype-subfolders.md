---
layout: post
title:  "Powershell to copy and sort files into dated folders and filetype subfolders"
date:   2019-08-26 00:00:00 +0000
categories: [Tech]
author: Joe
---
The below Powershell script will copy files from one location to another, picking only the ones that were created within a specified date range and matching particular filetypes/extensions.
Matching files are copied to dated folders with a custom description and further split by file extensions into subfolders.<!-- more -->
The steps it takes are listed below:

1. Default values are set in the `$defaultVals` hashtable in the script
 * Change the source and destination values to something appropriate!
1. Ask for `$daysOldest`, the farthest (in whole days) you want to go back in time e.g. 5
1. Ask for `$daysNewest`, the closest (in whole days) you want to go back in time e.g. 2
1. Ask for `$description`, the custom description to tack onto the end of the dated folders. e.g. *A Summer's Day* would give `2001-03-24 - A Summer's Day` as the dated folder
1. Ask for `$include[]`, an array of allowed file extensions in the form `include[0]: *.jpg, include[1]: *.txt` etc.
 * (you can leave this blank to include all files (*) by default)
1. Ask for `$source` and `$destination` directories – you should change these defaults even if you don’t change any others
1. Check that each file in the `$source` directory has an allowed file extension<br>
and was created within the date range specified
1. Work out the new folder structures and create them if they don’t exist
1. Check if the files has already been added to the destination
1. Copy the files

**NOTE: Replace the source and destination paths in the $defaultVals hashtable!**

## Dated_and_extension_file_sort.ps1:

You can find the latest version of the script in the [Dated and extension file sort repo][Repo] on my GitHub.

This was written to make transferring and organising photos a little easier, particularly after I found thousands of old photos in a giant mess when going through backups.
The end result looks a little something like this:

```
> Dated_and_extension_file_sort.ps1
daysOldest: 2
daysNewest: 1
description: Wedding
source: <e.g. an SD card from a camera>
destination: <e.g. holiday pics folder>
include[0]: *.jpg
include[1]: *.avi
include[2]: *.mp4
include[3]: *.xcf
include[4]: 
```

![screenshot]

Updated 2019-08-20 to be more robust.<br>
Updated 2019-08-21 to remove config file.<br>
Updated 2019-08-21 to fix issue with -Include filter (use the arrays, Luke) and description.<br>
Updated 2019-08-26 to remove vestigial Get-ScriptDirectory() function, moved daterange and directory validation to parameters, added # of copied and skipped files processed to output, cleaned inappropriate quotation marks

[Repo]: https://github.com/JGDR20/Dated-and-extension-file-sort
[screenshot]: /jgdr20/assets/2019-08-26-PS_folder_screenshot.png