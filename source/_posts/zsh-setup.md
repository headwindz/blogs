---
title: Mac下最强终端zsh!
date: 2016-12-26
tags: [zsh]
---

## 安装

```bash
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

安装好之后切换到zsh.

```bash
chsh -s /usr/local/bin/zsh
```

## 配置主题

人都是视觉动物。首要任务自然是给zsh挑一个风骚的主题。
我用的主题是[honukai-iterm-zsh](https://github.com/oskarkrawczyk/honukai-iterm-zsh).

## 插件

通过修改.zshrc里面的plugins可以修改或添加插件. 默认加载git.
```bash
plugins=(git)
```

一些牛逼轰轰的插件：

* [autojump](https://github.com/wting/autojump)
* [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
* [sublime](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/sublime) 内置.

<!-- more -->
```bash
# 内置git插件-常用快捷键
alias g='git'

alias ga='git add'
alias gaa='git add --all'

alias gb='git branch'
alias gbd='git branch -d'
alias gbl='git blame -b -w'

alias gcam='git commit -a -m'
alias gcsm='git commit -s -m'
alias gcb='git checkout -b'
alias gpristine='git reset --hard && git clean -dfx'
alias gcm='git checkout master'
alias gcd='git checkout dev'
alias gcmsg='git commit -m'
alias gco='git checkout'
alias gcp='git cherry-pick'
alias gd='git diff'
alias gdca='git diff --cached'
alias gdct='git describe --tags `git rev-list --tags --max-count=1`'
alias ggsup='git branch --set-upstream-to=origin/$(git_current_branch)'
alias gpsup='git push --set-upstream origin $(git_current_branch)'

alias gl='git pull'
alias glg='git log --stat'
alias glgp='git log --stat -p'
alias glgg='git log --graph'
alias glgga='git log --graph --decorate --all'
alias glgm='git log --graph --max-count=10'
alias glo='git log --oneline --decorate'
alias glol="git log --graph --pretty='%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
alias glola="git log --graph --pretty='%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all"
alias glog='git log --oneline --decorate --graph'
alias gloga='git log --oneline --decorate --graph --all'
alias gm='git merge'
alias gp='git push'

alias gr='git remote'
alias gra='git remote add'
alias grb='git rebase'
alias grba='git rebase --abort'
alias grbc='git rebase --continue'
alias grbi='git rebase -i'
alias grbm='git rebase master'
alias grbs='git rebase --skip'
alias grh='git reset HEAD'
alias grhh='git reset HEAD --hard'
alias grmv='git remote rename'
alias grrm='git remote remove'
alias grset='git remote set-url'
alias grt='cd $(git rev-parse --show-toplevel || echo ".")'
alias gru='git reset --'
alias grup='git remote update'
alias grv='git remote -v'

alias gsps='git show --pretty=short --show-signature'
alias gst='git status'
alias gsta='git stash save'
alias gstaa='git stash apply'
alias gstd='git stash drop'
alias gstl='git stash list'
alias gstp='git stash pop'

alias gts='git tag -s'
alias gtv='git tag | sort -V'
```

## 用过都说好！
[我的配置](https://github.com/mzjh/config/tree/master/zsh)