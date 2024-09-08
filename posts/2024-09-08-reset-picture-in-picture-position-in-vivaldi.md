---
title: Reset Picture-In-Picture View in Vivaldi
date: 2024-09-08
layout: doc
tags:
- Vivaldi
- Shell Scripting
---

<Title />

I use [Vivaldi Browser](https://vivaldi.com) as my daily driver, and it's mostly great. Just one nitpick. When using a multi-monitor setup and having a video play off the main screen using the Picture-in-Picture functionality (PIP), Vivaldi will store the coordinates and size of the video overlay in its preference file. When disconnecting the monitors the PIP functionality will continue to play the video off-screen, with no way to get it back. The video does show up in macOS' Exposé feature, but there is no way to place it on the screen.

[The issue is known](https://forum.vivaldi.net/topic/94219/reseting-picture-in-picture-position)

This script resets the PIP window's placement:

```bash
#!/bin/bash

# check if Vivaldi is running and close it if it is
if pgrep -x "Vivaldi" > /dev/null; then
    echo "Closing Vivaldi..."
    killall Vivaldi
else
    echo "Vivaldi is not running."
fi

# create backup of Preferences file
cp ~/Library/Application\ Support/Vivaldi/Default/Preferences ~/Library/Application\ Support/Vivaldi/Default/Preferences.bak

# reset PIP window position
jq '.vivaldi.pip_placement.height = 200 | .vivaldi.pip_placement.left = 10 | .vivaldi.pip_placement.top = 10 | .vivaldi.pip_placement.width = 200' ~/Library/Application\ Support/Vivaldi/Default/Preferences.bak > ~/Library/Application\ Support/Vivaldi/Default/Preferences

# remove backup file
rm ~/Library/Application\ Support/Vivaldi/Default/Preferences.bak

# re-open Vivaldi
open -a Vivaldi
````
