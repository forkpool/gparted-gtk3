GPARTED GTK3 [![Build Status](https://travis-ci.org/lb90/gparted-gtk3.svg?branch=gparted-gtk3-github)](https://travis-ci.org/lb90/gparted-gtk3)
=======

Gparted is the GNOME Partition Editor for creating, reorganizing, and
deleting disk partitions.

A hard disk is usually subdivided into one or more partitions.  These
partitions are normally not re-sizable (making one smaller and the
adjacent one larger.)  Gparted makes it possible for you to take a
hard disk and change the partition organization, while preserving the
partition contents.

More specifically, Gparted enables you to create, destroy, resize,
move, check, label, and copy partitions, and the file systems
contained within.  This is useful for creating space for new operating
systems, reorganizing disk usage, and mirroring one partition with
another (disk imaging).

Gparted can also be used with storage devices other than hard disks,
such as USB flash drives, and memory cards.

Visit [gparted.org](http://gparted.org) for more information.


LICENSING
---------
GParted is released under the General Public License version 2, or (at
your option) any later version.  (GPLv2+).  All files are released under
the GPLv2+ unless explicitly licensed otherwise.

The GParted Manual is released under the GNU Free Documentation License
version 1.2 or any later version.  (GFDLv1.2+).

Google Test C++ test framework is released under the 3-Clause BSD
License.  (BSD-3-Clause).

See these files for more details:
   COPYING      - GNU General Public License version 2
   COPYING-DOCS - GNU Free Documentation License version 1.2
   lib/gtest/LICENSE
                - 3-Clause BSD License


NEWS
----
Information about changes to this release, and past releases can be
found in the file:
   NEWS


INSTALL
-------
### Pre-built Binary ###

   Many GNU/Linux distributions already provide a pre-built binary
   package for GParted.  Instructions on how to install GParted on
   some distributions is given below:

      Debian or Ubuntu
      ----------------
      sudo apt-get install gparted

      Fedora or CentOS/RHEL
      ---------------------
      su -
      yum install gparted

      OpenSUSE
      --------
      sudo zypper install gparted

### Building from Source ###

   Briefly, build and install GParted into the default location of
   /usr/local using:
   ```
      ./configure
      make
      sudo make install
      sudo install -m 644 org.gnome.gparted.policy \
             /usr/share/polkit-1/actions/org.gnome.gparted.local.policy
   ```
   This assumes all the dependencies are already installed, builds the
   default configuration and polkit is being used as the graphical su
   program.

   The following dependencies are required to build GParted from source:
   *   g++
   *   e2fsprogs
   *   parted
   *   gnome-common
   *   gtkmm
   *   gettext
   *   gnome-doc-utils     (required if help documentation is to be built)

   On **Debian** or **Ubuntu**, these dependencies may be obtained by running
   one of the following commands:
   Either
   ```
      sudo apt-get build-dep gparted
   ```
   Or
   ```
      sudo apt-get install build-essential e2fsprogs uuid uuid-dev \
                           gnome-common libparted-dev libgtkmm-3.0-dev \
                           libdevmapper-dev gnome-doc-utils docbook-xml
   ```
   On **Fedora**, you will need to run (as root)
   ```
      yum install gtkmm30-devel parted-devel e2fsprogs-devel gettext \
                  'perl(XML::Parser)' desktop-file-utils libuuid-devel \
                  gnome-doc-utils docbook-dtds intltool gnome-common \
                  gcc-c++
      yum groupinstall 'Development Tools'
   ```
   On **openSUSE**, these dependencies may be obtained by running the
   following commands;
   ```
      sudo zypper install automake autoconf make gnome-common \
                          libuuid-devel parted-devel gtkmm3-devel \
                          gnome-doc-utils-devel docbook-xsl-stylesheets
      sudo zypper install -t pattern devel_c_c++
   ```
   Again, build GParted with the default configuration and install into
   the default location of /usr/local using:
   ```
      ./configure
      make
      sudo make install
   ```
   If you wish to build this package without the help documentation use
   the --disable-doc flag:
      E.g., `./configure --disable-doc`

   Scrollkeeper is needed on GNOME/Gtk 2 desktops to update the OMF
   catalog files to allow yelp to display the GParted help manual.  On
   GNOME/Gtk 3 desktops scrollkeeper in not required.  Use can be
   explicitly disabled with the --disable-scrollkeeper flag:
      E.g., `./configure --disable-scrollkeeper`
   Note that the configure script will automatically disable
   scrollkeeper if the scrollkeeper-update command is not found.

   If you wish to build this package to use native libparted /dev/mapper
   dmraid support use the --enable-libparted-dmraid flag:
      E.g., `./configure --enable-libparted-dmraid`

   If you wish to build this package with online resize support then
   the following is required:
      a)  Linux kernel version 3.6 or higher.
      b)  Libparted with online resize support.  Either:
          i)  Libparted version 3.2 or later which includes online
              resize support as standard.  In this case GParted is
              automatically built with online resize support.
          ii) Online resize support back ported into an earlier version
              of libparted.  This is only known to be included in Debian
              and derived distributions with parted version 2.3-14 and
              higher.  In this case online resize support must be
              specifically enabled with the --enable-online-resize flag:
                E.g., `./configure --enable-online-resize`

   If you wish to build GParted to allow it to use xhost to grant root
   access to the X11 server use the --enable-xhost-root flag.  This is
   required to allow GParted to display under Wayland.
      `./configure --enable-xhost-root`

   Please note that more than one configure flag can be used:
      E.g., `./configure --disable-doc --enable-libparted-dmraid`

   The INSTALL file contains further GNU installation instructions.

