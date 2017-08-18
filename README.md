display-visor
=============

This script, and related hooks, help updating the Xorg monitor configuration when dealing with multiple monitors.  
It is useful for desktop environments and window managers (e.g., [i3wm](http://i3wm.org/)) that don't handle this.  

This fork mainly adapts the original script to my work laptop and docking station, and fixes minor bugs.

How it works
------------

When executed, it checks the available and connected display outputs and sets the optimal resolution for each (as determined by xrandr). 
The script then waits for a signal to restart the procedure.

At the moment two outputs are defined: `eDP-1` and `DP-2-2`. For now, layout configuration is hard-coded. I am hoping to make this more dynamic.

How to use it
------------

    Usage: display-visor [-f] [-i] [-l [switch]]

		-i, --i3	Test for i3wm instance.
                             For avoiding conflict with multiple environments.
		-l, --lid	Check laptop lid status.
                             Ignored/Assumed closed if not given. 
                             It is possible to specify switch. Defaults to 'LID0'
                             If unsure, look under /proc/acpi/button/lid/...
		-v, --version	Print version info.


## Start:
Simply set the script to start upon login.

i3wm config example:

    exec --no-startup-id display-visor -f -l

## Signal:
The script waits for a `RTMIN+5` real-time signal. This can be sent with pkill like so:

    pkill -x -RTMIN+5 display-visor

## Events:
Some default event signallers are included.

 * __udev__ - A hotplug rule for when cables are (dis)connected.
 * __acpid__ - A lid switch event action. Useful when `-l` argument is used. [1]
 * __systemd-sleep__ - A wake-up hook. [2]

Installation
------------

The original script can be installed on Arch Linux through the [AUR](https://aur.archlinux.org/packages/display-visor) as `display-visor`.  

Or you can manually install it with:

    $ sudo make (un)install

Dependencies
------------

* [xorg-xrandr](http://www.x.org/wiki/Projects/XRandR/)
* [acpid](http://sourceforge.net/projects/acpid2/)(for lid events)

Notes
-----
 [1] Remember to restart `acpid` service for this to take effect.
 [2] Please see [Issue #8](https://github.com/beanaroo/display-visor/issues/8)


Credits: [codingtony](https://github.com/codingtony/udev-monitor-hotplug)

