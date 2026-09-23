---
context_type: task
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-task-b01daf](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-task-b01daf)

Overview: [001 Overview Install a new Gentoo system](../entry/001%20Overview%20Install%20a%20new%20Gentoo%20system.md)

# Journal

2026-07-27 Wk 31 Mon - 00:30 +03:00

[002 Enable my bluetooth speakers, bluetooth headset, and wired headset to play music on youtube](002%20Enable%20my%20bluetooth%20speakers,%20bluetooth%20headset,%20and%20wired%20headset%20to%20play%20music%20on%20youtube.md)

By now we at least have music in my wired headset. Let's set up steam.

https://wiki.gentoo.org/wiki/Steam

````sh
su
eselect repository enable steam-overlay
emaint sync -r steam-overlay
````

It seems we have to specify a long file for steam:

````sh
# in /etc/portage/package.use/steam
app-accessibility/at-spi2-core    abi_x86_32
app-arch/bzip2                    abi_x86_32
app-arch/lz4                      abi_x86_32
app-arch/xz-utils                 abi_x86_32
app-arch/zstd                     abi_x86_32
app-crypt/p11-kit                 abi_x86_32
dev-db/sqlite                     abi_x86_32
dev-lang/rust                     abi_x86_32
dev-lang/rust-bin                 abi_x86_32
dev-libs/dbus-glib                abi_x86_32
dev-libs/elfutils                 abi_x86_32
dev-libs/expat                    abi_x86_32
dev-libs/fribidi                  abi_x86_32
dev-libs/glib                     abi_x86_32
dev-libs/gmp                      abi_x86_32
dev-libs/icu                      abi_x86_32
dev-libs/json-glib                abi_x86_32
dev-libs/leancrypto               abi_x86_32
dev-libs/libevdev                 abi_x86_32
dev-libs/libffi                   abi_x86_32
dev-libs/libgcrypt                abi_x86_32
dev-libs/libgpg-error             abi_x86_32
dev-libs/libgudev                 abi_x86_32
dev-libs/libgusb                  abi_x86_32
dev-libs/libpcre2                 abi_x86_32
dev-libs/libtasn1                 abi_x86_32
dev-libs/libunistring             abi_x86_32
dev-libs/libusb                   abi_x86_32
dev-libs/libxml2                  abi_x86_32
dev-libs/lzo                      abi_x86_32
dev-libs/nettle                   abi_x86_32
dev-libs/nspr                     abi_x86_32
dev-libs/nss                      abi_x86_32
dev-libs/openssl                  abi_x86_32
dev-libs/wayland                  abi_x86_32
dev-util/glslang                  abi_x86_32
dev-util/spirv-tools              abi_x86_32
dev-util/sysprof-capture          abi_x86_32
dev-util/vulkan-utility-libraries abi_x86_32
gnome-base/librsvg                abi_x86_32
gui-libs/libdecor                 abi_x86_32
llvm-core/clang                   abi_x86_32
llvm-core/llvm                    abi_x86_32
media-gfx/graphite2               abi_x86_32
media-libs/alsa-lib               abi_x86_32
media-libs/flac                   abi_x86_32
media-libs/fontconfig             abi_x86_32
media-libs/freetype               abi_x86_32
media-libs/glu                    abi_x86_32
media-libs/harfbuzz               abi_x86_32
media-libs/lcms                   abi_x86_32
media-libs/libdisplay-info        abi_x86_32
media-libs/libepoxy               abi_x86_32
media-libs/libglvnd               abi_x86_32
media-libs/libjpeg-turbo          abi_x86_32
media-libs/libogg                 abi_x86_32
media-libs/libpng                 abi_x86_32
media-libs/libpulse               abi_x86_32
media-libs/libsdl2                abi_x86_32
media-libs/libsdl3                abi_x86_32
media-libs/libsndfile             abi_x86_32
media-libs/libva                  abi_x86_32
media-libs/libvorbis              abi_x86_32
media-libs/libwebp                abi_x86_32
media-libs/mesa                   abi_x86_32
media-libs/openal                 abi_x86_32
media-libs/opus                   abi_x86_32
media-libs/tiff                   abi_x86_32
media-libs/vulkan-layers          abi_x86_32
media-libs/vulkan-loader          abi_x86_32 layers
media-sound/lame                  abi_x86_32
media-sound/mpg123-base           abi_x86_32
media-video/pipewire              abi_x86_32
net-dns/c-ares                    abi_x86_32
net-dns/libidn2                   abi_x86_32
net-libs/gnutls                   abi_x86_32
net-libs/libasyncns               abi_x86_32
net-libs/libndp                   abi_x86_32
net-libs/libpsl                   abi_x86_32
net-libs/nghttp2                  abi_x86_32
net-libs/nghttp3                  abi_x86_32
net-libs/ngtcp2                   abi_x86_32
net-misc/curl                     abi_x86_32
net-misc/networkmanager           abi_x86_32
net-print/cups                    abi_x86_32
sys-apps/dbus                     abi_x86_32
sys-apps/lm-sensors               abi_x86_32
sys-apps/systemd                  abi_x86_32
sys-apps/systemd-utils            abi_x86_32
sys-apps/util-linux               abi_x86_32
sys-libs/gdbm                     abi_x86_32
sys-libs/gpm                      abi_x86_32
sys-libs/libcap                   abi_x86_32
sys-libs/libudev-compat           abi_x86_32
sys-libs/ncurses                  abi_x86_32
sys-libs/pam                      abi_x86_32
sys-libs/readline                 abi_x86_32
sys-libs/zlib                     abi_x86_32
virtual/glu                       abi_x86_32
virtual/libelf                    abi_x86_32
virtual/libiconv                  abi_x86_32
virtual/libintl                   abi_x86_32
virtual/libudev                   abi_x86_32
virtual/libusb                    abi_x86_32
virtual/opengl                    abi_x86_32
virtual/zlib                      abi_x86_32
x11-libs/cairo                    abi_x86_32
x11-libs/extest                   abi_x86_32
x11-libs/gdk-pixbuf               abi_x86_32
x11-libs/gtk+                     abi_x86_32
x11-libs/libdrm                   abi_x86_32
x11-libs/libICE                   abi_x86_32
x11-libs/libpciaccess             abi_x86_32
x11-libs/libSM                    abi_x86_32
x11-libs/libvdpau                 abi_x86_32
x11-libs/libX11                   abi_x86_32
x11-libs/libXau                   abi_x86_32
x11-libs/libxcb                   abi_x86_32
x11-libs/libXcomposite            abi_x86_32
x11-libs/libXcursor               abi_x86_32
x11-libs/libXdamage               abi_x86_32
x11-libs/libXdmcp                 abi_x86_32
x11-libs/libXext                  abi_x86_32
x11-libs/libXfixes                abi_x86_32
x11-libs/libXft                   abi_x86_32
x11-libs/libXi                    abi_x86_32
x11-libs/libXinerama              abi_x86_32
x11-libs/libxkbcommon             abi_x86_32
x11-libs/libXrandr                abi_x86_32
x11-libs/libXrender               abi_x86_32
x11-libs/libXScrnSaver            abi_x86_32
x11-libs/libxshmfence             abi_x86_32
x11-libs/libXtst                  abi_x86_32
x11-libs/libXxf86vm               abi_x86_32
x11-libs/pango                    abi_x86_32
x11-libs/pixman                   abi_x86_32
x11-libs/xcb-util-keysyms         abi_x86_32
x11-misc/colord                   abi_x86_32

