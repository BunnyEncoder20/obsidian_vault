# Tmux: Terminal multiplexer
- Introduction [Video]((https://www.youtube.com/watch?v=nTqu6w2wc68))
- [Tmux Wiki](https://github.com/tmux/tmux/wiki/Getting-Started)
- Tmux Cheat Sheet / Quick Ref: [Link](https://tmuxcheatsheet.com/)
- it is kind of terminal virtualization
- Tmux has 3 layers:
    1. Session (name on the bottom left)
    2. Window (the thing after the session name, also has the terminal we're using [default is bash])
    3. Panes (when we split the window)


## Configuring My Tmux
- I will be following this [youtube tutorial](https://www.youtube.com/watch?v=wpODsyBHxH0&t=87s).

**Note** that the Tmix config by convention should be in the `~/.config/tmux/tmux.conf`

### Tmux Custom Keybindings Reference

This reference documents the custom keybindings defined in your `tmux.conf` file, using the default prefix (`C-b` or `C-a`, depending on your setup) unless the binding is marked as **(No Prefix)**.

#### Pane Selection and Movement

- These bindings allow you to quickly switch between open panes.

| **Key**        | **Binding Type** | **Action**       | **Description**                                  |
| -------------- | ---------------- | ---------------- | ------------------------------------------------ |
| **Prefix + w** | Custom           | `select-pane -U` | Move to the pane **Up**                          |
| **Prefix + a** | Custom           | `select-pane -L` | Move to the pane **Left**                        |
| **Prefix + s** | Custom           | `select-pane -D` | Move to the pane **Down**                        |
| **Prefix + d** | Custom           | `select-pane -R` | Move to the pane **Right**                       |
| **Prefix + ↑** | Override         | `select-pane -U` | Move to the pane **Up** (Disables key repeat)    |
| **Prefix + ↓** | Override         | `select-pane -D` | Move to the pane **Down** (Disables key repeat)  |
| **Prefix + ←** | Override         | `select-pane -L` | Move to the pane **Left** (Disables key repeat)  |
| **Prefix + →** | Override         | `select-pane -R` | Move to the pane **Right** (Disables key repeat) |
| **Alt + w**    | **(No Prefix)**  | `select-pane -U` | Move to the pane **Up**                          |
| **Alt + a**    | **(No Prefix)**  | `select-pane -L` | Move to the pane **Left**                        |
| **Alt + s**    | **(No Prefix)**  | `select-pane -D` | Move to the pane **Down**                        |
| **Alt + d**    | **(No Prefix)**  | `select-pane -R` | Move to the pane **Right**                       |

#### Pane Splitting

These bindings create new horizontal or vertical panes.

|   |   |   |   |
|---|---|---|---|
|**Key**|**Binding Type**|**Action**|**Description**|
|**Prefix + h**|Custom|`split-window -h`|Split the current pane **Horizontally**|
|**Prefix + \|**|Custom|`split-window -h`|Split the current pane **Horizontally**|
|**Prefix + v**|Custom|`split-window -v`|Split the current pane **Vertically**|
|**Prefix + -**|Custom|`split-window -v`|Split the current pane **Vertically**|

#### Pane Resizing and Utility

These bindings control the size and state of the current pane.

|   |   |   |   |
|---|---|---|---|
|**Key**|**Binding Type**|**Action**|**Description**|
|**Prefix + W**|Repeatable|`resize-pane -U`|Resize pane **Up** (Hold key to repeat)|
|**Prefix + A**|Repeatable|`resize-pane -L`|Resize pane **Left** (Hold key to repeat)|
|**Prefix + S**|Repeatable|`resize-pane -D`|Resize pane **Down** (Hold key to repeat)|
|**Prefix + D**|Repeatable|`resize-pane -R`|Resize pane **Right** (Hold key to repeat)|
|**Prefix + f**|Custom|`resize-pane -Z`|**Toggle Pane Zoom** (Maximize or restore current pane)|
|**Alt + f**|**(No Prefix)**|`resize-pane -Z`|**Toggle Pane Zoom** (Maximize or restore current pane)|
|**Prefix + q**|Custom|`detach-client`|Detach the current client from the session|
|**Prefix + e**|Custom|`choose-window -Z`|Choose a window from a list and automatically **Zoom** into it|

#### Window Management

- These bindings switch between and select windows (tabs).

|                        |                  |                         |                                                    |
| ---------------------- | ---------------- | ----------------------- | -------------------------------------------------- |
| **Key**                | **Binding Type** | **Action**              | **Description**                                    |
| **Prefix + C-t**       | Custom           | `new window`            | Make a **new window**                              |
| **Prefix + C-w**       | Custom           | `close/kill window`     | **closes/kills** current window                    |
| **Ctrl + Tab**         | **(No Prefix)**  | `next-window`           | Go to the **Next Window**                          |
| **Ctrl + Shift + Tab** | **(No Prefix)**  | `previous-window`       | Go to the **Previous Window**                      |
| **Alt + 1 to 9**       | **(No Prefix)**  | `select-window -t :[#]` | Immediately select the corresponding window number |

---

## Tmux Default Commands
##### Session Commands
- To create a session and attach to it
```bash
tmux
```
- To look at all sessions
```bash
tmux ls
```
- To attach to session:
```bash
tmux a  # attaches to most recetly used session
tmux a -t <session index>
tmux a -t <session name>
```
- To detach from a session:
```bash
>> ctrl - B
>> D
```
- To create a named session:
```bash
tmux new -s bob  # name is at the bottom left of the session window
```
- To kill a session:
```bash
tmux kill-session  # kills most recent session
tmux kill-session -t <session index/name>
```
- To kill all sessions at once
```bash
tmux kill-server
```

---

##### Window Commands
- To make a new window (current one marked with '*'):
```bash
>> ctrl + B
>> C
```
- To cycle through all the windows:
```bash
>> ctrl + B
>> N
```
- To Navigate between sessions and windows:
```bash
>> ctrl + B
>> W
```
- To Kill a window:
```bash
>> ctrl + B
>> W
# Be on the window you want to kill
>> ctrl + B
>> X
```

---

##### Pane Commands
- To Make a new pane (vertical split)
```bash
>> ctrl + B
>> shift + 5     # (%)
```
- To make a new pane (horizontal split)
```bash
>> ctrl + B
>> shift + '    # (")
```
- To Navigate between panes
```bash
>> ctrl + B
>> arrow keys to navigate
```
- To jump to a specific pane
```bash
>> ctrl + B
>> Q            # will show the index of panels
>> <index number>
```
- To size panes
```bash
>> ctrl + B
>> ctrl / alt (hold)    # which ever size of adjustment
>> <arrow keys>
```

---
---
---

## tmux.conf
- Attached the tmux.conf code here so that can be replicated on my work machine
```bash
# Options
set -sg terminal-overrides ",*:RGB"  # true color support
set -g escape-time 0  # disable delays on escape sequences
set -g mouse on
set -g renumber-windows on  # keep numbering sequential
set -g repeat-time 1000  # increase "prefix-free" window

# Better Prefix key:
unbind C-b
set -g prefix `
bind ` send-prefix

# Indexing starting from 1
set -g base-index 1
set -g pane-base-index 1

# === Look & Feel ===
# Theme: borders
set -g pane-border-lines simple
set -g pane-border-style fg=black,bright
set -g pane-active-border-style fg=magenta

# Theme: status
set -g status-style bg=default,fg=black,bright
set -g status-left ""
set -g status-right "#[fg=green]#S"

# Theme: status (windows)
set -g window-status-format "●"
set -g window-status-current-format "●"

set -g window-status-current-style "#{?window_zoomed_flag,fg=yellow,fg=magenta,nobold}"
set -g window-status-bell-style "fg=red,nobold"

# Keybindings: wasd
bind w select-pane -U
bind a select-pane -L
bind s select-pane -D
bind d select-pane -R

bind -r W resize-pane -U
bind -r A resize-pane -L
bind -r S resize-pane -D
bind -r D resize-pane -R

# Keybindings: disable repeat for arrows
bind Up select-pane -U
bind Left select-pane -L
bind Down select-pane -D
bind Right select-pane -R

# Keybindings: split
bind h split-window -h
bind | split-window -h
bind v split-window -v
bind - split-window -v

# Keybindings: windows
bind C-t new-window    # Prefix + C-t to create a new window
bind C-w kill-window   # Prefix + C-w to close the current window
bind -n C-Tab next-window
bind -n C-S-Tab previous-window

bind -n M-1 select-window -t :1
bind -n M-2 select-window -t :2
bind -n M-3 select-window -t :3
bind -n M-4 select-window -t :4
bind -n M-5 select-window -t :5
bind -n M-6 select-window -t :6
bind -n M-7 select-window -t :7
bind -n M-8 select-window -t :8
bind -n M-9 select-window -t :9

# Keybindings: other
bind f resize-pane -Z
bind q detach-client
bind e choose-window -Z

bind -n M-w select-pane -U
bind -n M-a select-pane -L
bind -n M-s select-pane -D
bind -n M-d select-pane -R
bind -n M-f resize-pane -Z
```