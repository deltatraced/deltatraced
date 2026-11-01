---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-entry-e9868c](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-entry-e9868c)

# What?

A raw journal in this case is one where I take notes outside of the clusterline note taking system. I have yet to develop clusterline for vim so that it is available during system installation, but this would be a good idea so we don't have to do this again.

# Raw Journal

2026-07-26 Wk 30 Sun - 14:02 +03:00 | Captured Raw Journal

## Journal

Wk 30 Tue 10:47

````sh
date

# out
Tue Jul 21 10:47:34 UTC 2026
````

Time via C-t is a bit off, My time is 2h55min ahead. 15:00 -> 17:55

We are UTC+03:00.

## Entries

### Grammatical errors I end up finding in the handbook

todo\[t:rem t:status=todo\] Contribute these

(1)

In https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks,

 > 
 > That said, MBR and legacy BIOS boot may still used in virtualized cloud environments such as AWS.

Should be "may still **be** used"

(2)

In https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks,

 > 
 > The official Gentoo boot media provides support for many advanced filesystem and tool setups, which offer more flexible changes, snapshots and some cases more caching abitles

Should be "and **in** some cases, more caching **abilities.**"

(3)

In https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks,

 > 
 > Although usage is not covered in the handbook, below is a list helpful guides to get the system running:

Should be "list **of** helpful guides"

(4)

In https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base,

 > 
 >      * /dev/ is a regular file system which contains all device. It is partially managed by the Linux device manager (usually udev)
 >     

Should be "which contains all **devices**."

(5)

In https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base,

 > 
 > Optionally system administrators can also define accepted licenses per-package as shown in the following directory of files example. Note that the package.license directory will need created if it does not already exist:

Should be "directory will need **to be** created if"

(6)

In https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel,

 > 
 >      * Running file system consistency check fsck, a tool to check and repair consistency of a file system in such events of uncleanly shutdown a system.
 >     

Should be "of a file system **in events such as unclean system shutdown**"

## Done

### Setting up notes access and tmux

Wk 30 Tue 09:56

Temporary file. Writing notes on my phone.

Currently doing the gentoo installation on my PC, so access to notes are limited. I have setup syncthing previously with my phone for my notes,
so now I am SSH'd into my phone (thanks to termux), and I can find my notes under /storage/emulated/0.

Using Gentoo vim alongside tmux. I figured by experimentation (no man files in this installation environment) and memory that I can create a new session with

````sh
tmux new -t {session_id}
````

and switch with

````sh
tmux switch -t {session_id}
````

So I can have a session for notes/terminal browsering of the handbook with links. And another for operations done.

Wk 30 Tue 09:58

For some reason I'm getting a bunch of segfaults with (termux) vi. But note this is not gentoo vim but via an SSH session to my phone!

We could try to install another editor:

````sh
# in phone
pkg install vim
````

Hopefully no segfaults with vim now.

Wk 30 Tue 10:02

Alright so. Not sure how to copy and paste stuff around here. Ctrl+Ins and stuff isn't helping.

Let's check links and see if we get how default tmux handles copy mode, because it's different than what I configured.

````sh
# in user lan
links https://www.duckduckgo.com/
````

--/ Wk 30 Tue 12:04

https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media#Extra_hardware_configuration
recommends not using `links` in root following https://en.wikipedia.org/wiki/Principle_of_least_privilege. Make sure to first create a user:

````sh

# set a password for root
passwd

# create a new user
useradd -m -G users,wheel myuser
passwd myuser

# switch to myuser
su - myuser
````

Updating my links commands to reflect that we are in my user shell.

--/

Left/Right navigates history. Up/Down navigates elements. Though I'd like to be able to jump to an element that's char coded...

Use PU/PD for navigating content up and down. p and l for right/left. (View F1 for key shortcuts)

In tmux I move back and forth with my notes in the session. I can create horizontal panes with C-b ", I can
create new tabs with C-b c, I can detach from tmux with C-b d, I can close tabs with C-b x y, I can switch back and forth between tabs with C-b p, I can switch between horizontal panes with C-b arrow.

Wk 30 Tue 10:28

Article recommends setting vim mode keybindings for tmux:

````sh
tmux setw -g mode-keys vi
````

````sh
tmux show-option -gw mode-keys

# out
mode-keys vi
````

Ooh! That's all it took. Now it's much more familiar to the copy mode I know. I guess the primary issue was that it defaults to emacs mode.

The only difference is instead of `y` by default you press Enter to initiate the copy of the region.

The article is in https://www.terminal.guide/tools/multiplexer/tmux/copy-mode-guide/#what-is-copy-mode (now that we have copy-paste powers with tmux!)

They also recommend to persist the settings. It shouldn't matter much for us right now, but it would still be annoying if I exit tmux that I have to redo it. Let's just do it:

````sh
# in /root/.tmux.conf
setw -g mode-keys vi
````

Now just update this settings, and apply with `tmux source /root/.tmux.conf`.

Also, check backups for some more basic configuration. No plugins yet at this stage.
--/

Wk 30 Tue 10:35

Okay so how do we quickly switch between tmux sessions by default? I know I can type `tmux switch -t1` and `tmux switch -t2`.

By the way I've been getting the time with tmux's `prefix - t` or `C-b t` which is fun.

https://tmuxai.dev/tmux-change-session/

These guys helped: Just use `C-b s up Enter` to switch!

--/ Wk 30 Tue 17:17

Actually you can also use `/` to search the sessions; this makes it so we can actually go to session by label, just as I did with my customized tmux!

So for example: `C-b s /notes Enter Enter`.

Remember to use C-b d to detach in order to create new sessions with `tmux new -tsession_name`

--/

Yay! Through tmux I have quick context switching between execution and notes/browser. I have good copy pasting support, and we have access to our notes through a remote device. Let's carry on with the installation.

Though note on my live USB environment, I can also toggle between different terminals with alt+Arrow. But I prefer to use tmux only for my switching.

### Setting up Gentoo Up to boot

Wk 30 Tue 10:47

* https://wiki.gentoo.org/wiki/Handbook
* -> https://wiki.gentoo.org/wiki/Handbook:AMD64
* -> Last we reached is the Booting section of https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media

````sh
# in user lan
links https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media
````

Hmm. Let me try to reboot and see the option that was selected. It should've defaulted to some first option, but they are talking about a progress bar, and I'm not sure what they're referring to.

Wk 30 Tue 11:10

Ok I rebooted. Basically it has me select the kernel from four options, and I have 15 seconds to respond:

* Boot LiveCD (kernel: gentoo)
* Boot LiveCD (kernel: gentoo) (cached)
* Boot LiveCD (kernel: gentoo) (acessibility)
* Memtest86+ 64bit UEFI

 > 
 > At the boot prompt, users get the option of displaying the available kernels (F1) and boot options (F2).

Maybe we should try to press F1 and F2 and see what happens. We haven't tried that.

Wk 30 Tue 11:51

Anyway let's just process with the gentoo kernel. I was unable to get any effect from F1 or F2 in that menu. I was only able to select it.

