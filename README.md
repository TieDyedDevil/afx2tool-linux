AFX II Tool
===========

```
version: 1.0.1
Jun 21, 2016
```

The `afx2-tool` works with Fractal Audio Systems' Axe-FX II guitar
processors. The Mark 1, Mark 2, FX and FX+ models are all supported. You
can use the `afx2-tool` to backup and restore individual patches, patch
banks, user cabs (IRs) and system data.

You can also use `afx2-tool` to upload new firmware to your Axe-FX II.

The `afx2-tool` is designed to run from the command line or a script.

Prerequisites
-------------

In addition to the usual complement of Linux tools, `afx2-tool` requires
the `amidi` utility. The `afx2-tool` will check for the presence of
`amidi`. If `amidi` is not installed on your system, check your package
manager.  The ALSA Utilities package usually contains `amidi`.

Your Linux system must be connected to the Axe-FX via a USB cable. In
order for your computer to be able to communicate with the Axe-FX, the
computer must first load a driver onto the Axe-FX. This driver loader
can be enabled on your computer using the `afx2usb-linux` package;
you'll need to install and run `afx2usb-linux` before using `afx2-tool`.

Installation
------------

Copy the `afx2-tool` file to a directory on your $PATH. Typically this
will be either ~/bin (some Linux distros set up user accounts with ~/bin
on $PATH) or /usr/local/bin.

In the first case, just copy or move the file.

In the second case, it's easiest to use the `install` command:

```
$ sudo install -m 755 <path-to>/afx2-tool /usr/local/bin
```

You may also find it useful to stash this (`README.md`) file somewhere
as a convenient reference.

Prefix Export vs. Download
--------------------------

The `afx2-tool` has two ways to save an Axe-FX preset to a file: export
and download.

The export (`-E`) operation saves the contents of the Axe-FX edit
buffer. The preset number and name are used to create the name of the
file. Any unsaved modifications in the edit buffer will be saved to
the file.

*IMPORTANT*: Unsaved changes in the Axe-FX edit buffer are lost by the
`-b`, `-e` and `-p` options.

To export a preset not already in the edit buffer, `afx2-tool` first
selects the preset.

An unnamed preset is never saved by the export operation.

The download operation saves the contents of the Axe-FX preset
memory. Only the preset number is used to create the name of the file. The
download operation never selects the preset to be downloaded.

All presets, whether named or not, are saved by the download (not export)
operation.

The `afx2-tool` behavior upon uploading a preset (via the `-u` option)
depends upon whether the preset was downloaded (`-p`) or exported (`-E`
or `-e`). A downloaded preset is automatically saved to the preset
memory denoted by the numeric portion of the filename. You can change
the location to which `afx2-tool` stores a downloaded preset simply
by editing the numeric portion of the preset file's name. Be careful
to specify the intended preset number; `afx2-tool` restores downloaded
presets without confirmation.

An exported preset is stored only in the Axe-FX edit buffer until you
manually save the preset using the Axe-FX's front-panel STORE button;
this gives you the opportunity to audition the preset and then save it
to a location of your choosing.

You should, as a rule of thumb, download presets as part of your backup
regime and export presets to share with others or to store in a library.

Downloaded and exported presets contain exactly the same data; only the
filename is different. The name of a download preset *must* end with
exactly `Preset<digits>.syx`. Additional words at the beginning of the
filename (e.g. a timestamp) must be set off by at least one space. Any
preset file not adhering to these naming conventions will be treated as
a preset export file.

Timestamps and Overwrites
-------------------------

Files of the same name are normally overwritten without warning. This is
most useful in the case where you want your downloaded files to reflect
the current state of your Axe-FX.

If you add a `-t` option to the `afx2-tool` command line, each file name
will be prefixed with a date and time. This is useful in case you wish to
record a history of changes to your Axe-FX programming.

Dependencies
------------

Your Axe-FX programming depends upon information stored in several places.
A preset, for example, depends upon system settings and possibly a user
cab. It is up to you to download all necessary files; the `afx2-tool` does
not keep track of dependencies.

