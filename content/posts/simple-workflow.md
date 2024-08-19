---
title: "Simple Workflow using Debian and DWM"
date: 2024-07-04T21:21:15+07:00
draft: false
tags: [
  "linux",
  "linux-distro",
  "debian",
  "dwm",
]
---

I have been using Linux for more than a decade, I mostly tweaking my dev environment to gain better productivity.
These are my must have apps for my dev machine: zsh, neovim, alacritty, tmux, sublime, tailscale. 
There is a reason I have two text editors, sublime is really good at opening big files, so I'll keep it until neovim can handle big files.

Ubuntu and Gnome has been always become my first choice of DE, it just works, ui feels intuitive, and looks good.
I used to have 4-5 workspaces that I can switch using Super + number (1...5 assigned to each workspace).
Somehow, if I have a lot of programs running and want them to switch places(one screen with 3 or 4 programs), it becomes cumbersome.
I need to arrange the layout manually.
In addition, if you want to customize your Gnome, you may install additional applets those created using javascript with c binding under the hood.
In the last 8 months, I change my distro and window manager. It is Debian and DWM, they have been amazing so far.
You can check them [here](https://github.com/kru/dotfiles/tree/debian).

![debian-fastfetch](/assets/images/debian.jpg)

I need to set a bash script to automate the installation and setup process.
