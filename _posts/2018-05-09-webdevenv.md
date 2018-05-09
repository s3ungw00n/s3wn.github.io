---

layout: post
title: 'Setup Web Dev Environment on New Mac OS X'
categories: [dev]
tags: [web]

---

# Setup Web Dev Environment on Mac OS X
 - Django + Node
 - on High Sierra

---
# Bring Tools

- ## [Chrome](https://www.google.co.kr/chrome/index.html)
	- Chrome will not be executed after you clean install High Sierra
	- Type on terminal : `$sudo spctl --master-disable`
 
- ## [Parallels](https://www.parallels.com/products/desktop/download/) # In case you need Windows
	
- ## [Marp](https://yhatt.github.io/marp/) # nice markdown writing tool

---
# Terminal Setup

- ## [iTerm2](https://www.iterm2.com/downloads.html) # for more configurable

- ## [Oh My Zsh](http://ohmyz.sh/) # nice theme
  - install with `$sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
  - `ZSH_THEME=amuse` on `~/.zshrc` # personally use `amuse` theme

- ## SSH
  - `$ssh-keygen [enter * 3]`
  - `$pbcopy < ~/.ssh/id_rsa.pub` # copy to github

---
# Equip weapons

- ## [Homebrew](https://brew.sh/index_ko) # fetch weapons
	- install with `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`


- ## Python # powerful snakes
  - install multiple versions of snakes with [pyenv](https://github.com/pyenv/pyenv#homebrew-on-mac-os-x)
  - `brew install pyenv readline xz`
  - `echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.` # restart shell
  - `pyenv install 3.6.4` # takes long time
  - `pyenv global 3.6.4` -> restart shell -> `python --version` 
  - [Pycharm](https://www.jetbrains.com/pycharm/download/#section=mac)

---
- # JDK # for some python packages
  - `$java`
  - install with downaloded dmg
  - `$echo export "JAVA_HOME=\$(/usr/libexec/java_home)" >> ~/.zshrc`

- # Node.js
	- `brew install node`
	- `npm i -g yarn`
