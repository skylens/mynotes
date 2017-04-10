# Linux Tools Configure

### `tmux` 配置

+ 编辑`~/.tmux.conf`文件
```bash
# 更改快捷键前缀
unbind C-b
set-g prefix C-a
# 绑定`R`使新的配置文件立即生效
bind R source-file ~/.tmux.conf;display-message "Config Reloaded..."
# 颜色
set -g status-bg black
set -g status-fg white
# 对齐方式
set-option -g status-justify centre
# 左下角
set-option -g status-left '#[bg=black,fg=green][#[fg=cyan]#S#[fg=green]]'
set-option -g status-left-length 20
# 窗口列表
setw -g automatic-rename on
set-window-option -g window-status-format '#[dim]#I:#[default]#W#[fg=grey,dim]'
set-window-option -g window-status-current-format '#[fg=cyan,bold]#I#[fg=blue]:#[fg=cyan]#W#[fg=dim]'
# 右下角
set -g status-right '#[fg=green][#[fg=cyan]%Y-%m-%d#[fg=green]]'
```

### `vim` 配置

+ 编辑`~/.vimrc`文件
```bash
"高亮显示
syntax on
"设置行号
set number

```
