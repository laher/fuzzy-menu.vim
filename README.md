# fuzzymenu.vim

 * A menu for vim or neovim, built on top of [fzf](https://github.com/junegunn/fzf). 
 * Discover some vim features easily, without needing a mouse.

For now this includes a bunch of features which often need but I don't always remember. ...

## Install

Install fuzzymenu and dependencies using a plugin manager.

For example, use vim-plug:

```vim
Plug 'junegunn/fzf'
Plug 'junegunn/fzf.vim'
Plug 'laher/fuzzymenu.vim'
```

## Invoking fuzzymenu

`:Fzm`: You can invoke fuzzymenu with a command `:Fzm`

For convenience, you can create a key mapping. 

 * I like using space as a 'leader' key, and to hit space twice to bring up fuzzymenu.
 * By default, the leader key is mapped to `\\`. (I map leader to ' ': `mapleader=' '`)

This can be specified in your .vimrc (or init.vim, for neovim users):

```vim
  let mapleader=" "
  nmap <Leader><Leader> <Plug>Fzm
```

Now you can use 'fuzzy search' & up/down keys to choose menu items. 

Just bring up fuzzymenu, and start typing what you want...

## Bundled menu items

 * Various fzf.vim commands.
 * A few fundamentals: setting case-[in]sensitive searches, show/hiding line numbers and whitespace characters.
 * Various LSP features (requires [vim-lsp](https://github.com/prabirshrestha/vim-lsp): go to definition/implementation/references. rename, format, organize imports).
 * Various git features (requires [fugitive](https://github.com/tpope/vim-fugitive) ).
 * Various go tools (requires [gothx.vim](https://github.com/laher/gothx.vim) ).


More to follow

## Extend fuzzymenu.vim

There are a few ways you can introduce your own entries...

0. Create entries in your own .vimrc (or init.vim) file
1. Submit a PR with additional mappings for this project. If it seems useful I will probably approve it.
2. Define mappings in your own plugin, which can be loaded whenever fuzzymenu.vim is installed.

## Defining entries

Adding an entry looks like this:

```vim
call fuzzymenu#Add('FZF: Key mappings', {'exec': 'Maps', 'mode': 'insert', 'help': 'vim key mappings'})
```

The first parameter is a unique key. The second parameter is a map of options:

- `exec` is compulsory. This is the command which will be invoked. For example, `'Maps'` above is the same as running `:Maps` from normal mode. If you want to invoke a function instead, use `call`, e.g. `'call function#name()'`.
- `mode` is optional. If you need fuzzymenu to drop into insert mode after running your command, specify `'mode': 'insert'` (only insert mode is implemented). This is necessary for fzf features.
- `help` is for adding an additional explanation.
- `for` specifies a filetype. 

### Defining an entry in your plugin

Your plugin can add some entries to fuzzymenu, but just make sure to add them _only_ when fuzzymenu is installed. Like this (for an imaginary plugin 'teddy', which works on `.ted` files):

```vim
if &rtp =~ 'fuzzymenu.vim'
call fuzzymenu#Add('teddy bingo', {'exec': 'TddyBingo', 'for': 'ted'})
endif
```

Please use a central `plugin/*.vim` file to define filetype-specific mappings (rather than `ftplugin/`). fuzzymenu will look after which entries to show based on the `for` parameter and the filetype. It's just how fuzzymenu's registration mechanism works.

## Configuration

`g:fuzzymenu_auto_add`: set to `0` to prevent fuzzymenu adding its own entries. Now define your own instead. Default is 1
`g:fuzzymenu_position`: position of menu (default is 'down'. See fzf.vim) 
`g:fuzzymenu_size`: relative size of menu (default is `33%`. See fzf.vim) 


## Areas of interest

Contributors: I'm keen to focus on particular areas of functionality:

 * Fundamentals - any more general purpose tools like 'set nonumber', where the naming is not intuitive? Give it a more intuitive key name (fuzzymenu will still find it by the original name if necessary).
 * LSP clients - keen to add coc, LanguageClient-neovim, etc.
 * FZF - any more useful fzf-based commands? Happy to add the definitions here.
 * Language-specific features ... _BUT I'd prefer NOT to include language-specific features whenever LSP has an equivalent._

