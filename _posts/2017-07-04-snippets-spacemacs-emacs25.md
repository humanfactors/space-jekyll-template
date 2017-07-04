---
layout: post
title: "Snippets: Spacemacs (Emacs25)"
date: Tue Jul  4 20:44:32 AWST 2017
tags:
 - snippets
---

I recently started using Emacs, specifically, the community Spacemacs
distribution. Here are some of the most useful functions I have found so far.
For the latest version, please consult my [config](https://github.com/humanfactors/skanky-config/blob/master/.spacemacs)

## Visual Line Mode
I prefer soft wrapping my lines. Note two parts of this configuration. Firstly,
I set variables that make buffers soft wrap. Next, I change my evil binds to
operate on visual lines rather than logical lines.

{% highlight lisp %} 
(setq word-wrap 1)
(global-visual-line-mode t)
;; Make evil-mode up/down operate in screen lines instead of logical lines 
(define-key evil-motion-state-map "j" 'evil-next-visual-line)
(define-key evil-motion-state-map "k" 'evil-previous-visual-line)
;; Also in visual mode
(define-key evil-visual-state-map "j" 'evil-next-visual-line)
(define-key evil-visual-state-map "k" 'evil-previous-visual-line)
{% endhighlight %}

## Insert date/timestamp Emacs Keybinding
I find this function to be very useful and was surprised it wasn't already in
emacs. Bind to whatever you would like and get a date.

{% highlight lisp %}
(defun insert-current-datetime () (interactive)
	(insert (shell-command-to-string "echo -n $(date '+%A (%B %d) @ %H:%m')")))
(global-set-key "\C-x\M-d" `insert-current-datetime)
{% endhighlight %}

## Open Orgmode heading in right window
I found this gem from Stack. Essentially, I wanted a way to quickly preview the
contents of the org heading content in the right window. Sadly, in its current
form it does break links.

{% highlight lisp %}
(defun org-tree-open-in-right-frame ()
(interactive)
(org-tree-to-indirect-buffer)
(windmove-right))

(add-hook 'org-mode-hook
	(lambda ()
	(define-key evil-normal-state-local-map (kbd "S-<return>") 'org-tree-open-in-right-frame)
	(define-key evil-normal-state-local-map (kbd "<return>") 'org-tree-to-indirect-buffer)
))
{% endhighlight %}

## Toggle NeoTree for Directory Viewing
I find this provides a more natural IDE experience
{% highlight lisp %}
(global-set-key [f8] 'neotree-toggle)
{% endhighlight %}

## General Orgmode Config
I think these are quite sensible defaults for new users to Orgmode.
{% highlight lisp %}
(cua-mode t)
(transient-mark-mode 1) ;; No region when it is not highlighted
(setq cua-keep-region-after-copy t) ;; Standard Windows behaviour
(setq cua-auto-tabify-rectangles nil) ;; Don't tabify after rectangle commands
(define-key cua-global-keymap [C-return] nil)  ;;rebind rectangle mark as I want to use C-return for R/ESS and
(delete-selection-mode 1)
(setq save-interprogram-paste-before-kill t)
(setf org-blank-before-new-entry '((heading . nil) (plain-list-item . nil)))
(setq org-bullets-mode nil)
(setq org-blank-before-new-entry nil)
(setq org-support-shift-select t)
(define-key org-mode-map (kbd "C-M-<return>") 'org-insert-subheading)
{% endhighlight %}

