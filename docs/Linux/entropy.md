---
title: Entropy
layout: default
nav_order: 1
parent: "Linux"
---

## Increase the entropy on Linux

Some programs are relying on cryptography libraries and won't start properly if the system has not enough entropy.
The more the OS is used, the bigger the entropy is on the OS. 
For a new VM, it is possible that the entropy is too low for some cryptography libraries to work.

To create more entropy we can install rng-tools. Here are the commands lines on Debian:

```bash
#Check the entropy
cat /proc/sys/kernel/random/entropy_avail

#Generate more entropy with rng-tools
sudo apt-get install rng-tools
sudo vim /etc/default/rng-tools

changed the line:
#HRNGDEVICE=/dev/hwrng
To:
HRNGDEVICE=/dev/urandom

sudo /etc/init.d/rngd start
```