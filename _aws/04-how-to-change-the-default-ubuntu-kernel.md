---
title: "How to Change the Default Ubuntu Kernel"
permalink: /aws/changing-default-ubuntu-kernel.html
excerpt: "On AWS"
header:
  overlay_image: /assets/images/aws/04-how-to-change-the-default-ubuntu-kernel/change-ubuntu-kernel.jpg
  teaser: /assets/images/aws/04-how-to-change-the-default-ubuntu-kernel/change-ubuntu-kernel.jpg
  overlay_filter: 0.5
last_modified_at: 2018-08-05T15:59:07-04:00
toc: true
author: Mano
---

These instructions were tested on Ubuntu 16.04 environment.

## Install the Generic Kernel

```bash
sudo apt-get update
sudo apt-get -y install linux-image-extra-virtual
```

## Backup First

sudo cp /etc/default/grub /etc/default/grub.bak

## Show Loaded Kernel

```bash
uname -r

```

#Show Availalbe options
```bash
grep -A100 submenu  /boot/grub/grub.cfg |grep menuentry
```

output

```bash
submenu 'Advanced options for Ubuntu' $menuentry_id_option 'gnulinux-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29' {
    menuentry 'Ubuntu, with Linux 4.4.0-1062-aws' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-4.4.0-1062-aws-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29' {
    menuentry 'Ubuntu, with Linux 4.4.0-1062-aws (recovery mode)' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-4.4.0-1062-aws-recovery-4a67ec61-9cd5-4a26-b00f-9391a34c8a29' {
    menuentry 'Ubuntu, with Linux 4.4.0-1061-aws' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-4.4.0-1061-aws-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29' {
    menuentry 'Ubuntu, with Linux 4.4.0-1061-aws (recovery mode)' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-4.4.0-1061-aws-recovery-4a67ec61-9cd5-4a26-b00f-9391a34c8a29' {
    menuentry 'Ubuntu, with Linux 4.4.0-131-generic' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-4.4.0-131-generic-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29' {
    menuentry 'Ubuntu, with Linux 4.4.0-131-generic (recovery mode)' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-4.4.0-131-generic-recovery-4a67ec61-9cd5-4a26-b00f-9391a34c8a29' {
```

#Create the default menu entry

find the ids of parent and child menu entries. 
For example,
menu entry id for `Advanced options for Ubuntu` is `gnulinux-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29`

menu entry for `Ubuntu, with Linux 4.4.0-131-generic` is `gnulinux-4.4.0-131-generic-recovery-4a67ec61-9cd5-4a26-b00f-9391a34c8a29`

Concat those two strings with `>`. Result would be like

`"gnulinux-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29>gnulinux-4.4.0-131-generic-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29"`

## Edit Grub

`vim /etc/default/grub`

and replace `GRUB_DEFAULT` with above value (With Quotes)

File would look like

```bash
GRUB_DEFAULT="gnulinux-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29>gnulinux-4.4.0-131-generic-advanced-4a67ec61-9cd5-4a26-b00f-9391a34c8a29"
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT=0
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0"
GRUB_CMDLINE_LINUX=""
```


#Update and Reboot

```bash
sudo update-grub
sudo reboot
```

  

