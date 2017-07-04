---
layout: post
title: "Snippets: ZSH and Terminal"
date: Tue Jul  4 20:07:15 AWST 2017
tags:
 - Snippets
---

## My Terminal Environment

I use a lightly modified version of [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)


## Different ZSH Configs per OS
I use a different configuration on all my Linux computers than on my Macbook.
This are the packages I use for Arch/Debian and for MacOS. Also note how I
change my config across operating systems.

{% highlight bash %}
case `uname` in
  Darwin)
    # MacOS Config
		plugins=(zsh-autosuggestions git osx colored-man
        		colorize brew pip python command-not-found)
  ;;
  Linux)
    # Linux Config
		plugins=(git zsh-autosuggestions colored-man autojump)
  ;;
esac

{% endhighlight%}
I use this same logic for aliases particular to a machine

{% highlight bash %}
case `uname` in
  Darwin)
    # MacOS Config
		bindkey "^U" backward-kill-line
		alias m2l="pbpaste | pandoc -f markdown -t latex | pbcopy"
		alias tunnelon='networksetup -setsocksfirewallproxy "Wi-Fi" localhost 1420'
		alias tunneloff='networksetup -setsocksfirewallproxystate "Wi-Fi" off'
		alias osx='less ~/.oh-my-zsh/plugins/osx/README.md'

  ;;
  Linux)
  		export ARCHFLAGS="-arch x86_64"
		alias say="espeak"
		alias sshoff="sudo systemctl stop sshd"
		alias sshon="sudo systemctl start sshd"
  ;;
esac
source $ZSH/oh-my-zsh.sh
{% endhighlight%}


## Automatically print ls on cd
I like to automatically print the directory listing when I change directories
{% highlight bash %}
auto-ls () {
	emulate -L zsh;
	# explicit sexy ls'ing as aliases arent honored in here.
	hash gls >/dev/null 2>&1 && CLICOLOR_FORCE=1 gls -aFh --color --group-directories-first || ls
}
chpwd_functions=( auto-ls $chpwd_functions )
{% endhighlight%}

## Retrieving Public IP

{% highlight bash %}
alias whatsmyip="wget -qO- http://ipecho.net/plain ; echo"
{% endhighlight%}


## Terminal Australian Radio
{% highlight bash %}
alias radioabcnews="mplayer -playlist http://www.abc.net.au/res/streaming/audio/mp3/news_radio.pls" #
alias radiobbcnews="mplayer -playlist http://minnesota.publicradio.org/tools/play/streams/news.pls" #
alias radiorn="mplayer -playlist http://www.abc.net.au/res/streaming/audio/mp3/radio_national.pls" #
alias radiojjj="mplayer -playlist  http://www.abc.net.au/res/streaming/audio/mp3/triplej.pls" #
alias radiobbcnews="mplayer -playlist http://minnesota.publicradio.org/tools/play/streams/news.pls" #
alias radiosleepbot="mplayer -playlist http://sleepbot.com/ambience/cgi/listen.cgi/listen.pls" # Sleepbot Environmental Broadcast 56K MP3
{% endhighlight%}
