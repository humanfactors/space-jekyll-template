---
layout: post
title: "Caps Lock as Hyper Key Modifier"
image: ''
date:   2016-12-21 00:06:31
published: true
categories:
- Workflows
description: "A quick tutorial and how-to rebind caps lock key in Windows 10, MacOS and Debian."
---

Binding Caps Lock to the ["Hyper Key"](https://en.wikipedia.org/wiki/Modifier_key) gives you an entire new pallete of shortcut commands. For instance, you could bind `Caps Lock + L` to ___?

## Hyper Key Modifer to Caps Lock in Debian

_(Note: Experienced Debian users can jump straight down to the script below and run it an appropriate time after the xserver launches)._ Linux was surprisingly the hardest of the three operating systems to rebind Caps Lock to something even remotely useful. At time of writing, I am running the following configuration:

~~~

         _,met$$$$$gg.           Michael@Debian
      ,g$$$$$$$$$$$$$$$P.        OS: Debian testing stretch
    ,g$$P""       """Y$$.".      Kernel: x86_64 Linux 4.8.0-2-amd64
   ,$$P'              `$$$.      Uptime: 1h 30m
  ',$$P       ,ggs.     `$$b:    Packages: 2600
  `d$$'     ,$P"'   .    $$$     Shell: zsh 5.2
   $$P      d$'     ,    $$P     Resolution: 1920x1080
   $$:      $$.   -    ,d$$'     DE: Cinnamon 3.0.7
   $$\;      Y$b._   _,d$P'      WM: Muffin
   Y$$.    `.`"Y$$$$P"'          WM Theme: cinnamon (Menta)
   `$$b      "-.__               GTK Theme: Menta [GTK2/3]
    `Y$$                         Icon Theme: menta
     `Y$$.                       Font: Noto Sans 10
       `$$b.                     CPU: Intel Core i5-4690 CPU @ 3.9GHz
         `Y$$b.                  GPU: GeForce GTX 970/PCIe/SSE2
            `"Y$b._              RAM: 2072MiB / 7914MiB
~~~

In Linux, all keyinput is controlled by your Xserver. You can inspect your current modifer key bindings with a simple call to `xmodmap` which will yield something like the code output below. On the left you have the raw function (shift, alt, control etc) and on the right the functions currently mapped to these keys. **Yours may look a little different and you may have to update my following instructions arcordingly**. As can be seen below, Xserver (by default) maps the hyper key to the windows key modifier (mod4). Before going further, save this xmodmap somewhere as a default failsafe.

{% highlight bash %}
shift       Shift_L (0x32),  Shift_R (0x3e)
lock      
control     Control_L (0x25),  Control_R (0x69)
mod1        Alt_L (0x40),  Alt_R (0x6c),  Meta_L (0xcd)
mod2        Num_Lock (0x4d)
mod3        
mod4        Super_L (0x85),  Super_R (0x86),  Super_L (0xce), Hyper_L (0x42),  Hyper_L (0xcf)
mod5        ISO_Level3_Shift (0x5c),  Mode_switch (0xcb)
{% endhighlight %}

In order to get around this, we need to edit the `xmodmap` configuration by calling `xmodmap -e <command> <modifier> - <function>`. Have a play around with these commands now and see if you can get your system to give the configuration you desire. 

Great, if you've got the configuration you are after: we are almost there (but if you are still struggling, I suggest trying to clear entries one by one and adding them back in). One  problem remains though, the parameters for `xmodmap` are reset at every login to default, so you can either edit your `.xsessionrc` or `.xprofile` files (as documented here [here for arch](https://bbs.archlinux.org/viewtopic.php?id=66453) and [here for debian](https://askubuntu.com/questions/54157/how-do-i-set-xmodmap-on-login)). Therefore, we need to write a script that can be set at launch as seen below. This will need to be run after every time you login. Depending on your distribution this task may be the final step, or under some distributions this post may be just a stopover in your journey to your solution (so you can be productive again, right?).

### Shell Script to Rebind Caps to Hyper Key

{% highlight bash %}
#!/bin/bash
#Removes Hyper binding from windows modifier
xmodmap -e "remove Mod4 = Hyper_L"
#Maps caps lock to Hyper_L and Hyper_R
xmodmap -e "keycode 66 = Hyper_L Hyper_R"
# Make mod3 also map to Hyper keys
xmodmap -e "add mod3 = Hyper_L Hyper_R"
{% endhighlight %}

Save this as an execuatable bash script with `sudo chmod 775 hyperkey.sh` and then move it `~sudo mv hyperkey.sh /bin/hyperkey.sh`. Finally head to the "Cinnamon Search Menu" (hit Windows key) and navigate to "Startup Applications". Then click add > custom command > browse, and select your newly create shell script. I made mine have a delay timer of 8 seconds to be sure it would override any other X configuration performed by Cinnamon. See example below.

![]({{ site.url }}/assets/img/debian-hyperkey-binding/Startup%20Applications_006.png)

Now all Debian and Cinnamon UIs will register our "Caps Hyper" as a proper key for keybindings. 

![Use Hyper key to bind to your favourite functions]({{ site.url }}/assets/img/debian-hyperkey-binding/Keyboard_008.png)

Word of caution here though: hyperkey may not work as intended in all applications, for instance [Atom text editor](https://atom.io).

## Hyper Key in Windows

Windows will require the use of a program called [AutoHotKey](https://autohotkey.com/). Whilst binding to a _true_ hyper key in windows is hard, we can bind `Ctrl + Alt + Shift` together as modifiers on Caps Lock (how many commands do you know that use all three?)

Simply follow the instructions on their site for managing macros and then create (with notepad) the following AHK script:

{% highlight ruby %}
*CapsLock::
  SetKeyDelay -1
  Send {Blind}{Ctrl DownTemp}{Alt DownTemp}{Shift DownTemp}
return

*CapsLock up::
  SetKeyDelay -1
  Send {Blind}{Ctrl Up}{Alt Up}{Shift Up}
return
{% endhighlight %}

## Hyper Key in MacOS Sierra

This is a bit of a qwerky solution and also uses the `CMD+ Ctrl + Alt + Shift ` - but it works. It requires two steps:

1. Unbind the funcionality of caps_lock in your Sys Preferences > Keyboard Settings > Modifiers > Caps Lock > None

2. For Sierra users, download the Rebase with [@k0nserv](https://github.com/k0nserv) changes [Karabiner-Elements-0.90.55.dmg](https://s3-us-west-2.amazonaws.com/mrooney-public/Karabiner-Elements-0.90.68.dmg) For those interested, [here](https://github.com/chrisfsmith/Karabiner-Elements/commits/cmd-alt-shift-ctrl-hyper) is the branch Christ F Smith it built from. **This version enables hyperkey support**

![]({{ site.url }}/assets/img/debian-hyperkey-binding/Screen%20Shot%202016-12-22%20at%2012.49.28%20am.png)
![]({{ site.url }}/assets/img/debian-hyperkey-binding/Screen%20Shot%202016-12-22%20at%2012.49.04%20am.png)

And there you have it, caps lock now functional.

