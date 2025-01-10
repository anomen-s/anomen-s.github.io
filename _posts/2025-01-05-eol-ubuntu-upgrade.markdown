---
layout: post
title:  "How to upgrade end-of-life Ubuntu Lunar Lobster"
date:   2025-01-05 10:21:48 +0100
categories: linux
---

If you haven't done upgrade of Ubuntu system for some time and you may find outthat automatic upgrade from older release is not supported.

# Upgrade Tool

At first it's not very clear how to proceed, without backuping and reinstalling whole system.
Fortunately there are some guides at [AskUbuntu][askubuntu].

You have to perform upgrade step by step. For each release there is an upgrade tool.
I started with [Mantic][mantic] and was lucky that upgrade from Mantic Minotaur still works.
In future next step would be to continue with upgrade to [Noble][noble].

If the link to upgrade tool doesn't work you can try changing server to one of these:
* http://archive.ubuntu.com
* http://archive.ubuntu.com
 
Also [changelog][changelog] should contain correct links to the Upgrade Tool. 

# APT repositories
Tweaking of hosting server mentioned abouve might be also needed for `apt` repositories.
If you get `404` error when downloading packages, try to add repo from `archive` or `old-releases`.

File to update is `/etc/apt/sources.list`.

[askubuntu]: https://askubuntu.com/questions/1521444/upgrading-an-end-of-life-lunar-installation
[changelog]: https://changelogs.ubuntu.com/meta-release
[mantic]: http://old-releases.ubuntu.com/ubuntu/dists/mantic-updates/main/dist-upgrader-all/current/
[noble]: http://archive.ubuntu.com/ubuntu/dists/noble-updates/main/dist-upgrader-all/current/
