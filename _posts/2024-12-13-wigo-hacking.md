---
layout: post
title:  "Wherigo cartridge hacking"
date:   2024-12-13 17:01:33 +0100
categories: Geocaching Lua Wherigo
---

# Wigo cartridge hacking

This post consists of notes I made when decompiling WIG cartridges.


## Intro
Whereigo cartridges are Geocaching games playable on some types of GPS devices, ancient cell phones with J2ME (OpenWIG application) and Android phones (WhereYouGo application).
Some information in this post come from [here][gcwizard].

## GWC
Cartridge files consists of:

* header
* compiled Lua script
* resources

## LUA
Start of compiled LUA script can be identified by magic bytes "\1bLua".
Use [LUA decompiler][decompiler] to get LUA source code.

What can be seen in decompiled sources might vary depending on tool used to build the cartridge.
Some of the following notes may allpy only to [Urwigo][urwigo] builder.

Coordinates of the zones are usually not encrypted.

Obfuscation function.
Purpose of this function is to hide strings from plain sight when using tools like `strings`, hexviewers and hexeditors and even decompilers.
Implementation of this function is quite simple, it's just performs substitution of ASCII characters using table.
`decrypt.py` script in [repo][repo] can read and decode obfuscated strings. It's Python tool. You first have to find substitution table and convert it to Python string.

`Hash` function. This function is used to verify answer.
Only hash of correct answer is stored in the cartridge and handler which handles user input computes hash of provided answer and compares it with correct value.
Finding correct answer depends on range of possible values. Finding number consisting of few digits will be fast.  Long text string will be more difficult.


[gcwizard]: https://blog.gcwizard.net/manual/en/wherigo-tools-encryption-and-codes/00-wherigo-cartridges-a-look-behind-the-scenes/
[decompiler] : https://luadec.metaworm.site/
[repo]: https://github.com/anomen-s/programming-challenges/tree/master/geocaching.com/Wherigo
[urwigo]: https://www.urwigo.cz/