# Because we have an nvidia card
gui-libs/egl-gbm            abi_x86_32
gui-libs/egl-wayland        abi_x86_32
gui-libs/egl-wayland2       abi_x86_32
gui-libs/egl-x11            abi_x86_32
x11-drivers/nvidia-drivers  abi_x86_32

# Recommended to enable X through emerge
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime]
# required by games-util/steam-launcher (argument)
>=sys-apps/dbus-1.16.2 X
# required by gui-libs/gtk-4.20.4::gentoo[vulkan]
# required by gui-libs/libadwaita-1.8.6::gentoo
# required by gnome-extra/zenity-4.2.2::gentoo
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime,dialogs]
# required by games-util/steam-launcher (argument)
>=media-libs/vulkan-loader-1.4.341.0 X
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[video_cards_nvidia]
# required by games-util/steam-launcher (argument)
>=x11-drivers/nvidia-drivers-595.84 X
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[vulkan,video_cards_nvidia]
# required by games-util/steam-launcher (argument)
>=media-libs/vulkan-layers-1.4.341.0 X
# required by x11-drivers/nvidia-drivers-595.84::gentoo[X]
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[video_cards_nvidia]
# required by games-util/steam-launcher (argument)
>=media-libs/libglvnd-1.7.0 X
# required by x11-libs/gtk+-2.24.33-r3::gentoo
# required by x11-themes/gtk-engines-adwaita-3.28-r1::gentoo
>=x11-libs/cairo-1.18.4-r1 X
# required by app-i18n/ibus-1.5.33::gentoo[gtk4]
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime]
# required by games-util/steam-launcher (argument)
>=gui-libs/gtk-4.20.4 X
# required by gui-libs/gtk-4.20.4::gentoo[X]
# required by gui-libs/libadwaita-1.8.6::gentoo
# required by gnome-extra/zenity-4.2.2::gentoo
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime,dialogs]
# required by games-util/steam-launcher (argument)
>=media-libs/mesa-26.0.8 X

