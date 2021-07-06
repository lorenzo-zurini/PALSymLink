# What is PortableApps.com?
PortableApps.com is the world's most popular portable software solution allowing you to take your favorite software with you. A fully open source and free platform, it works from any synced cloud folder (DropBox, Google Drive, Box, etc), from your local PC on an internal or external drive, or on any portable storage device (USB flash drive, memory card, portable SSD or hard drive, etc) moved between PCs. 


# PALSymlink
PALSymlink is a small script I wrote that adds Symbolic Link functionality to the PortableApps.com-format applications. 

# Why Symlinks instead of PortableApps' DirectoriesMove functionality?
When the Data directory of a portable app gets big (or if the computer it's being run on has a slow HDD), there is a risk of data loss. 

If the hidden move operation is somehow aborted, the data will be affected. Normally, this isn't a problem as it takes miliseconds with small programs but with software that stores a lot of data on the system (think videogame savegames or shader caches), the risk is significant. 

I actually experienced this several times. Besides data-safety, using my script instead of PA's DirectoriesMove greatly improves app startup and post-launch cleanup speed as there's no moving around of large ammounts of data. 


# Disadvantages
Applications using this script can only be run from NTFS-formatted drive so putting them on FAT32 flash drives (the main use case for PApps) is not possible as that filesystem doesn't support symbolic links.

⚠️Administrator privileges are also required for the creation of symbolic links so be sure to enable the RunAsAdmin=compile-force flag in your appname.ini PRIOR to recompiling. This will trigger UAC every time the app is run which can be annoying. I personally always disable UAC so that's not a problem for me but it might be for you. This also means that you can't run the application in an environment where you don't have administrator privileges. 


# Usage
1. Download Custom.nsh from this repo and put it in your Appinfo>Launcher directory, next to the appname.ini file.
2. Be sure to enable administrator privileges flag in your appname.ini. This can be done by adding the following line in the [Launch] section of the file.
 ```
 RunAsAdmin=compile-force
 ```
3. Recompile the portable application using the Portableapps Launcher Generator. This can be done by downloading [this](https://portableapps.com/apps/development/portableapps.com_launcher) , installing it somewhere and drag-and-dropping the directory containing the application (the one that contains the App and Data dirs) that you want to make portable to PortableApps.comLauncherGenerator.exe
 
4. In your appname.ini file, create the following section for each symbolic link that you want to make:

```
[SymLinkRedirectN]
Path= 
Target= 
```

 ⚠️Replace the N in SymLinkRedirectN with the number of the SymLink, like you were using FileWriteN. See the example below.
  
  
For Path, use the path where the symbolic link wil be located. This is the path of the directory that you want to make portable, the path that you would put on the RIGHT side of the "=" if you were using DirectoriesMove. Most wilidcards from PA are supported (more below). The DirectoriesCleanupIfEmpty section is also replicated by my script and should be left empty (if you aren't using DirectoriesMove as well). The parent directories of the SymLink will be recursively removed if they are empty. If there is already a directory at this path, it will be renamed when the app is run and renamed back when it is closed.

For Target, use the name of the directory in your PAL:DataDir (the Data directory that is created after the app is run) that the symbolic link will point to. This is the path that you would put on the LEFT if you were using DirectoriesMove. If this directory doesn't already exist (like if you put it in DefaultData), it will be automatically created.
 
 5. Run the app and see if it works. Be aware that it will not run from a non-NTFS formatted drive as other filesystems like FAT32 don't support Symbolic Links.
  
 # Wildcards
  
 My script supports the following wildcards that PortableApps uses:
```
  %PAL:AppDir%
  %PAL:DataDir
  %UserProfile%
  %AllUsersProfile%
  %LocalAppData%
  %AppData%
  %Documents%
  %SystemDrive%
```
  
 Use them like you would if you were using DirectoriesMove. Documentation [here](https://portableapps.com/manuals/PortableApps.comLauncher/ref/envsub.html)
  
 I also added two more varibles that I thought would be useful:
```
  %AppDataLocalLow% - translates to %UserProfile%\AppData\LocalLow
  %ProgramData% - translates to %SystemDrive%\ProgramData (same as %AllUsersProfile%).
```
  # Example appname.ini:
```
[Launch]
Name=AppName
ProgramExecutable=AppDir\appexe.exe
DirectoryMoveOK=yes
WorkingDirectory=%PAL:AppDir%\AppDir
runasadmin=compile-force        ⚠️

[SymLinkRedirect1]
Path=%PAL:AppDir%\Config
Target=Configs
  
[SymLinkRedirect2]
Path=%AppData%\AppName
Target=User Data
```
  # Licence
Feel free to use this in your portable apps however you like.  
I would appreciate a credit if you feel so inclined.  
My PortableApps.com forum username is https://portableapps.com/user/303068  
  
  
  
 
