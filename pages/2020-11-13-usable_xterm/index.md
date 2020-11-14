---
layout: post
title: "Usable xterm"
categories: tips, linux, unix, terminal, cli
date: "13-11-2020"
---

As a security engineer, I spend most of my working day in a linux CLI environment. So I'd like to have the fastest & least resource hogging tool for the job.

I did some research, and found two interesting takes on the issue, one on [lwn.net](https://lwn.net/Articles/751763/) and one on [danluu.com](https://danluu.com/term-latency/).

After reading that, I decided to try out xterm, so I ran `xterm`.

![Too bright, too small](images/no_arg_xterm.png)

As you could see, there's a slight size and color issue.

I looked around xterm's man page, and managed to put together the following command to get a usable xterm.

```
xterm \
  -fa 'Monospace' \
  -fs 12 \
  -bg black \
  -fg lightgray \
  -sl 1000000 \
  -aw \
  -cr darkgreen \
  -j +vb +mb \
  -xrm 'XTerm*selectToClipboard: true' \
  -xrm 'xterm*VT100.Translations: #override \ Ctrl Shift <Key>V:    insert-selection(CLIPBOARD) \n\ Ctrl Shift <Key>C:    copy-selection(CLIPBOARD)'
```

![Just right](images/arg_xterm.png)

It works much better now.
