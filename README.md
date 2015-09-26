AFX II Tool
===========

The `afx2-tool` works with Fractal Audio Systems' Axe-FX II guitar
processors. The Mark 1, Mark 2, FX and FX+ models are all supported. You
can use the `afx2-tool` to backup and restore individual patches, patch
banks, user cabs (IRs) and system data.

Prerequisites
-------------

The `afx2-tool` is a command-line script. In addition to the usual
complement of Linux tools, `afx2-tool` also requires the `amidi`
utility. The `afx2-tool` will check for the presence of `amidi`. If
`amidi` is not installed on your system, check your package manager. The
ALSA Utilities package usually contains `amidi`.

Your Linux system must be connected to the Axe-FX via a USB cable. In
order for your computer to be able to communicate with the Axe-FX, the
computer must first load a driver onto the Axe-FX. This driver loader
can be enabled on your computer using the `afx2usb-linux` package;
you'll need to install `afx2usb-linux` before using `afx2-tool`.

Operation
---------

You'll need to know the MIDI channel number of your Axe-FX. You can find
this at the top of the `I/O > MIDI` page on the front-panel display of
your Axe-FX. The MIDI channel number defaults to 1 in both the Axe-FX
and `afx2-tool`. If your Axe-FX uses a channel other than 1, inform
`afx2-tool` via its `-c #` option.

The `afx2-tool` uses MIDI sysex commands to transfer data. These
commands do not address individual devices. Therefore, it is important
to disconnect other USB MIDI devices from the computer port to which
your Axe-FX is connected. Make sure that the Axe-FX (and only **one**
Axe-FX) is the only MIDI-capable device connected to your computer's
USB port before running `afx2-tool`. It is OK to have other USB devices
connected so long as they don't have a MIDI-over-USB interface.

Individual patches are transferred from the Axe-FX by first selecting
the patch in the Axe-FX. The `afx2-tool` automates this process via the
`-p <range>` option. It's a good idea to turn off or mute all amplifiers
and monitors connected to your Axe-FX before invoking `afx2-tool` with
the `-p <range>` option. This precaution can save you from being startled
when `afx2-tool` switches your Axe-FX to a patch having a lot of volume
or gain. Note that `afx2-tool` will, when it's finished downloading all
patches in the specified range, select the patch that was active prior
to the downloads.

Current Implementation Status
-----------------------------

September 25, 2015: version 0.9.0

Although `afx2-tool` is designed to support all Axe-FX II models, it
has only been tested (as I write this note) on my own Axe-FX II XL+.

Likewise, although `afx2-tool` is written using common Linux tools,
I have only tested it on my own Fedora 22 systems.

I am reasonably convinced that the backups of individual presets,
preset banks and system data are viable, based upon the size and content
of the downloaded files. However, I have not implemented the upload
functionality; therefore, I will not assert that the backups are usable.

I don't yet have all of the information I need regarding the user cab
commands. It's quite likely that the downloaded cab won't be the one
you specified on the command line. This will be corrected for the 1.0.0
release.

Support
-------

My time is limited by the demands of a day job and the desire to have a
reasonable work/life balance. While I will read commentary sent to me,
I cannot promise to respond in a timely fashion. If you wish to offer
a bug fix, please do so via a pull request to the project's GitHub
repository.

Acknowledgments
---------------

Thanks to Michael Pickens for detailing some of the necessary sysex
commands.

References
----------

1. [afx2usb-linux](https://github.com/TieDyedDevil/afx2usb-linux)
2. [afx2-tool](https://github.com/TieDyedDevil/afx2tool-linux)
