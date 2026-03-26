# DIY-DietPi-server

Project: DIY Linux Server with Dietpi
Introduction
This is a small home server which is operated by the DietPi operating system powered by a reused damaged laptop. First my goal was to switch from windows 10 which was running a server for dns and file sharing. So choosing a linux distro and started to research on it, later it expanded to from smba and dns to gitea and python bots.
Technical Environment

Component
Specification
Os
DietPi (Debian-based lightweight distribution)
Hardware
Repurposed Laptop (Hp: Pentium N3540)
Storage
Usb 1 (32gb)
Hdd 1 (320gb): smba (ntfx format).
Hdd 2 (320gb), exfat multipurpose.

Goal
Replacing Windows 10 (using as a server) to another OS which is light weight and doesn't waste resources like ram memory, cpu cycles and hdds life.
Operating system planning
Available Physical Hardware

Components
Information
Storage
Sata1: Hdd 320gb 
SlimSata2: Hdd 320gb (Caddy)
Connection ports
Hdmi, usb ports * 3
Cpu
Pentium N3540 2.14Ghz - 4 cores
GPU
Igpu (unknown)
ram
4gb - ddr3 - single slot
Bios
non-csm

Decision
Goal: The goal was to select a linux distro which is light weight and uses less ram plus cpu cycles also i had some experience using it.
Three distro were selected for this purpose: ubuntu server, debian and dietpi. By comparing these i had only experience with ubuntu desktop, debian 23.0 and dietpi.

Distro
Pros and cons
Comments
Ubuntu server


2gb installation media file plus too big for a single client person and for a laptop.
Debain


Light weight but needs some maintenance. Old libraries by default which exclude latest stability.
DietPi


Perfect for servers, light weight. I had more experience with ubuntu and debian. It can run on usb.


Selected distro: DietPi
Installation media: DietPi_NativePc_img
Goal: Install DietPi on the usb. The purpose is to extend the life of hdds by releasing os loads.
Dns server (unbond service)
Goal: Change form Acrylic DNS proxy window based server to linux developed dns server.
The unbound service which is dietpi community based bns server which has more benefits and control on dns server.
File Server (samba.service)
Goal: Share file server same as in windows 10.
Python bots integration
Goal: create environments for game static bot and run them.
GitHub (gitea.service)
After doing some exploration I found out that you can host your own GitHub on your home server. For this purpose I chose gitea service with sqlite which became extremely light weight.
