# Storage

Watch this virtual workshop to learn more about CCR's storage options & policies:  
![type:video](https://youtube.com/embed/EzQuL59DPjA)    

## Enterprise-level Network Attached Storage  
  - Vast Data 4.6PB flash disk system designed for 99% uptime
  - At most, 2 scheduled maintenance outages/year  
  - Serves the following file systems:  

    - **Home directories:**  
        - Directories found in `/user/[CCRusername]`  
        - Home directories are owned and accessible only to the user.  Access should not be shared with other users.  Data sharing should be done with shared project directories.  
        - 25GB quota  
        - To protect the filesystem, there is a limit of 10 million files per home directory
        - Automatically created for new users  
        - Backed up nightly off campus by UBIT
        - Backups maintained for 30 days
    - **Project directories for academics:**  
        - Shared by a single research group or course  
        - Directories found in `/projects/academic` or `/projects/academic/courses` for classes
        - 1TB provided for free to UB faculty groups
        - Use ColdFront to request an allocation for the `Project Storage` resource
        - Additional [storage can be purchased](#purchasing-project-storage)  
        - To protect the file system, there is a limit of 200 million files per project directory
        - Backed up nightly off campus by UBIT  
        - Backups maintained for 30 days    
    - **Project directories for industry customers:**  
        - Quotas vary based on Cooperative Use Agreement  
        - **NOT backed up unless specified in your company's Cooperative Use Agreement**  
        - Directories found in `/projects/industry/`  
        - Use ColdFront to request an allocation for the `Project Storage` resource  
    - **Project directories for Roswell Park users:**  
        - Directories for individual research groups found in `/projects/rpci`  
        - Use ColdFront to request an allocation for the 'Project Storage' resource  
        - Quotas are listed on individual allocations in ColdFront & can be viewed on the systems as [described below](#checking-quotas)  
        - RPCI IT staff are responsible for dividing up the storage purchased from CCR.  Please contact them if you'd like your quota increased  
        - ==**NO RPCI DIRECTORIES ARE BACKED UP**==    

!!! Warning  
      Due to limitations of the campus backup service, any directory containing over 50 million files is **NOT BACKED UP**.  The directory owner will be contacted if their directory reaches this level prior to stopping the backups.

**Requesting a restore from backup:**  
To request a restore of deleted files, please complete [this form](https://ubuffalo.teamdynamix.com/TDClient/55/Portal/Requests/ServiceDet?ID=363).  Please provide the full path of the file/directory you would like restored and the date and time you would like us to target.  This would be the last known time the file/directory was on the file system.  If it has not been on the system at least 24 hours, it will not have made it to the backup tapes.  There is no guarantee your data is available for recovery but we will attempt to recover it.  

## Global Scratch
  
   - There is no guarantee of uptime  
   - Scratch file systems are designed for temporary storage and shorter-term processing of data  
   - To be used during job runs and moved or deleted at the completion of a job
   - Data that has not been accessed in more than 60 days is automatically deleted nightly. See [Scratch Usage Policy](../policies/misuse.md#scratch-usage-policies)  
   - 10TB provided for free to all groups
   - Use ColdFront to request an allocation for the `Global Scratch Storage` resource
   - Directories found in `/vscratch/grp-[YourGroupName]`  
   - To protect the file system, groups are limited to 200 million files per directory.
   - ==**NO SCRATCH FILE SYSTEMS ARE BACKED UP**==  

!!! Danger "Automatic data deletions!"  
    Data that has not been accessed in more than 60 days is automatically deleted nightly.  To avoid data loss, please remove all data promptly after your job completes

## Local Scratch  

- All compute nodes have local disk space in `/scratch`
- The local scratch space on a compute node is only available to batch jobs running on that node   
- Data in local scratch is removed automatically when the job completes  
- Users should copy all data from local disks before the job ends  
- Limited by the space available on the node which varies across node types  
- In theory, local scratch will provide the fastest I/O because there will be no network latency that other storage options may contend with  
- ==**There is NO backup of data in /scratch**==  

## Cloud Storage  

- Accessible only from cloud instances  
- There is no guarantee of uptime or performance on cloud storage  
- Available only to research groups with active cloud subscriptions  
- [See here](../howto/purchases.md#cloud-storage) for cloud storage pricing
- ==**There is NO backup of data for the research cloud**==  

## Purchasing Project Storage  

Refer [here](../howto/purchases.md#storage) for instructions on how to initiate a purchase or renew an annual purchase.

## Checking Quotas

**Your user quota (home directory - `/user/[CCRusername]`):**

```
ccrkinit
NOTE: You will see "Enter OTP Token Value:" - Enter your password AND one-time token all in the same line here

iquota

or

iquota -p /user/[CCRusername]
```

!!! Tip  
    If you see an error like this it is usually related to conda environments:  
    `kinit: Unknown credential cache type while getting default ccache`  
    See [the FAQ](../faq.md#why-am-i-see-the-error-kinit-unknown-credential-cache-type-while-getting-default-ccache-when-using-ccrkinit) for more details  


**Your group's shared project directory quota:**

NOTE: You must specify the full path of your directory.  Not all project directories are in /projects/academic


```
ccrkinit
NOTE: You will see "Enter OTP Token Value:" - Enter your password AND one-time token all in the same line here


iquota -p /projects/academic/[YourGroupName]

HINT: Do not include the trailing slash in the directory name
```

**Your group's shared global scratch directory quota:**

```
ccrkinit
NOTE: You will see "Enter OTP Token Value:" - Enter your password AND one-time token all in the same line here

iquota -p /vscratch/grp-[YourGroupName]

HINT: Do not include the trailing slash in the directory name
```

!!! Tip  
    iquota only reports the total number of files for the group on the storage. If you're getting 'out of space' or I/O errors and the group is not over the file limit, it could be your individual limit has been reached. Please email CCR help to request a file quota check.

```
iquota usage:

The iquota command has much more built into it than former versions.  Use iquota -h to see all the options.  

NOTE: You will need to get a kerberos key before running the command, with:  kinit

[ccruser@login1:~]$ iquota -h

NAME:

   iquota - displays CCR quotas

USAGE:

   iquota [global options] command [command options] [arguments...]

AUTHOR:

   Andrew E. Bruno <aebruno2@buffalo.edu>

COMMANDS:

   help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:

   --conf value, -c value                                Path to conf file

   --debug, -d                                           Print debug messages

   --user, -u                                            Print user quota

   --long, -l                                            display long listing

   --show-user value                                     Print user quota for specified user (super-user only)

   --show-group value                                    Print group quota for specified group

   -p value, --path value, -f value, --filesystem value  report quota for filesystem path

   --help, -h                                            show help
```

**What do quotas limit?**  
These spaces all have a quota for usage AND number of files.  This is to prevent errant processes from creating so many files it crashes the system or makes it unstable for others.  If you are getting 'out of space' or I/O errors, please verify you are not over quota in either space or number of files.

## Calculating disk usage at the Linux command line  

Knowing your quota and how much of it you've used is helpful, but often we want to know where the disk usage is actually taking place.  You can run the `ncdu` command (NCurses Disk Usage) to calculate the total size of each file and sub-directory and then sort it from largest to smallest.  You will only be able to run this where you have permission to read files, so it's appropriate to run in your home directory:  `/user/[CCRusername]` or your group's project directory.

There are many options for sorting as well as the ability to delete files within this interface.  Use the `man` command to see options: `man ncdu`  

## Alternative Quota Lookup  

Users can access their quota information in [ColdFront](../portals/coldfront.md) as well as on the command line

## Starfish Storage Usage  

CCR offers [Starfish](https://starfishstorage.com/) portal access to provide research groups the ability to access detailed information about their data to better sort through and make decisions on what to keep, what to backup up outside of CCR, and what to delete.  PIs or faculty group managers may request a 30 day allocation to the Starfish storage portal.  To do so, request an allocation in [ColdFront](https://coldfront.ccr.buffalo.edu) for the 'Starfish' software resource.  The PI is automatically added to the allocation.  **If the PI will not be the one doing the data inspection and tagging, please add only one very trusted project member to the allocation.**  This person will have access to view all files in the group's shared project directory.  It may be that your group has set the system permissions such that everyone in the group can already read all files; this is CCR's default but many groups change their permissions.  In this case, the permissions in Starfish would be the same.  However, the user with Starfish access **will be able to tag files and directories for deletion so please be very careful with this!**  If you need multiple group members to make decisions on tagging data, please do NOT share your account information with them.  Request additional access through [CCR help](../help.md).

**Logging into Starfish:**  
Once the group has been given access to Starfish the PI will be able to login using their CCR username, password, and one time token.  

!!! Warning "VPN Required"
    Access to ColdFront is restricted to UB and Roswell Park networks
    (either on campus or connected to their VPN services). [See here](../getting-access.md#vpn-access)

[Starfish Portal](https://starfish.ccr.buffalo.edu)

!!! Note "Special Login Procedure!"
    Logins require password plus the one-time token (OTP) generated from your authentication app.  These are entered back to back with no spaces or extra characters between them into the password field.  For example:  
    _Password:_ BuffaloLove!  
    _OTP code displayed:_ 123 456  
    _Enter in the password box:_  BuffaloLove!123456  

**Using Starfish:**

You will be considered a Starfish Zone Admin and have access to detailed usage information for your group's shared project directory.  At the top right, under the Help menu, there is a link to the Zone Admin User Guide.  This contains all the information you need to understand how Starfish works, how it calculates usage (for example, usage is calculated in TiB not TB), and how to tag your data for deletion or other classification.  You may need to be logged into Starfish to view these.  

[Zone Admin Quick Reference Guide](https://starfish-stable-updates.s3.amazonaws.com/release_notes/Zoneadmin_quick_reference.pdf)  
[Zone Admin Guide](https://starfish-stable-updates.s3.amazonaws.com/release_notes/Starfish_GUI_Zoneadmin_Guide.pdf)  

Starfish provides many avenues for viewing metadata about your data.  Users can visualize the data by age, size, and other options.  We're able to view the last time data was accessed.  You can download reports about your data in CSV format allowing you to share information with group members.  Please refer to the documentation above for more information.  

**Hints:**  
The Starfish GUI offers alot of information and capabilities!  Their "Hints" feature is very useful for basic information about each of the icons, tools, and sections.  Click on the "Hints" button at the top right to see all the available hints.  Then hover over the question mark icons to get more info.  

**Tagging Files & Directories:**  
We provide "action" tags to mark files and directories for deletion in 30 or 60 days and also a "classification" tag to mark a file or directory for off-site backup (archive).  If you mark files or directories with action tags, nothing happens to the data until you tell CCR to move forward with the deletion process. Right click on a file or directory to view the different types of Tags and Actions you can take on them.  CCR does not provide archival storage.  However, groups may wish to go through their data, identify files to backup off of CCR (i.e. archive) and then generate a report for their group members to review those marked for archive.  Groups would do the archiving on their own outside of Starfish but this classification process is a nice way for groups to periodically review whether old and untouched data needs to remain taking up quota on CCR's systems.  

**All done?**  
Once you're done with reviewing and tagging your data, please contact [CCR Help](../help.md) to let us know it's ok to run the delete process.  At the end of your 30 day allocation, access to Starfish will be removed to free up the license for other groups.
