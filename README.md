Here is a single-file, batteries-included `~/.tmux.conf` aimed at heavy daily use (fast, vim-friendly, good defaults, plugin-ready).[1][2][3]

```tmux
##### Basics #####

# Use 256-color and modern terminal features
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",xterm-256color:RGB"

# Start window/pane numbering at 1 (muscle memory friendly)
set -g base-index 1
set -g pane-base-index 1

# Increase scrollback history
set -g history-limit 50000

# Faster key handling
set -sg escape-time 10

# Better messages
set -g display-time 2000
set -g display-panes-time 2000

# Mouse support (resize, select panes, copy)
set -g mouse on


##### Prefix & Essentials #####

# Easier prefix: C-a (like screen)
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# Reload config
unbind r
bind r source-file ~/.tmux.conf \; display-message "Reloaded ~/.tmux.conf"

# Quick detach
bind D detach


##### Status Line & Appearance #####

# Refresh status less often for performance
set -g status-interval 5

# Basic styling (tune to taste)
set -g status on
set -g status-justify centre
set -g status-bg colour234
set -g status-fg colour245

set -g message-style "bg=colour235,fg=colour223"
set -g pane-border-style "fg=colour238"
set -g pane-active-border-style "fg=colour39"

# Left side: session and window
set -g status-left-length 40
set -g status-left "#[fg=colour39,bold] #S #[default]"

# Right side: time, host
set -g status-right-length 80
set -g status-right "#[fg=colour244]#H #[fg=colour39]%Y-%m-%d %H:%M #[default]"

# Window title formats
setw -g automatic-rename on
setw -g monitor-activity on
set -g visual-activity off


##### Splits, Panes, Windows #####

# More intuitive splits
unbind '"'
unbind %
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"

# Navigate panes with vim keys
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Resize panes with shift + vim keys
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# Toggle last active pane
bind Space last-pane

# Swap panes
bind < swap-pane -U
bind > swap-pane -D

# New window in same dir
bind c new-window -c "#{pane_current_path}"

# Quickly kill pane / window
bind x kill-pane
bind & kill-window


##### Copy Mode (vim-like) #####

setw -g mode-keys vi

# Enter copy mode
bind -T prefix [ copy-mode

# Use vim keys in copy mode
bind -T copy-mode-vi v send -X begin-selection
bind -T copy-mode-vi y send -X copy-selection-and-cancel
bind -T copy-mode-vi r send -X rectangle-toggle

# System clipboard integration (requires xclip, pbcopy, or wl-copy)
# Linux (xclip example):
if-shell "command -v xclip >/dev/null 2>&1" \
    "bind -T copy-mode-vi y send -X copy-pipe-and-cancel 'xclip -sel clip -i'"

# macOS (pbcopy example):
if-shell "command -v pbcopy >/dev/null 2>&1" \
    "bind -T copy-mode-vi y send -X copy-pipe-and-cancel 'pbcopy'"


##### Quality-of-life #####

# Remap pane switching to Alt+arrow (no prefix)
bind -n M-Left  select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up    select-pane -U
bind -n M-Down  select-pane -D

# Quick clear
bind C-l send-keys C-l \; run-shell "tmux clear-history"

# Toggle synchronize-panes
bind S setw synchronize-panes

# Zoom pane
bind z resize-pane -Z


##### Performance-Tuned Options #####

# Avoid unnecessary redraws
set -g aggressive-resize on

# Quieter terminal
set -g bell-action none

# Enable focus-events for modern terminals / editors
set -g focus-events on


##### TPM (tmux plugin manager) #####
# Requires: git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'

# Auto-save/restore sessions
set -g @continuum-restore 'on'
set -g @continuum-boot 'on'

# Initialize TPM (keep this last)
run -b '~/.tmux/plugins/tpm/tpm'
```

## How to use this

- Save as `~/.tmux.conf`, then reload inside tmux with `Prefix (Ctrl-a)`, then `r`.[4][3]
- Install TPM once: `git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`, then press `Prefix + I` to install plugins.[3][5]

If you share your editor (vim/neovim) keybinds or want bank-friendly tweaks (e.g., no auto-resurrect on certain hosts), this can be tailored further.

[1](https://hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/)
[2](https://www.linkedin.com/pulse/complete-tmux-configuration-guide-modern-developers-2025-sadiq-ali-9x6dc)
[3](https://thevaluable.dev/tmux-config-mouseless/)
[4](https://www.hostinger.com/tutorials/tmux-config)
[5](https://github.com/rothgar/awesome-tmux)
[6](https://dev.to/krishnam/tmux-13-cool-tweaks-to-make-it-personal-and-powerful-487p)
[7](https://github.com/tmux/tmux/wiki/Advanced-Use)
[8](https://www.youtube.com/watch?v=jaI3Hcw-ZaA)
[9](https://stackoverflow.com/questions/10966323/how-to-make-tmuxs-reaction-faster)
[10](https://builtin.com/articles/tmux-config)
[11](https://github.com/tmux/tmux/issues/2551)
[12](https://www.codementor.io/tmux/tutorial/tmux-tutorial-advanced-tips-tricks)
[13](https://www.reddit.com/r/linux/comments/5ovfgz/tmux_really_slows_down_the_terminal_performance_a/)
[14](https://www.reddit.com/r/commandline/comments/1erhre8/practical_tmux_a_howto_guide_beyond_the_basics/)
[15](https://www.reddit.com/r/vim/comments/10in7wl/a_beautiful_tmux_setup_in_3_minutes/)