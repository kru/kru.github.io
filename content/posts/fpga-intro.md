---
title: "Fpga Intro"
date: 2025-02-27T21:09:48+07:00
tags: [
  "fpga",
  "fpga-intro",
]
---

## Motivation

In the pursue of the understanding fundamental knowledge about how computer works, I spent sometimes where I can start at the lowest level possible but not out of my reach. I looks into FPGA and found it interesting.

## Objective

FPGA is very vast topic, we can approach it from physics all the way to software engineering. So, I need to set some objectives, this way I have good understanding to inner working but not too deep to the level how electrons forms up altogether to build logic gates and its characteristic, let's leave that to Electrical or Physic guys. My objectives are to understand how FPGA works in hardware level and run some programs on it.

## Setup

I bought Xilinx Spartan XC6SL9, Unfortunately, the support software (ISE 14.7) for this is no longer maintained. So, I need to install the development kit in a Linux OS inside a VM. so the following needs to be downloaded before I can start the journey.

1. VMware workstation
2. ISE 14.7
3. Kubuntu 19
4. 

Note: My host OS to run the VM is Linux Mint 21, I create another VM (Debian for my work related stuff) this way I'll not messed up each other as they are on different guest OS.

## Simple Program 