c. Installing polkit's Action File

   GParted needs to run as root therefore it needs a graphical switch
   user program to allow normal users to run it.  Most desktops now use
   polkit as their preferred authorisation mechanism.  Therefore
   ./configure looks for polkit's pkexec as the first choice with
   fallbacks in order being: gksudo, gksu, kdesudo and finally xdg-su.
   Also polkit reads action files only from directory
   /usr/share/polkit-1/actions.  Therefore it is likely that a polkit
   action file will need to be installed into this directory.

   To determine if polkit's pkexec program is being used as the
   graphical privilege escalation program examine the output from
   ./configure.  These lines report that pkexec is being used:

      checking for pkexec >= 0.102... 0.112 found

   Where as either of these lines of ./configure output report that
   pkexec is not being used because either it was too old a version or
   it was not found:

      checking for pkexec >= 0.102... 0.96 found

      checking for pkexec >= 0.102... not found

   When GParted is configured with prefix /usr (using command
   ./configure --prefix=/usr) then make install will automatically
   install the polkit action file into the correct directory and no
   further steps need to be taken.  This is typically the case for
   distribution builds of GParted.

   However when GParted is configured with the default prefix of
   /usr/local, or any prefix other than /usr, then the polkit action
   file has to be manually installed into the correct directory.  Also
   it should have a unique file name to avoid overwriting the action
   file from the distribution's package.  Install the polkit action file
   with a unique name including an extra ".local" in the name:
   ```
      sudo install -m 644 org.gnome.gparted.policy \
             /usr/share/polkit-1/actions/org.gnome.gparted.local.policy
   ```
d. Building using a Specific (lib)parted Version

   1) Download the parted version you wish to use (e.g., 3.2) from:

      http://ftp.gnu.org/gnu/parted/

   2) Build and install parted.

      Extract parted tarball, configure, make, and sudo make install.
      Note that by default this will install into /usr/local.

   3) Set environment variables to inform the GParted build system to
      use libparted from /usr/local:
      ```
        export CPPFLAGS=-I/usr/local/include
        export LDFLAGS=-L/usr/local/lib
        export LD_RUN_PATH=/usr/local/lib
        export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
      ```
   4) Build gparted using steps listed above in "Building from Source".

      Note that when you run ./configure you should see the specific
      version of parted listed in the check for libparted >= 1.7.1.

      You will also see the libparted version listed when running
      gparted from the command line.


