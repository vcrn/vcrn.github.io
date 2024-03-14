+++
title = "My zsh set-up"
date = "2022-10-29"

[taxonomies]
tags = ["zsh"]
+++

This is a short guide for my zsh set-up. I've saved it here for it to be more accessible to me when setting it up on new computers, 
and in the unlikely event that it would be useful for someone else. In the end, you'll have a quite minimalistic 
interface where you can do fuzzy searching of your command history by pressing `Ctrl+r`.
<!-- more -->

# Installing zsh as your shell

If you don't already have `zsh` installed, then install it. To check what shell you are using, open a terminal and run
```bash
echo $SHELL
```
which on a new system usually points to `bash`, returning something like `/bin/bash`. All the available shells 
are listed in the file `/etc/shells`. Run
```bash
cat /etc/shells
```
and you'll be presented with paths to the available shells. To set `zsh` as your default shell, run
```bash
chsh -s /usr/bin/zsh
```
or whatever path available to `zsh`. Sometimes, you need to enter `chsh` by itself followed by the path 
when prompted. And sometimes, you might need to restart the system for the change to take effect.

# Installing fzf

Run
```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```
Then, you need to append two lines to `~/.zshrc`, both starting with `source` followed by the paths to 
`key-bindings.zsh` and `completion.zsh`, which are located under `fzf/`. The lines to append might look like:
```
source /usr/share/fzf/key-bindings.zsh
source /usr/share/fzf/completion.zsh
```
or 
```
source /usr/share/doc/fzf/examples/key-bindings.zsh
source /usr/share/doc/fzf/examples/completion.zsh
```

# Installing oh-my-zsh

Run
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
in your terminal. Done!

# Installing PowerLevel10k

Optionally, start out by installing the recommended [fonts](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k) to get more capabilities. 

Then, run
```Bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
and set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`. Opening a new terminal
will prompt you to make some choices which can later be changed by running `p10k configure`.
