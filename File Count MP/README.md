Always test the management packs in a test environment before importing in production!

______________________________________________________________________

I come at a lot of customers to implement or support SCOM. 
Sometimes the same questions or troubles come up.

One of that questions is: *"Is it possible to monitor the count of files (with a specific extension) in a share?”*
The answer to this question is yes and no. There is a possibility to count files on Windows Servers that have an agent installed using this management pack: http://www.systemcentercentral.com/pack-catalog/file-system-management-pack-2/ but for shares located on non-Windows Servers, let’s say on a SAN for example I haven’t found a solution.

Therefore I created my own management pack to monitor the file count, independent of the location of the file share (Windows Server or not). In this post I describe how the management pack works. WIth the management pack you can count files with a specific extension (or no extension if everything should be counted) in a share (optionally also subfolders included). There is also the ability to add a specific age zo the given scenario is possible: Count if there are more then 20 files in a share (subfolders included) that are older then 10 minutes.

![Alt text](Images/6.png?raw=true "Registry ")

First of all we need a seed discovery which is targeted to a registry key located on a SCOM agent monitored Windows Server. The value in the registry is located under SOFTWARE\Filecount. The value is “CSV” and it should contain the path to a CSV file. The server will be discovered as a “File Count Watcher Node”

Next stop is the csv file itself, for every share to be monitored it should contain a line with a specific syntax shown in the screenshot below

![image](https://github.com/bpinoy/ManagementPacks/assets/32384899/d20c2efd-3d13-418a-b042-e8c4ea506c83)



Different parameters are added:
- ID: Must be unique per share
- Name: this represents the name the displayname used in SCOM (this can be different from the share)
- Share: UNC path of the share
- Extension: The extension of the files that needs to be counted, leave empty to count all files in the share
- Count: How many files must be present for a critical state
- Time: This is the time in minutes of the maximum file age of file count
- Recurse: 0 = No need to count files in subfolders / 1 = Count also files in subfolders

When the info is filled in, SCOM will discover every line as a “File Count Share”.

![image](https://github.com/bpinoy/ManagementPacks/assets/32384899/aad76ac1-bbe5-4a64-917d-bc2d0b9a8114)


The properties are used to configure the monitoring.A monitor is also defined based on the properties filled in the csv file, but it’s basically a powershell script with necessary parameters.The core of the script is this command: 

`$count  = Get-ChildItem -Recurse $strShare\$strExtension | where{$_.LastWriteTime -le (Get-Date).AddMinutes(-$strAge)}|Measure-Object |%{$_.Count}`

`$countall = Get-ChildItem -Recurse $strShare\$strExtension |Measure-Object  |%{$_.Count}`

The file count is also gathered as a performance counter so it can be included in reporting or in a Squared Up dashboard for example.

![alt text](Images/8.png?raw=true "Perf" )

Example of a SquaredUp dashboard: 

![alt text](Images/7.png?raw=true "Perf" )

The management pack is also configured to use a specific Run As account. This account needs rights on the shares: at least Read-only Share rights and Read-Only NTFS rights.

![alt text](Images/Runas.png?raw=true "User" )

I’ve been able to help some customers already by using this management pack.The first customer where I set this up is a big hospital in Belgium where they use this management pack to monitor shares which are used to store (and process) images and movies made during surgery. The content should be processed from the network share and transferred somewhere else but sometimes the processing hangs and the share is getting full without anyone knowing. Since they have the management pack in place this hasn’t happened anymore.

If you have questions, feedback,... drop me a line at bert@bpitconsulting.be
