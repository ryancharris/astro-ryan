---
title: "Notes on 'Mastering emacs'"
publishDate: "2020-08-09"
published: true
layout: "../../layouts/BlogPost.astro"
description: "Raw notes from a 'Mastering emacs' bookclub I participated in."
category: "general"
tags:
  - "emacs"
---

I just started going through ['Mastering emacs'](https://www.masteringemacs.org/) by Mickey Petersen with a new Egghead book club cohort organized by [Ian Jones](https://www.ianjones.us).

'Why emacs?' you may ask. Good question. I've been happily using VS Code for a few years and enjoy it. My interest in emacs is primarily due to one thing: org mode. Imagine Notion, iCal and Todoist (plus more I don't even know about yet) all inside your editor. That's org mode.

Below are my evolving notes as we make our way through the book.

### Week 1 - Ch. 1 & 2

#### Reading and discussion notes

- GNU emacs is most common
- emacs is "for tinkerers"
- Built on elisp
  - 95% elisp
  - 5% C
- Has had a speech interface for 20+ years (emacspeak)
- Some terms predate modern computing terms
  - "killing" -- cutting
  - "yanking" -- pasting
  - "saving to the kill ring" -- copying

#### Usage notes

Commands are semantic:

- **Spc w** -- window commands
- **Spc b** -- buffer commands
- **Spc f** -- file commands
- **Spc p** -- project commands

**Spc o t**

- toggle integrated terminal window

**Spc b b**

- Show recent buffers

To add Prettier formatting:

- Add Prettier to `packages.el`

```elisp
(package! prettier-js)
```

- Require Prettier in `config.el`

```elisp
(require 'prettier-js)
```

- Add hooks to format on save in TypeScript and JavaScript modes

```elisp
(add-hook 'javascript-mode 'prettier-js-mode)
(add-hook 'typescript-mode 'prettier-js-mode)
```

### Week 2 - Chapter 3

#### Discussion notes

Everyone else is using org-roam for networked thought

- We were also roped into emacs by the Jones brothers!
- Most of us are keeping all our todos in todo.org
- Kevin has multiple files for todos and has his config set to load them all

Resources to reference:

- https://www.amazon.com/How-Take-Smart-Notes-Nonfiction-ebook/dp/B06WVYW33Y
- https://www.dschapman.com/
- https://linuxconfig.org/vim-tutorial
- https://vim-adventures.com/
- https://www.orgroam.com/manual/
- https://www.youtube.com/watch?v=rCMh7srOqvw&list=PLhXZp00uXBk4np17N39WvB80zgxlZfVwj&index=1
- https://www.youtube.com/watch?v=FtieBc3KptU&t=1561s
- https://tecosaur.github.io/emacs-config/config.html#rudimentary-configuration

org-roam comes in when you want to connect your notes and link them

- Keeps a flat file structure

Time tracking in emacs with org-mode tables

- You can do it manually with org-table
- Can you Pomodoro plugin

Reading was a bit dry this week

- Not much that was super useful since we're all using Doom emacs

Many folks have heard that `magit` is a great

- Super powerful git client

emacs has such a cool ecosystem and lots of great customizations

- Buying in too much is dangerous cause you can never leave

Lauro and I want to have our markdown files with a preview window next to it

- Being worked on

#### Questions for the group

- I'm confused about the emacs client-server concept

#### Usage notes

`C-g`: Bailout command

`M-x lunar-phases`: Shows a list of the upcoming lunar cycles

`M-x info`: Pull up the emacs info manual

`g d`: Global describe

#### Reading notes

Starting emacs

- You can run emacs either via the terminal or its GUI
- Using it from the terminal is a limited experience

> The emacs way is to keep it running and do all your editing in a dedicated Emacs instance.

- emacs starts slow cause of the packages and features
- Meant to be left open for long running sessions

Emacs client-server

1. **Persistent session** ??? use same session instead of spawning new instance
2. **It works well with \$EDITOR** ??? what exactly does this mean?
3. **Fast file opening** ??? uses `emacsclient`

"Key sequence"

- Keyboard or mouse actions that invoke a user command
- Commands are functions that are accessible to the user

**Keys**

Modifier keys

- `C -` ??? Control
- `M -` ??? Meta (i.e. Alt)
- `S -` ??? Shift

Discovering a remembering keys

- emacs is auto documented, so you want to leverage this as you learn
- `C-h` will show a help menu

> Tinking with emacs is every emacs hacker's favorite pastime

There are two ways to change your configuration in emacs:

1. Add elisp snippets to you config file
2. Use the built-in Customize UI

Customize UI

- Does not support every feature in emacs
- When making changes, you must `Save and Apply` or they will not persist to other sessions
- Changes here get persisted in your `init.el`
- More specific commands get you to specific settings like `M-x customize-face`

Evaluating Elisp code

- To evalue Elisp, you don't have to fully restart emacs
- `M-x eval-buffer` -- evaluates your entire current buffer
- `M-x eval-region` -- evaluates only the highlighted region

The package manager

- ELPA: official package manager
- MELPA: community package manager
- Add this config snippet to include ELPA and MELPA packages

```elisp
(setq package-archives
'(("gnu" . "http://elpa.gnu.org/packages/")
  ("melpa" . "http://melpa.org/packages/")))
```

Getting help

- emacs is self documenting and this is a huge key into mastering it
- Knowing where to find the answers is the important part because:
  - **emacs knows best** -- Your customizations can make it hard to find solutions online, so the best reference is your own emacs instance
  - **you discover more emacs** -- Learning this way helps you stumble upon new functionality you weren't aware of
  - **helps you solve more problems** -- The key isn't knowing everything, but where to look

Apropos

- Helpful when you're not sure exactly what command you're looking for
- Supports RegEx
- EX: `C-h a -word$` lists all commands ending with "word"
- To sort your apropos results by relevancy

```elisp
(setq apropos-sort-by-scores t)
```

- You can scope your search with commands like `apropos-documentation`, `apropos-user-option`, etc.

> The describe system is not static.

- The describe system has multiple helpful commands
  - `M-x describe-mode` or `C-h m` ??? Displays documentation for any major mode (uses your current buffer)
  - `M-x describe-function` or `C-h f` ??? Describes a function
  - `M-x describe-variable` or `C-h v` ??? Describes a variable
  - `M-x describe-key` or `C-h k` ??? Describes what a given keybinding does
