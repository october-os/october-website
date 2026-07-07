+++
title = 'Configuration'
weight = 4
+++

October Linux ships with a default pre-configured desktop environment composed of Hyprland, a Quickshell bar and the Wofi app launcher.

The configuration lives within the `~/.config/october-config/` directory. You can change anything you want in this directory,
but it might get overriden when you update. However, some subdirectories are designed to let you add on to the configuration, safe from updates.

## Configuring Hyprland

A base configuration lives in the `hypr/base/` directory, but you can add your own configuration files in the `hypr/user` directory to modify it.
Any files in this directory are automatically loaded into Hyprland on top of the base configuration files.

## Adding wallpapers

You can add your wallpapers in the `wallpapers/` directory. [octoberctl](docs/usage/#octoberctl) can help you manage this with a command.

## Setting a profile picture

The profile picture for the lockscreen is taken from `profile_picture.jpg`: override it with any image you'd like to make it your profile picture.
[octoberctl](docs/usage/#octoberctl) can help you manage this with a command.

## Updating the configuration

The October Linux configuration files are kept up-to-date by us. To update it with the latest changes from GitHub, use the `octoberctl update` command.