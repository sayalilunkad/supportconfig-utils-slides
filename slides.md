## Vision for tools

* Deliver **fastest possible path** from being given a supportconfig to start useful debugging
* Make most likely **area to investigate immediately available**
* Make **debugging as efficient as possible**
* Extend to **support all SUSE products** out of the box

## Overview of tools used

* supportconfig-utils is built using:

	* **lnav** - Log file navigator
	* **tmux** - terminal multiplexer
	* **unpack** - utility for unpacking any kind of archive


## Install and Configure

* Install the supportconfig-utils package (this also installs tmux-lib)
	* `zypper in supportconfig-utils`

* Default terminal session launched using `xdg-terminal`

* Can be changed in ~/.config/supportconfig-utils/tmux-window or /etc/sysconfig/supportconfig-utils
	* `SUPPORTCONFIG_UTILS_TMUX_TERMINAL="urxvt-256color -g 200x60 -e"`

## Workflow

unpack -> sets up new tmux session -> lnav filters

## Introduction to tmux

## How to use it?

## tmux windows

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
