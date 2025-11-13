# LazyVim

- My Ghosty terminal settings:

```Bash
font-family = SpaceMono Nerd Font Mono
font-size = 15
background-opacity = 0.9
background = #1e1e2e
background-blur-radius = 10
theme = Argonaut
shell-integration-features = no-cursor
cursor-style = block
cursor-style-blink = true
```

## Basic Essentials Keys

| **Category**       | **Action**               | **Key Binding (Normal Mode)**                                  | **VS Code Equivalent**                 |
| ------------------ | ------------------------ | -------------------------------------------------------------- | -------------------------------------- |
| **Find/Replace**   | **Find Files**           | `<leader>ff`                                                   | `Ctrl/Cmd + P`                         |
|                    | **Find Text**            | `<leader>fg`                                                   | `Ctrl/Cmd + Shift + F` (Global Search) |
|                    | **Replace Text**         | `<leader>sr`                                                   | `Ctrl/Cmd + H` (Find and Replace)      |
| **Splits/Windows** | **Split Right**          | `                                                              | `                                      |
|                    | **Split Down**           | `<leader>-`                                                    | Split Editor Down                      |
|                    | **Move Window**          | `<C-h>`, `<C-j>`, `<C-k>`, `<C-l>`                             | Move focus between splits              |
|                    | **Delete Window**        | `<leader>wd`                                                   | Close Split                            |
| **Commenting**     | **Toggle Comment**       | `gc`                                                           | `Ctrl/Cmd + /`                         |
|                    | **Visual Toggle**        | `gcc` (in Normal mode to comment line) or `gc` in Visual mode. |                                        |
| **Code Actions**   | **Code Action**          | `<leader>ca`                                                   | `Ctrl/Cmd + .`                         |
|                    | **Format Code**          | `<leader>cf`                                                   | Format Document                        |
|                    | **Rename Symbol**        | `<leader>cr`                                                   | `F2`                                   |
| **LSP/Go-to**      | **Go to Definition**     | `gd`                                                           | `F12`                                  |
|                    | **Show Hover**           | `K`                                                            | Hover                                  |
|                    | **Code Diagnostics**     | `<leader>cd`                                                   | Show Problems                          |
| **Navigation**     | **Next/Prev Buffer**     | `<S-l>`, `<S-h>`                                               | `Ctrl/Cmd + Tab` (Cycle tabs)          |
|                    | **Toggle File Explorer** | `<leader>e`                                                    | Toggle Sidebar                         |

## Buffers

In **Vim/Neovim**:

- Buffer: The in-memory text of a file. Every file you open gets its own buffer. A buffer can exist even if it is not currently visible in a window.
- Window: A viewport onto a buffer. You can split your editor screen to show multiple windows, each displaying a different buffer (or even the same buffer). This is similar to VS Code's split view.
- Tab (Tab Page): A collection of windows. Similar to tabs in VS Code, but a Vim tab page often contains a complete window layout (splits).

| **Action**             | **Key Binding (Normal Mode)**    | **Description**                                                                                                          |
| ---------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **File Explorer**      | `<leader>e`                      | Toggle the file explorer (like VS Code's sidebar). Use `l` or `<CR>` to open a file/directory, `h` to go up a directory. |
| **Find Files**         | `<leader>ff`                     | Fuzzy find any file in your project (like VS Code's `Ctrl/Cmd + P`).                                                     |
| **New File**           | `<leader>fn`                     | Create a new file.                                                                                                       |
| **Save File**          | `<C-s>`                          | Save the current file/buffer.                                                                                            |
| **Close File**         | `<leader>bd`                     | **Delete/Close the current buffer.** The file remains on disk, but it is removed from memory and the buffer list.        |
| **Switch Buffers**     | `<S-h>` or `[b`                  | Go to the Previous buffer.                                                                                               |
| **Switch Buffers**     | `<S-l>` or `]b`                  | Go to the Next buffer.                                                                                                   |
| **List/Select Buffer** | `<leader>bb` or `<leader>` + `\` | Switch to another buffer via a fuzzy finder.                                                                             |
| **Quit Editor**        | `<leader>qq` or `:qa`            | **Q**uit **a**ll (exits Neovim).                                                                                         |

## Integrated Terminal

| **Action**          | **Key Binding (Normal Mode)** | **Description**                                                                                                              |
| ------------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Toggle Terminal** | `<leader>ft` or `<C-/>`       | **T**erminal (Root Dir) - Opens a terminal in a floating window or a split pane. Press the same key again to hide/toggle it. |
| **Open Terminal**   | `<leader>fT`                  | **T**erminal (cwd) - Opens a terminal in the current working directory.                                                      |
| **Hide Terminal**   | `<C-/>` or `C-\` or `<Esc>`   | (When _in_ the terminal) Closes the terminal and returns to the buffer.                                                      |

## Git Integrations (The Git Panel)

| **Action**           | **Key Binding (Normal Mode)** | **Description**                                                           |
| -------------------- | ----------------------------- | ------------------------------------------------------------------------- |
| **Toggle Git Panel** | `<leader>gg`                  | Open the **Lazygit** UI in a new split window.                            |
| **Show Git Status**  | `gs`                          | Show a pop-up with **G**it **S**tatus (files changed).                    |
| **Stage Hunk**       | `ghs`                         | Stage the **h**unk of code under your cursor.                             |
| **Git Blame**        | `<leader>gb`                  | **G**it **B**lame - Shows who wrote the current line.                     |
| **Git Diff**         | `gd`                          | **G**it **D**iff - Opens a split showing the changes in the current file. |

## Theme

| **Action**             | **Key Binding (Normal Mode)** | **Description**                                                                                             |
| ---------------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Change Colorscheme** | `<leader>uC`                  | Opens an interactive list of installed colorschemes (themes) for you to preview and select.                 |
| **Change UI Options**  | `<leader>u`                   | Opens the **U**I menu, where you can toggle things like Zen Mode, Indent Guides, and other visual settings. |

## Things I Wish I knew earlier 

| **Action**                           | **Key Binding (Normal Mode)**                               | **Description**                                                              |
| ------------------------------------ | ----------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **Quit Vim With saving**             | `<ZZ>`                                                      | Quit vim quickly and saving all work                                         |
| **Go to Line number**                | `<number>G`                                                 | Takes Cursor to that line number line                                        |
| **Go to previous cursor position**   | `<C-o>`                                                     | Takes cursor to the previous position (kinda like a history for your cursor) |
| **Select inside the bracket**        | `vi(` or `vib` <br>`vi{` or `viB`                           | Select the content inside the specified brackets                             |
| **Change Command**                   | `C` <br>combines with all motions. Eg: `cw`,`cb`,`ce`,`ciw` | delete and enter into insert mode                                            |
| **Commets**                          | `gcc` (comment line)<br>`gc` (comment para in Visual Block) | Comments the line directly from Normal-Visual modes                          |
| **mini.surround add surrounding**    | `gsa<brac>` or `gsa<qoutes>`                                | ***Select*** the text and add surrounding bracket or qoutes                  |
| **mini.surround delete surrounding** | `gsd<brac>` or `gsd<qoutes>`                                | Deletes the surrounding bracket or qoutes specified                          |
| **Join to lines**                    | `J`                                                         | Brings the below line to the current line and adds in between both lines     |
