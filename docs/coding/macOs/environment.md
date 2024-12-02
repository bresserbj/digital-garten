---
title: Environment
date: 20241202
author: bresserbj
---

Diese Seite soll mir dabei helfen, Anforderungen an mein Macbook zu definieren und als Art Anleitung zum neuaufsetzen eines Macbooks.

## Anforderungen an mein Macbook

- Softwareentwicklung mit folgenden Frameworks, Technologien und Software
    - Kotlin
    - Java
    - Docker
    - Gradle
    - Python
    - Git
    - IntelliJ

### Alfred

Alfred bietet eine effizientere Erweiterung fuer MacOs als die Spotlight-Suche vom Os selbst.
Daher sollte die AlfredApp installiert werden: https://www.alfredapp.com/


### Brew

Brew ist relativ einfach installiert. Man oeffnet das Terminal und gibt folgende Zeile ein

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Nachfolgend sind die weiteren Installationen relativ schnell erledigt.

### Softwareinstallationen mit Brew

```
# GIT
brew install git

# Docker
brew install docker
```

### Terminal

Als Terminal faellt die Wahl auf [iTerm2](https://iterm2.com/) mit einigen Modifikationen.

```
brew cask install iterm2
```

#### zsh & "oh-my-zsh"

Um zsh zu installieren nutzten wir im Terminal: 

```
brew install zsh
```

Fuer eine Installation von oh-my-zsh nutzen wir nachfolgenden Befehl, wodurch das Standardterminal 
anschliessend automatisch durch zsh ersetzt wird. 

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

#### Install 'Fuck'

Eins der besten Plugins fuer Faule und Menschen die gerne Tippfehler in der Konsole begehen.ds

```
brew install thefuck
```

#### Wichtige Anpassungen

Die zsh_fallout.json zu den Profilen hinzufuegen.

##### Theme

Zuerst muessen wir das Theme konfigurieren. Hierzu benoetigen wie einige Fonts etc.

```
sudo apt install fonts-powerline

git clone https://github.com/abertsch/Menlo-for-Powerline.git
cd Menlo-for-Powerline
cp "Menlo for Powerline.ttf" ~/.fonts
```

```
vi ~/.zshrc

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
ZSH_THEME="agnoster"
```

##### Plugins

Um externe Plugins zu installieren kann dieser Befehl genutzt werden
```
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

```
vi ~/.zshrc


# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(
  git
  github
  brew
  docker
  npm
  macos
  bgnotify
  zsh-syntax-highlighting
  zsh-autosuggestions
  web-search
)
```

##### Alias Parameter

```
vi ~/.zshrc

# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"

alias zshconfig="nano ~/.zshrc"
alias ohmyzsh="nano ~/.oh-my-zsh"
alias gpf='git push -f'

# Docker alias
alias dkps="docker ps"
alias dkst="docker stats"
alias dkpsa="docker ps -a"
alias dkimgs="docker images"
alias dkcpup="docker-compose up -d"
alias dkcpdown="docker-compose down"
alias dkcpstart="docker-compose start"
alias dkcpstop="docker-compose stop"

# Kubectl alias
alias kdev='kubectl -n dev'
alias kpg='kubectl -n playground'
alias ktest='kubectl -n test'
alias kprod='kubectl -n prod'
alias kpreprod='kubectl -n preprod'
alias vpnstart='~/Scripts/vpn-start.sh'
alias vpn-start='~/Scripts/vpn-start.sh'

eval $(thefuck --alias)
```

### Macbook shortcuts

TBD