DIRECTORIES
------------
* compose  - contains String::ucompose() function

* data     - contains desktop icons

* doc      - contains manual page documentation

* help     - contains GParted Manual and international translations

* include  - contains source header files

* lib/gtest
         - contains Google Test C++ test framework libraries

* m4       - contains macro files

* po       - contains international language translations

* src      - contains C++ source code


DISTRIBUTION NOTES
------------------
GParted uses GNU libparted to detect and manipulate devices and
partition tables.  The blkid command is also required to detect those
file systems which libparted doesn't detect.  (The blkid command should
be considered a mandatory requirement).

GParted also queries and manipulates the file systems within those
devices and partitions.  When available, it uses each file system's
specific commands.  The following optional file system specific packages
provide this support:
```
   btrfs-progs / btrfs-tools
   e2fsprogs
   f2fs-tools
   dosfstools
   mtools          - required to read and write FAT16/32 volume labels
                     and UUIDs
   hfsutils
   hfsprogs
   jfsutils
   nilfs-utils
   ntfs-3g / ntfsprogs
   reiser4progs
   reiserfsprogs / reiserfs-utils / reiserfs
   udftools
   xfsprogs, xfsdump
```

For dmraid support, the following packages are required:  

   **dmsetup**         - removes /dev/mapper entries  
   **dmraid**          - lists dmraid devices and creates /dev/mapper
                         entries

For GNU/Linux distribution dmraid support, the following are required:
   - kernel built with Device Mapping and Mirroring built.  From
     menuconfig, it is under Device Drivers -> <something> (RAID & LVM).
   - dmraid drive arrays activated on boot (e.g., dmraid -ay).


For LVM2 Physical Volume support the following command is required:  
   **lvm**             - LVM2 administration tool
And device-mapper support in the kernel.


For accurate detection and reporting of Linux Software RAID Arrays the
following command is required:  

   **mdadm**           - SWRaid administration tool


For LUKS support the following commands are required:  
   **cryptsetup**      - LUKS volume administration tool  
   **dmsetup**         - Device-mapper administration tool


For attempt data rescue for lost partitions, the following package
is required:  
   **gpart**           - guesses PC-type hard disk partitions


Several more commands are optionally used by GParted if found on the
system.  These commands include:

   * **blkid** [mandatory requirement] used to detect file systems
               libparted doesn't, read UUIDs and volume labels
   * **hdparm** used to query disk device serial numbers
   * **udisks** used to prevent automounting of file systems
   * **devkit-disks** used to prevent automounting of file systems
   * **{filemanager}** used in attempt data rescue to display discovered
                       file systems.  (e.g., nautilus, pcmanfm)
   * **hal-lock** used to prevent automounting of file systems
   * **pkexec** used to acquire root privileges in gparted shell
                script wrapper, but only if available when gparted
                source is configured
   * **gksudo** alternatively used to acquire root privileges in
                gparted shell script wrapper, second choice if
                available when gparted source is configured
   * **gksu** alternatively used to acquire root privileges in
              gparted shell script wrapper, third choice if
              available when gparted source is configured
   * **kdesudo** alternatively used to acquire root privileges in
                 gparted shell script wrapper, fourth choice if
                 available when gparted source is configured
   * **xdg-su** alternatively used to acquire root privileges in
                gparted shell script wrapper, last choice if
                available when gparted source is configured
   * **udevinfo** used in dmraid to query udev name
   * **udevadm** used in dmraid to query udev name
   * **yelp** used to display help manual
   * **xhost** used to grant root access to the X11 server, only
               when configured to do so
