Setup powerline on Debian
-------------------------

**Setup the right vim version**  
`# apt-get install vim-nox`  
`# update-alternatives --config editor`  
Select `vim.nox`

**Install python setup tools**  
`# apt-get install python-setuptools`

**Install powerline**  
`$ git clone https://github.com/Lokaltog/powerline.git`  
`$ sudo su`  
`# cd powerline`  
`# ./setup.py install`

**Add these two lines to `.vimrc`**  
`set rtp+=/path/to/powerline/powerline/bindings/vim`  
`set laststatus=2`

**Install `tmux`**  
`# apt-get install tmux`

**Add this to your `.bashrc`**  
`$ echo ". /path/to/powerline/powerline/bindings/bash/powerline.sh" >> ~/.bashrc`

**Install fonts for `powerline`**  
`$ git clone https://github.com/Lokaltog/powerline-fonts`  
`$ cp powerline-fonts/your_favorite_fonts/*.otf ~/.fonts`

Written by: _Tuan T. Pham_
