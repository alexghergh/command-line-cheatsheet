# Tmux cheatsheet

Useful keybinds and commands for tmux and plugins can be found here.

--------------------------------------------------------------------------------

## 1. Builtin keybinds

Tmux has a number of builtin commands and keybinds (`man tmux` for a list of
everything that you need to know, strongly suggest going through that). Most of
the tmux keybinds can be accessed through the prefix key (by default mapped to
`Ctrl + b`, but overriden to `Ctrl + a`).

Some of the default keybinds can be found in `man tmux`, under `DEFAULT KEY
BINDINGS`. All these keybinds call commands, that can also be accessed under the
tmux command prompt (accessible by `prefix; :`). Moreover, all the keybinds can
be listed inside tmux using `prefix; ?`. However, these do not display custom
user keybinds.

All the tmux commands can be found in `man tmux` under the section `COMMANDS`.
They can also be listed with `tmux list-commands`.

All the keybinds (tmux and user-defined) can be listed with `tmux list-keys`.

## 2. Custom keybinds

Some of the custom keybinds are (these are preceded  by the prefix key):
- `r`: Source the tmux config file.
- `Ctrl + p`: Previous window.
- `Ctrl + n`: Next window.
- `Ctrl + l`: Last (visited) window.
- `v`: Split vertically.
- `h`: Split horizontally.
- `Alt + h/j/k/l` (without prefix): Pane switching (even inside vim).
- `Alt + ;` (without prefix): Switch to last pane (even inside vim).
- `Ctrl + h`: Open a new window running `htop`.
- `Ctrl + t`: Open a new window running `vim` containing `tmux.conf`.
- `Ctrl + v`: Open a new window running `vim` containing `init.vim`.
- `Ctrl + b`: Open a new window running `vim` containing `.zshrc`.

## 3. Plugins

Additionally, there are a few plugins that map more keybinds. These are
(preceded by prefix unless noted):
- `I`: install tmux plugins listed inside `tmux.conf`.
- `U`: update all tmux plugins.
- `alt + u`: uninstall all tmux plugins.
- `Ctrl + s`: save the tmux server state (all sessions, windows, panes'
  contents).
- `Ctrl + r`: restore the last saved state.
