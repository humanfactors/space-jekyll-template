---
layout: post
title: "A Plain Text Note-taking System"
categories:
 - Workflows
published: false
---

I've often been told that as a PhD student, "95% of your job is knowing how to frame the problem."
I believe that this same intuition applies when choosing software solutions, specifically, 
you should be spending the majority of your time thinking about the problem you are trying to tackle, not the tools you will do it.

That said, I have used almost all of the major note-taking software, and a good handful of code editors/IDEs. What I have come to realize is that no particular environment or program is best for any one person. In fact, no single note taking solution is perfect for even a single person! I have a few different techniques (which I will show you) and each of them serve a particular purpose.

For each tool/solution I list in this guide, I will explain _firstly_ the problem state that motivated me to use the tool; and _secondly_ I will provide a brief summary and explanation. Remember that the tools themselves are just means to an end.

## Why Plain Text?

Plain text is the practice of writing your work with just plain text (think `.txt.`). Plain text is computer readable making it incredibly portable, powerful and quick. You can edit plain text documents on virtually every computer built in the past forty odd years and it's unlikely it will ever depreciate.

But why plain text over the abundance of other feature rich editors? Well, let's start from the beginning. The first note taking platform I used was OneNote and it served me well. It is incredibly powerful, and if you are a GUI oriented person - I suggest you probably need not read further. However, over time I grew weary. Exporting the files to another format (e.g. blog post, PDF file) was difficult, embedding code snippets within the documents (with highlighting) was impossible, and the format was simply too rigid. This led me to start writing all my content in Markdown.

## The Start: Markdown Notes

My aim was to have:

  - A single managed note system with minimal system requirements for writing or searching.
  - Able to sync the note system across any operating system.
  - Easily maintain this note system (i.e. automated).

My first solution was Nvatom.

## Nvatom (Nvalt)

I really love the concept of the Atom package[nvatom Atom](https://github.com/seongjaelee/nvatom). Those who have used nvalt will recognises the similarity. Nvatom has a single interface for creating, searching and finding your notes from a single keypress.

**Example:** `F5` brings up your entry window, and as you type in a phrase (e.g., `git`), nvalt returns a list of all files with git in the filename _or_ the body. If the file doesn't exist **nvatom** will create a new file with search term. Therefore you can instantly create new notes or open old ones.


### How should I sort my notes?

Personally, I like to label all my plain text notes with key words seperated by a space. See the output of my `ls | head -n 10`. I find spaces between "tags" a more useful, natural and readible than alternatives.

```
$ ls | head -n 10
2017 Annual Form PHD.md
Feb 07 idea git.md
Feb todo.md
PHD Form Details.md
PMH webdesign notes.md
blog post shrell script.md
```

I rarely will interact with notes in here for long durations of time. It is like a cache of ideas I sent to be filtered in specific projects. I just need to know where to find them when they are here.
 
 I label all my notes with either a date `2017February01 phd exp3.md`; this makes it great when I'm unsure if I have written about it before - or when I am deciding
 
 However, I thought I would start to make a quick local shell script that more or less does the same job
 
 
 ### Script

The following script is pretty easy to set up in your own environment (works on zsh). It's nothing special but makes a great basic shell tutorial.

If you call nn
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
## Emacs

## Org Mode

