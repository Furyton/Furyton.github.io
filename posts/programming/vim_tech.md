---
title: vim techs
author: Shiguang Wu
date: 2022-03-08
categories: [vim, programming]
---

## yank abs path or relative path in NerdTree

add key mapping yourself

save below as `xxx.vim` in `/path/to/nerdtree/nerdtree_plugin/xxx.vim`

`yy` for abs path of current node, `yr` for rel path

```{{vimscript}}
call NERDTreeAddKeyMap({
        \ 'key': 'yy',
        \ 'callback': 'NERDTreeYankFullPath',
        \ 'quickhelpText': 'put full path of current node into the default register' })

function! NERDTreeYankFullPath()
    let n = g:NERDTreeFileNode.GetSelected()
    if n != {}
        call setreg('"', n.path.str())
    endif
    call nerdtree#echo("Node full path yanked!")
endfunction

call NERDTreeAddKeyMap({
        \ 'key': 'yr',
        \ 'callback': 'NERDTreeYankRelativePath',
        \ 'quickhelpText': 'put relative path of current node into the default register' })


function! NERDTreeYankRelativePath()
    let n = g:NERDTreeFileNode.GetSelected()
    if n != {}
        call setreg('"', fnamemodify(n.path.str(), ':.'))
    endif
    call nerdtree#echo("Node relative path yanked!")
endfunction
```

## add sth. surround

`ys` takes a motion or text object, and then the char you want to put surround with

like `ysiw"` surr current word with `"`
