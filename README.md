# tmux-sessionizer (tms)

The fastest way to manage projects as tmux sessions

## What is tmux-sessionizer?

Tmux Sessionizer is a tmux session manager that is based on ThePrimeagen's
[tmux-sessionizer](https://github.com/ThePrimeagen/.dotfiles/blob/master/bin/.local/scripts/tmux-sessionizer)
but is opinionated and personalized to my specific tmux workflow. And it's awesome. Git worktrees
are automatically opened as new windows, specific directories can be excluded, a default session can
be set, killing a session jumps you to a default, and it is a goal to handle more edge case
scenarios.

Tmux has keybindings built-in to allow you to switch between sessions. By default these are
`leader-(` and `leader-)`

Switching between windows is done by default with `leader-p` and `leader-n`

![tms-gif](images/tms-v0_1_1.gif)

## Usage

### The `tms` command

Running `tms` will find the repos and fuzzy find on them. It is very conveneint to bind the tms
command to a tmux keybinding so that you don't have to leave your text editor to open a new project.
I have this tmux binding `bind C-o display-popup -E "tms"`. See the image below for what this look
like with the `tms switch` keybinding

### The `tms switch` command

There is also the `tms switch` command that will show other active sessions with a fuzzy finder and
a preview window. This can be very useful when used with the tmux `display-popup` which can open a
popup window above the current session. That popup window with a command can have a keybinding. The
config could look like this `bind C-j display-popup -E "tms switch"`. Then when using leader+C-j the
popup is displayed (and it's fast)

![tms-switch](images/tms_switch-v2_1.png)

Use `tms --help`

```
Scan for all git folders in specified directories, select one and open it as a new tmux session

Usage: tms [COMMAND]

Commands:
  config    Configure the defaults for search paths and excluded directories
  start     Initialize tmux with the default sessions
  switch    Display other sessions with a fuzzy finder and a preview window
  kill      Kill the current tmux session and jump to another
  sessions  Show running tmux sessions with asterisk on the current session
  help      Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

### Configuring defaults

```
Configure the defaults for search paths and excluded directories

Usage: tms config [OPTIONS]

Options:
  -p, --paths <search paths>...      The paths to search through. Shell like expansions such as `~` are supported
  -s, --session <default session>    The default session to switch to (if avaliable) when killing another session
      --excluded <excluded dirs>...  As many directory names as desired to not be searched over
      --remove <remove dir>...       As many directory names to be removed from the exclusion list
      --full-path <true> <false>     Use the full path when displaying directories [possible values: true, false]
  -h, --help                         Print help

```

## Installation

### Cargo

Install with `cargo install tmux-sessionizer` or

### From source

Clone the repository and install using `cargo install --path . --force`

## Usage Notes

The 'tms sessions' command can be used to get a styled output of the active sessions with an
asterisk on the current session. The configuration would look something like this

```
set -g status-right " #(tms sessions)"
```

E.g. ![tmux status bar](images/tmux-status-bar.png) If this configuration is used it can be helpful
to rebind the default tmux keys for switching sessions so that the status bar is refreshed on every
session switch. This can be configured with settings like this.

```
bind -r '(' switch-client -p\; refresh-client -S
bind -r ')' switch-client -n\; refresh-client -S
```