To be clear: to back up all of your Axe-FX programming, you must download
(or export) all of your presets or banks, all of your user cabs and the
system data. I suggest doing all of this in a single command. For example,
the following command will download preset banks A through D, user cabs
1 through 20 and the system data and prepend the same timestamp to all
files so you'll know that they belong together:

```
$ afx2-tool -t -b A-D -i 1-20 -s
```

Cab (IR) Names
--------------

The `afx2-tool` does not extract Cabinet/IR names from the Axe-FX. Instead,
it uses the ordinal of the user cabinet.

This is done because `afx2-tool` currently has no robust way to determine
where user cabinets begin in the list of cabinets which is front-loaded
with system cabinets.

File Compatibility
------------------

The `axf2-tool` will only upload files to the same model of Axe-FX II on
which the file was created. (Axe-FX II Mark 1 and Mark 2 are compatible
with each other.) Use other tools to convert files for use on a different
model.

Operation
---------

You'll need to know the MIDI channel number of your Axe-FX. You can find
this at the top of the `I/O > MIDI` page on the front-panel display of
your Axe-FX. The MIDI channel number defaults to 1 in both the Axe-FX
and `afx2-tool`. If your Axe-FX uses a channel other than 1, inform
`afx2-tool` via its `-c #` option. Note that the MIDI channel number is
needed only for the export operation; you may safely omit specifying a
non-default MIDI channel if your `afx2-tool` command line does not have
a `-E` or `-e` option.

The `afx2-tool` uses MIDI sysex commands to transfer data. These
commands do not address individual devices. Therefore, it is important
to disconnect other USB MIDI devices from the computer port to which
your Axe-FX is connected. Make sure that the Axe-FX (and only **one**
Axe-FX) is the only MIDI-capable device connected to your computer's
USB port before running `afx2-tool`. It is OK to have other USB devices
connected so long as they don't have a MIDI-over-USB interface.

Individual patches are exported from the Axe-FX by first selecting the
patch in the Axe-FX. The `afx2-tool` automates this process via the `-e
<range>` option. It's a good idea to turn off or mute all amplifiers and
monitors connected to your Axe-FX before invoking `afx2-tool` with the
`-e <range>` option. This precaution can save you from being startled
when `afx2-tool` switches your Axe-FX to a patch having a lot of volume
or gain. Note that `afx2-tool` will, when it has finished exporting all
patches in the specified range, select the patch that was active prior
to the downloads.

Display Offset
--------------

If the lowest-numbered patch on your Axe-FX is patch 001, add the `-1`
option to your `afx2-tool` command in order to make patch numbers
on the command line match the patch numbers on your Axe-FX. If the
lowest-numbered patch on your Axe-FX if patch 000, don't use the `-1`
option.

Example Commands
----------------

```
# Export the currently-selected preset.
$ afx2-tool -E

# Download patches 3 through 14 and 21, the system data and user
#  cabs (IRs) 9 and 12.
$ afx2-tool -p 3-14,21 -s -i 9,12

# Download patch banks A through C.
$ afx2-tool -b A-C

# Upload three saved presets and saved system data.
$ afx2-tool -u Preset512.syx -u Preset513.syx -u Preset514.syx -u System.syx
```

Firmware Upload
---------------

The `afx2-tool` may be used to upload a new firmware file to your Axe-FX II:

1. Connect your computer to the Axe-FX II via a USB cable. Note that you must
   have already installed the Axe-FX II driver setup on your computer.

2. Press the UTILITY button on your Axe-FX and navigate to the FIRMWARE page.

3. Press the ENTER button to ready the Axe-FX to receive new firmware.

4. Run this command on your computer: `$ afx2-tool -u <filename>` , where
   `<filename>` is the name of the Axe-FX II firmware file; this will have a
   `.syx` extension.

5. Watch the screen on your Axe-FX II; follow its instructions.

Current Implementation Status
-----------------------------

Although `afx2-tool` is designed to support all Axe-FX II models, it
has had only limited testing.

Likewise, although `afx2-tool` is written using common Linux tools, it
has been tested only on a small number of Linux distributions.

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
