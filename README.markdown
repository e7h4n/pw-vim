# pw-vim : E7h4n's Vim Distribution

 ______   __     __        __   __   __     __    __
    /\  == \ /\ \  _ \ \      /\ \ / /  /\ \   /\ "-./  \
    \ \  _-/ \ \ \/ ".\ \     \ \ \'/   \ \ \  \ \ \-./\ \
     \ \_\    \ \__/".~\_\     \ \__|    \ \_\  \ \_\ \ \_\
      \/_/     \/_/   \/_/      \/_/      \/_/   \/_/  \/_/


[pw-vim] is a distribution of vim plugins and resources for Vim, Gvim and [MacVim], an forked repository from [spf13-vim].

# Installation
## Requirements
To make all the plugins work, specifically [neocomplete](https://github.com/Shougo/neocomplete.vim), you need [vim with lua](https://github.com/Shougo/neocomplete.vim#requirements).

## Linux, \*nix, Mac OSX Installation

The easiest way to install pw-vim is to use our [automatic installer](https://raw.githubusercontent.com/e7h4n/pw-vim/master/bootstrap.sh) by simply copying and pasting the following line into a terminal. This will install pw-vim and backup your existing vim configuration. If you are upgrading from a prior version (before 3.0) this is also the recommended installation.

```bash
    curl https://raw.githubusercontent.com/e7h4n/pw-vim/master/bootstrap.sh -L -o - | sh
```

## Updating to the latest version
The simpliest (and safest) way to update is to simply rerun the installer. It will completely and non destructively upgrade to the latest version.

```bash

    curl https://raw.githubusercontent.com/e7h4n/pw-vim/master/bootstrap.sh -L -o - | sh

```

Alternatively you can manually perform the following steps. If anything has changed with the structure of the configuration you will need to create the appropriate symlinks.

```bash
    cd $HOME/to/pw-vim/
    git pull
    vim +NeoBundleInstall! +NeoBundleClean +q
```

## Customization

Create `~/.vimrc.local` and `~/.gvimrc.local` for any local
customizations.

For example, to override the default color schemes:

```bash
    echo colorscheme ir_black  >> ~/.vimrc.local
```

### Before File

Create a `~/.vimrc.before.local` file to define any customizations
that get loaded *before* the spf13-vim `.vimrc`.

For example, to prevent autocd into a file directory:
```bash
    echo let g:spf13_no_autochdir = 1 >> ~/.vimrc.before.local
```
For a list of available spf13-vim specific customization options, look at the `~/.vimrc.before` file.


### Fork Customization

There is an additional tier of customization available to those who want to maintain a
fork of pw-vim specialized for a particular group. These users can create `.vimrc.fork`
and `.vimrc.bundles.fork` files in the root of their fork.  The load order for the configuration is:

1. `.vimrc.before` - spf13-vim before configuration
2. `.vimrc.before.fork` - fork before configuration
3. `.vimrc.before.local` - before user configuration
4. `.vimrc.bundles` - spf13-vim bundle configuration
5. `.vimrc.bundles.fork` - fork bundle configuration
6. `.vimrc.bundles.local` - local user bundle configuration
6. `.vimrc` - spf13-vim vim configuration
7. `.vimrc.fork` - fork vim configuration
8. `.vimrc.local` - local user configuration

See `.vimrc.bundles` for specifics on what options can be set to override bundle configuration. See `.vimrc.before` for specifics
on what options can be overridden. Most vim configuration options should be set in your `.vimrc.fork` file, bundle configuration
needs to be set in your `.vimrc.bundles.fork` file.

You can specify the default bundles for your fork using `.vimrc.before.fork` file. Here is how to create an example `.vimrc.before.fork` file
in a fork repo for the default bundles.
```bash
    echo let g:spf13_bundle_groups=[\'general\', \'programming\', \'misc\', \'youcompleteme\'] >> .vimrc.before.fork
```
Once you have this file in your repo, only the bundles you specified will be installed during the first installation of your fork.

You may also want to update your `README.markdown` file so that the `bootstrap.sh` link points to your repository and your `bootstrap.sh`
file to pull down your fork.

For an example of a fork of spf13-vim that provides customization in this manner see [taxilian's fork](https://github.com/taxilian/spf13-vim).

### Easily Editing Your Configuration

`<Leader>ev` opens a new tab containing the .vimrc configuration files listed above. This makes it easier to get an overview of your
configuration and make customizations.

`<Leader>sv` sources the .vimrc file, instantly applying your customizations to the currently running vim instance.

These two mappings can themselves be customized by setting the following in .vimrc.before.local:
```bash
let g:spf13_edit_config_mapping='<Leader>ev'
let g:spf13_apply_config_mapping='<Leader>sv'
```
# Plugins

pw-vim contains a curated set of popular vim plugins, colors, snippets and syntaxes. Great care has been made to ensure that these plugins play well together and have optimal configuration.

## Adding new plugins

Create `~/.vimrc.bundles.local` for any additional bundles.

To add a new bundle, just add one line for each bundle you want to install. The line should start with the word "Bundle" followed by a string of either the vim.org project name or the githubusername/githubprojectname. For example, the github project [spf13/vim-colors](https://github.com/spf13/vim-colors) can be added with the following command

```bash
    echo NeoBundle \'spf13/vim-colors\' >> ~/.vimrc.bundles.local
```

Once new plugins are added, they have to be installed.

```bash
    vim +BundleInstall! +BundleClean +q
```

## Removing (disabling) an included plugin

Create `~/.vimrc.local` if it doesn't already exist.

Add the NeoBundleDisable command to this line. It takes the same input as the Bundle line, so simply copy the line you want to disable and add 'Un' to the beginning.

For example, disabling the 'AutoClose' and 'scrooloose/syntastic' plugins

```bash
    echo NeoBundleDisable \'AutoClose\' >> ~/.vimrc.local
    echo NeoBundleDisable \'scrooloose/syntastic\' >> ~/.vimrc.local
```

**Remember to run ':NeoBundleClean!' after this to remove the existing directories**

# Difference betweet pw-vim and [spf13-vim]

Main changes:

* Use [Unite] instead [ctrlp].
* Use [snipmate] instead [neocomplete].
* Map `jj` to `<ESC>`.
* Swap `0` and `^`, `0` is more convenience.
* Use my favorite color theme [molokai] instead [solarized].
* Reset some Vim settings to default:
    * Unbind vim register and system clipboard.
    * Remove `options` from `viewoptions`.
    * `nospell`.
    * `wrap`.
    * `scrolljump` to 0.
    * `scrolloff` to 0.
* Reset some map to default setting
    * Unmap `<C-J>`, `<C-K>`, `<C-L>`, `<C-H>`.
    * Unmap `<` and `>`.
    * Unmap `j`/`k`.

The reason is very simple: I don't need them.

# Fork me on GitHub

I'm always happy to take pull requests from others. Go ahead and fork me.

[![Analytics](https://ga-beacon.appspot.com/UA-7131036-5/spf13-vim/readme)](https://github.com/igrigorik/ga-beacon)
[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/spf13/spf13-vim/trend.png)](https://bitdeli.com/free "Bitdeli Badge")


[Git]:http://git-scm.com
[Curl]:http://curl.haxx.se
[Vim]:http://www.vim.org/download.php#pc
[msysgit]:http://msysgit.github.io
[Chocolatey]: http://chocolatey.org/
[spf13-vim package]: https://chocolatey.org/packages/spf13-vim
[MacVim]:http://code.google.com/p/macvim/
[pw-vim]:https://github.com/e7h4n/pw-vim
[spf13-vim]:https://github.com/spf13/spf13-vim
[contributors]:https://github.com/spf13/spf13-vim/contributors

[NeoBundle]:https://github.com/Shougo/neobundle.vim
[PIV]:https://github.com/spf13/PIV
[NERDCommenter]:https://github.com/scrooloose/nerdcommenter
[Undotree]:https://github.com/mbbill/undotree
[NERDTree]:https://github.com/scrooloose/nerdtree
[ctrlp]:https://github.com/kien/ctrlp.vim
[solarized]:https://github.com/altercation/vim-colors-solarized
[neocomplete]:https://github.com/shougo/neocomplete
[Fugitive]:https://github.com/tpope/vim-fugitive
[Surround]:https://github.com/tpope/vim-surround
[Tagbar]:https://github.com/majutsushi/tagbar
[Syntastic]:https://github.com/scrooloose/syntastic
[vim-easymotion]:https://github.com/Lokaltog/vim-easymotion
[YouCompleteMe]:https://github.com/Valloric/YouCompleteMe
[Matchit]:http://www.vim.org/scripts/script.php?script_id=39
[Tabularize]:https://github.com/godlygeek/tabular
[EasyMotion]:https://github.com/Lokaltog/vim-easymotion
[Airline]:https://github.com/bling/vim-airline
[Powerline]:https://github.com/lokaltog/powerline
[Powerline Fonts]:https://github.com/Lokaltog/powerline-fonts
[AutoClose]:https://github.com/spf13/vim-autoclose
[Ack.vim]:https://github.com/mileszs/ack.vim

[spf13-vim-img]:https://i.imgur.com/UKToY.png
[spf13-vimrc-img]:https://i.imgur.com/kZWj1.png
[autocomplete-img]:https://i.imgur.com/90Gg7.png
[tagbar-img]:https://i.imgur.com/cjbrC.png
[fugitive-img]:https://i.imgur.com/4NrxV.png
[nerdtree-img]:https://i.imgur.com/9xIfu.png
[phpmanual-img]:https://i.imgur.com/c0GGP.png
[easymotion-img]:https://i.imgur.com/ZsrVL.png
[airline-img]:https://i.imgur.com/D4ZYADr.png
[snipmate]:https://github.com/garbas/vim-snipmate
