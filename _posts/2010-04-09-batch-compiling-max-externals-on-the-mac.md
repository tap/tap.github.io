---
layout: post
title: "Batch Compiling Max Externals on the Mac"
date: 2010-04-09
categories: electrotap
source: "Originally published on electrotap.com"
---

Things have been a little overwhelming lately, as they likely will be for the summer. I thought I would post an AppleScript that I use for compiling the Tap.Tools Max objects. It could be easily modified for others who have a lot of of externals to compile all at once.

```
set project_path_unix to "/Users/tap/Developer/_SVN/TapToolsMax/Max_Projects"
set project_path to POSIX file project_path_unix as text
set project_folder_list to list folder project_path
set continue_flag to 1
try
 display dialog project_path as text
on error
 display dialog "Yo! Can't get the path for the Max Projects folder"
 display dialog (dialog reply)
end try

tell application "Finder"

 repeat with project_folder in project_folder_list
 if (project_folder does not contain ".svn") then
 try
 set project_file_list to list folder (project_path & ":" & project_folder)
 repeat with project_file in project_file_list
 if (project_file contains ".mcp") and (continue_flag is equal to 1) then
 tell application "CodeWarrior IDE"
 activate
 open (project_path & ":" & project_folder & ":" & project_file) as alias
 try
 Make Project
 Close Project
 on error
 display dialog "There was an error building " & project_file & ".
 Try to continue?" buttons {"Yes", "No"} default button 2
 if the button returned of the result is "No" then
 --quit
 set continue_flag to 0
 end if

 end try

 end tell
 end if
 end repeat
 on error
 --display dialog project_folder & " is not a folder - skipping"
 end try
 end if
 end repeat

end tell
```
