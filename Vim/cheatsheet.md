# Vim Cheatsheet

- [Basics](#basics)
- [Movement (Normal Mode)](#movement-normal-mode)
- [Operation (Normal Mode)](#operation-normal-mode)
- [Insert Mode](#insert-mode)
- [Visual Mode](#visual-mode)
- [Misc](#misc)

## Basics
| What                          | Command                    |
| :---------------------------- | :------------------------- |
| Start `vim`                   | `$ vim`                    |
| Open file `name`              | `$ vim name`               |
| Open file `name` in read-only | `$ vim -R name`            |
| Show diff                     | `$ vim -d file1 file2 ...` |

## Movement (Normal Mode)
| What                | Key         |
| :------------------ | :---------- |
| Cursor left         | `h`         |
| Cursor down         | `j`         |
| Cursor up           | `k`         |
| Cursor right        | `l`         |
| Up one line         | `C-y`       |
| Down one line       | `C-e`       |
| Up a chunk          | `C-u`       |
| Down a chunk        | `C-d`       |
| Up full screen      | `C-b`       |
| Down full screen    | `C-f`       |
| Screen to middle    | `zz`        |
| Screen to top       | `zt`        |
| Screen to bottom    | `zb`        |
| To next word        | `w`         |
| To previous word    | `b`         |
| To end of line      | `$`         |
| To start of line    | `0`         |
| To paragraph start  | `{`         |
| To paragraph end    | `}`         |
| To top of screen    | `H`         |
| To middle of screen | `M`         |
| To bottom of screen | `L`         |
| To middle of file   | `50%`       |
| To start of file    | `gg`        |
| To end of file      | `G`         |
| To left split       | `C-w Left`  |
| To right split      | `C-w Right` |
| To top split        | `C-w Up`    |
| To bottom split     | `C-w Down`  |

## Operation (Normal Mode)
| What                                      | Key/Command |
| :---------------------------------------- | :---------- |
| Force quit                                | `:q!`       |
| Save quit                                 | `:wq`       |
| Yank and delete current line              | `dd`        |
| Yank and delete current line after cursor | `D`         |
| Yank current line                         | `yy`        |
| Paste from yanked buffer                  | `p`         |
| Search `regex` downwards                  | `/regex`    |
| Search `?regex` upwards                   | `?regex`    |
| Search for exact match of `word`          | `/\<word\>` |
| To next match                             | `n`         |
| To previous match                         | `N`         |
| Record macro and save it in `a`           | `qa`        |
| Stop recording macro                      | `q`         |
| Run macro in `a`                          | `@a`        |

## Insert Mode
| What                                         | Key   |
| :------------------------------------------- | :---- |
| Enter insert mode at cursor                  | `i`   |
| Jump to start of line and ender insert mode  | `I`   |
| Jump to next character and enter insert mode | `a`   |
| Jump to end of line and enter insert mode    | `A`   |
| Enter normal mode temporary                  | `C-o` |

## Visual Mode
| What                      | Key   |
| :------------------------ | :---- |
| Enter visual mode         | `v`   |
| Enter line visual mode    | `V`   |
| Enter block visual mode   | `C-v` |
| Yank and delete selection | `d`   |
| Yank selection            | `y`   |

## Misc
| What                    | Key/Command    |
| :---------------------- | :------------- |
| Clear all highlights    | `:noh`         |
| Go to line number `num` | `:num`         |
| Split vertical          | `:Vexplore`    |
| Split horizontal        | `:Sexplore`    |
| Next file in buffer     | `:bn`          |
| Previous file in buffer | `:bp`          |
| Enter paste mode        | `:set paste`   |
| Exit paste mode         | `:set nopaste` |
| Increment number        | `C-a`          |
| Decrement number        | `C-x`          |
