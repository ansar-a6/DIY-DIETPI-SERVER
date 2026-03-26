# Project: DIY Linux Server with Dietpi
## Introduction
This is a small home server which is operated by the DietPi operating system powered by a reused damaged laptop. First my goal was to switch from windows 10 which was running a server for dns and file sharing. So choosing a linux distro and started to research on it, later it expanded to from smba and dns to gitea and python bots.

## Goal
Replacing Windows 10 (using as a server) to another OS which is light weight and doesn't waste resources like ram memory, cpu cycles and hdds life.
## Operating system planning
### Available Physical Hardware

Storage: Sata1 (Hdd 320gb) SlimSata2 (Hdd 320gb - Caddy)
Connection ports: Hdmi, usb ports * 3
Cpu: Pentium N3540 2.14Ghz - 4 cores
GPU: Igpu (unknown)
RAM: 4gb - ddr3 - single slot
Bios: non-csm

### Decision
Goal: The goal was to select a linux distro which is light weight and uses less ram plus cpu cycles also i had some experience using it.

Three distro were selected for this purpose: ubuntu server, debian and dietpi. By comparing these i had only experience with ubuntu desktop, debian 23.0 and dietpi.

Selected distro: DietPi
Goal: Install DietPi on the usb. The purpose is to extend the life of hdds by releasing os loads.
Installation media: DietPi_NativePc_img

## Services Deployed
### DNS server
Unbond.service which was installed through dietpi software installer and configured manually. see dns_server.md for details
### samba server
samba.service which was installed through dietpi software installer and configured manually. see samba.md for details
### Python 3.11.9 setup
Manually installation was executed due to the spt don't host py 3.11.9 version any more. see python.md for details
### gitea server
gitea.service with sqlite was installed through dietpi software installer and configured later. see gitea.md for details