# required by x11-drivers/nvidia-drivers-595.84::gentoo[tools]
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[video_cards_nvidia]
# required by games-util/steam-launcher (argument)
>=x11-libs/gtk+-3.24.52 X
# required by gui-libs/gtk-4.20.4::gentoo
# required by gui-libs/libadwaita-1.8.6::gentoo
# required by gnome-extra/zenity-4.2.2::gentoo
# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime,dialogs]
# required by games-util/steam-launcher (argument)
>=media-libs/libepoxy-1.5.10-r3 X

games-util/steam-launcher -steamruntime

````

````sh
# in /etc/portage/package.accept_keywords/steam
*/*::steam-overlay
games-util/game-device-udev-rules
sys-libs/libudev-compat
````

````sh
# in /etc/portage/package.license/steam
games-util/steam-launcher ValveSteamLicense
````

````
From https://wiki.gentoo.org/wiki/Steam,

The overlay enables the Steam runtime by default. If you'd like to rely solely on Gentoo packages, then disable the `steamruntime` USE flag. Use the esteam utility later to scan your installed native Linux games for additional Gentoo packages required by them. Note that Gentoo packages do not cover the entirety of the runtime, so a small number of games may not work.
````

Let's try to disable `steamruntime`. Added to above use flags.

````sh
su
emerge --ask games-util/steam-launcher
````

Moving `/etc/portage/package.license` the file into `/etc/portage/package.license/license` so we could also add a `steam` license file.

There are some configurations that are recommended to be added, and they all depend on `X`. But I don't want to add `X`.

````sh
find /etc/portage -type f | grep '._cfg' # out {
	/etc/portage/package.use/x11-drivers/._cfg0000_nvidia-drivers
# }

cat /etc/portage/package.use/x11-drivers/._cfg0000_nvidia-drivers # out (relevant) {
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime]
	# required by games-util/steam-launcher (argument)
	>=sys-apps/dbus-1.16.2 X
	# required by gui-libs/gtk-4.20.4::gentoo[vulkan]
	# required by gui-libs/libadwaita-1.8.6::gentoo
	# required by gnome-extra/zenity-4.2.2::gentoo
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime,dialogs]
	# required by games-util/steam-launcher (argument)
	>=media-libs/vulkan-loader-1.4.341.0 X
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[video_cards_nvidia]
	# required by games-util/steam-launcher (argument)
	>=x11-drivers/nvidia-drivers-595.84 X
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[vulkan,video_cards_nvidia]
	# required by games-util/steam-launcher (argument)
	>=media-libs/vulkan-layers-1.4.341.0 X
	# required by x11-drivers/nvidia-drivers-595.84::gentoo[X]
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[video_cards_nvidia]
	# required by games-util/steam-launcher (argument)
	>=media-libs/libglvnd-1.7.0 X
	# required by x11-libs/gtk+-2.24.33-r3::gentoo
	# required by x11-themes/gtk-engines-adwaita-3.28-r1::gentoo
	>=x11-libs/cairo-1.18.4-r1 X
	# required by app-i18n/ibus-1.5.33::gentoo[gtk4]
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime]
	# required by games-util/steam-launcher (argument)
	>=gui-libs/gtk-4.20.4 X
	# required by gui-libs/gtk-4.20.4::gentoo[X]
	# required by gui-libs/libadwaita-1.8.6::gentoo
	# required by gnome-extra/zenity-4.2.2::gentoo
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime,dialogs]
	# required by games-util/steam-launcher (argument)
	>=media-libs/mesa-26.0.8 X
# }

# second pass
cat /etc/portage/package.use/x11-drivers/._cfg0000_nvidia-drivers # out (relevant) {
	# required by x11-drivers/nvidia-drivers-595.84::gentoo[tools]
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[video_cards_nvidia]
	# required by games-util/steam-launcher (argument)
	>=x11-libs/gtk+-3.24.52 X
	# required by gui-libs/gtk-4.20.4::gentoo
	# required by gui-libs/libadwaita-1.8.6::gentoo
	# required by gnome-extra/zenity-4.2.2::gentoo
	# required by games-util/steam-launcher-1.0.0.85-r3::steam-overlay[-steamruntime,dialogs]
	# required by games-util/steam-launcher (argument)
	>=media-libs/libepoxy-1.5.10-r3 X
# }
````

Let's add the above to `/etc/portage/package.use/steam`

https://wiki.gentoo.org/wiki/Steam seems to suggest it is required to have it though.

We also are getting a circular dependency with `sys-libs/ncurses`, which they resolve with:

````sh
USE="-gpm" emerge --ask --oneshot sys-libs/ncurses
emerge --ask games-util/steam-launcher
emerge --ask --oneshot sys-libs/ncurses gpm
````

There are 163 packages for steam here.

2026-07-27 Wk 31 Mon - 07:27 +03:00

````
3DERROR: ld.so: object '/usr/lib/libextest.so' from LD_PRELOAD cannot be preloaded (wrong ELF class: ELFCLASS32): ignored.
ERROR: ld.so: object '/usr/lib/libextest.so' from LD_PRELOAD cannot be preloaded (wrong ELF class: ELFCLASS32): ignored.
ERROR: ld.so: object '/usr/lib/libextest.so' from LD_PRELOAD cannot be preloaded (wrong ELF class: ELFCLASS32): ignored.
````

When trying to run `steam` in sway

````
* Error: esteam started as unprivileged user and sudo unavailable
````

When we try to run `esteam` instead.

 > 
 > Do not run emerge --unmerge @steam to remove Steam as it may make the system unusable. Instead use emerge --ask --depclean @steam for this method.

Something to keep in mind.

They had a section on running this as chroot to isolate its dependencies from our own. Might try later if I run into issues.

It is supposed to run as X.

https://wiki.gentoo.org/wiki/Xwayland

````sh
emerge --ask x11-base/xwayland
````

 > 
 > The most common game related issues are solved by enabling the `stack-realign` USE flag on the [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) package and re-emerge the [@world set](https://wiki.gentoo.org/wiki/World_set_(Portage) "World set (Portage)"). It is a good

 > 
 > If you want to play games through proton, don't forget to add the `vulkan` USE flag on the [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) package.

````sh
# in /etc/portage/package.use/sys-libs/glibc {
	sys-libs/glibc stack-realign
# }

# in /etc/portage/package.use/media-libs/mesa {
	media-libs/mesa vulkan
# }

emerge --ask --changed-use media-libs/mesa # did not run any install
emerge --ask --changed-use --deep @world
````

2026-07-27 Wk 31 Mon - 09:44 +03:00

I have `X` disabled for sway:

````sh
equery uses sway

# out (relevant)
 * Found these USE flags for gui-wm/sway-1.11:
 U I
 - - X          : Enable support for X11 applications (XWayland)
````

Enable and rebuild. We need Xwayland for steam.

````diff
# in /etc/portage/package.use/gui-wm/sway
-gui-wm/sway wallpapers -X
+gui-wm/sway wallpapers X
````

````sh
# recommended changes
find /etc/portage/ -type f | grep _cfg # out {
	/etc/portage/package.use/x11-drivers/._cfg0000_nvidia-drivers
# }

cat /etc/portage/package.use/x11-drivers/._cfg0000_nvidia-drivers # out (relevant) {
	# required by gui-wm/sway-1.11::gentoo
	# required by @selected
	# required by @world (argument)
	>=gui-libs/wlroots-0.19.2:0.19 X
# }
````

````sh
# in /etc/portage/package.use/gui-libs/wlroots {
	# required by gui-wm/sway-1.11::gentoo
	# required by @selected
	# required by @world (argument)
	>=gui-libs/wlroots-0.19.2:0.19 X
# }

su
emerge --ask --changed-use gui-wm/sway
````

2026-07-27 Wk 31 Mon - 10:32 +03:00

With a reboot we're able to launch steam with xwayland. Downloaded some games for testing. We're able to start a windows only game, as well as a linux game.

There is however no sound in these games. Restarting `gentoo-pipewire-launcher restart` a few times brought sound back in my headset in youtube. Let's try with a game.

Okay it works with a game. So this is not related to steam.

2026-07-27 Wk 31 Mon - 12:21 +03:00

Tekken 8 Starts!

OK
