---
layout: article
title: "Key mapping collision between vim-navigation and vim-latex"
date: 2015-01-04T23:14:02-08:00
modified:
categories: articles
excerpt: "Key mapping collision for <C-J>"
comments: true
tags: [vim]
ads: true
image:
  feature:
  teaser:
---

I just added the `vim-latex` plugin and found out that the vim-tmux navigation <C-J> was not working.

1. I remap the <c-j> to :TmuxNavigateDown<cr> at the end of .vimrc but it does nothing. How odd!

    map <silent> <C-J> :TmuxNavigateDown<cr>
    nmap <silent> <C-J> :TmuxNavigateDown<cr>
    nnoremap <silent> <C-J> :TmuxNavigateDown<cr>

2. Typed `:verbose map <c-j>` and got key mappings for <c-j>

3. Just commented out the key mapping in ~/.vim/bundle/vim-latex/plugin/imaps.vim in line

~~~
if !hasmapto('<Plug>IMAP_JumpForward', 'i')
    imap <C-J> <Plug>IMAP_JumpForward
endif
if !hasmapto('<Plug>IMAP_JumpForward', 'n')
    nmap <C-J> <Plug>IMAP_JumpForward
endif
if exists('g:Imap_StickyPlaceHolders') && g:Imap_StickyPlaceHolders
	if !hasmapto('<Plug>IMAP_JumpForward', 'v')
		vmap <C-J> <Plug>IMAP_JumpForward
	endif
else
	if !hasmapto('<Plug>IMAP_DeleteAndJumpForward', 'v')
		vmap <C-J> <Plug>IMAP_DeleteAndJumpForward
	endif
endif
~~~