15:00 (It's actually 17:55; adjust accordingly for all.)

https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Networking

Let's do some network testing. Though everything works fine over eithernet by default:

````sh
ip route

ping -c 3 1.1.1.1

# out {
	PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
	64 bytes from 1.1.1.1: icmp_seq=2 ttl=55 time=59.9 ms
	64 bytes from 1.1.1.1: icmp_seq=3 ttl=55 time=84.1 ms

	--- 1.1.1.1 ping statistics ---
	3 packets transmitted, 2 received, 33.3333% packet loss, time 2011ms
	rtt min/avg/max/mdev = 59.930/72.033/84.137/12.103 ms
# }

curl --location gentoo.org --output /dev/null

# out {
	  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
					 Dload  Upload  Total   Spent   Left   Speed
	  0      0   0      0   0      0      0      0                              0
	100    162 100    162   0      0     95      0   00:01   00:01              0
	100  24052 100  24052   0      0   9106      0   00:02   00:02              0
# }

````

Wk 30 Tue 17:31

https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks

* Block devices are handles for an abstract disk (interface). They can be used independently of their implementation details as storage devices, and can be treated as a contriguous block of random-access bytes,
  despite how they may be physically represented.

Wk 30 Wed 03:00

Let's check the block devices we have on our system to prepare for partitioning:

````sh
lsblk
````

To see information about the type system of your device blocks:

````sh
lsblk --help

# out (relevant)
 -f, --fs             output info about filesystems
````

Use `lsblk --fs`.

For any block device, we can `mount /dev/{blkdev} /path/to/mount` and unmount with `umount /dev/{blkdev}`. It also accepts the path mounted instead of the device.

Use this to confirm where originally our / was. Let's call that block device `/dev/{blkhome}`. Let's call our primary disk /dev/{diskpri}.

Wk 30 Wed 04:25

Creating an EFI System Partition (ESP),

````sh
fdisk /dev/{diskpri}
````

I deleted my EFI partition from my ubuntu system. We'll recreate this. For the Swap partition, let's aim for half the size as RAM amount we have.
It doesn't seem like it needs to be this much, but in my case this is neglegable and also
there were cases like hibernation that make use of full size, but likely do not apply to me.

The rename partition function will not modify the partition identifier itself. It just adds a name attribute, which can be removed by setting the name to no input.

In my case, it was in partition 4 the 1 gig recommended, rather than in 1 from sector 2048. This worked before, so it should be okay. No need to try to reorder my existing partitions.

Let's recreate partition 4 with 1 gig.

It still recognizes the partition has a vfat signature, but let's reset this ourselves:

````
Created a new partition 4 of type 'Linux filesystem' and of size 1 GiB.
Partition #4 contains a vfat signature.

Do you want to remove the signature? [Y]es/[N]o:
````

So selecting \[Y\]es.

Then change the type explicitly (I did after saving with w) to EFI System (1).

Mounting into that new block device, I can still see we have `EFI/ubuntu/`, so the act of deleting and recreating the partition did not operate on the underlying data.

We should be aware if this will become a problem as we configure boot.

Wk 30 Wed 05:14

Creating Swap Partition,

~~We decided on RAM/2. Do that amount: +{RAM / 2}G when creating the sector via fdisk.~~

--/ Wk 30 Wed 07:16

Let's just do the RAM amount. It shouldn't possibly need more, even though we don't know if it's going to need it all, and expect it to need less. We have the space to spare.

We know at least one case where it can possibly use it all, although it may not apply to us. (hybernation).

--/

This is going to be trickier for me, since I am not starting from a new partition table.

I need to resize one of my partitions to free up this space for swap (somehow I didn't have a swap partition before!)

https://unix.stackexchange.com/questions/505045/resize-partition-with-fdisk-without-data-loss

The advice is to back up data when doing operations like this. Let's do that. I have a sector in partition 1 that is just data. Let's delete anything unimportant, backup the rest in a different disk, then delete this partition.

Wk 30 Wed 07:05

Backed up everything. Will delete the partition and create two in its place, swap, and a new one.

Deleted partition 2. The good news is that we also prior deleted partition 1 which was unrelated to this. Now we can give a more proper ordering to this! We can delete partition 4 for the ESP and recreate it as partition 1!

Set the partition type for partition 2 to 19 (Linux Swap).

Okay, so let's create a new partition for the remaining space for our old deleted partition 2. This is not the root partition.

Wk 30 Wed 07:37

Reached Section 'Creating file systems' in https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks.

They suggest to use `smartctl` to check if we need to update the firmware of our SSD storage devices.

````sh
smartctl -a /dev/{diskpri}
````

I've been using ext4, but gentoo recommends xfs, which has features it does not. This includes reflinks and copy on write operations which the handbook says benefits gentoo high compiling load operations.

Its notable downside is no partition shrinking; but I can accept this for my use case here. I'm going to move all content out of the primary disk to port over from ext4 to xfs.

https://www.starwindsoftware.com/blog/xfs-vs-ext4/

 > 
 > * \[for ext4\] Robust volume size support: Manages filesystems up to 1 Exabyte and files up to 16 Terabytes, more than enough for almost any desktop or small server task.

Did not know about this max limit. Not that I have ever even gotten close to creating a 16Tb file!

https://blogs.oracle.com/linux/xfs-data-block-sharing-reflink

So from this it seems that xfs gives us the ability to copy files with reflinks enabled, and this copying is very quick and free and does not consume storage. Then when the copied file is modified, only the parts of the shared content that are modified are tracked by xfs! It's like a patching sort of system out of the original big shared data! Now you have one big file, and a modified reflink off of it, for the storage of just the big file plus the patch, rather than two big files as we would have to do with ext4.

This kind of copy on write reminds me of immutable data structures that come with structural updates. Which also can use a tree data structure and manage patches rather than copy over big contiguous blocks of memory over and over.

(aside)

````
vi --help

# out
BusyBox v1.36.1 (2026-02-15 17:54:45 -00) multi-call binary.

Usage: vi [-c CMD] [-R] [-H] [FILE]...

Edit FILE

        -c CMD  Initial command to run ($EXINIT and ~/.exrc also available)
        -R      Read-only
        -H      List available features

````

Interestingly when I used `V` it said it's not implemented for this.

(/aside)

Wk 30 Wed 12:48

At this point we should have a fully partitioned system and we should know the paths for each device block. We need /dev/{blkesp} (EFI System Partition (ESP), /dev/{blkswap}, /dev/{blkroot},
and in my case /dev/{blkextra} for a final partition that takes the remaining of the space. Let's initialize a filesystem for each one (and hence; format them):

````sh
mkfs.vfat -F 32 /dev/{blkesp}
mkswap /dev/{blkswap}
mkfs.xfs -c options=/usr/share/xfsprogs/mkfs/lts_6.18.conf /dev/{blkroot}
mkfs.xfs -c options=/usr/share/xfsprogs/mkfs/lts_6.18.conf /dev/{blkextra}
````

Make sure to enable swap during the live image environment:

````sh
swapon /dev/{blkswap}
````

````
# in /usr/share/xfsprogs/mkfs/lts_6.18.conf
# V5 features that were the mkfs defaults when the upstream Linux 6.18 LTS
# kernel was released at the end of 2025.
````

Noteably, it has the option `reflink=1`.

Wk 30 Wed 14:45

Mount the partitions,

````sh
mkdir -p /mnt/gentoo/efi
mount /dev/{blkroot} /mnt/gentoo
mount /dev/{blkesp} /mnt/gentoo/efi
````

We will also need this:

````sh
mkdir -p /mnt/gentoo/tmp
chmod 1777 /mnt/gentoo/tmp
````

From https://www.man7.org/linux/man-pages/man2/chmod.2.html,

````
1777 Has the bits

1000
        S_ISVTX  (01000)
               sticky bit (restricted deletion flag, as described in
               unlink(2))
0100
        S_IXUSR  (00100)
               execute/search by owner ("search" applies for directories,
               and means that entries within the directory can be
               accessed)
0200
        S_IWUSR  (00200)
               write by owner
0400
        S_IRUSR  (00400)
               read by owner
0010
        S_IXGRP  (00010)
               execute/search by group
0020
        S_IWGRP  (00020)
               write by group
0040
        S_IRGRP  (00040)
               read by group
0001
        S_IXOTH  (00001)
               execute/search by others
0002
        S_IWOTH  (00002)
               write by others
0004
        S_IROTH  (00004)
               read by others
````

Wk 30 Wed 15:00

We reached https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage.

Seems we need to select a [profile](https://wiki.gentoo.org/wiki/Profile_(Portage)). They also really recommend against nomultilib option.

I guess ideally I want only 64-bit, but fallback is fine as a last resort.

So far my choices:

* OpenRC for init system; I wanna try something other than systemd! From what I read, this is one point that comes up a lot that people appreciate Gentoo for.
* Multilib; Seems to be the safer and more inclusive option for later choices.

We need to update our time, apparently it can interfere with download speeds or cause unpredictable errors.

````sh
chronyd -q

# out
2026-07-22T15:21:24Z chronyd version 4.8 starting (+CMDMON +REFCLOCK +RTC +PRIVDROP +SCFILTER -SIGND +NTS +SECHASH +IPV6 -DEBUG)
2026-07-22T15:21:24Z Wrong owner of /run/chrony (UID != 0)
2026-07-22T15:21:24Z Disabled command socket /run/chrony/chronyd.sock
2026-07-22T15:21:24Z Running with root privileges
2026-07-22T15:21:29Z System clock wrong by -296.990122 seconds (step)
2026-07-22T15:16:33Z chronyd exiting
````

Ouch what an offset. Though still says 15:17 and it's 18:17 here. But this is probably a locale problem instead.

 > 
 > UTC time is recommended for all Linux systems. Later, a system timezone is defined, which changes the offset when the date is displayed.

That is interesting. I guess this is more consistent, and if you know UTC, you can derive all others, so they can be treated as derivative.

Wk 30 Wed 15:19

````sh
date

# out
Wed Jul 22 15:19:46 UTC 2026
````

I guess the drift happens fast:

````
2026-07-22T15:22:40Z System clock wrong by -0.101484 seconds (step)
2026-07-22T15:22:50Z System clock wrong by 0.022495 seconds (step)
````

````sh
links https://www.gentoo.org/downloads/mirrors/
````

->

````sh
# in /mnt/gentoo
rsync rsync://mirror.aarnet.edu.au/pub/gentoo/

# out (relevant)
 * rsync://mirror.aarnet.edu.au/pub/

   Directories under the `pub` module are now generally provided as top-level rsync modules.

   For example, rsync://mirror.aarnet.edu.au/pub/debian/ is now available as rsync://mirror.aarnet.edu.au/debian/.
````

todo\[t:rem t:status=todo\]: We need to probably update this for the Gentoo mirrors website.

->

````sh
# in /mnt/gentoo
rsync rsync://mirror.aarnet.edu.au/gentoo/

# out (relevant)
drwxr-xr-x              7 2026/03/10 04:42:50 .
drwxr-xr-x            290 2026/07/22 11:52:01 distfiles
drwxr-xr-x             23 2026/07/22 10:35:00 experimental
drwxrwxr-x              6 2026/03/10 15:59:40 pub
drwxr-xr-x             22 2023/10/09 04:48:40 releases
drwxrwxr-x            110 2026/07/22 00:51:52 snapshots
````

Wk 30 Thu 02:55

https://www.man7.org/linux/man-pages/man1/rsync.1.html

We could use `rsync -avz rsync://mirror.aarnet.edu.au/gentoo/ .` to download it as an archive, but as it is, it has too many unrelated files. We need to find a relevant stage file.

The handbook says to see the files under `releases/amd64/autobuilds/`. View the list with `rsync rsync://mirror.aarnet.edu.au/gentoo/releases/amd64/autobuilds/ | less`

We can grep for the features we're interested in:

````sh
rsync rsync://mirror.aarnet.edu.au/gentoo/releases/amd64/autobuilds/ | grep 'desktop\|openrc' | grep -v 'systemd\|nomultilib' | less
````

https://wiki.gentoo.org/wiki/Stage_file explains that we should seek stage3. stage4 is more specialized for extra things configured on top of stage3.

https://wiki.gentoo.org/wiki/Hardened_Gentoo explained the hardened keyword

On ABIs:

* https://wiki.gentoo.org/wiki/Musl
* post https://superuser.com/questions/1716835/on-which-systems-should-i-prefer-musl-or-gnu-binaries
  * User mentions that musl may be slower for some rust applications

For now will go with mainstream glibc.

This leaves us with this choice: `current-stage3-amd64-desktop-openrc`

(issue)

````sh
# in /mnt/gentoo
mkdir stage
cd stage
rsync -auz rsync://mirror.aarnet.edu.au/gentoo/releases/amd64/autobuilds/current-stage3-amd64-desktop-openrc .
````

These only give us links:

````sh
lrwxrwxrwx 1  300  300   75 Jun 10 23:01 stage3-amd64-desktop-openrc-20260610T214636Z.tar.xz.asc -> ../20260610T214636Z/stage3-amd64-desktop-openrc-20260610T214636Z.tar.xz.asc
lrwxrwxrwx 1  300  300   75 Jun 14 20:01 stage3-amd64-desktop-openrc-20260614T170130Z.tar.xz.asc -> ../20260614T170130Z/stage3-amd64-desktop-openrc-20260614T170130Z.tar.xz.asc
lrwxrwxrwx 1  300  300   75 Jun 21 18:21 stage3-amd64-desktop-openrc-20260621T164603Z.tar.xz.asc -> ../20260621T164603Z/stage3-amd64-desktop-openrc-20260621T164603Z.tar.xz.asc
````

Add the `-L` flag to `rsync` so that we can transform the links to files:

````sh
# in /mnt/gentoo
mkdir stage
cd stage
rsync -auzL rsync://mirror.aarnet.edu.au/gentoo/releases/amd64/autobuilds/current-stage3-amd64-desktop-openrc .
````

I thought these were links due to the red color, but they're actual content:

````
-rw-r--r-- 1  300  300 827372140 Jul 19 17:55 stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz
-rw-r--r-- 1  300  300   1485586 Jul 19 17:55 stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.CONTENTS.gz
````

(/issue)

````sh
# in /mnt/gentoo
mkdir stage
cd stage
rsync -auzL rsync://mirror.aarnet.edu.au/gentoo/releases/amd64/autobuilds/current-stage3-amd64-desktop-openrc .
````

````sh
# in /mnt/gentoo/stage/current-stage3-amd64-desktop-openrc
tree -a .

# out
.
├── latest-stage3-amd64-desktop-openrc.txt
├── stage3-amd64-desktop-openrc-20260610T214636Z.tar.xz.asc
├── stage3-amd64-desktop-openrc-20260614T170130Z.tar.xz.asc
├── stage3-amd64-desktop-openrc-20260621T164603Z.tar.xz.asc
├── stage3-amd64-desktop-openrc-20260628T170101Z.tar.xz.asc
├── stage3-amd64-desktop-openrc-20260705T170105Z.tar.xz.asc
├── stage3-amd64-desktop-openrc-20260712T170110Z.tar.xz.asc
├── stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz
├── stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.CONTENTS.gz
├── stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.DIGESTS
├── stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.asc
└── stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.sha256
````

Wk 30 Thu 05:11

Let's verify the files.

From "Verifying and validating" section of https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage,

* I had to pass a `*.asc` for `gpg --verify`, but the handbook specified a `*.tar.xz`.

````sh
gpg --import /usr/share/openpgp-keys/gentoo-release.asc

# out {
    gpg: directory '/root/.gnupg' created
    gpg: key A13D0EF1914E7A72: 1 signature not checked due to a missing key
    gpg: /root/.gnupg/trustdb.gpg: trustdb created
    gpg: key A13D0EF1914E7A72: public key "Gentoo repository mirrors (automated git signing key) <repomirrorci@gentoo.org>" imported
    gpg: key DB6B8C1F96D8BF6D: 2 signatures not checked due to missing keys
    gpg: key DB6B8C1F96D8BF6D: public key "Gentoo ebuild repository signing key (Automated Signing Key) <infrastructure@gentoo.org>" imported
    gpg: key 9E6438C817072058: 1 signature not checked due to a missing key
    gpg: key 9E6438C817072058: public key "Gentoo Linux Release Engineering (Gentoo Linux Release Signing Key) <releng@gentoo.org>" imported
    gpg: key BB572E0E2D182910: 1 signature not checked due to a missing key
    gpg: key BB572E0E2D182910: public key "Gentoo Linux Release Engineering (Automated Weekly Release Key) <releng@gentoo.org>" imported
    gpg: Total number processed: 4
    gpg:               imported: 4
    gpg: no ultimately trusted keys found
# }

gpg --verify stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.asc

# out {
    gpg: assuming signed data in 'stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz'
    gpg: Signature made Sun Jul 19 18:21:07 2026 UTC
    gpg:                using RSA key 534E4209AB49EEE1C19D96162C44695DB9F6043D
    gpg: Good signature from "Gentoo Linux Release Engineering (Automated Weekly Release Key) <releng@gentoo.org>" [unknown]
    gpg: WARNING: This key is not certified with a trusted signature!
    gpg:          There is no indication that the signature belongs to the owner.
          13EBBDBEDE7A12775DFDB1BABB572E0E2D182910
          534E4209AB49EEE1C19D96162C44695DB9F6043D
# }

gpg --output stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.DIGESTS.verified --verify stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.DIGESTS

# out {
    gpg: Signature made Sun Jul 19 18:21:08 2026 UTC
    gpg:                using RSA key 534E4209AB49EEE1C19D96162C44695DB9F6043D
    gpg: Good signature from "Gentoo Linux Release Engineering (Automated Weekly Release Key) <releng@gentoo.org>" [unknown]
    gpg: WARNING: This key is not certified with a trusted signature!
    gpg:          There is no indication that the signature belongs to the owner.
          13EBBDBEDE7A12775DFDB1BABB572E0E2D182910
          534E4209AB49EEE1C19D96162C44695DB9F6043D
# }

cat stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.DIGESTS.verified

# out {
    # SHA512 HASH
    65c8e11b3dceb52699d67f12d7c0985243fcd63669763b411028436942d2e79f51d0faa251bc7e13262e7182d564265757d1bd039ce937d78464c4549667aae9  stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz
    # BLAKE2B HASH
    919af6dd532e978145c33c2ee653e9845012d47fd268df5f86ff1a757dc4a06ae69df519471d15a6f0428030011e955467b4c057d9ec6a8ac94e902297eddd3f  stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz
    # SHA512 HASH
    fbdd3fe6c181eede5cad45189b8669e2564ea12d666a41bf0c092e5b057d9567852d0564a6a357149653f9a1576f894453c9f1e035d0ba4ff1945dbfaaeae267  stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.CONTENTS.gz
    # BLAKE2B HASH
    0128cbd6ec8d000a8c7eb1a68b299770ad1791ef188240b7f308dbaed6443b7216883bf65f9fc2b81900ddfc05d13995e6fdcccc103a949555ffa0885564b03f  stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.CONTENTS.gz
# }

￼
￼
gpg --output stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.sha256.verified --verify stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.sha256

# out {
    gpg: Signature made Sun Jul 19 18:21:08 2026 UTC
    gpg:                using RSA key 534E4209AB49EEE1C19D96162C44695DB9F6043D
    gpg: Good signature from "Gentoo Linux Release Engineering (Automated Weekly Release Key) <releng@gentoo.org>" [unknown]
    gpg: WARNING: This key is not certified with a trusted signature!
    gpg:          There is no indication that the signature belongs to the owner.
          13EBBDBEDE7A12775DFDB1BABB572E0E2D182910
          534E4209AB49EEE1C19D96162C44695DB9F6043D
# }

cat stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.sha256.verified

# out {
    # SHA256 HASH
    9c5c8ded84cb759c00038f07608ff23af2a7fb1423bab12cba5aec06fb28c9ac  stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz
# }

# Checking the checksums in the *.DIGESTs.verified:

verify_checksum() {
    checksum=$1
    checksum_grep=$2
    
    # *.DIGESTS.verified contains two files per checksum *.tar.xz and *.CONTENTS.gz. 

    act=$(openssl dgst -r -$checksum stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz | cut -d' ' -f1)
    exp=$(cat stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.DIGESTS.verified | grep $checksum_grep -A1 | head -n2 | tail -n1 | cut -d' ' -f1)

    act_content=$(openssl dgst -r -$checksum stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.CONTENTS.gz | cut -d' ' -f1)
    exp_content=$(cat stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.DIGESTS.verified | grep $checksum_grep -A1 | tail -n2 | tail -n1 | cut -d' ' -f1)

    echo "For *.tar.xz:"
    if [ $act = $exp ]; then 
        echo -e "OK\n(act $act)\n(exp $exp)"; 
    else 
        echo -e "Bad\n(act $act)\n(exp $exp)"; 
    fi

    echo "For *.CONTENTS.gz:"
    if [ $act_content = $exp_content ]; then 
        echo -e "OK\n(act $act_content)\n(exp $exp_content)"; 
    else 
        echo -e "Bad\n(act $act_content)\n(exp $exp_content)"; 
    fi
}

verify_checksum sha512 SHA512

# out {
For *.tar.xz:
OK
(act 65c8e11b3dceb52699d67f12d7c0985243fcd63669763b411028436942d2e79f51d0faa251bc7e13262e7182d564265757d1bd039ce937d78464c4549667aae9)
(exp 65c8e11b3dceb52699d67f12d7c0985243fcd63669763b411028436942d2e79f51d0faa251bc7e13262e7182d564265757d1bd039ce937d78464c4549667aae9)
For *.CONTENTS.gz:
OK
(act fbdd3fe6c181eede5cad45189b8669e2564ea12d666a41bf0c092e5b057d9567852d0564a6a357149653f9a1576f894453c9f1e035d0ba4ff1945dbfaaeae267)
(exp fbdd3fe6c181eede5cad45189b8669e2564ea12d666a41bf0c092e5b057d9567852d0564a6a357149653f9a1576f894453c9f1e035d0ba4ff1945dbfaaeae267)
# }


verify_checksum blake2b512 BLAKE2B

# out {
For *.tar.xz:
OK
(act 919af6dd532e978145c33c2ee653e9845012d47fd268df5f86ff1a757dc4a06ae69df519471d15a6f0428030011e955467b4c057d9ec6a8ac94e902297eddd3f)
(exp 919af6dd532e978145c33c2ee653e9845012d47fd268df5f86ff1a757dc4a06ae69df519471d15a6f0428030011e955467b4c057d9ec6a8ac94e902297eddd3f)
For *.CONTENTS.gz:
OK
(act 0128cbd6ec8d000a8c7eb1a68b299770ad1791ef188240b7f308dbaed6443b7216883bf65f9fc2b81900ddfc05d13995e6fdcccc103a949555ffa0885564b03f)
(exp 0128cbd6ec8d000a8c7eb1a68b299770ad1791ef188240b7f308dbaed6443b7216883bf65f9fc2b81900ddfc05d13995e6fdcccc103a949555ffa0885564b03f)
# }


sha256sum --check stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz.sha256.verified

# out {
    stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz: OK
# }
````

OK

Wk 30 Thu 07:04

Let's install the verified stage file.

(quote)

From "Installing a stage file" section of https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage,

````
   root #tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner -C /mnt/gentoo

   Before extracting verify the options:

     * x extract, instructs tar to extract the contents of the archive.
     * p preserve permissions.
     * v verbose output.
     * f file, provides tar with the name of the input archive.
     * --xattrs-include='*.*' Preserves extended attributes in all namespaces stored in the archive.
     * --numeric-owner Ensure that the user and group IDs of files being extracted from the tarball remain the same as Gentoo's release engineering team intended (even if adventurous users are not using official Gentoo live environments
       for the installation process).
     * -C /mnt/gentoo Extract files to the root partition regardless of the current directory.
````

(/quote)

````sh
tar xpvf stage3-amd64-desktop-openrc-20260719T170103Z.tar.xz --xattrs-include='*.*' --numeric-owner -C /mnt/gentoo
````

Why does this add `/mnt/gentoo/opt/rust-bin-1.95.0/`? I am intending for a fully source-based installation.

* https://forums.gentoo.org/viewtopic.php?t=1171787
* https://serverfault.com/questions/1034576/install-gentoo-do-you-need-internet-stage-3
* https://forums.gentoo.org/viewtopic.php?t=1033802
  * Claims that the precompiled tools in stage3 are a minimal set of tools of the install and compiling them yourself will not change the optimizations done in the post stage 3 builds
* https://www.reddit.com/r/Gentoo/comments/wcdu7r/deciding_on_stage3/
  * Some discussion on musl use and security

so it is a precompiled seed/toolchain to continue the installation from. particularing including things that need bootstrapping like gcc.

it is the standard route for installing however, so we can look further later for replacing any binaries when we have a working system.

Wk 30 Thu 08:33

Check /mnt/gentoo/usr/share/portage/config/make.conf.example for examples. We edit the configuration at /mnt/gentoo/etc/portage/make.conf.

https://dev.gentoo.org/~zmedico/portage/doc/man/make.conf.5.html

````diff
# in /mnt/gentoo/etc/portage/make.conf

-COMMON_FLAGS="-O2 -pipe"
+COMMON_FLAGS="-march=native -O2 -pipe"

+RUSTFLAGS="${RUSTFLAGS} -C target-cpu=native"
````

(quote)

From "Configuring compile options" section of https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage,

````
  MAKEOPTS
   [...]
   A good choice is the smaller of: the number of threads the CPU has, or the total amount of system RAM divided by 2 GiB.
   [...]
   A good recommendation is to have at least 2 GiB of RAM for every job specified (so, e.g. -j6 requires at least 12 GiB).
````

(/quote)

````sh
lsmem | grep "Total online" | rev | cut -d' ' -f1 | rev # out { 66G }
nproc                                                   # out { 24 }
````

nproc is smaller for me: (#\_ nproc 24) \< (#\_ halfram 33). Since it will default to nproc, I will not modify.

Wk 30 Thu 10:40

Reached https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base

Copy over DNS info,

````sh
cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
````

Let's chroot in!

````sh
arch-chroot /mnt/gentoo

# in chroot /mnt/gentoo
export PS1="(chroot) ${PS1}"
````

Note since I am still using tmux in the live image environment, this needs to be done on a per-pane basis.

You can check /mnt/. Does it still have /mnt/gentoo? as a way to test which environment you're in. Otherwise, try to use `gcc` or `rustc`. I have not installed this on the live image environment.

And of course, setting `export PS1="(chroot) ${PS1}"` can help too.

Wk 30 Thu 11:10

Getting the esbuild repository snapshot,

````sh
# in chroot /mnt/gentoo
emerge-webrsync

# out (relevant)
 * IMPORTANT: 22 news items need reading for repository 'gentoo'.
 * Use eselect news read to view new items.
````

`idea[t:status=none t:tags="+config +network +gentoo"]` The handbook mentions that we can configure a mirror here for faster snapshot updates.

````sh
# in chroot /mnt/gentoo

eselect profile show

# out {
Current /etc/portage/make.profile symlink:
  default/linux/amd64/23.0/desktop
# }

cat /etc/portage/binrepos.conf/gentoo.conf

# out {
    # These settings were set by the catalyst build script that automatically
    # built this stage.
    # Please consider using a local mirror.

    [gentoo]
    priority = 1
    sync-uri = https://distfiles.gentoo.org/releases/amd64/binpackages/23.0/x86-64
    location = /var/cache/binhost/gentoo
    verify-signature = true
# }

emerge --info | grep ^USE

# out {
    [added line breaks]
    USE="X a52 aac acl acpi alsa amd64 avif bluetooth branding bzip2 cairo cdda cdr cet crypt cups dbus dri dts dvd dvdr elogind encode exif flac gdbm gif gpm gtk gui iconv icu ipv6 jpeg jpegxl lcms libnotify libtirpc mad mng mp3 mp4 mpeg multilib ncurses nls ogg opengl openmp pam pango pcre pdf pipewire png policykit ppds pulseaudio qml qt6 readline screencast sdl seccomp sound spell ssl startup-notification svg test-rust tiff truetype udev udisks unicode upower usb vorbis vulkan wayland webp wxwidgets x264 xattr xcb xft xml xv zlib"

    ABI_X86="64" ADA_TARGET="gcc_15" APACHE2_MODULES="authn_core authz_core socache_shmcb unixd actions alias auth_basic authn_anon authn_dbm authn_file authz_dbm authz_groupfile authz_host authz_owner authz_user autoindex cache cgi cgid dav dav_fs dav_lock deflate dir env expires ext_filter file_cache filter headers include info log_config logio mime mime_magic negotiation rewrite setenvif speling status unique_id userdir usertrack vhost_alias" CALLIGRA_FEATURES="karbon sheets words" COLLECTD_PLUGINS="df interface irq load memory rrdtool swap syslog" CPU_FLAGS_X86="mmx mmxext sse sse2" ELIBC="glibc" GPSD_PROTOCOLS="ashtech aivdm earthmate evermore fv18 garmin garmintxt gpsclock greis isync itrax navcom oncore skytraq superstar2 tsip tripmate tnt" GUILE_SINGLE_TARGET="3-0" GUILE_TARGETS="3-0" INPUT_DEVICES="libinput" KERNEL="linux" LCD_DEVICES="bayrad cfontz glk hd44780 lb216 lcdm001 mtxorb text" LLVM_TARGETS="X86" LUA_SINGLE_TARGET="lua5-1" LUA_TARGETS="lua5-1" OFFICE_IMPLEMENTATION="libreoffice" PHP_TARGETS="php8-3" POSTGRES_TARGETS="postgres17" PYTHON_SINGLE_TARGET="python3_14" PYTHON_TARGETS="python3_14" QEMU_SOFTMMU_TARGETS="x86_64" RUBY_TARGETS="ruby33" VIDEO_CARDS="amdgpu fbdev intel nouveau radeon radeonsi vesa dummy" XTABLES_ADDONS="quota2 psd pknock lscan length2 ipv4options ipp2p iface geoip fuzzy condition tarpit sysrq proto logmark ipmark dhcpmac delude chaos account"
# }
````

Find the USE values that can be used at `/var/db/repos/gentoo/profiles/use.desc`.

I have an nvidia GPU. https://wiki.gentoo.org/wiki/NVIDIA

 > 
 > For best performance in heavy 3D workloads and games, the proprietary driver is usually preferred, especially on newer hardware.

Sad. I am a gamer; so I will default to properiety for now.

````sh
# in /mnt/gentoo/etc/portage/package.use/00video_cards
*/* VIDEO_CARDS: -* nvidia
````

--/ Wk 30 Sun 09:02 +03:00
We need to also install firmware drivers here. In my case following https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers
--/

Gentoo Linux Enhancement Proposal 23 (GLEP 23) gives us a concept of `license groups`. Let's set it to OSI-approved FOSS and similar. This should be the default. Any exceptions should be made explicit:

````diff
# in /mnt/gentoo/etc/portage/make.conf
ACCEPT_LICENSE="-* @FREE"
````

--/ Wk 30 Thu 13:05

idea\[t:topic-contrib t:subtopic=clarify t:status=mightdo\] Handbook refers to unspecified env variable. Maybe we can suggest clarification here.

in https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base,

 > 
 > user $portageq envvar ACCEPT_LICENSE

See https://wiki.gentoo.org/wiki/Portageq.

--/

 > 
 > Readers who are performing an 'install Gentoo speed run' may safely skip @world set updates until after their system has rebooted into the new Gentoo environment.

A speed run! I'm more doing the slow run route.

````sh
# in chroot /mnt/gento
emerge --ask --verbose --update --deep --changed-use @world
````

Hmm. We're installing some X11 stuff while planning to go to wayland right now. Something to look into later.

Wk 30 Thu 15:55

Next is to check the dependency cleaning, and see if we need to keep anything with `emerge --noreplace foo`:

````sh
# in chroot /mnt/gentoo
emerge --ask --pretend --depclean
````

This removes obsolete packages apparently, I do not need to keep anything after review.

````sh
# in chroot /mnt/gentoo
emerge --ask --depclean
````

Setting up timezone. Need to choose location, but will be like `ln -sf ../usr/share/zoneinfo/Europe/Brussels /etc/localtime`.

List of locales: /usr/share/i18n/SUPPORTED

Edit /etc/locale.gen with for example `en_US.UTF-8 UTF-8`

Then generate with `locale-gen` and verify with `locale -a`.

Note that you can include more than one locale in /etc/locale.gen.

````sh
# in shroot /mnt/gentoo
eselect locale list
eselect locale set {n}
````

Reload the environment: `env-update && source /etc/profile && export PS1="(chroot) ${PS1}"`

Wk 30 Thu 16:32

Reached https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel

````sh
# in shroot /mnt/gentoo
emerge --ask sys-kernel/linux-firmware

# out (relevant)
These are the packages that would be merged, in order:

Calculating dependencies... done!
Dependency resolution took 1.45 s (backtrack: 0/20).

!!! All ebuilds that could satisfy "sys-kernel/linux-firmware" have been masked.
!!! One of the following masked packages is required to complete your request:
- sys-kernel/linux-firmware-99999999::gentoo (masked by: || ( ) linux-fw-redistributable license(s), missing keyword)
A copy of the 'linux-fw-redistributable' license is located at '/var/db/repos/gentoo/licenses/linux-fw-redistributable'.

- sys-kernel/linux-firmware-20260622::gentoo (masked by: || ( ) linux-fw-redistributable license(s), ~amd64 keyword)
- sys-kernel/linux-firmware-20260519::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20260410::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20260309::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20260221::gentoo (masked by: || ( ) linux-fw-redistributable license(s))

For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.
````

Our first licensing-based obstacle! Seems like a lot of these firmware images are non-foss (but distributable).

From https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Licenses,

````sh
# in /mnt/gentoo/etc/portage/package.license

# Accepting the license for linux-firmware
sys-kernel/linux-firmware linux-fw-redistributable

# Not sure if we need this, prefer to build still:
# Accepting any license that permits redistribution
#sys-kernel/linux-firmware @BINARY-REDISTRIBUTABLE
````

This adds an exception for linux-firmware.

Now we get something. Let's approve this:

````sh
# in chroot /mnt/gentoo
emerge --ask sys-kernel/linux-firmware

# out (relevant)
These are the packages that would be merged, in order:

Calculating dependencies... done!
Dependency resolution took 2.27 s (backtrack: 0/20).

[ebuild  N     ] app-arch/cpio-2.15  USE="nls"
[ebuild  N     ] app-alternatives/cpio-0  USE="gnu -libarchive (-split-usr)"
[ebuild  N     ] sys-kernel/linux-firmware-20260519  USE="initramfs redistributable -bindist -compress-xz -compress-zstd -deduplicate -dist-kernel -savedconfig (-unknown-license)"

Would you like to merge these packages? [Yes/No]
````

````sh
# in chroot /mnt/gentoo
emerge --ask sys-firmware/sof-firmware

# out (relevant)
These are the packages that would be merged, in order:

Calculating dependencies... done!
Dependency resolution took 2.16 s (backtrack: 0/20).

[ebuild  N     ] sys-firmware/sof-firmware-2025.12.2  USE="-tools"

Would you like to merge these packages? [Yes/No]
````

Approve

* https://packages.gentoo.org/packages/sys-firmware/intel-microcode
* -> https://gitweb.gentoo.org/repo/gentoo.git/tree/sys-firmware/intel-microcode/intel-microcode-20260512_p20260513.ebuild

````sh
# in chroot /mnt/gentoo
emerge --ask sys-firmware/intel-microcode

# out (relevant)
!!! All ebuilds that could satisfy "sys-firmware/intel-microcode" have been masked.
!!! One of the following masked packages is required to complete your request:
- sys-firmware/intel-microcode-20260512_p20260513::gentoo (masked by: intel-ucode license(s))
A copy of the 'intel-ucode' license is located at '/var/db/repos/gentoo/licenses/intel-ucode'.

- sys-firmware/intel-microcode-20260227_p20260227::gentoo (masked by: intel-ucode license(s))
- sys-firmware/intel-microcode-20260210_p20260211::gentoo (masked by: intel-ucode license(s))

For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.
````

Another license obstacle. This time to update CPU firmware. We should add an excemption for sys-firmware/intel-microcode:

````sh
# in /mnt/gentoo/etc/portage/package.license

# Accepting the license for sys-firmware/intel-microcode
sys-firmware/intel-microcode intel-ucode
````

Note it needs `intel-ucode` exemption, and not `linux-fw-redistributable`.

````sh
# in chroot /mnt/gentoo
emerge --ask sys-firmware/intel-microcode

# out (relevant)
These are the packages that would be merged, in order:

Calculating dependencies... done!
Dependency resolution took 2.18 s (backtrack: 0/20).

[ebuild  N     ] sys-apps/iucode_tool-2.3.1-r2
[ebuild  N     ] sys-firmware/intel-microcode-20260512_p20260513  USE="initramfs split-ucode -dist-kernel -hostonly -vanilla"

Would you like to merge these packages? [Yes/No]
````

Yup.

For now we will use GRUB for bootloader and it's time to install `sys-kernel/installkernel`:

(issue)

````sh
# in chroot /mnt/gentoo
emerge --ask sys-kernel/installkernel

# out (relevant)
Dependency resolution took 2.20 s (backtrack: 0/20).

[ebuild  N     ] sys-kernel/installkernel-68-r1  USE="-dracut -efistub -grub -refind -systemd -systemd-boot -ugrd -uki -ukify"

Would you like to merge these packages? [Yes/No]
````

Yup.

I'll try to do this again but with explicit grub use flag. Observe it built with `-grub`.

````sh
# in /mnt/gentoo/etc/portage/package.use/installkernel
sys-kernel/installkernel grub
````

Luck

````sh
# in chroot /mnt/gentoo
emerge --ask sys-kernel/installkernel

Dependency resolution took 6.52 s (backtrack: 0/20).

[ebuild  N     ] app-text/mandoc-1.14.6-r1  USE="-cgi (-selinux) -system-man -test"
[ebuild  N     ] sys-libs/efivar-39-r1  USE="-test"
[ebuild  N     ] sys-boot/grub-themes-gentoo-1.0-r2
[ebuild  N     ] sys-fs/lvm2-2.03.39  USE="readline udev -lvm -nvme -sanlock (-selinux) -static -static-libs -systemd -test -thin -valgrind -vdo -xfs"
[ebuild  N     ] sys-apps/pciutils-3.15.0  USE="kmod udev zlib -dns -static-libs -verify-sig" ABI_X86="(64) -32 (-x32)"
[ebuild  N     ] sys-boot/efibootmgr-18-r2
[ebuild  N     ] sys-boot/grub-2.14-r5  USE="branding device-mapper fonts nls sdl themes truetype -doc -efiemu -libzfs -mount -protect -secureboot (-test) -verify-sig" GRUB_PLATFORMS="efi-64 pc -coreboot -efi-32 -emu -ieee1275 (-loongson) -multiboot -qemu (-qemu-mips) -uboot -xen -xen-32 -xen-pvh"
[ebuild   R    ] sys-kernel/installkernel-68-r1  USE="grub*"

Would you like to merge these packages? [Yes/No]
````

Yup. That's much more now that we have a grub dependency.

Luckily just running emerge again with new USE flags is easy!

Adding also the USE flag of dracut to installkernel so that it initializes initramfs

(/issue)

````sh
# in /mnt/gentoo/etc/portage/package.use/installkernel
sys-kernel/installkernel grub dracut

# in chroot /mnt/gentoo
emerge --ask sys-kernel/installkernel
````

We're currently going for automation with https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Distribution_kernels, so that updating the kernel is relatively easier than manual maintenance.

--/ Wk 30 Thu 18:18

And so we are adding this USE flag so we can update the kernel periodically:

````sh
# in /mnt/gentoo/etc/portage/make.conf

USE="${USE} dist-kernel"
````

--/

Wk 30 Thu 17:57

Setting the `modules-sign` USE flag so that we can sign the kernel we'll build:

````sh
# in /mnt/gentoo/etc/portage/make.conf
USE="modules-sign"
````

Let's generate a key for use with this

````sh
# in chroot /mnt/gentoo
openssl req -new -noenc -utf8 -sha256 -x509 -outform PEM -out kernel_key.pem -keyout kernel_key.pem
````

Put that file somewhere only accessible to root. We will refer to it as `/path/to/kernel_key.pem`.

Now we can add

````sh
# in /mnt/gentoo/etc/portage/make.conf
MODULES_SIGN_KEY="/path/to/kernel_key.pem"
MODULES_SIGN_CERT="/path/to/kernel_key.pem" # Only required if the MODULES_SIGN_KEY does not also contain the certificate.
MODULES_SIGN_HASH="sha512" # Defaults to sha512.
````

The file also is rw for me. Make sure to chown and chmod it accordingly:

````sh
# in chroot /mnt/gentoo
chown root:root /path/to/kernel_key.pem
chmod 400 /path/to/kernel_key.pem
````

Wk 30 Thu 18:20

Time to build the kernel from source!

`sys-kernel/gentoo-kernel` should be run after a correctly configured `sys-kernel/installkernel`

````sh
# in chroot /mnt/gentoo
emerge --ask sys-kernel/gentoo-kernel
````

Big build! The next step when it's done should be https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System

Wk 30 Thu 19:15

````sh
# in chroot /mnt/gentoo
ls -al usr/src/linux

# out
lrwxrwxrwx 1 root root 25 Jul 23 22:02 usr/src/linux -> linux-6.18.39-gentoo-dist
````

Reached https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System

We can use `blkid` to view filesystem UUIDs as well as corresponding partition UUIDs.

Using partition UUIDs in /etc/fstab can be a reliable identifier, which are not erased when changing filesystems.

We need to create /etc/fstab. This is used to configure the mount points of the system.

The handbook explains the columns to insert:

````
    1. The first field shows the block special device or remote filesystem to be mounted. Several kinds of device identifiers are available for block special device nodes, including paths to device files, filesystem labels and UUIDs, and
       partition labels and UUIDs.
    2. The second field shows the mount point at which the partition should be mounted.
    3. The third field shows the type of filesystem used by the partition.
    4. The fourth field shows the mount options used by mount when it wants to mount the partition. As every filesystem has its own mount options, so system admins are encouraged to read the mount man page (man mount) for a full listing.
       Multiple mount options are comma-separated.
    5. The fifth field is used by dump to determine if the partition needs to be dumped or not. This can generally be left as 0 (zero).
    6. The sixth field is used by fsck to determine the order in which filesystems should be checked if the system wasn't shut down properly. The root filesystem should have 1 while the rest should have 2 (or 0 if a filesystem check is
       not necessary).

    [...]

    To improve performance, most users would want to add the noatime mount option, which results in a faster system since access times are not registered (those are not needed generally anyway). This is also recommended for systems with
   solid state drives (SSDs). Users may wish to consider lazytime instead.

    [...]

 /dev/sda1   /boot        xfs    defaults    0 2
 /dev/sda2   none         swap    sw                   0 0
 /dev/sda3   /            xfs    defaults,noatime              0 1

````

We should specify for all partitions previously created:

* /dev/{blkesp} goes to /efi. It uses the filesystem vfat.
* /dev/{blkswap} goes to "none" -- it isn't mounted anywhere.
* /dev/{blkroot} goes to /, it is xfs
* /dev/{blkextra} goes to /mnt/extra, it is xfs.

We are going to refer to UUID variables like {partuuid blkesp}. Find this out using `blkid` as necessary.

````sh
# in chroot /mnt/gentoo
mkdir /mnt/extra

# in /mnt/gentoo/etc/fstab
PARTUUID={partuuid blkesp}      /efi            vfat    defaults             0   2
PARTUUID={partuuid blkswap}     none            swap    sw                   0   0
PARTUUID={partuuid blkroot}     /               xfs     defaults,noatime     0   1
PARTUUID={partuuid blkextra}    /mnt/extra      xfs     defaults,noatime     0   2
````

(errata)

--/ Wk 30 Sat 01:09

WARN: The following is wrong.

I forgot to add the typesystem column field here. Also prefer to make PARTUUID= explicit.

````sh
# in chroot /mnt/gentoo
mkdir /mnt/extra

# in /mnt/gentoo/etc/fstab
{partuuid blkesp}      /efi            defaults             0   2
{partuuid blkswap}     none            sw                   0   0
{partuuid blkroot}     /               defaults,noatime     0   1
{partuuid blkextra}    /mnt/extra      defaults,noatime     0   2
````

Correction:

````sh
# in chroot /mnt/gentoo
mkdir /mnt/extra

# in /mnt/gentoo/etc/fstab
PARTUUID={partuuid blkesp}      /efi            vfat    defaults             0   2
PARTUUID={partuuid blkswap}     none            swap    sw                   0   0
PARTUUID={partuuid blkroot}     /               xfs     defaults,noatime     0   1
PARTUUID={partuuid blkextra}    /mnt/extra      xfs     defaults,noatime     0   2
````

(/errata)

Name your machine. Rename {machinename} as needed.

````sh
# in chroot /mnt/gentoo
echo {machinename} > /etc/hostname
````

--/ Wk 30 Sat 10:38
/etc/hosts already contained configuration for 127.0.0.1 and ::1 for me, so we do not have to configure it.

Otherwise, this is what I've done before for the record:

````sh
# in /mnt/gentoo/etc/hosts
127.0.0.1     {machinename}.homenetwork {machinename} localhost
::1           {machinename}.homenetwork {machinename} localhost
````

--/

Wk 30 Fri 08:37

Set a password for root:

````sh
# in chroot /mnt/gentoo
passwd
````

Review OpenRC Serivces: /etc/rc.conf

There are other services that can be configured in the handbook here in the section "System Information".

Since we're on a stage 3 desktop profile,

````sh
# in chroot /mnt/gentoo
dbus-uuidgen --ensure=/etc/machine-id
````

It went from empty to having some 32-character UUID.

Wk 30 Fri 08:58

Reached https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools

 > 
 > if needed, everything that happens on the system can be logged in a log file.

It could be good to log system changes. New packages installed, packages removed. Maybe even be able to do something like a `git commit` but for the world state of the PC,
which would allow me to revert back to prior snapshots, branch, export patches, import patches, diff, and so on.

But that's something to look into later. This is more about handling system logs, like logging services of modules.

In https://packages.gentoo.org/packages/app-admin/metalog it mentions also we need to chose only one system logger here.

* https://forums.gentoo.org/viewtopic.php?t=818267
  * Post about logger choices people made
  * One Mentions that metalog is able to do rotation logs out of the box so you don't run into system crashes due to logs filling up your system, while others do it by also emerging logrotate in addition

https://github.com/hvisage/metalog Seems fairly simple with its configuration syntax, and it supports automatic log file rotation. Let's use it. It requires zlib for compression with rotated logs.

````sh
# in /mnt/gentoo/etc/portage/package.use/installkernel
sys-kernel/installkernel grub
````

--/ Wk 30 Fri 09:38
ques\[t:topic-gentoo t:status=pend\] Why am I not able to remove (unicode) use from app-admin/metalog?

 > 
 > \[ebuild  N     \] app-admin/metalog-20260221  USE="(unicode) -zlib"

--/ Wk 30 Sun 13:20 +03:00

I suspected it was that it was required. this ebuild example seems to further corroborate this:

https://gpo.zugaina.org/AJAX/Ebuild/56369776/View

````sh
IUSE="appindicator wayland +X"
REQUIRED_USE="|| ( wayland X )"
````

--/

````sh
# in /mnt/gentoo/etc/portage/package.use/app-admin/metalog
app-admin/metalog zlib

# in chroot /mnt/gentoo
emerge --ask app-admin/metalog
````

Refer to https://packages.gentoo.org/packages/app-admin/metalog for configuration. Check /etc/conf.d/metalog.

Add the service to run on system boot via OpenRC in runlevel default:

````sh
# in chroot /mnt/gentoo
rc-update add metalog default

# out
* service metalog added to runlevel default
````

Adding a cron daemon, will stick with the default recommended by the handbook for now:

````sh
emerge --ask sys-process/cronie
rc-update add cronie default
````

Add a cron job to check for new hardware periodically (every 6 hours):

````sh
# in /mnt/gentoo/etc/crontab
0 */6 * * *     /usr/bin/modprobed-db store &> /dev/null
````

--/ Wk 30 Fri 12:50

todo\[t:status=todo t:rem\] Complete cron job setup later

 > 
 > At a later date of at least a week, please visit the kernel build section of the modprobed-db article to complete the setup.

https://wiki.gentoo.org/wiki/Modprobed-db#Building_kernels

--/

File system indexing:

````sh
# in chroot /mnt/gentoo
emerge --ask sys-apps/mlocate

# out (relevant)
 * Messages for package sys-apps/mlocate-0.26-r3:

 * The database for the locate command is generated daily by a cron job,
 * if you install for the first time you can run the updatedb command manually now.
 * Note that the /etc/updatedb.conf file is generic,
 * please customize it to your system requirements.
````

Add a new user to be able to ssh into this system:

https://wiki.gentoo.org/wiki/FAQ#How_do_I_add_a_normal_user.3F

````sh
# in chroot /mnt/gentoo
useradd -m -G users,audio,wheel {youruser}
passwd {youruser}
````

Now you can switch to user user with `su {youruser}`.

Configure sshd service to run on boot:

````sh
# in chroot /mnt/gentoo
rc-update add sshd default
````

Getting some nice completions:

````sh
# in chroot /mnt/gentoo
emerge --ask app-shells/bash-completion
````

A daemon for periodic time synchronization setup with OpenRC,

````sh
# in chroot /mnt/gentoo
emerge --ask net-misc/chrony
rc-update add chronyd default
````

Tools for the filesystems we're using and rules for nvme devices since I use them:

Currently on the live image environment we have for example `mkfs.xfs` but not on the chrooted stage 3.

````sh
# xfs
emerge --ask sys-fs/xfsprogs

# vfat
emerge --ask sys-fs/dosfstools

emerge --ask sys-block/io-scheduler-udev-rules
````

After emerging `sys-fs/xfsprogs` now we have `mkfs.xfs` for `chroot /mnt/gentoo`.

Wk 30 Fri 16:30

Now in https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader

````
quote https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader

If GRUB was somehow emerged without enabling GRUB_PLATFORMS="efi-64", the line (as shown above) can be added to make.conf and then dependencies for the world package set can be re-calculated by passing the --update --newuse options to
   emerge:

   root #emerge --ask --update --newuse --verbose sys-boot/grub
````

Hmm. I did run emerge before twice, just changing USE flags without --update --newuse. My expectation though was to install it again anew, rather than reconfigure. This was before for sys-kernel/installkernel
which I installed multiple times.

Ok we're going with grub for the bootloader. We are on a UEFI system:

````sh
# in /mnt/gentoo/etc/portage/make.conf
GRUB_PLATFORMS="efi-64"

# in chroot /mnt/gentoo
emerge --ask sys-boot/grub
````

Make sure that `/mnt/gentoo/efi` is mounted (use lsblk in chroot, it shows mount points per device blocks), then:

````sh
# in chroot /mnt/gentoo
grub-install --efi-directory=/efi

# out
Installing for x86_64-efi platform.
Installation finished. No error reported.
````

There's grub config at /etc/default/grub and /etc/grub.d used to generate /boot/grub/grub.cfg

Default config should suffice for us. Review then configure:

````sh
# in chroot /mnt/gentoo
grub-mkconfig -o /boot/grub/grub.cfg

# out
Generating grub configuration file ...
Found theme: /boot/grub/themes/gentoo_glass/theme.txt
Found linux image: /boot/vmlinuz-6.18.39-gentoo-dist
Found initrd image: /boot/intel-uc.img /boot/amd-uc.img /boot/initramfs-6.18.39-gentoo-dist.img
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done
````

Wk 30 Fri 17:19

Time for a reboot! First unmount all partitions in and outside chroot, exit chroot, then `reboot`! You can unmount with `umount /dev/{blkdev}` and check with `lsblk`.

OK So we are able to reboot! I'm now on the gentoo installation system itself!

Some issues:

(1)

init script errors

Transcribing from image:

````
* ERROR: bootmisc failed to start

* ERROR: sshd failed to start

* Create Volatile Files and Directories ...
Failed to create directory or subvolume "/srv": Read-only file system fchownat() of /etc/polkit-1/rules.d failed: Read-only file system
Failed to create directory or subvolume "/tmp/portage": Read-only file sy [ !! ]

* ERROR: systemd-tmpfiles-setup failed to start

* failed to start user.lan
* ERROR: user.lan failed to start
````

(2)

extra partitions are not mounted. I had /dev/{blkextra} and /dev/{blkesp} specified with mount points in /etc/fstab. But only /dev/{blkroot} is mounted.

For now, let's install some necessary software. We need to return to the handbook and be able to browse the internet still. Let's use elinks instead of links:

````sh
emerge --ask www-client/elinks
````

We can't even install new packages now. We get some

````
portage.exception.ReadOnlyFileSystem: [Errno 30] Read-only file system: '/var/db/.pkg.portage_lockfile'
````

So let's go back to the live image enviornment and look into this.

````
[ebuild  N     ] www-client/elinks-0.19.1  USE="X bzip2 doc gpm mouse nls ssl unicode xml zlib -bittorrent -brotli -curl -debug -finger -ftp -gemini -gnutls -gopher -guile -idn -javascript -libcss -lua -lzma -nntp -perl -python -samba -sftp -test -tre -zstd" GUILE_SINGLE_TARGET="3-0 -2-2" LUA_SINGLE_TARGET="lua5-1 -lua5-3 -lua5-4 -luajit" PYTHON_SINGLE_TARGET="python3_14 -python3_12 -python3_13"
````

````sh
# in /mnt/gentoo/etc/portage/package.use/www-client/elinks
www-client/elinks libcss

# in chroot /mnt/gentoo
emerge --ask www-client/elinks
````

--/ Wk 30 Sat 01:18

elinks can be pretty bright. Something to look into. If it gets too much, we need to have links to fall back to:

````sh
emerge --ask www-client/links
````

--/

````sh
# in /mnt/gentoo/etc/portage/package.use/app-misc/tmux
app-misc/tmux vim-syntax

# in chroot /mnt/gentoo
emerge --ask app-misc/tmux
````

````sh
# in chroot /mnt/gentoo
emrege --ask app-editors/vim
````

Wk 30 Fri 21:10

elinks uses C-n and C-p instead of links' p and l for scrolling. links also supports C-n and C-p.

--/ Wk 30 Sun 12:56 +03:00 | Also note that p and l can get stuck in input fields and then spam lllllll and stuff. No such problem with C-n and C-p on links.
--/

In the boot logs dracut attempts to mount /dev/{blkroot} with -o 0,r0. Then we get an xfs error via fsconfig: Unknown parameter '0'.

So I was going off of the handbook example, I should add clear PARTUUID=\* in my /etc/fstab

````
PARTUUID=4f68bce3-e8cd-4db1-96e7-fbcaf984b709   /           xfs     defaults,noatime             0 1
````

And I should also figure out what that `defaults` mean.

````sh
man fstab

# out (relevant) {
defaults
   use default options. The default depends on the kernel and the filesystem. mount(8) does not have any hardcoded set of default options. The kernel default is usually rw, suid, dev, exec, auto, nouser, and async.
# }

man mount

# out (relevant) {
defaults
   Use the default options: rw, suid, dev, exec, auto, nouser, and async.

   Note that the real set of all default mount options depends on the kernel and filesystem type. See the beginning of this section for more details.
# }
````

Yikes. I skipped the filesystem type column in /etc/fstab! Correcting

ok, the read-only filesystem issue was fixed once I fixed the missing filesystem column problem with /etc/fstab. `dracut` remounting with readonly still occurs and is normal:

It is likely a read-only bind mount. Read about that in https://www.man7.org/linux/man-pages/man8/mount.8.html.

````
The alternative (classic) way to create a read-only bind mount is
        to use the remount operation, for example:
````

We're able to do `emerge sync` and compile stuff during a normal boot now!

Wk 30 Sat 09:26

`lsblk` now shows the other partitions specified automatically mounted too.

Wk 30 Sat 10:54

Note that entries in /etc/fstab get automatically mounted on boot and automatically unmounted on reboot, so we don't have to worry about manually doing of that.

Now unless specified otherwise, all commands will be executed from user lan on boot, or from root through su.

### More config for tmux

Wk 30 Sat 09:15

Now that we have booted into the system, we can afford some more permenant tmux config. I will take a few from my backups that can be currently applicable.

Tmux allows plugins which we can also use here.

Wk 30 Sat 10:21 +03:00

My current `LanHikari22/tmux-sessionist` plug is out of date from upstream. We need to update this.

If you need a new ssh identity, generate with `ssh-keygen` and configure code forges with it.

````sh
mkdir -p ~/src/cloned/gh/LanHikari22
cd ~/src/cloned/gh/LanHikari22
git clone git@github.com:LanHikari22/tmux-sessionist
````

Configure upstream

````sh
# in /home/lan/src/cloned/gh/LanHikari22/tmux-sessionist/.git/config
[remote "upstream"]
        url = git@github.com:tmux-plugins/tmux-sessionist
        fetch = +refs/heads/*:refs/remotes/origin/*
````

Update:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/tmux-sessionist/
git config pull.rebase false
git pull upstream master
````

Hmm a conflict.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/tmux-sessionist/scripts/goto_session.sh
#!/usr/bin/env bash

CURRENT_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

main() {
<<<<<<< HEAD
        tmux run-shell -b "$CURRENT_DIR/list_sessions.sh | $CURRENT_DIR/fzf-tmux-pane.sh | xargs tmux switch-client -t || true"
=======
        # displays tmux session list
        tmux run-shell -b "$CURRENT_DIR/list_sessions.sh"
        # goto command prompt
        tmux command -p session: "run '$CURRENT_DIR/switch_or_loop.sh \"%1\"'"
>>>>>>> a315c423328d9bdf5cf796435ce7075fa5e1bffb
}
main
````

Let's keep the HEAD version. That was the point of my fork to integrate fzf tmux pane switching.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/tmux-sessionist/
git commit

# out
[master 0a8e89d] merge with upstream but keep my fzf switching
 Date: Sat Jul 25 11:16:04 2026 +0300
````

Also this is a fork, move from `/home/lan/src/cloned/gh/LanHikari22/` to `/home/lan/src/forked/LanHikari22/tmux-plugs/tmux-sessionist/`.

Currently it still blocks prefix-T from showing the time. And it remaps prefix-C from a config menu to a prompt to enter a new session.

We likely require installation of `fzf` for it to work.

https://packages.gentoo.org/packages/app-shells/fzf

````sh
su
emerge --ask app-shells/fzf
````

Fetch logs for current emerge are in /var/log/emerge-fetch.log.

Now with `fzf` in our system, prefix-g works for fuzzy selecting a session to switch to.

It still is the case that we no longer have prefix-t, now it just does nothing. Why?

Wk 30 Sat 10:05 +03:00

This configuration we had before has bad UX. It undoes the pane maximizing if the user presses arrows immediately after:

````
# for switching panes in zoomed state. Use prefix-e and prefix-E.
bind -r e select-pane -t .+1 \; resize-pane -Z
bind -r E select-pane -t .-1 \; resize-pane -Z
````

Wk 30 Sat 10:04 +03:00

Changing `~/.tmux.conf` to:

````sh
# to install tpm, run
# git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
# You can run tmux source ~/.tmux.conf to load settings first time in-session.
# to install plugins: prefix - I. Update them with prefix - U then write all.

# -- Declare plugins to use --

# For installing other plugins
set -g @plugin 'tmux-plugins/tpm'

# Allows prefix-c for creating new session, and if fzf is instanlled, prefix-g for fuzzy switch to session.
set -g @plugin 'LanHikari22/tmux-sessionist'

# Allows us to do prefix-| for horizontal split and prefix-- for vertical split.
set -g @plugin 'tmux-plugins/tmux-pain-control'

# Fzf menu for tmux
set -g @plugin 'sainnhe/tmux-fzf'

# Changes some settings like letting us switch between panes with hjkl
set -g @plugin 'tmux-plugins/tmux-sensible'

# -- Configure plugins --

# Configs for sainhe/tmux-fzf
set -g @tmux-fzf-launch-key 'C-f'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf or plug config)
run '~/.tmux/plugins/tpm/tpm'

# -- Non-plugin Configurations --

# Set prefix to C-a
set-option -g prefix C-a
unbind-key C-b
bind-key C-a send-prefix

# Uses vim keybindings for copy mode and others
setw -g mode-keys vi

# to allow tmux-yank to copy to system clipboard
set -g set-clipboard on

# for switching panes in zoomed state. Use prefix-e and prefix-E.
bind -r e select-pane -t .+1 \; resize-pane -Z
bind -r E select-pane -t .-1 \; resize-pane -Z
````

Then configure it:

````sh
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
# Run C-a I
````

### Setting up Gentoo After Boot

Wk 30 Sat 21:15 +03:00

Now in https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Finalizing

There's mostly advice here for later maintenance. Compiled files are stored as tarballs in /var/cache/distfiles (and bin packages in /var/cache/binhost/gentoo)

If this becomes an issue, there are utilities to clear these caches. Let's isntall them.

````sh
su
emerge --ask app-portage/gentoolkit
````

Now we can clean cached source code tarballs with `eclean-dist` and binary packages with `eclean-pkg`.

Ok so we still are in the system console here. We need to get some desktop environment going. I want to try Wayland/sway.

I remember there also was a way to launch a single application graphically that could be good for testing.

It was a wayland kioske of the name of cage I have used before. Anyway let's look for instructions of setting up Wayland/sway on Gentoo.

https://wiki.gentoo.org/wiki/Sway

````sh
# in /etc/portage/package.use/gui-wm/sway {
    gui-wm/sway -X wallpapers
# }

# in /etc/portage/package.use/gui-apps/swaybg {
    >=gui-apps/swaybg-1.2.2 gdk-pixbuf
# }

# in /etc/portage/make.conf {
    USE="${USE} -X"
# }

su
emerge --ask gui-wm/sway
````

Ended up having to include `X`, and it wanted the dk-pixbuf USE flag specified too. Complaining with a message `The following USE changes are necessary to proceed`

Added `-X` as a global USE flag. We should try to avoid this until we run into trouble. I want to run just wayland if I can.

````
man emerge

# out (relevant)
CONFIGURATION FILES UPDATE TOOLS
       Tools such as dispatch-conf, cfg-update, and etc-update are also available to aid in the merging of these files. They provide interactive merging and can auto-merge trivial changes.
````

`dispatch-conf` lead me to find out it added some config in

````
/etc/portage/package.use/www-client/._cfg0002_elinks
````

to be merged with that tool. Removing that file makes it no longer be considered for processing by `dispatch-conf`. `dispatch-conf` also tells us when we have some issue in our own package.use config files which is nice.

We need a terminal emulator to use once we run sway:

````sh
su
emerge --ask gui-apps/foot
````

Right now both sway and foot will fail to start due to invalid `XDG_RUNTIME_DIR`:

````sh
foot

# out
error: XDG_RUNTIME_DIR is invalid or not set in the environment.
 err: wayland.c:1712: failed to connect to wayland; no compositor running?
````

Wk 30 Sun 07:50 +03:00

* http://jorgicio.github.io/setup-purest-openrc-in-gentoo-(and-other-unix-like-flavours).html
  * Mentions that /etc/inittab is not used anymore? But it was recommended for configuration in the gentoo wiki, so is this right?
    * They mention: `The file /etc/inittab is ignored as being part of SysVinit. Instead, the TTYs must be enabled manually using the agetty daemon.`
  * Mentions that binaries like `halt` and `reboot` won't be available as they are SysVinit-based. Though I have access to reboot.
    I do have access to openrc-shutdown.
  * Recommends to use ConsoleKit

Let's look to insalling and running wayland first:

https://wiki.gentoo.org/wiki/Wayland

Ok so it's a wayland compositor that we want, wheras wayland is more like the underlying framework and specifications.

Looking at startup docs for Sway at https://wiki.gentoo.org/wiki/Sway

We're expected to set XDG_RUNTIME_DIR:

````sh
# from https://wiki.gentoo.org/wiki/Sway
 #!/bin/sh
 if test -z "${XDG_RUNTIME_DIR}"; then
   export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir
     if ! test -d "${XDG_RUNTIME_DIR}"; then
         mkdir "${XDG_RUNTIME_DIR}"
         chmod 0700 "${XDG_RUNTIME_DIR}"
     fi
 fi
````

For now we set it temporarily:

````sh
export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir && mkdir -p "${XDG_RUNTIME_DIR}" && chmod 0700 "${XDG_RUNTIME_DIR}"
````

Now whether we run sway on its own or with `dbus-run-session` we get an error:

````sh
su
dbus-run-session sway

# out (error)
00:00:00.003 [wlr] [libseat] [libseat/backend/logind.c:640] Could not get primary session for user: No data available
00:00:00.003 [wlr] [libseat] [libseat/libseat.c:79] No backend was able to open a seat
00:00:00.003 [wlr] [backend/session/session.c:83] Unable to create seat: Function not implemented
00:00:00.003 [wlr] [backend/session/session.c:256] Failed to load session backend
00:00:00.003 [wlr] [backend/backend.c:79] Failed to start a session
00:00:00.003 [wlr] [backend/backend.c:399] Failed to start a DRM session
00:00:00.003 [sway/server.c:247] Unable to create backend
````

That was on root, but issue persists outside as well.

Section `4.4 No backend was able to open a seat` of https://wiki.gentoo.org/wiki/Sway indicates we missed some steps, like installing sys-auth/seatd or sys-auth/elogind.

In section `2.18.1 OpenRC`, they recommend to add elogind to runlevel boot: `rc-update add elogind boot`

Let's copy sway configuration for our user:

````sh
mkdir -p ~/.config/sway/
cp /etc/sway/config ~/.config/sway/
````

````sh
swaymsg -t get_outputs

# out (error)
00:00:00.017 [swaymsg/main.c:509] Unable to retrieve socket path
````

https://wiki.gentoo.org/wiki/Configuring_a_system_without_elogind

This has more info on XDG_RUNTIME_DIR, also suggesting to either do it in /run/user/${UID} due to some apps assuming this, 
or also export XDG_RUNTIME_DIR=$(mktemp -d "${UID}-runtime-dir.XXX") which would be more random. It might be suitable to guard against other users reusing our XDG_RUNTIME_DIR.

It also mentions that seatd needs to be installed if one does not want to use elogind which came from systemd tools:

````sh
su
emerge --ask sys-auth/seatd
````

Hmm. I tried to `rc-update` add sead to default but it says no service found. We installed `sys-auth/seatd-0.9.3-r1`.

https://wiki.gentoo.org/wiki/Seatd

We need to check our groups.

https://forums.gentoo.org/viewtopic-t-59510-start-0.html -> We can `cat /etc/group | grep {youruser}`.

As we specified when we added groups, it's only currently wheel, audio, users.

For seatd, we additionally need to be part of the video and seat groups:

````
su
gpasswd -a {youruser} video
````

They mention a seat group also but we do not currently have any seat group. Nor can we add seatd to default run level via `rc-update add _ default` or start it immediately via `rc-service _ start`.

Let's try to add in the `builtin` and `server` use flags for it (even though I don't see them mentioned during `emerge --ask _ )`:

````sh
# in /etc/portage/package.use/sys-auth/seatd {
sys-auth/seatd builtin server
# }
````

Yup it does make a difference when we rerun

````sh
su
emerge --ask sys-auth/seatd
````

Now we have a seat group:

````sh
su
gpasswd -a {youruser} seat
````

Add the service to default run level and start it:

````sh
su
rc-update add seatd default
rc-service seatd start
````

We still get some errors

````sh
su # don't use; run it under the user. Retained here for audit.
export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir && mkdir -p "${XDG_RUNTIME_DIR}" && chmod 0700 "${XDG_RUNTIME_DIR}"
dbus-run-session sway

# out (error)
libEGL warning: egl: failed to create dri2 screen
00:00:00.056 [wlr] [EGL] command: eglInitialize, error: EGL_NOT_INITIALIZED (0x3001), message: "DRI2: failed to create screen"
libEGL warning: egl: failed to create dri2 screen
00:00:00.061 [wlr] [EGL] command: eglInitialize, error: EGL_NOT_INITIALIZED (0x3001), message: "DRI2: failed to create screen"
00:00:00.065 [wlr] [EGL] command: eglInitialize, error: EGL_NOT_INITIALIZED (0x3001), message: "DRI2: failed to load driver"
00:00:00.066 [wlr] [EGL] command: eglInitialize, error: EGL_NOT_INITIALIZED (0x3001), message: "eglInitialize"
00:00:00.066 [wlr] [render/egl.c:269] Failed to initialize EGL
00:00:00.066 [wlr] [render/egl.c:611] Failed to initialize EGL context
00:00:00.066 [wlr] [render/gles2/renderer.c:499] Could not initialize EGL
00:00:00.066 [wlr] [render/vulkan/vulkan.c:182] Could not create instance: ERROR_INCOMPATIBLE_DRIVER (-9)
00:00:00.066 [wlr] [render/vulkan/renderer.c:2509] creating vulkan instance for renderer failed
00:00:00.066 [wlr] [render/wlr_renderer.c:279] Could not initialize renderer
00:00:00.066 [sway/server.c:256] Failed to create renderer
````

* https://bugs.launchpad.net/ubuntu/+source/gnome-shell/+bug/2067374
  * They recommend `libnvidia-egl-wayland1` (ubuntu), but couldn't on a quick search find a gentoo package for this.
* https://bbs.archlinux.org/viewtopic.php?id=297596
  * Mentions `eglinfo`: https://github.com/dv1/eglinfo

Let's install dv1/eglinfo for diagnostics:

````sh
mkdir -p ~/src/cloned/gh/dv1
cd ~/src/cloned/gh/dv1
git clone git@github.com:dv1/eglinfo

````

It's unclear what our DEVICE envvar should be from the README.md. Let's leave it as generic. We can check the devices available via `ls src/platform_* `

````sh
# in /home/lan/src/cloned/dv1/eglinfo
mkdir bin
./waf configure --platform=fb --device=generic --prefix=./bin --sysroot=/

# out (error)
./waf
  File "/home/lan/src/cloned/dv1/eglinfo/./waf", line 166
    #BZh91AY&SY¶ñöÿÿÿ¼¶tÿÿÿÿÿÿÿÿÿÿÿ
SyntaxError: source code cannot contain null bytes
````

Hmm. Let's not pursue this for now. `rm -rf ~/src/cloned/gh/dv1/eglinfo`.

--/ Wk 30 Sun 10:49 +03:00 | Amend: put under ~/src/cloned/gh/dv1 now ~/src/cloned/dv1. Mistake.
--/

So far it seems this might be related to my nvidia drivers.

We don't yet have nvidia-smi or anything either here.

* https://wiki.gentoo.org/wiki/NVidia
* -> https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers

 > 
 > The open source driver only works on Turing GPUs and newer (i.e. GTX 1650 and newer). The kernel-open flag will need to be disabled on older cards.

https://www.techpowerup.com/gpu-specs/geforce-gtx-1650.c3366 -> "The GeForce GTX 1650 is a mid-range graphics card by NVIDIA, launched on April 23rd, 2019."

https://www.techpowerup.com/gpu-specs/titan-rtx.c3311 -> "The TITAN RTX was an enthusiast-class graphics card by NVIDIA, launched on December 18th, 2018."

So I guess we need to disable `kernel-open`.

Seems we missed to do this before. We only set `VIDEO_CARDS: -* nvidia`.

There's warnings when emerging `x11-drivers/nvidia-drivers` for hardware `470.xx` and `390.xx` is no longer supported. We should be in the 500s though.

We will also need to make an exemption for the license of `x11-drivers/nvidia-drivers`. Removed some redundant commends from `/etc/portage/package.license`.

We do have `dist-kernel` setup as a global use flag, as recommended.

````sh
# in /etc/portage/portage.license {
    x11-drivers/nvidia-drivers NVIDIA-2025
# }

# in /etc/portage/package.use/x11-drivers/nvidia-drivers {
    x11-drivers/nvidia-drivers -kernel-open
# }

su
emerge --ask x11-drivers/nvidia-drivers
````

This also installs `gui-libs/egl-wayland2-1.0.1::gentoo`, related to the issues we've been having with sway.

`nvidia-smi` won't detect the driver right away, but it does after a system reboot.

````sh
nvidia-smi

# out
Sun Jul 26 09:22:14 2026
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 595.84                 Driver Version: 595.84         CUDA Version: 13.2     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA TITAN RTX               Off |   00000000:01:00.0  On |                  N/A |
| 41%   40C    P8             18W /  280W |       9MiB /  24576MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
````

````sh
# su - don't use; run it under the user.
export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir && mkdir -p "${XDG_RUNTIME_DIR}" && chmod 0700 "${XDG_RUNTIME_DIR}"
dbus-run-session sway

# out
00:00:00.011 [sway/server.c:163] !!! Proprietary Nvidia drivers are in use !!!
00:00:00.011 [sway/server.c:165] Use Nouveau instead
00:00:00.011 [sway/server.c:175] Proprietary drivers are NOT supported. To launch sway anyway, launch with --unsupported-gpu and DO NOT report issues.
````

Ouch. We can look into this later to compare with nouveau, but for now `--unsupported-gpu`.

````sh
# su - don't use; run it under the user.
export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir && mkdir -p "${XDG_RUNTIME_DIR}" && chmod 0700 "${XDG_RUNTIME_DIR}"
dbus-run-session sway --unsupported-gpu 2>&1 | tee sway_logs.log
````

We also added a `hello` command that can be used for some logging tests:

````sh
su
echo 'echo "$(date) | helloooo"' > /usr/local/bin/hello
chmod +x /usr/local/bin/hello
````

Wk 30 Sun 09:39 +03:00

We're in sway now!

* https://www.reddit.com/r/linuxquestions/comments/1ivv74h/i_have_no_idea_how_to_use_swaywm/
* We're able to start a terminal emulator with Win+Enter. Likely foot, as running it in Win+Enter opens a similar tiled window.
  * Apparently Ctrl+Shift+N also works for me.
* As they recommend, sway being heavily inspired by i3 means we can find some use guidance with i3.

Remember we have configuration under `~/.config/sway/config`

https://i3wm.org/docs/userguide.html

Whoa! We can use win+l and win+h to switch back and forth between monitors! You can also use arrows

And win+F for full screen to remove the sway status stuff. Be aware you might create windows with Win+Enter and you won't see em due to full screen.

i3, dmenu is supposed to be opened with win+d (or $mod+d) which seems like it would be a similar workflow to what I do with apps like rofi where I do alt+Enter and type the app name fuzzily.

https://wiki.gentoo.org/wiki/Sway

* So we have `dev-libs/bemenu` and `gui-apps/wmenu` as alternatives over at sway. It defaults to `gui-apps/wmenu`.

````sh
su
emerge --ask gui-apps/wmenu
````

Now Win+d lets me basically start any application, even `ssh`, which I can see in my tmux session to print usage, or echo.

So I assume it just tries to launch any application available to me in $PATH. Let's test this. `/usr/bin` has many executables, but my $PATH also includes `/usr/local/bin` which currently has nothing.

If it fetches from `$PATH`, it should be able to recognize an app put there. Put this there:

````sh
su
echo 'echo Hiii' > /usr/local/bin/hello
chmod +x /usr/local/bin/hello
````

It works. Now we can find a `hello` application via Win+D. And in the tmux session running sway, it logs `Hiii`.

````sh
su
rm /usr/local/bin/hello
````

Win+Shift+q to close a window.

We can use Win+{num} in a selected monitor, and putting a different number like `[3]` (I have two monitors so they right away occupy `[1]` and `[2]`) opens a new workspace in that monitor.

Might be good to keep the odds in the left monitor and the events to the right. Also note that before launching sway, the default behavior was mirroring monitors. Now they can have different things!

For my workflow, I need a way to **switch** to a window by label.

I'm able to run `wmenu-run` from a newly created terminal emulator but not from within tmux that was launched before sway was run because it gives an error `Failed to connect to display`.
It also won't do it if we're not root, and the terminal emulators launch directly into su.

````sh
export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir && mkdir -p "${XDG_RUNTIME_DIR}" && chmod 0701 "${XDG_RUNTIME_DIR}"
dbus-run-session sway --unsupported-gpu
````

--/ Wk 30 Sun 22:47 +03:00
Putting the above in a quick script /home/lan/start_sway.sh with some amendments:

````sh
# in /home/lan/start_sway.sh
#!/bin/sh
export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir && mkdir -p "${XDG_RUNTIME_DIR}" && chmod 0701 "${XDG_RUNTIME_DIR}"
dbus-run-session sway --unsupported-gpu | tee sway_log.log
````

Start this outside tmux, and start tmux from the foot terminal emulator so that tmux gets access to sway context environmental variables like `$DBUS_SESSION_BUS_ADDRESS`.
--/

This confirms we can run it without su. Amending prior entries to recommend removing `su`.

https://github.com/AdrienLeGuillou/sway_window_swithcher_dmenu

````sh
export REPO=AdrienLeGuillou/sway_window_swithcher_dmenu && git clone git@github.com:$REPO /home/lan/src/cloned/gh/$REPO
````

This requires jq:

https://packages.gentoo.org/packages/app-misc/jq

````sh
su
emerge --ask app-misc/jq
````

This works: `./sws.sh --dmenu-cmd wmenu`. Let's set it up:

````sh
su
echo '/home/lan/src/cloned/gh/AdrienLeGuillou/sway_window_swithcher_dmenu/sws.sh --dmenu-cmd wmenu' > /usr/local/bin/sws
chmod +x /usr/local/bin/sws
````

Right now we're able to run this via Win+d sws. We need a shorter path to this. I used to use Alt+Enter, I want to use Win+s, but it's tacking by the stacking layout switching in `~/.config/sway/config`.

Update the stacking layout switcher to Win+shift+s so that we can reserve Win+s for switching:

````diff
# in ~/.config/sway/config
-bindsym $mod+s layout stacking
+bindsym $mod+Shift+s layout stacking
````

and add:

````sh
# in ~/.config/sway/config
# Custom config

set $switcher sws

bindsym $mod+s exec $switcher
````

Forgot to update the binsym for layout stacking and got a warning and had to exit sway after restarting it.

But now it works! Use Win+Shift+s for stacking, Win+w for tabbed, Win+e to switch between stacking horizonta/vertical!

Wk 30 Sun 11:38 +03:00

Okay now that we have sway, and my workflow of window switching, and an ability to open windows. It's time to get some graphical tools.

Let's prioritize obsidian so that we can go back to proper note-taking on this device.

I've been SSHing into my phone from my PC on tmux to take notes. We need to also sync all of these notes back to my new gentoo system now.

Let's get back our syncthing data, syncthing, and get obsidian. I would like have clusterline (my note taking system) on silverbullet and vim, but currently it's functional only on obsidian.

We also still have to make sway run automatically on boot, but we'll get to that later as we're currently installing things and testing.

````sh
export REPO=syncthing/syncthing && git clone git@github.com:$REPO /home/lan/src/cloned/gh/$REPO

# in ~/src/cloned/gh/syncthing/syncthing
./build.sh
````

Though we'll need a graphical browser for this first, since it requires js (though we could try enabling js with elinks)

````
# in /home/lan/src/cloned/cb/deltatraced/deltatraced
git commit

# out
[main 692c28d] save pre-gentoo install
````

For now we can sync manually. I put the mobile raw journal notes all in a `tmp.md` file which is going to be easy to copy over to my PC now.

### Installing Obsidian

Let's get obsidian.

https://github.com/obsidianmd/obsidian-releases/releases/latest

https://github.com/obsidianmd/obsidian-releases/archive/refs/tags/v1.12.7.tar.gz

To decompress a `*.tar.gz`, use `tar -xf`.

Yup don't use that. In the README.md in that compressed tarball:

````sh
Obsidian is not open source software and this repo _DOES NOT_ contain the source code of Obsidian. However, if you wish to contribute to Obsidian, you can easily do so with our extensive plugin system. A plugin guide can be found here: https://docs.obsidian.md
````

https://github.com/obsidianmd/obsidian-releases/releases/download/v1.12.7/obsidian-1.12.7.tar.gz

Can't run directly:

````
./obsidian: error while loading shared libraries: libnspr4.so: cannot open shared object file: No such file or directory
````

Easiest should be the appimage:

https://github.com/obsidianmd/obsidian-releases/releases/download/v1.12.7/Obsidian-1.12.7.AppImage

````sh
mkdir -p ~/data/releases/gh/obsidianmd/obsidian-releases

# in /home/lan/data/releases/gh/obsidianmd/obsidian-releases
wget https://github.com/obsidianmd/obsidian-releases/releases/download/v1.12.7/Obsidian-1.12.7.AppImage
chmod +x Obsidian-1.12.7.AppImage

# in /home/lan/data/releases/gh/obsidianmd/obsidian-releases
./Obsidian-1.12.7.AppImage

# out
dlopen(): error loading libfuse.so.2

AppImages require FUSE to run.
You might still be able to extract the contents of this AppImage
if you run it with the --appimage-extract option.
See https://github.com/AppImage/AppImageKit/wiki/FUSE
for more information
````

https://wiki.gentoo.org/wiki/Appimage

There are different slots for this. Verion 2 of fuse in slot 0 and version 3 in slot 3.

````sh
su
emerge --ask sys-fs/fuse:0
````

Same issue anyway: `/tmp/.mount_ObsidiRAAcr3/obsidian: error while loading shared libraries: libnspr4.so: cannot open shared object file: No such file or directory`.

Which makes sense, it's a dynamic dependency.

Let's go fix this with the prior compressed tarball.

````sh
# in /home/lan/data/releases/gh/obsidianmd/obsidian-releases
wget https://github.com/obsidianmd/obsidian-releases/releases/download/v1.12.7/obsidian-1.12.7.tar.gz
tar -xf obsidian-1.12.7.tar.gz

# in /home/lan/data/releases/gh/obsidianmd/obsidian-releases/obsidian-1.12.7
./obsidian

# out (error)
./obsidian: error while loading shared libraries: libnspr4.so: cannot open shared object file: No such file or directory
````

https://packages.gentoo.org/packages/dev-libs/nspr | Netscape Portable Runtime

````sh
su
emerge --ask dev-libs/nspr
````

Now we get

````
./obsidian: error while loading shared libraries: libnss3.so: cannot open shared object file: No such file or directory
````

There's some portage overlay that references obsidian, but it isn't in an official package. https://gpo.zugaina.org/app-text/obsidian

https://gpo.zugaina.org/AJAX/Ebuild/56369776/View

I probably should eventually make my own ebuilds and have a public repo of ebuilds instead of manually installing like this.

Worrying though that it says X is required. I have globally declared "-X". I'm on wayland, I should not require both Wayland and X. But let's see if this applies to us.

https://packages.gentoo.org/packages/dev-libs/nss | Mozilla's Network Security Services library that implements PKI support

This is starting to look like it might be heavy.

````sh
su
emerge --ask dev-libs/nss
````

````
[120297:0726/133223.802914:ERROR:dbus/bus.cc:408] Failed to connect to the bus: Failed to connect to socket /run/dbus/system_bus_socket: No such file or directory
````

Alright once in a terminal emulator directly obsidian does start now.
