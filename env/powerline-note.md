Setup powerline on Debian
-------------------------

**Setup the right vim version**
```
# apt-get install vim-nox
# update-alternatives --config editor
```
Select `vim.nox`

**Install python setup tools**
```
# apt-get install python-setuptools
```

**Install powerline**
```
$ git clone https://github.com/Lokaltog/powerline.git
$ sudo su
# cd powerline
# ./setup.py install
```

**Add these two lines to `.vimrc`**
```
set rtp+=/path/to/powerline/powerline/bindings/vim
set laststatus=2
```

**Install `tmux`**
```
# apt-get install tmux
```

**Add this to your `.bashrc`**
```
export POWERLINE_COMMAND=/path/to/powerline/scripts/powerline
. /path/to/powerline/powerline/bindings/bash/powerline.sh
```

**Install fonts for `powerline`**
```
$ git clone https://github.com/Lokaltog/powerline-fonts
$ mkdir ~/.fonts
$ cd ~/.fonts
$ ln -s /path/to/powerline-fonts/your_favorite_fonts/*.otf .
```
Basically, you make symbolic links at `$HOME/.fonts` to your
downloaded font (`*.otf` files). After this, these modified
fonts should be available to your system. In the `Preferences`
option of your terminal, change the current font to this
new font. Powerline will use this modified font to display
certain glyphs at the...powerline.

If you update `powerline` and see error `ImportError: No module named config`
when you open a new shell, it is likely that the new library path is mangled
with the old one. As root, edit the file `/usr/local/lib/python2.7/dist-packages/easy-install.pth`
Keep the new one `./powerline_status-dev-py2.7.egg` and delete the old one.
Probably, it is something like `./Powerline-dev-py2.7.egg`

Written by: _Tuan T. Pham_
