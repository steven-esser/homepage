****************************************
Install MacPorts Without Root Privileges
****************************************

:title: Install MacPorts Without Root Privileges
:slug: macports-without-root-privileges
:date: 2019-08-13
:category: Posts
:tags: macos, macports

The following document describes how to install MacPorts in a user's home directory,
without root privileges. I have done this on my personal machine to prevent my root file
system and start up items from being polluted by ports and their various files. It is
also a convenient way to install MacPorts if you only have access to a non-admin user
account.

You do lose out on a few things when installing MacPorts this way. Because this is a 
non-standard installation method, you will have to compile each and every port you want
to install. Depending on the port, this can take a long time. Also, some portfiles may
need to be modified if they contain hard-coded root invocations.

Additionally, you will have to compile MacPorts from source.

Procedure
#########
Once you have all your XCode/Command Line Tools installed, download the latest MacPorts_
source. Extract and ``cd`` into the MacPorts source folder and run the following command:
::

    $ ./configure --prefix "$HOME/MacPorts" --with-install-user=$(id -u -n) --with-install-group=$(id -g -n) --with-no-root-privileges
    $ make && make install


Next, add the following to the MacPorts ``variants.conf`` file. This will ensure we use
non-root port variants, if they are available:
::

    +no_root -startupitem

Finally, add the following to the MacPorts ``macports.conf`` file. This will ensure that no
startup items (daemons) are installed for ports that contain them:
::

    startupitem_type none
    startupitem_install no

Once this is done, you can access the ``port`` command and subsequent installed software at
``$HOME/MacPorts/bin`` and ``$HOME/MacPorts/sbin``

.. _MacPorts: https://distfiles.macports.org/MacPorts/?C=M;O=D

macOS 10.15 Update
******************
In order for non-root installations of MacPorts to work properly in macOS 10.15, you must
add the following to your ``macports.conf`` file:
::

    hfscompression no
