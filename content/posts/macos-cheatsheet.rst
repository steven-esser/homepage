****************
MacOS Cheatsheet
****************

:title: MacOS Cheatsheet
:date: 2020-12-07
:category: Posts
:slug: macos-cheatsheet
:tags: macos, macports

Here are some tricks that may make life easier for power users.

Properly set the computer name
##############################
The following is how to properly set the computer name for a MacOS machine in all the
right places. These are often the first set of commands that I run when setting up a new
machine.
::

    sudo scutil --set ComputerName "insert-name-here"
    sudo scutil --set HostName "insert-name-here"
    sudo scutil --set LocalHostName "insert-name-here"
    sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.smb.server NetBIOSName “insert-name-here”
    dscacheutil -flushcache

Once finished, you can restart the system to fully clear the DNS cache.

Permanently remove the "Guest" user
###################################
On some installations of MacOS, a "Guest" user gets created alongside your own created 
user. To permanently remove this user, run the following command:
::

    sudo fdesetup remove -user Guest
 
Set default permissions for ``/usr/local/``
###########################################
To reset ``/usr/local/`` permissions, run the following command:
::

    sudo chown root:wheel /usr/local

Common diskutil commands
########################
``diskutil`` is a useful tool found in MacOS that can be used to manipulate disks, like
external storage or other drives.

To list all attached disks, run:
::

    diskutil list

The output will show a list of disks. From there, you can manipulate these disks
individually. For example, here is how to partition a USB stick as a FAT32 formatted
drive:
::

    diskutil partitionDisk disk3 MBR "MS-DOS FAT32" "DRIVE-NAME" 100%

In the above example, our USB stick is ``disk3``. Consult the output of ``diskutil list`` for
the correct disk name if you attempt this on your own machine.

Convert a Linux ISO to a usb-writable format
############################################
In order to write a linux ``.iso`` image to a removable drive it must first be converted to
a proper macos friendly image using the ``hdiutil`` command:
::

    hdiutil convert /path/to/ubuntu.iso -format UDRW -o /path/to/target.img

After conversion, ``target.img`` will be ready to write to a storage medium using a utility
like ``dd``.

Set MacVim as the default editor for all files
##############################################
::

    defaults write com.apple.LaunchServices/com.apple.launchservices.secure LSHandlers -array-add '{LSHandlerContentType=public.plain-text;LSHandlerRoleAll=org.vim.MacVim;}'

Then restart your machine.

Fully remove Python.pkg
#######################
To fully remove an installed python.pkg version (from the python website), run:
::

    sudo rm -rf /Library/Frameworks/Python.framework
    cd /usr/local/bin
    ls -l . | grep '../Library/Frameworks/Python.framework' | awk '{print $9}' | xargs sudo rm

    # Then finally:
    sudo rm -rf "/Applications/Python x.y"

Fully remove macports
#####################
Sometimes it is necessary to completely remove an installation of macports. Use the
following command to remove ``macports`` and its artifacts, run:
::

    sudo port -fp uninstall installed
    sudo port -fp uninstall --follow-dependents installed

    # Then run:
    sudo rm -rf \
        /opt/local \
        /Applications/DarwinPorts \
        /Applications/MacPorts \
        /Library/LaunchDaemons/org.macports.* \
        /Library/Receipts/DarwinPorts*.pkg \
        /Library/Receipts/MacPorts*.pkg \
        /Library/StartupItems/DarwinPortsStartup \
        /Library/Tcl/darwinports1.0 \
        /Library/Tcl/macports1.0 \
        ~/.macports

Note: You may need to adjust some of the above paths if you installed ``macports`` into a
custom prefix.

Fully remove pkgin
##################
Sometimes it is necessary to completely remove an installation of ``pkgin``/``pkgsrc``. Use
the following command to remove ``pkgin`` and its artifacts, run:
::

    sudo rm -r /opt/pkg /var/db/pkgin /etc/{man,}paths.d/pkgsrc
