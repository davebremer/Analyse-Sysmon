# Export-SysmonLogs

===============================

_**DEPRECATED**_

_This module was a first crack at trying to get sysmon logs into some format thats easier to work with. Ther's a huge amount of hard coding which was a pain every time there was an upgrade and new data types were introduced.

Theres another module called [export-sysmondb](https://github.com/davebremer/Export-SysmonDB) which dynamically figures out what fields belong to what event types, and loads them into a sqlite database.

to be honest - im not sure doing this is useful. still its been a useful exercise for me.

==============================


This module provides a way to export Sysmon Logs into a number of CSV file, one for each type

Early days. So far focusing on pulling logs out and getting them into csv and (soon) sqlite.

Really though - the state that this things at, its really just a way to export sysmon logs to CSV

##Functions

For exporting there is:

1. Export-SysmonLogs
  * Reads either the live sysmon logs, or an offline saved evtx file
  * Each record is then thrown through the appropriate ConvertFrom-Sysmon file
  * The resulting object is saved to a csv file, each type going to seperate csv files
  * There are a bunch of flags which manipulate `get-WinEvent` used inside the script to select specific types, date range, etc.
  
2. ConvertFrom-Sysmon###### scripts (eg ConvertFrom-SysmonProcessCreate)
  * Seperate scripts, one for each Sysmon type. You can get a list of these events with
     
     `(Get-WinEvent -ListProvider "Microsoft-Windows-Sysmon" ).Tasks | sort value | Select Value,Displayname`
   * A smarter person than me could just have a single script that used the events from -listprovider to identify the fields in each type. You can check these out with
   
     `(Get-WinEvent -ListProvider "Microsoft-Windows-Sysmon" ).Events | sort id | Select id,description |format-table -wrap`
   * Each script also has an alias matching the type-id eg ConvertFrom-SysmonType1. These are used to dynamically select each script within `Export-SysmonLogs`
