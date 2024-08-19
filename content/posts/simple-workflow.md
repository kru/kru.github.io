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

![debian-fastfetch](https://private-user-images.githubusercontent.com/11352023/359104621-ac144c2b-6d29-4cf1-90e3-74c0d37c7152.jpg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjQwNzE4MDYsIm5iZiI6MTcyNDA3MTUwNiwicGF0aCI6Ii8xMTM1MjAyMy8zNTkxMDQ2MjEtYWMxNDRjMmItNmQyOS00Y2YxLTkwZTMtNzRjMGQzN2M3MTUyLmpwZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MTklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODE5VDEyNDUwNlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTMyZTdmZGY4MzQ5NTEyNjUzMDI2ZTk1ZjZiZTFmNmJkYWM5Y2M5ZTgxODJmYWYwMGNhMzY2MzM5MWYzMGVkODQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.falvFvmAk8U2P4CVKsvOCleNfq3VI8o8D0dshOsUMng)

I need to set a bash script to automate the installation and setup process.
