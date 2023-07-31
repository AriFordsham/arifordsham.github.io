---
layout: post
title: "How to run Linux commands without installing"
date: 2023-07-31
---

Maybe you want to try out a Linux command, or use it as a one-off. If you're 
like me, it makes you vaguely uneasy to have your machine littered with software
you forgot why you installed. Wouldn't it be nice if there was some way of 
running commands, from the Internet, without having them leaving residue on your
machine? Sounds impossible, no?

(Github Copilt is sitting on my shoulder, and it suggested Docker. Nice try, but
not quite. Besides for the trouble of getting it set up, what if I want easy 
access to my local files or hardware? Try again, wise guy.)

I'm going to show you a solution using [Nix](https://nixos.org/). Nix is a part 
package manager, part
build system, that has tremendous power, but also suffers currently from 
tremendous [usability issues](https://github.com/AriFordsham/nix-gripes). I want
to show you how to use its power, while avoiding the frustrating bits.

To run a command without installing:

- First you need to [install Nix](https://nixos.org/download.html). Basically,
you execute this:

  ```bash
  $ sh <(curl -L https://nixos.org/nix/install) --daemon
  ```

It will call `sudo` to install, unfortunately, but it can mainly be used 
afterward without superuser privileges. 

- You will need to restart your shell for Nix to work properly.

- Next, configure Nix for all the latest features:

  ```bash
  $ echo "experimental-features = nix-command flakes" | sudo tee -a /etc/nix/nix.conf
  ```

- Now, find the name of the Nix package and command you want using the 
  [Nix package search](https://search.nixos.org/packages). You may know the name 
  of the command, or you can find a list of commands available by package in the
  package details section.

- If the command is the same as the package name (or the package is configured 
with a default command), you can run it like this:

  ```bash
  $ nix run nixpkgs#<package> -- <args>
  ```

  If you need to run a different command from the package, the invocation is:

  ```bash
  $ nix shell nixpkgs#<package> -c <command> -- <args>
  ```

  Nix will download the package and all its dependencies to an isolated location
  on your system, then execute the command!

  If you're going to invoke a command from a package more than once, you can 
  start a bash shell with a package loaded:

  ```bash
  $ nix shell nixpkgs#<package> ...
  ```

  You can specify more than one package. The commands from the package then 
  become avalable until you exit the Nix shell with `Ctrl-D`.

### What happens to the downloaded packages afterward?

Nix downloads all it's packages and all their dependencies right down to the 
Linux kernel API to an isolated location on your system - `/nix/store`. Nix will
completely ignore any programs or libraries you have installed on your system. 
It then keeps them around in case you want to use them again. (This is not as 
useful as it sounds, because a second invocation of the same package is likely 
to redownload if there is a new version of the package _or any of its 
dependencies_.)

So a disadvantage of using Nix is that it can consume a lot of disk space. You
can free up space by clearing cached packages with:

```bash
$ nix store gc
```

You should configure Nix to use less space by:

```bash
$ echo "auto-optimise-store = true" | sudo tee -a /etc/nix/nix.conf
```

## Addendum: Installing packages without root (or building)

Often, you may want to give a Linux user without sudo privileges the ability to
install packages. None of the popular package managers provide this.

There is a reason for this. Installing packages into the system default location
needs root privileges, of course. So instead, you want to install to a directory
within the user's home directory. Trouble is, Linux programs typically have many
paths, such as dynamic libraries, hardcoded into their binaries. Therefore, the 
package managers that distribute prebuilt binaries can only install into 
predetermined locations. Gentoo, which builds all packages from source, indeed
allows restricted users to install packages with a custom 'prefix.'

The next option is to build everything you need fro source, but this is 
time-consuming and error-prone. 

With Nix, restricted users can install packages into their own user profiles.
Nix installs all packages int its 'store', and considers it safe to allow restricted
users to do so, as the store is read-only. You can install packages with:

```bash
$ nix profile install nixpkgs#<package>
```

This creates a garbage collection 'root', so this package won't go away if you 
run `nix store gc`. You can remove it with:

```bash
$ nix profile remove packages.x86_64-linux.<package>
```

These are some of the powerful and cool things you can do with Nix. Nix has the
ability to do much more, but it's not for the faint of heart. 