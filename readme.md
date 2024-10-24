# A bunch of configuration files

A place to backup my system's configuration.

## About the repository structure

This repo uses a directory tree structure of *packages*, which are organized as:

```text
root-dir
├── package-1
│   └── dotfiles-tree
├── package-2
│   └── dotfiles-tree
├── ...
└── package-N
    └── dotfiles-tree
```

and each package includes their respective configuration files tree structure.
Note that the package contents should match the structure of your user's
`$HOME`. As an example, here are `tmux` and `neovim` package structures:

```text
tmux
└── .config
    └── tmux
        └── tmux.conf
```

and

```text
neovim/
└── .config/
    └── nvim/
        ├── coc-configuration.vim
        ├── colourschemes/
        │   ├── mountains-on-mars.vim
        │   ├── red-star.vim
        │   └── ruiner.vim
        ├── init.vim
        └── plug.vim
```

which means, you could copy and paste the contents of a package's root directory
into your user's home. Just remember to backup things first before copy-pasting
anything, OK?

## Manual installation

One way to install the configs is to clone this repository and then, copy the
contents of the required files into your system, making the necessary changes to
deal with hard-coded paths.

## Installation using GNU Stow

If using `stow` to install configurations, note that [it first checks for
conflicts between the target and destination directories before making any
change to the file system][deferred-op], which will reduce the chances that the
target directory will be left in an inconsistent state. With that in mind, you
can go ahead and clone this repository if you didn't already

```zsh
git clone https://github.com/daniloBlera/ConfigFiles.git
```

then, install a specific package by running something with the following
structure:

```zsh
stow [--no-folding] -d SOURCE-DIR -t TARGET-DIR PACKAGE [PACKAGE ...]
```

where `SOURCE-DIR` is the path to the root of the cloned repository,
`TARGET-DIR` is the root to where the files should be installed (the user's home
in the case for this repository) and `--no-folding` is an option if you would
want to disable folding (more on the section [About directory
folding](#about-directory-folding)).

As an example, to install the `zsh`, `neovim`, and  `tmux` configurations you
should run

```zsh
stow -d ConfigFiles -t ~/ zsh neovim tmux
```

Supposing that no conflicts happened - as stow won't overwrite existing files -
the symlinks should be on their right location on the user's home.

### About directory folding

Stow by default will fold directories, i.e., it will create a single symlink
pointing to the root of the package's files instead of re-creating the same
subdirectory tree structure and populating it with symlinks to the source files.
If you'd want to add other files to the stowed directory without cluttering the
package's source, use the `--no-folding` flag. Using the same *stow* (`-d`) and
*target* (`-t`) paths as the example before:

```zsh
stow --no-folding -d ConfigFiles -t ~/ scripts
```

with the `--no-folding` option set, stow will create a directory tree structure
matching the same structure from the source files and populating it with
symlinks to the individual source files instead of placing a single symlink
pointing to the root of the package's tree.

[deferred-op]: https://www.gnu.org/software/stow/manual/stow.html#Deferred-Operation-1
