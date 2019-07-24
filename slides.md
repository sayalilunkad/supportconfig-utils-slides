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

## UI

* Purely terminal based
* Can be run from existing terminal session
  via `supportconfig-tmux`
* Can be launched in new terminal via `supportconfig-tmux-window`
    * One click from browser to create new analysis session!
* Currently 1 terminal / tmux session per supportconfig
    * May get smarter in the future

## Introduction to lnav

* https://lnav.org
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
* Client/server model (UNIX sockets)
* Multiple sessions per server
* Multiple windows per session
* Multiple panes per window
* Slightly easier to script automated setup of sessions

## Workflow

* Launch from browser or CLI
* Automatic session setup:
    * Unpacks supportconfig
    * Unpacks individual files into `rootfs/`
    * Sets up new `tmux` session
    * Extracts essential information
    * Determines SUSE product and context (e.g. SOC Crowbar admin node)
    * Launches windows with `lnav` etc. showing relevant info
* Start analysing!

## tmux crash course for GNU screen users

* Escape key is `Control+b` not `Control+a`
* Otherwise somewhat similar default bindings
* `Control+b ?` for other key bindings

## Install and Configure

* Install the supportconfig-utils package from OBS:

    * https://software.opensuse.org/package/supportconfig-utils

  This also installs `tmux-lib` and `unpack`, and sets up
  MIME types / desktop handlers etc.

* Default terminal session launched using `xdg-terminal`.
  Can be changed in
    * `~/.config/supportconfig-utils/tmux-window` or
    * `/etc/sysconfig/supportconfig-utils`

  e.g.

        SUPPORTCONFIG_UTILS_TMUX_TERMINAL=\
        "urxvt-256color -g 200x60 -e"

* `lnav` from OBS
    * https://build.opensuse.org/package/show/home:nicolasbock:lnav/lnav
    * https://build.opensuse.org/package/show/server:monitoring/lnav

## Features

* Initial commands are "pushed" in as simulated keystrokes
    * just press `Up` to reuse shell history
* `lnav2` wrapper around `lnav`
    * jumps to first error on launch
    * sets up `r`/`R` keyboard shortcuts for jumping by minute
* Automatically extracts/displays useful product-specific info
    * e.g. nodes / IPs in cluster
    * tmux window per node-specific logs
* Optionally auto-delete supportconfig files when closing tmux session

## tmux windows

Currently oriented towards SOCC (please help us change this!)

E.g. for SOCC controller nodes:

* `0`: spare shell window
* `VM`: `/var/log/messages`
* `lcc`: local `chef-client` log

## Cloud-specific features

* `lnav-crowbar-admin`
* `lnav-chef-sync-marks`
* `post-process-crowbar-logs`

## `supportconfig-tmux-safe`

When `unpack` fails, safe wrapper will show the error before closing
the session and hence the window.

## Scaling up to lots of logs

Post-processing:

* Mapping of MAC and IP addresses to aliases for readability
    * Within log files
    * Within log file path names
* Extraction of "topics" (keystone, nova, ...)
    * Pipe `STDOUT` from post-processor `| xargs lnav2` to start
      `lnav` session for selected topics

Tip: manually maintain an 'annotations' file with timestamps / comments

  * Allows human analysis to be presented in `lnav` views
    alongside the log files.

## Questions?
