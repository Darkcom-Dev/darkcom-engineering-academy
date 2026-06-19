# Basic Editor Ops

Quick reference for common editing operations in terminal text editors. Use this when you just need to remember the keystroke.

## Legend

| Icon | Meaning |
|------|---------|
| `nano` | nano keystroke |
| `vim` | vim keystroke (**Normal Mode** unless noted) |
| `emacs` | emacs keystroke |
| `⚡` | Universal / shell-level shortcut |

## Opening and saving

| Operation | nano | vim | emacs |
|-----------|------|-----|-------|
| Open file | `nano file` | `vim file` | `emacs file` |
| Save | `^O` | `:w` | `C-x C-s` |
| Save As... | `^O` (rename) | `:w newname` | `C-x C-w` |
| Save and quit | `^X` then `Y` | `:wq` or `ZZ` | `C-x C-s` then `C-x C-c` |
| Quit without saving | `^X` then `N` | `:q!` or `ZQ` | `C-x C-c` then `n` |
| Quit (no changes) | `^X` | `:q` | `C-x C-c` |

## Navigation

| Operation | nano | vim | emacs |
|-----------|------|-----|-------|
| Cursor left/down/up/right | arrows | `h j k l` | `C-b C-n C-p C-f` |
| Next / prev word | — | `w b` | `M-f M-b` |
| Beginning / end of line | `^A` `^E` | `0` `$` | `C-a C-e` |
| Beginning / end of file | — | `gg` `G` | `M-<` `M->` |
| Go to line N | `^_` then N | `:N` then Enter | `M-g g` then N |
| Next page / prev page | — | `C-d` `C-u` | `C-v` `M-v` |

## Editing

| Operation | nano | vim | emacs |
|-----------|------|-----|-------|
| Insert text | just type | `i` (enter insert mode) | just type |
| Insert at line end | — | `A` | just type at end |
| New line below | — | `o` | `C-o` |
| Delete character | `Del` | `x` | `C-d` |
| Delete word | — | `dw` | `M-d` |
| Delete line | `^K` | `dd` | `C-a C-k` |
| Delete to end of line | — | `d$` or `D` | `C-k` |
| Undo | `M-U` | `u` | `C-x u` or `C-/` |
| Redo | `M-E` | `C-r` | `C-g` (or `M-x redo`) |

## Copy / Cut / Paste

| Operation | nano | vim | emacs |
|-----------|------|-----|-------|
| Copy (yank) line | — | `yy` | — |
| Copy selected | `M-6` | `y` (visual mode) | `M-w` |
| Cut line | `^K` | `dd` | `C-k` |
| Cut selected | `^K` then `^U` | `d` (visual mode) | `C-w` |
| Paste | `^U` | `p` (after cursor) / `P` (before) | `C-y` |

## Search & Replace

| Operation | nano | vim | emacs |
|-----------|------|-----|-------|
| Search forward | `^W` | `/pattern` | `C-s` |
| Search backward | `^W` then `B` | `?pattern` | `C-r` |
| Next / prev match | `M-W` then `W` | `n` / `N` | `C-s` again / `C-r` again |
| Search & replace | `^\` | `:%s/old/new/g` | `M-%` |
| Replace with confirmation | — | `:%s/old/new/gc` | `M-%` (then y/n) |

## Select (Visual / Mark)

| Operation | nano | vim | emacs |
|-----------|------|-----|-------|
| Start selection | `M-A` | `v` (character) / `V` (line) / `C-v` (block) | `C-Space` |
| Select all | — | `gg V G` | `C-x h` |
| Cut selection | `^K` after mark | `d` | `C-w` |
| Copy selection | `M-6` after mark | `y` | `M-w` |

## Multi-file

| Operation | vim | emacs |
|-----------|-----|-------|
| Open multiple files | `vim f1 f2` | `emacs f1 f2` |
| Next file | `:next` | `C-x C-f` (visit new) |
| Previous file | `:prev` | `C-x <left>` |
| Split horizontal | `:sp file` | `C-x 2` |
| Split vertical | `:vsp file` | `C-x 3` |
| Switch pane | `C-w w` | `C-x o` |
| Close pane | `:q` | `C-x 0` |

## Shell integration

| Operation | vim | emacs |
|-----------|-----|-------|
| Run shell command | `:!command` | `M-! command` |
| Shell from editor | `:shell` | `M-x shell` |
| Insert command output | `:r !command` | `C-u M-! command` |

## Quick reference cards

### vim — survival kit

```text
i       Enter insert mode
Esc     Back to Normal mode
:wq     Save and quit
:q!     Quit without saving
dd      Delete line
yy      Copy line
p       Paste
u       Undo
/foo    Search "foo"
n       Next match
```

### nano — survival kit

```text
^O      Save (WriteOut)
^X      Exit
^K      Cut line
^U      Paste
^W      Search
^\      Search & replace
^G      Help
`_      Go to line
```

### Emacs — survival kit

```text
C-x C-s   Save
C-x C-c   Quit
C-x C-f   Open file
C-s       Search
C-k       Cut to end of line
C-y       Paste (yank)
C-Space   Start selection
M-w       Copy selection
C-w       Cut selection
C-/       Undo
M-x       Run command by name
C-g       Cancel current command
```

## Common shell editing shortcuts

These work in Bash, zsh, and most shells (emacs mode — default):

| Shortcut | Action |
|----------|--------|
| `C-a` | Go to beginning of line |
| `C-e` | Go to end of line |
| `C-u` | Delete from cursor to beginning |
| `C-k` | Delete from cursor to end |
| `C-w` | Delete word backwards |
| `C-l` | Clear screen |
| `C-r` | Search command history |
| `C-d` | Exit shell / delete character |
| `C-z` | Suspend process |
| `C-c` | Interrupt process |
| `M-b` | Back one word |
| `M-f` | Forward one word |
| `M-.` | Insert last argument of previous command |
| `TAB` | Autocomplete |

---

> 💡 **Tip**: Print this page or bookmark it. The first month of using a terminal editor is mostly looking things up. Muscle memory comes with practice.

## Relacionados:
- [[editores-de-texto-en-terminal]] #anterior 
- [[anatomia-de-scripts-de-bash]] #siguiente 