---
layout: post
title: "Spice up your Gnome"
categories: tips, linux, gnome, gui, bash
date: "30-12-2020"
---

I use Ubuntu 20.04 on most of my machines. It ships Gnome by default, and I'm pretty used to it.
However, the default backgrounds in Ubuntu 20.04 are quite boring to say the least.

I tried finding an app that automatically changes backgrounds for me, but most seemed bloated, and had to run constantly in the background, consuming resources.

So I did it with a cronjob, using a locally stored image library.

## Background changer script

```bash
#!/bin/bash

IMG_LIBRARY='<PATH TO YOUR IMAGE LIBRARY HERE>'

# Show all files in a single column, then randomly select one of them
PIC=$(ls -w 1 $IMG_LIBRARY/* | shuf -n 1) 

# Fetch the Gnome process PID
PID=$(pgrep -f 'gnome-session' | head -n 1) 

# Set the background
DBUS_SESSION_BUS_ADDRESS=$(grep -z DBUS_SESSION_BUS_ADDRESS /proc/$PID/environ | cut -d= -f2-) gsettings set org.gnome.desktop.background picture-uri file://$PIC
```


## crontab

Add this line to your crontab to run the script every 15 minutes.

```bash
*/15 * * * * bash <ABSOLUTE PATH TO YOUR SCRIPT>
```
