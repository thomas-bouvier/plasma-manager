# Manage KDE Plasma with Home Manager

This project aims to provide [Home Manager][home-manager] modules which allow you
to configure KDE Plasma using Nix.

## Table of contents
- [What's supported](#whats-supported)
- [What's not supported](#whats-not-well-supported-at-the-moment)
- [Getting started](#getting-started)
- [Enabling declarative configuration](#make-your-configuration-more-declarative-with-overrideconfig)
- [Capturing existing configuration](#capturing-your-current-configuration-with-rc2nix)
- [Contributions and maintenance](#contributions-and-maintenance)
- [Special thanks](#special-thanks)

## Supported versions
`plasma-manager` supports both plasma 5 and plasma 6. The `trunk` branch is the
most up-to-date branch and is mainly focused on plasma 6, but may still work on
plasma 5. If you are running plasma 5, it's recommended to use the `plasma-5`
branch, which is designed to have better compatibility with plasma 5. To do this
with flakes you can use "github:pjones/plasma-manager/plasma-5" as you flake
url, or if you are using nix-channels you can set the channel url to
"https://github.com/pjones/plasma-manager/archive/plasma-5.tar.gz".

## What's supported
At the moment `plasma-manager` supports configuring the following:
- KDE configuration files (via the `files` module)
- Global themes, colorschemes, icons, cursortheme, wallpaper (via the `workspace` module)
- Configuration of spectacle shortcuts (via the `spectacle` module)
- Shortcuts (via the `shortcuts` module)
- Hotkeys (via the `hotkeys` module)
- Panels (via the `panels` module)
- Fonts (via the `fonts` module)
- KDE apps (via the `apps` module). In particular the following kde apps have
  modules in `plasma-manager`:
  - kate
  - konsole

Additionally there are more functionality than just listed above, and more
functionality to come in the future!

## What's not well supported (at the moment)
There also are some things which at the moment isn't very well supported, in
particular:
- Real-time updates of configuration without having to log out and back in
- Configuration of LibInput devices (see https://github.com/pjones/plasma-manager/issues/47 and https://github.com/pjones/plasma-manager/pull/123)
- Usage of high-level modules in the configuration generated by `rc2nix`
- Configuration of kwin window rules
- Keybindings to some key combinations (`Ctrl+Alt+T` and `Print` for example, see https://github.com/pjones/plasma-manager/issues/109 and https://github.com/pjones/plasma-manager/issues/136)

There may also be more things we aren't aware of. If you find some other
limitations don't hesitate to open an issue or submit a pr.

## Getting started
We provide some examples to help you get started. These are located in the
[examples](./examples/) directory. Here you in particular can find:
- [An example home-manager configuration](./examples/homeManager/home.nix) [with instructions](./examples/homeManager/README.md)
- [An example flake.nix for usage with home-manager only](./examples/homeManagerFlake//flake.nix)
- [An example flake.nix for usage with the system configuration](./examples/systemFlake/flake.nix)
- [An example home.nix showing some of the capabilities of plasma-manager](./examples/home.nix)

With more to come! These should give you some idea how to get started with
`plasma-manager`.

## Make your configuration more declarative with overrideConfig
By default `plasma-manager` will simply write the specified configurations to
various config-files and leave all other options alone. This way settings not
specified in `plasma-manager` will be left alone, meaning that configurations
made outside `plasma-manager` will still be set. This can lead to different
settings on different machines even with the same `plasma-manager`
configuration. If you like a declarative approach better consider enabling
`overrideConfig`. This makes it so all options not set by `plasma-manager` will
be set to the default on login. In practice this then becomes a declarative
setup, much like what you would expect from most `home-manager` options/modules.

One thing to keep in mind is that enabling this option will delete all the KDE
config-files on `home-manager` activation, and replace them with config-files
generated by `plasma-manager`. Therefore make sure you backup your KDE
config-files before enabling this option if you don't want to lose them.

## Capturing Your Current Configuration with rc2nix

To make it easier to migrate to `plasma-manager`, and to help maintain your Nix
configuration when not using `overrideConfig`, this project includes a tool
called `rc2nix`.

This tool will read KDE configuration files and translate them to Nix.  The
translated configuration is written to standard output.  This makes it easy to:

- Generate an initial Plasma Manager configuration file.
- See what settings are changed by a GUI tool by capturing a file
  before and after using the tool and then using `diff`.

Keep in mind that the `rc2nix` module isn't perfect and often will give somewhat
suboptimal configurations (it will in some cases prefer using the `files` module
when better configurations can be achieved using higher-level modules). However,
it is still a useful tool to quickly get your configuration up and running or
converting config-files generated by the gui settings app to nix expressions.

To run the `rc2nix` tool without having to clone this repository run
the following shell command:

```sh
nix run github:pjones/plasma-manager
```

## Contributions and Maintenance

This is a community project and we welcome all contributions.  If there's enough
interest we would love to move this into [nix-community] once it has matured.

## Special Thanks

This work was inspired by the suggestions on [Home Manger Issue #607][hm607] by
people such as [bew](https://github.com/bew) and
[kurnevsky](https://github.com/kurnevsky).  Thank you.

[home-manager]: https://github.com/nix-community/home-manager
[hm607]: https://github.com/nix-community/home-manager/issues/607
[nix-community]: https://github.com/nix-community
