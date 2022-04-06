# Zsh cheatsheet

Useful keybinds and commands for zsh and plugins can be found here.

--------------------------------------------------------------------------------

## 1. Zsh builtin widgets

Zsh has a number of builtin functions (called widgets) that can be accessed
through keybinds. Some of them are mapped by default, while others are
user-bound. A list with all the builtin widgets can be accessed through `zle
-la`, while user widgets (user-defined and shell-defined-for-user) can be
accessed through `zle -lL`. A list of keybinds for the said widgets can be
accessed through `bindkey` (I suggest reading through `man zshzle(1)`, the
sections on `bindkey` and `zle`).

The keybinds in the description below require either a combination of keypresses
to be pressed at the same time, or multiple such combinations one after the
other. `Ctrl + x` means press the _control_ key at the same time as the key _x_,
while `Ctrl + x; y` means press the _control_ key at the same time as the key
_x_, followed by the key _y_ (without pressing _control_ the second time).
Additionally, there may be combinations of the form `Ctrl + x; Ctrl + y`. This
simply means "press the combination _control + x_, followed by _control + y_".
You don't need to let go of the _control_ key for this combination.

Also, the keybinds below are those offered in the `emacs` emulation mode. `vi`
emulation mode offers different keybinds. To enable `emacs` emulation, `bindkey
-e` should be used.

Some of the default shell keybinds (many here present in bash and other shells
as well), are (notice that each one of these keybinds accesses a widget, so the
keybinds could both be remapped to something else, as well as map another
keybind for the specific widget):
- `Ctrl + a`: Go to beginning of line.
- `Ctrl + b`: Go back one character (same as left arrow).
- `Ctrl + c`: (**Note**: This is not a real shell builtin command. Rather, the
  terminal application intercepts this control sequence and sends a _SIGINT_ to
  the shell when this combination of keys is pressed, and the shell acts
  accordingly. This keybind - and others similar - can be changed through
  `stty`. See the man page for more info.) Abort the editing of the current
  line, open a new prompt. If instead a completion menu, a reverse search or
  other such menus are open, abort those and continue editing the current line.
- `Ctrl + d`: Delete one character to the right (same as the delete key on a
  standard keyboard); if instead the cursor is on the last character on the
  line, open the completion menu.
- `Ctrl + e`: Go to the end of line.
- `Ctrl + f`: Go forward one character (same as right arrow).
- `Ctrl + h`: Delete one character to the left (same as backspace).
- `Ctrl + i`: Expand or complete (same as tab).
- `Ctrl + j`: Accept line (same as enter).
- `Ctrl + k`: Kill line (delete everything to the right of the cursor).
- `Ctrl + l`: Clear screen.
- `Ctrl + m`: Accept line (same as enter).
- `Ctrl + n`: Down line or history (same as down arrow). On multi-line commands,
  go through the command down. Otherwise, go down in command history.
- `Ctrl + o`: Accept line and down history. Useful to scroll up in history, and
  initiate a new sequence of commands from there. For example:
    ```zsh
    echo a
    echo b
    echo c
    ```
  After the following commands, going back in history to `echo a` and pressing
  `Ctrl + o` will execute the command, and fetch the next command in history,
  which is `echo b`. Subsequently, pressing `Ctrl + o` again will execute the
  command and fetch `echo c`. Notice that the command `echo a` is (potentially,
  depending on options) now appended in history, and so pressing `Ctrl + o`
  again will bring it on the command line. This creates an infinite loop. Of
  course, `Ctrl + c` shall be executed to abort the operation.
- `Ctrl + p`: Up line or history (same as up arrow). On multi-line commands, go
  through the command up. Otherwise, go up in history.
