# Sourced from https://github.com/N-able/CustomMonitoring/tree/master/Vulnerability%20-%20CVE-2021-44228%20(Log4j)


Name: get-log4jrcevulnerability.ps1

Version: 0.2.4.3 (23rd December 2021)

Author: Prejay Shah (Doherty Associates)

Thanks: Christopher Bledsoe (IPM Computers) for some bugfixes, 
        Robby Swartenbroekx (b-Inside) for some ideas, 
        Arctic Wolf for coming up with a way to detect patched files
        
Purpose: Detection of jar files vulnerable to the "Log4Shell" log4j RCE vulnerability (CVE-2021-44228)
Originally Utilizing JNDILookup detection method posted to https://gist.github.com/Neo23x0/e4c8b03ff8cdf1fa63b7d15db6e3860b with some slight modifications to make it more RMM friendly
Log4J 2.15 fixed the original Log4Shell CVE, but was suspectable to CVE-2021-44228 which was elevated to an RCE vulnerability and fixed in Log4J 2.16
I then adopted a method presented to me on behalf of ArcticWolf (https://github.com/rtkwlf/wolf-tools/blob/main/log4shell/log4shell_deep_scan.ps1) of extracting the JAR/WAR/EAR file to corroborate whether the Log4J 2.16 changes have been applied to a JAR file.
Robby S then suggested an amendment to be able to track the updated fix in 2.17 for CVE-2021-45105 that was introduced in Log4J 2.16.

NOTE: Only scans against NTFS File Format
      RMM Output optimized for N-Central
      Not currently compatible with Powershell 2.0
      Have excluded files within windows\system32\spool\drivers from being scanned due to access denied issues disrupting the output. 
      
-----
- 0.1 Initial Release
- 0.1.1 Added Dedupe to Vulnerable .JAR Listings
- 0.1.2 Public Release
- 0.1.3 Found that use of -force isn't working for all scans. Have added a non forced mode to see what outputs can be obtained
- 0.1.4 Experimenting with Unicode/Robocopy to bypass 260 character file path limit / access denied errors
- 0.1.5 added support for PSEverything module
- 0.1.5.1 changed detection to be module based rather than command based
- 0.1.5.2 Cleaned up Output
- 0.1.6 Have revamped order to PSEverything, Robocopy, GCI
- 0.1.6.1 Fixed Typo, Modification for N-Central AMP Output of file names when robocopy is utilized
- 0.1.7 Some bugfixes courtesy of Christopher Bledsoe (IPM Computers). Who knew inaccessible cloud only JAR files would be an issue?
-         won't try checking an empty file path
-         should ignore those "placeholder.jar" files in things like dropbox cache
-         it uses "|" for the delimiter when reading the CSV/txt file created, so any file paths with "," in them should not get unintentionally split
- 0.1.7.1 Excluding spool\drivers jar files from being scanned
- 0.1.7.2 Updated gci to use -filter and -file rather than -include after finding it to be much more performant
- 0.1.8 Fix for Everything/gci compatibility, Update for Log4j 2.16 update
- 0.1.8.1 Improved Try/Catch methodology for when Everything search Fails. Thanks to Robby Swartenbroekx (b-Inside) for the assist.
- 0.1.8.2 Made robocopy window hidden by request
- 0.1.8.3 Improved Vulnerable File Output for RMM
- 0.1.9 Expanded Search Criteria to all fixed drives on a device, and added update for Log4j 2.17 Compatibility (Thx to Robby S)
- 0.2 Separated detection of Log4j 2.16 Patched and 2.17 Patched States
- 0.2.1 Adding better output for when Everything fails to scan via RMM PS wrapping
- 0.2.2 Expanded search to cover .jar/.war/.ear files, and partial fix for oddity with scanning certain file names (Thx to Robby S)
- 0.2.3 Adding more error logging to RMM output in order to surface files that aren't being scanned, amended query to exclude drives that aren't formatted to NTFS
- 0.2.4 Moved Tasks into Functions, Fixed output bug for number counts.
- 0.2.4.1 Changed robocopy export/csv import encoding to fix issue with display of special characters in file names
- 0.2.4.2 Bug with previous fix. I think I have now fixed the robocopy export/csv import encoding based on example files/folders available to me.
-         Have made further modifications to try to prevent N-Central RMM from showing a misconfigured error even when there is valuable output that could be shown
- 0.2.4.3 Started work on PS 2.0 Compatibility, working on some code cleanup as well. Thx to Robby S for his assistance with this!

To Do:
- Achieve PS 2.0 Compatibility
- Look at lifting the restriction to only scan NTFS
- Better Error Handling for N-Central RMM AMP usage
- File/Folder exclusions for scanning/flagging as vulnerable 
- Detection/Exclusion of Cloud-Only OneDrive Files that cannot be scanned
        
