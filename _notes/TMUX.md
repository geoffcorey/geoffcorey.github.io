---
title: TMUX
tags: bash dotfiles tmux
season: summer
---

## Windows(tabs)

- c create window
- w list windows
- n next window
- p previous window
- f find window
- , name window
- & kill window

## Panes (splits)

- % vertical split
- " horizontal split
- o swap panes
- q show pane numbers
- x kill pane
- `+` break pane into window (e.g. to select text by mouse to copy)
- `-` restore pane from window
- ⍽ space - toggle between layouts
- prefix + q (Show pane numbers, when the numbers show up type the key to goto
  that pane)
- prefix + { (Move the current pane left)
- prefix + } (Move the current pane right)
- prefix + z toggle pane zoom

## Navigation

- prefix + h
- select pane on the left
- prefix + j
- select pane below the current one
- prefix + k
- select pane above
- prefix + l
- select pane on the right

Note: This overrides tmux's default binding for toggling between last active
windows, prefix + l. tmux-sensible gives you a better binding for that,
prefix + a (if your prefix is C-a).

## Resizing panes

- prefix + shift + h
- resize current pane 5 cells to the left
- prefix + shift + j
- resize 5 cells in the up direction
- prefix + shift + k
- resize 5 cells in the down direction
- prefix + shift + l
- resize 5 cells to the right

These mappings are repeatable. The amount of cells to resize can be configured
with @pane_resize option. See configuration section for the details.

## Splitting panes

- prefix + |
- split current pane horizontally
- prefix + -
- split current pane vertically
  Newly created pane always has the same path as the original pane.

## Swapping windows

- prefix + \< - moves current window one position to the left
- prefix + > - moves current window one position to the right

## Copy mode bindings

- y - copy selection to system clipboard
- Y (shift-y) - "put" selection - equivalent to copying a selection, and
  pasting it to the command line
- Alt-y - performs both of the above: copy to system clipboard and put to
  command line

## Search

- prefix + / - regex search (strings work too)

Example search entries:

- foo - searches for string foo
- \[0-9\]+ - regex search for numbers Grep is used for searching. Searches are
  case insensitive.

Predefined searches

- prefix + ctrl-f - simple file search
- prefix + ctrl-g - jumping over git status files (best used after git status command)
- prefix + ctrl-u - url search (http, ftp and git urls)
- prefix + ctrl-d - number search (mnemonic d, as digit)
- prefix + alt-i - ip address search

These start "copycat mode" and jump to first match. "Copycat mode" bindings
These are enabled when you search with copycat:

- n - jumps to the next match
- N - jumps to the previous match

To copy a highlighted match:

- Enter - if you're using Tmux vi mode
- ctrl-w or alt-w - if you're using Tmux emacs mode
- Copying a highlighted match will take you "out" of copycat mode. Paste with
  prefix + \] (this is Tmux default paste).

## TPM

- prefix + Ctrl-s - save
- prefix + Ctrl-r - restore TPM Installation of Plugins
- prefix + Iinstalls new plugins from github or any other git reporefreshes
  TMUX environment
- prefix + U updates plugin(s)
- prefix + g - prompts for session name and switches to it. Performs 'kind-of'
  name completion. Faster than the built-in prefix + s prompt for long session lists.
- prefix + C (shift + c) - prompt for creating a new session by name.
- prefix + S (shift + s) - switches to the last session. The same as built-in
  prefix + L that everyone seems to override with some other binding.
- prefix + @ - promote current pane into a new session. Analogous to how
  prefix + ! breaks current pane to a new window.
  [https://gist.github.com/MohamedAlaa/2961058](https://gist.github.com/MohamedAlaa/2961058)
