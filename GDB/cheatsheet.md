# GDB Cheatsheet

- [Breakpoint](#breakpoint)
- [Disassemble](#disassemble)
- [TUI Layout](#tui-layout)
- [Misc](#misc)

## Breakpoint
| What                                 | Command            | Shortcut    |
| :----------------------------------- | :----------------- | :---------- |
| List breakpoints                     | `info breakpoints` | `i b`       |
| Set breakpoint (at `0x1111`)         | `break *0x1111`    | `b *0x1111` |
| Set breakpoint (at `main`)           | `break main`       | `b main`    |
| Delete breakpoint (`3`rd breakpoint) | `delete 3`         | `d 3`       |
| Delete breakpoint (all)              | `delete`           | `d`         |

## Disassemble
| What                               | Command                        | Shortcut              |
| :--------------------------------- | :----------------------------- | :-------------------- |
| Disassemble (`main`)               | `disassemble main`             | `disas main`          |
| Disassemble (`0x1111` to `0x2222`) | `disassemble 0x1111,0x2222`    | `disas 0x1111,0x2222` |
| Use intel flavour                  | `set disassembly-flavor intel` | N/A                   |

## TUI Layout
| What                         | Command            | Shortcut     |
| :--------------------------- | :----------------- | :----------- |
| Show ASM window              | `layout asm`       | `la a`       |
| Show register window         | `layout regs`      | `la r`       |
| Show source window           | `layout src`       | `la s`       |
| Set focus to `asm`           | `focus asm`        | `foc a`      |
| List windows                 | `info win`         | `i wi`       |
| Increase `asm` height by `3` | `winheight asm +3` | `win asm +3` |
| Decrease `asm` height by `3` | `winheight asm -3` | `win asm -3` |

## Misc
| What           | Command   | Shortcut |
| :------------- | :-------- | :------- |
| Refresh screen | `refresh` | `ref`    |
