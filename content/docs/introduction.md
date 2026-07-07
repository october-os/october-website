+++
title = 'Introduction'
weight = 1
+++

October Linux is an Arch-based Linux distribution. 
It provides a fast installer written in Go that takes a JSON configuration as input to deploy a full system.
While heavily dependent on network operations for installing packages, October Linux can be installed in the 3 to 15 minutes range,
depending on how many extra packages you want to install.

For reference, my laptop installation was ready in 6 minutes while my desktop took 10 minutes.
The time difference was mostly dependent on network connectivity (Wi-Fi on the laptop vs. ethernet on the desktop) and number of extra packages (more on the desktop).

A default pre-configured desktop environment composed of Hyprland, a Quickshell bar and the Wofi app launcher is installed, along with other base packages.
The full base packages list is available [here](https://github.com/october-os/october-iso/tree/main/docs/packages).

A nice perk is that the desktop environment's colors automatically adapt to your wallpaper:
![](img/screenshot.png)
![](img/screenshot2.png)