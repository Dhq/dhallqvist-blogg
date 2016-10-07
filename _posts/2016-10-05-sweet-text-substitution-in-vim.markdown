---
layout: post
title:  "Sweet text substitution in Vim"
date:   2016-10-5
---

Substituting/replacing text in vim is powerful, fast and easy. But one thing that has really been lacking is a native and simple way to substitute text
on a project wide level. There have been a few techniques, myself I used the [vim-easygrep](http://github.com/dkprice/vim-easygrep) plugin.
But the plugin had its flaws, and didn't let you easily create filters like only substituting in specific filetypes or folders. It also felt a bit
annoying that a plugin was even needed.

So, in Vim 7.4.858 a new command was added: `cdo`. What it does is let you perform commands on the current items in the quicklist. I hadn't
heard about this until recently, when I got tired of vim-easygrep's limitations and started looking for a new way to perform global searches during a
huge variable refactoring in a sass-project. This means that we can populate the quicklist with our search friends
[Ack](https://github.com/mileszs/ack.vim) or [Ag](https://github.com/rking/ag.vim), using all the switches and options for filtering they offer,
and then run our subsitute command. So you don't have to write what you want to replace multiple times, an idea could be to yank it before running the search
and then use <Ctrl+0> in command mode to get it from the yank register. If the cursor is standing on whatever you want to replace, get it from the word register
with <Ctrl+w>. So say I want to replace "1333337" with "l33t" in all css-files under the "site" folder, it could look like this:

* yank "1333337" from my current file
* `:Ag -G site* --css "1333337"` (<Ctrl+0)> to get blue from the yank register)
* `:cdo s/1333337/l33t/g`
* `:wa` (Write/save all buffers)
* `:ccl` (to close the quicklist window, I have it bound to &lt;leader&gt;x)

As always it's a good idea to begin with committing your changes before, so you can easily undo if you change your mind. Also, you can of course add the c-switch to the substitute-command
if you want to confirm before all changes. I tried creating a simple vimscript-function that does all these steps with two arguments, but vimscript is still terrible
and I didn't have the patience to tackle with it right now, maybe some other day! :)

Another, general, tip that comes in handy when performing these search/replaces is `q:` (no, not :q). This opens the command history window, with
your latest commands on each row. Now you can navigate and edit the lines like in any other vim window, and then press enter to run it.
Handy if you performed a search, made a typo and want to quickly fix it and run the command again without having to type it all over.

Here are some other, pluginless, bindings I use to subsitute within the current file:

```vimscript
"Substitute word under cursor on current line
nmap <leader>sl :s/<c-r>=expand("<cword>")<cr>//g<Left><Left>

" Substitute word under cursor in current file
nmap <leader>sf :%s/<c-r>=expand("<cword>")<cr>//g<Left><Left>
```

Happy to hear if anyone has other tips when it comes to `cdo` or substitution in vim!
