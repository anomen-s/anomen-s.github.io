---
layout: post
title:  "Histedit in TortoiseHG"
date:   2019-03-10 11:22:33 +0100
categories: mercurial
---

If you are regular user of Mercurial, you probably know [TortoiseHg][thg].
And if you also discovered [Histedit][histedit] extension,
you may have wondered if it is possible to use Histedit from TortoiseHg.

And the answer is: yes, but....

As can be found in Issue tracking, there is still [open request][issue2533].
There you can find an advice to configure it as custom tool.
Simple command `hg histedit {REV}` works well in Windows, but in case of Linux there is some more tweaking needed.


First problem is that for some reason (issue?), output is not visible in the Console (no problem in Windows version).
And second problem is wrong editor - same editor is used as for `hg` in command line.
To deal with these issues, final command I use is as follows:

```
 EDITOR=/usr/bin/leafpad hg histedit {REV} 2> /dev/stdout | xmessage -file -
```

[thg]: https://www.mercurial-scm.org/wiki/TortoiseHg
[histedit]: https://www.mercurial-scm.org/wiki/HisteditExtension
[issue2533]: https://bitbucket.org/tortoisehg/thg/issues/2533/access-histedit-extension-from-the
