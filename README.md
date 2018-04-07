# Troubleshoot

## Adjust mouse speed and enable natural scrolling

To show information about all input devices, type:

~~~
$ xinput list
⎡ Virtual core pointer                          id=2    [master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer                id=4    [slave  pointer  (2)]
⎜   ↳ Logitech M325                             id=10   [slave  pointer  (2)]
⎜   ↳ Logitech K360                             id=11   [slave  pointer  (2)]
⎣ Virtual core keyboard                         id=3    [master keyboard (2)]
    ↳ Virtual core XTEST keyboard               id=5    [slave  keyboard (3)]
    ↳ Power Button                              id=6    [slave  keyboard (3)]
    ↳ Video Bus                                 id=7    [slave  keyboard (3)]
    ↳ Power Button                              id=8    [slave  keyboard (3)]
    ↳ Sleep Button                              id=9    [slave  keyboard (3)]
    ↳ Logitech K360                             id=12   [slave  keyboard (3)]
~~~

To list all the properties of a particular device, type:

~~~
$ xinput --list-props <device id>
~~~

In my case, type:

~~~
$ xinput --list-props 10
Device 'Logitech M325':
        Device Enabled (139):   1
        Coordinate Transformation Matrix (141): 1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000
        libinput Natural Scrolling Enabled (274):       0
        libinput Natural Scrolling Enabled Default (275):       0
        libinput Left Handed Enabled (276):     0
        libinput Left Handed Enabled Default (277):     0
        libinput Accel Speed (278):     0.000000
        libinput Accel Speed Default (279):     0.000000
        libinput Accel Profiles Available (280):        1, 1
        libinput Accel Profile Enabled (281):   1, 0
        libinput Accel Profile Enabled Default (282):   1, 0
        libinput Scroll Methods Available (283):        0, 0, 1
        libinput Scroll Method Enabled (284):   0, 0, 0
        libinput Scroll Method Enabled Default (285):   0, 0, 0
        libinput Button Scrolling Button (286): 2
        libinput Button Scrolling Button Default (287): 2
        libinput Middle Emulation Enabled (288):        0
        libinput Middle Emulation Enabled Default (289):        0
        libinput Send Events Modes Available (259):     1, 0
        libinput Send Events Mode Enabled (260):        0, 0
        libinput Send Events Mode Enabled Default (261):        0, 0
        Device Node (262):      "/dev/input/event3"
        Device Product ID (263):        1133, 16394
        libinput Drag Lock Buttons (290):       <no items>
        libinput Horizontal Scroll Enabled (291):       1
~~~

I wanted to enable natural scrolling and slow down my mouse. So I am interested in modifying

~~~
libinput Natural Scrolling Enabled (274):       0
libinput Accel Speed (278):     0.000000
~~~

To make these settings persistent, I have to modify xorg config file.

~~~
$ cd /etc/X11/xorg.conf.d/
$ sudo vi 00-mouse-conf
~~~

Inside vi, type in

~~~
Section "InputClass"
  Identifier "00-custom-mouse-conf"
  MatchDriver "libinput"
  MatchProduct "Logitech M325"
  Option "AccelSpeed" "-0.75"
  Option "NaturalScrolling" "1"
EndSection
~~~

Exit vi

~~~
:wq
~~~

Reboot the system, and the settings should be persistent.

~~~
$ reboot
~~~

# Installed Software List
- Chrome
- Atom - Markdown Editor
- Git
- Redshift - F.lux alternative
- Synapse - Alfred alternative
