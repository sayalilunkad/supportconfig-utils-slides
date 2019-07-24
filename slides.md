## Vision for tools

* Deliver **fastest possible path** from being given
  supportconfigs to starting useful debugging
* Make most likely **area to investigate immediately available**
* Make **debugging as efficient as possible**
* Provide a **rich UI** for navigating / analysing
  (potentially many) log files
* Extend to **support all SUSE products** out of the box

## Overview of tools used

* supportconfig-utils is built using:

    * **lnav** - Log file navigator
    * **tmux** - terminal multiplexer
    * **unpack** - utility for unpacking any kind of archive
    * Some small-ish hacks in Ruby and shell

## Workflow

One click from browser to create new analysis session!

* Automatically unpacks supportconfig
* Automatically unpacks individual files into `rootfs/`
* Sets up new tmux session
* Extracts essential information
* Determines SUSE product and context (e.g. SOC Crowbar admin node)
* Launches `lnav` filters

## Install and Configure

* Install the supportconfig-utils package from OBS:

    * https://software.opensuse.org/package/supportconfig-utils

  This also installs `tmux-lib` and `unpack`, and sets up
  MIME types / desktop handlers etc.

* Default terminal session launched using `xdg-terminal`

* Can be changed in
    * `~/.config/supportconfig-utils/tmux-window` or
    * `/etc/sysconfig/supportconfig-utils`

  e.g.

        SUPPORTCONFIG_UTILS_TMUX_TERMINAL="urxvt-256color -g 200x60 -e"

## Introduction to lnav

* `less(1)` on steroids
* Specialises in timestamped log files
* Can auto-detect and parse many formats out of the box
    * even more via `lnav -i extra`
    * easy to add your own
* Chronological merging of files
* `tail -f` watching of live files

## Introduction to lnav (part 2)

Packed with [useful features](http://lnav.org/features)!

* Syntax highlighting
* Time-based navigation (e.g. jump back 1 hour)
* Navigation to errors / warnings
    * including via zoomable histogram!
* Regexp filtering
* Data extraction
* Full SQLite query engine embedded!
* Command completion

## Introduction to tmux

* Terminal multiplexer
* A modern alternative to GNU screen
* Slightly easier to script automated setup of sessions

## tmux crash course for GNU screen users

* Escape key is `Control+b` not `Control+a`
* Otherwise somewhat similar default bindings
* `Control+b ?` for other key bindings

## tmux windows

Currently oriented towards SOCC (please help us change this)

* 0: spare shell window
* VM: /var/log/messages
* lcc: local chef client
* cc: chef-client

## supportconfig-tmux features

* Initial commands are "pushed" in as simulated keystrokes => can reuse shell history
* lnav2 wrapper
* List of deployment layout, i.e. nodes and clusters
* Tmux window per node
* Option to configure auto-delete supportconfig files at the end (after closing the tmux session)
* lnav2 will show an error after launching supportconfig-utils
* sets up r/R keyboard shortcuts for jumping by minute

## Cloud specific features

* lnav-crowbar-admin
* lnav-chef-sync-marks
* post-process-crowbar-logs

## supportconfig-tmux-safe

* When unpack fails will show the error before closing the session

## postprocessing

* Extraction of "topics" (keystone, nova, ...)
* Could add an 'annotations' file with timestamp / comment to add personal comments to the lnav views
* Mapping of MAC and IP addresses to aliases for readability
* Postprocessor stdout can be xarg'ed into lnav to look at the relevant logfiles in an lnav session