- `Ctrl + q`: Push the current line onto the editing stack. While writing a
  command, pressing `Ctrl + q` will push that line onto the editing stack, and
  the current command line will be emptied and ready to accept a new command.
  After executing (or aborting) this second command, the first one will be
  written back onto the prompt. The editing stack may accept multiple entries
  (as the name suggests). In this case, pressing `Ctrl + q` on subsequent
  commands pushes them to the stack in a LIFO fashion.

  **Note:** Usually `Ctrl + q` is intercepted by the terminal application, same
  as `Ctrl + s`. These are used for _serial flow control_ (a historical artefact
  left from long ago, more about that [here](https://retrocomputing.stackexchange.com/questions/7263/history-of-ctrl-s-and-ctrl-q-for-flow-control) and [here](https://www.mit.edu/afs.new/athena/system/rhlinux/redhat-6.2-docs/HOWTOS/other-formats/html/Text-Terminal-HOWTO-html/Text-Terminal-HOWTO-10.html)). This means that
  they won't correctly go through to the shell. They can both be disabled via
  `stty -ixon` (which should be added as a command to `.zshrc`), in order for
  the control characters to actually go to the shell. Another workaround is to
  use `Alt + q`, which calls the same widget.
- `Ctrl + r` or `Ctrl + x; r`: Incrementally search backward in history. Opens a
  prompt for input, and goes back through the command line history. Repeated
  presses of the combination cycle through the matches backward.
- `Ctrl + s` or `Ctrl + x; s`: Incrementally search forward in history. Opens a
  prompt for input, and goes forward through the command line history. Repeated
  presses of the combination cycle through the matches forward.

  **Note:** See the note above for `Ctrl + q` for why this might not work. The
  combination `Ctrl + x; s` might be used for the same widget as a workaround.
- `Ctrl + t`: Transpose two characters, the one under the cursor and the one to
  the left, while also moving the cursor 1 position to the right. If the cursor
  is already at the rightmost position on the line, simply transpose the last 2
  characters.
- `Ctrl + u` or `Ctrl + x; u`: Kill the whole line. The shell alternative for
  `Ctrl + c` (see the explanation above for `Ctrl + c`). On multi-line commands,
  only kills the cursor line.
- `Ctrl + v`: Insert the next character typed into the buffer literally. Can be
  used, for example, to insert the newline character literally (`Ctrl + v; enter`)
- `Ctrl + w`: Kill one word backward. Respects the shell variable `WORDCHARS`
  (similar to `Ctrl + backspace` on other GUI applications).
- `Ctrl + x; Ctrl + b`: If on a bracket, go to the matching bracket. Similar to
  the `%` operator in vim.
- `Ctrl + x; Ctrl + f`: Followed by a character, jump to that character (if it
  exists) on the current editing line. Similar to the `f` operator in vim.
- `Ctrl + x; Ctrl + j`: On multi-line commands, join the line below with the
  current one. Doesn't have any effect on the last line of a multi-line command
  or on a command line with just one line. Similar to the `j` operator in vim.
- `Ctrl + x; Ctrl + k`: Kill the whole editing buffer. On a single-line command,
  works the same as `Ctrl + u`. On multi-line commands, kills the whole command.
- `Ctrl + x; Ctrl + o`: Turn on/off overwrite mode (similar to the insert key
  on a standard keyboard).
- `Ctrl + x; Ctrl + u` or `Ctrl + x; u`: Undo the last addition/edit/deletion on
  the command line.

Additionally, there's a set of keybinds that use the `Esc` key as a control
modifier instead of `Ctrl`. Due to how the terminal sends some key codes and the
shell interprets them, the `Esc` key can be shortened to `Alt` in some of the
keybinds below, i.e. instead of pressing `Esc; x` as **two separate** keys, `Alt + x`
can be pressed instead as a single combination. These keybinds are:
- `Alt + '`: Quote the current command line by escaping special characters.
- `Alt + .`: Insert the last argument of the last command. Can be repeated to
  insert the last argument of commands up in history. By prepending a numeric
  argument, bring that argument from the last place into the command line (the
  last one is the first). For example:
    ```zsh
    echo a b c d e f
    ```
  After running the command above, pressing `Alt + .` will bring `f` onto the
  command line, while running `Alt + 4; Alt + .` will bring `c` onto the command
  line.
- `Esc; <` or `Alt + Shift + ,`: Go to the beginning of the global history.
  Pressing down arrow (or similar commands) cycles from that point forward.
- `Esc; ?` or `Alt + Shift + /`: Which command (whence).
- `Alt + a`: Accept the command line, but write it again on the next command
  prompt.
- `Alt + c`: Capitalize the current word.
- `Alt + b`: Backward one word. Respects `$WORDCHARS`.
- `Alt + f`: Forward one word. Respects `$WORDCHARS`.
- `Esc; h` or `Alt + Shift + h`: Run help for the current command.
- `Esc; l`: Down case the word under the cursor. Other combinations might work,
  but currently bound to something else. For example, `Alt + l` is bound to
  _tmux right pane_, while `Alt + Ctrl + l` is bound to logout.
- `Alt + q`: Push line onto editing stack. Same as `Ctrl + q`, see above.
- `Alt + t`: Transpose words.
- `Alt + u`: Up case the word under the cursor.

## 2. Custom widgets and keybinds

All these custom defined widgets can be found in the dotfiles (under
`zsh/.config/zsh/.zsh_other/.zsh_keybinds`).

For custom defined widgets and keybinds, below is a list:
- `Ctrl + x; Ctrl + p`: Search up by prefix, i.e. search backward in history
  commands that start with the current characters on the command line. For
  example, if the current line contains `echo hey`, pressing `Ctrl + x; Ctrl +
  p` will search backward in history only for commands that start with `echo
  hey`. Without any character, acts the same as `Ctrl + p`.
- `Ctrl + x; Ctrl + n`: Search down by prefix. Opposite of the above. Without
  characters, acts as `Ctrl + n`.
- `Alt + e`: Copy the previous word. Useful for long paths, where only the last
  segment would be changed.
- `Alt + z`: Transpose words. Same as `Alt + t`, except it keeps the cursor in
  place.
- `Alt + q` (overriden): Push line _or edit_ (as opposed to only push line). If
  on multi-line commands with the `PS2` prompt, bring the whole multi-line
  command on the prompt. Otherwise, just push line.
- `Ctrl + x; Ctrl + g`: Down line or local history. Depending on options, the
  current command line might share history with other open shells (open in, for
  example, multiple tmux panes or terminal windows). This command only goes down
  through the local history (i.e., only of the current command line, not the
  global history). If on a multi-line command, go down instead (similar to down
  arrow).
- `Ctrl + x; Ctrl + h`: Up line or local history. Opposite of the above.
