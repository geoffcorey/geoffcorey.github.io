---
id: 244
title: iTerm2 C-h key fixed for VIM and NeoVim
date: 2015-10-12T01:40:13+00:00
author: Geoff Corey
layout: post
guid: http://www.geoffcorey.com/?p=244
permalink: /2015/10/iterm2-c-h-key-fixed-for-vim-and-neovim/
categories:
  - OS/X
  - Uncategorized
---
# Why this broken?

Most VIM folks remap for quicker window pane navigation. So instead of hitting Ctrl-wh to move the cursor to left window pane the Ctrl-h key is remapped to do the same thing.

Ctrl-H is &#8220;ASCII backspace&#8221; but terminals ought to be sending ASCII DEL for the Backspace key instead.  Which means to go to the left pane I had to type C-w-h. Found the above solution in this lengthy thread for <a href="https://github.com/neovim/neovim/issues/2048" target="_blank">NeoVIM Issue #2048</a> and it should work for VIM users as well.

&nbsp;

# iTerm2 C-h key fixed for VIM and NeoVim

This has been driving me nuts this week since I&#8217;m back on OS/X from Linux.   Decided to hunt down a fix tonight and found a very simple solution.

For future reference, this is how to configure iTerm2 to send the correct CSI for `ctrl+h`

**Edit** -> **Preferences** -> **Keys**
  
Press `+`
  
Press `Ctrl+h` as **Keyboard Shortcut**
  
Choose `Send Escape Sequence` as **Action**
  
Type `[104;5u` for **Esc+**

Enjoy.