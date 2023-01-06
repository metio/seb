---
title: Proper hostnames in your local network
date: 2022-01-08
menu: topic
categories:
- network
tags:
- hostnames
- rfc
---

Thanks to [RFC 8375](https://www.rfc-editor.org/rfc/rfc8375.html), we now have a proper domain to use for all our local devices. Simply move everything underneath `.home.arpa` in order to join the fun. In case you have `hostnamectl` available on your system run the following command to change the hostname of a device:

```console
# set hostname
$ hostnamectl hostname some-device.home.arpa

# check hostname
$ hostnamectl status
```
