# Radare2 Cheatsheet

- [Basics](#basics)
- [Debugging](#debugging)
- [Debugging (Visual Pane Mode)](#debugging-visual-pane-mode)
- [Disassembly](#disassembly)
- [Graph Mode](#graph-mode)

## Basics
| What                      | Command          |
| :------------------------ | :--------------- |
| `r2` for static analysis  | `$ r2 -A ./bin`  |
| `r2` for debugging        | `$ r2 -Ad ./bin` |
| Analyse binary            | `aaa`            |
| Seek to function          | `s func`         |
| Show binary info          | `ia`             |
| Show current seek address | `s`              |

## Debugging
| What                   | Command                   |
| :--------------------- | :------------------------ |
| Start debug            | `doo`                     |
| Run / continue         | `dc`                      |
| Continue until address | `dcu 0x1111`              |
| List breakpoints       | `db`                      |
| Set breakpoint         | `db 0x1111` / `db func`   |
| Delete breakpoint      | `db -0x1111` / `db -func` |
| Step into              | `ds`                      |
| Step over              | `dso`                     |
| Register info          | `dr`                      |
| Change register value  | `dr eax = 0xffff`         |

## Debugging (Visual Pane Mode)
| What                     | Command                           |
| :----------------------- | :-------------------------------- |
| Enter pane visual mode   | `V!`                              |
| Select/deselect menu bar | `m`                               |
| Command prompt           | `:`                               |
| Step into                | `s` / `F7`                        |
| Step over                | `S` / `F8`                        |
| Insert breakpoint        | `F2`                              |
| Switch pane focus        | `TAB`                             |
| Move inside pane         | `h` / `j` / `k` / `l` / arrow key |
| Close pane               | `X`                               |
| Split pane (vertical)    | `\|`                              |
| Split pane (horizontal)  | `-`                               |
| Show help pane           | `?`                               |
| Quit visual pane mode    | `qq`                              |

## Disassembly
| What                    | Command             |
| :---------------------- | :------------------ |
| Disassemble             | `pd [N]`            |
| Disassemble function    | `pdf` / `pdf @func` |
| List functions          | `afl`               |
| Show function args      | `afa`               |
| Show function summary   | `pdfs`              |
| Show function variables | `afv`               |

## Graph Mode
| What                                | Command    |
| :---------------------------------- | :--------- |
| Enter graph mode                    | `VV`       |
| Enter graph mode and display `main` | `VV @main` |
| Zoom in                             | `+`        |
| Zoom out                            | `-`        |
| Quit graph mode                     | `qq`       |
