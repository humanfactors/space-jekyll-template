---
layout: post
title: "Plain Text Note-taking Systems"
date: Jul  2 21:30:21 AWST 2017
categories:
 - Workflows
published: True
---

I've often been told that as a PhD student, "95% of your job is knowing how to
frame the problem." I believe that this same intuition applies when choosing
software solutions, specifically, you should be spending the majority of your
time thinking about the problem you are trying to tackle, not the tools you will
do it.

I am not very good at following my own advice. Over the years, I've used *a lot*
of tools, and often am looking for problems to solve with the tools. But note
taking is different. I have used almost all of the major note-taking software
(Onenote, Google Docs, Typora, Simplenote, nvALT), and text editors (Emacs, Vim, Atom, Visual Code,
Sublime). I've learned 3 important lessons from these tools:

1. No particular environment or program is best for any one person (Some people don't like rich graphical interfaces - other people are wrong.)
3. It is important to take notes in plain text
2. Emacs is awesome

In this article, I will be breaking down past and current Notetaking systems,
but I will be try and concentrate on the problem which each system solves,
rather than the tool itself. Remember that the tools themselves are just means
to an end.

## Write in Plain Text

The first note taking platform I used (other than pen and paper) was OneNote and
it served me well. It is incredibly powerful, and if you are a GUI oriented
person - I suggest you probably need not read further. However, over time I grew
weary. Exporting the files to another format (e.g. blog post, PDF file) was
difficult, embedding code snippets within the documents (with highlighting) was
impossible, and the format was simply too rigid. This led me to start writing
all my content in Plain Text.

It is critical that you learn the importance of writing your notes in plain
text - even if you choose for another option. Plain text is the practice of
writing your work with just plain text (think `.txt.`). Why you ask? Plain text
is computer readable making it incredibly portable, powerful and quick. You can
convert plain-text documents to almost any format with Pandoc. You can edit
plain text documents on virtually every computer built in the past forty odd
years and it's unlikely it will ever depreciate in the future. The other major
benefit is speed. I don't want to be waiting for loading times to quickly get a
note into place. This is what motivated me to use plain text.

**The Problem** here is I needed a quick and easy method to write Markdown notes
my aim was to have:

  - A single managed note system with minimal system requirements for writing or searching.
  - Able to sync the note system across any operating system.
  - Easily maintain this note system (i.e. automated).

## The Notational Velocity Method

Notational Velocity](http://notational.net/) is a method of rapid note taking in
which you can search for notes, create notes, or edit notes all with the same
keystroke. I enjoy their self declared mission statement "It is an attempt to
loosen the mental blockages to recording information and to scrape away the
tartar of convention that handicaps its retrieval. The solution is by nature
nonconformist."

Later last year I was experimenting with the Atom editor and my first NV version
was the Atom package[nvatom Atom](https://github.com/seongjaelee/nvatom). Nvatom
features the single interface for creating, searching and finding your notes
from a single keypress. **For example,** `F5` brings up your entry window, and
as you type in a phrase (e.g., `git`), nvalt returns a list of all files with
git in the filename _or_ the body. If the file doesn't exist **nvatom** will
create a new file with search term. Therefore you can instantly create new notes
or open old ones.

More recently, I have replaced this entirely with the Emacs
package [deft](http://jblevins.org/projects/deft/) and still use it almost
everyday. I will touch on Emacs some more in a bit. For now, let's keep
concentrated on the problem at hand: writing notes.

To give you an idea of how I like to label all my plain text notes, see the
output of my `ls | head -n 10` in my notes directory. Notice that key words are
seperated by a space. I find spaces between "tags" a more useful, natural and
readible than alternatives for notes. I generally called notes keywords that I
knew I could mash in later to find what I was looking for.

```
2017 Annual Form PHD.md
Feb 07 idea git.md
Feb todo.md
PHD Form Details.md
PMH webdesign notes.md
blog post shrell script.md
```

I rarely will interact with notes in here for long durations of time. It is like
a cache of ideas I sent to be filtered in specific projects. I just need to know
remember they are here (fun fact, sometimes I find things in my notes I forgot I
wrote - like whole chapters). 

One problem with my naming system was that it was too convoluted to mash in the
tags when I had seconds to get an idea out. Therefore, I now try and label all
my notes with either a date `2017February01 phd exp3.md`. I thought I would
start to make a quick local shell script that more or less does the same job.
The following script is pretty easy to set up in your own environment (works on
zsh).
 
If this file is added to path, when you call `nn` you create a new note with
current date and a random word from the dictionary (thanks perl). I find this
useful as the words serve as nice mnemonics.

{% highlight shell %}
#!/bin/bash

EXT=".md"
NOTESDIR=~/Google\ Drive/notes
MDEDITOR=Macdown

function nn {
  FILE=$1
  if [ -f "$NOTESDIR/$FILE" ]
   then
     open -a $MDEDITOR "$NOTESDIR/$FILE"
  fi

  if [ -z "$FILE" ]
   then
    WORD=`perl -e 'open IN, "</usr/share/dict/words";rand($.) < 1 && ($n=$_) while <IN>;print $n'`
    DATE=`date "+%y-%b%d"`
    FILENAME=$DATE$WORD$EXT
    touch "$NOTESDIR/$FILENAME" && open -a Macdown "$NOTESDIR/$FILENAME"
  else
    FILENAME=$1'.md'
    touch "$NOTESDIR/$FILENAME" && open -a Macdown "$NOTESDIR/$FILENAME"
  fi
}

noteatom() {
  atom "$NOTESDIR"/"$*".md
}

nsdir() {
  ls -c "$NOTESDIR" | grep "$*"
}

ns() {
  grep -ri "$*" "$NOTESDIR"/*
}
{% endhighlight %}

This script really never got a lot of daily usage, as soon after I started using
deft in Emacs.

## Through Chaos Emerges Orgmode (Emacs)

The Notational Velocity system worked great for simple jobs, but I found myself
yearning for more structure in my notes. Tags could only filter my notes so
well, and I was finding myself spending a lot of time cleaning up and
aggregating notes. My friend introduced me to Orgmode in Emacs and I haven't
looked back. I will cover this in more detail in a future post.

The highlights of Orgmode writing for me were:
- A focus on single long length documents
- Keybindings to quickly capture ideas (just google Org capture templates). For
  example, `ctrl-c C t` prompts you to input details for a todo item.
- I could add and track todo items
- Plain text, but powerful customization.

The main limitations of Emacs was the learning curve. To become fully productive
in the environment takes some time.

## Conclusions

I will update this post like a living object over time. If you are interested in
expanding your note taking power - I suggest finding an Notational Velocity
system. If you are currently using something like Onenote, I suggest taking a
look into how you can convert your system to a plain text one. It's hard to ever
go back.

