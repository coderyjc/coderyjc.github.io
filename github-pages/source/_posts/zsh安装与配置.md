---
title: 'zsh安装与配置'
date: 2022-06-18 01:06:26
tags:
---

## apt安装zsh

### 安装zsh

```
sudo apt install zsh
```

### 切换shell为zsh

```
chsh -s %(which zsh)
```

### 查看当前shell

```
echo $SHELL
```

## 源码安装zsh

### 下载zsh

```
wget https://nchc.dl.sourceforge.net/project/zsh/zsh/5.8/zsh-5.8.tar.xz
```

### 解压并安装

```
解压
tar xvf zsh-5.8.tar.xz
安装
cd zsh-5.8
./configure --prefix=$HOME/.local
make && make install
```

### 配置环境变量

```
vim .bashrc
添加
export PATH=$PATH:$HOME/.local/bin	# 设置环境变量
export SHELL=`which zsh`      		# 设置$SHELL为zsh
exec `which zsh` -l           		# 设置登录为zsh
```

## 配置zsh

### 安装oh-my-zsh

- 从github安装

```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh

zsh install.sh
```

- 从gitee安装

    ```
    1.下载安装文件
    wget https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh
    
    2.修改install.sh文件内容
    找到以下内容
    ----------------------------------------------------------------
    # Default settings
    ZSH=${ZSH:-~/.oh-my-zsh}
    REPO=${REPO:-ohmyzsh/ohmyzsh}
    REMOTE=${REMOTE:-https://github.com/${REPO}.git}
    BRANCH=${BRANCH:-master}
    ----------------------------------------------------------------
    将其中的第3，4行替换为以下内容
    ----------------------------------------------------------------
    REPO=${REPO:-mirrors/oh-my-zsh}
    REMOTE=${REMOTE:-https://gitee.com/${REPO}.git}
    ----------------------------------------------------------------
    3.保存文件，赋予执行权限
    4.执行
    5.修改仓库地址
    cd ~/.oh-my-zsh
    git remote set-url origin https://gitee.com/mirrors/oh-my-zsh.git
    git pull
    6.结束
    ```

### 安装插件

- autojump
- autosuggestioins
- syntax-highlighting

### 安装autjump

```
#.下载插件autojump到/.oh-my-zsh/custom目录中 
git clone https://gitee.com/haha-web/autojump.git $ZSH_CUSTOM/plugins/autojump 

#.到目录autojump中 
cd $ZSH_CUSTOM/plugins/autojump 

#执行install.py 
./install.py
```

在Gitee上直接搜一个就可以了

install完之后会提示在zshrc上添加一段(会提示的)

```
[[ -s /home/jancoyan/.autojump/etc/profile.d/autojump.sh ]] && source /home/jancoyan/.autojump/etc/profile.d/autojump.sh

autoload -U compinit && compinit -u  
```

### 安装autosuggestioins

```
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://gitee.com/phpxxo/zsh-autosuggestions.git ${ZSH_CUSTOM}/plugins/zsh-autosuggestions
```

### 安装syntax-highlighting

```
github
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

gitee
git clone https://gitee.com/wxzxingtian/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### 加载插件

```
$ vim ~/.zshrc
修改以下内容
plugins=(git)为

plugins=(git extract zsh-autosuggestions zsh-syntax-highlighting)

$ source~/.zshrc
```

------

[参考文章: 安装与配置zsh](https://www.wangt.cc//2021/11/安装与配置zsh/)