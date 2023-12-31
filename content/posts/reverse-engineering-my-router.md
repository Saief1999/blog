---
author: "Saief Zneti"
title: "Reverse Engineering My Router"
description: "How I managed to reverse engineer my portable router"
date: 2023-02-12T19:37:51+01:00
draft: true
cover:
    image: "/images/reverse-engineering-my-router/poster.jpg"
    alt: "Router"
    relative: false # To use relative path for cover image, used in hugo Page-bundles 
---

## Motivation

The idea behind this project came from the fact that I overshared my router password. This not only makes the internet connection much slower (with many users connected at all times), but it also drains data, that normally holds me for around a month, in less than a few days.

And since I can't say no to people, the second best solution was to change the password every few days. This was a frustrating task since I had to navigate to the router Dashboard each time. Log in as the admin user, search for the field that needs changing and manually change it. In Addition to that, I'll have to retype the password on all of my devices. So I was wondering: **Is there a way to automate this process?**

This led me to reverse Engineering how my router works, which I will be describing in the next few sections.

## Trying SSH

Some router providers enable network administration using the [SSH](https://en.wikipedia.org/wiki/Secure_Shell) protocol (or sometimes the older [Telnet](https://en.wikipedia.org/wiki/Telnet) protocol). So naturally, I started by looking into that.

For reference, my router is a 4G `Tecno` router. The model that I owned should have an administration server. But the SSH port was **closed**. `Telnet` was enabled but the access was very limited and did not enable me to change the Wifi password.

I grabbed a copy of the firmware that was freely available on the internet. As I presumed, the administration functionalities over SSH and Telnet were there. My internet provider customly disabled most of these functionalities making the process much harder.

One idea that came to mind was to jailbreak my router and gain root privileges. This can potentially break my router and get it out of its warranty, So I left it as a last resort...

## The router web server, how does it work?

The router is exposing a web page on `192.168.1.1` over HTTP. This means that there is a web server, listening for client requests and handling them on the router itself. This also means that, theoretically, if I can *emulate* the exact environment my client requests are coming from, I could possibly *trick* the router into accepting my requests and therefore change the password programmatically. Essentially, I'll be trying to **reverse-engineer the Router API**

## Reverse Engineering the API

First things first, I started sniffing the traffic between my web browser and the router while making requests.

> Show different requests

As it turns out, the router has a private API that, given the right parameters, can give us information about the router (current messages, battery percentage, connected hosts...) in addition to performing multiple administrative tasks. One of which is **Changing the SSID password**.

I looked into the specific request that gets called when the password is changed

> Image of request

uhmmm, The passed parameters seem to be encrypted in some way.

The ... is ....

....

There was no way for me to gather the actual source of these parameters. So started looking into the actual frontend bundle (on the browser). The code was heavily obfuscated and uglified, reading through it was a tedious task. But after a few hours I stumbled upon this:
