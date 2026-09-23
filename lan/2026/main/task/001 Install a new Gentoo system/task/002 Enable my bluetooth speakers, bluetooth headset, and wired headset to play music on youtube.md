---
context_type: task
status: todo
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-task-1760b1](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-task-1760b1)

Overview: [001 Overview Install a new Gentoo system](../entry/001%20Overview%20Install%20a%20new%20Gentoo%20system.md)

# Objective

We have librewolf now: [001 Install a browser on gentoo - librewolf](001%20Install%20a%20browser%20on%20gentoo%20-%20librewolf.md), but going to youtube, we can't play any music.

I have a bluetooth headset. A wired one. And finally bluetooth speakers. I would like to test each and ensure it's working.

* [x] Bluetooth speaker and wired headset tested and can be switched to
* [ ] We have a good cli tool for sound control
* [ ] Resolved issue with `gentoo-pipewire-launcher` needing to be relaunched at boot

# Journal

2026-07-26 Wk 30 Sun - 21:18 +03:00

https://wiki.gentoo.org/wiki/Bluetooth

Let's enable bluetooth in general as we have bluetooth devices:

````sh
# in /etc/portage/make.conf {
	USE="${USE} bluetooth"
# }

su
emerge --ask --update --changed-use --deep @world
emerge --ask --noreplace net-wireless/bluez
````

emerging `net-wireless/bluez` didn't install anything however, so we have it.

````sh
rc-update add bluetooth default
rc-service bluetooth start
````

Now we're able to use `bluetoothctl`. It was given a segfault before.

We can use `bluetoothctl > scan on` to initiate discovery. Then we can use `bluetoothctl > connect {dev_addr}` to connect to that device. The scan and also `bluetoothctl > devices` should show us the address, which looks like the device's MAC.

2026-07-26 Wk 30 Sun - 22:16 +03:00

Simply connecting to the bluetooth speaker device was not enough to configure librewolf to redirect sound to it.

https://wiki.gentoo.org/wiki/PulseAudio

https://wiki.gentoo.org/wiki/PipeWire

For cases of systems that still rely on pulse audio, pipe wire says it can emulate it. So it is in a sense backwards compatible with it.

The pipewire section says all desktop profiles already come with it.

Check that we have a pipewire group:

````sh
cat /etc/group | grep pipewire

# out
pipewire:x:509:
````

We need to be part of this group:

````sh
su
usermod -aG pipewire lan
````

2026-07-26 Wk 30 Sun - 22:57 +03:00

Create system wide pipewire config,

````sh
su
cp /usr/share/pipewire/pipewire.conf /etc/pipewire
````

`$XDG_CONFIG_HOME` is expected to be in `~/.config` though right now it's not set for me. But we don't need to use it right now.

It's recommended we use `gentoo-pipewire-launcher`, since apparently there's currently no way to configure pipewire directly by openrc

````
From https://wiki.gentoo.org/wiki/PipeWire,

gentoo-pipewire-launcher is a temporary solution for OpenRC-based systems. Eventually, once OpenRC user services (introduced in OpenRC 0.60) are well-established, gentoo-pipewire-launcher might be split into a separate package or removed altogether.
````

Hmm. Some audio related sway logs

````
[Child 5386, MediaDecoderStateMachine #1] WARNING: 7f8a99d60ee0 OpenCubeb() failed to init cubeb: file librewolf-153.0-3/dom/media/AudioStream.cpp:279
[Child 5386, MediaDecoderStateMachine #1] WARNING: Decoder=7f8a9d410100 [OnMediaSinkAudioError]: file librewolf-153.0-3/dom/media/MediaDecoderStateMachine.cpp:4630
````

Seems we should only start `gentoo-pipewire-launcher` where `DBUS_SESSION_BUS_ADDRESS` is set. This is not set in tmux, but when I start a new foot window in sway it is.

This is no longer a problem if we start tmux in foot instead. I made it so that `./start_sway.sh` logs to `sway_log.log` in case we need to `tail -f` it. Amending [000 Install new Gentoo system raw journal](../entry/000%20Install%20new%20Gentoo%20system%20raw%20journal.md)

2026-07-27 Wk 31 Mon - 00:28 +03:00

After a reboot, which may have had an effect, this now works on my wired headset on librewolf. Just had to launch `gentoo-pipewire-launcher` and play a song on youtube.

In fact when I connect my bluetooth speakers, it will then switch from my wired headset to the speakers. Then I can use `bluetoothctl disconnect DEV_ADDR` to disconnect it, and it switches back to my wired headset.

2026-07-27 Wk 31 Mon - 08:37 +03:00

It seems we already have the user services:

https://wiki.gentoo.org/wiki/PipeWire

````sh
rc-update add -U pipewire default
rc-update add -U pipewire-pulse default
````

We can use `pw-cli` to control this. It's pretty low level control.

We can find some sound nodes by `pw-cli ls | grep "node.name"` or just mark it: `pw-cli ls | grep "node.name\|" --color=always | less -R`.

Hmm. Audio is still not playing even after doing those `rc-update`s on reboot.

But if you try to run `gentoo-pipewire-launcher`, it says it's already running:

````sh
PipeWire already running, exiting.
(Use 'gentoo-pipewire-launcher restart' to restart PipeWire and WirePlumber.)
````

But manually restarting it with `gentoo-pipewire-launcher restart` got the audio working again.

2026-08-05 Wk 32 Wed - 16:45 +03:00

Need to get a simpler sound control TUI,

* https://wiki.gentoo.org/wiki/PipeWire
* $\to$ https://wiki.gentoo.org/wiki/Wiremix

````sh
# in /etc/portage/package.accept_keywords/media-sound/wiremix {
	media-sound/wiremix ~amd64
# }

su
emerge --ask media-sound/wiremix
````
