# Dotfiles

Shell helpers managed with GNU Stow.

## Install with Stow

On Arch Linux:

```sh
sudo pacman -S stow
```

With this repository cloned at `~/kris-gh/dotfiles`, preview the links:

```sh
stow --simulate --verbose \
  --dir="$HOME/kris-gh" \
  --target="$HOME" \
  --ignore='^(README\.md|LICENSE)$' \
  dotfiles
```

If the preview looks correct, create them:

```sh
stow --verbose \
  --dir="$HOME/kris-gh" \
  --target="$HOME" \
  --ignore='^(README\.md|LICENSE)$' \
  dotfiles
```

This creates `~/.dockerfuncs` as a symlink to this repository's file.
The ignore pattern excludes the README and license; Stow ignores `.git`
by default. If Stow reports an existing-file conflict, inspect and back up
that file before replacing it.

## Load the functions in Zsh

Stow creates links; it does not load shell functions. Add this line once
to `~/.zshrc`:

```sh
source "$HOME/.dockerfuncs"
```

Open a new terminal, or load the functions into your current Zsh session:

```sh
source "$HOME/.dockerfuncs"
```

Check that both functions are available:

```sh
type aws discord
```

Edits to `.dockerfuncs` in this repository are visible through the symlink.
Source it again or open a new terminal to load the updated definitions.

## Usage

Docker must be installed, running, and accessible to your user. The functions
use the `ice88/awscli:latest` and `ice88/discord:latest` images.

```sh
aws --version
aws s3 ls
discord
```

The `aws` function mounts `~/.aws` for AWS configuration and the current
directory at `/aws` inside the container. Ensure `~/.aws` exists before
running it:

```sh
mkdir -p "$HOME/.aws"
```

AWS requests requiring authentication also need valid credentials.
The `discord` function expects a Wayland session with `XDG_RUNTIME_DIR` and
`WAYLAND_DISPLAY` set, plus the system D-Bus socket.

## Remove the links

Remove the `source "$HOME/.dockerfuncs"` line from `~/.zshrc`, then run:

```sh
stow --delete --verbose \
  --dir="$HOME/kris-gh" \
  --target="$HOME" \
  --ignore='^(README\.md|LICENSE)$' \
  dotfiles
```

This removes the Stow-managed symlink, not the repository file. Open a new
terminal afterward; functions already loaded in an existing session remain
defined until that session ends.
