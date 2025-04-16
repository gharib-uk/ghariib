---
title: "Disabling gnome-tracker"
date: 2025-04-10
draft: false
type: posts
categories:
  - gnome-tracker, gnome
tags:
  - gnome, gnome-tracker
summary: "Disabling GNOME Tracker and Other Info"
---


# Disabling GNOME Tracker and Other Info
=======================================

GNOME's tracker is a CPU and privacy hog. There's a pretty good case as to why
it's neither useful nor necessary here:
http://lduros.net/posts/tracker-sucks-thanks-tracker/

After discovering it chowing 2 cores, I decided to go about disabling it.

## Directories
------------

```
~/.cache/tracker
~/.local/share/tracker
```

After wiping and letting it do a fresh index on my almost new desktop, the total
size of each of these directories was a whopping 3.9 GB!


## Startup Files
--------------

On my Ubuntu GNOME setup, I found the following files:

```
$ ls  /etc/xdg/autostart/tracker-*
/etc/xdg/autostart/tracker-extract.desktop
/etc/xdg/autostart/tracker-miner-fs.desktop
/etc/xdg/autostart/tracker-store.desktop
/etc/xdg/autostart/tracker-miner-apps.desktop
/etc/xdg/autostart/tracker-miner-user-guides.desktop
```

You can disable these by adding `Hidden=true` to them. It's best done in your
local `.config` directory because 1) you don't need sudo and 2) you are pretty
much guaranteed that your changes won't be blown away by an update.


## The `tracker` Binary
---------------------

Running `tracker` will give you a vast array of tools to check on tracker and
manage its processes.

```
$ tracker
usage: tracker [--version] [--help]
               <command> [<args>]

Available tracker commands are:
   daemon    Start, stop, pause and list processes responsible for indexing content
   info      Show information known about local files or items indexed
   index     Backup, restore, import and (re)index by MIME type or file name
   reset     Reset or remove index and revert configurations to defaults
   search    Search for content indexed or show content by type
   sparql    Query and update the index using SPARQL or search, list and tree the ontology
   sql       Query the database at the lowest level using SQL
   status    Show the indexing progress, content statistics and index state
   tag       Create, list or delete tags for indexed content

See 'tracker help <command>' to read about a specific subcommand.
```


## Non-Invasive Disable Cheat Sheet
---------------------------------

This disables everything but `tracker-store`, which even though it has a
`.desktop` file, seems tenacious and starts up anyway. However, nothing gets
indexed.

```
tracker daemon -t
cd ~/.config/autostart
cp -v /etc/xdg/autostart/tracker-*.desktop ./
for FILE in tracker-*.desktop; do echo Hidden=true >> $FILE; done
rm -rf ~/.cache/tracker ~/.local/share/tracker
```

Note that `tracker daemon -t` is for graceful termination. If you are having
issues terminating processes or just want to take your frustration out,
`tracker daemon -k` immediately kills all processes.

After this is done, `tracker-store` will still start on the next boot. However,
nothing will be indexed. Your disk and CPU will be better for wear.

```
$ tracker status
Currently indexed: 0 files, 0 folders
Remaining space on database partition: 123 GB (78.9%)
All data miners are idle, indexing complete
```


## Other References
-----------------

 * https://wiki.gnome.org/Projects/Tracker/
 * https://wiki.ubuntu.com/Tracker
 * http://standards.freedesktop.org/desktop-entry-spec/latest/ar01s05.html
